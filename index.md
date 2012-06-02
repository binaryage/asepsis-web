---
layout: product
title: Asepsis is a system utility for prevention of .DS_Store files
product: asepsis
product_title: Asepsis
product_subtitle: a smart solution for .DS_Store pollution
note: Asepsis was originally a feature of <a href="http://totalfinder.binaryage.com">TotalFinder</a>.
download: http://downloads.binaryage.com/Asepsis-1.2.dmg
downloadtitle: Download v1.2
downloadsubtitle: Requires OS X 10.7 or higher
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

It prevents creation of [.DS_Store](http://en.wikipedia.org/wiki/.DS_Store) files. It redirects their creation into a special folder.

### Why is .DS_Store a problem?

Well, it is not really a problem for most Mac users because .DS_Store files are hidden in Finder.

But I'm a developer and I run Finder with [TotalFinder](http://totalfinder.binaryage.com) with visible hidden files. Also I run a lot of command-line tools via terminal. The problem is that sometimes .DS_Store files get into a way. I hate when my clean new folders get polluted by those small tiny files holding unimportant garbage. I hate when I zip a folder using some unix command and it includes .DS_Store files in the archive. I hate when I visit a network volume and that pollutes its content with those nasty files. To put it simply I don't want my geeky Windows friends to laugh at me because this makes me look incompetent. 

That is why I decided to solve this for myself and I'm sharing my solution with other Mac geeks out there to help them protect their egos :-)

### Asepsis does .DS_Store redirection. How does it work technically?

Apple implemented a private system framework `DesktopServicesPriv` which is responsible for creating and manipulating .DS_Store files. This framework is used mainly by Finder, but there are also other system apps which link against it and may use it (yes mdworker I'm looking at you!). DesktopServicesPriv uses standard libc calls to manipulate .DS_Store files.

At core Asepsis provides a dynamic library `DesktopServicesPrivWrapper` which gets loaded into every process linking against DesktopServicesPriv.framework. It interposes some libc calls used by DesktopServicesPriv to access .DS_Store files. Interposed functions detect paths talking about .DS_Store files and redirect them into a special prefix folder. This seems to be transparent to DesktopServicesPriv.

Additionally Asepsis implements a kernel extension and an user-space system-wide daemon `asepsisd` whose purpose is to monitor system-wide folder renames (or deletes) and mirror those operations in the prefix folder. This is probably the best we can do. This way you don't lose your settings after renaming folders because rename is also executed on a folder structure in the prefix directory.

### The prefix folder is **`/usr/local/.dscage`**

  1. `DesktopServicesPrivWrapper` - a proxy library for interposing file manipulation calls in DesktopServicesPriv
  2. `Asepsis.kext` - a kernel extension for watching folder operations
  3. `asepsisd` - a system-wide daemon for mirroring folder renames and deletes in the prefix folder
  4. `asepsisctl` - a command-line utility for controlling Asepsis operation

## Installation

### Installation

To install the software:

  1. Run the [installer]({{page.download}})
  2. Restart your computer

After reboot .DS_Store files are no longer created when you open Finder and browse folders (in case .DS_Store files are not already present there).

### Troubleshooting

In Terminal.app run:

	asepsisctl diagnose

### Uninstallation

Just run provided uninstaller from the DMG archive or in Terminal.app enter: 
    
    asepsisctl uninstall
	reboot
	
Alternatively you may use uinstaller which comes with the DMG archive. It launches `asepsisctl uninstall` for you.

### Installing from sources

You may want to review the source code and install it by compiling it from sources. Please follow instructions in [GitHub repository](http://github.com/binaryage/asepsis). 

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
	    diagnose                         Diagnoses asepsis setup.

	Helper commands:
	    neton                            Enables DS_Store files on network volumes.
	    netoff                           Disables ... (http://support.apple.com/kb/ht1629).
	    migratein                        Migrates .DS_Store files in /usr/local/.dscage.
	    migrateout                       Migrates .DS_Store files from /usr/local/.dscage.
	    prune                            Removes empty directories from /usr/local/.dscage.
	    clean                            Deletes all content from /usr/local/.dscage.

	Installation commands:
	    install                          Performs reinstallation from sources in /System/Library/Extensions/Asepsis.kext.
	    uninstall                        Performs complete uninstallation.
	    install_wrapper                  Installs DesktopServicesPriv.framework wrapper.
	    uninstall_wrapper                Uninstalls DesktopServicesPriv.framework wrapper.
	    unload_kext                      Unloads asepsis kernel extension.
	    load_kext                        Loads asepsis kernel extension.
	    remove_symlink                   Removes asepsisctl symlink from /usr/local/bin.
	    create_symlink                   Creates asepsisctl symlink in /usr/local/bin.
	    make_dscage                      Makes sure /usr/local/.dscage exists with sufficient rights.
	    kill_daemon                      Stops asepsis daemon.
	    uninstall_daemon                 Uninstalls asepsis daemon.
	    install_daemon                   Installs asepsis daemon.
	    launch_daemon                    Launches asepsis daemon.
	    enable_warnings                  Enables warnings caused by mach_override (vm.shared_region_unnest_logging)
	    disable_warnings                 Disables warnings caused by mach_override (vm.shared_region_unnest_logging)
	    uninstall_updater                Uninstalls asepsis updater.
	    install_updater                  Installs asepsis updater.

	Backward compatibility:
	    uninstall_dylib                  Removes libAsepsis.dylib from /etc/launchd.conf.
   
	Where options are:
	    -r, --root /Users/darwin         Root folder for migration
	    -d, --[no-]dry                   Run migration in dry mode
	    -v, --[no-]verbose               Be more verbose
	    -f, --[no-]force                 Force operation
	    -h, --help                       Show this message
	    -V, --version                    Print version
        
## Known issues

### The list of known issues

#### General

  * when copying folders, DS_Store settings are not copied over (daemon is unable to distinguish this class of file operations)

Please [report any issues](mailto:support@binaryage.com).

## FAQ

#### Sounds scary. Is this safe?

> Well uhmmm, use it at your own risk :-) It sounds scary but it should be a pretty lightweight solution. You should [review the code](http://github.com/binaryage/asepsis) to see what it does.

#### Could this be ported to Snow Leopard?

> Probably yes. Pre-Lion DesktopServicesPriv calls File Manager APIs from CoreServices. Technically the same approach could be done to File Manager. Actually this is what TotalFinder (prior to version 1.3) did under Snow Leopard using mach_override. I'm not going to implement it for Snow Leopard because I've already switched to Lion personally.

#### What if DS_Store file is already present in a folder?

> Asepsis will use it. It won't redirect it in this case. This way Asepsis works seamlessly with DMG archives or folders you browse on a network. If you delete the .DS_Store file, the next time it will be created in the prefix folder. This is pretty simple approach how to adopt Asepsis incrementally on folder-by-folder basis.

#### How could I migrate existing DS_Store files?

> Please see `asepsisctl migratein` command
>
> Note: when running some migration utility you probably want to suspend Asepsis operation prior launching it: `asepsisctl suspend`. See [Control section](#control) for more info.

#### Are there any processes which don't work well with Asepsis?

> Asepsis has white-list of processes known to manipulate with .DS_Store files. Right now it is effective in Finder.app and mdwrite process.

#### What about using resource forks for hiding DS_Store files?

> Good question! This would be probably a more elegant approach but it would work only for HFS+ volumes. I haven't tried to implement it via resource forks. I decided to run with this prefix-folder approach which seemed to me as a more predictable approach. Feel free to [fork and experiment](https://github.com/binaryage/asepsis).

#### How can I keep Asepsis up-to-date?

> Asepsis is using Sparkle updater. You should be prompted when a new version comes out.

#### What happened in 10.7.4 update?

> Previously Asepsis used DYLD_INSERT_LIBRARIES to modify DesktopServicesPriv during load time. Unfortunately Flashback malware used the same mechanism and Apple decided to block system-wide usage of DYLD_INSERT_LIBRARIES in 10.7.4 update. As a workaround Asepsis 1.2 modifies DesktopServicesPriv on the disk during installation. This is more dirty solution. At least I have implemented post-login check which will inform you that someone reverted Asepsis changes back. For example after you do a system update and Apple may rewrite DesktopServicesPriv framework with new version. In this case `asepsisctl install_wrapper` should patch it again but better wait for my update.

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