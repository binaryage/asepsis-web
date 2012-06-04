---
layout: product
title: Asepsis is a system utility for prevention of .DS_Store files
product: asepsis
product_title: Asepsis
product_subtitle: smart solution for .DS_Store pollution
note: Asepsis was originally a feature of <a href="http://totalfinder.binaryage.com">TotalFinder</a>.
download: http://downloads.binaryage.com/Asepsis-1.1.dmg
downloadtitle: Download v1.1
downloadsubtitle: Requires OS X 10.7 (Lion)<br><span style="color:#f33">Conflicts with Xcode 4.2 and Glims!</span><br><span style="color:#f33">Broken under 10.7.4 update - expect a fix soon!</span>
repo: http://github.com/binaryage/asepsis
meta_title: Asepsis is a system utility for prevention of .DS_Store files
meta_keywords: totalfinder,asepsis,osx,simbl,binaryage,productivity,software,system,utility
meta_description: Asepsis is a system utility for prevention of .DS_Store files created by Finder and other applications
meta_image: http://www.binaryage.com/shared/img/icons/asepsis-256.png
facebook: 1
retweet: 1
buzz: 0
fbsdk: 1
ogmeta: {
    site_name: "BinaryAge website",
    description: "Asepsis is a system utility for prevention of .DS_Store files",
    email: "support@binaryage.com",
    type: "product",
    title: "Asepsis",
    url: "http://asepsis.binaryage.com",
    image: "http://www.binaryage.com/shared/img/icons/asepsis-256.png"
}
shots: [{
    title: ".DS_Store files are like ghost chasing you in every folder you visit",
    thumb: "/shared/img/asepsis-mainshot.png"
}]
---

## About

### What it does for me?

It prevents creation of .DS_Store files. It redirects their creation into a special folder.

### What is .DS_Store?

This is pretty famous file which even got its [own page on wikipedia](http://en.wikipedia.org/wiki/.DS_Store).

### Why is .DS_Store a problem?

Well it is really not a problem for 99.99% users. Even Apple folks think it is not worth solving. I'm a developer and I run Finder with [TotalFinder](http://totalfinder.binaryage.com) and with displayed hidden files. Also I run a lot of command-line tools via terminal. The problem is that .DS_Store files get into a way sometimes. I hate when my clean new folders get polluted by those small files holding mostly unimportant garbage to me. I hate when I zip folder using a unix command-line tool and it includes .DS_Store files in the archive. I hate when I visit a network volume or a flash drive and that pollutes its content with those tiny files. I don't want my geeky Windows friends to laugh at me because this makes me look incompetent. 

That is why I decided to solve this for myself and I'm sharing my solution for other Mac geeks out there to help them protect their egos :-)

### Asepsis does .DS_Store redirection. How does it work technically?

Apple implemented a private system framework `DesktopServicesPriv` which is responsible for creating and manipulating .DS_Store files. This framework is used mainly by Finder, but there are also other system apps which link against it and may use it (yes mdworker I'm looking at you!). DesktopServicesPriv uses standard libc calls to manipulate .DS_Store files (this is new in Lion).

At core Asepsis provides a dynamic library `libAsepsis.dylib` which gets loaded into every launching process thanks to `DYLD_INSERT_LIBRARIES`. It interposes some libc calls used by DesktopServicesPriv to access .DS_Store files. Interposed functions detect paths talking about .DS_Store files and redirect them into a special prefix folder. This seems to be transparent to DesktopServicesPriv.

Additionally Asepsis implements a kernel extension and an user-space system-wide daemon `asepsisd` whose purpose is to monitor system-wide folder renames (or deletes) and mirror those operations in the prefix folder. This is probably the best we can do. This way you don't lose your settings after renaming folders because rename is also executed on folder structure in the prefix directory.

### The prefix folder is **`/usr/local/.dscage`**

  1. `libAsepsis.dylib` - a shared library for interposing file manipulation calls
  2. `Asepsis.kext` - a kernel extension for watching folder operations
  3. `asepsisd` - a system-wide daemon for mirroring folder renames and deletes in the prefix folder
  4. `asepsisctl` - a command-line utility for controlling Asepsis operation

## Installation

### Installation

To install the software:

  1. Run the installer
  2. Restart your computer

After reboot .DS_Store files are no longer created when you open Finder and browse folders (in case .DS_Store files were not already present there).

### Uninstallation

Just run provided uninstaller from the DMG archive or in Terminal.app enter: 
    
    open /System/Library/Extensions/Asepsis.kext/Contents/Resources/Asepsis\ Uninstaller.app

### Installing from sources

You may want to review the source code and install it by compiling it from sources. Please follow instructions in [GitHub repository](http://github.com/binaryage/asepsis). 

## Migration

### Migration from TotalFinder 1.3

    mv /usr/local/.dscache /usr/local/.dscage

## Control

### You may control asepsis operation via asepsisctl

During the installation asepsisctl tool is symlinked into `/usr/local/bin`, so it should be visible from the command-line.

    > asepsisctl --help
    The control script for asepsis operations.

    Usage:
        asepsisctl [command] [options]

    Commands:
        suspend                          Suspends immediate asepsis operations.
        disable                          Disables asepsis.
        enable                           Enables asepsis.
        neton                            Enables DS_Store files on network volumes.
        netoff                           Disables ... (http://support.apple.com/kb/ht1629).
        diagnose                         Diagnoses asepsis setup
        migratein                        Migrates .DS_Store files in /usr/local/.dscage
        migrateout                       Migrates .DS_Store files from /usr/local/.dscage
        prune                            Removes empty directories from /usr/local/.dscage
        clean                            Deletes all content from /usr/local/.dscage

    Where options are:
        -r, --root /Users/darwin         Root folder for migration
        -d, --[no-]dry                   Run migration in dry mode
        -v, --[no-]verbose               Be more verbose
        -h, --help                       Show this message
        -V, --version                    Print version
        
## Known issues

### The list of known issues

#### General

  * when copying folders, DS_Store settings are not copied over (daemon is unable to distinguish this class of file operations)

#### Bugs

  * sometimes you may see something about lost messages in Console.app - this is not a fatal issue, I will fix it in version 1.2
  
#### Glims
  
  * Glims uses the same method of injecting code by setting `DYLD_INSERT_LIBRARIES`. Glims overrides Asepsis. This will be fixable but it needs more investigation from my side. Please [comment on it here](http://getsatisfaction.com/binaryage/topics/asepsis_not_working_if_glims_is_installed).

#### Xcode 4.2, iOS 5.0 Simulator

  * Some developers reported that Asepsis breaks iOS 5.0 Simulator and causes Xcode 4.2 fail in loading XIB files

#### OS X 10.7.4

  * Something changed and Asepsis does not work as advertised. Will investigate and release a fix soon.


Please [report any issues](mailto:support@binaryage.com). A similar technical approach was used in TotalFinder for more than 2 years without significant troubles. I'm aware that it is not a perfect solution but still it improves my situation because I don't care about .DS_Store files that much and can afford losing them from time to time.

## FAQ

#### Sounds scary. Is this safe?

> Well uhmmm, use it at your own risk :-) It sounds scary but it should be a pretty lightweight solution. You should [review the code](http://github.com/binaryage/asepsis) to see what it does. I have been running Asepsis on my own machine for some time and I didn't encounter any problems so far.

#### After the installation my computer no longer restarts back into Lion. What should I do?

> Calm down. Don't panic. You will be able to restart into single-user mode and remove boot-time Asepsis initialization to recover.
> 
> 1. Restart to enter into [singe-user mode](http://support.apple.com/kb/ht1492) by holding CMD+S during the reboot.
> 2. Follow the on-screen instruction to mount your main disk as writable: `/sbin/mount -uw /`
> 3. Delete or rename the launchd.conf: `mv /etc/launchd.conf /etc/launchd-conf.backup`
> 4. Type `reboot` and restart back into Lion in normal mode
> 5. Uninstall Asepsis using uninstaller: `open /System/Library/Extensions/Asepsis.kext/Contents/Resources/Asepsis\ Uninstaller.app`
>
> Note: this should never happen, but it happened to me during the development when libAsepsis.dylib was corrupted in a very bad way. I'm describing it here for you to see that a recovery from fatal case of Asepsis hiccup is pretty simple.

#### Why is this better than [TotalFinder](http://totalfinder.binaryage.com)?

> Thanks to DYLD_INSERT_LIBRARIES this solution is applied system-wide (it affects all processes linking DesktopServicesPriv, not only Finder). Also there was a timing problem with TotalFinder. Due to a nature how Input Managers work, the plugin gets usually injected into the Finder after some delay. Prior an injection the Finder is running and can arbitrarily touch some .DS_Store files. More than that! After the injection it could cause an internal state confusion because the Finder has already cached some of .DS_Store files in-memory. For example ~/Desktop/.DS_Store was a common pain-point. DYLD_INSERT_LIBRARIES is a stable solution, because dydl interposes libc calls immediately at the point of dynamic linking prior any code is executed, so there is no chance for Asepsis to miss some calls.

#### Could this be ported to Snow Leopard?

> Probably yes. Pre-Lion DesktopServicesPriv calls File Manager APIs from CoreServices. Technically the same approach could be done to File Manager. Actually this is what TotalFinder (prior to version 1.3) did under Snow Leopard using mach_override. I'm not going to implement it for Snow Leopard because I've already switched to Lion personally.

#### What if DS_Store file is already present in a folder?

> Asepsis will use it. It won't redirect it in this case. This way Asepsis works seamlessly with DMG archives or folders you browse on a network. If you delete the .DS_Store file, the next time it will be created in the prefix folder. This is pretty simple approach how to adopt Asepsis incrementally on folder-by-folder basis.

#### How could I migrate existing DS_Store files?

> I don't provide any migration tools at this point. Some people wrote simple scripts which are able to move all existing DS_Store files into prefix folder or vice versa. Most people just delete DS_Store files and start again from scratch with Asepsis enabled.
>
> Note: when running some migration utility you probably want to suspend Asepsis operation prior launching it: `asepsisctl suspend`. See [Control section](#control) for more info.

#### Are there any processes which don't work well with Asepsis?

> Yes. For example `backupd`. There is [a hard-coded list of processes](https://github.com/binaryage/asepsis/blob/master/dylib/init.c#L22) which are known to operate globally. For example backup utilities should probably see the disk as a whole without any redirection.

#### What about using resource forks for hiding DS_Store files?

> Good question! This would be probably a more elegant approach but it would work only for HFS+ volumes. I haven't tried to implement it via resource forks. I decided to run with this prefix-folder approach which seemed to me as a more predictable approach. Feel free to [fork and experiment](https://github.com/binaryage/asepsis).

#### Do you use DYLD_FORCE_FLAT_NAMESPACE?

> No! DYLD_FORCE_FLAT_NAMESPACE would probably cause bad incompatibilities. DYLD_INSERT_LIBRARIES with dyld interposing works just fine with two-level-namespaces. This is pretty clean solution I believe.

## Changelog

<div class="changelogx"></div>

<script type="text/javascript" charset="utf-8">
    $(function() {
        $('.changelogx').load('changelog.html?x='+((Math.random()+"").substring(2))+' #page');
    });
    
    function showBetaHint() {
        $('.betahint').toggle();
    }
</script>