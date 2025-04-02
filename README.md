The Software Manager project
Introduction to the "Ruby Software Manager" Project (RSM), formerly called "Ruby Build Tools" (RBT)


Welcome to the Ruby Software Manager project, also called the RSM suite - getting work done! Making compiling and installing software great again. \o/

This documentation here attempts to detail the various objectives for the RSM project, including its history, as well as how people may benefit from the project.

Let's now jump right into the main goals of this project - the primary objectives.

The primary objective of the RSM project
The primary objective for the RSM project is to support and help users whenever the need arises to install software, on a given computer system. For instance, if the user wishes to install or compile a specific program.

The RSM project should remain flexible for such a use case. For example, say that there is a source archive available at the following remote URL:

https://github.com/htop-dev/htop/releases/download/3.3.0/htop-3.3.0.tar.xz

We would like to support the following commandline invocation, in order to compile this source archive (a .tar.xz tarball) from source:

rsm https://github.com/htop-dev/htop/releases/download/3.3.0/htop-3.3.0.tar.xz

The RSM suite will do various steps to achieve this. Specifically it will do at the least the following steps:

(1) download the remote archive locally
(2) extract the tarball
(3) prepare the archive for compilation
(4) try to compile the program at hand
(5) report all findings that are relevant to the user [SUCSESS / FAILURE]

In the ideal outcome, this remote tarball has been downloaded and then compiled successfully (aka installed successfull).

This is not the only use case, of course.

If the remote URL is a binary package, then RSM should install it. If it is a local archive, then this should be installed as well.

This basically sums up the primary objective of the RSM suite. Let's next have a look at secondary objectives for this project.

The secondary objectives of the RSM project
The RSM project should help in regards to users wishing to maintain the software on their computer system in general. RSM thus needs to be able to act as a layer that can be helpful to the user, no matter the underlying operating system.

Various additional scripts are available that aid in various situations. For instance, one script can modify the PREFIX used in a Makefile. This allows the user to quickly use any other prefix variable, irrespective over as to whether that Makefile supports it or not.

Support for different operating systems by the RSM project
The primary operating system for the RSM suite is Linux, that is the Linux operating systems (henceforthwith simply called Linux in this document.

Linux is, in this regard, a first-class citizen. Everything pertaining to the RSM project will be tested and developed first and foremost on Linux.

However had, other operating systems than Linux should also be supported. Whenever ruby works on a given operating system, the RSM suite will attempt to support that operating system. This may require some changes every now and then, though, as different operating systems have different features. For instance, HaikuOS favours a filesystem that is not easily supported by Ruby.

Additionally, when we refer to allowing a user to install software, this typically refers to taking the source code and compiling this source code - in other words: to simply compile the program at hand, from a remote URL.

While the primary platform for the RSM project is predominantly the Linux platform, whenever possible support for other platforms will be added as well. In fact, it is a secondary goal for the RSM project to support all platforms ruby is able to run on in the long term.

How realistic it is to reach that goal is another matter, but as a goal RSM will eventually try to support as many different operating systems as possible, provided that ruby can run on such a system.

As mentioned already, Linux is the main target OS for the RSM project, so things will always be tested on Linux first. Most development for the SoftwareManager-project does indeed take place on a Linux system currently.

Ideally, in the future, we could reach a state where we can test the SoftwareManager suite on less commonly used systems as well, such as HaikuOS or ReactOS to name only a few here. And, of course, the more popular ones such as OSX or Microsoft's Windows suite as well, naturally. Windows is the second operating system that SoftwareManager will support, by the way; it is less well-tested than the Linux-specific code of the SoftwareManager-project.


The History of the SoftwareManager Project (RSM).
Back in the days - and I still am of this opinion even today - I thought that shell scripts ultimately tend to grow in complexity way faster and steeper than equivalent code writte in other, better programming languages.

They also lean towards becoming really ugly, which makes it hard to further extend them with more code, at which point sooner or later maintaining them becomes a burden.

When maintaining code becomes hard to maintain, you often start to lose interest and eventually cease maintaining it or adding anything new to it. This was the case the with Linux distribution called GNU Sorcery - all their recipes were written in shell scripts.

Back in 2004 or so, I was using GoboLinux, and while the shell scripts used in GoboLinux were fairly elegant, I did not understand them at all. So I decided to re-create the logic found in those shell scripts, but primarily in ruby, storing the instructions in .yml (YAML) files.

Specifying another source location
Normally all source archives are stored into /home/x/src/ - or, rather, the variable the user set in the configuration file. (On my home system this is the directory I use for keeping all source-archives.)

But is this always the case, that a user keeps all source archives within a single directory? Of course not. Sometimes the user may want to use another source location. Precisely due to this reason, a commandline flag exists, to allow specifying the target path to another directory that is to be used as source archive. That commandline-flag is called --use-this-source-location=.

An example for using this follows, in order to make use of the path /depot/j/gcc-12.3.0.tar.xz.

ry gcc ntrad --use-this-source-location=/depot/j/gcc-12.3.0.tar.xz

Specifying another programs directory
On my home setup, the programs-directory I use is /home/Programs/.

GoboLinux uses the directory /Programs/ as the programs-directory.

Some users may want to use another directory, such as /opt/. We thus need flexibility here.

The user needs a way to designate a different programs directory. This is done via the commandline-flag --use-this-programs-dir=.

Invocation examples:

ry htop --ntrad --use-this-program-dir=/opt
ry htop --ntrad --use-this-programs-dir=/opt

This will become the programs directory for the current compilation run. So, if compilation succeeds, the user may then have the AppDir at /opt/Htop/3.3.0/.

Obtaining the number of registered programs
The number of registered programs in the SoftwareManager-project can be obtained via the commandline, by issuing any of the following commandline instructions:

ry --registered_programs?
ry --n_programs?
ry registered_programs?

The number of registered programs can be queried via the commandline as well, like in this way:

ry --n_programs?

Although the output may be a little bit different.

This would then output something such as this:

→ 3330 programs registered as of 30.01.2018.
→ 3414 programs registered as of 18.09.2018.
→ 3635 programs registered as of 01.12.2019.
→ 3656 programs registered as of 12.01.2020.
→ 3846 programs registered as of 28.02.2025.

(The format may be a bit different to the above, but the information content is the same.)

Static Compilation
It is possible to compile an application statically, by passing static to the main class SoftwareManager (software_manager.rb).

Specific example:

ry make static --ntrad

Note that this will not work for every program.

In the event that you, on the commandline, try to enable static compilation, and the yaml file specifies otherwise, such as via a configure switch like --disable-static, we will ignore the latter option and allow the user to specifically overrule this setting. The SoftwareManager-project always assumes that the user knows what he or she is doing.

Note that you can also zero these flags via a commandlie option:

ry strace --no-compile-time-flags
ry strace --plain

This option means that all CFLAGS etc... will be ignored altogether; that is, they will set to an empty again.

Using a different set of configure-options
Sometimes you may want to overrule, on the commandline, the configure options that are to be used by a program.

You can do so via the switch --use-these-configure-options=.

Usage example:

--use-these-configure-options="--enable-shared --enable-bootstrap --enable-languages=c,c++,lto,objc,fortran --enable-threads=posix --enable-checking=release --enable-objc-gc --with-system-zlib --enable-libstdcxx-dual-abi --with-default-libstdcxx-abi=gcc4-compatible --disable-libunwind-exceptions --enable-__cxa_atexit --enable-libssp --enable-lto --disable-install-libiberty --with-gnu-ld --verbose --with-arch-directory=amd64 --disable-gtktest --disable-multilib --target=x86_64-slackware-linux --build=x86_64-slackware-linux --host=x86_64-slackware-linux"

Complete usage example:

ry gcc --use-these-configure-options="--enable-shared --enable-bootstrap --enable-languages=c,c++,lto,objc,fortran --enable-threads=posix --enable-checking=release --enable-objc-gc --with-system-zlib --enable-libstdcxx-dual-abi --with-default-libstdcxx-abi=gcc4-compatible --disable-libunwind-exceptions --enable-__cxa_atexit --enable-libssp --enable-lto --disable-install-libiberty --with-gnu-ld --verbose --with-arch-directory=amd64 --disable-gtktest --disable-multilib --target=x86_64-slackware-linux --build=x86_64-slackware-linux --host=x86_64-slackware-linux"

ry poppler --use-these-configure-options="--enable-xpdf-headers"

Disabling the use of cookbook aliases
The cookbooks submodule, part of the SoftwareManager-project, makes use of several aliases to program names. This is mostly a convenience feature.

Sometimes this may lead to unexpected or unwanted results, such as when you want to compile program a, but the cookbooks-submodule automatically attempts to renames the program to b instead.

In order to avoid situations similar to this, a commandline-option exists for the main SoftwareManager class.

Invoke it like so:

ry sdlttf --do-not-use-cookbook-aliases
ry sdlttf --do-not-use-cookbook-alias

This will specifically disable making use of cookbook-aliases for the current compilation run.

Compiling via the --rootprefix option
Using the --rootprefix is exactly identical to passing the following flag:

--prefix=/

Nothing more than that.

The option was added in November 2021 because I needed to compile all coreutils binaries into the /bin/ subdirectory on my home setup, and I needed these to be compiled statically, to avoid any problems during reboot when some shared libraries can not be found.

The exact command I used was:

ry coreutils --static --rootprefix

Installing/Compiling the various xcb components
If you, like me, get tired of installing the various xorg-related xcb components then the following command should be of help here:

ry --compile-xcb-components

This will install the xcb-components in the correct order (actually it will compile the xcb-components).

Aliases exist for this:

ry --compile-the-xcb-components
ry --compile-all-the-xcb
ry --compile-xcb

Program Versions
You can overrule the version specified in the cookbook yaml file from the commandline. To do this, look at the following two examples:

ry nano --version=2.1.9
ry nano --version=2.2.2
ry nano --version=2.4.3
ry gtk+ --version=2.24.29

Do note that this has a chance to fail right now, i.e. if the remote mirror does not have this specific version, or if you do not have this program available locally.

You can also use a slight shortcut:

ry nano --v=2.4.3

And an even shorter variant is to just give the leading name, and have the SoftwareManager script just pick one that fits:

ry python --2

You can also use a more complete copy-paste version setting (like you copy, then paste the full name), like this:

ry nano --version=pango-1.24.2.tar.bz2


Searching for something
To search for something, do this:

ry ruby search

To find a specific list of packages, do this:

ry pyt*

The * will tell us that we intend to find all packages starting with "pyt".

This can work with single letters too, that is - if you wish to discover all programs that are registered in the Cookbooks project starting with a, you can do:

ry e*

The "*" is the important token here that will trigger this functionality.

Be careful though if you use a shell like Bash or Zsh - the * can be expanded by the shell.

Prefix Tag
You can specify a different prefix to use when compiling a program.

To do this, do:

ry gimp --prefix=$J
ry gimp --prefix=/opt

Some nifty shortcuts exist. For example, to compile gimp into a standalone directory, into /Programs/Gimp/LatestVersion, you can do:

ry gimp ntrad
ry gimp non_traditional

Please also note that you can use the Option noprefix to avoid using any specific prefix. (This is the same as passing --prefix=/usr/local though on GNU systems.)

Do note that you can also use "compound-prefixes". These are prefixes such as /Programs/Xfce/4.12, where you may want to put all xfce-related programs into.

Syntax for this functionality goes like this:

ry xfce4panel --prefix=:xfce

In order for this to work, an environment variable called XFCE_PREFIX may have to exist. If such an environment variable does not exist, then the program_version is guessed. I recommned to set such a prefix variant.

The leading : is important here - do not forget it.

Copy the whole yaml database.
You can copy the whole yaml database by issuing:

ry x copy_yaml

Gobolinux Recipes
In order to create Gobolinux Recipes for i.e. python, do this:

ry python goborecipe
ry python --goborecipe

Take note that this will create the recipe in the current working directory.

class SoftwareManager::Action::Cookbooks::MultiUrlDisplayer
class SoftwareManager::Action::Cookbooks::MultiUrlDisplayer can be used to quickly show several programs and their associated remote URLs to their respective source archive on the commandline.

In order for this to work, naturally, these have to be part of a component, and registered in the cookbooks.

Since as of April 2024 this class is now part of the Actionsubmodule, and thus an actionable component of the SoftwareManager suite.

Let's look at a specific usage example to demonstrate this functionality:

action(:MultiUrlDisplayer, ARGV)
action(:MultiUrlDisplayer, 'kde-plasma5')
action(:MultiUrlDisplayer, 'plasma5')
action(:MultiUrlDisplayer, 'audio_suite')
SoftwareManager.action(:MultiUrlDisplayer, :audio_suite)

Or from the commandline, via multi_url_displayer:

multi_url_displayer plasma5

The output will show all remote URLs of, for example, the KDE5 plasma components.

Example listing:

plasmabrowserintegration: https://download.kde.org/stable/plasma/5.19.4/plasma-browser-integration-5.19.4.tar.xz
plasmanano: https://download.kde.org/stable/plasma/5.19.4/plasma-nano-5.19.4.tar.xz
plasmaphonecomponents: https://download.kde.org/stable/plasma/5.19.4/plasma-phone-components-5.19.4.tar.xz
kwaylandserver: https://download.kde.org/stable/plasma/5.19.4/kwayland-server-5.19.4.tar.xz

On my computer system the output this will generate is as follows (as an image):

If you are only interested in the raw URL entries, on the right side, without the leading name, such as when you wish to use wget to batch-download this easily, try any of the following variants from the commandline:

multi_url_displayer plasma5 --show-only-URLs
multi_url_displayer plasma5 --only-URLs
multi_url_displayer plasma5 --show-only-urls
multi_url_displayer plasma5 --raw

The entry status_of_the_project in a cookbooks .yml file
In June 2023 the entry status_of_the_project was added.

Possible values include:

- outdated
- possibly outdated
- low maintenance
- actively maintained

This is somewhat experimental for now. The key idea is to indicate the maintenance health of a project. Most will default to the value actively maintained, so this entry is primarily useful when a project was abandoned.

Installing older versions of programs
What if the user wishes to install an older version of, say, python?

The corresponding cookbook.yml file normally "supports" only the version that is specified in archive_name:.

For example, if you jump to the "python:" entry in the cookbook.yml file, you will see that its version is 2.5.1 or something like that.

This can be changed permanently by writing another name/version there, into that file called python.yml.

Because editing of various cookbook.yml files can be a little annoying, and because flexibility is nice to have, there exists another way to specify a different version manually on the commandline:

compile python --version=2.4.1

This works with every version, even "newer" ones.

The name of the version should however be part of the filename.

For more information about this, and an even easier way, jump here to.

Global flags
Global flags are compile-time flags that you can use, which will be appended to GNU configure runs.

These flags are stored in the file global_flags.yml

You can either manually modify this file, or use commandline instructions to modify this file.

For example, if you wish to append --disable-static to all configure invocations via RBT, then you can issue the following command:

ry --add-global-flag=--disable-static
ry --add-global-flag --disable-static

Likewise, to remove a global flag, you can do:

ry --remove-global-flag=--disable-static
ry --remove-global-flag --disable-static

You can reset all global flags again, by issuing:

ry --clear-global-flags

Note that since as of March 2025, global-flags are slightly deprecated; they never were too terribly useful to begin with, from the point of view of the SoftwareManager project.

Optional dependencies for the SoftwareManager-project
The **wget gem** is recommended as an optional dependency for the SoftwareManager gem, but you can also use the SoftwareManager project without the wget gem just fine. In that case, if the wget gem is not available, the SoftwareManager project will attempt to use the wget binary via a **system()**-call, hence an "external" program, to ruby.

This requires the installation of **wget** e. g. from this URL (or from your package manager's repository), unless wget is already available on your local computer system:

http://ftp.gnu.org/pub/gnu/wget/?C=M;O=D
Of course, most Linux systems will have "wget" available by default, so the above more applies towards operating systems such as windows (although more recent windows versions also include a linux-subsystem including bash, the **WSL**, so this should also not be a problem).

How does the format of a cookbook recipe, the respective .yml file, look like?
Each cookbook entry is just an individual yaml file, with the extension name being .yml.

For instance, information about the programming language ruby will be stored in the file called ruby.yml; information about the programming language python will be stored in the file python.yml; information about the mate-desktop will be stored in the file matedesktop.yml.

If there are any "-" tokens as part of the name, then these will be removed when RSM tries to determine the filename.

This yaml file should contain all the required information to get that particular program to install on a given system.

Additional information can be added as well, such as maintainer(s), extra information of the program at hand, tips and hints in how to install or use the program, sed-modifications, pkgconfig files (aka .pc files) that are installed alongside with, patchsets that are to be applied, binaries, headers and libraries and so on and so forth.

It can be a quite detailed file, mostly due to being able to allow some flexibility during installation.

Many programs "out in the wild" are well-behaving and will honour a --prefix variable given (or -DCMAKE_INSTALL_PREFIX=, such as in use by programs that depend on cmake, for compilation). Such programs usually require little to no modification.

Unfortunately, some other programs come with flawed installation scripts, hardcoded paths or workarounds that have to be employed to get these to work - very often sed-operations, such as done in the LFS project. Some programs may also have already become obsoleted, due to changes e. g. in glibc/gcc and other programs. If there is no maintainer of the downstream package then this normally means that other people have to either maintain that package; or provide patches to get this projects to work again.

As a reference project, the LFS project provides a lot of useful information in how to get many programs to work. I consider the LFS project to be hugely important for the whole Linux ecosystem, much more important than the various different distributions.

At any rate, let's now have a look at the fish.yml recipe, to give a specific example and explain that.

This fish.yml file will look like this, as of February 2025:

fish:
binaries:
- fish
- fish_indent
- fish_key_reader
short_description: |
FISH is an interactive (commandline) shell.
description: |
FISH is the Friendly Interactive Shell, a smart and user-friendly
command line shell for macOS, Linux, and the rest of the family.
url1: https://github.com/fish-shell/fish-shell/releases/download/2.7.1/fish-2.7.1.tar.gz
url2: http://fishshell.com/files/2.1.0
url3: http://sourceforge.net/projects/fish/
homepage: http://fishshell.com/
tags:
- shell
prefix: f
keep_extracted: yes
last_update: 10.02.2025

The very first entry, on top, is the name of the program itself, in this case, fish.

While this part could in theory be omitted (since the name of the program is already stored as the first part of the very filename), I found that it is useful to have this entry within the file, primarily as a visual identifier if you work with it in an editor. It helps me a lot, for instance.

Note that no - or _ characters are allowed there in this entry - it is really a stripped down name of the program at hand only.

If the name would be e. g. 'gtk-doc', with an URL such as **http://ftp.gnome.org/pub/gnome/sources/gtk-doc/1.26/gtk-doc-1.26.tar.xz**, then the name there for the gtkdoc.yml file would have to be **gtkdoc:** - thus, all '-' were removed. The name that appears there is also the name of the .yml file at hand. The .yml files may also not include any '-' or '_' characters. Do also note that while I am following this convention since many years, it may one day be changed to allow for '-' and '_'. But for the time being, the strict requirement is that we may not have '-' or '_' characters there.

Next, let's look at the entry **binaries:**. This entry tells us which binaries/executables this particular program will install. This also allows us to remove binaries in the /usr/bin/ hierarchy, if you compiled into an AppDir prefix - for more information here, see class **SoftwareManager::PurgeBinariesOfThisProgram**.

The next entry is **short_description**, which is e. g. equivalent to slackware's pkg-format, as introduction to the specific program at hand, in a short notation.

Then we have the entry **main description**, followed by several **url** entries. The first url entry, aptly named **url1**, is the most important entry. It tells us where to find the archive to the program at hand.

This is ideal the URL to an existing archive, but it could also be a github repository, in principle.

Then we also have the homepage entry, which denotes the homepage of the program (or any other webpage that has relevant information, in the event that there no homepage entry exists for that particular program).

Then we have tags: which just groups the program into popular tag-classifiers, such as "shell" in this case. Then the prefix in use, which is f for false, meaning "non-traditional" aka "appdir" prefix such as GoboLinux makes use of. Then there is the keep_extracted setting, which tells us whether to keep the archive extracted or not after extracting/compiling it. Last but not least, the last_update tag tells us when this program was last updated. The default format here is dd.mm.yyy, but shortcuts are allowed. For example, Oct, for October, can be used rather than 10, and so on.

The next entry is prefix. This one is mostly synonymous for the --prefix= option given to GNU autoconfigure based projects.

A prefix value of t means true, which stands for /usr/. A prefix value of f means false, which stands for the particular AppDir-like prefix. In the case here, for fish, this would expand to /Programs/Fish/2.7.1. GoboLinux uses a very similar AppDir layout by default, although with slightly different program names that can also be upcased, e. g. "KDE-Libs" and so forth. (the RSM project will offer both variants here; the GoboLinux naming scheme, and the simpler scheme where only the first character is upcased, and the other characters are all downcased.)

There are many more entries, most of which should be documented under the doc/ subdirectory of this gem. What follows next are some more mentions of important (and less important) entries.

The entry configure_base_dir: unix determines which base directory is to be used. In the above example, RBT would cd into the directory called unix/. This is important for some programs such as tk or xvid - have a look at their directory structure to understand this setting.

Since as of December 2018 it is possible to use github URL that are truncated. Take the program conky as an example. It has an url like the following:

https://github.com/brndnmtthws/conky/archive/v1.11.0.tar.gz

This will now be automatically expanded into the correct naming scheme, aka **conky-1.11.0**. No idea why GitHub is not doing so on their own automatically; the tarball is even wrongly built, since it extracts to precisely that name after download. But anyway, ask GitHub about this, not me.

The content of these individual .yml files is essentially just a Hash that describes the various attributes of the given project at hand (and some additional information when it is required for installing/compiling this program).

Note that since as of January 2020, you can also just provide a new URL and register a program this way anew.

Commandline Example for this:

ry http://ftp.gnu.org/pub/gnu/kawa/kawa-3.1.tar.gz


Packaging the SoftwareManager scripts
You can package all the latest SoftwareManager scripts by using a commandline flag such as this:

ry package_cookbooks
ry --package_cookbooks

This will create a directory in /depot/archives/ and package the SoftwareManager scripts into that directory.

Of course you can do so manually as well, but I wanted to have this functionality at the least available for when a user is on windows and may quickly and conveniently need to transfer the files, for whatever the reason(s).

Remove Program
To remove a program, you can use:

remove_this_program

On GoboLinux this would be done via RemoveProgram.

Porg
You can use the program called porg to install a program.

Porg will keep track of program files that are installed.

You can also disable porg from the commandline via:

ry --disable-porg
ry --disable_porg
ry --disableporg

Or just directly edit the respective .yml file.

Available programs versions

0install                  2.3.14       22 Feb 2021  https://sourceforge.net/projects/zero-install/files/0install/2.3.14/0install-2.3.14.tar.bz2
3ddesktop                 0.2.9        01 Jun 2014  https://sourceforge.net/projects/desk3d/files/3ddesktop/0.2.9/3ddesktop-0.2.9.tar.gz
3dpong                    0.5          01 May 2014  https://tuxpaint.org/ftp/unix/x/3dpong/src/3dpong-0.5.tar.gz
a2png                     0.1.5        01 Jun 2014  http://sourceforge.net/projects/a2png/files/a2png/0.1.5/a2png-0.1.5.tar.gz
a2ps                      4.15.5       25 Jun 2023  https://ftp.gnu.org/pub/gnu/a2ps/a2ps-4.15.5.tar.gz
a52dec                    0.7.4        01 Oct 2013  http://liba52.sourceforge.net/files/a52dec-0.7.4.tar.gz
aalib                     1.4rc5       01 Jan 2013  http://sourceforge.net/projects/aa-project/files/aa-lib/1.4rc5/aalib-1.4rc5.tar.gz
aamath                    0.3          01 May 2014  http://fuse.superglue.se/aamath/aamath-0.3.tar.gz
abcde                     2.8.1        23 Oct 2017  https://abcde.einval.com/download/abcde-2.8.1.tar.gz
abe                       1.1          01 May 2014  https://sourceforge.net/projects/abe/files/abe/abe-1.1/abe-1.1.tar.gz
abiword                   3.0.5        04 Jul 2021  https://www.abisource.com/downloads/abiword/3.0.5/source/abiword-3.0.5.tar.gz
abnfgen                   0.17         16 Sep 2016  http://www.quut.com/abnfgen/abnfgen-0.17.tar.gz
abstk                     0.1          07 Feb 2016  https://gobolinux.org/abstk/AbsTK-0.1.tar.gz
abuse                     0.8          01 Dec 2012  https://abuse.zoy.org/raw-attachment/wiki/download/abuse-0.8.tar.gz
accerciser                3.42.0       16 Oct 2023  https://download.gnome.org/sources/accerciser/3.42/accerciser-3.42.0.tar.xz
accountsservice           22.08.8      07 Mar 2022  https://www.freedesktop.org/software/accountsservice/accountsservice-22.08.8.tar.xz
acct                      6.6.4        11 Nov 2019  http://ftp.gnu.org/gnu/acct/acct-6.6.4.tar.bz2
acl                       2.3.2        06 Feb 2024  https://download.savannah.gnu.org/releases/acl/acl-2.3.2.tar.xz
acoc                      0.7.1        24 Nov 2019  https://rubygems.org/downloads/acoc-0.7.1.gem
acpi                      1.7          28 Feb 2023  https://sourceforge.net/projects/acpiclient/files/acpiclient/1.7/acpi-1.7.tar.gz
acpicaunix                20080701     11 Oct 2017  http://www.acpica.org/download/acpica-unix-20080701.tar.gz
acpid                     2.0.34       18 Sep 2022  http://downloads.sourceforge.net/acpid2/acpid-2.0.34.tar.xz
acpitool                  0.5.1        12 Oct 2017  https://sourceforge.net/projects/acpitool/files/acpitool/0.5.1/acpitool-0.5.1.tar.gz
actioncable               7.0.3.1      28 Aug 2022  https://rubygems.org/downloads/actioncable-7.0.3.1.gem
actionmailer              7.0.3        27 May 2022  https://rubygems.org/downloads/actionmailer-7.0.3.gem
actionpack                7.0.3.1      09 Sep 2022  https://rubygems.org/downloads/actionpack-7.0.3.1.gem
actionview                7.0.3.1      26 Aug 2022  https://rubygems.org/downloads/actionview-7.0.3.1.gem
actionwebservice          1.2.6        21 Sep 2017  https://rubygems.org/downloads/actionwebservice-1.2.6.gem
activejob                 7.0.3        27 May 2022  https://rubygems.org/downloads/activejob-7.0.3.gem
activemodel               7.0.3        27 May 2022  https://rubygems.org/downloads/activemodel-7.0.3.gem
activerecord              7.0.3        27 May 2022  https://rubygems.org/downloads/activerecord-7.0.3.gem
activeresource            6.0.0        27 May 2022  https://rubygems.org/downloads/activeresource-6.0.0.gem
activestorage             7.0.3        27 May 2022  https://rubygems.org/downloads/activestorage-7.0.3.gem
activesupport             7.0.3        27 May 2022  https://rubygems.org/downloads/activesupport-7.0.3.gem
addressable               2.8.0        27 May 2022  https://rubygems.org/downloads/addressable-2.8.0.gem
adwaitaicontheme          46.2         07 Jul 2024  https://ftp.acc.umu.se/pub/GNOME/sources/adwaita-icon-theme/46/adwaita-icon-theme-46.2.tar.xz
aegisub                   3.2.2        24 Jan 2018  http://ftp.aegisub.org/pub/archives/releases/source/aegisub-3.2.2.tar.xz
aeolus                    0.9.7        24 Nov 2019  http://kokkinizita.linuxaudio.org/linuxaudio/downloads/aeolus-0.9.7.tar.bz2
afew                      2.0.0        24 Nov 2019  https://files.pythonhosted.org/packages/56/db/75c67cf650f64d5c5a294cbead17856aaadb4517911ba6f5de91d843c16a/afew-2.0.0.tar.gz
afterstep                 2.2.12       21 May 2020  ftp://ftp.afterstep.org/stable/AfterStep-2.2.12.tar.bz2
aircrackng                1.3          18 Sep 2018  http://download.aircrack-ng.org/aircrack-ng-1.3.tar.gz
airsnort                  0.2.7e       01 Oct 2013  http://sourceforge.net/projects/airsnort/files/airsnort/airsnort-0.2.7e/airsnort-0.2.7e.tar.gz
airstrike                 pre6a        01 Dec 2012  http://icculus.org/airstrike/airstrike-pre6a.tar.gz
aisleriot                 3.22.9       29 Sep 2019  http://ftp.gnome.org/pub/gnome/sources/aisleriot/3.22/aisleriot-3.22.9.tar.xz
akonadi                   24.12.0      12 Dec 2024  https://download.kde.org/stable/release-service/24.12.0/src/akonadi-24.12.0.tar.xz
akonadicalendar           24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akonadi-calendar-24.02.2.tar.xz
akonadicalendartools      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akonadi-calendar-tools-24.02.2.tar.xz
akonadiconsole            24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akonadiconsole-24.02.2.tar.xz
akonadicontacts           24.12.1      22 Jan 2025  https://download.kde.org/stable/release-service/24.12.1/src/akonadi-contacts-24.12.1.tar.xz
akonadiimportwizard       24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akonadi-import-wizard-24.02.2.tar.xz
akonadimime               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akonadi-mime-24.02.2.tar.xz
akonadinotes              24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akonadi-notes-24.02.2.tar.xz
akonadisearch             24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akonadi-search-24.02.2.tar.xz
akregator                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/akregator-24.02.2.tar.xz
alacarte                  3.52.0       24 Mar 2024  https://download.gnome.org/sources/alacarte/3.52/alacarte-3.52.0.tar.xz
alien                     8.95         06 Oct 2015  http://http.debian.net/debian/pool/main/a/alien/alien_8.95.tar.xz
allegro                   5.2.9.1      20 Feb 2024  https://github.com/liballeg/allegro5/releases/download/5.2.9.1/allegro-5.2.9.1.tar.gz
alligator                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/alligator-24.02.2.tar.xz
alltray                   0.70         12 Nov 2017  https://launchpad.net/alltray/historic-releases/0.70/+download/alltray-0.70.tar.gz
almanah                   0.12.3       08 Mar 2021  https://download.gnome.org/sources/almanah/0.12/almanah-0.12.3.tar.xz
alock                     94           01 Jul 2011  http://alock.googlecode.com/files/alock-94.tar.bz2
alsadriver                1.0.25       20 Jun 2015  ftp://ftp.alsa-project.org/pub/driver/alsa-driver-1.0.25.tar.bz2
alsafirmware              1.2.4        23 Oct 2020  https://www.alsa-project.org/files/pub/firmware/alsa-firmware-1.2.4.tar.bz2
alsalib                   1.2.13       14 Nov 2024  https://www.alsa-project.org/files/pub/lib/alsa-lib-1.2.13.tar.bz2
alsaoss                   1.1.8        26 Jan 2019  ftp://ftp.alsa-project.org/pub/oss-lib/alsa-oss-1.1.8.tar.bz2
alsaplayer                0.99.81      01 Jun 2013  https://alsaplayer.sourceforge.net/alsaplayer-0.99.81.tar.gz
alsaplugins               1.2.7.1      21 Jun 2022  https://www.alsa-project.org/files/pub/plugins/alsa-plugins-1.2.7.1.tar.bz2
alsatools                 1.2.5        05 Jun 2021  https://www.alsa-project.org/files/pub/tools/alsa-tools-1.2.5.tar.bz2
alsautils                 1.2.13       14 Nov 2024  https://www.alsa-project.org/files/pub/utils/alsa-utils-1.2.13.tar.bz2
amarok                    3.0.1        19 Jul 2024  https://download.kde.org/stable/amarok/3.0.1/amarok-3.0.1.tar.xz
amidiplug                 0.7          01 Nov 2012  http://www.develia.org/files/amidi-plug-0.7.tar.bz2
amigadepacker             0.04         01 Jun 2014  http://zakalwe.fi/~shd/foss/amigadepacker/amigadepacker-0.04.tar.bz2
amoebax                   0.2.1        01 Jan 2013  https://www.emma-soft.com/games/amoebax/download/amoebax-0.2.1.tar.bz2
amphetamine               0.8.10       01 Jun 2014  http://n.ethz.ch/student/loehrerl/amph/files/amphetamine-0.8.10.tar.bz2
amtk                      5.6.1        17 Nov 2022  https://download.gnome.org/sources/amtk/5.6/amtk-5.6.1.tar.xz
amule                     2.3.1        01 Jun 2014  https://sourceforge.net/projects/amule/files/aMule/2.3.1/aMule-2.3.1.tar.xz
analitza                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/analitza-24.02.2.tar.xz
analogger                 1.1.0        08 Jul 2018  https://rubygems.org/downloads/analogger-1.1.0.gem
angelfish                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/angelfish-24.02.2.tar.xz
anjuta                    3.34.0       09 Sep 2019  https://ftp.gnome.org/pub/gnome/sources/anjuta/3.34/anjuta-3.34.0.tar.xz
anjutaextras              3.26.0       11 Sep 2017  http://ftp.gnome.org/pub/gnome/sources/anjuta-extras/3.26/anjuta-extras-3.26.0.tar.xz
ansi                      1.5.0        09 Jan 2018  https://rubygems.org/downloads/ansi-1.5.0.gem
antiword                  0.37         01 May 2014  http://www.winfield.demon.nl/linux/antiword-0.37.tar.gz
anyremote                 6.7.3        07 Dec 2019  https://sourceforge.net/projects/anyremote/files/anyremote/6.7.3/anyremote-6.7.3.tar.gz
aoetools                  36           22 Feb 2017  https://downloads.sourceforge.net/project/aoetools/aoetools/aoetools-36.tar.gz
apacheant                 1.10.2       19 Feb 2018  https://archive.apache.org/dist/ant/source/apache-ant-1.10.2.tar.xz
apachemaven               3.8.6        24 May 2022  https://dlcdn.apache.org/maven/maven-3/3.8.5/binaries/apache-maven-3.8.6-bin.tar.gz
apl                       1.8          15 Dec 2019  http://ftp.gnu.org/gnu/apl/apl-1.8.tar.gz
apophenia                 19.01.2023   18 Jan 2023  http://osdn.dl.sourceforge.net/sourceforge/apophenia/apophenia-19.01.2023.tgz
aporia                    27.12.2011   01 Jun 2014  aporia-27.12.2011
appdatatools              0.1.8        01 Sep 2014  http://people.freedesktop.org/~hughsient/releases/appdata-tools-0.1.8.tar.xz
appfs                     1.14         23 Sep 2020  http://www.rkeene.org/devel/appfs/appfs-1.14.tar.gz
appres                    1.0.7        17 Jun 2024  https://www.x.org/releases/individual/app/appres-1.0.7.tar.xz
appstream                 1.0.4        11 Dec 2024  https://www.freedesktop.org/software/appstream/releases/AppStream-1.0.4.tar.xz
appstreamglib             0.8.3        20 Sep 2024  https://people.freedesktop.org/~hughsient/appstream-glib/releases/appstream-glib-0.8.3.tar.xz
apr                       1.7.5        04 Sep 2024  https://dlcdn.apache.org//apr/apr-1.7.5.tar.gz
apricots                  0.2.6        01 May 2014  http://www.fishies.org.uk/apricots-0.2.6.tar.gz
aproc                     0.1          22 Mar 2016  http://stinfwww.informatik.uni-leipzig.de/~mai00bgn/aproc/tb/aproc-0.1.tar.bz2
aprutil                   1.6.3        03 Feb 2023  http://archive.apache.org/dist/apr/apr-util-1.6.3.tar.bz2
apt                       2.1.7        06 Aug 2019  http://ftp.debian.org/debian/pool/main/a/apt/apt_2.1.7.tar.xz
apulse                    02.10.2017   02 Oct 2017  http://freedesktop.org/software/pulseaudio/releases/apulse-02.10.2017
arandr                    0.1.10       01 Aug 2014  http://christian.amsuess.com/tools/arandr/files/arandr-0.1.10.tar.gz
aravis                    0.8.10       13 May 2021  https://download.gnome.org/sources/aravis/0.8/aravis-0.8.10.tar.xz
archivezip                1.68         16 Mar 2020  https://cpan.metacpan.org/authors/id/P/PH/PHRED/Archive-Zip-1.68.tar.gz
arel                      9.0.0        22 Jul 2018  https://rubygems.org/downloads/arel-9.0.0.gem
argtable2                 13           09 Sep 2018  https://sourceforge.net/projects/argtable/files/argtable/argtable-2.13/argtable2-13.tar.gz
aria2                     1.37.0       23 Sep 2024  https://github.com/aria2/aria2/releases/download/release-1.37.0/aria2-1.37.0.tar.xz
arianna                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/arianna-24.02.2.tar.xz
aris                      2.2          11 Dec 2017  https://ftp.gnu.org/gnu/aris/aris-2.2.tar.gz
ark                       24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/ark-24.02.2.tar.xz
armagetronad              0.2.9.1.0    14 Apr 2021  https://launchpad.net/armagetronad/0.2.9/0.2.9.1.0/+download/armagetronad-0.2.9.1.0.tbz
art                       5.3          01 Dec 2021  https://files.pythonhosted.org/packages/97/74/6d99019856b638a697547107e5904513452ff01f8036b52307eabdddc500/art-5.3.tar.gz
artanis                   0.4.1        26 Dec 2019  http://ftp.gnu.org/pub/gnu/artanis/artanis-0.4.1.tar.bz2
artemis                   v17.0.1      26 May 2020  ftp://ftp.sanger.ac.uk/pub4/resources/software/artemis/v17/v17.0.1/artemis-v17.0.1.jar
artikulate                24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/artikulate-24.02.2.tar.xz
asc                       2.6.1.0      14 Feb 2016  https://sourceforge.net/projects/asc-hq/files/ASC%20Source/2.6.1/asc-2.6.1.0.tar.bz2
ascal                     0.1.1        01 May 2014  https://sourceforge.net/projects/ascal/files/ascal/the%20last%20one%20this%20year/ascal-0.1.1.tar.bz2
asciidoc                  9.0.2        23 Jul 2020  https://github.com/asciidoc/asciidoc-py3/releases/download/9.0.2/asciidoc-9.0.2.tar.gz
asciidoctor               2.0.23       19 May 2024  https://rubygems.org/downloads/asciidoctor-2.0.23.gem
asciimatics               1.13.0       26 Mar 2022  https://files.pythonhosted.org/packages/ef/14/9021858085fc372a1df52bc3f216aa5defa4ba743859654e4bd9e3f85fcb/asciimatics-1.13.0.tar.gz
aspell                    0.60.8.1     01 Mar 2024  https://ftp.gnu.org/gnu/aspell/aspell-0.60.8.1.tar.gz
ast                       2.4.2        15 Apr 2021  https://rubygems.org/downloads/ast-2.4.2.gem
asunder                   2.9.5        09 Jan 2020  http://littlesvr.ca/asunder/releases/asunder-2.9.5.tar.bz2
asymptote                 2.90         13 Jul 2024  https://downloads.sourceforge.net/asymptote/asymptote-2.90.tgz
aterm                     1.0.1        01 May 2014  ftp://ftp.afterstep.org/apps/aterm/aterm-1.0.1.tar.bz2
atk                       2.38.0       25 Mar 2022  https://download.gnome.org/sources/atk/2.38/atk-2.38.0.tar.xz
atkmm                     2.36.3       08 Feb 2024  https://download.gnome.org/sources/atkmm/2.36/atkmm-2.36.3.tar.xz
atomix                    3.34.0       11 Jan 2020  http://ftp.gnome.org/pub/gnome/sources/atomix/3.34/atomix-3.34.0.tar.xz
atool                     0.39.0       01 May 2014  http://savannah.nongnu.org/download/atool/atool-0.39.0.tar.gz
atril                     1.28.1       16 Dec 2024  https://pub.mate-desktop.org/releases/1.28/atril-1.28.1.tar.xz
atspi                     1.32.0       01 Dec 2012  http://ftp.gnome.org/pub/gnome/sources/at-spi/1.32/at-spi-1.32.0.tar.bz2
atspi2atk                 2.38.0       13 Sep 2020  http://ftp.gnome.org/pub/gnome/sources/at-spi2-atk/2.38/at-spi2-atk-2.38.0.tar.xz
atspi2core                2.55.0.1     16 Jan 2025  https://download.gnome.org/sources/at-spi2-core/2.55/at-spi2-core-2.55.0.1.tar.xz
attica                    6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/attica-6.8.0.tar.xz
attr                      2.5.2        07 Feb 2024  https://download.savannah.gnu.org/releases/attr/attr-2.5.2.tar.gz
attrs                     22.2.0       17 Mar 2024  https://files.pythonhosted.org/packages/source/a/attrs/attrs-22.2.0.tar.gz
aubio                     0.4.7        06 Jun 2019  https://aubio.org/pub/aubio-0.4.7.tar.bz2
audacious                 4.3          07 Mar 2023  https://distfiles.audacious-media-player.org/audacious-4.3.tar.bz2
audaciousplugins          4.1          11 May 2021  https://distfiles.audacious-media-player.org/audacious-plugins-4.1.tar.bz2
audacitysources           3.2.0        23 Sep 2022  https://www.fosshub.com/Audacity.html?dwl=audacity-sources-3.2.0.tar.gz
audiere                   1.9.4        01 May 2014  http://prdownloads.sourceforge.net/audiere/audiere-1.9.4.tar.gz
audiocdkio                24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/audiocd-kio-24.02.2.tar.xz
audiofile                 0.3.6        01 May 2014  https://github.com/downloads/mpruett/audiofile/audiofile-0.3.6.tar.gz
audiotube                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/audiotube-24.02.2.tar.xz
augeas                    1.12.0       03 Feb 2020  http://download.augeas.net/augeas-1.12.0.tar.gz
aumix                     2.9.1        17 Nov 2015  http://sourceforge.net/projects/aumix/files/aumix/2.9.1/aumix-2.9.1.tar.bz2
aurabrowser               5.27.11      06 Mar 2024  https://download.kde.org/stable/plasma/5.27.11/aura-browser-5.27.11.tar.xz
autoconf                  2.72         23 Dec 2023  https://ftp.gnu.org/gnu/autoconf/autoconf-2.72.tar.xz
autoconfarchive           2023.02.20   22 Feb 2023  https://ftp.gnu.org/pub/gnu/autoconf-archive/autoconf-archive-2023.02.20.tar.xz
autofs                    5.1.9        11 Nov 2023  https://www.kernel.org/pub/linux/daemons/autofs/v5/autofs-5.1.9.tar.xz
autogen                   5.18.16      09 Sep 2018  https://ftp.gnu.org/pub/gnu/autogen/rel5.18.16/autogen-5.18.16.tar.xz
automake                  1.17         12 Jul 2024  https://ftp.gnu.org/gnu/automake/automake-1.17.tar.xz
automoc4                  0.9.88       01 Jan 2014  http://download.kde.org/stable/automoc4/0.9.88/automoc4-0.9.88.tar.bz2
autotoolset               0.11.4       01 May 2014  autotoolset-0.11.4
autovivification          0.18         16 Sep 2020  http://search.cpan.org/CPAN/authors/id/V/VP/VPIT/autovivification-0.18.tar.gz
avahi                     0.8          02 Mar 2020  https://avahi.org/download/avahi-0.8.tar.gz
avantwindownavigator      0.4.2        01 Sep 2014  https://launchpad.net/awn/0.4/0.4.2/+download/avant-window-navigator-0.4.2.tar.gz
avc                       0.8.2        01 May 2014  http://avc.inrim.it/dist/avc-0.8.2.tar.gz
avidemux                  2.7.6        07 Mar 2021  http://downloads.sourceforge.net/avidemux/avidemux_2.7.6.tar.gz
7                         0.7.45       01 May 2014  https://sourceforge.net/projects/avifile/files/avifile/0.7.45-20060306/avifile-0.7-0.7.45.tar.bz2
avinfo                    1.0a16       01 May 2014  http://shounen.ru/soft/avinfo/avinfo-1.0a16.zip
awesome                   4.3          28 Jan 2019  https://github.com/awesomeWM/awesome/releases/download/v4.3/awesome-4.3.tar.xz
axel                      2.17.11      10 Sep 2022  https://github.com/axel-download-accelerator/axel/releases/download/v2.17.11/axel-2.17.11.tar.xz
axtls                     2.1.5        03 May 2020  https://sourceforge.net/projects/axtls/files/2.1.5/axTLS-2.1.5.tar.gz
axv                       0.2          10 May 2020  https://sourceforge.net/projects/axv/files/axv/0.2/axv.0.2.tar.gz
b43fwcutter               015          01 May 2014  http://bues.ch/b43/fwcutter/b43-fwcutter-015.tar.bz2
babl                      0.1.110      17 Jan 2025  https://download.gimp.org/pub/babl/0.1/babl-0.1.110.tar.xz
backports                 3.23.0       27 May 2022  https://rubygems.org/downloads/backports-3.23.0.gem
bacula                    9.6.3        03 May 2020  https://sourceforge.net/projects/bacula/files/bacula/9.6.3/bacula-9.6.3.tar.gz
bakery                    2.5.1        01 May 2014  http://ftp.gnome.org/pub/GNOME/sources/bakery/2.5/bakery-2.5.1.tar.bz2
ballbuster                1.1          01 Jan 2013  http://www.patrickavella.com/ballbuster-1.1.tar.bz2
baloo                     6.10.0       19 Jan 2025  https://download.kde.org/stable/frameworks/6.10/baloo-6.10.0.tar.xz
baloowidgets              24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/baloo-widgets-24.02.2.tar.xz
balsa                     2.6.4        24 Sep 2022  https://pawsa.fedorapeople.org/balsa/balsa-2.6.4.tar.xz
bandit                    1.5.1        02 Feb 2019  https://files.pythonhosted.org/packages/c9/60/2c967faf70596fcef42a0737c63fe1321b8e51e15eec8f7883e333eba5a5/bandit-1.5.1.tar.gz
baobab                    46.0         22 Mar 2024  https://download.gnome.org/sources/baobab/46/baobab-46.0.tar.xz
barcode                   0.99         11 May 2020  https://ftp.gnu.org/gnu/barcode/barcode-0.99.tar.xz
barrage                   1.0.6        04 Mar 2023  https://sourceforge.net/projects/lgames/files/barrage/barrage-1.0.6.tar.gz
bash                      5.2.37       24 Sep 2024  https://ftp.gnu.org/gnu/bash/bash-5.2.37.tar.gz
bashcompletion            2.10         11 May 2020  https://github.com/scop/bash-completion/releases/download/2.10/bash-completion-2.10.tar.xz
bazar                     1.3.1        15 May 2020  https://www.epfl.ch/labs/cvlab/wp-content/uploads/2018/08/bazar-1.3.1.tar.gz
bc                        6.7.6        08 Sep 2024  https://github.com/gavinhoward/bc/releases/download/6.7.6/bc-6.7.6.tar.xz
bcftools                  1.15.1       05 Aug 2022  https://sourceforge.net/projects/samtools/files/samtools/1.15.1/bcftools-1.15.1.tar.bz2
bchunk                    1.2.2        22 May 2020  https://github.com/hessu/bchunk/archive/release/bchunk-1.2.2.tar.gz
bdftopcf                  1.1          09 Nov 2017  https://www.x.org/releases/individual/app/bdftopcf-1.1.tar.gz
beagle                    0.3.6        01 May 2014  ftp://ftp.gnome.org/pub/GNOME/sources/beagle/0.3/beagle-0.3.6.tar.bz2
bedtools                  2.30.0       27 Dec 2021  https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools-2.30.0.tar.gz
beecrypt                  4.2.1        01 May 2014  https://sourceforge.net/projects/beecrypt/files/beecrypt/4.2.1/beecrypt-4.2.1.tar.gz
beforelight               1.0.6        29 Jan 2023  https://xorg.freedesktop.org/releases/individual/app/beforelight-1.0.6.tar.xz
benchmarkips              2.10.0       27 May 2022  https://rubygems.org/downloads/benchmark-ips-2.10.0.gem
bettererrors              2.9.1        15 Apr 2021  https://rubygems.org/downloads/better_errors-2.9.1.gem
bftpd                     5.0          29 Oct 2018  https://sourceforge.net/projects/bftpd/files/bftpd/bftpd-5.0/bftpd-5.0.tar.gz
bigdecimal                3.1.2        27 May 2022  https://rubygems.org/downloads/bigdecimal-3.1.2.gem
bigreqsproto              1.1.2        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/proto/bigreqsproto-1.1.2.tar.bz2
bijiben                   40.1         30 Apr 2021  https://download.gnome.org/sources/bijiben/40/bijiben-40.1.tar.xz
biloba                    0.9.3        12 Mar 2019  https://sourceforge.net/projects/biloba/files/Biloba/0.9.3/biloba-0.9.3.tar.gz
bind                      9.20.4       11 Dec 2024  https://ftp.isc.org/isc/bind9/9.20.4/bind-9.20.4.tar.xz
binutils                  2.43.1       21 Jan 2025  https://ftp.gnu.org/gnu/binutils/binutils-2.43.1.tar.gz
bio                       2.0.4        17 Sep 2022  https://rubygems.org/downloads/bio-2.0.4.gem
bioperl                   1.007002     02 Jan 2018  https://cpan.metacpan.org/authors/id/C/CJ/CJFIELDS/BioPerl-1.007002.tar.gz
biopython                 1.84         09 Sep 2024  http://biopython.org/DIST/biopython-1.84.tar.gz
biosdevname               0.7.2        15 May 2020  http://linux.dell.com/files/biosdevname/biosdevname-0.7.2/biosdevname-0.7.2.tar.gz
bird                      2.0.7        05 Mar 2021  https://bird.network.cz/download/bird-2.0.7.tar.gz
bison                     3.8.2        25 Sep 2021  ftp://ftp.gnu.org/gnu/bison/bison-3.8.2.tar.xz
bitcoin                   16.04.2024   16 Apr 2024  https://download.kde.org/stable/bitcoin-16.04.2024.tar.xz
bitlbee                   3.6          15 May 2020  http://get.bitlbee.org/src/bitlbee-3.6.tar.gz
bitmap                    1.1.1        20 Feb 2024  https://www.x.org/releases/individual/app/bitmap-1.1.1.tar.xz
bittorrent                4.4.0        01 Jun 2014  http://www.bittorrent.com/dl/BitTorrent-4.4.0.tar.gz
bkchem                    0.13.0       01 Feb 2014  http://bkchem.zirael.org/download/bkchem-0.13.0.tar.gz
blackbox                  0.77         06 Mar 2024  https://github.com/bbidulock/blackboxwm/releases/download/0.77/blackbox-0.77.tar.lz
blender                   3.3.0        07 Sep 2022  https://download.blender.org/source/blender-3.3.0.tar.xz
bless                     0.5.2        01 Nov 2012  http://download.gna.org/bless/bless-0.5.2.tar.gz
blessings                 1.7          16 May 2020  https://files.pythonhosted.org/packages/5c/f8/9f5e69a63a9243448350b44c87fae74588aa634979e6c0c501f26a4f6df7/blessings-1.7.tar.gz
blinken                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/blinken-24.02.2.tar.xz
blobwars                  1.04-1       01 Jan 2013  http://www.parallelrealities.co.uk/download.php?file=blobwars-1.04-1.tar.gz&type=zip
blocaled                  0.2          22 Dec 2019  https://github.com/pierre-labastie/blocaled/releases/download/v0.2/blocaled-0.2.tar.xz
blockattacklinux          2.5.0        15 May 2020  https://github.com/blockattack/blockattack-game/releases/download/v2.5.0/blockattack-linux-2.5.0.tar.bz2
bluedevil                 6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/bluedevil-6.2.5.tar.xz
bluefish                  2.2.15       20 Sep 2024  https://www.bennewitz.com/bluefish/stable/source/bluefish-2.2.15.tar.bz2
blueman                   2.4.3        26 Jul 2024  https://github.com/blueman-project/blueman/releases/download/2.4.3/blueman-2.4.3.tar.xz
bluez                     5.79         02 Nov 2024  https://mirrors.edge.kernel.org/pub/linux/bluetooth/bluez-5.79.tar.xz
bluezalsa                 11.09.2022   11 Sep 2022  bluez-alsa-11.09.2022
bluezgnome                0.14         01 Jun 2014  http://www.kernel.org/pub/linux/bluetooth/bluez-gnome-0.14.tar.gz
bluezqt                   6.10.0       15 Jan 2025  https://download.kde.org/stable/frameworks/6.10/bluez-qt-6.10.0.tar.xz
bluezutils                2.25         28 Jul 2016  http://bluez.sf.net/download/bluez-utils-2.25.tar.gz
bogofilter                1.2.5        21 Oct 2019  https://sourceforge.net/projects/bogofilter/files/bogofilter-stable/bogofilter-1.2.5.tar.xz
bomber                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/bomber-24.02.2.tar.xz
bomberclone               0.11.9       10 Oct 2017  https://sourceforge.net/projects/bomberclone/files/bomberclone/bomberclone-0.11.9.tar.gz
bombic                    0.0.1        01 Jan 2013  https://sourceforge.net/projects/bombic/files/bombic/Bombic%200.0.1/bombic-0.0.1.tar.gz
bomns                     0.99.1       01 Jun 2014  http://ovh.dl.sourceforge.net/sourceforge/greenridge/bomns-0.99.1.tar.bz2
bookify                   2.9.0        17 Jun 2020  https://rubygems.org/downloads/bookify-2.9.0.gem
boost                     1_87_0       17 Jan 2025  https://sourceforge.net/projects/boost/files/boost/1.87.0/boost_1_87_0.tar.bz2
boswars                   2.7          01 Aug 2013  https://www.boswars.org/dist/releases/boswars-2.7.tar.gz
botocore                  1.12.146     12 May 2019  https://files.pythonhosted.org/packages/5a/4b/85cd24763385a680df8de460be28d195f44880a9727fe34b71273a9cdd95/botocore-1.12.146.tar.gz
bovo                      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/bovo-24.02.2.tar.xz
brasero                   3.12.2       01 Aug 2017  http://ftp.gnome.org/pub/gnome/sources/brasero/3.12/brasero-3.12.2.tar.xz
breeze                    6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/breeze-6.2.5.tar.xz
breezegrub                6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/breeze-grub-6.2.5.tar.xz
breezegtk                 6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/breeze-gtk-6.2.5.tar.xz
breezeicons               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/breeze-icons-6.8.0.tar.xz
breezeplymouth            6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/breeze-plymouth-6.2.5.tar.xz
bridgeutils               1.7.1        27 Mar 2021  https://mirrors.edge.kernel.org/pub/linux/utils/net/bridge-utils/bridge-utils-1.7.1.tar.xz
briquolo                  0.5.7        01 Jun 2014  http://briquolo.free.fr/download/briquolo-0.5.7.tar.bz2
brlcad                    7.26.0       22 Feb 2017  https://sourceforge.net/projects/brlcad/files/BRL-CAD%20Source/7.26.0/brlcad-7.26.0.tar.bz2
brltty                    6.0          25 Feb 2019  http://mielke.cc/brltty/archive/brltty-6.0.tar.xz
brotli                    1.1.0        21 Jan 2025  https://github.com/google/brotli/archive/v1.1.0/brotli-1.1.0.tar.gz
bsc                       2.27         01 Jun 2014  http://www.beesoft.org/download/bsc_2.27_src.tar.gz
btrbk                     0.27.1       06 Dec 2018  https://digint.ch/download/btrbk/releases/btrbk-0.27.1.tar.xz
btrfsprogs                v6.11        19 Sep 2024  https://mirrors.edge.kernel.org/pub/linux/kernel/people/kdave/btrfs-progs/btrfs-progs-v6.11.tar.xz
bubblewrap                0.10.0       21 Sep 2024  https://github.com/containers/bubblewrap/releases/download/v0.10.0/bubblewrap-0.10.0.tar.xz
162                       1            01 Jun 2014  https://launchpad.net/ubuntu/+source/bubbros/1.6.2-1
budgiedesktop             v10.5.3      28 Apr 2021  https://github.com/solus-project/budgie-desktop/releases/download/v10.5.3/budgie-desktop-v10.5.3.tar.xz
bugbuddy                  2.32.0       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/bug-buddy/2.32/bug-buddy-2.32.0.tar.bz2
builder                   3.2.4        28 May 2020  https://rubygems.org/downloads/builder-3.2.4.gem
buildroot                 2020.02.1    30 May 2020  https://git.busybox.net/buildroot/snapshot/buildroot-2020.02.1.tar.gz
buildstream               1.6.1        07 Sep 2021  https://download.gnome.org/sources/BuildStream/1.6/BuildStream-1.6.1.tar.xz
bullet                    3.09         17 Apr 2021  https://github.com/bulletphysics/bullet3/archive/bullet-3.09.tar.gz
bundler                   2.3.16       22 Jun 2022  https://rubygems.org/downloads/bundler-2.3.16.gem
burgerspace               1.9.3        31 May 2020  http://perso.b2b2c.ca/~sarrazip/dev/burgerspace-1.9.3.tar.gz
businessisbn              3.005        13 Dec 2019  https://www.cpan.org/authors/id/B/BD/BDFOY/Business-ISBN-3.005.tar.gz
busybox                   1.36.1       15 Jan 2025  https://www.busybox.net/downloads/busybox-1.36.1.tar.bz2
butt                      0.1.17       04 Mar 2019  http://sourceforge.net/projects/butt/files/butt/butt-0.1.17/butt-0.1.17.tar.gz
byebug                    11.1.3       28 May 2020  https://rubygems.org/downloads/byebug-11.1.3.gem
bytename                  1.12         01 Jun 2020  http://billposer.org/Software/Downloads/Bytename-1.12.tar.gz
bzip2                     1.0.8        14 Jul 2019  https://sourceware.org/pub/bzip2/bzip2-1.0.8.tar.gz
bzr                       2.7.0        14 Mar 2017  https://launchpad.net/bzr/2.7/2.7.0/+download/bzr-2.7.0.tar.gz
cabextract                1.9.1        03 Jun 2020  https://www.cabextract.org.uk/cabextract-1.9.1.tar.gz
cachetools                5.3.2        28 Oct 2023  https://files.pythonhosted.org/packages/10/21/1b6880557742c49d5b0c4dcf0cf544b441509246cdd71182e0847ac859d5/cachetools-5.3.2.tar.gz
cacti                     1.2.13       14 Jul 2020  http://www.cacti.net/downloads/cacti-1.2.13.tar.gz
cairo                     1.18.2       15 Jan 2025  https://www.cairographics.org/releases/cairo-1.18.2.tar.xz
cairodock                 3.4.1        04 Mar 2017  https://github.com/Cairo-Dock/cairo-dock-core/releases/download/3.4.1/cairo-dock-3.4.1.tar.gz
cairomm                   1.18.0       21 Mar 2024  https://www.cairographics.org/releases/cairomm-1.18.0.tar.xz
caja                      1.28.0       21 Feb 2024  https://pub.mate-desktop.org/releases/1.28/caja-1.28.0.tar.xz
cajadropbox               1.24.0       07 Dec 2022  https://pub.mate-desktop.org/releases/1.24/caja-dropbox-1.24.0.tar.xz
cajaextensions            1.28.0       23 Feb 2024  https://pub.mate-desktop.org/releases/1.28/caja-extensions-1.28.0.tar.xz
cajun                     4.2          01 Jun 2014  https://sourceforge.net/projects/cajun/files/cajun/4.2/cajun-4.2.tar.gz
cal3d                     0.11.0       28 Jul 2016  http://download.gna.org/cal3d/sources/cal3d-0.11.0.tar.gz
calendarsupport           24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/calendarsupport-24.02.2.tar.xz
calibre                   7.23.0       20 Dec 2024  https://download.calibre-ebook.com/7.23.0/calibre-7.23.0.tar.xz
calindori                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/calindori-24.02.2.tar.xz
callgrind                 0.9.12       01 Jun 2014  https://kcachegrind.sourceforge.net/callgrind-0.9.12.tar.gz
calligra                  4.0.0        27 Aug 2024  https://download.kde.org/stable/calligra/calligra-4.0.0.tar.xz
camping                   2.1.532      30 Nov 2017  https://rubygems.org/downloads/camping-2.1.532.gem
cantarellfonts            0.303.1      10 Dec 2021  https://download.gnome.org/sources/cantarell-fonts/0.303/cantarell-fonts-0.303.1.tar.xz
cantor                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/cantor-24.02.2.tar.xz
capybara                  3.37.1       27 May 2022  https://rubygems.org/downloads/capybara-3.37.1.gem
capybaramechanize         1.12.0       27 May 2022  https://rubygems.org/downloads/capybara-mechanize-1.12.0.gem
cares                     1.34.4       17 Dec 2024  https://github.com/c-ares/c-ares/releases/download/v1.34.4/c-ares-1.34.4.tar.gz
caribou                   0.4.20       16 Feb 2016  http://ftp.gnome.org/pub/GNOME/sources/caribou/0.4/caribou-0.4.20.tar.xz
castlecombat              0.8.1        28 Jul 2016  http://prdownloads.sourceforge.net/castle-combat/castle-combat-0.8.1.tar.gz
caxlsx                    3.2.0        27 May 2022  https://rubygems.org/downloads/caxlsx-3.2.0.gem
cbase                     1.3.7        28 Sep 2019  http://www.hyperrealm.com/packages/cbase-1.3.7.tar.gz
cbindgen                  0.27.0       15 Jan 2025  https://github.com/mozilla/cbindgen/archive/v0.27.0/cbindgen-0.27.0.tar.gz
ccache                    4.10.2       24 Jul 2024  https://github.com/ccache/ccache/releases/download/v4.10.2/ccache-4.10.2.tar.gz
ccaudio2                  2.2.0        19 Mar 2019  http://ftp.gnu.org/pub/gnu/ccaudio/ccaudio2-2.2.0.tar.gz
ccmath                    2.2.1        01 Jun 2014  http://www.ibiblio.org/pub/Linux/libs/ccmath-2.2.1.tar.gz
cdemuclient               3.2.1        27 Jun 2020  http://downloads.sourceforge.net/cdemu/cdemu-client-3.2.1.tar.bz2
cdemudaemon               3.0.4        28 Jul 2016  http://downloads.sourceforge.net/cdemu/cdemu-daemon-3.0.4.tar.bz2
cdrdao                    1.2.4        28 Jun 2020  https://sourceforge.net/projects/cdrdao/files/cdrdao-1.2.4.tar.bz2
cdrkit                    1.1.11       01 Jun 2014  http://cdrkit.org/releases/cdrkit-1.1.11.tar.gz
cdrtools                  3.02a09      29 May 2021  https://sourceforge.net/projects/cdrtools/files/alpha/cdrtools-3.02a09.tar.gz
ceferino                  0.97.8       01 Jun 2014  http://www.loosersjuegos.com.ar/_media/juegos/ceferino/descargas/ceferino-0.97.8.tar.gz
cegui                     0.8.7        30 Jun 2020  http://prdownloads.sourceforge.net/crayzedsgui/cegui-0.8.7.tar.bz2
celt                      0.11.3       08 Aug 2016  http://downloads.xiph.org/releases/celt/celt-0.11.3.tar.gz
centericq                 4.21.0       31 Oct 2017  http://thekonst.net/download/centericq-4.21.0.tar.bz2
ceph                      15.2.4       02 Jul 2020  http://download.ceph.com/tarballs/ceph-15.2.4.tar.gz
certifi                   2019.11.28   01 Dec 2019  https://files.pythonhosted.org/packages/41/bf/9d214a5af07debc6acf7f3f257265618f1db242a3f8e49a9b516f24523a6/certifi-2019.11.28.tar.gz
cervisia                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/cervisia-24.02.2.tar.xz
cfengine                  3.16.0       03 Jul 2020  https://cfengine-package-repos.s3.amazonaws.com/tarballs/cfengine-3.16.0.tar.gz
cffi                      1.15.0       13 Feb 2022  https://files.pythonhosted.org/packages/00/9e/92de7e1217ccc3d5f352ba21e52398372525765b2e0c4530e6eb2ba9282a/cffi-1.15.0.tar.gz
cfitsio                   4.5.0        12 Oct 2024  https://heasarc.gsfc.nasa.gov/FTP/software/fitsio/c/cfitsio-4.5.0.tar.gz
cflow                     1.7          30 Dec 2021  https://ftp.gnu.org/pub/gnu/cflow/cflow-1.7.tar.xz
cgiirc                    0.5.9        04 Jul 2020  https://sourceforge.net/projects/cgiirc/files/cgi-irc/0.5.9/cgiirc-0.5.9.tar.gz
cgilib                    0.6          01 Jun 2014  http://www.infodrom.org/projects/cgilib/download/cgilib-0.6.tar.gz
changepassword            0.9          01 Jun 2014  http://prdownloads.sourceforge.net/changepassword/changepassword-0.9.tar.gz
char                      linux        01 Jun 2014  http://ltsword.allegronetwork.com/char-linux.tar.gz
charlix                   0.4          01 Jun 2014  http://charlix.sourceforge.net/charlix-0.4.tar.gz
charsetnormalizer         2.0.9        06 Dec 2021  https://files.pythonhosted.org/packages/68/e4/e014e7360fc6d1ccc507fe0b563b4646d00e0d4f9beec4975026dd15850b/charset-normalizer-2.0.9.tar.gz
check                     0.15.2       09 Aug 2020  https://github.com/libcheck/check/releases/download/0.15.2/check-0.15.2.tar.gz
checkinstall              1.6.2        01 Jun 2014  http://asic-linux.com.mx/~izto/checkinstall/files/source/checkinstall-1.6.2.tar.gz
cheese                    44.0         10 Apr 2023  https://download.gnome.org/sources/cheese/44/cheese-44.0.tar.xz
cheetah                   2.4.4        15 Jan 2019  https://files.pythonhosted.org/packages/cd/b0/c2d700252fc251e91c08639ff41a8a5203b627f4e0a2ae18a6b662ab32ea/Cheetah-2.4.4.tar.gz
chemtool                  1.6.14       01 Mar 2014  http://ruby.chemie.uni-freiburg.de/~martin/chemtool/chemtool-1.6.14.tar.gz
cherokee                  1.2.99       01 Apr 2014  http://www.cherokee-project.com/download/1.2/1.2.99/cherokee-1.2.99.tar.gz
childprocess              4.1.0        27 May 2022  https://rubygems.org/downloads/childprocess-4.1.0.gem
chkrootkit                0.49         01 Jun 2014  ftp://ftp.pangeia.com.br/pub/seg/pac/chkrootkit-0.49.tar.gz
chmlib                    0.40         17 Mar 2017  http://www.jedrea.com/chmlib/chmlib-0.40.tar.gz
chromaprint               1.4.2        22 Sep 2018  https://bitbucket.org/acoustid/chromaprint/downloads/chromaprint-1.4.2.tar.gz
chromegnomeshell          10.1         02 Oct 2018  http://ftp.gnome.org/pub/gnome/sources/chrome-gnome-shell/10.1/chrome-gnome-shell-10.1.tar.xz
chromium                  64.0.3282.186 28 Sep 2019  https://ftp.osuosl.org/pub/blfs/conglomeration/chromium/chromium-64.0.3282.186.tar.xz
chromiumbsu               0.9.16.1     01 Jan 2017  https://sourceforge.net/projects/chromium-bsu/files/Chromium%20B.S.U.%20source%20code/chromium-bsu-0.9.16.1.tar.gz
chronic                   0.10.2       22 May 2020  https://rubygems.org/downloads/chronic-0.10.2.gem
chronojump                2.2.0        09 Jan 2022  https://download.gnome.org/sources/chronojump/2.2/chronojump-2.2.0.tar.xz
chrpath                   0.16         31 Oct 2015  https://alioth.debian.org/frs/download.php/latestfile/813/chrpath-0.16.tar.gz
chunkypng                 1.4.0        15 Apr 2021  https://rubygems.org/downloads/chunky_png-1.4.0.gem
cifsutils                 7.0          17 Apr 2024  https://download.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-7.0.tar.bz2
cinelerra                 7.3          08 Jul 2021  https://sourceforge.net/projects/heroines/files/cinelerra-7.3.tar.xz
cinepaint                 0.22.1       01 Jun 2014  http://sourceforge.net/projects/cinepaint/files/CinePaint/CinePaint-0.22-1/cinepaint-0.22.1.tar.gz
circuslinux               1.0.3        01 Jun 2014  ftp://ftp.tuxpaint.org/unix/x/circus-linux/src/circuslinux-1.0.3.tar.gz
clamav                    1.3.1        18 Apr 2024  https://github.com/Cisco-Talos/clamav/releases/download/clamav-1.3.1/clamav-1.3.1.tar.gz
clang                     14.0.0       28 Mar 2022  https://github.com/llvm/llvm-project/releases/download/llvmorg-14.0.0/clang-14.0.0.tar.xz
clanlib                   24.10.2015   24 Oct 2015  http://www.clanlib.org/download/releases-3.0/ClanLib-24.10.2015
classaccessor             0.51         28 Oct 2017  http://search.cpan.org/CPAN/authors/id/K/KA/KASEI/Class-Accessor-0.51.tar.gz
classlib                  0.1          01 Jun 2014  http://students.washington.edu/aemk/cm/releases/classlib-0.1.tar.gz
classtiny                 1.004        01 Apr 2016  http://search.cpan.org/CPAN/authors/id/D/DA/DAGOLDEN/Class-Tiny-1.004.tar.gz
wwwclaws                  mail         01 Jun 2014  https://www.claws-mail.org/
clearsilver               0.10.5       01 Jun 2014  http://www.clearsilver.net/downloads/clearsilver-0.10.5.tar.gz
click                     7.1.2        12 Jul 2020  https://files.pythonhosted.org/packages/27/6f/be940c8b1f1d69daceeb0032fee6c34d7bd70e3e649ccac0951500b4720e/click-7.1.2.tar.gz
clisp                     2.49         01 May 2014  https://sourceforge.net/projects/clisp/files/clisp/2.49/clisp-2.49.tar.gz
clive                     1.0.2        01 Jun 2014  http://download.gna.org/clive/1.0.x/clive-1.0.2.tar.bz2
cliver                    0.3.2        13 Aug 2018  https://rubygems.org/downloads/cliver-0.3.2.gem
cloog                     0.18.4       11 Feb 2017  http://www.bastoul.net/cloog/pages/download/cloog-0.18.4.tar.gz
cloogparma                0.16.1       01 Nov 2012  http://www.bastoul.net/cloog/pages/download/cloog-parma-0.16.1.tar.gz
cloogppl                  0.15.11      01 Nov 2012  ftp://gcc.gnu.org/pub/gcc/infrastructure/cloog-ppl-0.15.11.tar.gz
clucenecore               2.3.3.4      28 Jul 2016  https://sourceforge.net/projects/clucene/files/clucene-core-unstable/2.3/clucene-core-2.3.3.4.tar.gz
clustalomega              1.2.4        09 Sep 2018  http://www.clustal.org/omega/clustal-omega-1.2.4.tar.gz
clustalw                  2.1          11 Apr 2016  http://www.clustal.org/download/current/clustalw-2.1.tar.gz
clutter                   1.26.4       09 Mar 2020  https://download.gnome.org/sources/clutter/1.26/clutter-1.26.4.tar.xz
cluttergst                3.0.27       07 Feb 2019  http://ftp.gnome.org/pub/gnome/sources/clutter-gst/3.0/clutter-gst-3.0.27.tar.xz
cluttergtk                1.8.4        09 Aug 2017  http://ftp.gnome.org/pub/gnome/sources/clutter-gtk/1.8/clutter-gtk-1.8.4.tar.xz
cluttergtkmm              1.6.0        18 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/clutter-gtkmm/1.6/clutter-gtkmm-1.6.0.tar.xz
cluttermm                 1.17.3       01 May 2014  http://ftp.gnome.org/pub/GNOME/sources/cluttermm/1.17/cluttermm-1.17.3.tar.xz
cmake                     3.31.4       11 Jan 2025  https://cmake.org/files/v3.31/cmake-3.31.4.tar.gz
cmark                     0.29.0       26 May 2020  https://github.com/commonmark/cmark/archive/cmark-0.29.0.tar.gz
cmdftp                    0.9.7        01 Jun 2014  http://download.savannah.nongnu.org/releases/cmdftp/cmdftp-0.9.7.tar.gz
cmdparse                  3.0.7        06 Mar 2021  https://rubygems.org/downloads/cmdparse-3.0.7.gem
codeblocks                20.03        31 Mar 2020  https://sourceforge.net/projects/codeblocks/files/Sources/20.03/codeblocks-20.03.tar.xz
codebrowser               4.9          01 Dec 2012  http://tibleiz.net/download/code-browser-4.9.tar.gz
coderay                   1.1.3        17 Jun 2020  https://rubygems.org/downloads/coderay-1.1.3.gem
coffeescript              2.4.1        10 Dec 2017  https://rubygems.org/downloads/coffee-script-2.4.1.gem
cogito                    0.17.2       01 Jun 2014  http://www.kernel.org/pub/software/scm/cogito/cogito-0.17.2.tar.bz2
cogl                      1.22.8       06 Jun 2020  https://ftp.gnome.org/pub/gnome/sources/cogl/1.22/cogl-1.22.8.tar.xz
collectd                  5.11.0       30 Jul 2020  https://storage.googleapis.com/collectd-tarballs/collectd-5.11.0.tar.bz2
collectl                  4.3.1        30 Jul 2013  https://sourceforge.net/projects/collectl/files/collectl/collectl-4.3.1/collectl-4.3.1.tar.gz
colorama                  0.4.6        08 Mar 2023  https://files.pythonhosted.org/packages/d8/53/6f443c9a4a8358a93a6792e2acffb9d9d5cb0a5cfd8802644b7b1c9a02e4/colorama-0.4.6.tar.gz
colord                    1.4.6        19 Mar 2023  https://www.freedesktop.org/software/colord/releases/colord-1.4.6.tar.xz
colordgtk                 0.2.0        30 Jun 2019  https://www.freedesktop.org/software/colord/releases/colord-gtk-0.2.0.tar.xz
colordkde                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/colord-kde-24.02.2.tar.xz
colored                   1.4.3        07 Jul 2022  https://files.pythonhosted.org/packages/f4/57/fe3e4e96efa3c68d3781a0903de0933ea2afa744852d907b290a2cb2294e/colored-1.4.3.tar.gz
colortools                1.3.0        22 Jul 2017  https://rubygems.org/downloads/color-tools-1.3.0.gem
colour                    0.1.5        02 Dec 2018  https://files.pythonhosted.org/packages/a0/d4/5911a7618acddc3f594ddf144ecd8a03c29074a540f4494670ad8f153efe/colour-0.1.5.tar.gz
combinepdf                1.0.22       27 May 2022  https://rubygems.org/downloads/combine_pdf-1.0.22.gem
comix                     4.0.4        11 Oct 2017  https://sourceforge.net/projects/comix/files/comix/comix-4.0.4/comix-4.0.4.tar.gz
compface                  1.5.2        01 Jan 2013  http://ftp.xemacs.org/pub/xemacs/aux/compface-1.5.2.tar.gz
compilerrt                14.0.0       28 Mar 2022  https://github.com/llvm/llvm-project/releases/download/llvmorg-14.0.0/compiler-rt-14.0.0.tar.xz
compiz                    0.9.14.1     27 Nov 2019  https://launchpad.net/compiz/0.9.14/0.9.14.1/+download/compiz-0.9.14.1.tar.xz
compositeproto            0.4.2        08 Aug 2016  https://www.x.org/releases/individual/proto/compositeproto-0.4.2.tar.gz
comptonconf               0.16.0       06 Nov 2020  https://github.com/lxqt/compton-conf/releases/download/0.16.0/compton-conf-0.16.0.tar.xz
concurrentruby            1.1.10       27 May 2022  https://rubygems.org/downloads/concurrent-ruby-1.1.10.gem
confuse                   3.3          03 Aug 2020  https://github.com/martinh/libconfuse/releases/download/v3.3/confuse-3.3.tar.xz
v                         1.15.0       20 Nov 2024  https://github.com/brndnmtthws/conky/archive/refs/tags/v1.15.0.tar.gz
connections               3.38.1       19 Oct 2020  https://download.gnome.org/sources/connections/3.38/connections-3.38.1.tar.xz
connman                   1.33         22 Feb 2017  https://www.kernel.org/pub/linux/network/connman/connman-1.33.tar.xz
consolekit                0.4.6        01 Jun 2014  http://anduin.linuxfromscratch.org/sources/BLFS/svn/c/ConsoleKit-0.4.6.tar.xz
consolekit2               1.2.1        14 Nov 2018  https://github.com/Consolekit2/ConsoleKit2/releases/download/1.2.1/ConsoleKit2-1.2.1.tar.bz2
constype                  1.0.5        12 Apr 2023  https://xorg.freedesktop.org/releases/individual/app/constype-1.0.5.tar.xz
ource                     3.4.1        03 Nov 2024  https://www.contextfreeart.org/download/ContextFreeSource3.4.1.tar.xz
convmv                    2.05         22 May 2020  https://www.j3e.de/linux/convmv/convmv-2.05.tar.gz
coreutils                 9.6          18 Jan 2025  https://ftp.gnu.org/gnu/coreutils/coreutils-9.6.tar.xz
cparser                   1.22.0       04 Jan 2016  https://github.com/MatzeB/cparser/archive/cparser-1.22.0.tar.gz
cpio                      2.14         02 May 2023  https://ftp.gnu.org/pub/gnu/cpio/cpio-2.14.tar.bz2
cppunit                   1.15.1       22 Oct 2024  http://dev-www.libreoffice.org/src/cppunit-1.15.1.tar.gz
cpufreqd                  2.4.2        24 Dec 2015  https://sourceforge.net/projects/cpufreqd/files/Source%20packages/2.4.2/cpufreqd-2.4.2.tar.bz2
cracklib                  2.10.2       05 Aug 2024  https://github.com/cracklib/cracklib/releases/download/v2.10.2/cracklib-2.10.2.tar.xz
crass                     1.0.6        28 May 2020  https://rubygems.org/downloads/crass-1.0.6.gem
cream                     0.43         01 Jun 2014  https://sourceforge.net/projects/cream/files/Cream/0.43/cream-0.43.tar.gz
crimson                   0.5.3        01 Jan 2013  http://crimson.seul.org/files/crimson-0.5.3.tar.bz2
1                         i386         01 Jun 2014  http://dagobah.ucc.asn.au/wacky/cryopid-0.5.9.1-i386.tar.gz
cryptopp                  870          04 Dec 2022  https://www.cryptopp.com/cryptopp870.zip
cryptsetup                2.7.5        05 Sep 2024  https://www.kernel.org/pub/linux/utils/cryptsetup/v2.7/cryptsetup-2.7.5.tar.xz
crystalspace              2.0          01 Jun 2014  http://www.crystalspace3d.org/downloads/release/crystalspace-2.0.tar.bz2
ctags                     5.8          01 Jun 2014  https://prdownloads.sourceforge.net/ctags/ctags-5.8.tar.gz
ctioga                    1.11.1       01 Dec 2017  https://rubygems.org/downloads/ctioga-1.11.1.gem
ctorrent                  1.3.4        01 Jun 2014  https://sourceforge.net/projects/ctorrent/files/CTorrent-1.3.4/CTorrent-1.3.4/ctorrent-1.3.4.tar.bz2
ctypes                    1.0.2        01 Jan 2013  https://downloads.sourceforge.net/project/ctypes/ctypes/1.0.2/ctypes-1.0.2.tar.gz
cuiterm                   0.9.9        01 Jun 2014  ftp://linux.pte.hu/pub/CUI/cuiterm-0.9.9.tar.gz
cunit                     2.1-3        20 Feb 2017  https://downloads.sourceforge.net/project/cunit/CUnit/2.1-3/CUnit-2.1-3.tar.bz2
cups                      2.4.11       02 Oct 2024  https://github.com/OpenPrinting/cups/releases/download/v2.4.11/cups-2.4.11-source.tar.gz
cupsfilters               1.28.17      24 Feb 2024  https://github.com/OpenPrinting/cups-filters/releases/download/1.28.17/cups-filters-1.28.17.tar.xz
cupspdf                   2.6.1        01 Jul 2013  https://www.physik.uni-wuerzburg.de/~vrbehr/cups-pdf/src/cups-pdf_2.6.1.tar.gz
curl                      8.11.1       11 Dec 2024  https://curl.se/download/curl-8.11.1.tar.xz
cutmp3                    3.0.1        12 Dec 2015  http://www.puchalla-online.de/cutmp3-3.0.1.tar.bz2
cvs                       1.11.23      01 Mar 2013  http://ftp.gnu.org/non-gnu/cvs/source/stable/1.11.23/cvs-1.11.23.tar.bz2
cvtool                    0.1.0        01 Jun 2014  https://sourceforge.net/projects/cvtool/cvtool-0.1.0.tar.gz
cxxtools                  3.0          28 Jun 2021  http://www.tntnet.org/download/cxxtools-3.0.tar.gz
cyrussasl                 2.1.28       25 Feb 2022  https://github.com/cyrusimap/cyrus-sasl/releases/download/cyrus-sasl-2.1.28/cyrus-sasl-2.1.28.tar.gz
cython                    3.0.11       22 Sep 2024  https://files.pythonhosted.org/packages/84/4d/b720d6000f4ca77f030bd70f12550820f0766b568e43f11af7f7ad9061aa/cython-3.0.11.tar.gz
daemons                   1.4.1        27 May 2022  https://rubygems.org/downloads/daemons-1.4.1.gem
damageproto               1.2.1        08 Aug 2016  https://xorg.freedesktop.org/releases/individual/proto/damageproto-1.2.1.tar.bz2
daq                       2.0.5        20 May 2015  https://www.snort.org/downloads/snort/daq-2.0.5.tar.gz
darcs                     2.14.2       28 Sep 2019  http://darcs.net/releases/darcs-2.14.2.tar.gz
darktable                 4.8.1        25 Oct 2024  https://github.com/darktable-org/darktable/releases/download/release-4.8.1/darktable-4.8.1.tar.xz
dash                      0.5.12       04 Mar 2023  http://gondor.apana.org.au/~herbert/dash/files/dash-0.5.12.tar.gz
dasher                    4.11         01 Jun 2014  http://ftp.acc.umu.se/pub/GNOME/sources/dasher/4.11/dasher-4.11.tar.bz2
datamash                  1.6          24 Feb 2020  http://ftp.gnu.org/pub/gnu/datamash/datamash-1.6.tar.gz
dateutils                 0.4.6        19 Mar 2019  https://bitbucket.org/hroptatyr/dateutils/downloads/dateutils-0.4.6.tar.xz
db                        5.3.28       02 Jun 2020  http://anduin.linuxfromscratch.org/BLFS/bdb/db-5.3.28.tar.gz
dblatex                   0.3          01 Jun 2014  http://prdownloads.sourceforge.net/dblatex/dblatex-0.3.tar.bz2
dbus                      1.16.0       15 Jan 2025  https://dbus.freedesktop.org/releases/dbus/dbus-1.16.0.tar.xz
dbusglib                  0.112        30 Mar 2021  https://dbus.freedesktop.org/releases/dbus-glib/dbus-glib-0.112.tar.gz
dbuspython                1.3.2        08 Sep 2022  https://dbus.freedesktop.org/releases/dbus-python/dbus-python-1.3.2.tar.gz
dconf                     0.40.0       13 Mar 2021  https://download.gnome.org/sources/dconf/0.40/dconf-0.40.0.tar.xz
dconfeditor               3.38.2       24 Nov 2020  https://download.gnome.org/sources/dconf-editor/3.38/dconf-editor-3.38.2.tar.xz
dcraw                     9.15         01 Jun 2014  http://www.cybercom.net/~dcoffin/dcraw/archive/dcraw-9.15.tar.gz
ddclient                  3.8.3        07 Feb 2016  https://sourceforge.net/projects/ddclient/files/ddclient/ddclient-3.8.3/ddclient-3.8.3.tar.bz2
ddd                       3.3.12       01 Jun 2014  http://ftp.gnu.org/gnu/ddd/ddd-3.3.12.tar.gz
ddrescue                  1.27         23 Jan 2023  https://ftp.gnu.org/pub/gnu/ddrescue/ddrescue-1.27.tar.lz
defendguin                0.0.12       05 Jan 2019  ftp://ftp.tuxpaint.org/unix/x/defendguin/src/defendguin-0.0.12.tar.gz
dejagnu                   1.6.3        17 Jun 2021  https://ftp.gnu.org/pub/gnu/dejagnu/dejagnu-1.6.3.tar.gz
dejavufontsttf            2.37         22 Apr 2021  https://sourceforge.net/projects/dejavu/files/dejavu/2.37/dejavu-fonts-ttf-2.37.tar.bz2
deluge                    2.1.1        03 Jan 2022  http://download.deluge-torrent.org/source/2.1/deluge-2.1.1.tar.xz
denemo                    2.6.0        13 Mar 2022  https://ftp.gnu.org/pub/gnu/denemo/denemo-2.6.0.tar.gz
deplate                   0.8.5        15 Apr 2021  https://rubygems.org/downloads/deplate-0.8.5.gem
dermixd                   2.0.1        16 Apr 2016  http://thomas.orgis.org/dermixd/dermixd-2.0.1.tar.bz2
deskbarapplet             2.20.0       01 Jun 2014  http://ftp.acc.umu.se/pub/GNOME/sources/deskbar-applet/2.20/deskbar-applet-2.20.0.tar.bz2
desksanity                1.1.0        05 Feb 2016  https://download.enlightenment.org/rel/apps/enlightenment/desksanity-1.1.0.tar.xz
desktopfileutils          0.28         29 Oct 2024  https://www.freedesktop.org/software/desktop-file-utils/releases/desktop-file-utils-0.28.tar.xz
devhelp                   41.3         26 Aug 2022  https://download.gnome.org/sources/devhelp/41/devhelp-41.3.tar.xz
devil                     1.7.8        19 Jun 2015  http://downloads.sourceforge.net/openil/DevIL-1.7.8.tar.gz
dfeet                     0.3.16       08 May 2021  https://download.gnome.org/sources/d-feet/0.3/d-feet-0.3.16.tar.xz
dhcp                      4.4.3        10 Mar 2022  https://downloads.isc.org/isc/dhcp/4.4.3/dhcp-4.4.3.tar.gz
dhcpcd                    10.0.1       22 Apr 2023  https://github.com/NetworkConfiguration/dhcpcd/releases/download/v10.0.1/dhcpcd-10.0.1.tar.xz
dia                       0.97.3       05 Sep 2014  http://ftp.gnome.org/pub/GNOME/sources/dia/0.97/dia-0.97.3.tar.xz
diakonos                  0.9.4        15 Nov 2015  http://diakonos.pist0s.ca/archives/diakonos-0.9.4.tar.gz
dico                      2.11         27 Apr 2021  https://ftp.gnu.org/pub/gnu/dico/dico-2.11.tar.xz
dictd                     1.12.1       01 Jun 2014  https://sourceforge.net/projects/dict/files/dictd/dictd-1.12.1/dictd-1.12.1.tar.gz
didyoumean                1.6.1        27 May 2022  https://rubygems.org/downloads/did_you_mean-1.6.1.gem
dietlibc                  0.33         11 Oct 2017  https://www.fefe.de/dietlibc/dietlibc-0.33.tar.bz2
difflcs                   1.5.0        28 Jan 2022  https://rubygems.org/downloads/diff-lcs-1.5.0.gem
diffutils                 3.10         22 May 2023  https://ftp.gnu.org/gnu/diffutils/diffutils-3.10.tar.xz
digger                    20020314     04 Apr 2024  https://www.digger.org/digger-20020314.tar.gz
digikam                   8.5.0        17 Nov 2024  https://download.kde.org/stable/digikam/8.5.0/digiKam-8.5.0.tar.xz
dillo                     3.1.1        08 Jun 2024  https://github.com/dillo-browser/dillo/releases/download/v3.1.1/dillo-3.1.1.tar.bz2
ding                      1.9          21 Feb 2022  https://ftp.tu-chemnitz.de/pub/Local/urz/ding/ding-1.9.tar.gz
dinglibs                  0.6.2        19 Mar 2024  https://github.com/SSSD/ding-libs/releases/download/0.6.2/ding-libs-0.6.2.tar.gz
dirac                     1.0.0        01 Jun 2014  https://sourceforge.net/projects/dirac/files/dirac-codec/Dirac-1.0.0/dirac-1.0.0.tar.gz
direvent                  5.3          31 Dec 2021  https://ftp.gnu.org/pub/gnu/direvent/direvent-5.3.tar.gz
discount                  2.2.3a       21 Jul 2018  https://www.pell.portland.or.us/~orc/Code/discount/discount-2.2.3a.tar.bz2
discover                  6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/discover-6.2.5.tar.xz
disko                     1.8.0        01 May 2014  http://www.diskohq.com/repository/sources/disko_1.8.0.tar.gz
distcc                    3.4          13 May 2021  https://github.com/distcc/distcc/releases/download/v3.4/distcc-3.4.tar.gz
dit                       0.9          05 Oct 2023  https://hisham.hm/dit/releases/0.9/dit-0.9.tar.gz
diva                      0.0.2        01 May 2014  http://files.diva-project.org/releases/diva-0.0.2.tar.gz
django                    4.1.5        30 Jan 2023  https://files.pythonhosted.org/packages/41/c8/b3c469353f9d1b7f0e99b45520582b891da02cd87408bc867affa6e039a3/Django-4.1.5.tar.gz
djview                    4.12         06 Feb 2024  http://downloads.sourceforge.net/djvu/djview-4.12.tar.gz
djvulibre                 3.5.28       16 Oct 2021  http://sourceforge.net/projects/djvu/files/DjVuLibre/3.5.28/djvulibre-3.5.28.tar.gz
v                         2.7.1        07 Aug 2019  https://github.com/dell/dkms/archive/v2.7.1.tar.gz
dmidecode                 3.6          11 Jan 2025  https://download.savannah.gnu.org/releases/dmidecode/dmidecode-3.6.tar.xz
dmxproto                  2.3.1        01 May 2014  https://xorg.freedesktop.org/releases/individual/proto/dmxproto-2.3.1.tar.bz2
dnsmasq                   2.89         06 Feb 2023  https://thekelleys.org.uk/dnsmasq/dnsmasq-2.89.tar.xz
dnspython                 1.15.0       04 Nov 2017  http://www.dnspython.org/kits/1.15.0/dnspython-1.15.0.tar.gz
docbook2x                 0.8.8        01 May 2014  http://downloads.sourceforge.net/docbook2x/docbook2X-0.8.8.tar.gz
docbookutils              0.6.14       27 Oct 2015  http://sources-redhat.mirrors.redwire.net/docbook-tools/new-trials/SOURCES/docbook-utils-0.6.14.tar.gz
docbookxml                4.5          10 Feb 2024  https://www.docbook.org/xml/4.5/docbook-xml-4.5.zip
docbookxsl                1.79.2       30 Dec 2017  https://github.com/docbook/xslt10-stylesheets/releases/download/release/1.79.2/docbook-xsl-1.79.2.tar.bz2
docutils                  0.21.2       15 Jan 2025  https://files.pythonhosted.org/packages/source/d/docutils/docutils-0.21.2.tar.gz
docx2txt                  1.4          24 Oct 2016  http://downloads.sourceforge.net/docx2txt/docx2txt-1.4.tgz
dolphin                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/dolphin-24.02.2.tar.xz
dolphinplugins            24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/dolphin-plugins-24.02.2.tar.xz
domainname                0.5.20190701 26 Aug 2019  https://rubygems.org/downloads/domain_name-0.5.20190701.gem
dosbox                    0.74-3       23 Jun 2020  https://sourceforge.net/projects/dosbox/files/dosbox/0.74-3/dosbox-0.74-3.tar.gz
dosfstools                4.2          14 Jan 2025  https://github.com/dosfstools/dosfstools/releases/download/v4.2/dosfstools-4.2.tar.gz
dotconf                   1.0.13       09 Apr 2024  http://www.azzit.de/dotconf/download/v1.0/dotconf-1.0.13.tar.gz
doubleconversion          3.3.0        17 Mar 2024  https://github.com/google/double-conversion/archive/v3.3.0/double-conversion-3.3.0.tar.gz
dovecot                   2.3.20       22 Dec 2022  http://www.dovecot.org/releases/2.3/dovecot-2.3.20.tar.gz
doxygen                   1.13.0       28 Dec 2024  https://doxygen.nl/files/doxygen-1.13.0.tar.gz
dpkg                      1.20.5       11 Jul 2020  http://ftp.debian.org/debian/pool/main/d/dpkg/dpkg_1.20.5.tar.xz
dragon                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/dragon-24.02.2.tar.xz
dragonhunt                3.56         01 Jun 2014  http://emhsoft.com/dh/Dragon_Hunt-3.56.tar.gz
dri2proto                 2.8          01 Jan 2014  http://xorg.freedesktop.org/releases/individual/proto/dri2proto-2.8.tar.bz2
dri3proto                 1.0          01 Nov 2013  http://xorg.freedesktop.org/releases/individual/proto/dri3proto-1.0.tar.bz2
drkonqi                   6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/drkonqi-6.2.5.tar.xz
drupal                    8.7.7        29 Sep 2019  http://ftp.drupal.org/files/projects/drupal-8.7.7.tar.gz
dssi                      1.1.1        01 Jun 2014  https://sourceforge.net/projects/dssi/files/dssi/1.1.1/dssi-1.1.1.tar.gz
duktape                   2.7.0        01 Jan 2023  https://duktape.org/duktape-2.7.0.tar.xz
duma                      2_5_15       27 May 2022  https://sourceforge.net/projects/duma/files/duma/2.5.15/duma_2_5_15.tar.gz
dunelegacy                0.96.4       21 Aug 2017  https://sourceforge.net/projects/dunelegacy/files/dunelegacy/0.96.4/dunelegacy-0.96.4.tar.bz2
dvd+rwtools               7.1          01 May 2014  http://fy.chalmers.se/~appro/linux/DVD+RW/tools/dvd+rw-tools-7.1.tar.gz
dvdauthor                 0.7.1        01 Sep 2014  https://sourceforge.net/projects/dvdauthor/files/dvdauthor/0.7.1/dvdauthor-0.7.1.tar.gz
dvdrip                    0.98.11      01 May 2014  http://www.exit1.org/dvdrip/dist/dvdrip-0.98.11.tar.gz
dvdripomatic              0.94         01 May 2014  http://surfnet.dl.sourceforge.net/sourceforge/dvdripomatic/DVDRipOMatic-0.94.tar.bz2
dvdrtools                 0.3.1        01 May 2014  http://www.arklinux.org/download/dvdrtools-0.3.1.tar.bz2
dvd                       slideshowforge.net 01 May 2014  http://dvd-slideshow.sourceforge.net/
dvgrab                    04.04.2024   01 May 2014  https://github.com/ddennedy/dvgrab/dvgrab-04.04.2024
dvisvgm                   2.10         15 Aug 2020  https://github.com/mgieseki/dvisvgm/releases/download/2.10/dvisvgm-2.10.tar.gz
dwm                       6.1          25 Dec 2015  http://dl.suckless.org/dwm/dwm-6.1.tar.gz
dxpy                      0.172.0      06 Dec 2015  https://pypi.python.org/packages/source/d/dxpy/dxpy-0.172.0.tar.gz
dzen2                     0.4.5        28 Jul 2016  https://github.com/robm/dzen2-0.4.5.tar.xz
e16                       1.0.17       16 Aug 2015  http://sourceforge.net/projects/enlightenment/files/e16/1.0.17/e16-1.0.17.tar.gz
e2fsprogs                 1.47.1       21 May 2024  https://downloads.sourceforge.net/e2fsprogs/e2fsprogs-1.47.1.tar.gz
easytag                   2.4.3        10 Sep 2018  https://download.gnome.org/sources/easytag/2.4/easytag-2.4.3.tar.xz
ebooktools                0.2.2        09 Sep 2022  https://sourceforge.net/projects/ebook-tools/files/ebook-tools/0.2.2/ebook-tools-0.2.2.tar.gz
ecasound                  2.9.0        01 Jun 2014  http://ecasound.seul.org/download/ecasound-2.9.0.tar.gz
ecawave                   0.6.1        01 Jun 2014  http://ecasound.seul.org/download/ecawave-0.6.1.tar.gz
ecell                     3.2.2        01 Nov 2012  http://sourceforge.net/projects/ecell/files/E-Cell%203.2/3.2.2/ecell-3.2.2.tar.bz2
echievements              2            01 Dec 2012  http://download.enlightenment.fr/releases/echievements-2.tar.bz2
econnman                  1.1          13 Apr 2019  https://download.enlightenment.org/rel/apps/econnman/econnman-1.1.tar.xz
ecore                     1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/ecore-1.7.10.tar.gz
ed                        1.20.1       21 Feb 2024  https://ftp.gnu.org/gnu/ed/ed-1.20.1.tar.lz
edbus                     1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/e_dbus-1.7.10.tar.gz
ede                       2.1          17 Sep 2014  https://sourceforge.net/projects/ede/files/ede/2.1/ede-2.1.tar.gz
edelib                    2.1          24 Feb 2017  https://sourceforge.net/projects/ede/files/edelib/2.1/edelib-2.1.tar.gz
editres                   1.0.9        04 Mar 2024  https://www.x.org/releases/individual/app/editres-1.0.9.tar.xz
edje                      1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/edje-1.7.10.tar.gz
v                         1.2.7        30 Oct 2022  https://github.com/Martinsos/edlib/archive/refs/tags/v1.2.7.tar.gz
eet                       1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/eet-1.7.10.tar.gz
eeze                      1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/eeze-1.7.10.tar.gz
efivar                    39           21 Feb 2024  https://github.com/rhboot/efivar/archive/39/efivar-39.tar.gz
efl                       1.27.0       19 Feb 2024  https://download.enlightenment.org/rel/libs/efl/efl-1.27.0.tar.xz
efltk                     2.0.8        21 Jul 2019  https://sourceforge.net/projects/ede/files/efltk/2.0.8/efltk-2.0.8.tar.gz
efreet                    1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/efreet-1.7.10.tar.bz2
eggdbus                   0.6          01 Nov 2012  http://cgit.freedesktop.org/~david/eggdbus/snapshot/eggdbus-0.6.tar.bz2
eigen                     3.4.0        28 Aug 2022  https://gitlab.com/libeigen/eigen/-/archive/3.4.0/eigen-3.4.0.tar.gz
eina                      1.7.10       01 Jan 2014  https://download.enlightenment.org/releases/eina-1.7.10.tar.gz
eio                       1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/eio-1.7.10.tar.gz
eject                     2.1.5        01 Nov 2012  http://ca.geocities.com/jefftranter-rogers.com/eject-2.1.5.tar.gz
ekiga                     4.0.1        01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/ekiga/4.0/ekiga-4.0.1.tar.xz
eldbus                    1.7.9        01 Dec 2013  http://download.enlightenment.org/releases/eldbus-1.7.9.tar.bz2
elementary                1.7.10       28 Jan 2016  https://download.enlightenment.org/releases/elementary-1.7.10.tar.gz
elfutils                  0.192        21 Oct 2024  https://sourceware.org/elfutils/ftp/0.192/elfutils-0.192.tar.bz2
elinks                    0.11.7       28 Aug 2016  http://elinks.or.cz/download/elinks-0.11.7.tar.bz2
elisa                     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/elisa-24.02.2.tar.xz
v                         1.11.4       25 Apr 2021  https://github.com/elixir-lang/elixir/archive/refs/tags/v1.11.4.tar.gz
elogind                   255.17       15 Jan 2025  https://github.com/elogind/elogind/archive/v255.17/elogind-255.17.tar.gz
elph                      1.0.1        29 Jun 2017  ftp://ftp.cbcb.umd.edu/pub/software/elph/ELPH-1.0.1.tar.gz
emacs                     29.4         23 Jun 2024  https://ftp.gnu.org/gnu/emacs/emacs-29.4.tar.xz
emech                     3.0.99p3     01 Jun 2014  http://www.energymech.net/files/emech-3.0.99p3.tar.gz
emelfm                    0.9.2        01 Jun 2014  http://emelfm.sourceforge.net/emelfm-0.9.2.tar.gz
empathy                   3.25.90      17 Aug 2017  http://ftp.gnome.org/pub/gnome/sources/empathy/3.25/empathy-3.25.90.tar.xz
enca                      1.9          01 Jun 2014  http://trific.ath.cx/Ftp//enca/enca-1.9.tar.bz2
enchant                   2.8.2        17 Aug 2024  https://github.com/AbiWord/enchant/releases/download/v2.8.2/enchant-2.8.2.tar.gz
encodings                 1.1.0        04 Mar 2024  https://xorg.freedesktop.org/releases/individual/font/encodings-1.1.0.tar.xz
engrampa                  1.28.2       29 Aug 2024  https://pub.mate-desktop.org/releases/1.28/engrampa-1.28.2.tar.xz
enigma                    1.30         09 Jun 2023  https://github.com/Enigma-Game/Enigma/releases/download/1.30/Enigma-1.30.tar.gz
enlightenment             0.27.0       12 Jan 2025  https://download.enlightenment.org/rel/apps/enlightenment/enlightenment-0.27.0.tar.xz
enscript                  1.6.6        01 Sep 2012  https://ftp.gnu.org/gnu/enscript/enscript-1.6.6.tar.gz
enter                     0.0.9        13 Feb 2016  https://sourceforge.net/projects/enter/files/enter/0.0.9/enter-0.0.9.tar.gz
entrance                  0.9.0.013    01 Jun 2014  http://download.enlightenment.org/snapshots/2007-08-26/entrance-0.9.0.013.tar.gz
enum34                    1.1.6        10 Aug 2018  https://files.pythonhosted.org/packages/bf/3e/31d502c25302814a7c2f1d3959d2a3b3f78e509002ba91aea64993936876/enum34-1.1.6.tar.gz
enventor                  0.3.2        01 Sep 2014  http://download.enlightenment.org/rel/apps/enventor/enventor-0.3.2.tar.gz
eog                       45.4         14 Nov 2024  https://download.gnome.org/sources/eog/45/eog-45.4.tar.xz
eogplugins                42.1         25 Apr 2022  https://download.gnome.org/sources/eog-plugins/42/eog-plugins-42.1.tar.xz
eom                       1.28.0       18 Feb 2024  https://pub.mate-desktop.org/releases/1.28/eom-1.28.0.tar.xz
epdfview                  0.1.8        01 Jun 2014  http://trac.emma-soft.com/epdfview/chrome/site/releases/epdfview-0.1.8.tar.bz2
epic5                     2.1.5        08 Oct 2021  http://ftp.epicsol.org/pub/epic/EPIC5-PRODUCTION/epic5-2.1.5.tar.xz
epiphany                  46.0         13 Apr 2024  https://download.gnome.org/sources/epiphany/46/epiphany-46.0.tar.xz
epkg                      2.3.9        01 Jun 2014  ftp://ftp.encap.org/pub/encap/epkg/epkg-2.3.9.tar.gz
epm                       4.1          01 Jun 2014  http://www.nu6.org/_/mirror/ftp.easysw.com/pub/epm/4.1/epm-4.1-source.tar.bz2
eps                       1.5          01 Jun 2014  http://www.inter7.com/eps/eps-1.5.tar.gz
otp                       26.2.3       02 Apr 2024  https://github.com/erlang/otp/releases/download/OTP-26.2.3/otp_src_26.2.3.tar.gz
error                     0.17029      20 Sep 2022  https://cpan.metacpan.org/authors/id/S/SH/SHLOMIF/Error-0.17029.tar.gz
erubi                     1.10.0       15 Apr 2021  https://rubygems.org/downloads/erubi-1.10.0.gem
erubis                    2.7.0        03 Dec 2017  https://rubygems.org/downloads/erubis-2.7.0.gem
esdl                      1.3.1        02 Aug 2017  https://sourceforge.net/projects/esdl/files/esdl/esdl-1.3.1/esdl-1.3.1.tgz
eselect                   1.4.27       02 Apr 2024  https://gitweb.gentoo.org/proj/eselect.git/snapshot/eselect-1.4.27.tar.bz2
esmtp                     1.2          01 Jun 2014  https://sourceforge.net/projects/esmtp/files/esmtp/1.2/esmtp-1.2.tar.bz2
esound                    0.2.41       05 Dec 2018  https://ftp.gnome.org/pub/GNOME/sources/esound/0.2/esound-0.2.41.tar.gz
espeak                    1.46.02      01 Jun 2014  http://sourceforge.net/projects/espeak/files/espeak/espeak-1.46/espeak-1.46.02-source.zip
eterm                     0.9.6        01 Jul 2013  http://www.eterm.org/download/Eterm-0.9.6.tar.gz
ethtool                   5.19         28 Aug 2022  https://mirrors.edge.kernel.org/pub/software/network/ethtool/ethtool-5.19.tar.xz
ethumb                    1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/ethumb-1.7.10.tar.gz
eudev                     3.2.14       14 Mar 2024  https://github.com/eudev-project/eudev/releases/download/v3.2.14/eudev-3.2.14.tar.gz
evas                      1.7.10       01 Jan 2014  http://download.enlightenment.org/releases/evas-1.7.10.tar.gz
eve                       0.3.0        01 Dec 2012  http://download.enlightenment.org/snapshots/2010-12-03/eve-0.3.0.tar.bz2
event                     1.28         08 May 2021  https://cpan.metacpan.org/authors/id/E/ET/ETJ/Event-1.28.tar.gz
eventmachine              1.2.7        27 May 2019  https://rubygems.org/downloads/eventmachine-1.2.7.gem
eventviews                24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/eventviews-24.02.2.tar.xz
evieext                   1.1.1        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/proto/evieext-1.1.1.tar.bz2
evilvte                   0.5.2~pre1   01 Jun 2013  http://www.calno.com/evilvte/evilvte-0.5.2~pre1.tar.xz
evilwm                    1.4.3        14 Apr 2024  https://www.6809.org.uk/evilwm/dl/evilwm-1.4.3.tar.gz
evince                    46.3.1       25 Jul 2024  https://download.gnome.org/sources/evince/46/evince-46.3.1.tar.xz
evisum                    0.2.6        29 Nov 2019  https://download.enlightenment.org/rel/apps/evisum/evisum-0.2.6.tar.xz
evolution                 3.54.2       22 Nov 2024  https://download.gnome.org/sources/evolution/3.54/evolution-3.54.2.tar.xz
evolutiondataserver       3.48.2       27 May 2023  https://download.gnome.org/sources/evolution-data-server/3.48/evolution-data-server-3.48.2.tar.xz
evolutionews              3.48.2       27 May 2023  https://download.gnome.org/sources/evolution-ews/3.48/evolution-ews-3.48.2.tar.xz
evolutionmapi             3.48.1       27 May 2023  https://download.gnome.org/sources/evolution-mapi/3.48/evolution-mapi-3.48.1.tar.xz
exactimage                1.0.1        04 Jan 2018  http://dl.exactcode.de/oss/exact-image/exact-image-1.0.1.tar.bz2
execjs                    2.8.1        09 Dec 2021  https://rubygems.org/downloads/execjs-2.8.1.gem
exempi                    2.6.2        27 Jun 2022  https://libopenraw.freedesktop.org/download/exempi-2.6.2.tar.bz2
exfatprogs                1.1.2        22 May 2021  https://github.com/exfatprogs/exfatprogs/releases/download/1.1.2/exfatprogs-1.1.2.tar.xz
exfatutils                1.3.0        20 Aug 2020  https://github.com/relan/exfat/releases/download/v1.3.0/exfat-utils-1.3.0.tar.gz
exim                      4.98         10 Jul 2024  https://ftp.exim.org/pub/exim/exim4/exim-4.98.tar.xz
exiv2                     0.28.3       04 Oct 2024  https://github.com/Exiv2/exiv2/archive/v0.28.3/exiv2-0.28.3.tar.gz
exo                       4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/exo/4.20/exo-4.20.0.tar.bz2
expat                     2.6.2        13 Mar 2024  https://sourceforge.net/projects/expat/files/expat/2.6.2/expat-2.6.2.tar.xz
expect                    5.45.4       18 Feb 2018  https://sourceforge.net/projects/expect/files/Expect/5.45.4/expect5.45.4.tar.gz
expedite                  1.7.10       22 Dec 2015  http://download.enlightenment.org/releases/expedite-1.7.10.tar.bz2
explore                   v11          01 Jun 2014  http://mesh.dl.sourceforge.net/sourceforge/openexplore/explore_v11_src.zip
exportertiny              1.006002     08 Apr 2024  https://cpan.metacpan.org/authors/id/T/TO/TOBYINK/Exporter-Tiny-1.006002.tar.gz
extracmakemodules         6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/extra-cmake-modules-6.8.0.tar.xz
extutilsdepends           0.405        22 Feb 2016  http://search.cpan.org/CPAN/authors/id/X/XA/XAOC/ExtUtils-Depends-0.405.tar.gz
extutilshelpers           0.026        15 Sep 2016  http://search.cpan.org/CPAN/authors/id/L/LE/LEONT/ExtUtils-Helpers-0.026.tar.gz
extutilsmakemaker         7.38         09 Nov 2019  https://cpan.metacpan.org/authors/id/B/BI/BINGOS/ExtUtils-MakeMaker-7.38.tar.gz
extutilspkgconfig         1.16         30 Aug 2017  http://search.cpan.org/CPAN/authors/id/X/XA/XAOC/ExtUtils-PkgConfig-1.16.tar.gz
exult                     04.04.2024   04 Apr 2024  https://github.com/exult/exult/exult-04.04.2024.tar.xz
eyed3                     0.9.6        26 Oct 2021  https://files.pythonhosted.org/packages/fb/f2/27b42a10b5668df27ce87aa22407e5115af7fce9b1d68f09a6d26c3874ec/eyeD3-0.9.6.tar.gz
faac                      1.29.9       11 Nov 2017  https://downloads.sourceforge.net/faac/faac-1.29.9.tar.gz
facter                    1.5.4rc1     01 May 2013  http://reductivelabs.com/downloads/facter/facter-1.5.4rc1.tgz
factorygirl               4.9.0        06 Jan 2018  https://rubygems.org/downloads/factory_girl-4.9.0.gem
falkon                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/falkon-24.02.2.tar.xz
fam                       2.7.0        01 Jun 2014  http://gd.tuwien.ac.at/opsys/linux/gentoo/distfiles/fam-2.7.0.tar.gz
faraday                   2.3.0        27 May 2022  https://rubygems.org/downloads/faraday-2.3.0.gem
farstream                 0.2.9        08 Sep 2022  https://freedesktop.org/software/farstream/releases/farstream/farstream-0.2.9.tar.gz
fbdesk                    1.4.1        01 Jun 2014  http://fluxbox.sourceforge.net/download/fbdesk-1.4.1.tar.gz
fbida                     2.14         29 Sep 2019  https://www.kraxel.org/releases/fbida/fbida-2.14.tar.gz
fdkaac                    2.0.1        13 Oct 2019  https://downloads.sourceforge.net/opencore-amr/fdk-aac-2.0.1.tar.gz
feh                       3.10         08 Apr 2023  https://feh.finalrewind.org/feh-3.10.tar.bz2
ferret                    0.11.8.7     09 Jan 2018  https://rubygems.org/downloads/ferret-0.11.8.7.gem
ferrum                    0.11         15 Apr 2021  https://rubygems.org/downloads/ferrum-0.11.gem
fetchmail                 6.4.34       29 Oct 2022  https://sourceforge.net/projects/fetchmail/files/branch_6.4/fetchmail-6.4.34.tar.xz
ffe                       0.3.9        14 Feb 2022  https://sourceforge.net/projects/ff-extractor/files/ff-extractor/0.3.9/ffe-0.3.9.tar.gz
ffi                       1.15.5       27 May 2022  https://rubygems.org/downloads/ffi-1.15.5.gem
ffirzmq                   2.0.7        30 May 2020  https://rubygems.org/downloads/ffi-rzmq-2.0.7.gem
ffmpeg                    7.1          30 Sep 2024  https://ffmpeg.org/releases/ffmpeg-7.1.tar.xz
ffmpeg2theora             0.30         30 Sep 2017  http://v2v.cc/~j/ffmpeg2theora/downloads/ffmpeg2theora-0.30.tar.bz2
ffmpeg                    phpforge.net 01 Jun 2014  http://ffmpeg-php.sourceforge.net/
ffmpegthumbs              24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/ffmpegthumbs-24.02.2.tar.xz
ffpocket                  0.8.0        14 Feb 2022  https://members.hellug.gr/djart/ffpocket/ffpocket-0.8.0.tar.bz2
fftw                      3.3.10       18 Sep 2021  https://www.fftw.org/fftw-3.3.10.tar.gz
fiddle                    1.1.0        27 May 2022  https://rubygems.org/downloads/fiddle-1.1.0.gem
fife                      0.3.4        01 Mar 2013  http://sourceforge.net/projects/fife/files/active/src/fife_0.3.4.tar.gz
fifoembed                 2.1.1        01 Jun 2014  https://sourceforge.net/projects/fifoembed/files/fifoembed/2.1.1/fifoembed-2.1.1.tar.bz2
figaro                    1.2.0        28 May 2020  https://rubygems.org/downloads/figaro-1.2.0.gem
figlet                    2.2.5        23 Aug 2019  ftp://ftp.figlet.org/pub/figlet/program/unix/figlet-2.2.5.tar.gz
file                      5.46         27 Nov 2024  https://astron.com/pub/file/file-5.46.tar.gz
filechdir                 0.1010       12 Apr 2019  https://cpan.metacpan.org/authors/id/D/DA/DAGOLDEN/File-chdir-0.1010.tar.gz
filelight                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/filelight-24.02.2.tar.xz
fileroller                44.3         23 May 2024  https://download.gnome.org/sources/file-roller/44/file-roller-44.3.tar.xz
filesharedir              1.116        05 Jun 2018  https://cpan.metacpan.org/authors/id/R/RE/REHSACK/File-ShareDir-1.116.tar.gz
fileslurper               0.010        26 Dec 2017  http://search.cpan.org/CPAN/authors/id/L/LE/LEONT/File-Slurper-0.010.tar.gz
filetype                  1.1.0        21 Aug 2022  https://files.pythonhosted.org/packages/ba/42/53be9807e47b164a172721ad7960acdc7ec6ad45216c9962f5e933b0e314/filetype-1.1.0.tar.gz
filewhich                 1.23         03 Jan 2019  http://search.cpan.org/CPAN/authors/id/P/PL/PLICEASE/File-Which-1.23.tar.gz
filezilla                 3.60.2_      01 Jun 2014  https://dl3.cdn.filezilla-project.org/client/FileZilla_3.60.2_src.tar.bz2?h=JhMUNz0ahytBVzFRlgWxFg&x=1661556941
fim                       0.6          30 Dec 2018  http://download.savannah.nongnu.org/releases/fbi-improved/fim-0.6-trunk.tar.gz
findlib                   1.7.1        11 Mar 2017  http://download.camlcity.org/download/findlib-1.7.1.tar.gz
findutils                 4.10.0       01 Jun 2024  https://ftp.gnu.org/pub/gnu/findutils/findutils-4.10.0.tar.xz
32609                     0            01 Oct 2016  https://sourceforge.net/projects/firebird/files/firebird/3.0.1-Release/Firebird-3.0.1.32609-0.tar.bz2
firefox                   102.12.0esr  14 Jun 2023  https://archive.mozilla.org/pub/firefox/releases/102.12.0esr/source/firefox-102.12.0esr.source.tar.xz
firejail                  0.9.56       19 Sep 2018  https://sourceforge.net/projects/firejail/files/firejail/firejail-0.9.56.tar.xz
fische                    3.2.2        01 Nov 2012  http://26elf.at/files/fische-3.2.2.tar.gz
fish                      3.7.1        15 Jan 2025  https://github.com/fish-shell/fish-shell/releases/download/3.7.1/fish-3.7.1.tar.xz
fitd                      0.1          01 Jun 2014  http://sourceforge.net/project/showfiles.php?group_id=104884/fitd-0.1
fiveormore                3.32.2       28 Mar 2020  http://ftp.gnome.org/pub/gnome/sources/five-or-more/3.32/five-or-more-3.32.2.tar.xz
fixesproto                5.0          01 Jun 2014  http://xorg.freedesktop.org/releases/individual/proto/fixesproto-5.0.tar.bz2
flac                      1.4.3        25 Jun 2023  https://downloads.xiph.org/releases/flac/flac-1.4.3.tar.xz
flam3                     3.0.1        01 Aug 2014  http://flam3.googlecode.com/files/flam3-3.0.1.zip
flask                     2.0.1        23 May 2021  https://files.pythonhosted.org/packages/c0/df/c516b5f38a670b6b0de604c2637ed5860db03692c2f8542fd1f60c2552a7/Flask-2.0.1.tar.gz
flatbuffers               26.12.2018   26 Dec 2018  flatbuffers-26.12.2018
flatpak                   1.14.8       01 May 2024  https://github.com/flatpak/flatpak/releases/download/1.14.8/flatpak-1.14.8.tar.xz
flatpakkcm                6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/flatpak-kcm-6.2.5.tar.xz
flatzebra                 0.1.6        31 May 2020  http://perso.b2b2c.ca/sarrazip/dev/flatzebra-0.1.6.tar.gz
fldigi                    3.23.12      20 Jul 2016  https://sourceforge.net/projects/fldigi/files/fldigi/fldigi-3.23.12.tar.gz
flex                      2.6.4        07 May 2017  https://github.com/westes/flex/releases/download/v2.6.4/flex-2.6.4.tar.gz
flimp                     0.0.11       25 Apr 2015  http://www.ecademix.com/JohannesHofmann/flimp-0.0.11.tar.gz
flitcore                  3.10.1       03 Nov 2024  https://files.pythonhosted.org/packages/d5/ae/09427bea9227a33ec834ed5461432752fd5d02b14f93dd68406c91684622/flit_core-3.10.1.tar.gz
florence                  0.6.3        25 Apr 2015  http://sourceforge.net/projects/florence/files/florence/0.6.3/florence-0.6.3.tar.bz2
fltk                      1.3.10       20 Nov 2024  https://fltk.org/pub/fltk/1.3.10/fltk-1.3.10-source.tar.gz
v                         2.3.5        31 Dec 2024  https://github.com/FluidSynth/fluidsynth/archive/refs/tags/v2.3.5.tar.gz
fluxbox                   1.3.7        11 Jul 2024  https://sourceforge.net/projects/fluxbox/files/fluxbox/1.3.7/fluxbox-1.3.7.tar.xz
fmt                       11.1.2       16 Jan 2025  https://github.com/fmtlib/fmt/archive/refs/tags/11.1.2/fmt-11.1.2.tar.gz
folks                     0.15.9       24 Mar 2024  https://download.gnome.org/sources/folks/0.15/folks-0.15.9.tar.xz
fontadobe100dpi           1.0.0        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/font/font-adobe-100dpi-1.0.0.tar.bz2
fontadobe75dpi            1.0.0        01 Jun 2014  https://xorg.freedesktop.org/releases/individual/font/font-adobe-75dpi-1.0.0.tar.bz2
fontadobeutopia100dpi     1.0.1        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/font/font-adobe-utopia-100dpi-1.0.1.tar.bz2
fontadobeutopia75dpi      1.0.1        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/font/font-adobe-utopia-75dpi-1.0.1.tar.bz2
fontadobeutopiatype1      1.0.4        01 May 2014  http://xorg.freedesktop.org/releases/individual/font/font-adobe-utopia-type1-1.0.4.tar.gz
fontalias                 1.0.4        09 Aug 2020  http://xorg.freedesktop.org/releases/individual/font/font-alias-1.0.4.tar.bz2
fontbh75dpi               1.0.0        01 Jan 2013  http://xorg.freedesktop.org/releases/individual/font/font-bh-75dpi-1.0.0.tar.bz2
fontbitstream100dpi       1.0.0        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/font/font-bitstream-100dpi-1.0.0.tar.bz2
fontbitstream75dpi        1.0.3        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/font/font-bitstream-75dpi-1.0.3.tar.bz2
fontcacheproto            0.1.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/proto/fontcacheproto-0.1.2.tar.bz2
fontconfig                2.16.0       18 Jan 2025  https://www.freedesktop.org/software/fontconfig/release/fontconfig-2.16.0.tar.xz
fontforge                 20200314     15 Mar 2020  https://github.com/fontforge/fontforge/releases/download/20200314/fontforge-20200314.tar.xz
fontmanager               0.7.4.3      19 Dec 2019  https://github.com/FontManager/master/releases/download/0.7.4.3/font-manager-0.7.4.3.tar.bz2
fontopia                  1.7          21 Aug 2016  http://ftp.gnu.org/pub/gnu/fontopia/fontopia-1.7.tar.gz
fontsonymisc              1.0.3        01 Aug 2013  http://xorg.freedesktop.org/releases/individual/font/font-sony-misc-1.0.3.tar.bz2
fontsproto                2.1.3        01 Apr 2014  http://xorg.freedesktop.org/releases/individual/proto/fontsproto-2.1.3.tar.bz2
fonttosfnt                1.2.4        15 Oct 2024  https://www.x.org/releases/individual/app/fonttosfnt-1.2.4.tar.xz
fontttf                   1.06         16 Dec 2018  https://cpan.metacpan.org/authors/id/B/BH/BHALLISSY/Font-TTF-1.06.tar.gz
fontutil                  1.4.1        20 Feb 2024  https://www.x.org/releases/individual/font/font-util-1.4.1.tar.xz
fontxfree86type1          1.0.4        01 May 2014  http://xorg.freedesktop.org/releases/individual/font/font-xfree86-type1-1.0.4.tar.bz2
formatador                1.1.0        27 May 2022  https://rubygems.org/downloads/formatador-1.1.0.gem
fosfat                    0.2.3.orig   01 May 2014  https://download.gna.org/fosfat/fosfat_0.2.3.orig.tar.gz
fourinarow                3.38.1       03 Dec 2020  https://download.gnome.org/sources/four-in-a-row/3.38/four-in-a-row-3.38.1.tar.xz
fox                       1.7.85       25 Sep 2024  http://fox-toolkit.org/ftp/fox-1.7.85.tar.gz
fpdf                      1.5.317      01 Jun 2014  http://www.fpdf.de/downloads/fpdf-1.5.317.tgz
fpm                       1.14.2       27 May 2022  https://rubygems.org/downloads/fpm-1.14.2.gem
fprintd                   0.6.0        01 Mar 2015  http://people.freedesktop.org/~hadess/fprintd-0.6.0.tar.xz
frameworkintegration      6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/frameworkintegration-6.8.0.tar.xz
freealut                  1.1.0        01 Jun 2014  http://www.openal.org/openal_webstf/downloads/freealut-1.1.0.tar.gz
freebasic                 0.23.0       01 Jun 2014  https://sourceforge.net/projects/fbc/files/Source%20Code/FreeBASIC-0.23.0-source.tar.gz
freeciv                   2.5.4        21 Jul 2016  http://prdownloads.sourceforge.net/freeciv/freeciv-2.5.4.tar.bz2
freecycle                 0.6.1.1alpha 25 Dec 2015  http://download.savannah.gnu.org/releases/freecycle/freecycle-0.6.1.1alpha.tar.bz2
freeglut                  3.4.0        09 Oct 2022  https://sourceforge.net/projects/freeglut/files/freeglut/3.4.0/freeglut-3.4.0.tar.gz
freeimage                 3180         08 Sep 2022  http://downloads.sourceforge.net/freeimage/FreeImage3180.zip
freeipmi                  1.6.15       17 Jan 2025  https://ftp.gnu.org/pub/gnu/freeipmi/freeipmi-1.6.15.tar.gz
freepop                   0.6.0        01 Dec 2012  https://sourceforge.net/projects/freepop/files/freepop/0.6.0/freepop-0.6.0.tar.gz
freerdp                   3.10.3       16 Jan 2025  https://github.com/freerdp/freerdp/archive/3.10.3/FreeRDP-3.10.3.tar.gz
freesteam                 2.0          01 Jun 2014  http://sourceforge.net/projects/freesteam/files/freesteam/2.0/freesteam-2.0.tar.bz2
freesynd                  0.7.5        14 Apr 2017  https://sourceforge.net/projects/freesynd/files/freesynd/freesynd-0.7.5/freesynd-0.7.5.tar.gz
freetds                   1.1.1        19 Mar 2019  ftp://ftp.freetds.org/pub/freetds/stable/freetds-1.1.1.tar.gz
freetype                  2.13.3       12 Aug 2024  https://downloads.sourceforge.net/freetype/freetype-2.13.3.tar.xz
freevo                    1.9.2b2      11 Dec 2017  https://sourceforge.net/projects/freevo/files/Freevo%20releases/1.9.2b2/freevo-1.9.2b2.tar.bz2
freewrl                   1.18.4       01 Jun 2014  http://ovh.dl.sourceforge.net/sourceforge/freewrl/freewrl-1.18.4.tar.gz
frei0rplugins             1.8.0        13 Mar 2024  https://files.dyne.org/frei0r/releases/frei0r-plugins-1.8.0.tar.gz
fribidi                   1.0.16       27 Sep 2024  https://github.com/fribidi/fribidi/releases/download/v1.0.16/fribidi-1.0.16.tar.xz
frogr                     1.5          25 Nov 2018  http://ftp.gnome.org/pub/gnome/sources/frogr/1.5/frogr-1.5.tar.xz
frozenbubble              2.2.0        01 Jan 2013  http://www.frozen-bubble.org/data/frozen-bubble-2.2.0.tar.bz2
fslsfonts                 1.0.6        08 Oct 2022  https://www.x.org/releases/individual/app/fslsfonts-1.0.6.tar.xz
fspot                     0.6.2        01 Jun 2014  http://ftp.gnome.org/Public/GNOME/sources/f-spot/0.6/f-spot-0.6.2.tar.bz2
fstobdf                   1.0.7        08 Oct 2022  https://www.x.org/releases/individual/app/fstobdf-1.0.7.tar.xz
ft2demos                  2.12.1       19 Mar 2023  https://download.savannah.gnu.org/releases/freetype/ft2demos-2.12.1.tar.xz
ftpd                      2.1.0        02 Jul 2020  https://rubygems.org/downloads/ftpd-2.1.0.gem
fuse                      3.16.2       14 Oct 2023  https://github.com/libfuse/libfuse/releases/download/fuse-3.16.2/fuse-3.16.2.tar.gz
fuseiso                   20070708     29 Oct 2014  https://sourceforge.net/projects/fuseiso/files/fuseiso/20070708/fuseiso-20070708.tar.bz2
fvwm                      2.7.0        05 Nov 2022  https://github.com/fvwmorg/fvwm/releases/download/2.7.0/fvwm-2.7.0.tar.gz
fvwm3                     1.0.4        04 Jan 2021  https://github.com/fvwmorg/fvwm3/releases/download/1.0.4/fvwm3-1.0.4.tar.gz
fxruby                    1.6.45       27 May 2022  https://rubygems.org/downloads/fxruby-1.6.45.gem
g3d                       10.00        01 May 2014  https://sourceforge.net/projects/g3d/files/g3d-cpp/10.00/G3D-10.00-source.zip
gaa                       1.6.6        01 May 2014  http://sourceforge.net/projects/gaa/files/gaa/gaa-1.6.6/gaa-1.6.6.tar.gz
gabedit                   218Src       01 Jun 2014  http://dfn.dl.sourceforge.net/sourceforge/gabedit/Gabedit218Src.tar.gz
galculator                2.1.4        02 Aug 2017  http://galculator.mnim.org/downloads/galculator-2.1.4.tar.bz2
gama                      2.24         16 Feb 2023  https://ftp.gnu.org/pub/gnu/gama/gama-2.24.tar.gz
gambas                    3.17.3       19 Sep 2022  https://gitlab.com/gambas/gambas/-/archive/3.17.3/gambas-3.17.3.tar.bz2
v                         4.9.3        23 Feb 2019  https://github.com/gambit/gambit/archive/v4.9.3.tar.gz
changelogs                13.          01 Jun 2014  http://www.gamgi.org/changelogs/changelogs13.html
gamin                     0.1.10       01 Jan 2013  http://www.gnome.org/~veillard/gamin/sources/gamin-0.1.10.tar.gz
garcon                    4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/garcon/4.20/garcon-4.20.0.tar.bz2
garnome                   2.24.0       18 Dec 2018  http://ftp.gnome.org/pub/gnome/sources/garnome/2.24/garnome-2.24.0.tar.bz2
gaupol                    0.28.2       29 Aug 2015  http://download.gna.org/gaupol/0.28/gaupol-0.28.2.tar.xz
gav                       0.9.0        01 Dec 2012  https://sourceforge.net/projects/gav/files/gav/0.9.0/gav-0.9.0.tar.gz
gavl                      1.4.0        13 Dec 2019  https://sourceforge.net/projects/gmerlin/files/gavl/1.4.0/gavl-1.4.0.tar.gz
gawk                      5.3.1        18 Sep 2024  https://ftp.gnu.org/gnu/gawk/gawk-5.3.1.tar.xz
gbench                    2.6.0        01 Oct 2012  ftp://ftp.ncbi.nlm.nih.gov/toolbox/gbench/ver-2.6.0/gbench-2.6.0.tgz
gc                        8.2.8        10 Sep 2024  https://github.com/ivmai/bdwgc/releases/download/v8.2.8/gc-8.2.8.tar.gz
gcab                      1.5          24 Dec 2022  https://download.gnome.org/sources/gcab/1.5/gcab1.5.tar.xz
gcal                      3.6.2        01 Jun 2014  http://ftpmirror.gnu.org/gcal/gcal-3.6.2.tar.xz
gcalctool                 6.6.2        24 Sep 2014  http://ftp.gnome.org/pub/GNOME/sources/gcalctool/6.6/gcalctool-6.6.2.tar.xz
gcc                       25.06.2024   15 Jan 2025  https://ftp.gnu.org/gnu/gcc/gcc-14.1.0/gcc-25.06.2024.tar.xz
gccmakedep                1.0.3        01 May 2014  http://xorg.freedesktop.org/releases/individual/util/gccmakedep-1.0.3.tar.bz2
gcdemu                    1.2.0        01 Jun 2014  http://sourceforge.net/projects/cdemu/files/CDemu%20clients/gcdemu-1.2.0/gcdemu-1.2.0.tar.bz2
gchart                    1.0.0        01 Dec 2017  https://rubygems.org/downloads/gchart-1.0.0.gem
gcide                     0.53         27 Apr 2021  https://ftp.gnu.org/pub/gnu/gcide/gcide-0.53.tar.xz
gconf                     3.2.6        16 Sep 2017  http://ftp.acc.umu.se/pub/GNOME/sources/GConf/3.2/GConf-3.2.6.tar.xz
gconfcleaner              0.0.3        01 Jun 2014  https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/gconf-cleaner/gconf-cleaner-0.0.3.tar.gz
gconfmm                   2.28.3       20 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/gconfmm/2.28/gconfmm-2.28.3.tar.xz
gcr                       4.3.0        13 Apr 2024  https://download.gnome.org/sources/gcr/4.3/gcr-4.3.0.tar.xz
gdado                     2.2          01 Jun 2014  http://sourceforge.net/projects/gdado/files/souce%20code/gdado-2.2/gdado-2.2.tar.gz
gdal                      2.1.4        20 Sep 2017  http://download.osgeo.org/gdal/2.1.4/gdal-2.1.4.tar.xz
gdb                       16.1         18 Jan 2025  https://ftp.gnu.org/gnu/gdb/gdb-16.1.tar.xz
gdbm                      1.23         16 Sep 2022  https://ftp.gnu.org/pub/gnu/gdbm/gdbm-1.23.tar.gz
gdesklets                 0.35.4       01 Jun 2014  http://www.gdesklets.de/files/gDesklets-0.35.4.tar.bz2
gdkpixbuf                 2.42.12      21 May 2024  https://download.gnome.org/sources/gdk-pixbuf/2.42/gdk-pixbuf-2.42.12.tar.xz
gdkpixbufxlib             2.40.2       13 Nov 2020  https://download.gnome.org/sources/gdk-pixbuf-xlib/2.40/gdk-pixbuf-xlib-2.40.2.tar.xz
gdl                       3.40.0       27 Aug 2021  https://download.gnome.org/sources/gdl/3.40/gdl-3.40.0.tar.xz
gdm                       46.0         19 Mar 2024  https://download.gnome.org/sources/gdm/46/gdm-46.0.tar.xz
gdsl                      1.8          25 Apr 2015  http://download.gna.org/gdsl/gdsl-1.8.tar.gz
geany                     1.38         27 Oct 2024  https://download.geany.org/geany-1.38.tar.gz
geanyplugins              1.38         18 Mar 2023  https://plugins.geany.org/geany-plugins/geany-plugins-1.38.tar.gz
geary                     44.1         18 Aug 2023  https://download.gnome.org/sources/geary/44/geary-44.1.tar.xz
gedit                     48.1         11 Dec 2024  https://download.gnome.org/sources/gedit/48/gedit-48.1.tar.xz
geditcodeassistance       3.16.0       30 Apr 2015  http://ftp.gnome.org/pub/GNOME/sources/gedit-code-assistance/3.16/gedit-code-assistance-3.16.0.tar.xz
geditplugins              44.1         26 Apr 2023  https://download.gnome.org/sources/gedit-plugins/44/gedit-plugins-44.1.tar.xz
geeqie                    2.5          22 Sep 2024  https://github.com/BestImageViewer/geeqie/releases/download/v2.5/geeqie-2.5.tar.xz
gegl                      0.4.52       18 Jan 2025  https://download.gimp.org/pub/gegl/0.4/gegl-0.4.52.tar.xz
geis                      2.2.16       01 Sep 2014  https://launchpad.net/geis/trunk/2.2.16/+download/geis-2.2.16.tar.xz
gengetopt                 2.23         09 Jun 2019  ftp://ftp.gnu.org/gnu/gengetopt/gengetopt-2.23.tar.xz
genius                    1.0.27       28 Oct 2021  https://download.gnome.org/sources/genius/1.0/genius-1.0.27.tar.xz
genshi                    0.7          10 Aug 2018  https://ftp.edgewall.org/pub/genshi/Genshi-0.7.tar.gz
genx4r                    0.05         01 Jun 2014  https://rubygems.org/downloads/genx4r-0.05.gem
geoclue                   2.7.2        06 Sep 2024  https://gitlab.freedesktop.org/geoclue/geoclue/-/archive/2.7.2/geoclue-2.7.2.tar.bz2
geocodeglib               3.26.4       19 Sep 2022  https://download.gnome.org/sources/geocode-glib/3.26/geocode-glib-3.26.4.tar.xz
geos                      3.6.2        20 Sep 2017  http://download.osgeo.org/geos/geos-3.6.2.tar.bz2
geshi                     1.0.9.0      20 Sep 2017  https://sourceforge.net/projects/geshi/files/geshi/GeSHi%201.0.9.0/GeSHi-1.0.9.0.tar.bz2
getmail                   5.15         01 Jan 2021  http://pyropus.ca/software/getmail/old-versions/getmail-5.15.tar.gz
gettext                   0.23.1       31 Dec 2024  https://ftp.gnu.org/gnu/gettext/gettext-0.23.1.tar.xz
gexiv2                    0.14.3       17 Jan 2025  https://download.gnome.org/sources/gexiv2/0.14/gexiv2-0.14.3.tar.xz
gfbgraph                  0.2.5        30 Oct 2021  https://download.gnome.org/sources/gfbgraph/0.2/gfbgraph-0.2.5.tar.xz
gftp                      2.0.19       01 Jan 2013  http://gftp.seul.org/gftp-2.0.19.tar.bz2
ggv                       2.12.0       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/ggv/2.12/ggv-2.12.0.tar.bz2
ghemical                  3.0.0        01 Nov 2012  https://bioinformatics.org/ghemical/download/current/ghemical-3.0.0.tar.gz
gherkin                   9.0.0        28 May 2020  https://rubygems.org/downloads/gherkin-9.0.0.gem
ghex                      46.1         22 Nov 2024  https://download.gnome.org/sources/ghex/46/ghex-46.1.tar.xz
ghostscript               10.04.0      16 Jan 2025  https://github.com/ArtifexSoftware/ghostpdl-downloads/releases/download/gs10040/ghostscript-10.04.0.tar.xz
ghostwriter               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/ghostwriter-24.02.2.tar.xz
giblib                    1.2.4        01 Jun 2014  http://linuxbrit.co.uk/downloads/giblib-1.2.4.tar.gz
gif2apng                  1.6          01 Jun 2014  https://sourceforge.net/projects/gif2apng/files/1.6/gif2apng-1.6
giflib                    5.2.2        24 Feb 2024  https://sourceforge.net/projects/giflib/files/giflib-5.2.2.tar.gz
gifsicle                  1.88         18 Mar 2017  http://www.lcdf.org/gifsicle/gifsicle-1.88.tar.gz
gift                      0.1.14       01 Jun 2014  ftp://ftp.gnu.org/gnu/gift/gift-0.1.14.tar.gz
gimp                      2.8.8        27 Oct 2024  https://download.gimp.org/pub/gimp/v2.8/gimp-2.8.8.tar.bz2
gimpsharp                 0.13         01 Jun 2014  http://gimp-sharp.sourceforge.net/gimp-sharp-0.13.tar.gz
gingerblue                6.2.0        03 Sep 2022  https://download.gnome.org/sources/gingerblue/6.2/gingerblue-6.2.0.tar.xz
giostandalone             0.1.2        01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gio-standalone/0.1/gio-standalone-0.1.2.tar.bz2
girara                    0.2.6        18 Aug 2016  https://pwmt.org/projects/girara/download/girara-0.2.6.tar.gz
girl                      9.9.8        27 Apr 2017  http://ftp.gnome.org/pub/gnome/sources/girl/9.9/girl-9.9.8.tar.xz
gist                      6.0.0        15 Apr 2021  https://rubygems.org/downloads/gist-6.0.0.gem
git                       2.48.1       15 Jan 2025  https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.48.1.tar.xz
v                         2.10         27 Jul 2017  https://github.com/git-cola/git-cola/archive/v2.10.tar.gz
gitdb                     0.6.4        10 Aug 2017  https://pypi.python.org/packages/e3/95/7e5d7261feb46c0539ac5e451be340ddd64d78c5118f2d893b052c76fe8c/gitdb-0.6.4.tar.gz#md5=44e4366b8bdfd306b075c3a52c96ae1a
gitg                      3.32.0       23 May 2019  http://ftp.gnome.org/pub/gnome/sources/gitg/3.32/gitg-3.32.0.tar.xz
giv                       0.9.15       01 Jun 2014  http://downloads.sourceforge.net/giv/giv-0.9.15.tar.gz
gjs                       1.82.1       17 Jan 2025  https://download.gnome.org/sources/gjs/1.82/gjs-1.82.1.tar.xz
gkrellm                   2.4.0        21 Jan 2025  https://gkrellmbox.net/releases/gkrellm-2.4.0.tar.bz2
glabels                   3.4.1        29 Apr 2018  http://ftp.gnome.org/pub/gnome/sources/glabels/3.4/glabels-3.4.1.tar.xz
glad                      2.0.8        07 Oct 2024  https://github.com/Dav1dde/glad/archive/v2.0.8/glad-2.0.8.tar.gz
glade                     3.40.0       11 Aug 2022  https://download.gnome.org/sources/glade/3.40/glade-3.40.0.tar.xz
glade3                    3.8.6        08 Aug 2017  http://ftp.gnome.org/pub/gnome/sources/glade3/3.8/glade3-3.8.6.tar.xz
glamoregl                 0.6.0        01 Jan 2014  https://www.x.org/releases/individual/driver/glamor-egl-0.6.0.tar.bz2
glark                     1.7.10       28 Jul 2016  https://sourceforge.net/projects/glark/files/glark/1.7.10/glark-1.7.10.tar.gz
glew                      2.2.0        05 Jun 2020  https://sourceforge.net/projects/glew/files/glew/2.2.0/glew-2.2.0.tgz
glfw                      3.3          03 Oct 2019  https://github.com/glfw/glfw/releases/download/3.3/glfw-3.3.zip
glib                      2.82.4       15 Jan 2025  https://download.gnome.org/sources/glib/2.82/glib-2.82.4.tar.xz
glibc                     2.40         22 Jul 2024  https://ftp.gnu.org/gnu/glibc/glibc-2.40.tar.xz
glibclibidn               2.10.1       01 Jun 2014  http://ftp.gnu.org/gnu/libc/glibc-libidn-2.10.1.tar.bz2
glibmm                    2.82.0       23 Sep 2024  https://download.gnome.org/sources/glibmm/2.82/glibmm-2.82.0.tar.xz
glibnetworking            2.80.0       20 Mar 2024  https://download.gnome.org/sources/glib-networking/2.80/glib-networking-2.80.0.tar.xz
glibopenssl               2.50.7       27 Feb 2018  http://ftp.gnome.org/pub/gnome/sources/glib-openssl/2.50/glib-openssl-2.50.7.tar.xz
glimmer                   302b         24 Jun 2017  http://ccb.jhu.edu/software/glimmer/glimmer302b.tar.gz
glm                       0.9.9.8      05 Jun 2020  https://github.com/g-truc/glm/releases/download/0.9.9.8/glm-0.9.9.8.7z
glob2                     0.9.4.4      01 Jul 2013  http://dl.sv.nongnu.org/releases/glob2/0.9.4/glob2-0.9.4.4.tar.gz
globalid                  1.0.0        27 May 2022  https://rubygems.org/downloads/globalid-1.0.0.gem
glom                      1.30.4       11 Jun 2016  http://ftp.gnome.org/pub/GNOME/sources/glom/1.30/glom-1.30.4.tar.xz
glpk                      5.0          17 Dec 2020  https://ftp.gnu.org/pub/gnu/glpk/glpk-5.0.tar.gz
glproto                   1.4.17       01 Dec 2013  http://xorg.freedesktop.org/releases/individual/proto/glproto-1.4.17.tar.bz2
glslang                   15.0.0       29 Sep 2024  https://github.com/KhronosGroup/glslang/archive/15.0.0/glslang-15.0.0.tar.gz
glu                       9.0.2        26 Jun 2021  ftp://ftp.freedesktop.org/pub/mesa/glu/glu-9.0.2.tar.xz
glue                      1.1.1        01 Dec 2017  https://rubygems.org/downloads/glue-1.1.1.gem
glut                      3.7          01 Dec 2012  http://www.opengl.org/resources/libraries/glut/glut-3.7.tar.gz
glycinloaders             1.0.1        31 Mar 2024  https://download.gnome.org/sources/glycin-loaders/1.0/glycin-loaders-1.0.1.tar.xz
gmail                     0.7.1        22 Jul 2018  https://rubygems.org/downloads/gmail-0.7.1.gem
gmailxoauth               0.4.2        27 Dec 2017  https://rubygems.org/downloads/gmail_xoauth-0.4.2.gem
gmerlin                   1.2.0        03 Oct 2019  https://sourceforge.net/projects/gmerlin/files/gmerlin/1.2.0/gmerlin-1.2.0.tar.gz
gmic                      1.5.7.1      01 Oct 2013  http://downloads.sourceforge.net/project/gmic/gmic_1.5.7.1.tar.gz
gmime                     3.2.13       20 Mar 2023  https://github.com/jstedfast/gmime/releases/download/3.2.13/gmime-3.2.13.tar.xz
gmp                       6.3.0        21 Jan 2024  https://gmplib.org/download/gmp/gmp-6.3.0.tar.xz
gmsh                      1.65.0       01 Jun 2014  http://geuz.org/gmsh/src/gmsh-1.65.0-source.tgz
gmtk                      1.0.9a       01 Nov 2013  http://gmtk.googlecode.com/files/gmtk-1.0.9a.tar.gz
gnash                     0.8.10       03 Jun 2014  http://ftp.gnu.org/gnu/gnash/0.8.10/gnash-0.8.10.tar.bz2
gnet                      2.0.8        01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gnet/2.0/gnet-2.0.8.tar.bz2
gneutronica               0.33         01 Jun 2014  https://sourceforge.net/projects/gneutronica/files/gneutronica/gneutronica-0.33/gneutronica-0.33.tar.gz
gno3dtet                  1.96.1       28 Jul 2016  http://www.eseb.net/ftp/gno3dtet/gno3dtet-1.96.1.tgz
gnoise                    0.1.15       01 Jun 2014  https://sourceforge.net/projects/gnoise/files/gnoise/0.1.15/gnoise-0.1.15.tar.gz
gnomeapplets              3.50.0       19 Feb 2024  https://download.gnome.org/sources/gnome-applets/3.50/gnome-applets-3.50.0.tar.xz
gnomeautoar               0.4.4        29 May 2023  https://download.gnome.org/sources/gnome-autoar/0.4/gnome-autoar-0.4.4.tar.xz
gnomebackgrounds          44.0         14 Apr 2023  https://download.gnome.org/sources/gnome-backgrounds/44/gnome-backgrounds-44.0.tar.xz
gnomebatterybench         3.15.4       07 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/gnome-battery-bench/3.15/gnome-battery-bench-3.15.4.tar.xz
gnomebluetooth            42.5         13 Dec 2022  https://download.gnome.org/sources/gnome-bluetooth/42/gnome-bluetooth-42.5.tar.xz
gnomebooks                40.0         16 Oct 2021  https://ftp.acc.umu.se/pub/gnome/sources/gnome-books/40/gnome-books-40.0.tar.xz
gnomeboxes                46.0         19 Mar 2024  https://download.gnome.org/sources/gnome-boxes/46/gnome-boxes-46.0.tar.xz
gnomebtdownload           0.0.32       01 Jun 2014  http://sourceforge.net/projects/gnome-bt/files/gnome-bt/0.0.32/gnome-btdownload-0.0.32.tar.gz
gnomebuild                0.2.4        01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-build/0.2/gnome-build-0.2.4.tar.bz2
gnomebuilder              46.2         29 Jun 2024  https://download.gnome.org/sources/gnome-builder/46/gnome-builder-46.2.tar.xz
gnomecalculator           42.2         02 Jul 2022  https://download.gnome.org/sources/gnome-calculator/42/gnome-calculator-42.2.tar.xz
gnomecalendar             44.1         22 Aug 2023  https://download.gnome.org/sources/gnome-calendar/44/gnome-calendar-44.1.tar.xz
gnomecharacters           42.0         04 Sep 2022  https://download.gnome.org/sources/gnome-characters/42/gnome-characters-42.0.tar.xz
gnomechemistryutils       0.9.2        01 Jun 2014  http://download.savannah.nongnu.org/releases/gchemutils/0.9/gnome-chemistry-utils-0.9.2.tar.bz2
gnomechess                42.1         16 Sep 2022  https://download.gnome.org/sources/gnome-chess/42/gnome-chess-42.1.tar.xz
gnomeclocks               46.0         24 Sep 2024  https://download.gnome.org/sources/gnome-clocks/46/gnome-clocks-46.0.tar.xz
gnomecodeassistance       3.16.0       30 Apr 2015  http://ftp.gnome.org/pub/GNOME/sources/gnome-code-assistance/3.16/gnome-code-assistance-3.16.0.tar.xz
gnomecolormanager         3.36.2       18 Jan 2025  https://download.gnome.org/sources/gnome-color-manager/3.36/gnome-color-manager-3.36.2.tar.xz
gnomecommander            1.16.1       10 Jul 2023  https://download.gnome.org/sources/gnome-commander/1.16/gnome-commander-1.16.1.tar.xz
gnomecommon               3.18.0       22 Sep 2015  http://ftp.gnome.org/pub/GNOME/sources/gnome-common/3.18/gnome-common-3.18.0.tar.xz
gnomeconnections          44.1         24 Apr 2023  https://download.gnome.org/sources/gnome-connections/44/gnome-connections-44.1.tar.xz
gnomecontacts             46.0         26 Jul 2024  https://download.gnome.org/sources/gnome-contacts/46/gnome-contacts-46.0.tar.xz
gnomecontrolcenter        46.2         28 May 2024  https://download.gnome.org/sources/gnome-control-center/46/gnome-control-center-46.2.tar.xz
gnomecupsmanager          0.33         01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-cups-manager/0.33/gnome-cups-manager-0.33.tar.bz2
gnomedesktop              44.1         13 Sep 2024  https://download.gnome.org/sources/gnome-desktop/44/gnome-desktop-44.1.tar.xz
gnomedictionary           3.26.1       02 Oct 2017  http://ftp.gnome.org/pub/gnome/sources/gnome-dictionary/3.26/gnome-dictionary-3.26.1.tar.xz
gnomedirectorythumbnailer 0.1.10       26 Jan 2018  http://ftp.gnome.org/pub/gnome/sources/gnome-directory-thumbnailer/0.1/gnome-directory-thumbnailer-0.1.10.tar.xz
gnomediskutility          46.0         09 Mar 2024  https://download.gnome.org/sources/gnome-disk-utility/46/gnome-disk-utility-46.0.tar.xz
gnomedocuments            3.30.1       19 Jan 2019  http://ftp.gnome.org/pub/gnome/sources/gnome-documents/3.30/gnome-documents-3.30.1.tar.xz
gnomedocutils             0.20.10      20 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/gnome-doc-utils/0.20/gnome-doc-utils-0.20.10.tar.xz
gnomedvbdaemon            0.2.90       01 Aug 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-dvb-daemon/0.2/gnome-dvb-daemon-0.2.90.tar.xz
gnomeepubthumbnailer      1.6          02 Oct 2018  http://ftp.gnome.org/pub/gnome/sources/gnome-epub-thumbnailer/1.6/gnome-epub-thumbnailer-1.6.tar.xz
gnomeflashback            3.42.1       26 Nov 2021  https://download.gnome.org/sources/gnome-flashback/3.42/gnome-flashback-3.42.1.tar.xz
gnomefontviewer           42.0         19 Sep 2022  https://download.gnome.org/sources/gnome-font-viewer/42/gnome-font-viewer-42.0.tar.xz
gnomegames                40.0         21 Aug 2022  https://download.gnome.org/sources/gnome-games/40/gnome-games-40.0.tar.xz
gnomehearts               0.2          01 Jun 2014  http://www.jejik.com/files/gnome-hearts/gnome-hearts-0.2.tar.gz
gnomeicontheme            3.12.0       01 Apr 2014  http://ftp.gnome.org/pub/gnome/sources/gnome-icon-theme/3.12/gnome-icon-theme-3.12.0.tar.xz
gnomeiconthemeextras      3.12.0       01 Apr 2014  http://ftp.gnome.org/pub/gnome/sources/gnome-icon-theme-extras/3.12/gnome-icon-theme-extras-3.12.0.tar.xz
gnomeiconthemesymbolic    3.12.0       01 Apr 2014  http://ftp.gnome.org/pub/gnome/sources/gnome-icon-theme-symbolic/3.12/gnome-icon-theme-symbolic-3.12.0.tar.xz
gnomeinitialsetup         46.0         16 Mar 2024  https://download.gnome.org/sources/gnome-initial-setup/46/gnome-initial-setup-46.0.tar.xz
gnomeinternetradiolocator 12.7.0       14 Sep 2022  https://download.gnome.org/sources/gnome-internet-radio-locator/12.7/gnome-internet-radio-locator-12.7.0.tar.xz
gnomekeyring              46.1         09 Apr 2024  https://download.gnome.org/sources/gnome-keyring/46/gnome-keyring-46.1.tar.xz
gnomekeyringmanager       2.20.0       01 Jun 2014  ftp://ftp.gnome.org/pub/gnome/sources/gnome-keyring-manager/2.20/gnome-keyring-manager-2.20.0.tar.bz2
gnomekiosk                46.0         16 Jan 2025  https://download.gnome.org/sources/gnome-kiosk/46/gnome-kiosk-46.0.tar.xz
gnomeklotski              3.38.2       24 Nov 2020  https://download.gnome.org/sources/gnome-klotski/3.38/gnome-klotski-3.38.2.tar.xz
gnomekraorathumbnailer    1.3          01 Dec 2013  http://ftp.gnome.org/pub/GNOME/sources/gnome-kra-ora-thumbnailer/1.3/gnome-kra-ora-thumbnailer-1.3.tar.xz
gnomelatex                3.42.0       03 Nov 2022  https://download.gnome.org/sources/gnome-latex/3.42/gnome-latex-3.42.0.tar.xz
gnomelogs                 42.0         28 Mar 2022  https://download.gnome.org/sources/gnome-logs/42/gnome-logs-42.0.tar.xz
gnomemag                  0.15.1       01 Jun 2014  http://ftp.gnome.org/pub/gnome/sources/gnome-mag/0.15/gnome-mag-0.15.1.tar.bz2
gnomemahjongg             3.38.1       19 Sep 2020  http://ftp.gnome.org/pub/gnome/sources/gnome-mahjongg/3.38/gnome-mahjongg-3.38.1.tar.xz
gnomemainmenu             1.8.0        01 Jun 2014  http://pub.mate-desktop.org/releases/1.8/gnome-main-menu-1.8.0.tar.xz
gnomemaps                 46.12        11 Oct 2024  https://download.gnome.org/sources/gnome-maps/46/gnome-maps-46.12.tar.xz
gnomemenus                3.36.0       12 Mar 2020  https://ftp.gnome.org/pub/gnome/sources/gnome-menus/3.36/gnome-menus-3.36.0.tar.xz
gnomemines                40.1         19 Feb 2022  https://download.gnome.org/sources/gnome-mines/40/gnome-mines-40.1.tar.xz
gnomemultiwriter          3.30.0       05 Sep 2018  http://ftp.gnome.org/pub/gnome/sources/gnome-multi-writer/3.30/gnome-multi-writer-3.30.0.tar.xz
gnomemusic                44.0         23 Mar 2023  https://download.gnome.org/sources/gnome-music/44/gnome-music-44.0.tar.xz
gnomenetstatus            2.28.2       02 Aug 2017  http://ftp.gnome.org/pub/GNOME/sources/gnome-netstatus/2.28/gnome-netstatus-2.28.2.tar.gz
gnomenettool              42.0         07 Apr 2022  https://download.gnome.org/sources/gnome-nettool/42/gnome-nettool-42.0.tar.xz
gnomenetwork              1.99.5       01 Jun 2014  ftp://ftp.gnome.org/pub/GNOME/sources/gnome-network/1.99/gnome-network-1.99.5.tar.bz2
gnomenibbles              3.38.1       05 Oct 2020  http://ftp.gnome.org/pub/gnome/sources/gnome-nibbles/3.38/gnome-nibbles-3.38.1.tar.xz
gnomeonlineaccounts       3.52.3.1     16 Jan 2025  https://download.gnome.org/sources/gnome-online-accounts/3.52/gnome-online-accounts-3.52.3.1.tar.xz
gnomeonlineminers         3.34.0       11 Sep 2019  http://ftp.gnome.org/pub/gnome/sources/gnome-online-miners/3.34/gnome-online-miners-3.34.0.tar.xz
gnomepackagekit           3.30.0       05 Sep 2018  http://ftp.gnome.org/pub/gnome/sources/gnome-packagekit/3.30/gnome-packagekit-3.30.0.tar.xz
gnomepanel                3.54.0       11 Oct 2024  https://download.gnome.org/sources/gnome-panel/3.54/gnome-panel-3.54.0.tar.xz
gnomephonemanager         0.69         24 Sep 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-phone-manager/0.69/gnome-phone-manager-0.69.tar.xz
gnomephotos               3.38.1       19 Feb 2021  https://download.gnome.org/sources/gnome-photos/3.38/gnome-photos-3.38.1.tar.xz
gnomepowermanager         3.30.0       05 Sep 2018  http://ftp.gnome.org/pub/gnome/sources/gnome-power-manager/3.30/gnome-power-manager-3.30.0.tar.xz
gnomepythondesktop        2.23.0       01 Jan 2013  http://ftp.gnome.org/pub/GNOME/sources/gnome-python-desktop/2.23/gnome-python-desktop-2.23.0.tar.bz2
gnomeradio                64.0         09 Nov 2022  https://download.gnome.org/sources/gnome-radio/64/gnome-radio-64.0.tar.xz
gnomerecipes              1.2.0        08 May 2017  http://ftp.gnome.org/pub/gnome/sources/gnome-recipes/1.2/gnome-recipes-1.2.0.tar.xz
gnomeremotedesktop        46.0         17 Mar 2024  https://download.gnome.org/sources/gnome-remote-desktop/46/gnome-remote-desktop-46.0.tar.xz
gnomerobots               3.36.1       27 Mar 2020  http://ftp.gnome.org/pub/gnome/sources/gnome-robots/3.36/gnome-robots-3.36.1.tar.xz
gnomescan                 0.5.94       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-scan/0.5/gnome-scan-0.5.94.tar.bz2
gnomeschedule             2.2.2        01 Jun 2014  http://sourceforge.net/projects/gnome-schedule/files/gnome-schedule-2/gnome-schedule-2-2-2/gnome-schedule-2.2.2.tar.gz
gnomescreensaver          3.6.1        24 Sep 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-screensaver/3.6/gnome-screensaver-3.6.1.tar.xz
gnomescreenshot           41.0         15 Nov 2021  https://download.gnome.org/sources/gnome-screenshot/41/gnome-screenshot-41.0.tar.xz
gnomesession              46.0         13 Apr 2024  https://download.gnome.org/sources/gnome-session/46/gnome-session-46.0.tar.xz
gnomesettingsdaemon       44.1         18 Apr 2023  https://download.gnome.org/sources/gnome-settings-daemon/44/gnome-settings-daemon-44.1.tar.xz
gnomesharp                2.24.0       01 Jun 2014  http://ftp.gnome.org/pub/gnome/sources/gnome-sharp/2.24/gnome-sharp-2.24.0.tar.bz2
gnomeshell                46.8         16 Jan 2025  https://download.gnome.org/sources/gnome-shell/46/gnome-shell-46.8.tar.xz
gnomeshellextensions      42.1         06 May 2022  https://download.gnome.org/sources/gnome-shell-extensions/42/gnome-shell-extensions-42.1.tar.xz
gnomesoftware             40.4         13 Aug 2021  https://download.gnome.org/sources/gnome-software/40/gnome-software-40.4.tar.xz
gnomesoundrecorder        42.0         05 Apr 2022  https://download.gnome.org/sources/gnome-sound-recorder/42/gnome-sound-recorder-42.0.tar.xz
gnome                     speech       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-speech/
gnomesubtitles            1.1          01 Jun 2014  http://downloads.sourceforge.net/gnome-subtitles/gnome-subtitles-1.1.tar.gz
gnomesudoku               42.0         17 Mar 2022  https://download.gnome.org/sources/gnome-sudoku/42/gnome-sudoku-42.0.tar.xz
gnomesystemmonitor        40.1         30 Apr 2021  https://download.gnome.org/sources/gnome-system-monitor/40/gnome-system-monitor-40.1.tar.xz
gnomesystemtools          3.0.0        20 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/gnome-system-tools/3.0/gnome-system-tools-3.0.0.tar.bz2
gnometaquin               3.34.1       08 Oct 2019  http://ftp.gnome.org/pub/gnome/sources/gnome-taquin/3.34/gnome-taquin-3.34.1.tar.xz
gnometerminal             3.54.1       26 Oct 2024  https://gitlab.gnome.org/GNOME/gnome-terminal/-/archive/3.54.1/gnome-terminal-3.54.1.tar.gz
gnometetravex             3.38.2       24 Nov 2020  https://download.gnome.org/sources/gnome-tetravex/3.38/gnome-tetravex-3.38.2.tar.xz
gnomethemes               3.0.0        01 Jun 2014  ftp://ftp.gnome.org/pub/GNOME/sources/gnome-themes/3.0/gnome-themes-3.0.0.tar.bz2
gnomethemesextra          3.28         24 Mar 2018  http://ftp.gnome.org/pub/gnome/sources/gnome-themes-extra/3.28/gnome-themes-extra-3.28.tar.xz
gnomethemesstandard       3.27.90      14 Feb 2018  http://ftp.gnome.org/pub/gnome/sources/gnome-themes-standard/3.27/gnome-themes-standard-3.27.90.tar.xz
gnometodo                 40.1         16 Jun 2021  https://download.gnome.org/sources/gnome-todo/40/gnome-todo-40.1.tar.xz
gnometweaks               46.0         13 Apr 2024  https://download.gnome.org/sources/gnome-tweaks/46/gnome-tweaks-46.0.tar.xz
gnometweaktool            3.26.4       30 Dec 2017  http://ftp.gnome.org/pub/gnome/sources/gnome-tweak-tool/3.26/gnome-tweak-tool-3.26.4.tar.xz
gnomeusage                3.37.1       05 Aug 2020  http://ftp.gnome.org/pub/gnome/sources/gnome-usage/3.37/gnome-usage-3.37.1.tar.xz
gnomeusershare            3.34.0       22 Nov 2019  http://ftp.gnome.org/pub/GNOME/sources/gnome-user-share/3.34/gnome-user-share-3.34.0.tar.xz
gnomeutils                3.2.1        20 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/gnome-utils/3.2/gnome-utils-3.2.1.tar.xz
gnomevfsmm                2.26.0       01 Jun 2014  http://ftp.acc.umu.se/pub/GNOME/sources/gnome-vfsmm/2.26/gnome-vfsmm-2.26.0.tar.bz2
gnomevideoarcade          0.8.8        31 Oct 2017  http://ftp.gnome.org/pub/gnome/sources/gnome-video-arcade/0.8/gnome-video-arcade-0.8.8.tar.xz
gnomevideoeffects         0.4.1        01 Mar 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-video-effects/0.4/gnome-video-effects-0.4.1.tar.xz
gnomevoicecontrol         0.4          01 Jun 2014  http://prdownload.berlios.de/festlang/gnome-voice-control-0.4.tar.gz
gnomevolume               manager      01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gnome-volume-manager/
gnomeweather              46.0         05 Apr 2024  https://download.gnome.org/sources/gnome-weather/46/gnome-weather-46.0.tar.xz
gnonlin                   1.2.0        01 Apr 2014  http://gstreamer.freedesktop.org/src/gnonlin/gnonlin-1.2.0.tar.xz
gnopernicus               1.1.2        01 Jun 2014  http://ftp.gnome.org/pub/gnome/sources/gnopernicus/1.1/gnopernicus-1.1.2.tar.bz2
gnormalize                0.63         01 Jun 2014  http://prdownloads.sourceforge.net/gnormalize/gnormalize-0.63.tar.gz
gnote                     46.0         23 Mar 2024  https://download.gnome.org/sources/gnote/46/gnote-46.0.tar.xz
gnuastro                  0.20         01 May 2023  https://ftp.gnu.org/pub/gnu/gnuastro/gnuastro-0.20.tar.gz
gnucash                   5.10         16 Dec 2024  https://downloads.sourceforge.net/gnucash/gnucash-5.10.tar.bz2
gnuchess                  6.2.9        15 Jul 2021  https://ftp.gnu.org/pub/gnu/chess/gnuchess-6.2.9.tar.gz
gnucobol                  3.1.1        12 Dec 2020  https://ftp.gnu.org/pub/gnu/gnucobol/gnucobol-3.1.1.tar.xz
gnugo                     3.8          01 Jan 2013  http://ftp.gnu.org/gnu/gnugo/gnugo-3.8.tar.gz
gnuhealth                 4.2.0        13 Feb 2023  https://ftp.gnu.org/pub/gnu/health/gnuhealth-4.2.0.tar.gz
gnujump                   1.0.5        01 Dec 2012  http://download.savannah.gnu.org/releases/gnujump/gnujump-1.0.5.tar.gz
gnumeric                  1.12.57      12 Feb 2024  https://download.gnome.org/sources/gnumeric/1.12/gnumeric-1.12.57.tar.xz
gnump3d                   3.0          01 Jun 2014  http://savannah.gnu.org/download/gnump3d/gnump3d-3.0.tar.bz2
gnun                      1.3          09 Nov 2022  https://ftp.gnu.org/pub/gnu/gnun/gnun-1.3.tar.gz
gnunet                    0.19.4       01 Apr 2023  https://ftp.gnu.org/pub/gnu/gnunet/gnunet-0.19.4.tar.gz
gnunetgtk                 0.21.0       04 Apr 2024  https://git.gnunet.org/gnunet-gtk.git/snapshot/gnunet-gtk-0.21.0.tar.gz
gnupdf                    04.04.2024   04 Apr 2024  https://www.gnu.org/software/pdf/gnupdf-04.04.2024
gnupg                     2.4.7        26 Nov 2024  https://gnupg.org/ftp/gcrypt/gnupg/gnupg-2.4.7.tar.bz2
gnuplot                   6.0.1        01 Jun 2024  https://sourceforge.net/projects/gnuplot/files/gnuplot/6.0.1/gnuplot-6.0.1.tar.gz
gnuradio                  3.9.0.0      18 Jan 2021  https://www.gnuradio.org/releases/gnuradio/gnuradio-3.9.0.0.tar.xz
gnurl                     7.70.0       01 May 2020  http://ftp.gnu.org/pub/gnu/gnunet/gnurl-7.70.0.tar.gz
gnustepbase               1.24.0       01 Jun 2014  ftp://ftp.gnustep.org/pub/gnustep/core/gnustep-base-1.24.0.tar.gz
gnustepmake               1.12.0       01 Jun 2014  ftp://ftp.gnustep.org/pub/gnustep/core/gnustep-make-1.12.0.tar.gz
gnutls                    3.8.8        06 Nov 2024  https://www.gnupg.org/ftp/gcrypt/gnutls/v3.8/gnutls-3.8.8.tar.xz
gnuuid                    0.5.1        14 Oct 2018  https://rubygems.org/downloads/gn_uuid-0.5.1.gem
go                        1.4.2        19 May 2015  https://storage.googleapis.com/golang/go1.4.2.tar.gz
gob2                      2.0.20       01 Dec 2013  http://ftp.gnome.org/pub/GNOME/sources/gob2/2.0/gob2-2.0.20.tar.xz
gobby                     0.5.0        02 Jun 2018  http://releases.0x539.de/gobby/gobby-0.5.0.tar.gz
gobjectintrospection      1.82.0       16 Sep 2024  https://download.gnome.org/sources/gobject-introspection/1.82/gobject-introspection-1.82.0.tar.xz
gocr                      0.47         02 Aug 2017  https://sourceforge.net/projects/jocr/files/gocr/0.47/gocr-0.47.tar.gz
god                       0.13.7       03 Sep 2022  https://rubygems.org/downloads/god-0.13.7.gem
goffice                   0.10.57      23 Feb 2024  https://download.gnome.org/sources/goffice/0.10/goffice-0.10.57.tar.xz
gok                       2.30.1       20 Aug 2017  http://ftp.acc.umu.se/pub/GNOME/sources/gok/2.30/gok-2.30.1.tar.gz
golly                     4.0          18 Jan 2023  https://sourceforge.net/projects/golly/files/golly/golly-4.0/golly-4.0.tar.gz
gom                       0.5.2        10 Jul 2024  https://download.gnome.org/sources/gom/0.5/gom-0.5.2.tar.xz
gonzui                    1.2          01 Jun 2014  https://rubygems.org/downloads/gonzui-1.2.gem
goobox                    3.6.0        10 Apr 2019  https://ftp.gnome.org/pub/gnome/sources/goobox/3.6/goobox-3.6.0.tar.xz
goocanvas                 3.0.0        26 Jan 2021  https://download.gnome.org/sources/goocanvas/3.0/goocanvas-3.0.0.tar.xz
googleauth                1.1.3        27 May 2022  https://rubygems.org/downloads/googleauth-1.1.3.gem
googletest                27.03.2024   27 Mar 2024  https://download.kde.org/stable/googletest-27.03.2024
gosu                      1.4.3        27 May 2022  https://rubygems.org/downloads/gosu-1.4.3.gem
gpa                       0.10.0       30 Oct 2018  https://www.gnupg.org/ftp/gcrypt/gpa/gpa-0.10.0.tar.bz2
v                         0.7.1        27 Feb 2018  https://github.com/gpac/gpac/archive/v0.7.1.tar.gz
gpaint2                   0.3.3        01 Jun 2014  ftp://alpha.gnu.org/gnu/gpaint/gpaint-2-0.3.3.tar.gz
gparted                   1.6.0        27 Feb 2024  https://downloads.sourceforge.net/gparted/gparted-1.6.0.tar.gz
gperf                     3.1          20 Jul 2017  https://ftp.gnu.org/pub/gnu/gperf/gperf-3.1.tar.gz
gperiodic                 2.0.10       01 Jun 2014  http://www.frantz.fi/software/gperiodic-2.0.10.tar.gz
gpgme                     1.24.1       06 Dec 2024  https://www.gnupg.org/ftp/gcrypt/gpgme/gpgme-1.24.1.tar.bz2
gphoto2                   2.5.28       05 Jan 2022  https://sourceforge.net/projects/gphoto/files/gphoto/2.5.28/gphoto2-2.5.28.tar.bz2
gpm                       1.20.7       18 Feb 2019  http://anduin.linuxfromscratch.org/BLFS/gpm/gpm-1.20.7.tar.bz2
gpredict                  2.2.1        04 Apr 2024  https://github.com/csete/gpredict/releases/download/v2.2.1/gpredict-2.2.1.tar.bz2
gprolog                   1.5.0        09 Jul 2021  https://ftp.gnu.org/pub/gnu/gprolog/gprolog-1.5.0.tar.gz
gpsd                      2.36         01 Jun 2014  http://download.berlios.de/gpsd/gpsd-2.36.tar.gz
gptfdisk                  1.0.10       24 Feb 2024  https://downloads.sourceforge.net/gptfdisk/gptfdisk-1.0.10.tar.gz
gqview                    2.1.5        02 Jan 2018  http://prdownloads.sourceforge.net/gqview/gqview-2.1.5.tar.gz
v                         0.66.0       12 Jul 2022  https://github.com/sciapp/gr/archive/refs/tags/v0.66.0.tar.gz
grabc                     1.1          01 Jun 2014  http://freshmeat.net/projects/grabc-1.1.tar.xz
grace                     5.1.25       19 Oct 2018  ftp://plasma-gate.weizmann.ac.il/pub/grace/src/grace5/grace-5.1.25.tar.gz
gradle                    6.2          22 Feb 2020  https://services.gradle.org/distributions/gradle-6.2-bin.zip
gramps                    5.1.3        22 Sep 2020  https://sourceforge.net/projects/gramps/files/Stable/5.1.3/gramps-5.1.3.tar.gz
granatier                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/granatier-24.02.2.tar.xz
grantlee                  5.3.1        16 Nov 2022  https://github.com/steveire/grantlee/releases/download/v5.3.1/grantlee-5.3.1.tar.gz
grantleeeditor            24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/grantlee-editor-24.02.2.tar.xz
grantleetheme             24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/grantleetheme-24.02.2.tar.xz
grape                     1.6.2        27 May 2022  https://rubygems.org/downloads/grape-1.6.2.gem
graphene                  1.10.8       25 Mar 2022  https://download.gnome.org/sources/graphene/1.10/graphene-1.10.8.tar.xz
graphicsmagick            1.3.36       27 Feb 2021  https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick/1.3.36/GraphicsMagick-1.3.36.tar.xz
graphite2                 1.3.14       11 Jan 2021  https://github.com/silnrsi/graphite/releases/download/1.3.14/graphite2-1.3.14.tgz
graphviz                  12.2.1       15 Jan 2025  https://gitlab.com/api/v4/projects/4207231/packages/generic/graphviz-releases/12.2.1/graphviz-12.2.1.tar.xz
graphvizcairo             2.8          01 Jun 2014  http://www.graphviz.org/pub/graphviz/ARCHIVE/graphviz-cairo-2.8.tar.gz
greenfish                 1.0.8        30 Apr 2021  greenfish-1.0.8
grep                      3.11         13 May 2023  https://ftp.gnu.org/gnu/grep/grep-3.11.tar.xz
grilo                     0.3.16       27 May 2023  https://download.gnome.org/sources/grilo/0.3/grilo-0.3.16.tar.xz
griloplugins              0.3.16       05 Apr 2023  https://download.gnome.org/sources/grilo-plugins/0.3/grilo-plugins-0.3.16.tar.xz
grism                     0.9.0        16 Feb 2017  http://www.grism.org/grism-0.9.0.tar.gz
groff                     1.23.0       06 Jul 2023  https://ftp.gnu.org/gnu/groff/groff-1.23.0.tar.gz
gromacs                   2022.2       28 Aug 2022  https://ftp.gromacs.org/gromacs/gromacs-2022.2.tar.gz
groonga                   10.0.7       29 Oct 2020  https://packages.groonga.org/source/groonga/groonga-10.0.7.tar.gz
grub                      2.12         20 Dec 2023  https://ftp.gnu.org/gnu/grub/grub-2.12.tar.xz
gruff                     0.17.0       22 Jun 2022  https://rubygems.org/downloads/gruff-0.17.0.gem
gsasl                     2.0.1        17 Jul 2022  https://ftp.gnu.org/pub/gnu/gsasl/gsasl-2.0.1.tar.gz
gscan2pdf                 2.2.0        01 Dec 2018  https://sourceforge.net/projects/gscan2pdf/files/gscan2pdf/2.2.0/gscan2pdf-2.2.0.tar.xz
gscanbus                  0.8          04 Nov 2017  https://sourceforge.net/projects/gscanbus.berlios/files/gscanbus-0.8.tar.gz
gsettingsdesktopschemas   46.0         20 Mar 2024  https://download.gnome.org/sources/gsettings-desktop-schemas/46/gsettings-desktop-schemas-46.0.tar.xz
gsl                       2.8          30 May 2024  https://ftp.gnu.org/gnu/gsl/gsl-2.8.tar.gz
gslapt                    0.5.4c       16 Oct 2017  http://software.jaos.org/source/gslapt/gslapt-0.5.4c.tar.gz
gsmartcontrol             0.8.7        11 Apr 2015  http://sourceforge.net/projects/gsmartcontrol/files/0.8.7/gsmartcontrol-0.8.7.tar.bz2
gsound                    1.0.3        19 Aug 2021  https://download.gnome.org/sources/gsound/1.0/gsound-1.0.3.tar.xz
gspell                    1.14.0       19 Sep 2024  https://download.gnome.org/sources/gspell/1.14/gspell-1.14.0.tar.xz
gss                       1.0.4        09 Aug 2022  https://ftp.gnu.org/pub/gnu/gss/gss-1.0.4.tar.gz
gssdp                     1.6.3        09 Nov 2023  https://download.gnome.org/sources/gssdp/1.6/gssdp-1.6.3.tar.xz
gstdebugger               0.90.0       09 Oct 2015  http://ftp.gnome.org/pub/GNOME/sources/gst-debugger/0.90/gst-debugger-0.90.0.tar.xz
gsteditingservices        1.17.1       10 Aug 2020  https://gstreamer.freedesktop.org/src/gst-editing-services/gst-editing-services-1.17.1.tar.xz
gstlibav                  1.24.3       15 May 2024  https://gstreamer.freedesktop.org/src/gst-libav/gst-libav-1.24.3.tar.xz
gstpluginsbad             1.24.8       02 Oct 2024  https://gstreamer.freedesktop.org/src/gst-plugins-bad/gst-plugins-bad-1.24.8.tar.xz
gstpluginsbase            1.24.8       21 Sep 2024  https://gstreamer.freedesktop.org/src/gst-plugins-base/gst-plugins-base-1.24.8.tar.xz
gstpluginsgood            1.24.8       21 Sep 2024  https://gstreamer.freedesktop.org/src/gst-plugins-good/gst-plugins-good-1.24.8.tar.xz
gstpluginsugly            1.24.10      18 Dec 2024  https://gstreamer.freedesktop.org/src/gst-plugins-ugly/gst-plugins-ugly-1.24.10.tar.xz
gstpython                 0.10.22      20 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/gst-python/0.10/gst-python-0.10.22.tar.xz
gstreamer                 1.24.11      07 Jan 2025  https://gstreamer.freedesktop.org/src/gstreamer/gstreamer-1.24.11.tar.xz
gstreamermm               1.10.0       21 Oct 2017  http://ftp.gnome.org/pub/gnome/sources/gstreamermm/1.10/gstreamermm-1.10.0.tar.xz
gstrtspserver             1.4.5        02 Mar 2015  http://gstreamer.freedesktop.org/src/gst-rtsp/gst-rtsp-server-1.4.5.tar.xz
gsview                    4.9          01 Jun 2014  http://mirror.cs.wisc.edu/pub/mirrors/ghost/ghostgum/gsview-4.9.tar.gz
gtef                      1.99.2       12 Mar 2017  http://ftp.gnome.org/pub/GNOME/sources/gtef/1.99/gtef-1.99.2.tar.xz
gthumb                    3.12.6       12 Mar 2024  https://download.gnome.org/sources/gthumb/3.12/gthumb-3.12.6.tar.xz
gtk                       4.17.2       15 Jan 2025  https://download.gnome.org/sources/gtk/4.17/gtk-4.17.2.tar.xz
gtk+                      3.24.43      11 Jul 2024  https://download.gnome.org/sources/gtk+/3.24/gtk+-3.24.43.tar.xz
gtkam                     0.2.0        01 Apr 2014  http://sourceforge.net/projects/gphoto/files/gtkam/0.2.0/gtkam-0.2.0.tar.gz
gtkchtheme                0.3.1        01 Dec 2012  http://plasmasturm.org/code/gtk-chtheme/gtk-chtheme-0.3.1.tar.bz2
gtkclearlooks             0.5          01 Dec 2012  http://ftp.gnome.org/pub/GNOME/teams/art.gnome.org/themes/gtk_engines/gtk-clearlooks-0.5.tar.bz2
gtkdatabox                0.8.0.0      01 Dec 2012  http://www.eudoxos.net/gtk/gtkdatabox/download/gtkdatabox-0.8.0.0.tar.gz
gtkdoc                    1.34.0       18 Mar 2024  https://download.gnome.org/sources/gtk-doc/1.34/gtk-doc-1.34.0.tar.xz
gtkengines                2.91.1       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/gtk-engines/2.91/gtk-engines-2.91.1.tar.bz2
gtkglarea                 2.1.0        01 Apr 2014  http://ftp.gnome.org/pub/gnome/sources/gtkglarea/2.1/gtkglarea-2.1.0.tar.xz
gtkglext                  1.2.0        01 Nov 2012  http://downloads.sourceforge.net/gtkglext/gtkglext-1.2.0.tar.gz
gtkhtml                   4.10.0       21 Sep 2015  http://ftp.gnome.org/pub/GNOME/sources/gtkhtml/4.10/gtkhtml-4.10.0.tar.xz
gtkinternetradiolocator   0.0.3        14 Jul 2018  http://ftp.gnome.org/pub/gnome/sources/gtk-internet-radio-locator/0.0/gtk-internet-radio-locator-0.0.3.tar.xz
gtk                       iptablesforge.net 01 Jun 2014  http://gtk-iptables.sourceforge.net/
gtkmm                     4.16.0       11 Sep 2024  https://download.gnome.org/sources/gtkmm/4.16/gtkmm-4.16.0.tar.xz
gtkperl                   0.7010       21 Aug 2017  http://search.cpan.org/CPAN/authors/id/M/ML/MLEHMANN/Gtk-Perl-0.7010.tar.gz
gtkrecordmydesktop        0.3.8        29 Sep 2018  https://sourceforge.net/projects/recordmydesktop/files/gtk-recordMyDesktop/0.3.8/gtk-recordmydesktop-0.3.8.tar.gz
gtksharp                  2.12.45      21 Aug 2017  https://download.mono-project.com/sources/gtk-sharp212/gtk-sharp-2.12.45.tar.gz
gtksourceview             5.14.1       15 Oct 2024  https://download.gnome.org/sources/gtksourceview/5.14/gtksourceview-5.14.1.tar.xz
gtksourceview2            3.4.3        28 May 2020  https://rubygems.org/downloads/gtksourceview2-3.4.3.gem
gtksourceview3            3.5.1        27 May 2022  https://rubygems.org/downloads/gtksourceview3-3.5.1.gem
gtksourceviewmm           3.91.1       04 Dec 2018  http://ftp.gnome.org/pub/gnome/sources/gtksourceviewmm/3.91/gtksourceviewmm-3.91.1.tar.xz
gtkspell3                 3.0.10       03 Jan 2019  https://sourceforge.net/projects/gtkspell/files/3.0.10/gtkspell3-3.0.10.tar.xz
gtkvnc                    1.3.1        18 Jul 2022  https://download.gnome.org/sources/gtk-vnc/1.3/gtk-vnc-1.3.1.tar.xz
gtkwave                   3.3.98       02 Jan 2019  http://gtkwave.sourceforge.net/gtkwave-3.3.98.tar.gz
gtkxfceengine             3.2.0        30 Aug 2017  http://archive.xfce.org/src/xfce/gtk-xfce-engine/3.2/gtk-xfce-engine-3.2.0.tar.bz2
gtranslator               46.0         21 Mar 2024  https://download.gnome.org/sources/gtranslator/46/gtranslator-46.0.tar.xz
gucharmap                 15.1.3       24 Apr 2024  https://gitlab.gnome.org/GNOME/gucharmap/-/archive/15.1.3/gucharmap-15.1.3.tar.bz2
guessit                   3.1.0        23 Sep 2019  https://files.pythonhosted.org/packages/3e/4d/0f3c55ff4fbef55e6e1086e9f273cef4f70d8aa1b975f2a4d9821e0fdc34/guessit-3.1.0.tar.gz
guichan                   0.8.2        01 Jun 2014  http://guichan.googlecode.com/files/guichan-0.8.2.tar.gz
guile                     3.0.10       24 Jun 2024  https://ftp.gnu.org/pub/gnu/guile/guile-3.0.10.tar.xz
guilib                    1.2.1        01 Jun 2014  http://www.libsdl.org/projects/GUIlib/src/GUIlib-1.2.1.tar.gz
guiloader                 2.99.0       01 Jun 2014  http://nothing-personal.googlecode.com/files/guiloader-2.99.0.tar.xz
gujin                     2.8.7        08 Mar 2017  https://sourceforge.net/projects/gujin/files/gujin/2.8.7/gujin-2.8.7.tar.gz
gumboparser               14.08.2013   01 Aug 2013  gumboparser-14.08.2013.tar.xz
gupnp                     1.6.7        24 Sep 2024  https://download.gnome.org/sources/gupnp/1.6/gupnp-1.6.7.tar.xz
gupnpav                   0.14.1       06 Jun 2022  https://download.gnome.org/sources/gupnp-av/0.14/gupnp-av-0.14.1.tar.xz
gupnpdlna                 0.11.0       09 Jul 2021  https://download.gnome.org/sources/gupnp-dlna/0.11/gupnp-dlna-0.11.0.tar.xz
gupnpigd                  1.6.0        15 Apr 2023  https://download.gnome.org/sources/gupnp-igd/1.6/gupnp-igd-1.6.0.tar.xz
gupnptools                0.12.0       12 Oct 2022  https://download.gnome.org/sources/gupnp-tools/0.12/gupnp-tools-0.12.0.tar.xz
gutenprint                5.3.4        18 Dec 2021  https://downloads.sourceforge.net/gimp-print/gutenprint-5.3.4.tar.xz
guyum                     0.4.1        01 Jun 2014  http://sourceforge.net/projects/guyum/files/guyum/guyum-0.4/guyum-0.4.1.tar.gz
gv                        3.7.4        21 Jan 2019  http://ftp.gnu.org/gnu/gv/gv-3.7.4.tar.gz
gvfs                      1.56.1       26 Oct 2024  https://download.gnome.org/sources/gvfs/1.56/gvfs-1.56.1.tar.xz
gvpe                      3.1          26 Oct 2018  https://ftp.gnu.org/pub/gnu/gvpe/gvpe-3.1.tar.gz
gwenview                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/gwenview-24.02.2.tar.xz
gwyddion                  2.52         19 Dec 2018  https://sourceforge.net/projects/gwyddion/files/gwyddion/2.52/gwyddion-2.52.tar.xz
gxine                     0.5.910      13 Jan 2018  https://sourceforge.net/projects/xine/files/gxine/0.5.910/gxine-0.5.910.tar.xz
gxml                      0.20.0       11 Aug 2020  http://ftp.gnome.org/pub/gnome/sources/gxml/0.20/gxml-0.20.0.tar.xz
gzip                      1.13         20 Aug 2023  https://ftp.gnu.org/gnu/gzip/gzip-1.13.tar.gz
hal                       0.5.14       01 Jun 2014  https://hal.freedesktop.org/releases/hal-0.5.14.tar.bz2
haml                      5.2.2        05 Apr 2022  https://rubygems.org/downloads/haml-5.2.2.gem
hanami                    1.3.5        27 May 2022  https://rubygems.org/downloads/hanami-1.3.5.gem
hanamiassets              1.3.5        15 Apr 2021  https://rubygems.org/downloads/hanami-assets-1.3.5.gem
harfbuzz                  10.2.0       14 Jan 2025  https://github.com/harfbuzz/harfbuzz/releases/download/10.2.0/harfbuzz-10.2.0.tar.xz
hashie                    5.0.0        27 May 2022  https://rubygems.org/downloads/hashie-5.0.0.gem
hatchling                 1.24.2       23 Apr 2024  https://files.pythonhosted.org/packages/2d/18/dbb6b0c167c21429e4f4b03744f5da79d4a534f12c13fd41d2d3033b96da/hatchling-1.24.2.tar.gz
haveged                   1.9.18       12 Apr 2022  https://github.com/jirka-h/haveged/archive/v1.9.18/haveged-1.9.18.tar.gz
hawknl                    17b1src      01 Jun 2014  http://hawksoft.com/download/files/HawkNL17b1src.zip
hd2u                      1.0.4        11 Apr 2023  https://hany.sk/~hany/_data/hd2u/hd2u-1.0.4.tgz
hdf5                      1.10.4       30 Jan 2019  https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-1.10.4/src/hdf5-1.10.4.tar.gz
hdparm                    9.65         08 Sep 2022  https://downloads.sourceforge.net/hdparm/hdparm-9.65.tar.gz
heckle                    1.4.3        30 Nov 2017  https://rubygems.org/downloads/heckle-1.4.3.gem
heimdal                   1.6rc2       22 Sep 2015  http://www.h5l.org/dist/src/heimdal-1.6rc2.tar.gz
hello                     2.10         09 Jul 2018  http://ftp.gnu.org/gnu/hello/hello-2.10.tar.gz
help2man                  1.49.3       16 Dec 2022  https://ftp.gnu.org/pub/gnu/help2man/help2man-1.49.3.tar.xz
herbstluftwm              0.8.3        12 Aug 2020  https://www.herbstluftwm.org/tarballs/herbstluftwm-0.8.3.tar.gz
hexahop                   1.1.0        26 Feb 2017  https://sourceforge.net/projects/hexahop/files/1.1.0/hex-a-hop-1.1.0.tar.gz
hexapdf                   0.32.2       17 May 2023  https://rubygems.org/downloads/hexapdf-0.32.2.gem
hexchat                   2.16.2       12 Mar 2024  https://github.com/hexchat/hexchat/releases/download/v2.16.2/hexchat-2.16.2.tar.xz
hexedit                   0.9.7        01 Jun 2014  http://www.rogoyski.com/adam/programs/hexedit/hexedit-0.9.7.tar.gz
hicoloricontheme          0.18         23 May 2024  https://icon-theme.freedesktop.org/releases/hicolor-icon-theme-0.18.tar.xz
hiera                     3.9.0        27 May 2022  https://rubygems.org/downloads/hiera-3.9.0.gem
highlight                 4.14         24 Sep 2024  http://www.andre-simon.de/zip/highlight-4.14.tar.bz2
highline                  2.0.3        28 May 2020  https://rubygems.org/downloads/highline-2.0.3.gem
highmoon                  1.2.4        01 Jun 2014  http://highmoon.gerdsmeier.net/highmoon-1.2.4.tar.gz
highway                   1.2.0        22 Jan 2025  https://github.com/google/highway/releases/download/1.2.0/highway-1.2.0.tar.gz
group                     group        01 Feb 2013  http://rubyforge.org/frs/?group_id=1444&release_id=4578
histogram                 0.2.4.1      02 Feb 2021  https://rubygems.org/downloads/histogram-0.2.4.1.gem
hitori                    44.0         03 Mar 2023  https://download.gnome.org/sources/hitori/44/hitori-44.0.tar.xz
hmmer                     3.4          19 Oct 2023  http://eddylab.org/software/hmmer/hmmer-3.4.tar.gz
group                     group        01 Jan 2013  https://sourceforge.net/project/showfiles.php?group_id=186602&package_id=217560
hoe                       3.24.0       22 Jun 2022  https://rubygems.org/downloads/hoe-3.24.0.gem
hornetseyeffmpeg          1.2.5        10 Oct 2017  https://rubygems.org/downloads/hornetseye-ffmpeg-1.2.5.gem
hotbabe                   1.1.1        01 Jun 2014  http://dindinx.net/hotbabe/hotbabe-1.1.1.tar.xz
hplip                     3.24.4       17 Jun 2024  https://downloads.sourceforge.net/hplip/hplip-3.24.4.tar.gz
hpricot                   0.8.6        17 Oct 2018  https://rubygems.org/downloads/hpricot-0.8.6.gem
hsakmt                    1.0.0        26 Oct 2015  http://xorg.freedesktop.org/releases/individual/lib/hsakmt-1.0.0.tar.bz2
hspell                    1.4          31 Dec 2018  http://hspell.ivrix.org.il/hspell-1.4.tar.gz
html5lib                  1.1          10 Sep 2022  https://files.pythonhosted.org/packages/ac/b6/b55c3f49042f1df3dcd422b7f224f939892ee94f22abcf503a9b7339eaf2/html5lib-1.1.tar.gz
htmlentities              4.3.4        15 May 2020  https://rubygems.org/downloads/htmlentities-4.3.4.gem
htmlparser                3.76         13 Mar 2021  https://www.cpan.org/authors/id/O/OA/OALDERS/HTML-Parser-3.76.tar.gz
htop                      3.3.0        09 Mar 2024  https://github.com/htop-dev/htop/releases/download/3.3.0/htop-3.3.0.tar.xz
htslib                    1.21         15 Jan 2025  https://sourceforge.net/projects/samtools/files/samtools/1.21/htslib-1.21.tar.bz2
httparty                  0.20.0       05 Mar 2022  https://rubygems.org/downloads/httparty-0.20.0.gem
httpclient                2.8.3        13 Nov 2017  https://rubygems.org/downloads/httpclient-2.8.3.gem
httpcookie                1.0.5        27 May 2022  https://rubygems.org/downloads/http-cookie-1.0.5.gem
httpd                     2.4.62       17 Jul 2024  https://downloads.apache.org/httpd/httpd-2.4.62.tar.gz
hugin                     2019.0.0     28 Jul 2019  https://sourceforge.net/projects/hugin/files/hugin/hugin-2019.0/hugin-2019.0.0.tar.bz2
humphrey                  24.06.2014   01 Jun 2014  http://retrospec.sgn.net/users/ignacio/downloads/humphrey-24.06.2014
hunspell                  1.7.2        13 Mar 2024  https://github.com/hunspell/hunspell/releases/download/v1.7.2/hunspell-1.7.2.tar.gz
hwd                       4.8.1        01 Jun 2014  http://user-contributions.org/projects/hwd/src/hwd-4.8.1.tar.gz
hwloc                     1.11.7       28 Dec 2018  https://www.open-mpi.org/software/hwloc/v1.11/downloads/hwloc-1.11.7.tar.gz
hydrogen                  0.9.5.1      01 Nov 2015  http://sourceforge.net/projects/hydrogen/files/Hydrogen/0.9.5%20Sources/hydrogen-0.9.5.1.tar.gz
hyperbole                 7.0.9        17 Feb 2020  http://ftp.gnu.org/pub/gnu/hyperbole/hyperbole-7.0.9.tar.gz
hypermammut               0.0.1        01 Jun 2014  http://switch.dl.sourceforge.net/sourceforge/hypermammut/hypermammut-0.0.1.tar.bz2
hyphen                    2.8.8        11 Jun 2015  https://sourceforge.net/projects/hunspell/files/Hyphen/2.8/hyphen-2.8.8.tar.gz
hypothesis                4.7.7        21 Feb 2019  https://files.pythonhosted.org/packages/02/52/0b683679bc27200d3fbc51beebf39075b0b28e139e56858b0c7be1b27cd5/hypothesis-4.7.7.tar.gz
i18n                      1.10.0       27 May 2022  https://rubygems.org/downloads/i18n-1.10.0.gem
i3                        4.18         17 Feb 2020  https://i3wm.org/downloads/i3-4.18.tar.bz2
iagno                     3.37.91      28 Aug 2020  http://ftp.gnome.org/pub/gnome/sources/iagno/3.37/iagno-3.37.91.tar.xz
ianaetc                   20240905     08 Sep 2024  https://github.com/Mic92/iana-etc/releases/download/20240905/iana-etc-20240905.tar.gz
iat                       0.1.7        12 Apr 2019  https://sourceforge.net/projects/iat.berlios/files/iat-0.1.7.tar.bz2
ibus                      1.5.31       08 Nov 2024  https://github.com/ibus/ibus/archive/1.5.31/ibus-1.5.31.tar.gz
ice                       3.4.2        01 Jun 2014  http://www.zeroc.com/download/Ice/3.4/Ice-3.4.2.tar.gz
iceauth                   1.0.10       12 Mar 2024  https://www.x.org/releases/individual/app/iceauth-1.0.10.tar.xz
icebreaker                1.9.8        01 Jun 2014  http://mattdm.org/icebreaker/1.9.x/icebreaker-1.9.8.tgz
icecast                   2.4.4        02 Nov 2018  https://downloads.xiph.org/releases/icecast/icecast-2.4.4.tar.gz
icecc                     1.3          14 Sep 2019  https://github.com/icecc/icecream/releases/download/1.3/icecc-1.3.tar.xz
icedtea                   2.4.7        01 May 2014  http://icedtea.classpath.org/download/source/icedtea-2.4.7.tar.xz
icewm                     3.6.0        17 Jun 2024  https://github.com/ice-wm/icewm/releases/download/3.6.0/icewm-3.6.0.tar.lz
ico                       1.0.6        01 Sep 2022  https://www.x.org/releases/individual/app/ico-1.0.6.tar.xz
iconnamingutils           0.8.90       01 Jun 2014  http://tango.freedesktop.org/releases/icon-naming-utils-0.8.90.tar.bz2
icoutils                  0.31.2       08 Mar 2017  http://savannah.nongnu.org/download/icoutils/icoutils-0.31.2.tar.bz2
icu4c76                   1            15 Jan 2025  https://github.com/unicode-org/icu/releases/download/release-76-1/icu4c-76_1.tgz
id3ed                     1.10.4       01 Jun 2014  http://www.ibiblio.org/pub/Linux/apps/sound/editors/id3ed-1.10.4.tar.gz
id3lib                    3.8.3        01 May 2014  https://sourceforge.net/projects/id3lib/files/id3lib/3.8.3/id3lib-3.8.3.tar.gz
idesk                     0.7.5        04 Sep 2015  https://nchc.dl.sourceforge.net/sourceforge/idesk/idesk-0.7.5.tar.bz2
iftop                     0.17         01 Jun 2014  http://www.ex-parrot.com/pdw/iftop/download/iftop-0.17.tar.gz
igtgputools               1.29         08 Sep 2024  https://www.x.org/releases/individual/app/igt-gpu-tools-1.29.tar.xz
ijs                       0.35         01 Jan 2013  http://www.openprinting.org/download/ijs/download/ijs-0.35.tar.bz2
ilmbase                   2.2.0        09 Jul 2015  https://download.savannah.nongnu.org/releases/openexr/ilmbase-2.2.0.tar.gz
imageexiftool             13.12        15 Jan 2025  https://exiftool.org/Image-ExifTool-13.12.tar.gz
imagemagick               7.1.1-39     02 Nov 2024  https://imagemagick.org/archive/ImageMagick-7.1.1-39.tar.xz
imageworsener             1.3.0        02 Dec 2015  http://entropymine.com/imageworsener/imageworsener-1.3.0.tar.gz
imaging                   1.1.6        01 Jun 2014  http://effbot.org/downloads/Imaging-1.1.6.tar.gz
imake                     1.0.10       12 Feb 2024  https://www.x.org/releases/individual/util/imake-1.0.10.tar.xz
img2pdf                   0.5.1        16 Dec 2023  https://files.pythonhosted.org/packages/36/92/6ac4d61951ba507b499f674c90dfa7b48fa776b56f6f068507f8751c03f1/img2pdf-0.5.1.tar.gz
imlib                     1.9.15       01 Sep 2013  http://ftp.gnome.org/pub/GNOME/sources/imlib/1.9/imlib-1.9.15.tar.bz2
imlib2                    1.12.3       15 Jul 2024  https://sourceforge.net/projects/enlightenment/files/imlib2/1.12.3/imlib2-1.12.3.tar.xz
inch                      0.8.0        01 May 2018  https://rubygems.org/downloads/inch-0.8.0.gem
incidenceeditor           24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/incidenceeditor-24.02.2.tar.xz
incron                    0.5.10       20 Sep 2018  http://inotify.aiken.cz/download/incron/incron-0.5.10.tar.bz2
indent                    2.2.12       08 Sep 2018  http://mirror.rit.edu/gnu/indent/indent-2.2.12.tar.xz
inetutils                 2.5          08 Sep 2024  https://ftp.gnu.org/pub/gnu/inetutils/inetutils-2.5.tar.xz
inih                      r53          09 Jun 2021  https://github.com/benhoyt/inih/archive/r53/inih-r53.tar.gz
iniparser                 15.06.2024   16 Jun 2024  https://github.com/ndevilla/iniparser/releases/download/iniparser-15.06.2024
initdtools                0.1.3        24 Jul 2017  http://people.freedesktop.org/~dbn/initd-tools/releases/initd-tools-0.1.3.tar.gz
initng                    0.6.10.2     01 Jun 2014  http://download.initng.org/initng/v0.6/initng-0.6.10.2.tar.bz2
inkscape                  1.4          14 Oct 2024  https://inkscape.org/gallery/item/53679/inkscape-1.4.tar.xz
inotifytools              3.14         01 Jun 2014  http://github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz
inputproto                2.3.2        31 Jul 2016  https://www.x.org/releases/individual/proto/inputproto-2.3.2.tar.bz2
inspec                    5.17.4       27 May 2022  https://rubygems.org/downloads/inspec-5.17.4.gem
instiki                   0.10.2       01 Jan 2013  https://rubygems.org/downloads/instiki-0.10.2.gem
intelgputools             1.21         23 Jan 2018  https://www.x.org/releases/individual/app/intel-gpu-tools-1.21.tar.xz
intelvaapidriver          2.4.1        30 Jun 2020  https://github.com/intel/intel-vaapi-driver/releases/download/2.4.1/intel-vaapi-driver-2.4.1.tar.bz2
intltool                  0.51.0       04 Apr 2015  https://launchpad.net/intltool/trunk/0.51.0/+download/intltool-0.51.0.tar.gz
iostring                  1.08         16 Dec 2018  https://cpan.metacpan.org/authors/id/G/GA/GAAS/IO-String-1.08.tar.gz
iotop                     0.6          01 Jun 2014  http://guichaz.free.fr/iotop/files/iotop-0.6.tar.gz
iproute2                  6.13.0       22 Jan 2025  https://mirrors.edge.kernel.org/pub/linux/utils/net/iproute2/iproute2-6.13.0.tar.xz
ipset                     7.7          31 Oct 2020  http://ipset.netfilter.org/ipset-7.7.tar.bz2
iptables                  1.8.11       10 Nov 2024  https://www.netfilter.org/projects/iptables/files/iptables-1.8.11.tar.xz
iptraf                    3.0.0        01 Jul 2011  ftp://iptraf.seul.org/pub/iptraf/iptraf-3.0.0.tar.gz
ipython                   7.22.0       29 Apr 2021  https://files.pythonhosted.org/packages/bd/59/5e4caa1b226d79508c8601888bd94af1f12ff464613daad90193c0e5fc88/ipython-7.22.0.tar.gz
irrlicht                  1.8.3        18 Mar 2016  http://downloads.sourceforge.net/irrlicht/irrlicht-1.8.3.zip
irssi                     1.4.4        03 Apr 2023  https://github.com/irssi-import/irssi/releases/download/1.4.4/irssi-1.4.4.tar.xz
isbf                      0.1          01 May 2014  http://freshmeat.net/redir/isbf/66799/url_tgz/isbf-0.1.tar.gz
isl                       0.26         16 May 2023  https://libisl.sourceforge.io/isl-0.26.tar.xz
isocodes                  4.16.0.orig  10 Feb 2024  https://ftp.debian.org/debian/pool/main/i/iso-codes/iso-codes_4.16.0.orig.tar.xz
isoimagewriter            24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/isoimagewriter-24.02.2.tar.xz
itinerary                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/itinerary-24.02.2.tar.xz
itstool                   2.0.7        28 Sep 2021  https://files.itstool.org/itstool/itstool-2.0.7.tar.bz2
iw                        6.9          19 May 2024  https://www.kernel.org/pub/software/network/iw/iw-6.9.tar.xz
v                         1.9.22       17 Jan 2025  https://github.com/jackaudio/jack2/archive/refs/tags/v1.9.22.tar.gz
jackaudioconnectionkit    0.125.0      15 Nov 2017  https://jackaudio.org/downloads/jack-audio-connection-kit-0.125.0.tar.gz
jailkit                   2.17         25 Apr 2015  https://olivier.sessink.nl/jailkit/jailkit-2.17.tar.bz2
jam                       105          01 Jun 2014  http://www.disi.unige.it/person/LagorioG/jam/jam105.zip
jansson                   2.14         29 Oct 2021  https://github.com/akheron/jansson/releases/download/v2.14/jansson-2.14.tar.bz2
jasper                    4.2.4        30 Apr 2024  https://github.com/jasper-software/jasper/releases/download/version-4.2.4/jasper-4.2.4.tar.gz
jbig2dec                  0.20         18 Mar 2024  https://github.com/ArtifexSoftware/jbig2dec/releases/download/0.20/jbig2dec-0.20.tar.gz
jbuilder                  2.11.5       27 May 2022  https://rubygems.org/downloads/jbuilder-2.11.5.gem
jed                       0.99-19      11 Mar 2017  https://www.jedsoft.org/releases/jed/jed-0.99-19.tar.bz2
jekyll                    4.3.2        19 Mar 2023  https://rubygems.org/downloads/jekyll-4.3.2.gem
jemalloc                  5.3.0        15 Sep 2022  https://github.com/jemalloc/jemalloc/releases/download/5.3.0/jemalloc-5.3.0.tar.bz2
jfsutils                  1.1.15       11 Jun 2015  http://jfs.sourceforge.net/project/pub/jfsutils-1.1.15.tar.gz
jhbuild                   3.36.0       06 Mar 2020  http://ftp.gnome.org/pub/gnome/sources/jhbuild/3.36/jhbuild-3.36.0.tar.xz
jigdo                     0.8.0        08 Apr 2021  https://www.einval.com/~steve/software/jigdo/download/jigdo-0.8.0.tar.xz
jinja2                    3.1.5        17 Jan 2025  https://files.pythonhosted.org/packages/af/92/b3130cbbf5591acf9ade8708c365f3238046ac7cb8ccba6e81abccb0ccff/jinja2-3.1.5.tar.gz
joe                       4.6          13 Jan 2018  https://downloads.sourceforge.net/joe-editor/joe-4.6.tar.gz
john                      1.8.0        18 Mar 2017  http://www.openwall.com/john/j/john-1.8.0.tar.xz
jokosher                  0.11.5       05 Oct 2019  https://launchpad.net/jokosher/trunk/0.11.5/+download/jokosher-0.11.5.tar.gz
jp2a                      1.0.6        01 Jun 2014  https://sourceforge.net/projects/jp2a/files/jp2a/1.0.6/jp2a-1.0.6.tar.bz2
jpegsr                    9c           09 Dec 2018  http://www.ijg.org/files/jpegsr-9c.zip
v                         1.5.0        29 Sep 2022  https://github.com/tjko/jpegoptim/archive/refs/tags/v1.5.0.tar.gz
jq                        1.7.1        01 Nov 2024  https://github.com/jqlang/jq/releases/download/jq-1.7.1/jq-1.7.1.tar.gz
jquery                    3.6.0        24 May 2021  https://code.jquery.com/jquery-3.6.0.js
jqueryui                  1.12.1       16 Jul 2020  http://code.jquery.com/jquery-ui-1.12.1.js
jrubydist                 9.4.3.0      04 Jul 2023  https://repo1.maven.org/maven2/org/jruby/jruby-dist/9.4.3.0/jruby-dist-9.4.3.0-bin.tar.gz
json                      2.6.2        27 May 2022  https://rubygems.org/downloads/json-2.6.2.gem
jsonc                     0.18         19 Sep 2024  https://s3.amazonaws.com/json-c_releases/releases/json-c-0.18.tar.gz
jsonglib                  1.10.6       11 Dec 2024  https://download.gnome.org/sources/json-glib/1.10/json-glib-1.10.6.tar.xz
jsonpp                    4.04         09 Nov 2019  https://cpan.metacpan.org/authors/id/I/IS/ISHIGAKI/JSON-PP-4.04.tar.gz
jsonpure                  2.6.2        27 May 2022  https://rubygems.org/downloads/json_pure-2.6.2.gem
jsonrpcglib               3.41.0       07 Jan 2022  https://download.gnome.org/sources/jsonrpc-glib/3.41/jsonrpc-glib-3.41.0.tar.xz
jthread                   1.3.1        01 Jun 2014  http://research.edm.uhasselt.be/jori/jthread/jthread-1.3.1.tar.bz2
judy                      1.0.5        01 Jun 2014  http://sourceforge.net/projects/judy/files/judy/Judy-1.0.5/Judy-1.0.5.tar.gz
juk                       24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/juk-24.02.2.tar.xz
julia                     1.9.2        15 Aug 2023  https://github.com/JuliaLang/julia/releases/download/v1.9.2/julia-1.9.2.tar.gz
jumpstart                 0.3          01 Jun 2014  http://bitwagon.com/jumpstart/jumpstart-0.3.tgz
jwm                       2.2.2        24 May 2015  http://joewing.net/projects/jwm/releases/jwm-2.2.2.tar.xz
k3b                       24.08.0      23 Aug 2024  https://download.kde.org/stable/release-service/24.08.0/src/k3b-24.08.0.tar.xz
kaccountsintegration      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kaccounts-integration-24.02.2.tar.xz
kaccountsproviders        24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kaccounts-providers-24.02.2.tar.xz
kactivities               5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kactivities-5.116.0.tar.xz
kactivitiesstats          5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kactivities-stats-5.116.0.tar.xz
kactivitymanagerd         6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kactivitymanagerd-6.2.5.tar.xz
kaddressbook              24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kaddressbook-24.02.2.tar.xz
kaffeine                  1.2.2        22 Aug 2022  http://downloads.sourceforge.net/kaffeine/kaffeine-1.2.2.tar.gz
kajaanikombat             0.7          01 Jan 2013  http://kombat.kajaani.net/dl/kajaani-kombat-0.7.tar.gz
kajongg                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kajongg-24.02.2.tar.xz
kalarm                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kalarm-24.02.2.tar.xz
kalarmcal                 21.12.3      03 Mar 2022  https://download.kde.org/stable/release-service/21.12.3/src/kalarmcal-21.12.3.tar.xz
kalendar                  23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/kalendar-23.04.3.tar.xz
kalgebra                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kalgebra-24.02.2.tar.xz
kalk                      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kalk-24.02.2.tar.xz
kalzium                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kalzium-24.02.2.tar.xz
kamera                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kamera-24.02.2.tar.xz
kamikaze                  0.2.2        01 Jun 2014  http://kamikaze.coolprojects.org/download/kamikaze-0.2.2.tar.gz
kamoso                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kamoso-24.02.2.tar.xz
kanagram                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kanagram-24.02.2.tar.xz
kapidox                   6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kapidox-6.8.0.tar.xz
kapman                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kapman-24.02.2.tar.xz
kapptemplate              24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kapptemplate-24.02.2.tar.xz
karchive                  6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/karchive-6.8.0.tar.xz
kasablanca                0.4.0.2      01 Jan 2013  http://download.berlios.de/kasablanca/kasablanca-0.4.0.2.tar.gz
kasts                     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kasts-24.02.2.tar.xz
katchtv                   katchtv      01 Jun 2014  http://www.digitalunleashed.com/downloads/katchtv/katchtv_latest_release.tar.bz2
kate                      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kate-24.02.2.tar.xz
katomic                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/katomic-24.02.2.tar.xz
kauth                     6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kauth-6.8.0.tar.xz
kawa                      3.1.1        17 Jan 2020  http://ftp.gnu.org/pub/gnu/kawa/kawa-3.1.1.tar.gz
kbackup                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kbackup-24.02.2.tar.xz
kbd                       2.6.4        21 Jan 2024  https://mirrors.edge.kernel.org/pub/linux/utils/kbd/kbd-2.6.4.tar.xz
                          1081006      01 Jun 2014  https://www.linux-apps.com/p/1081006/
kblackbox                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kblackbox-24.02.2.tar.xz
kblocks                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kblocks-24.02.2.tar.xz
kblog                     20.04.3      13 Aug 2020  https://download.kde.org/stable/release-service/20.04.3/src/kblog-20.04.3.tar.xz
kbookmarks                6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kbookmarks-6.8.0.tar.xz
kbounce                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kbounce-24.02.2.tar.xz
kbproto                   1.0.7        04 May 2015  http://xorg.freedesktop.org/releases/individual/proto/kbproto-1.0.7.tar.bz2
kbreakout                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kbreakout-24.02.2.tar.xz
kbruch                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kbruch-24.02.2.tar.xz
kcachegrind               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kcachegrind-24.02.2.tar.xz
kcalc                     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kcalc-24.02.2.tar.xz
kcalcore                  19.08.3      01 Dec 2019  https://download.kde.org/stable/applications/19.08.3/src/kcalcore-19.08.3.tar.xz
kcalendarcore             6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcalendarcore-6.8.0.tar.xz
kcalutils                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kcalutils-24.02.2.tar.xz
kcharselect               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kcharselect-24.02.2.tar.xz
kchmviewer                7.7          16 Sep 2017  https://sourceforge.net/projects/kchmviewer/files/kchmviewer/7.7/kchmviewer-7.7.tar.gz
kclock                    24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kclock-24.02.2.tar.xz
kcmutils                  6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcmutils-6.8.0.tar.xz
kcodecs                   6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcodecs-6.8.0.tar.xz
kcolorchooser             24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kcolorchooser-24.02.2.tar.xz
kcolorscheme              6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcolorscheme-6.8.0.tar.xz
kcompletion               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcompletion-6.8.0.tar.xz
kconfig                   6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kconfig-6.8.0.tar.xz
kconfigwidgets            6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kconfigwidgets-6.8.0.tar.xz
kcontacts                 6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcontacts-6.8.0.tar.xz
kcoreaddons               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcoreaddons-6.8.0.tar.xz
kcrash                    6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kcrash-6.8.0.tar.xz
kcron                     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kcron-24.02.2.tar.xz
kdav                      6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kdav-6.8.0.tar.xz
kdbg                      2.1.0        01 Jun 2014  http://prdownloads.sourceforge.net/kdbg/kdbg-2.1.0.tar.gz
kdbusaddons               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kdbusaddons-6.8.0.tar.xz
kdebaseruntime            4.3.90       08 Mar 2015  http://ftp.gwdg.de/pub/x11/kde/stable/4.0.2/src/kdebase-runtime-4.3.90.tar.xz
kdebugsettings            24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdebugsettings-24.02.2.tar.xz
kdeclarative              6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kdeclarative-6.8.0.tar.xz
kdeclitools               6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kde-cli-tools-6.2.5.tar.xz
kdeconnectkde             24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdeconnect-kde-24.02.2.tar.xz
kdecoration               6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kdecoration-6.2.5.tar.xz
kded                      6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kded-6.8.0.tar.xz
kdedevscripts             24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kde-dev-scripts-24.02.2.tar.xz
kdedevutils               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kde-dev-utils-24.02.2.tar.xz
kdeedudata                24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdeedu-data-24.02.2.tar.xz
kdegraphicsmobipocket     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdegraphics-mobipocket-24.02.2.tar.xz
kdegraphicsstrigianalyzer 4.12.97      01 Apr 2014  ftp://gd.tuwien.ac.at/kde/unstable/4.12.97/src/kdegraphics-strigi-analyzer-4.12.97.tar.xz
kdegraphicsthumbnailers   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdegraphics-thumbnailers-24.02.2.tar.xz
kdegtkconfig              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kde-gtk-config-6.2.5.tar.xz
kdeinotifysurvey          24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kde-inotify-survey-24.02.2.tar.xz
kdelibs4support           5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/kdelibs4support-5.116.0.tar.xz
kdenetworkfilesharing     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdenetwork-filesharing-24.02.2.tar.xz
kdenlive                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdenlive-24.02.2.tar.xz
kdepimaddons              24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdepim-addons-24.02.2.tar.xz
kdepimappslibs            20.08.3      05 Nov 2020  https://download.kde.org/stable/release-service/20.08.3/src/kdepim-apps-libs-20.08.3.tar.xz
kdepimruntime             24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdepim-runtime-24.02.2.tar.xz
kdeplasmaaddons           6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kdeplasma-addons-6.2.5.tar.xz
kdesdkkio                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdesdk-kio-24.02.2.tar.xz
kdesdkkioslaves           22.04.3      07 Jul 2022  https://download.kde.org/stable/release-service/22.04.3/src/kdesdk-kioslaves-22.04.3.tar.xz
kdesdkthumbnailers        24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdesdk-thumbnailers-24.02.2.tar.xz
kdesignerplugin           5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/kdesignerplugin-5.116.0.tar.xz
kdesu                     6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kdesu-6.8.0.tar.xz
kdesvn                    0.14.6       01 Jun 2014  http://www.alwins-world.de/programs/download/kdesvn/0.14.x/kdesvn-0.14.6.tar.bz2
kdevelop                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdevelop-24.02.2.tar.xz
kdevphp                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdev-php-24.02.2.tar.xz
kdevpython                24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdev-python-24.02.2.tar.xz
kdewebkit                 5.99.0       10 Oct 2022  https://download.kde.org/stable/frameworks/5.99/portingAids/kdewebkit-5.99.0.tar.xz
kdf                       24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdf-24.02.2.tar.xz
v                         2.7.0        17 Jun 2020  https://github.com/KDE/kdiagram/archive/v2.7.0.tar.gz
kdialog                   24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdialog-24.02.2.tar.xz
kdiamond                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kdiamond-24.02.2.tar.xz
kdnssd                    6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kdnssd-6.8.0.tar.xz
kdoctools                 6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kdoctools-6.8.0.tar.xz
kdsoap                    2.1.1        18 Sep 2022  https://github.com/KDAB/KDSoap/releases/download/kdsoap-2.1.1/kdsoap-2.1.1.tar.gz
keditbookmarks            24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/keditbookmarks-24.02.2.tar.xz
kemoticons                5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kemoticons-5.116.0.tar.xz
kexectools                2.0.18       31 Oct 2018  https://mirrors.edge.kernel.org/pub/linux/utils/kernel/kexec/kexec-tools-2.0.18.tar.xz
0                         0.3.2        27 May 2020  https://github.com/kupferlauncher/keybinder/releases/download/keybinder-3.0-v0.3.2/keybinder-3.0-0.3.2.tar.gz
keysmith                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/keysmith-24.02.2.tar.xz
keytouch                  2.4.0beta    01 Jun 2014  http://keytouch.sourceforge.net/keytouch-2.4.0beta.tar.xz
keyutils                  1.6.1        30 Nov 2019  http://people.redhat.com/~dhowells/keyutils/keyutils-1.6.1.tar.bz2
kfilemetadata             6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kfilemetadata-6.8.0.tar.xz
kfind                     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kfind-24.02.2.tar.xz
kflickr                   20140907     24 Apr 2016  http://sourceforge.net/projects/kflickr/files/kflickr/20140907/kflickr-20140907.tar.bz2
kfloppy                   23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/kfloppy-23.04.3.tar.xz
kfourinline               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kfourinline-24.02.2.tar.xz
kgamma                    6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kgamma-6.2.5.tar.xz
kgeography                24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kgeography-24.02.2.tar.xz
kget                      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kget-24.02.2.tar.xz
kglobalaccel              6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kglobalaccel-6.8.0.tar.xz
kglobalacceld             6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kglobalacceld-6.2.5.tar.xz
kgoldrunner               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kgoldrunner-24.02.2.tar.xz
kgpg                      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kgpg-24.02.2.tar.xz
kgraphviewer              1.99.1       01 Jun 2014  http://download.gna.org/kgraphviewer/kgraphviewer-1.99.1.tar.bz2
kguiaddons                6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kguiaddons-6.8.0.tar.xz
khangman                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/khangman-24.02.2.tar.xz
khard                     0.13.0       24 Dec 2018  https://files.pythonhosted.org/packages/75/17/5b97ce7dd15e835e801f513dd80fb4fee8c6f45f5c5be7c3a2b8b839121a/khard-0.13.0.tar.gz
khealthcertificate        24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/khealthcertificate-24.02.2.tar.xz
khelpcenter               24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/khelpcenter-24.02.2.tar.xz
kholidays                 6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kholidays-6.8.0.tar.xz
khotkeys                  5.27.9       24 Oct 2023  https://download.kde.org/stable/plasma/5.27.9/khotkeys-5.27.9.tar.xz
khtml                     5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/khtml-5.116.0.tar.xz
ki18n                     6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/ki18n-6.8.0.tar.xz
kiconthemes               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kiconthemes-6.8.0.tar.xz
kid                       0.9.6        17 Dec 2018  https://files.pythonhosted.org/packages/85/59/5256812b4b90cf379be2947142aa384f0b2f739643933ffab66914c79332/kid-0.9.6.tar.gz
kid3                      3.9.5        25 Feb 2024  https://prdownloads.sourceforge.net/kid3/kid3-3.9.5.tar.gz
kidentitymanagement       24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kidentitymanagement-24.02.2.tar.xz
kidletime                 6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kidletime-6.8.0.tar.xz
kig                       24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kig-24.02.2.tar.xz
kigo                      24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kigo-24.02.2.tar.xz
kildclient                2.7.0        01 Jun 2014  http://heanet.dl.sourceforge.net/sourceforge/kildclient/kildclient-2.7.0.tar.gz
kile                      2.9.93       26 Jul 2022  https://sourceforge.net/projects/kile/files/unstable/kile-3.0b3/kile-2.9.93.tar.bz2
killbots                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/killbots-24.02.2.tar.xz
kimageformats             6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kimageformats-6.8.0.tar.xz
kimagemapeditor           24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kimagemapeditor-24.02.2.tar.xz
kimap                     24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kimap-24.02.2.tar.xz
kimurai                   1.4.0        18 Feb 2019  https://rubygems.org/downloads/kimurai-1.4.0.gem
kimwitu++                 2.3.11       26 Sep 2017  https://www2.informatik.hu-berlin.de/sam/kimwitu++/kimwitu++-2.3.11.tar.gz
kinfocenter               6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kinfocenter-6.2.5.tar.xz
kinit                     5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kinit-5.116.0.tar.xz
kio                       6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kio-6.8.0.tar.xz
kioadmin                  24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kio-admin-24.02.2.tar.xz
kioextras                 24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kio-extras-24.02.2.tar.xz
kiogdrive                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kio-gdrive-23.08.5.tar.xz
kiozeroconf               23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kio-zeroconf-23.08.5.tar.xz
kipiplugins               23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kipi-plugins-23.08.5.tar.xz
kirigami                  6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kirigami-6.8.0.tar.xz
kirigami2                 5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kirigami2-5.116.0.tar.xz
kirigamigallery           23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kirigami-gallery-23.08.5.tar.xz
kiriki                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kiriki-23.08.5.tar.xz
kismet2020                04-R3        23 May 2020  https://www.kismetwireless.net/code/kismet-2020-04-R3.tar.xz
kite                      1.0.4        01 Jun 2014  http://www.kite-language.org/files/kite-1.0.4.tar.gz
kitemmodels               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kitemmodels-6.8.0.tar.xz
kitemviews                6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kitemviews-6.8.0.tar.xz
kiten                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kiten-23.08.5.tar.xz
kitinerary                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kitinerary-23.08.5.tar.xz
kiwi                      1.9.29       20 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/kiwi/1.9/kiwi-1.9.29.tar.xz
kjobwidgets               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kjobwidgets-6.8.0.tar.xz
kjournald                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kjournald-23.08.5.tar.xz
kjs                       5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/kjs-5.116.0.tar.xz
kjsembed                  5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/kjsembed-5.116.0.tar.xz
kjumpingcube              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kjumpingcube-23.08.5.tar.xz
klavaro                   3.03         01 Jun 2014  http://downloads.sourceforge.net/project/klavaro/klavaro-3.03.tar.bz2
kldap                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kldap-23.08.5.tar.xz
kleopatra                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kleopatra-23.08.5.tar.xz
klettres                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/klettres-23.08.5.tar.xz
klickety                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/klickety-23.08.5.tar.xz
klineakconfig             0.8-beta2    01 Jun 2014  http://nchc.dl.sourceforge.net/sourceforge/lineak/klineakconfig-0.8-beta2.tar.gz
klines                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/klines-23.08.5.tar.xz
kmag                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmag-23.08.5.tar.xz
kmahjongg                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmahjongg-23.08.5.tar.xz
kmail                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmail-23.08.5.tar.xz
kmailaccountwizard        23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmail-account-wizard-23.08.5.tar.xz
kmailtransport            24.02.2      12 Apr 2024  https://download.kde.org/stable/release-service/24.02.2/src/kmailtransport-24.02.2.tar.xz
kmarkdownwebview          0.3.0        01 Nov 2017  https://download.kde.org/stable/kmarkdownwebview/0.3.0/src/kmarkdownwebview-0.3.0.tar.xz
kmbox                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmbox-23.08.5.tar.xz
kmediaplayer              5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/kmediaplayer-5.116.0.tar.xz
kmenuedit                 6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kmenuedit-6.2.5.tar.xz
kmime                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmime-23.08.5.tar.xz
kmines                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmines-23.08.5.tar.xz
kmix                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmix-23.08.5.tar.xz
kmod                      33           14 Aug 2024  https://www.kernel.org/pub/linux/utils/kernel/kmod/kmod-33.tar.xz
kmousetool                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmousetool-23.08.5.tar.xz
kmouth                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmouth-23.08.5.tar.xz
kmp3burn                  0.3.1        01 Jun 2014  http://www.martin-keil.de/Kmp3burn-0.3.1
kmpg2                     1.95         01 Jun 2014  http://nchc.dl.sourceforge.net/sourceforge/kmpg2/KmPg2-1.95.tar.bz2
kmplayer                  0.12.0b      15 Jun 2018  http://mirror.freedif.org/KDE/ftp/stable/kmplayer/0.12/kmplayer-0.12.0b.tar.bz2
kmplot                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kmplot-23.08.5.tar.xz
kmuddy                    1.0.1        22 Mar 2016  http://www.kmuddy.com/releases/stable/kmuddy-1.0.1.tar.gz
kmymoney                  5.1.3        30 Jul 2022  https://download.kde.org/stable/kmymoney/5.1.3/src/kmymoney-5.1.3.tar.xz
knavalbattle              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/knavalbattle-23.08.5.tar.xz
knetwalk                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/knetwalk-23.08.5.tar.xz
knewstuff                 6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/knewstuff-6.8.0.tar.xz
knights                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/knights-23.08.5.tar.xz
knotes                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/knotes-23.08.5.tar.xz
knotifications            6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/knotifications-6.8.0.tar.xz
knotifyconfig             6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/knotifyconfig-6.8.0.tar.xz
kobodeluxe                0.5.1        01 Aug 2013  http://www.olofson.net/kobodl/download/KoboDeluxe-0.5.1.tar.bz2
koko                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/koko-23.08.5.tar.xz
kolf                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kolf-23.08.5.tar.xz
kollision                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kollision-23.08.5.tar.xz
kolourpaint               24.12.1      17 Jan 2025  https://download.kde.org/stable/release-service/24.12.1/src/kolourpaint-24.12.1.tar.xz
kompare                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kompare-23.08.5.tar.xz
konforka                  0.0          01 Jun 2014  http://kin.klever.net/dist/konforka-0.0.tar.bz2
kongress                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kongress-23.08.5.tar.xz
konqueror                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/konqueror-23.08.5.tar.xz
konquest                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/konquest-23.08.5.tar.xz
konsole                   24.12.1      17 Jan 2025  https://download.kde.org/stable/release-service/24.12.1/src/konsole-24.12.1.tar.xz
kontact                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kontact-23.08.5.tar.xz
kontactinterface          23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kontactinterface-23.08.5.tar.xz
kontrast                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kontrast-23.08.5.tar.xz
konversation              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/konversation-23.08.5.tar.xz
kopeninghours             23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kopeninghours-23.08.5.tar.xz
kopete                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kopete-23.08.5.tar.xz
korganizer                24.02.1      24 Mar 2024  https://download.kde.org/stable/release-service/24.02.1/src/korganizer-24.02.1.tar.xz
kosmindoormap             23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kosmindoormap-23.08.5.tar.xz
kpackage                  6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kpackage-6.8.0.tar.xz
kparts                    6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kparts-6.8.0.tar.xz
kpat                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kpat-23.08.5.tar.xz
kpeople                   6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kpeople-6.8.0.tar.xz
kpimtextedit              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kpimtextedit-23.08.5.tar.xz
kpipewire                 6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kpipewire-6.2.5.tar.xz
kpkpass                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kpkpass-23.08.5.tar.xz
kplayer                   0.6.3        01 Jun 2014  http://prdownloads.sourceforge.net/kplayer/kplayer-0.6.3.tar.bz2
kplotting                 6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kplotting-6.8.0.tar.xz
kpmcore                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kpmcore-23.08.5.tar.xz
kpty                      6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kpty-6.8.0.tar.xz
kpublictransport          23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kpublictransport-23.08.5.tar.xz
kqtquickcharts            23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kqtquickcharts-23.08.5.tar.xz
kquickcharts              6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kquickcharts-6.8.0.tar.xz
kramdown                  2.4.0        27 May 2022  https://rubygems.org/downloads/kramdown-2.4.0.gem
krb5                      1.21.3       08 Jul 2024  https://web.mit.edu/kerberos/dist/krb5/1.21/krb5-1.21.3.tar.gz
krb5authdialog            3.26.1       12 Nov 2017  http://ftp.gnome.org/pub/gnome/sources/krb5-auth-dialog/3.26/krb5-auth-dialog-3.26.1.tar.xz
krdc                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/krdc-23.08.5.tar.xz
krdp                      6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/krdp-6.2.5.tar.xz
krecorder                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/krecorder-23.08.5.tar.xz
kreversi                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kreversi-23.08.5.tar.xz
krfb                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/krfb-23.08.5.tar.xz
krita                     5.2.6        18 Jan 2025  https://download.kde.org/stable/krita/5.2.6/krita-5.2.6.tar.xz
kross                     5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/kross-5.116.0.tar.xz
krossinterpreters         23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kross-interpreters-23.08.5.tar.xz
kruler                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kruler-23.08.5.tar.xz
krunner                   6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/krunner-6.8.0.tar.xz
krusader                  2.9.0        01 Jan 2025  https://download.kde.org/stable/krusader/2.9.0/krusader-2.9.0.tar.xz
ksanecore                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ksanecore-23.08.5.tar.xz
kscreen                   6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kscreen-6.2.5.tar.xz
kscreenlocker             6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kscreenlocker-6.2.5.tar.xz
kseexpr                   14.09.2022   14 Sep 2022  kseexpr-14.09.2022
kservice                  6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kservice-6.8.0.tar.xz
kshisen                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kshisen-23.08.5.tar.xz
kshutdown                 1.0.2        01 Jun 2014  http://superb-west.dl.sourceforge.net/sourceforge/kshutdown/kshutdown-1.0.2.tar.bz2
ksirk                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ksirk-23.08.5.tar.xz
v                         5.11         22 Dec 2018  https://github.com/dangvd/ksmoothdock/archive/v5.11.tar.gz
ksmtp                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ksmtp-23.08.5.tar.xz
ksnakeduel                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ksnakeduel-23.08.5.tar.xz
kspaceduel                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kspaceduel-23.08.5.tar.xz
ksquares                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ksquares-23.08.5.tar.xz
ksshaskpass               6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/ksshaskpass-6.2.5.tar.xz
kstars                    3.7.0        09 Apr 2024  https://mirror.kumi.systems/kde/ftp/stable/kstars/kstars-3.7.0.tar.xz
kstatusnotifieritem       6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kstatusnotifieritem-6.8.0.tar.xz
ksudoku                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ksudoku-23.08.5.tar.xz
ksvg                      6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/ksvg-6.8.0.tar.xz
ksysguard                 5.21.5       04 May 2021  https://download.kde.org/stable/plasma/5.21.5/ksysguard-5.21.5.tar.xz
ksystemlog                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ksystemlog-23.08.5.tar.xz
ksystemstats              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/ksystemstats-6.2.5.tar.xz
kteatime                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kteatime-23.08.5.tar.xz
ktexteditor               6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/ktexteditor-6.8.0.tar.xz
ktexttemplate             6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/ktexttemplate-6.8.0.tar.xz
ktextwidgets              6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/ktextwidgets-6.8.0.tar.xz
ktimer                    24.02.1      21 Mar 2024  https://download.kde.org/stable/release-service/24.02.1/src/ktimer-24.02.1.tar.xz
ktnef                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ktnef-23.08.5.tar.xz
ktorrent                  24.02.1      21 Mar 2024  https://download.kde.org/stable/release-service/24.02.1/src/ktorrent-24.02.1.tar.xz
ktouch                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ktouch-23.08.5.tar.xz
ktpaccountskcm            23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-accounts-kcm-23.04.3.tar.xz
ktpapprover               23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-approver-23.04.3.tar.xz
ktpauthhandler            23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-auth-handler-23.04.3.tar.xz
ktpcallui                 23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-call-ui-23.04.3.tar.xz
ktpcommoninternals        23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-common-internals-23.04.3.tar.xz
ktpcontactlist            23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-contact-list-23.04.3.tar.xz
ktpcontactrunner          23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-contact-runner-23.04.3.tar.xz
ktpdesktopapplets         23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-desktop-applets-23.04.3.tar.xz
ktpfiletransferhandler    23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-filetransfer-handler-23.04.3.tar.xz
ktpkdedmodule             23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-kded-module-23.04.3.tar.xz
ktpsendfile               23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-send-file-23.04.3.tar.xz
ktptextui                 23.04.3      06 Jul 2023  https://download.kde.org/stable/release-service/23.04.3/src/ktp-text-ui-23.04.3.tar.xz
ktrip                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ktrip-23.08.5.tar.xz
ktuberling                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/ktuberling-23.08.5.tar.xz
kturtle                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kturtle-23.08.5.tar.xz
kubrick                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kubrick-23.08.5.tar.xz
kuklomenos                0.4.5        10 Oct 2017  http://mbays.freeshell.org/kuklomenos/src/kuklomenos-0.4.5.tar.gz
kunitconversion           6.8.0        25 Nov 2024  https://download.kde.org/stable/frameworks/6.8/kunitconversion-6.8.0.tar.xz
kvkbd?content=            56019        01 Jun 2014  http://www.kde-apps.org/content/show.php/Kvkbd?content=56019
kwalify                   0.7.2        01 Dec 2017  https://rubygems.org/downloads/kwalify-0.7.2.gem
kwallet                   5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kwallet-5.116.0.tar.xz
kwalletmanager            23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kwalletmanager-23.08.5.tar.xz
kwalletpam                6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kwallet-pam-6.2.5.tar.xz
kwave                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kwave-23.08.5.tar.xz
kwayland                  6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kwayland-6.2.5.tar.xz
kwaylandintegration       6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kwayland-integration-6.2.5.tar.xz
kwaylandserver            5.24.5       04 May 2022  https://download.kde.org/stable/plasma/5.24.5/kwayland-server-5.24.5.tar.xz
kweather                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kweather-23.08.5.tar.xz
kwidgetsaddons            5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kwidgetsaddons-5.116.0.tar.xz
kwifiselector             0.8          01 Jun 2014  https://sourceforge.net/projects/kwifiselector/files/kwifiselector/0.8/kwifiselector-0.8.tar.gz
kwin                      6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kwin-6.2.5.tar.xz
kwindowsystem             5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kwindowsystem-5.116.0.tar.xz
kwordquiz                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/kwordquiz-23.08.5.tar.xz
kwrited                   6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/kwrited-6.2.5.tar.xz
kxmlgui                   5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/kxmlgui-5.116.0.tar.xz
kxmlrpcclient             5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/portingAids/kxmlrpcclient-5.116.0.tar.xz
kytea                     0.4.7        09 Aug 2024  https://www.phontron.com/kytea/download/kytea-0.4.7.tar.gz
lablgtk                   2.10.0       01 Jun 2014  http://wwwfun.kurims.kyoto-u.ac.jp/soft/olabl/dist/lablgtk-2.10.0.tar.gz
labplot                   2.9.0        03 Sep 2022  https://download.kde.org/stable/labplot/2.9.0/labplot-2.9.0.tar.xz
ladspasdk                 1.13         01 Nov 2012  https://www.ladspa.org/download/ladspa_sdk-1.13.tgz
lame                      3.100        26 Oct 2017  https://sourceforge.net/projects/lame/files/lame/3.100/lame-3.100.tar.gz
lapack                    15.03.2024   11 May 2019  http://www.netlib.org/lapack/lapack-15.03.2024.tar.gz
larswm                    7.5.3        27 Jul 2017  http://porneia.free.fr/larswm/larswm-7.5.3.tar.gz
lasem                     0.5.1        24 May 2019  http://ftp.gnome.org/pub/gnome/sources/lasem/0.5/lasem-0.5.1.tar.xz
latexila                  3.27.1       09 Dec 2017  http://ftp.gnome.org/pub/gnome/sources/latexila/3.27/latexila-3.27.1.tar.xz
launchy                   2.5.0        27 May 2022  https://rubygems.org/downloads/launchy-2.5.0.gem
layershellqt              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/layer-shell-qt-6.2.5.tar.xz
lbreakout                 010315       01 Dec 2012  http://prdownloads.sourceforge.net/lgames/lbreakout-010315.tar.gz
lbreakout2                2.6.5        07 Jun 2018  http://prdownloads.sourceforge.net/lgames/lbreakout2-2.6.5.tar.gz
lbreakouthd               1.1.6        19 Feb 2024  https://sourceforge.net/projects/lgames/files/lbreakouthd/lbreakouthd-1.1.6.tar.gz
lbxproxy                  1.0.3        09 Jun 2018  https://www.x.org/releases/individual/app/lbxproxy-1.0.3.tar.bz2
lcms                      1.19         01 Jan 2013  http://sourceforge.net/projects/lcms/files/lcms/1.19/lcms-1.19.tar.gz
lcms2                     2.16         04 Aug 2024  https://sourceforge.net/projects/lcms/files/lcms/2.16/lcms2-2.16.tar.gz
ldmud                     3.5.0        22 Feb 2017  https://github.com/ldmud/ldmud/tarball/master/ldmud-3.5.0
ldns                      1.8.4        23 Jul 2024  https://www.nlnetlabs.nl/downloads/ldns/ldns-1.8.4.tar.gz
leafpad                   0.8.17       24 Nov 2017  http://savannah.nongnu.org/download/leafpad/leafpad-0.8.17.tar.gz
leptonica                 1.84.1       07 Jul 2024  https://github.com/DanBloomberg/leptonica/releases/download/1.84.1/leptonica-1.84.1.tar.gz
less                      668          17 Oct 2024  http://www.greenwoodsoftware.com/less/less-668.tar.gz
lesstif                   0.95.2       01 Jun 2014  https://sourceforge.net/projects/lesstif/files/lesstif/0.95.2/lesstif-0.95.2.tar.gz
lfhex                     0.4          01 Jun 2014  http://stoopidsimple.com/files/lfhex-0.4.tar.gz
lftp                      25.06.2024   25 Jun 2024  http://lftp.yar.ru/ftp/lftp-25.06.2024
lgeneral                  1.4.4        02 Apr 2022  http://prdownloads.sourceforge.net/lgeneral/lgeneral-1.4.4.tar.gz
libabigail                1.5          30 Oct 2018  http://mirrors.kernel.org/sourceware/libabigail/libabigail-1.5.tar.gz
libadwaita                1.6.3        22 Jan 2025  https://download.gnome.org/sources/libadwaita/1.6/libadwaita-1.6.3.tar.xz
libao                     1.2.0        14 Aug 2017  https://ftp.osuosl.org/pub/xiph/releases/ao/libao-1.2.0.tar.gz
libaom                    3.9.0        22 Jan 2025  https://storage.googleapis.com/aom-releases/libaom-3.9.0.tar.gz
libapplewm                1.3.0        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/lib/libAppleWM-1.3.0.tar.bz2
libapreq2                 2.08         01 Jun 2014  http://apache.4any.org/httpd/libapreq/libapreq2-2.08.tar.gz
libarchive                3.7.7        15 Oct 2024  https://github.com/libarchive/libarchive/releases/download/v3.7.7/libarchive-3.7.7.tar.xz
libartlgpl                2.3.21       01 Jun 2014  ftp://ftp.gnome.org/pub/GNOME/sources/libart_lgpl/2.3/libart_lgpl-2.3.21.tar.bz2
libass                    0.17.2       23 May 2024  https://github.com/libass/libass/releases/download/0.17.2/libass-0.17.2.tar.xz
libassuan                 2.5.7        08 Mar 2024  https://gnupg.org/ftp/gcrypt/libassuan/libassuan-2.5.7.tar.bz2
libast                    0.7          09 Jul 2015  http://www.eterm.org/download/libast-0.7.tar.gz
libatasmart               0.19         01 Jun 2014  http://0pointer.de/public/libatasmart-0.19.tar.xz
libatomicops              7.8.0        05 Apr 2023  https://github.com/ivmai/libatomic_ops/releases/download/v7.8.0/libatomic_ops-7.8.0.tar.gz
libav                     11.3         22 May 2015  https://libav.org/releases/libav-11.3.tar.xz
libavc1394                0.5.4        01 Jun 2014  https://sourceforge.net/projects/libavc1394/files/libavc1394/libavc1394-0.5.4.tar.gz
libavg                    0.7.0        01 Jun 2014  http://www.libavg.de/libavg-0.7.0.tar.gz
libavif                   1.0.4        12 Oct 2024  https://github.com/AOMediaCodec/libavif/archive/v1.0.4/libavif-1.0.4.tar.gz
libbacktrace              22.03.2024   22 Mar 2024  https://download.kde.org/stable/libbacktrace-22.03.2024.tar.xz
libbash                   0.9.10       01 Jun 2014  http://surfnet.dl.sourceforge.net/sourceforge/libbash/libbash-0.9.10.tar.bz2
libblockdev               3.2.1        10 Nov 2024  https://github.com/storaged-project/libblockdev/releases/download/3.2.1/libblockdev-3.2.1.tar.gz
libbluray                 1.3.4        02 Dec 2022  https://download.videolan.org/pub/videolan/libbluray/1.3.4/libbluray-1.3.4.tar.bz2
libbonobo                 2.32.1       01 May 2012  http://ftp.gnome.org/pub/GNOME/sources/libbonobo/2.32/libbonobo-2.32.1.tar.bz2
libbpg                    0.9.8        10 Nov 2018  https://bellard.org/bpg/libbpg-0.9.8.tar.gz
libbtctl                  0.11.1       01 Jun 2014  http://ftp.gnome.org/pub/gnome/sources/libbtctl/0.11/libbtctl-0.11.1.tar.bz2
libburn                   1.5.6        25 Jun 2023  https://files.libburnia-project.org/releases/libburn-1.5.6.tar.gz
libbytesize               2.11         22 Aug 2024  https://github.com/storaged-project/libbytesize/releases/download/2.11/libbytesize-2.11.tar.gz
libcaca                   0.99.beta19  12 Mar 2015  http://caca.zoy.org/files/libcaca/libcaca-0.99.beta19.tar.gz
libcacard                 2.5.2        17 Dec 2015  http://www.spice-space.org/download/libcacard/libcacard-2.5.2.tar.xz
libcanberra               0.30         01 Jan 2013  http://0pointer.de/lennart/projects/libcanberra/libcanberra-0.30.tar.xz
libcap                    2.73         04 Dec 2024  https://mirrors.edge.kernel.org/pub/linux/libs/security/linux-privs/libcap2/libcap-2.73.tar.xz
libcap2                   2.24.orig    08 Mar 2015  ftp://ftp.de.debian.org/debian/pool/main/libc/libcap2/libcap2_2.24.orig.tar.xz
libcapng                  0.8          10 Sep 2020  https://people.redhat.com/sgrubb/libcap-ng/libcap-ng-0.8.tar.gz
libcddb                   1.3.2        01 Jun 2014  https://sourceforge.net/projects/libcddb/files/libcddb/libcddb-1.3.2.tar.bz2
libcdio                   2.2.0        22 Jan 2025  https://github.com/libcdio/libcdio/releases/download/2.2.0/libcdio-2.2.0.tar.bz2
libcerf                   1.3          03 Jan 2016  https://sourceforge.net/projects/libcerf/files/libcerf-1.3.tgz
libcgroup                 0.41         02 Aug 2020  https://sourceforge.net/projects/libcg/files/libcgroup/v0.41/libcgroup-0.41.tar.gz
libchamplain              0.12.21      19 Jan 2023  https://download.gnome.org/sources/libchamplain/0.12/libchamplain-0.12.21.tar.xz
libclc                    19.1.0       22 Sep 2024  https://github.com/llvm/llvm-project/releases/download/llvmorg-19.1.0/libclc-19.1.0.tar.xz
libcloudproviders         0.3.6        22 Mar 2024  https://download.gnome.org/sources/libcloudproviders/0.3/libcloudproviders-0.3.6.tar.xz
libcm                     0.1.1        01 Jun 2014  http://ftp.acc.umu.se/pub/GNOME/sources/libcm/0.1/libcm-0.1.1.tar.bz2
libconfig                 1.7.3        16 Jan 2025  https://github.com/hyperrealm/libconfig/releases/download/v1.7.3/libconfig-1.7.3.tar.gz
libcroco                  0.6.13       06 Apr 2019  https://ftp.gnome.org/pub/gnome/sources/libcroco/0.6/libcroco-0.6.13.tar.xz
libcryptui                3.12.2       05 Oct 2019  http://ftp.acc.umu.se/pub/GNOME/sources/libcryptui/3.12/libcryptui-3.12.2.tar.xz
libcsv                    3.0.3        19 Feb 2022  https://sourceforge.net/projects/libcsv/files/libcsv/libcsv-3.0.3/libcsv-3.0.3.tar.gz
libdaemon                 0.14         01 Aug 2013  https://0pointer.de/lennart/projects/libdaemon/libdaemon-0.14.tar.gz
libdap                    3.20.3       19 Apr 2019  https://www.opendap.org/pub/source/libdap-3.20.3.tar.gz
libdatrie                 0.2.13       14 Mar 2024  https://github.com/tlwg/libdatrie/releases/download/v0.2.13/libdatrie-0.2.13.tar.xz
libdazzle                 3.44.0       25 Mar 2022  https://download.gnome.org/sources/libdazzle/3.44/libdazzle-3.44.0.tar.xz
libdbi                    0.9.0        04 Jul 2017  https://sourceforge.net/projects/libdbi/files/libdbi/libdbi-0.9.0/libdbi-0.9.0.tar.gz
libdbusmenu               16.04.0      12 Nov 2020  https://launchpad.net/libdbusmenu/16.04/16.04.0/+download/libdbusmenu-16.04.0.tar.gz
libdc1394                 2.2.6        03 Mar 2023  https://sourceforge.net/projects/libdc1394/files/libdc1394-2/2.2.6/libdc1394-2.2.6.tar.gz
libdca                    0.0.5        01 Jun 2014  http://download.videolan.org/pub/videolan/libdca/0.0.5/libdca-0.0.5.tar.bz2
libdeflate                1.20         23 Mar 2024  https://github.com/ebiggers/libdeflate/releases/download/v1.20/libdeflate-1.20.tar.gz
libdex                    0.7.0        29 Jun 2024  https://download.gnome.org/sources/libdex/0.7/libdex-0.7.0.tar.xz
libdisasm                 0.22         01 Jun 2014  http://freshmeat.net/redir/libdisasm/72406/url_tgz/libdisasm-0.22.tar.gz
libdiscid                 0.6.4        04 Mar 2023  http://ftp.musicbrainz.org/pub/musicbrainz/libdiscid/libdiscid-0.6.4.tar.gz
libdivecomputer           0.4.2        02 Jun 2015  http://www.libdivecomputer.org/releases/libdivecomputer-0.4.2.tar.gz
libdmtx                   0.7.4        19 Sep 2017  https://sourceforge.net/projects/libdmtx/files/libdmtx/0.7.4/libdmtx-0.7.4.tar.gz
libdmx                    1.1.5        05 Jun 2023  https://www.x.org/releases/individual/lib/libdmx-1.1.5.tar.xz
libdnet                   1.12         01 Jun 2014  http://libdnet.googlecode.com/files/libdnet-1.12.tgz
libdrm                    2.4.124      15 Jan 2025  https://dri.freedesktop.org/libdrm/libdrm-2.4.124.tar.xz
libdv                     1.0.0        27 Nov 2019  https://sourceforge.net/projects/libdv/files/libdv/1.0.0/libdv-1.0.0.tar.gz
libdvbpsi                 0.2.0        01 Jun 2014  http://download.videolan.org/pub/libdvbpsi/0.2.0/libdvbpsi-0.2.0.tar.bz2
libdvdcss                 1.4.3        20 Apr 2021  https://download.videolan.org/pub/libdvdcss/1.4.3/libdvdcss-1.4.3.tar.bz2
libdvdnav                 6.1.1        22 Apr 2021  http://download.videolan.org/pub/videolan/libdvdnav/6.1.1/libdvdnav-6.1.1.tar.bz2
libdvdread                6.1.3        26 May 2022  https://download.videolan.org/videolan/libdvdread/6.1.3/libdvdread-6.1.3.tar.bz2
libebml                   1.3.10       12 Dec 2019  https://dl.matroska.org/downloads/libebml/libebml-1.3.10.tar.xz
libedit20170329           3.1          11 Sep 2017  https://thrysoee.dk/editline/libedit-20170329-3.1.tar.gz
libei                     1.2.1        18 Mar 2024  https://gitlab.freedesktop.org/libinput/libei/-/archive/1.2.1/libei-1.2.1.tar.gz
libelf                    0.8.13       01 Jun 2014  http://www.mr511.de/software/libelf-0.8.13.tar.gz
libepc                    0.4.5        04 Dec 2018  http://ftp.gnome.org/pub/gnome/sources/libepc/0.4/libepc-0.4.5.tar.xz
libepoxy                  1.5.10       25 Mar 2022  https://download.gnome.org/sources/libepoxy/1.5/libepoxy-1.5.10.tar.xz
libesmtp                  1.0.6        01 Jun 2014  http://www.stafford.uklinux.net/libesmtp/libesmtp-1.0.6.tar.bz2
libetc                    0.4          31 Oct 2017  http://ordiluc.net/fs/libetc/libetc-0.4.tar.gz
libetpan                  1.1          25 Aug 2016  https://sourceforge.net/projects/libetpan/files/libetpan/1.1/libetpan-1.1.tar.gz
libevdev                  1.13.3       05 Sep 2024  https://www.freedesktop.org/software/libevdev/libevdev-1.13.3.tar.xz
libevent                  2.1.12       06 Jul 2020  https://github.com/libevent/libevent/releases/download/release-2.1.12-stable/libevent-2.1.12-stable.tar.gz
libexif                   0.6.25       18 Jan 2025  https://github.com/libexif/libexif/releases/download/v0.6.25/libexif-0.6.25.tar.xz
libextractor              1.11         08 Feb 2023  https://ftp.gnu.org/gnu/libextractor/libextractor-1.11.tar.gz
libfakekey                0.1          01 Sep 2014  http://downloads.yoctoproject.org/releases/matchbox/libfakekey/0.1/libfakekey-0.1.tar.bz2
libfame                   0.9.1        01 Jun 2014  https://puzzle.dl.sourceforge.net/sourceforge/fame/libfame-0.9.1.tar.gz
libferris                 1.5.19       11 Nov 2017  https://sourceforge.net/projects/witme/files/libferris/1.5.19/libferris-1.5.19.tar.xz
libffado                  2.3.0        13 Apr 2019  http://www.ffado.org/files/libffado-2.3.0.tgz
libffcall                 2.4          13 Jun 2021  https://ftp.gnu.org/pub/gnu/libffcall/libffcall-2.4.tar.gz
libffi                    3.4.6        24 Feb 2024  https://github.com/libffi/libffi/releases/download/v3.4.6/libffi-3.4.6.tar.gz
libfirm                   1.22.0       04 Jan 2016  https://github.com/MatzeB/libfirm/archive/libfirm-1.22.0.tar.gz
libfixbuf                 2.5.0        21 Dec 2024  https://tools.netsa.cert.org/releases/libfixbuf-2.5.0.tar.gz
libflowmanager            2.0.2        20 May 2018  http://research.wand.net.nz/software/libflowmanager/libflowmanager-2.0.2.tar.gz
libfm                     1.3.2        02 Feb 2023  https://sourceforge.net/projects/pcmanfm/files/PCManFM%20+%20Libfm%20%28tarball%20release%29/LibFM/libfm-1.3.2.tar.xz
libfmqt                   0.16.0       07 Nov 2020  https://github.com/lxqt/libfm-qt/releases/download/0.16.0/libfm-qt-0.16.0.tar.xz
libfontenc                1.1.8        04 Mar 2024  https://xorg.freedesktop.org/releases/individual/lib/libfontenc-1.1.8.tar.gz
libfprint                 1.90.0       24 Dec 2019  https://gitlab.freedesktop.org/libfprint/libfprint/uploads/1bba17b5daa130aa548bc7ea96dc58c4/libfprint-1.90.0.tar.xz
libfreebob                1.0.11       01 Jun 2014  https://sourceforge.net/projects/freebob/files/libfreebob-1.x/libfreebob-1.0.11/libfreebob-1.0.11.tar.gz
libfs                     1.0.10       04 Aug 2024  https://www.x.org/releases/individual/lib/libFS-1.0.10.tar.xz
libft                     0.4          01 Jun 2014  http://defiant.homedns.org/~erik/ft/libft/files/libft-0.4.tar.gz
libftdi                   0.13         01 Jun 2014  http://www.intra2net.com/de/produkte/opensource/ftdi/TGZ/libftdi-0.13.tar.gz
libgadu                   1.12.2       19 Sep 2017  http://github.com/wojtekka/libgadu/releases/download/1.12.2/libgadu-1.12.2.tar.gz
libgcrypt                 1.10.2       08 Apr 2023  https://www.gnupg.org/ftp/gcrypt/libgcrypt/libgcrypt-1.10.2.tar.bz2
libgd                     2.3.3        15 Sep 2021  https://github.com/libgd/libgd/releases/download/gd-2.3.3/libgd-2.3.3.tar.xz
libgda                    6.0.0        21 Feb 2022  https://download.gnome.org/sources/libgda/6.0/libgda-6.0.0.tar.xz
libgdamm                  4.99.11      04 Nov 2015  http://ftp.gnome.org/pub/GNOME/sources/libgdamm/4.99/libgdamm-4.99.11.tar.xz
libgdata                  0.18.1       08 Mar 2021  https://download.gnome.org/sources/libgdata/0.18/libgdata-0.18.1.tar.xz
libgdiplus                2.10.9       01 Jan 2013  http://download.mono-project.com/sources/libgdiplus/libgdiplus-2.10.9.tar.bz2
libgee                    0.20.8       17 Jan 2025  https://download.gnome.org/sources/libgee/0.20/libgee-0.20.8.tar.xz
libgepub                  0.7.1        14 Jun 2023  https://download.gnome.org/sources/libgepub/0.7/libgepub-0.7.1.tar.xz
libggi                    2.2.2        01 Jun 2014  http://prdownloads.sourceforge.net/ggi/libggi-2.2.2.tar.gz
libghemical               3.0.0        01 Jun 2014  https://bioinformatics.org/ghemical/download/current/libghemical-3.0.0.tar.gz
libgig                    4.4.1        22 Dec 2024  https://download.linuxsampler.org/packages/libgig-4.4.1.tar.bz2
v                         0.28.1       08 Aug 2017  https://github.com/libgit2/libgit2/archive/v0.28.1.tar.gz
libgit2glib               1.1.0        18 Jul 2022  https://download.gnome.org/sources/libgit2-glib/1.1/libgit2-glib-1.1.0.tar.xz
libgksu                   2.0.12       27 Dec 2017  http://people.debian.org/~kov/gksu/libgksu-2.0.12.tar.gz
libglade                  2.6.4        01 Jun 2014  http://ftp.acc.umu.se/pub/GNOME/sources/libglade/2.6/libglade-2.6.4.tar.bz2
libglademm                2.6.6        01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/libglademm/2.6/libglademm-2.6.6.tar.bz2
libglvnd                  v1.7.0       14 Sep 2023  https://gitlab.freedesktop.org/glvnd/libglvnd/-/archive/v1.7.0/libglvnd-v1.7.0.tar.bz2
libgnomecanvasmm          2.20.0       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/libgnomecanvasmm/2.20/libgnomecanvasmm-2.20.0.tar.bz2
libgnomecups              0.2.3        01 Jun 2014  http://ftp.acc.umu.se/pub/gnome/sources/libgnomecups/0.2/libgnomecups-0.2.3.tar.bz2
wwwgnome                  db           01 Jun 2014  http://www.gnome-db.org/
libgnomegamessupport      2.0.0        17 Mar 2022  https://download.gnome.org/sources/libgnome-games-support/2.0/libgnome-games-support-2.0.0.tar.xz
libgnomekbd               3.28.1       05 Sep 2022  https://download.gnome.org/sources/libgnomekbd/3.28/libgnomekbd-3.28.1.tar.xz
libgnomekeyring           3.12.0       10 Sep 2014  http://ftp.gnome.org/pub/GNOME/sources/libgnome-keyring/3.12/libgnome-keyring-3.12.0.tar.xz
libgnomemm                2.30.0       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/libgnomemm/2.30/libgnomemm-2.30.0.tar.bz2
libgnomeuimm              2.28.0       01 Jun 2014  http://ftp.acc.umu.se/pub/GNOME/sources/libgnomeuimm/2.28/libgnomeuimm-2.28.0.tar.bz2
libgovirt                 0.3.8        26 Feb 2021  https://download.gnome.org/sources/libgovirt/0.3/libgovirt-0.3.8.tar.xz
libgpgerror               1.51         12 Nov 2024  https://www.gnupg.org/ftp/gcrypt/libgpg-error/libgpg-error-1.51.tar.gz
libgphoto2                2.5.28       05 Jan 2022  https://sourceforge.net/projects/gphoto/files/libgphoto/2.5.28/libgphoto2-2.5.28.tar.bz2
libgravatar               23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libgravatar-23.08.5.tar.xz
libgrss                   0.7.0        17 Jul 2015  http://ftp.gnome.org/pub/GNOME/sources/libgrss/0.7/libgrss-0.7.0.tar.xz
libgsasl                  1.10.0       01 Jan 2021  https://ftp.gnu.org/pub/gnu/gsasl/libgsasl-1.10.0.tar.gz
libgsf                    1.14.53      02 Oct 2024  https://download.gnome.org/sources/libgsf/1.14/libgsf-1.14.53.tar.xz
libgtop                   2.41.3       19 Feb 2024  https://download.gnome.org/sources/libgtop/2.41/libgtop-2.41.3.tar.xz
libgudev                  238          14 Mar 2024  https://download.gnome.org/sources/libgudev/238/libgudev-238.tar.xz
libgusb                   0.4.8        09 Nov 2023  https://github.com/hughsie/libgusb/releases/download/0.4.8/libgusb-0.4.8.tar.xz
libgweather               4.4.2        21 Mar 2024  https://download.gnome.org/sources/libgweather/4.4/libgweather-4.4.2.tar.xz
libgxps                   0.3.2        27 Feb 2021  https://download.gnome.org/sources/libgxps/0.3/libgxps-0.3.2.tar.xz
libhandy                  1.8.3        15 Jan 2025  https://download.gnome.org/sources/libhandy/1.8/libhandy-1.8.3.tar.xz
libheif                   05.04.2024   17 Mar 2024  https://github.com/strukturag/libheif/releases/download/v1.17.6/libheif-05.04.2024.tar.gz
libhttpseverywhere        0.8.3        11 Sep 2018  http://ftp.gnome.org/pub/GNOME/sources/libhttpseverywhere/0.8/libhttpseverywhere-0.8.3.tar.xz
libical                   3.0.19       18 Jan 2025  https://github.com/libical/libical/releases/download/v3.0.19/libical-3.0.19.tar.gz
libicalglib               1.0.4        23 Jan 2016  http://ftp.gnome.org/pub/GNOME/sources/libical-glib/1.0/libical-glib-1.0.4.tar.xz
libice                    1.1.1        12 Dec 2022  https://www.x.org/releases/individual/lib/libICE-1.1.1.tar.xz
libiconv                  1.17         22 Feb 2024  https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.17.tar.gz
libid3tag                 0.15.1b      01 Jun 2014  https://sourceforge.net/projects/mad/files/libid3tag/0.15.1b/libid3tag-0.15.1b.tar.gz
libidl                    0.8.14       01 Jan 2013  http://ftp.gnome.org/pub/gnome/sources/libIDL/0.8/libIDL-0.8.14.tar.bz2
libidn                    1.41         27 Jun 2022  https://ftp.gnu.org/pub/gnu/libidn/libidn-1.41.tar.gz
libidn2                   2.3.4        24 Oct 2022  https://ftp.gnu.org/pub/gnu/libidn/libidn2-2.3.4.tar.gz
libimobiledevice          1.3.0        10 Sep 2022  https://github.com/libimobiledevice/libimobiledevice/releases/download/1.3.0/libimobiledevice-1.3.0.tar.bz2
libinput                  1.27.1       15 Jan 2025  https://gitlab.freedesktop.org/libinput/libinput/-/archive/1.27.1/libinput-1.27.1.tar.gz
libintlperl               1.33         17 Mar 2023  https://cpan.metacpan.org/authors/id/G/GU/GUIDO/libintl-perl-1.33.tar.gz
libiodbc                  3.52.15      22 Feb 2023  https://downloads.sourceforge.net/iodbc/libiodbc-3.52.15.tar.gz
libisoburn                1.5.2        29 Oct 2019  http://files.libburnia-project.org/releases/libisoburn-1.5.2.tar.gz
libisofs                  1.5.4        11 Apr 2023  http://files.libburnia-project.org/releases/libisofs-1.5.4.tar.gz
libjpegturbo              3.0.4        16 Sep 2024  https://github.com/libjpeg-turbo/libjpeg-turbo/releases/download/3.0.4/libjpeg-turbo-3.0.4.tar.gz
libjxl                    0.11.0       21 Jan 2025  https://github.com/libjxl/libjxl/archive/v0.11.0/libjxl-0.11.0.tar.gz
libkcddb                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkcddb-23.08.5.tar.xz
libkcompactdisc           23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkcompactdisc-23.08.5.tar.xz
libkdcraw                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkdcraw-23.08.5.tar.xz
libkdegames               23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkdegames-23.08.5.tar.xz
libkdepim                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkdepim-23.08.5.tar.xz
libkeduvocdocument        23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkeduvocdocument-23.08.5.tar.xz
libkexiv2                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkexiv2-23.08.5.tar.xz
libkface                  17.08.1      01 Jan 2018  https://download.kde.org/stable/applications/17.08.1/src/libkface-17.08.1.tar.xz
libkgapi                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkgapi-23.08.5.tar.xz
libkgeomap                20.08.3      05 Nov 2020  https://download.kde.org/stable/release-service/20.08.3/src/libkgeomap-20.08.3.tar.xz
libkipi                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkipi-23.08.5.tar.xz
libkleo                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkleo-23.08.5.tar.xz
libkmahjongg              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkmahjongg-23.08.5.tar.xz
libkomparediff2           23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libkomparediff2-23.08.5.tar.xz
libksane                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libksane-23.08.5.tar.xz
libksba                   1.6.7        24 Jun 2024  https://www.gnupg.org/ftp/gcrypt/libksba/libksba-1.6.7.tar.bz2
libkscreen                6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/libkscreen-6.2.5.tar.xz
libksieve                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libksieve-23.08.5.tar.xz
libksysguard              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/libksysguard-6.2.5.tar.xz
libktorrent               23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/libktorrent-23.08.5.tar.xz
liblbxutil                1.1.0        01 Jun 2014  https://xorg.freedesktop.org/releases/individual/lib/liblbxutil-1.1.0.tar.bz2
liblinear                 246          03 Mar 2023  https://github.com/cjlin1/liblinear/archive/v246/liblinear-246.tar.gz
liblinebreak              2.1          18 Jan 2018  https://sourceforge.net/projects/vimgadgets/files/liblinebreak/2.1/liblinebreak-2.1.tar.gz
liblo                     0.31         26 May 2022  https://sourceforge.net/projects/liblo/files/liblo/0.31/liblo-0.31.tar.gz
liblrdf                   0.4.0        01 Dec 2012  https://sourceforge.net/projects/lrdf/files/liblrdf/0.4.0/liblrdf-0.4.0.tar.gz
liblxqt                   1.4.0        02 Feb 2024  https://github.com/lxqt/liblxqt/releases/download/1.4.0/liblxqt-1.4.0.tar.xz
libmad                    0.15.1b      01 Sep 2013  ftp://ftp.mars.org/pub/mpeg/libmad-0.15.1b.tar.gz
libmanette                0.2.6        30 Nov 2020  https://download.gnome.org/sources/libmanette/0.2/libmanette-0.2.6.tar.xz
libmatekbd                1.28.0       13 Feb 2024  https://pub.mate-desktop.org/releases/1.28/libmatekbd-1.28.0.tar.xz
libmatemixer              1.28.0       13 Feb 2024  https://pub.mate-desktop.org/releases/1.28/libmatemixer-1.28.0.tar.xz
libmateweather            1.28.0       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/libmateweather-1.28.0.tar.xz
libmatheval               1.1.8        01 Jun 2012  http://ftp.gnu.org/gnu/libmatheval/libmatheval-1.1.8.tar.gz
release                   1.6.3        08 Mar 2021  https://github.com/Matroska-Org/libmatroska/archive/release-1.6.3.tar.gz
libmbim                   1.26.4       29 Apr 2022  https://www.freedesktop.org/software/libmbim/libmbim-1.26.4.tar.xz
libmcs                    0.7.2        12 Dec 2015  https://distfiles.atheme.org/libmcs-0.7.2.tgz
libmd                     0.3          21 Jul 2016  ftp://ftp.penguin.cz/pub/users/mhi/libmd/libmd-0.3.tar.bz2
libmediaart               1.9.6        03 Jun 2022  https://download.gnome.org/sources/libmediaart/1.9/libmediaart-1.9.6.tar.xz
libmemcached              0.22         01 Jun 2014  http://download.tangent.org/libmemcached-0.22.tar.gz
libmicrohttpd             0.9.77       29 May 2023  https://ftp.gnu.org/pub/gnu/libmicrohttpd/libmicrohttpd-0.9.77.tar.gz
libmikmod                 3.1.12       01 Jun 2014  https://sourceforge.net/projects/mikmod/files/libmikmod%20%28source%29/3.1.12/libmikmod-3.1.12.tar.gz
libmirage                 1.5.0        01 Jun 2014  http://downloads.sourceforge.net/cdemu/libmirage-1.5.0.tar.bz2
libmng                    2.0.3        22 Oct 2015  http://sourceforge.net/projects/libmng/files/libmng-devel/2.0.3/libmng-2.0.3.tar.xz
libmnl                    1.0.5        07 Apr 2022  https://netfilter.org/projects/libmnl/files/libmnl-1.0.5.tar.bz2
libmodplug                0.8.9.0      25 Sep 2019  https://sourceforge.net/projects/modplug-xmms/files/libmodplug/0.8.9.0/libmodplug-0.8.9.0.tar.gz
v                         2.1.3        09 Mar 2017  https://github.com/atheme/libmowgli-2/archive/v2.1.3.tar.gz
libmp3splt                0.7.2        01 Jun 2014  http://downloads.sourceforge.net/project/mp3splt/libmp3splt/0.7.2/libmp3splt-0.7.2.tar.gz
libmpdclient              2.16         23 Jul 2019  https://www.musicpd.org/download/libmpdclient/2/libmpdclient-2.16.tar.xz
libmpeg2                  0.5.1        01 Feb 2013  http://libmpeg2.sourceforge.net/files/libmpeg2-0.5.1.tar.gz
libmpeg3                  1.8          01 Feb 2013  http://downloads.sourceforge.net/heroines/libmpeg3-1.8.tar.bz2
libmspack                 0.0.20040308alpha 01 Jun 2014  http://www.kyz.uklinux.net/downloads/libmspack-0.0.20040308alpha.tar.gz
libmtp                    1.1.21       26 Apr 2023  https://sourceforge.net/projects/libmtp/files/libmtp/1.1.21/libmtp-1.1.21.tar.gz
libmusicbrainz            5.1.0        26 Nov 2018  https://github.com/metabrainz/libmusicbrainz/releases/download/release-5.1.0/libmusicbrainz-5.1.0.tar.gz
libmxp                    0.2.2        01 Jun 2014  http://www.kmuddy.net/libmxp/files/libmxp-0.2.2.tar.gz
libmypaint                1.6.1        13 May 2020  https://github.com/mypaint/libmypaint/releases/download/v1.6.1/libmypaint-1.6.1.tar.xz
libndp                    1.9          30 Jun 2024  http://libndp.org/files/libndp-1.9.tar.gz
libnet                    1.1.6        22 Feb 2019  https://sourceforge.net/projects/libnet-dev/files/libnet-1.1.6.tar.gz
libnftnl                  1.2.4        12 Nov 2022  https://netfilter.org/projects/libnftnl/files/libnftnl-1.2.4.tar.bz2
libnice                   0.1.19       04 Mar 2024  http://nice.freedesktop.org/releases/libnice-0.1.19.tar.gz
libnids                   1.24         01 May 2013  https://sourceforge.net/projects/libnids/files/libnids/1.24/libnids-1.24.tar.gz
libnih                    1.0.3        01 Jan 2013  https://launchpad.net/libnih/1.0/1.0.3/+download/libnih-1.0.3.tar.gz
libnl                     3.10.0       20 Jul 2024  https://github.com/thom311/libnl/releases/download/libnl3_10_0/libnl-3.10.0.tar.gz
libnma                    1.10.6       11 Jan 2023  https://download.gnome.org/sources/libnma/1.10/libnma-1.10.6.tar.xz
libnotify                 0.8.3        14 Oct 2023  https://download.gnome.org/sources/libnotify/0.8/libnotify-0.8.3.tar.xz
libnsl                    2.0.1        19 Oct 2023  https://github.com/thkukuk/libnsl/releases/download/v2.0.1/libnsl-2.0.1.tar.xz
libnvme                   1.11         02 Nov 2024  https://github.com/linux-nvme/libnvme/archive/v1.11/libnvme-1.11.tar.gz
libnxml                   0.18.3       31 Oct 2017  http://www.autistici.org/bakunin/libnxml/libnxml-0.18.3.tar.gz
liboauth                  1.0.3        19 Apr 2015  http://sourceforge.net/projects/liboauth/files/liboauth-1.0.3.tar.gz
libogg                    1.3.5        08 Jun 2021  http://downloads.xiph.org/releases/ogg/libogg-1.3.5.tar.xz
liboggz                   1.1.1        01 Jun 2014  http://downloads.xiph.org/releases/liboggz/liboggz-1.1.1.tar.gz
liboil                    0.3.17       23 Aug 2017  https://liboil.freedesktop.org/download/liboil-0.3.17.tar.gz
liboobs                   3.0.0        20 Mar 2015  http://ftp.gnome.org/pub/GNOME/sources/liboobs/3.0/liboobs-3.0.0.tar.bz2
htop                      2.0.2        26 Sep 2018  http://hisham.hm/htop/releases/2.0.2/htop-2.0.2.tar.gz
libopenmodeller           1.5.0        20 Sep 2017  http://sourceforge.net/projects/openmodeller/files/openModeller/1.5.0/libopenmodeller-1.5.0.zip
libopenraw                0.3.0        10 Apr 2022  https://libopenraw.freedesktop.org/download/libopenraw-0.3.0.tar.bz2
libopusenc                0.2.1        20 Oct 2018  https://archive.mozilla.org/pub/opus/libopusenc-0.2.1.tar.gz
libosinfo                 1.10.0       01 Apr 2023  https://releases.pagure.org/libosinfo/libosinfo-1.10.0.tar.xz
libosip2                  5.3.1        12 Oct 2022  https://ftp.gnu.org/pub/gnu/osip/libosip2-5.3.1.tar.gz
v                         2.13.0       16 Aug 2017  https://github.com/osmcode/libosmium/archive/v2.13.0.tar.gz
libostree                 2022.5       23 Aug 2022  https://github.com/ostreedev/ostree/releases/download/v2022.5/libostree-2022.5.tar.xz
libpaper                  2.2.5        12 Mar 2024  https://github.com/rrthomas/libpaper/releases/download/v2.2.5/libpaper-2.2.5.tar.gz
libpcap                   1.10.5       04 Sep 2024  https://www.tcpdump.org/release/libpcap-1.10.5.tar.gz
libpciaccess              0.18.1       24 Mar 2024  https://xorg.freedesktop.org/releases/individual/lib/libpciaccess-0.18.1.tar.xz
libpeas                   2.0.5        18 Jan 2025  https://download.gnome.org/sources/libpeas/2.0/libpeas-2.0.5.tar.xz
libpipeline               1.5.7        21 Jan 2024  https://download.savannah.gnu.org/releases/libpipeline/libpipeline-1.5.7.tar.gz
libplacebo                07.07.2024   07 Jul 2024  https://github.com/haasn/libplacebo/releases/tag/libplacebo-07.07.2024.tar.xz
libplasma                 6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/libplasma-6.2.5.tar.xz
libplist                  2.6.0        07 Jul 2024  https://github.com/libimobiledevice/libplist/releases/download/2.6.0/libplist-2.6.0.tar.bz2
libpng                    1.6.44       27 Sep 2024  https://sourceforge.net/projects/libpng/files/libpng16/1.6.44/libpng-1.6.44.tar.xz
libpod                    1.1.1        02 Mar 2019  https://github.com/containers/libpod/archive/libpod-1.1.1.tar.gz
libportal                 0.7.1        11 Apr 2024  https://github.com/flatpak/libportal/releases/download/0.7.1/libportal-0.7.1.tar.xz
libpsl                    0.21.5       20 Feb 2024  https://github.com/rockdaboot/libpsl/releases/download/0.21.5/libpsl-0.21.5.tar.gz
libpthreadstubs           0.5          10 Aug 2023  https://www.x.org/releases/individual/lib/libpthread-stubs-0.5.tar.xz
libptytty                 2.0          17 Jun 2022  http://dist.schmorp.de/libptytty/libptytty-2.0.tar.gz
libpwquality              1.4.5        21 Nov 2022  https://github.com/libpwquality/libpwquality/releases/download/libpwquality-1.4.5/libpwquality-1.4.5.tar.bz2
libqalculate              5.5.0        16 Jan 2025  https://github.com/Qalculate/libqalculate/releases/download/v5.5.0/libqalculate-5.5.0.tar.gz
libqglviewer              2.3.17       01 Jun 2014  http://www.libqglviewer.com/src/libQGLViewer-2.3.17.tar.gz
libqmi                    1.30.8       25 Jun 2022  https://www.freedesktop.org/software/libqmi/libqmi-1.30.8.tar.xz
libqtxdg                  3.9.1        15 Jul 2022  https://github.com/lxqt/libqtxdg/releases/download/3.9.1/libqtxdg-3.9.1.tar.xz
libquicktime              1.2.4        01 Jun 2014  https://sourceforge.net/projects/libquicktime/files/libquicktime/1.2.4/libquicktime-1.2.4.tar.gz
libquvi                   0.9.4        09 Jun 2016  https://sourceforge.net/projects/quvi/files/0.9/libquvi/libquvi-0.9.4.tar.xz
libquviscripts            0.9.20131130 09 Jun 2016  https://sourceforge.net/projects/quvi/files/0.9/libquvi-scripts/libquvi-scripts-0.9.20131130.tar.xz
libraw                    0.21.2       27 Feb 2024  https://www.libraw.org/data/LibRaw-0.21.2.tar.gz
libraw1394                2.0.5        01 Jun 2014  http://www.linux1394.org/dl/libraw1394-2.0.5.tar.gz
libredwg                  0.12.4       09 Apr 2021  https://ftp.gnu.org/pub/gnu/libredwg/libredwg-0.12.4.tar.xz
librejs                   7.18.1       14 Feb 2019  http://ftp.gnu.org/pub/gnu/librejs/librejs-7.18.1.tar.gz
libreoffice               6.3.3.2      15 Nov 2019  http://download.documentfoundation.org/libreoffice/src/6.3.3/libreoffice-6.3.3.2.tar.xz
libreofficebinfilter      3.6.4.1      08 Mar 2015  http://download.documentfoundation.org/libreoffice/src/4.2.2/libreoffice-binfilter-3.6.4.1.tar.xz
libreofficecore           3.6.4.3      06 Mar 2014  http://download.documentfoundation.org/libreoffice/src/4.2.2/libreoffice-core-3.6.4.3.tar.xz
libreofficehelp           6.1.0.3      16 Aug 2018  http://download.documentfoundation.org/libreoffice/src/6.1.0/libreoffice-help-6.1.0.3.tar.xz
librep                    0.92.6       25 Jul 2017  http://download.tuxfamily.org/librep/librep_0.92.6.tar.xz
libressl                  4.0.0        15 Oct 2024  https://ftp.openbsd.org/pub/OpenBSD/LibreSSL/libressl-4.0.0.tar.gz
librevenge                0.0.4        13 Mar 2016  https://sourceforge.net/projects/libwpd/files/librevenge/librevenge-0.0.4/librevenge-0.0.4.tar.xz
librsvg                   2.59.2       06 Nov 2024  https://download.gnome.org/sources/librsvg/2.59/librsvg-2.59.2.tar.xz
librsync                  2.3.2        26 Nov 2021  https://github.com/librsync/librsync/releases/download/v2.3.2/librsync-2.3.2.tar.gz
libsafe                   2.0-16       01 Jun 2014  https://github.com/tagatac/libsafe-CVE-2005-1125/blob/master/libsafe-2.0-16.tgz
libsamplerate             0.2.2        07 Sep 2021  https://github.com/libsndfile/libsamplerate/releases/download/0.2.2/libsamplerate-0.2.2.tar.xz
libsass                   3.6.5        22 May 2021  https://github.com/sass/libsass/archive/3.6.5/libsass-3.6.5.tar.gz
libseccomp                2.5.5        03 Dec 2023  https://github.com/seccomp/libseccomp/releases/download/v2.5.5/libseccomp-2.5.5.tar.gz
libsecret                 0.21.6       16 Jan 2025  https://download.gnome.org/sources/libsecret/0.21/libsecret-0.21.6.tar.xz
libselinux                3.7          26 Jun 2024  https://github.com/SELinuxProject/selinux/archive/libselinux-3.7.tar.gz
libsexy                   0.1.11       01 Jun 2014  http://releases.chipx86.com/libsexy/libsexy/libsexy-0.1.11.tar.gz
libshout                  2.4.6        22 Sep 2022  https://downloads.xiph.org/releases/libshout/libshout-2.4.6.tar.gz
libsigc++                 3.6.0        30 Oct 2023  https://download.gnome.org/sources/libsigc++/3.6/libsigc++-3.6.0.tar.xz
libsigsegv                2.14         07 Jan 2022  https://ftp.gnu.org/pub/gnu/libsigsegv/libsigsegv-2.14.tar.gz
foobar                    1.0.0        15 Mar 2024  https://download.kde.org/stable/foobar-1.0.0.tar.xz
libslab                   2.30.0       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/libslab/2.30/libslab-2.30.0.tar.gz
libsm                     1.2.4        08 Jan 2023  https://www.x.org/releases/individual/lib/libSM-1.2.4.tar.xz
libsndfile                1.2.0        18 Aug 2023  https://github.com/libsndfile/libsndfile/releases/download/1.2.0/libsndfile-1.2.0.tar.xz
libsodium                 1.0.20       27 May 2024  https://github.com/jedisct1/libsodium/releases/download/1.0.20-RELEASE/libsodium-1.0.20.tar.gz
libsoup                   3.6.4        17 Jan 2025  https://download.gnome.org/sources/libsoup/3.6/libsoup-3.6.4.tar.xz
libspectre                0.2.12       11 Mar 2024  http://libspectre.freedesktop.org/releases/libspectre-0.2.12.tar.gz
libspnav                  0.2.3        04 Jun 2016  https://sourceforge.net/projects/spacenav/files/spacenav%20library%20%28SDK%29/libspnav%200.2.3/libspnav-0.2.3.tar.gz
libspng                   0.4.5        31 Jul 2019  https://gitlab.com/randy408/libspng/uploads/6ddcaa59367b2cea474213a994b82012/libspng-0.4.5.tar.xz
v                         2.2          22 Sep 2018  https://github.com/cisco/libsrtp/archive/v2.2.tar.gz
libssh                    0.11.0       12 Aug 2024  https://www.libssh.org/files/0.11/libssh-0.11.0.tar.xz
libssh2                   1.11.1       18 Oct 2024  https://www.libssh2.org/download/libssh2-1.11.1.tar.gz
libstatgrab               0.92         26 Aug 2020  http://www.mirrorservice.org/sites/ftp.i-scream.org/pub/i-scream/libstatgrab/libstatgrab-0.92.tar.gz
libsysstat                0.4.6        14 Feb 2022  https://github.com/lxqt/libsysstat/releases/download/0.4.6/libsysstat-0.4.6.tar.xz
libtar                    1.2.11       01 Jun 2014  https://github.com/mdippery/libtar/libtar-1.2.11.tar.gz
libtasn1                  4.19.0       26 Aug 2022  https://ftp.gnu.org/pub/gnu/libtasn1/libtasn1-4.19.0.tar.gz
libthai                   0.1.29       18 Jan 2025  https://github.com/tlwg/libthai/releases/download/v0.1.29/libthai-0.1.29.tar.xz
libtheora                 05.04.2024   12 Aug 2017  https://downloads.xiph.org/releases/theora/libtheora-05.04.2024.tar.xz
libtirpc                  1.3.6        18 Oct 2024  https://downloads.sourceforge.net/libtirpc/libtirpc-1.3.6.tar.bz2
libtool                   2.5.4        22 Nov 2024  https://ftp.gnu.org/gnu/libtool/libtool-2.5.4.tar.gz
libtorrent                0.13.7       30 May 2019  https://github.com/rakshasa/rtorrent/releases/download/v0.9.7/libtorrent-0.13.7.tar.gz
libtorrentrasterbar       2.0.10       30 Sep 2024  https://github.com/arvidn/libtorrent/releases/download/v2.0.10/libtorrent-rasterbar-2.0.10.tar.gz
libtrace                  3.0.14       01 Jun 2014  http://research.wand.net.nz/software/libtrace/libtrace-3.0.14.tar.bz2
libtranslate              0.99         01 Jun 2014  http://savannah.nongnu.org/download/libtranslate/libtranslate-0.99.tar.gz
libui                     0.0.15       12 Mar 2022  https://rubygems.org/downloads/libui-0.0.15.gem
libunibreak               3.0          30 Aug 2016  https://github.com/adah1972/libunibreak/releases/download/libunibreak_3_0/libunibreak-3.0.tar.gz
libunicap                 0.9.12       30 Dec 2015  http://unicap-imaging.org/downloads/libunicap-0.9.12.tar.gz
libuninameslistdist       20200413     15 Jun 2020  https://github.com/fontforge/libuninameslist/releases/download/20200413/libuninameslist-dist-20200413.tar.gz
libunique                 3.0.2        01 Aug 2013  http://ftp.acc.umu.se/pub/GNOME/sources/libunique/3.0/libunique-3.0.2.tar.xz
libunistring              1.3          18 Oct 2024  https://ftp.gnu.org/pub/gnu/libunistring/libunistring-1.3.tar.xz
libunwind                 1.8.1        01 Jul 2024  https://github.com/libunwind/libunwind/releases/download/v1.8.1/libunwind-1.8.1.tar.gz
libupnp                   1.6.21       17 Apr 2017  https://sourceforge.net/projects/pupnp/files/pupnp/libUPnP%201.6.21/libupnp-1.6.21.tar.bz2
liburing                  02.05.2024   29 Aug 2024  https://github.com/axboe/liburing/liburing-02.05.2024
libusb                    1.0.26       26 Apr 2022  https://github.com/libusb/libusb/releases/download/v1.0.26/libusb-1.0.26.tar.bz2
libusbcompat              0.1.7        18 May 2021  https://github.com/libusb/libusb-compat-0.1/releases/download/v0.1.7/libusb-compat-0.1.7.tar.bz2
libusbmuxd                2.0.2        04 Mar 2023  https://github.com/libimobiledevice/libusbmuxd/releases/download/2.0.2/libusbmuxd-2.0.2.tar.bz2
libutempter               1.1.6        24 Feb 2019  ftp://ftp.altlinux.org/pub/people/ldv/utempter/libutempter-1.1.6.tar.bz2
libuv                     v1.50.0      16 Jan 2025  https://dist.libuv.org/dist/v1.50.0/libuv-v1.50.0.tar.gz
libva                     DD.MM.YYYY   16 Jan 2025  https://github.com/intel/libva/archive/2.21.0/libva-DD.MM.YYYY
libvautils                2.20.0       16 Jan 2025  https://github.com/intel/libva-utils/releases/download/2.20.0/libva-utils-2.20.0.tar.bz2
libvdpau                  1.5          12 Feb 2023  https://gitlab.freedesktop.org/vdpau/libvdpau/-/archive/1.5/libvdpau-1.5.tar.bz2
libvdpauvagl              0.3.4        11 Apr 2015  https://github.com/i-rinat/libvdpau-va-gl/releases/download/v0.3.4/libvdpau-va-gl-0.3.4.tar.gz
libviper                  1.4.6        01 Jun 2014  http://sourceforge.net/projects/libviper/files/libviper-1.4.6.tar.gz
libvirt                   11.0.0       15 Jan 2025  https://libvirt.org/sources/libvirt-11.0.0.tar.xz
libvisio                  0.1.8        23 Oct 2024  https://dev-www.libreoffice.org/src/libvisio/libvisio-0.1.8.tar.xz
libvisual                 0.4.0        01 Jan 2013  http://sourceforge.net/projects/libvisual/files/libvisual/libvisual-0.4.0/libvisual-0.4.0.tar.bz2
libvncserver              0.9.13       12 Mar 2021  https://github.com/LibVNC/libvncserver/archive/LibVNCServer-0.9.13.tar.gz
libvorbis                 1.3.7        04 Jul 2020  http://downloads.xiph.org/releases/vorbis/libvorbis-1.3.7.tar.gz
libvpx                    02.11.2024   02 Nov 2024  https://github.com/webmproject/libvpx/archive/v1.15.0/libvpx-02.11.2024.tar.gz
libvterm                  0.1.1        04 Oct 2019  http://www.leonerd.org.uk/code/libvterm/libvterm-0.1.1.tar.gz
libwacom                  2.14.0       15 Jan 2025  https://github.com/linuxwacom/libwacom/releases/download/libwacom-2.14.0/libwacom-2.14.0.tar.xz
libwebp                   1.5.0        15 Jan 2025  https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-1.5.0.tar.gz
libwindowswm              1.0.0        01 Jun 2014  http://ftp.cica.es/pub/X/pub/X11R7.3/src/lib/libWindowsWM-1.0.0.tar.bz2
v                         0.2.13       22 Apr 2023  https://github.com/caolanm/libwmf/archive/refs/tags/v0.2.13.tar.gz
libwnck                   43.2         16 Jan 2025  https://download.gnome.org/sources/libwnck/43/libwnck-43.2.tar.xz
libwpe                    1.14.1       05 Feb 2023  https://wpewebkit.org/releases/libwpe-1.14.1.tar.xz
libwps                    0.4.10       17 Sep 2018  https://sourceforge.net/projects/libwps/files/libwps/libwps-0.4.10/libwps-0.4.10.tar.xz
libwwwperl                6.35         29 Jul 2018  http://search.cpan.org/CPAN/authors/id/O/OA/OALDERS/libwww-perl-6.35.tar.gz
libx11                    1.8.10       30 Jul 2024  https://www.x.org/releases/individual/lib/libX11-1.8.10.tar.xz
libxau                    1.0.12       17 Dec 2024  https://www.x.org/pub/individual/lib/libXau-1.0.12.tar.xz
libxaw                    1.0.16       12 Mar 2024  https://www.x.org/releases/individual/lib/libXaw-1.0.16.tar.xz
libxaw3d                  1.6.6        04 Mar 2024  https://www.x.org/releases/individual/lib/libXaw3d-1.6.6.tar.xz
libxaw3dxft               1.6.2h       06 Oct 2020  https://sourceforge.net/projects/sf-xpaint/files/libxaw3dxft/libXaw3dXft-1.6.2h.tar.bz2
libxcb                    1.17.0       15 Jan 2025  https://xcb.freedesktop.org/dist/libxcb-1.17.0.tar.xz
libxcm                    0.5.3        10 Oct 2015  https://sourceforge.net/projects/oyranos/files/libXcm/libXcm-0.5/libXcm-0.5.3.tar.gz
libxcomposite             0.4.6        05 Dec 2022  https://www.x.org/releases/individual/lib/libXcomposite-0.4.6.tar.xz
libxcrypt                 4.4.36       21 Jan 2024  https://github.com/besser82/libxcrypt/releases/download/v4.4.36/libxcrypt-4.4.36.tar.xz
libxcursor                1.2.3        10 Nov 2024  https://www.x.org/releases/individual/lib/libXcursor-1.2.3.tar.xz
libxcvt                   0.1.3        16 Dec 2024  https://www.x.org/releases/individual/lib/libxcvt-0.1.3.tar.xz
libxdamage                1.1.6        05 Dec 2022  https://www.x.org/releases/individual/lib/libXdamage-1.1.6.tar.xz
libxdmcp                  1.1.5        04 Mar 2024  https://www.x.org/releases/individual/lib/libXdmcp-1.1.5.tar.gz
libxevie                  1.0.3        01 Jun 2014  https://xorg.freedesktop.org/releases/individual/lib/libXevie-1.0.3.tar.bz2
libxext                   1.3.6        12 Feb 2024  https://www.x.org/releases/individual/lib/libXext-1.3.6.tar.xz
libxfce4ui                4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/libxfce4ui/4.20/libxfce4ui-4.20.0.tar.bz2
libxfce4util              4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/libxfce4util/4.20/libxfce4util-4.20.0.tar.bz2
libxfce4windowing         4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/libxfce4windowing/4.20/libxfce4windowing-4.20.0.tar.bz2
libxfixes                 6.0.1        10 Apr 2023  https://www.x.org/releases/individual/lib/libXfixes-6.0.1.tar.xz
libxfont                  1.5.3        22 Oct 2017  https://www.x.org/releases/individual/lib/libXfont-1.5.3.tar.gz
libxfont2                 2.0.7        04 Aug 2024  https://www.x.org/releases/individual/lib/libXfont2-2.0.7.tar.xz
libxfontcache             1.0.4        01 Jun 2014  https://www.x.org/releases/individual/lib/libXfontcache-1.0.4.tar.bz2
libxft                    2.3.8        18 Apr 2023  https://www.x.org/releases/individual/lib/libXft-2.3.8.tar.xz
libxi                     1.8.2        06 Sep 2024  https://www.x.org/releases/individual/lib/libXi-1.8.2.tar.xz
libxinerama               1.1.5        31 Oct 2022  https://www.x.org/releases/individual/lib/libXinerama-1.1.5.tar.xz
libxkbcommon              1.7.0        25 Mar 2024  https://xkbcommon.org/download/libxkbcommon-1.7.0.tar.xz
libxkbfile                1.1.3        12 Feb 2024  https://www.x.org/releases/individual/lib/libxkbfile-1.1.3.tar.xz
libxkbui                  1.0.2        01 Jun 2014  https://www.x.org/releases/individual/lib/libxkbui-1.0.2.tar.bz2
libxklavier               5.3          01 Sep 2013  http://ftp.gnome.org/pub/GNOME/sources/libxklavier/5.3/libxklavier-5.3.tar.xz
libxls                    1.5.1        15 Apr 2019  https://github.com/libxls/libxls/releases/download/v1.5.1/libxls-1.5.1.tar.gz
libxml++                  5.0.3        23 Mar 2023  https://download.gnome.org/sources/libxml++/5.0/libxml++-5.0.3.tar.xz
libxml2                   2.13.5       14 Nov 2024  https://download.gnome.org/sources/libxml2/2.13/libxml2-2.13.5.tar.xz
libxml2python             2.6.15       01 Jan 2013  ftp://xmlsoft.org/libxml2/python/libxml2-python-2.6.15.tar.gz
libxmlb                   0.3.21       18 Oct 2024  https://github.com/hughsie/libxmlb/releases/download/0.3.21/libxmlb-0.3.21.tar.xz
libxmp                    4.6.1        22 Jan 2025  https://sourceforge.net/projects/xmp/files/libxmp/4.6.1/libxmp-4.6.1.tar.gz
libxmu                    1.2.1        18 Apr 2024  https://www.x.org/releases/individual/lib/libXmu-1.2.1.tar.xz
libxo                     1.0.4        15 May 2019  https://github.com/Juniper/libxo/releases/download/1.0.4/libxo-1.0.4.tar.gz
libxp                     1.0.4        13 Sep 2022  https://www.x.org/releases/individual/lib/libXp-1.0.4.tar.xz
libxpm                    3.5.16       18 Apr 2023  https://www.x.org/releases/individual/lib/libXpm-3.5.16.tar.xz
libxpresent               1.0.1        19 Oct 2022  https://www.x.org/releases/individual/lib/libXpresent-1.0.1.tar.xz
libxprintapputil          1.0.1        13 Sep 2018  https://www.x.org/releases/individual/lib/libXprintAppUtil-1.0.1.tar.gz
libxprintutil             1.0.1        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/lib/libXprintUtil-1.0.1.tar.bz2
libxrandr                 1.5.4        18 Nov 2023  https://www.x.org/releases/individual/lib/libXrandr-1.5.4.tar.xz
libxrender                0.9.11       24 Oct 2022  https://www.x.org/releases/individual/lib/libXrender-0.9.11.tar.gz
libxres                   1.2.2        05 Dec 2022  https://www.x.org/releases/individual/lib/libXres-1.2.2.tar.gz
libxscrnsaver             1.2.4        05 Dec 2022  https://www.x.org/releases/individual/lib/libXScrnSaver-1.2.4.tar.gz
libxshmfence              1.3.3        15 Dec 2024  https://www.x.org/releases/individual/lib/libxshmfence-1.3.3.tar.xz
libxslt                   1.1.42       07 Jul 2024  https://download.gnome.org/sources/libxslt/1.1/libxslt-1.1.42.tar.xz
libxspf                   1.2.0        16 Feb 2017  http://downloads.xiph.org/releases/xspf/libxspf-1.2.0.tar.bz2
libxt                     1.3.1        20 Nov 2024  https://www.x.org/releases/individual/lib/libXt-1.3.1.tar.xz
libxtst                   1.2.5        04 Aug 2024  https://www.x.org/releases/individual/lib/libXtst-1.2.5.tar.xz
libxv                     1.0.13       15 Dec 2024  https://www.x.org/releases/individual/lib/libXv-1.0.13.tar.xz
libxvmc                   1.0.14       12 Feb 2024  https://www.x.org/releases/individual/lib/libXvMC-1.0.14.tar.xz
libxxf86dga               1.1.6        05 Dec 2022  https://www.x.org/releases/individual/lib/libXxf86dga-1.1.6.tar.xz
libxxf86misc              1.0.4        06 Jul 2018  https://www.x.org/releases/individual/lib/libXxf86misc-1.0.4.tar.bz2
libxxf86vm                1.1.6        15 Dec 2024  https://www.x.org/releases/individual/lib/libXxf86vm-1.1.6.tar.xz
libyuv                    15.03.2024   14 Jul 2024  https://chromium.googlesource.com/libyuv/libyuv/libyuv-15.03.2024
libzapojit                0.0.3        17 Apr 2015  http://ftp.gnome.org/pub/GNOME/sources/libzapojit/0.0/libzapojit-0.0.3.tar.xz
libzeitgeist              0.3.18       01 Jan 2014  https://launchpad.net/libzeitgeist/0.3/0.3.18/+download/libzeitgeist-0.3.18.tar.gz
libzip                    1.11.3       22 Jan 2025  sorchttps://libzip.org/download/libzip-1.11.3.tar.xz
libzrtpcpp                2.0.0        01 Jun 2014  http://ftp.gnu.org/gnu/ccrtp/libzrtpcpp-2.0.0.tar.gz
licq                      1.8.2        09 Jul 2015  http://downloads.sourceforge.net/licq/licq-1.8.2.tar.bz2
liferea                   1.12.6b      10 Dec 2018  https://github.com/lwindolf/liferea/releases/download/v1.12.6/liferea-1.12.6b.tar.bz2
lightdm                   1.30.0       09 Jul 2019  https://github.com/CanonicalLtd/lightdm/releases/download/1.30.0/lightdm-1.30.0.tar.xz
lightning                 2.2.2        29 Apr 2023  https://ftp.gnu.org/pub/gnu/lightning/lightning-2.2.2.tar.gz
lightsoff                 46.0         16 Mar 2024  https://download.gnome.org/sources/lightsoff/46/lightsoff-46.0.tar.xz
lighttpd                  1.4.77       10 Jan 2025  https://download.lighttpd.net/lighttpd/releases-1.4.x/lighttpd-1.4.77.tar.xz
lilo                      24.2         23 Nov 2015  http://lilo.alioth.debian.org/ftp/sources/lilo-24.2.tar.gz
lilykde                   0.6.1        01 Jun 2014  http://lilykde.googlecode.com/files/lilykde-0.6.1.tar.gz
lilypond                  2.20.0       07 Oct 2020  https://lilypond.org/download/sources/v2.20/lilypond-2.20.0.tar.gz
lilyterm                  0.9.9.4      01 Jul 2013  http://lilyterm.luna.com.tw/file/lilyterm-0.9.9.4.tar.gz
linc                      1.0.3        03 Mar 2015  http://hisham.hm/htop/releases/1.0.3/linc-1.0.3.tar.gz
lincityng                 2.0          01 Jan 2013  http://download.berlios.de/lincity-ng/lincity-ng-2.0.tar.bz2
lineakconfig              0.3.2        01 Jun 2014  http://prdownloads.sourceforge.net/lineak/lineakconfig-0.3.2.tar.gz
lineakd                   0.9          01 May 2014  http://prdownloads.sourceforge.net/lineak/lineakd-0.9.tar.gz
linkgrammar               5.2.5        28 Sep 2015  http://www.abiword.org/downloads/link-grammar/5.2.5/link-grammar-5.2.5.tar.gz
links                     2.30         29 Jul 2024  http://links.twibright.com/download/links-2.30.tar.bz2
2                         07_02        01 Jun 2014  https://sourceforge.net/projects/linuxlibertine/files/linuxlibertine/5.3.0/LinLibertineSRC_5.3.0_2012_07_02.tgz
linux                     6.13         20 Jan 2025  https://www.kernel.org/pub/linux/kernel/v6.x/linux-6.13.tar.xz
linuxlive                 6.2.4        01 Jun 2014  ftp://ftp.slax.org/Linux-Live/linux-live-6.2.4.tar.gz
linuxpam                  1.7.0        16 Jan 2025  https://github.com/linux-pam/linux-pam/releases/download/v1.7.0/Linux-PAM-1.7.0.tar.xz
liquidwar                 5.6.5        23 Nov 2019  http://www.ufoot.org/download/liquidwar/v5/5.6.5/liquidwar-5.6.5.tar.gz
listen                    3.7.1        27 May 2022  https://rubygems.org/downloads/listen-3.7.1.gem
listmoreutils             0.428        17 Dec 2017  https://www.cpan.org/authors/id/R/RE/REHSACK/List-MoreUtils-0.428.tar.gz
listres                   1.0.6        04 Mar 2024  https://www.x.org/releases/individual/app/listres-1.0.6.tar.xz
littleutils               1.0.37       18 Jul 2017  https://sourceforge.net/projects/littleutils/files/littleutils-source/1.0.37/littleutils-1.0.37.tar.xz
lives                     3.0.2        28 Sep 2019  https://sourceforge.net/projects/lives/files/lives/3.0.2/lives-3.0.2.tar.bz2
liveslak                  1.1.9.1      01 Oct 2017  http://bear.alienbase.nl/cgit/liveslak/snapshot/liveslak-1.1.9.1.tar.xz
lksctptools               1.0.17       14 Oct 2018  https://sourceforge.net/projects/lksctp/files/lksctp-tools/lksctp-tools-1.0.17.tar.gz
llvm                      19.1.7       17 Jan 2025  https://github.com/llvm/llvm-project/releases/download/llvmorg-19.1.7/llvm-19.1.7.tar.xz
lmarbles                  1.0.8        26 Mar 2016  https://sourceforge.net/projects/lgames/files/lmarbles/lmarbles-1.0.8.tar.gz
lmsensors36               0            08 Dec 2019  https://github.com/lm-sensors/lm-sensors/archive/V3-6-0/lm-sensors-3-6-0.tar.gz
lndir                     1.0.5        25 Mar 2024  https://www.x.org/releases/individual/util/lndir-1.0.5.tar.xz
log4cplus                 2.1.1        18 Mar 2024  https://github.com/log4cplus/log4cplus/releases/download/REL_2_1_1/log4cplus-2.1.1.tar.xz
log4cpp                   1.1.2rc1     08 Dec 2015  https://sourceforge.net/projects/log4cpp/files/log4cpp-1.1.x%20%28new%29/log4cpp-1.1/log4cpp-1.1.2rc1.tar.gz
log4r                     1.1.10       25 Nov 2019  https://rubygems.org/downloads/log4r-1.1.10.gem
logrotate                 3.18.1       22 May 2021  https://github.com/logrotate/logrotate/releases/download/3.18.1/logrotate-3.18.1.tar.xz
lokalize                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/lokalize-23.08.5.tar.xz
lollypop                  1.2.20       23 Jan 2020  https://adishatz.org/lollypop/lollypop-1.2.20.tar.xz
longomatch                1.0.2        28 Feb 2015  http://ftp.gnome.org/pub/GNOME/sources/longomatch/1.0/longomatch-1.0.2.tar.xz
loofah                    2.18.0       27 May 2022  https://rubygems.org/downloads/loofah-2.18.0.gem
loopaes                   v3.7d        25 Jul 2015  http://sourceforge.net/projects/loop-aes/files/loop-aes/v3.7d/loop-AES-v3.7d.tar.bz2
lordsawar                 0.2.0        01 Jun 2014  http://download.savannah.gnu.org/releases-noredirect/lordsawar/lordsawar-0.2.0.tar.gz
loudmouth                 1.5.4        27 Jan 2021  https://github.com/mcabber/loudmouth/releases/download/1.5.4/loudmouth-1.5.4.tar.bz2
lpairs                    1.0.5        12 May 2019  http://prdownloads.sourceforge.net/lgames/lpairs-1.0.5.tar.gz
lpairs2                   2.1          15 Jul 2019  https://sourceforge.net/projects/lgames/files/lpairs2/lpairs2-2.1.tar.gz
lrzip                     0.650        02 Mar 2022  http://ck.kolivas.org/apps/lrzip/lrzip-0.650.tar.xz
lrzsz                     0.12.20      13 Jul 2017  https://ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz
lshw                      B.02.20      12 Jan 2025  https://www.ezix.org/software/files/lshw-B.02.20.tar.gz
lshwd                     05.04.2024   01 Jun 2014  https://github.com/bbidulock/lshwd/lshwd-05.04.2024.tar.xz
lskat                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/lskat-23.08.5.tar.xz
lsof                      4.95.0.linux 31 Oct 2015  https://github.com/lsof-org/lsof/releases/download/4.95.0/lsof_4.95.0.linux.tar.bz2
lsscsi                    0.31         23 Feb 2020  http://sg.danny.cz/scsi/lsscsi-0.31.tgz
ltris                     1.2.3        02 Apr 2022  http://prdownloads.sourceforge.net/lgames/ltris-1.2.3.tar.gz
ltris2                    2.0.2        21 Nov 2024  https://sourceforge.net/projects/lgames/files/ltris/ltris2-2.0.2.tar.gz
lua                       5.4.7        25 Jun 2024  https://www.lua.org/ftp/lua-5.4.7.tar.gz
luajit                    2.0.5        09 Oct 2017  http://luajit.org/download/LuaJIT-2.0.5.tar.gz
luarocks                  3.5.0        28 Mar 2021  https://luarocks.org/releases/luarocks-3.5.0.tar.gz
luit                      1.1.1        01 Nov 2012  https://www.x.org/releases/individual/app/luit-1.1.1.tar.bz2
lumina                    1.5.0        30 Apr 2019  https://github.com/lumina-desktop/lumina/archive/lumina-1.5.0.tar.gz
lxappearance              0.6.3        06 Oct 2017  https://sourceforge.net/projects/lxde/files/LXAppearance/lxappearance-0.6.3.tar.xz
lxc                       4.0.10       05 Aug 2021  https://linuxcontainers.org/downloads/lxc/lxc-4.0.10.tar.gz
lxdecommon                0.99.2       24 Feb 2017  https://sourceforge.net/projects/lxde/files/lxde-common%20%28default%20config%29/lxde-common%200.99/lxde-common-0.99.2.tar.xz
lxdm                      0.5.3        24 Feb 2017  https://sourceforge.net/projects/lxde/files/lxdm/lxdm%200.5.3/lxdm-0.5.3.tar.xz
lximageqt                 0.16.0       07 Nov 2020  https://github.com/lxqt/lximage-qt/releases/download/0.16.0/lximage-qt-0.16.0.tar.xz
lxml                      5.3.0        20 Sep 2024  https://files.pythonhosted.org/packages/e7/6b/20c3a4b24751377aaa6307eb230b66701024012c29dd374999cc92983269/lxml-5.3.0.tar.gz
lxpanel                   0.10.1       07 Feb 2021  http://downloads.sourceforge.net/lxde/lxpanel-0.10.1.tar.xz
lxqtabout                 1.1.0        30 Aug 2022  https://github.com/lxqt/lxqt-about/releases/download/1.1.0/lxqt-about-1.1.0.tar.xz
lxqtadmin                 1.1.0        30 Aug 2022  https://github.com/lxqt/lxqt-admin/releases/download/1.1.0/lxqt-admin-1.1.0.tar.xz
lxqtarchiver              0.9.1        25 Feb 2024  https://github.com/lxqt/lxqt-archiver/releases/download/0.9.1/lxqt-archiver-0.9.1.tar.xz
lxqtbuildtools            0.13.0       02 Feb 2024  https://github.com/lxqt/lxqt-build-tools/releases/download/0.13.0/lxqt-build-tools-0.13.0.tar.xz
lxqtconfig                1.1.0        12 Sep 2022  https://github.com/lxqt/lxqt-config/releases/download/1.1.0/lxqt-config-1.1.0.tar.xz
lxqtglobalkeys            0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-globalkeys/releases/download/0.16.0/lxqt-globalkeys-0.16.0.tar.xz
lxqtl10n                  0.13.0       17 Jan 2019  https://downloads.lxqt.org/downloads/lxqt-l10n/0.13.0/lxqt-l10n-0.13.0.tar.xz
lxqtnotificationd         0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-notificationd/releases/download/0.16.0/lxqt-notificationd-0.16.0.tar.xz
lxqtopensshaskpass        0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-openssh-askpass/releases/download/0.16.0/lxqt-openssh-askpass-0.16.0.tar.xz
lxqtpanel                 2.1.4        12 Jan 2025  https://github.com/lxde/lxqt-panel/releases/download/2.1.4/lxqt-panel-2.1.4.tar.xz
lxqtpolicykit             0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-policykit/releases/download/0.16.0/lxqt-policykit-0.16.0.tar.xz
lxqtpowermanagement       0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-powermanagement/releases/download/0.16.0/lxqt-powermanagement-0.16.0.tar.xz
lxqtqtplugin              0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-qtplugin/releases/download/0.16.0/lxqt-qtplugin-0.16.0.tar.xz
lxqtrunner                0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-runner/releases/download/0.16.0/lxqt-runner-0.16.0.tar.xz
lxqtsession               0.17.1       16 Apr 2021  https://github.com/lxqt/lxqt-session/releases/download/0.17.1/lxqt-session-0.17.1.tar.xz
lxqtsudo                  0.16.0       07 Nov 2020  https://github.com/lxqt/lxqt-sudo/releases/download/0.16.0/lxqt-sudo-0.16.0.tar.xz
lxqtthemes                1.1.0        30 Aug 2022  https://github.com/lxqt/lxqt-themes/releases/download/1.1.0/lxqt-themes-1.1.0.tar.xz
lxsession                 0.5.5        02 Mar 2020  https://downloads.sourceforge.net/lxde/lxsession-0.5.5.tar.xz
lxterminal                0.4.0        07 Feb 2021  https://sourceforge.net/projects/lxde/files/LXTerminal%20%28terminal%20emulator%29/LXTerminal%200.4.x/lxterminal-0.4.0.tar.xz
lyntin                    4.2          01 Jun 2014  https://sourceforge.net/projects/lyntin/files/lyntin/4.2/lyntin-4.2.tar.gz
lynx                      2.9.0        17 Mar 2024  https://invisible-mirror.net/archives/lynx/tarballs/lynx2.9.0.tar.bz2
lyx                       2.4.3        18 Jan 2025  http://ftp.lyx.org/pub/lyx/stable/2.4.x/lyx-2.4.3.tar.xz
lz4                       1.10.0       08 Sep 2024  https://github.com/lz4/lz4/releases/download/v1.10.0/lz4-1.10.0.tar.gz
lzip                      1.25         18 Jan 2025  https://download.savannah.gnu.org/releases/lzip/lzip-1.25.tar.lz
lzlib                     1.15         15 Jan 2025  https://download.savannah.gnu.org/releases/lzip/lzlib/lzlib-1.15.tar.gz
lzma                      4.32.7       01 Dec 2012  http://tukaani.org/lzma/lzma-4.32.7.tar.bz2
lzo                       2.10         07 Jan 2018  http://www.oberhumer.com/opensource/lzo/download/lzo-2.10.tar.gz
m17nlib                   1.7.0        03 Jan 2018  http://download.savannah.gnu.org/releases/m17n/m17n-lib-1.7.0.tar.gz
m2crypto                  0.36.0       16 Jul 2020  https://files.pythonhosted.org/packages/ff/df/84609ed874b5e6fcd3061a517bf4b6e4d0301f553baf9fa37bef2b509797/M2Crypto-0.36.0.tar.gz
m4                        1.4.19       29 May 2021  https://ftp.gnu.org/gnu/m4/m4-1.4.19.tar.xz
madplay                   0.15.2b      07 Dec 2019  https://sourceforge.net/projects/mad/files/madplay/0.15.2b/madplay-0.15.2b.tar.gz
madwifi                   0.9.4        01 Jun 2014  http://downloads.sourceforge.net/madwifi/madwifi-0.9.4.tar.bz2
magenta                   2.1.3        02 Apr 2021  https://files.pythonhosted.org/packages/f8/42/3d6443d3f93aebf88c10caaa32697fa5c350a6bada76ac6810e3cd94b5eb/magenta-2.1.3.tar.gz
mail                      2.7.1        14 Oct 2018  https://rubygems.org/downloads/mail-2.7.1.gem
mailcommon                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/mailcommon-23.08.5.tar.xz
mailimporter              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/mailimporter-23.08.5.tar.xz
mailman                   2.1.38       01 Dec 2021  https://ftp.gnu.org/pub/gnu/mailman/mailman-2.1.38.tgz
mailutils                 3.13         05 Aug 2021  https://ftp.gnu.org/pub/gnu/mailutils/mailutils-3.13.tar.xz
make                      4.4.1        27 Feb 2023  https://ftp.gnu.org/gnu/make/make-4.4.1.tar.lz
makedepend                1.0.9        12 Feb 2024  https://www.x.org/releases/individual/util/makedepend-1.0.9.tar.xz
mako                      1.3.8        15 Jan 2025  https://files.pythonhosted.org/packages/source/M/Mako/mako-1.3.8.tar.gz
mandb                     2.12.1       05 Apr 2024  https://download.savannah.nongnu.org/releases/man-db/man-db-2.12.1.tar.xz
mandvd                    2.4          01 Jun 2014  http://csgib36.ifrance.com/FTP/mandvd-2.4.tar.gz
manpages                  6.9          15 Jun 2024  https://www.kernel.org/pub/linux/docs/man-pages/man-pages-6.9.tar.xz
mantisbt                  1.2.5        08 Mar 2015  http://sourceforge.net/projects/mantisbt/files/mantis-stable/1.2.5/mantisbt-1.2.5.tar.gz
mapacman                  1.00         01 Jan 2013  http://prdownloads.sourceforge.net/arianne/mapacman-1.00.tar.gz
mapmvideo                 5.32         01 May 2012  http://sourceforge.net/project/showfiles.php?group_id=215566/mapmvideo-5.32.tar.gz
marble                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/marble-23.08.5.tar.xz
marcel                    1.0.2        27 May 2022  https://rubygems.org/downloads/marcel-1.0.2.gem
marco                     1.28.1       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/marco-1.28.1.tar.xz
mariadb                   11.4.3       02 Nov 2024  https://downloads.mariadb.org/interstitial/mariadb-11.4.3/source/mariadb-11.4.3.tar.gz
markdown                  3.6          18 Mar 2024  https://files.pythonhosted.org/packages/22/02/4785861427848cc11e452cc62bb541006a1087cf04a1de83aedd5530b948/Markdown-3.6.tar.gz
markdownpart              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/markdownpart-23.08.5.tar.xz
markupsafe                3.0.2        15 Jan 2025  https://files.pythonhosted.org/packages/b2/97/5d42485e71dfc078108a86d6de8fa46db44a1a9295e89c5d6d4a06e23a62/markupsafe-3.0.2.tar.gz
cms                       2            01 Dec 2012  http://www.marsnomercy.org/cms2/
massxpert                 3.6.0        12 Dec 2015  http://download.tuxfamily.org/massxpert/source/massxpert_3.6.0.tar.gz
mateapplets               1.28.1       16 Dec 2024  https://pub.mate-desktop.org/releases/1.28/mate-applets-1.28.1.tar.xz
matebackgrounds           1.28.0       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-backgrounds-1.28.0.tar.xz
matecalc                  1.28.0       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-calc-1.28.0.tar.xz
matecommon                1.28.0       13 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-common-1.28.0.tar.xz
matecontrolcenter         1.28.0       23 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-control-center-1.28.0.tar.xz
matedesktop               1.28.2       12 Mar 2024  https://pub.mate-desktop.org/releases/1.28/mate-desktop-1.28.2.tar.xz
mateicontheme             1.28.0       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-icon-theme-1.28.0.tar.xz
mateiconthemefaenza       1.20.0       07 Feb 2018  https://pub.mate-desktop.org/releases/1.20/mate-icon-theme-faenza-1.20.0.tar.xz
mateindicatorapplet       1.28.0       25 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-indicator-applet-1.28.0.tar.xz
matemedia                 1.28.0       21 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-media-1.28.0.tar.xz
matemenus                 1.28.0       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-menus-1.28.0.tar.xz
matenetbook               1.26.0       20 Aug 2021  https://pub.mate-desktop.org/releases/1.26/mate-netbook-1.26.0.tar.xz
matenotificationdaemon    1.28.3       15 Dec 2024  https://pub.mate-desktop.org/releases/1.28/mate-notification-daemon-1.28.3.tar.xz
matepanel                 1.28.4       14 Nov 2024  https://pub.mate-desktop.org/releases/1.28/mate-panel-1.28.4.tar.xz
matepolkit                1.28.0       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-polkit-1.28.0.tar.xz
matepowermanager          1.28.1       15 Dec 2024  https://pub.mate-desktop.org/releases/1.28/mate-power-manager-1.28.1.tar.xz
matescreensaver           1.28.0       23 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-screensaver-1.28.0.tar.xz
matesensorsapplet         1.26.0       20 Aug 2021  https://pub.mate-desktop.org/releases/1.26/mate-sensors-applet-1.26.0.tar.xz
matesessionmanager        1.28.0       19 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-session-manager-1.28.0.tar.xz
matesettingsdaemon        1.26.1       23 Jan 2024  https://pub.mate-desktop.org/releases/1.26/mate-settings-daemon-1.26.1.tar.xz
matesystemmonitor         1.28.1       01 Mar 2024  https://pub.mate-desktop.org/releases/1.28/mate-system-monitor-1.28.1.tar.xz
mateterminal              1.28.1       24 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-terminal-1.28.1.tar.xz
matethemes                3.22.26      09 Apr 2024  https://pub.mate-desktop.org/releases/themes/3.22/mate-themes-3.22.26.tar.xz
mateuserguide             1.24.0       10 Feb 2020  https://pub.mate-desktop.org/releases/1.24/mate-user-guide-1.24.0.tar.xz
mateusershare             1.26.0       20 Aug 2021  https://pub.mate-desktop.org/releases/1.26/mate-user-share-1.26.0.tar.xz
mateutils                 1.28.0       23 Feb 2024  https://pub.mate-desktop.org/releases/1.28/mate-utils-1.28.0.tar.xz
mathomatic                15.8.4       01 Jun 2014  http://mathomatic.org/mathomatic-15.8.4.tar.bz2
matplotlib                3.8.4        05 May 2024  https://files.pythonhosted.org/packages/38/4f/8487737a74d8be4ab5fbe6019b0fae305c1604cf7209500969b879b5f462/matplotlib-3.8.4.tar.gz
maxemumtvguide            7.3.2        01 Jun 2014  http://sourceforge.net/projects/mtvg/files/mtvg/7.3.2/maxemumtvguide-7.3.2.tar.gz
maxima                    5.46.0       10 Apr 2023  https://sourceforge.net/projects/maxima/files/Maxima-source/5.46.0-source/maxima-5.46.0.tar.gz
mboximporter              23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/mbox-importer-23.08.5.tar.xz
mc                        4.8.32       24 Aug 2024  https://www.midnight-commander.org/downloads/mc-4.8.32.tar.xz
mcabber                   1.1.0        25 Oct 2018  https://mcabber.com/files/mcabber-1.1.0.tar.bz2
mcelog                    176          22 Jul 2021  https://git.kernel.org/pub/scm/utils/cpu/mce/mcelog.git/snapshot/mcelog-176.tar.gz
mcron                     1.2.3        27 Mar 2023  https://ftp.gnu.org/pub/gnu/mcron/mcron-1.2.3.tar.gz
mcs                       0.4.1        01 Jun 2014  mcs-0.4.1
mcsim                     6.2.0        04 Jun 2020  http://ftp.gnu.org/pub/gnu/mcsim/mcsim-6.2.0.tar.gz
mdadm                     4.3          05 Mar 2024  https://mirrors.edge.kernel.org/pub/linux/utils/raid/mdadm/mdadm-4.3.tar.xz
meanwhile                 1.0.2        01 Jun 2014  https://sourceforge.net/projects/meanwhile/files/meanwhile/1.0.2/meanwhile-1.0.2.tar.gz
mechanize                 2.9.0        10 Apr 2023  https://rubygems.org/downloads/mechanize-2.9.0.gem
mediatomb                 0.12.1       01 Jun 2014  https://sourceforge.net/projects/mediatomb/files/MediaTomb/0.12.1/mediatomb-0.12.1.tar.gz
mediawiki                 1.25.1       28 May 2015  http://releases.wikimedia.org/mediawiki/1.25/mediawiki-1.25.1.tar.gz
medit                     1.2.0        09 May 2017  https://sourceforge.net/projects/mooedit/files/medit/1.2.0/medit-1.2.0.tar.bz2
megapov                   1.2.1        01 Jan 2013  http://megapov.inetart.net/packages/unix/megapov-1.2.1.tar.bz2
megatools                 1.10.3       18 Jun 2020  https://megatools.megous.com/builds/megatools-1.10.3.tar.gz
meld                      3.22.2       24 Mar 2024  https://download.gnome.org/sources/meld/3.22/meld-3.22.2.tar.xz
melons                    1.1.1        01 Jun 2014  http://www.imitationpickles.org/melons/melons-1.1.1.tgz
meme                      5.0.3        19 Jan 2019  http://meme-suite.org/meme-software/5.0.3/meme-5.0.3.tar.gz
memtester                 4.5.1        15 Sep 2022  https://pyropus.ca./software/memtester/old-versions/memtester-4.5.1.tar.gz
menucache                 1.1.0        11 Nov 2017  https://downloads.sourceforge.net/lxde/menu-cache-1.1.0.tar.xz
merb                      1.1.3        26 Nov 2019  https://rubygems.org/downloads/merb-1.1.3.gem
mercurial                 6.8.2        29 Oct 2024  https://www.mercurial-scm.org/release/mercurial-6.8.2.tar.gz
merkuro                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/merkuro-23.08.5.tar.xz
mesa                      24.3.3       04 Jan 2025  https://mesa.freedesktop.org/archive/mesa-24.3.3.tar.xz
mesademos                 6.5          01 Jun 2014  http://mesh.dl.sourceforge.net/sourceforge/mesa3d/MesaDemos-6.5.tar.bz2
mesaglut                  7.10         01 Jun 2014  ftp://ftp.freedesktop.org/pub/mesa/7.10/MesaGLUT-7.10.tar.bz2
mesk                      0.1.0        01 Jun 2014  http://mesk.nicfit.net/releases/mesk-0.1.0.tgz
meson                     1.6.1        14 Jan 2025  https://files.pythonhosted.org/packages/ef/0d/962a44cd22db0503cec62a169ec93c4ae646899e97b918113add2022cf11/meson-1.6.1.tar.gz
mesonpython               0.17.1       15 Jan 2025  https://files.pythonhosted.org/packages/67/66/91d242ea8dd1729addd36069318ba2cd03874872764f316c3bb51b633ed2/meson_python-0.17.1.tar.gz
messagelib                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/messagelib-23.08.5.tar.xz
metacity                  3.54.0       11 Oct 2024  https://download.gnome.org/sources/metacity/3.54/metacity-3.54.0.tar.xz
metaclass                 0.0.4        04 Aug 2018  https://rubygems.org/downloads/metaclass-0.0.4.gem
methodsource              1.0.0        28 May 2020  https://rubygems.org/downloads/method_source-1.0.0.gem
mftrace                   1.2.14       01 Jun 2013  http://www.xs4all.nl/~hanwen/mftrace/mftrace-1.2.14.tar.gz
mgaview                   0.0.30       01 Jun 2014  http://sourceforge.net/projects/mgaview/mgaview-0.0.30
midieye                   0.3.10       13 Dec 2019  https://rubygems.org/downloads/midi-eye-0.3.10.gem
midilib                   2.0.5        16 Jun 2015  https://rubygems.org/downloads/midilib-2.0.5.gem
midori                    0.5.1        01 Feb 2012  http://archive.xfce.org/src/apps/midori/0.5/midori-0.5.1.tar.bz2
milkytracker              0.90.85      01 Jun 2014  http://milkytracker.org/files/milkytracker-0.90.85.tar.bz2
milou                     6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/milou-6.2.5.tar.xz
mimemagic                 0.4.3        15 Apr 2021  https://rubygems.org/downloads/mimemagic-0.4.3.gem
mimetypes                 3.4.1        27 May 2022  https://rubygems.org/downloads/mime-types-3.4.1.gem
mimetypesdata             3.2022.0105  27 May 2022  https://rubygems.org/downloads/mime-types-data-3.2022.0105.gem
showfilesphp?group        id=18365&package_id=187304&release_id=611489 01 Jun 2014  http://sourceforge.net/project/showfiles.php?group_id=18365&package_id=187304&release_id=611489
mingetty                  1.07.orig    01 Jun 2014  http://ftp.de.debian.org/debian/pool/main/m/mingetty/mingetty-1.07.orig.tar.gz
minimagick                4.11.0       15 Apr 2021  https://rubygems.org/downloads/mini_magick-4.11.0.gem
minimap2                  2.17         08 Jan 2021  https://github.com/lh3/minimap2/releases/download/v2.17/minimap2-2.17.tar.bz2
minimime                  1.1.2        27 May 2022  https://rubygems.org/downloads/mini_mime-1.1.2.gem
miniportile2              2.8.0        27 May 2022  https://rubygems.org/downloads/mini_portile2-2.8.0.gem
minit                     0.10         03 Mar 2015  http://hisham.hm/htop/releases/1.0.3/minit-0.10.tar.xz
minitar                   0.9          28 May 2020  https://rubygems.org/downloads/minitar-0.9.gem
minitest                  5.16.1       22 Jun 2022  https://rubygems.org/downloads/minitest-5.16.1.gem
minitestsprint            1.2.2        22 Jun 2022  https://rubygems.org/downloads/minitest-sprint-1.2.2.gem
minizip                   ng           13 Feb 2023  https://github.com/zlib-ng/minizip-ng/
minuet                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/minuet-23.08.5.tar.xz
mirageimage               viewer       01 Jun 2014  http://raschix.de/article/mirage-image-viewer
missile                   1.0.1        01 Jun 2014  http://missile.sourceforge.net/dl/missile-1.0.1.tar.gz
mjpegtools                2.2.1        01 Mar 2023  https://sourceforge.net/projects/mjpeg/files/mjpegtools/2.2.1/mjpegtools-2.2.1.tar.gz
mkcfm                     1.0.1        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/app/mkcfm-1.0.1.tar.bz2
mkcomposecache            1.2.2        04 Apr 2022  https://www.x.org/releases/individual/app/mkcomposecache-1.2.2.tar.xz
mkfontdir                 1.0.7        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/app/mkfontdir-1.0.7.tar.bz2
mkfontscale               1.2.3        04 Mar 2024  https://www.x.org/releases/individual/app/mkfontscale-1.2.3.tar.xz
mkrf                      0.2.3        29 Nov 2017  https://rubygems.org/downloads/mkrf-0.2.3.gem
mktemp                    1.7          01 Dec 2012  https://www.mktemp.org/dist/mktemp-1.7.tar.gz
mkvtoolnix                41.0.0       01 Feb 2020  https://mkvtoolnix.download/sources/mkvtoolnix-41.0.0.tar.xz
mlr                       4.4.0        14 Aug 2016  https://github.com/johnkerl/miller/releases/download/v4.4.0/mlr-4.4.0.tar.gz
mlt                       7.30.0       21 Jan 2025  https://github.com/mltframework/mlt/releases/download/v7.30.0/mlt-7.30.0.tar.gz
mmcommon                  1.0.5        03 Dec 2022  https://download.gnome.org/sources/mm-common/1.0/mm-common-1.0.5.tar.xz
mobs                      2.3.2        01 Jun 2014  http://www.dervishd.net/Projects/mobs-2.3.2.tar.gz
moc                       2.5.2        09 Jun 2017  http://ftp.daper.net/pub/soft/moc/stable/moc-2.5.2.tar.bz2
mocha                     1.14.0       27 May 2022  https://rubygems.org/downloads/mocha-1.14.0.gem
modemmanager              1.18.12      22 Aug 2024  https://www.freedesktop.org/software/ModemManager/ModemManager-1.18.12.tar.xz
modemmanagerqt            5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/modemmanager-qt-5.116.0.tar.xz
modmono                   1.2.5        01 Jun 2014  http://go-mono.com/sources/mod_mono/mod_mono-1.2.5.tar.bz2
modperl                   2.0.11       06 Oct 2019  http://archive.apache.org/dist/perl/mod_perl-2.0.11.tar.gz
modpython                 3.5.0        31 Oct 2017  http://dist.modpython.org/dist/mod_python-3.5.0.tgz
modulebuild               0.4224       13 Oct 2018  https://cpan.metacpan.org/authors/id/L/LE/LEONT/Module-Build-0.4224.tar.gz
moduleinittools           3.16         01 Jun 2014  ftp://ftp.kernel.org/pub/linux/utils/kernel/module-init-tools/module-init-tools-3.16.tar.bz2
modulemd                  1.3.3        12 Jul 2020  https://files.pythonhosted.org/packages/7c/e3/b066246183455e8437774bb4408ef3f61f23adfca52efab6285160299378/modulemd-1.3.3.tar.gz
modules                   3.2.10       27 Aug 2015  https://sourceforge.net/projects/modules/files/Modules/modules-3.2.10/modules-3.2.10.tar.bz2
moe                       1.10         12 Apr 2023  https://github.com/fox0430/moe/archive/refs/tags/moe-1.10.tar.gz
mojolicious               2.0          01 Jan 2013  http://cpan.metacpan.org/authors/id/S/SR/SRI/Mojolicious-2.0.tar.gz
momomoto                  0.2.1        01 Dec 2017  https://rubygems.org/downloads/momomoto-0.2.1.gem
moneta                    1.5.1        27 May 2022  https://rubygems.org/downloads/moneta-1.5.1.gem
money                     6.16.0       27 May 2022  https://rubygems.org/downloads/money-6.16.0.gem
mongodb                   r3.4.6       20 Apr 2015  https://fastdl.mongodb.org/src/mongodb-r3.4.6.tar.gz
mongrel                   1.1.5        01 Aug 2017  https://rubygems.org/downloads/mongrel-1.1.5.gem
mongrel2                  v1.10.0      22 Dec 2015  https://github.com/mongrel2/mongrel2/releases/download/v1.10.0/mongrel2-v1.10.0.tar.bz2
monit                     5.33.0       14 Apr 2023  https://mmonit.com/monit/dist/monit-5.33.0.tar.gz
mono                      6.13.0.1255  20 Sep 2024  https://download.mono-project.com/sources/mono/nightly/mono-6.13.0.1255.tar.xz
monoaddins                0.4          01 Jun 2014  http://ftp.novell.com/pub/mono/sources/mono-addins/mono-addins-0.4.tar.bz2
monodevelop               7.8.4.1      28 Sep 2019  https://download.mono-project.com/sources/monodevelop/monodevelop-7.8.4.1.tar.bz2
monsterz                  0.7.1        10 Oct 2017  http://sam.zoy.org/monsterz/monsterz-0.7.1.tar.gz
moodle                    3.4          08 Dec 2017  https://sourceforge.net/projects/moodle/files/Moodle/stable34/moodle-3.4.tgz
moonbuggy                 1.0.51       24 Mar 2016  http://m.seehuhn.de/programs/moon-buggy-1.0.51.tar.gz
moserial                  3.0.21       27 Nov 2021  https://download.gnome.org/sources/moserial/3.0/moserial-3.0.21.tar.xz
mosh                      1.3.2        07 Apr 2019  https://mosh.org/mosh-1.3.2.tar.gz
most                      5.0.0a       01 Jun 2014  ftp://space.mit.edu/pub/davis/most/most-5.0.0a.tar.bz2
motif                     2.3.8        09 Dec 2017  https://sourceforge.net/projects/motif/files/Motif%202.3.8%20Source%20Code/motif-2.3.8.tar.gz
mousepad                  0.6.0        11 Feb 2023  https://archive.xfce.org/src/apps/mousepad/0.6/mousepad-0.6.0.tar.bz2
mousetrap                 3.17.3       23 Jun 2015  http://ftp.gnome.org/pub/GNOME/sources/mousetrap/3.17/mousetrap-3.17.3.tar.xz
mozart2                   13.12.2015   13 Dec 2015  http://www.mozart-oz.org/download/mozart-ftp/store/1.3.1-2004-06-16/mozart2-13.12.2015.tar.xz
firefox                   115.12.0esr  08 Jul 2024  https://archive.mozilla.org/pub/firefox/releases/115.12.0esr/source/firefox-115.12.0esr.source.tar.xz
mozo                      1.26.2       02 Nov 2022  https://pub.mate-desktop.org/releases/1.26/mozo-1.26.2.tar.xz
mp                        5.2.2        01 Jun 2014  http://triptico.com/download/mp-5.2.2.tar.gz
mp3blaster                3.2.6        17 Nov 2017  https://sourceforge.net/projects/mp3blaster/files/mp3blaster/mp3blaster-3.2.6/mp3blaster-3.2.6.tar.gz
mp3cat                    0.4          01 Jun 2014  http://tomclegg.net/software/mp3cat-0.4.tar.gz
mp3daemon                 0.63         01 May 2014  http://search.cpan.org/CPAN/authors/id/B/BE/BEPPU/MP3-Daemon-0.63.tar.gz
mp3diags                  1.0.12.079   01 Jan 2013  http://sourceforge.net/projects/mp3diags/files/mp3diags/MP3Diags-1.0.12.079.tar.gz
mp3info                   0.8.5        01 Jun 2014  ftp://ftp.ibiblio.org/pub/linux/apps/sound/mp3-utils/mp3info/mp3info-0.8.5.tgz
mp3splt                   2.4.2        01 Jun 2014  http://downloads.sourceforge.net/project/mp3splt/mp3splt/2.4.2/mp3splt-2.4.2.tar.gz
mp3val                    0.1.8        01 Jan 2013  http://downloads.sourceforge.net/mp3val/mp3val-0.1.8.tar.gz
mp3wrap                   0.5          01 Jun 2014  http://prdownloads.sourceforge.net/mp3wrap/mp3wrap-0.5.tar.gz
mpage                     2.5.6        07 Feb 2016  http://www.mesa.nl/pub/mpage/mpage-2.5.6.tgz
mpc                       1.3.1        16 Dec 2022  https://ftp.gnu.org/pub/gnu/mpc/mpc-1.3.1.tar.gz
mpd                       0.21.25      31 Aug 2020  http://www.musicpd.org/download/mpd/0.21/mpd-0.21.25.tar.xz
mpdecimal                 4.0.0        27 Mar 2024  https://www.bytereef.org/software/mpdecimal/releases/mpdecimal-4.0.0.tar.gz
mpeg2dec                  0.4.1        01 Jun 2014  https://libmpeg2.sourceforge.net/files/mpeg2dec-0.4.1.tar.gz
mpeg4ip                   1.5.0.1      01 Jun 2014  http://wiki.ubuntuusers.de/Archiv/mpeg4ip-1.5.0.1
mpfr                      4.2.1        22 Aug 2023  https://ftp.gnu.org/pub/gnu/mpfr/mpfr-4.2.1.tar.xz
mpg123                    1.32.10      20 Dec 2024  https://downloads.sourceforge.net/mpg123/mpg123-1.32.10.tar.bz2
mpg321                    0.2.13.3     01 Jun 2014  https://sourceforge.net/projects/mpg321/files/mpg321/0.2.13/mpg321-0.2.13.3.tar.gz
mpgedit0                  75dev2       01 Jun 2014  http://mpgedit.org/mpgedit/download/0_75/mpgedit_0-75dev2_src.tgz
mpgtx                     1.3.1        01 Jun 2014  http://ovh.dl.sourceforge.net/sourceforge/mpgtx/mpgtx-1.3.1.tgz
mplayer                   1.5          28 Feb 2022  https://mplayerhq.hu/MPlayer/releases/MPlayer-1.5.tar.xz
mpmath                    1.2.1        12 Jun 2021  https://files.pythonhosted.org/packages/95/ba/7384cb4db4ed474d4582944053549e02ec25da630810e4a23454bc9fa617/mpmath-1.2.1.tar.gz
mpop                      1.4.3        12 Feb 2019  https://marlam.de/mpop/releases/mpop-1.4.3.tar.xz
mppenc                    1.16         01 Jun 2014  http://files.musepack.net/source/mppenc-1.16.tar.bz2
v                         0.38.0       15 Jan 2025  https://github.com/mpv-player/mpv/archive/refs/tags/v0.38.0.zip
mrxvt                     0.5.4        01 Jun 2014  http://prdownloads.sourceforge.net/materm/mrxvt-0.5.4.tar.gz
msgpackc                  6.1.0        15 Sep 2024  https://github.com/msgpack/msgpack-c/releases/download/c-6.1.0/msgpack-c-6.1.0.tar.gz
msitools                  0.102        25 Jun 2023  https://download.gnome.org/sources/msitools/0.102/msitools-0.102.tar.xz
msleep                    0.1.0        25 Jul 2009  https://rubygems.org/downloads/msleep-0.1.0.gem
msmtp                     1.8.1        09 Dec 2018  https://marlam.de/msmtp/releases/msmtp-1.8.1.tar.xz
ms                        sysforge.net 01 Jun 2014  http://ms-sys.sourceforge.net/
mtail                     1.1.1        01 Jan 2013  http://matt.immute.net/src/mtail/mtail-1.1.1.tgz
mtdev                     1.1.7        06 Apr 2024  http://bitmath.org/code/mtdev/mtdev-1.1.7.tar.bz2
mtools                    4.0.43       22 Mar 2023  https://ftp.gnu.org/pub/gnu/mtools/mtools-4.0.43.tar.bz2
mtpaint                   3.50         30 Jan 2021  https://sourceforge.net/projects/mtpaint/files/mtpaint/3.50/mtpaint-3.50.tar.bz2
mtr                       0.95         13 Jan 2022  ftp://ftp.bitwizard.nl/mtr/mtr-0.95.tar.gz
mudlet                    1.0.5        01 Jun 2014  mudlet-1.0.5.tar.gz
mudmagic                  1.9          01 Jun 2014  https://sourceforge.net/projects/kyndig/files/Mud%20Magic%20Source/1.9/mudmagic-1.9.tar.gz
multiaterm                0.2.1        01 Jun 2014  http://www.nongnu.org/materm/multi-aterm-0.2.1.tar.gz
multijson                 1.15.0       15 Apr 2021  https://rubygems.org/downloads/multi_json-1.15.0.gem
multipartpost             2.2.3        22 Jun 2022  https://rubygems.org/downloads/multipart-post-2.2.3.gem
mer                       3.23         01 Nov 2013  https://sourceforge.net/projects/mummer/files/mummer/3.23/MUMmer3.23.tar.gz
muparser                  2.2.6.1      19 Jan 2019  https://github.com/beltoforion/muparser/archive/muparser-2.2.6.1.tar.gz
mupdf                     1.24.11      20 Nov 2024  https://www.mupdf.com/downloads/archive/mupdf-1.24.11-source.tar.gz
murmurhash3               0.1.7        22 Mar 2023  https://rubygems.org/downloads/murmurhash3-0.1.7.gem
muse                      2.2.1        25 Sep 2015  http://sourceforge.net/projects/lmuse/files/muse-2.2/muse-2.2.1.tar.gz
musepack                  r475         01 Nov 2012  http://files.musepack.net/source/musepack_src_r475.tar.gz
musescore                 3.0.1        17 Jan 2019  https://github.com/musescore/MuseScore/archive/musescore-3.0.1.tar.gz
musikcube                 0.61.0       14 Jan 2019  https://github.com/clangen/musikcube/archive/musikcube-0.61.0.tar.gz
musl                      1.2.3        02 Aug 2022  https://musl.libc.org/releases/musl-1.2.3.tar.gz
mustermann                1.1.1        28 May 2020  https://rubygems.org/downloads/mustermann-1.1.1.gem
mutagen                   1.43.0       04 Dec 2019  https://files.pythonhosted.org/packages/aa/fd/ed738775442e3614849fefdf41417d7bff3ccb010d49c8f729432cc3c1e5/mutagen-1.43.0.tar.gz
mutt                      2.2.13       09 Mar 2024  http://ftp.mutt.org/pub/mutt/mutt-2.2.13.tar.gz
mutter                    46.8         16 Jan 2025  https://download.gnome.org/sources/mutter/46/mutter-46.8.tar.xz
mxk                       1.10         01 Jun 2014  http://welz.org.za/projects/mxk/mxk-1.10.tar.gz
mxml                      3.2          07 Mar 2021  https://github.com/michaelrsweet/mxml/releases/download/v3.2/mxml-3.2.tar.gz
myghty                    1.0          01 Jun 2014  http://easynews.dl.sourceforge.net/sourceforge/myghty/Myghty-1.0.tar.gz
myman                     0.7.0        10 Oct 2017  https://sourceforge.net/projects/myman/files/myman/myman-0.7.0/myman-0.7.0.tar.gz
mypaint                   2.0.1        30 May 2020  https://github.com/mypaint/mypaint/releases/download/v2.0.1/mypaint-2.0.1.tar.xz
mypaintbrushes            2.0.2        22 Jun 2020  https://github.com/mypaint/mypaint-brushes/releases/download/v2.0.2/mypaint-brushes-2.0.2.tar.xz
mysql                     9.1.0        15 Oct 2024  https://cdn.mysql.com/Downloads/MySQL-9.0/mysql-9.1.0.tar.gz
mythplugins               0.20a        01 Jun 2014  http://www.mythtv.org/modules.php?name=Downloads&d_op=getit&lid=124/mythplugins-0.20a
nagios                    4.3.1        01 Jun 2014  https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.3.1.tar.gz#_ga=1.110313093.1455961778.1489835372
naim                      0.11.8.3.1   01 Jun 2014  http://naim.googlecode.com/files/naim-0.11.8.3.1.tar.bz2
najitool                  0.8.4        07 Oct 2017  https://sourceforge.net/projects/najitool/files/najitool/0.8.4/najitool-0.8.4.tar.bz2
nana                      201.5.4      08 Aug 2017  https://sourceforge.net/projects/nanapro/files/Nana/Nana%201.x/nana%201.5.4.zip
nano                      8.3          21 Dec 2024  https://www.nano-editor.org/dist/latest/nano-8.3.tar.gz
nap                       1.5.4        01 Jun 2014  http://nap.sourceforge.net/dist/nap-1.5.4.tar.gz
nas                       1.9.3        01 Aug 2013  http://sourceforge.net/projects/nas/files/nas/nas.1.9.3%20%28stable%29/nas-1.9.3.tar.gz
nasm                      2.16.03      18 Apr 2024  https://www.nasm.us/pub/nasm/releasebuilds/2.16.03/nasm-2.16.03.tar.xz
nast                      0.2.0        05 Jun 2014  http://download.berlios.de/nast/nast-0.2.0.tar.gz
nativepackageinstaller    1.1.4        27 May 2022  https://rubygems.org/downloads/native-package-installer-1.1.4.gem
nautilus                  46.1         21 Apr 2024  https://download.gnome.org/sources/nautilus/46/nautilus-46.1.tar.xz
nautilusactions           3.2.4        01 Aug 2014  http://ftp.gnome.org/pub/GNOME/sources/nautilus-actions/3.2/nautilus-actions-3.2.4.tar.xz
nautiluscdburner          2.22.1       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/nautilus-cd-burner/2.22/nautilus-cd-burner-2.22.1.tar.bz2
nautilusmedia             0.8.1        01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/nautilus-media/0.8/nautilus-media-0.8.1.tar.bz2
nautiluspython            1.2.3        19 Jul 2019  http://ftp.gnome.org/pub/gnome/sources/nautilus-python/1.2/nautilus-python-1.2.3.tar.xz
nautilussendto            3.8.6        09 Aug 2017  http://ftp.gnome.org/pub/gnome/sources/nautilus-sendto/3.8/nautilus-sendto-3.8.6.tar.xz
nazghul                   0.7.1        31 Mar 2015  https://downloads.sourceforge.net/project/nazghul/nazghul/nazghul-0.7.1/nazghul-0.7.1.tar.gz
ncbiblast                 2.11.0+      26 Apr 2021  https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.11.0+.zip
ncc                       2.8          01 Jun 2014  http://students.ceid.upatras.gr/~sxanth/ncc/ncc-2.8.tar.gz
ncdu                      1.19         05 Apr 2024  https://dev.yorhel.nl/download/ncdu-1.19.tar.gz
ncftp                     3.2.6        28 Nov 2016  ftp://ftp.ncftp.com/ncftp/ncftp-3.2.6.tar.xz
ncmpcpp                   0.5.10       01 Jun 2014  http://unkart.ovh.org/ncmpcpp/ncmpcpp-0.5.10.tar.bz2
ncompress                 4.2.4.5      29 Apr 2018  https://sourceforge.net/projects/ncompress/files/ncompress-4.2.4.5.tar.gz
ncurses                   6.5          28 Apr 2024  https://invisible-mirror.net/archives/ncurses/ncurses-6.5.tar.gz
ndeskdbusglib             0.4.1        01 Feb 2013  http://www.ndesk.org/archive/dbus-sharp/ndesk-dbus-glib-0.4.1.tar.gz
ndiswrapper               1.63         30 Jul 2020  https://sourceforge.net/projects/ndiswrapper/files/stable/ndiswrapper-1.63.tar.gz
nemesis                   1.5          22 Feb 2019  https://github.com/troglobit/nemesis/releases/download/v1.5/nemesis-1.5.tar.xz
nemiver                   0.9.6        24 Sep 2015  http://ftp.gnome.org/pub/GNOME/sources/nemiver/0.9/nemiver-0.9.6.tar.xz
neochat                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/neochat-23.08.5.tar.xz
neofetch                  7.0.0        23 Jun 2020  https://github.com/dylanaraps/neofetch/archive/neofetch-7.0.0.tar.gz
neon                      0.34.0       24 Nov 2024  https://notroj.github.io/neon/neon-0.34.0.tar.gz
net6                      1.3.14       16 Dec 2018  http://releases.0x539.de/net6/net6-1.3.14.tar.gz
netatalk                  3.1.13       22 Jan 2025  https://sourceforge.net/projects/netatalk/files/netatalk/3.1.13/netatalk-3.1.13.tar.bz2
netcat                    0.7.1        01 Jun 2014  http://sourceforge.net/projects/netcat/files/netcat/0.7.1/netcat-0.7.1.tar.gz
netdns                    1.27         17 Sep 2020  https://www.cpan.org/authors/id/N/NL/NLNETLABS/Net-DNS-1.27.tar.gz
nethack                   3.6.2        20 Oct 2019  https://sourceforge.net/projects/nethack/files/nethack/3.6.2/nethack-3.6.2.tgz
nethttp                   6.17         27 Sep 2017  http://search.cpan.org/CPAN/authors/id/O/OA/OALDERS/Net-HTTP-6.17.tar.gz
netkitbase                0.17         01 Jun 2014  ftp://ftp.uk.linux.org/pub/linux/Networking/netkit/netkit-base-0.17.tar.gz
netkitrsh                 0.17         01 Jun 2014  ftp://ftp.uk.linux.org/pub/linux/Networking/netkit/netkit-rsh-0.17.tar.gz
netpbm                    10.86.44     22 Jan 2025  https://sourceforge.net/projects/netpbm/files/super_stable/10.86.44/netpbm-10.86.44.tgz
netping                   2.0.8        27 May 2022  https://rubygems.org/downloads/net-ping-2.0.8.gem
netrc                     0.11.0       29 Sep 2017  https://rubygems.org/downloads/netrc-0.11.0.gem
netscp                    3.0.0        28 May 2020  https://rubygems.org/downloads/net-scp-3.0.0.gem
nettle                    3.10         17 Jun 2024  https://www.lysator.liu.se/~nisse/archive/nettle-3.10.tar.gz
networkmanager            1.51.4       23 Dec 2024  https://download.gnome.org/sources/NetworkManager/1.51/NetworkManager-1.51.4.tar.xz
networkmanagerapplet      1.32.0       17 Apr 2023  https://download.gnome.org/sources/network-manager-applet/1.32/network-manager-applet-1.32.0.tar.xz
networkmanagerfortisslvpn 1.3.90       16 Jul 2019  http://ftp.gnome.org/pub/gnome/sources/NetworkManager-fortisslvpn/1.3/NetworkManager-fortisslvpn-1.3.90.tar.xz
networkmanagerlibreswan   1.2.10       15 Oct 2018  http://ftp.gnome.org/pub/gnome/sources/NetworkManager-libreswan/1.2/NetworkManager-libreswan-1.2.10.tar.xz
networkmanageropenconnect 1.2.2        14 May 2016  http://ftp.gnome.org/pub/GNOME/sources/NetworkManager-openconnect/1.2/NetworkManager-openconnect-1.2.2.tar.xz
networkmanageropenvpn     1.8.14       30 Mar 2021  https://download.gnome.org/sources/NetworkManager-openvpn/1.8/NetworkManager-openvpn-1.8.14.tar.xz
networkmanagerpptp        1.2.8        04 Oct 2018  http://ftp.gnome.org/pub/gnome/sources/NetworkManager-pptp/1.2/NetworkManager-pptp-1.2.8.tar.xz
networkmanagerqt          5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/networkmanager-qt-5.116.0.tar.xz
networkmanagervpnc        1.2.6        20 Jul 2018  http://ftp.gnome.org/pub/gnome/sources/NetworkManager-vpnc/1.2/NetworkManager-vpnc-1.2.6.tar.xz
networkx                  2.3          12 Apr 2019  https://files.pythonhosted.org/packages/85/08/f20aef11d4c343b557e5de6b9548761811eb16e438cee3d32b1c66c8566b/networkx-2.3.zip
neverball                 1.6.0        27 Mar 2016  http://neverball.org/neverball-1.6.0.tar.gz
newlib                    2.0.0        01 Jun 2014  ftp://sourceware.org/pub/newlib/newlib-2.0.0.tar.gz
newrelicrpm               8.8.0        22 Jun 2022  https://rubygems.org/downloads/newrelic_rpm-8.8.0.gem
newsbeuter                2.9          03 Oct 2017  https://www.newsbeuter.org/downloads/newsbeuter-2.9.tar.gz
newt                      0.52.23      02 Dec 2022  https://releases.pagure.org/newt/newt-0.52.23.tar.gz
nfsutils                  2.8.2        13 Dec 2024  https://sourceforge.net/projects/nfs/files/nfs-utils/2.8.2/nfs-utils-2.8.2.tar.xz
nftables                  1.1.1        03 Oct 2024  https://www.nftables.org/projects/nftables/files/nftables-1.1.1.tar.xz
nghttp2                   1.63.0       28 Aug 2024  https://github.com/nghttp2/nghttp2/releases/download/v1.63.0/nghttp2-1.63.0.tar.xz
nginx                     1.26.2       15 Aug 2024  https://nginx.org/download/nginx-1.26.2.tar.gz
nglview                   3.0.8        28 Oct 2023  https://files.pythonhosted.org/packages/6a/3e/40ee8822c10bf8b00d037ede875ac2a1a26b07fae5e26fbd8af3404257e3/nglview-3.0.8.tar.gz
nihtest                   1.5.2        05 Apr 2024  https://github.com/nih-at/nihtest/releases/download/v1.5.2/nihtest-1.5.2.tar.gz
nim                       1.6.12       01 May 2023  https://nim-lang.org/download/nim-1.6.12.tar.xz
ninja                     13.05.2024   15 Jan 2025  https://github.com/ninja-build/ninja/archive/refs/tags/ninja-13.05.2024.tar.gz
nio4r                     2.5.8        27 May 2022  https://rubygems.org/downloads/nio4r-2.5.8.gem
nmap                      7.95         23 Apr 2024  https://nmap.org/dist/nmap-7.95.tar.bz2
node                      v22.13.0     17 Jan 2025  https://nodejs.org/dist/v22.13.0/node-v22.13.0.tar.xz
nokogiri                  1.13.8       04 Sep 2022  https://rubygems.org/downloads/nokogiri-1.13.8.gem
normalize                 0.7.7        01 Jun 2014  http://savannah.nongnu.org/download/normalize/normalize-0.7.7.tar.bz2
notificationdaemon        3.18.2       16 Feb 2016  http://ftp.gnome.org/pub/GNOME/sources/notification-daemon/3.18/notification-daemon-3.18.2.tar.xz
notify2                   0.3.1        04 Sep 2017  https://pypi.python.org/packages/aa/e8/d4b335aa739dc299a77766ecc5f1972d1de1993524aa94acef3219bba315/notify2-0.3.1.tar.gz
notifypython              0.1.1        25 Apr 2015  http://www.galago-project.org/files/releases/source/notify-python/notify-python-0.1.1.tar.bz2
npth                      1.8          14 Nov 2024  https://www.gnupg.org/ftp/gcrypt/npth/npth-1.8.tar.bz2
nsd                       4.2.1        11 Jul 2019  https://www.nlnetlabs.nl/downloads/nsd/nsd-4.2.1.tar.gz
nspluginwrapper           1.2.2        01 Jun 2014  http://gwenole.beauchesne.info/projects/nspluginwrapper/files/nspluginwrapper-1.2.2.tar.bz2
nspr                      4.35         17 Sep 2022  https://archive.mozilla.org/pub/nspr/releases/v4.35/src/nspr-4.35.tar.gz
nss                       3.105        01 Oct 2024  https://archive.mozilla.org/pub/security/nss/releases/NSS_3_105_RTM/src/nss-3.105.tar.gz
ntfs3gntfsprogs           2022.10.3    31 Oct 2022  https://tuxera.com/opensource/ntfs-3g_ntfsprogs-2022.10.3.tgz
ntp                       4.2.8p15     24 Jun 2020  https://www.eecis.udel.edu/~ntp/ntp_spool/ntp4/ntp-4.2/ntp-4.2.8p15.tar.gz
ntrack                    017          09 Jul 2015  https://launchpad.net/ntrack/main/017/+download/ntrack-017.tar.gz
numactl                   2.0.18       12 Aug 2024  https://github.com/numactl/numactl/releases/download/v2.0.18/numactl-2.0.18.tar.gz
numonarray                0.9.2.0      27 May 2022  https://rubygems.org/downloads/numo-narray-0.9.2.0.gem
numpy                     2.2.1        15 Jan 2025  https://files.pythonhosted.org/packages/f2/a5/fdbf6a7871703df6160b5cf3dd774074b086d278172285c52c2758b76305/numpy-2.2.1.tar.gz
nuvie                     0.5          19 Oct 2014  http://sourceforge.net/projects/nuvie/files/Nuvie/0.5/nuvie-0.5.tgz
nvclock                   0.8          01 Jun 2014  http://www.linuxhardware.org/nvclock/nvclock0.8b4.tar.gz
nwcc                      0.8.2        01 Jun 2014  http://sourceforge.net/projects/nwcc/files/nwcc/nwcc%200.8.2/nwcc_0.8.2.tar.gz
nzbget                    17.1         06 Sep 2016  https://github.com/nzbget/nzbget/releases/download/v17.1/nzbget-17.1.tar.gz
obby                      0.4.6        01 Jun 2014  http://releases.0x539.de/obby/obby-0.4.6.tar.gz
obconf                    2.0.4        23 Aug 2017  http://openbox.org/dist/obconf/obconf-2.0.4.tar.gz
obconfqt                  0.16.0       07 Nov 2020  https://github.com/lxqt/obconf-qt/releases/download/0.16.0/obconf-qt-0.16.0.tar.xz
obexdataserver            0.4.6        01 Dec 2012  http://tadas.dailyda.com/software/obex-data-server-0.4.6.tar.gz
ocaml                     4.04.0       25 Feb 2017  http://caml.inria.fr/pub/distrib/ocaml-4.04/ocaml-4.04.0.tar.gz
oceansoundtheme           6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/ocean-sound-theme-6.2.5.tar.xz
oclock                    1.0.5        01 Sep 2022  https://www.x.org/releases/individual/app/oclock-1.0.5.tar.xz
ocra                      1.3.11       28 May 2020  https://rubygems.org/downloads/ocra-1.3.11.gem
ocrad                     0.29         12 May 2024  https://ftp.gnu.org/gnu/ocrad/ocrad-0.29.tar.lz
ocrfeeder                 0.8.3        09 Mar 2020  http://ftp.gnome.org/pub/gnome/sources/ocrfeeder/0.8/ocrfeeder-0.8.3.tar.xz
octave                    9.3.0        14 Dec 2024  https://ftp.gnu.org/gnu/octave/octave-9.3.0.tar.xz
ogmrip                    1.0.1        30 Aug 2015  http://sourceforge.net/projects/ogmrip/files/ogmrip/1.0/1.0.1/ogmrip-1.0.1.tar.gz
ogmtools                  1.5          01 Jun 2014  http://www.bunkus.org/videotools/ogmtools/ogmtools-1.5.tar.bz2
v                         13.4.4       12 Sep 2022  https://github.com/OGRECave/ogre/archive/refs/tags/v13.4.4.tar.gz
oki                       0.1.6        01 Jun 2014  http://free.of.pl/s/szatkus/soft/gry/oki-0.1.6.tar.gz
okular                    24.08.3      28 Nov 2024  https://download.kde.org/stable/release-service/24.08.3/src/okular-24.08.3.tar.xz
omake                     0.10.5       28 Aug 2022  http://download.camlcity.org/download/omake-0.10.5.tar.gz
onig                      6.9.9        19 Oct 2023  https://github.com/kkos/oniguruma/releases/download/v6.9.9/onig-6.9.9.tar.gz
opal                      1.5.0        27 May 2022  https://rubygems.org/downloads/opal-1.5.0.gem
openal                    0.0.8        01 Jun 2014  http://www.openal.org/openal_webstf/downloads/openal-0.0.8.tar.gz
openalsoft                1.24.1       30 Nov 2024  https://openal-soft.org/openal-releases/openal-soft-1.24.1.tar.bz2
openbabel                 2.4.1        20 Sep 2017  https://sourceforge.net/projects/openbabel/files/openbabel/2.4.1/openbabel-2.4.1.tar.gz
openblas                  0.3.29       22 Jan 2025  https://github.com/OpenMathLib/OpenBLAS/releases/download/v0.3.29/OpenBLAS-0.3.29.tar.gz
openbox                   3.6.1        01 Jul 2015  http://openbox.org/dist/openbox/openbox-3.6.1.tar.gz
opencobol                 1.1          01 Dec 2013  https://downloads.sourceforge.net/project/open-cobol/open-cobol/1.1/open-cobol-1.1.tar.gz
openconnect               8.05         09 Mar 2020  ftp://ftp.infradead.org/pub/openconnect/openconnect-8.05.tar.gz
opencv                    4.9.0        06 Mar 2024  https://github.com/opencv/opencv/archive/4.9.0/opencv-4.9.0.tar.gz
openexr                   3.3.2        12 Nov 2024  https://github.com/AcademySoftwareFoundation/openexr/releases/download/v3.3.2/openexr-3.3.2.tar.gz
openinvaders              0.3          01 Dec 2012  http://www.jamyskis.net/invaders.php/open-invaders-0.3.tar.gz
openjade                  1.3.2        01 Jun 2014  https://prdownloads.sourceforge.net/openjade/openjade-1.3.2.tar.gz
openjpeg                  2.5.3        11 Dec 2024  https://github.com/uclouvain/openjpeg/archive/v2.5.3/openjpeg-2.5.3.tar.gz
openldap                  2.6.9        28 Nov 2024  https://openldap.org/software/download/OpenLDAP/openldap-release/openldap-2.6.9.tgz
openldev                  1.0          21 Aug 2017  https://sourceforge.net/projects/openldev/files/openldev/1.0/openldev-1.0.tar.gz
openmovieeditor           0.0.20090105 05 Apr 2024  http://downloads.sourceforge.net/openmovieeditor/openmovieeditor-0.0.20090105.tar.gz
openmpi                   4.1.4        20 Dec 2022  https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-4.1.4.tar.bz2
openobex                  1.7.2-Source 12 Jan 2019  https://sourceforge.net/projects/openobex/files/openobex/1.7.2/openobex-1.7.2-Source.tar.gz
openresolv                3.13.2       09 Feb 2024  https://github.com/NetworkConfiguration/openresolv/releases/download/v3.13.2/openresolv-3.13.2.tar.xz
openscenegraph            2.4.0        01 Jun 2014  OpenSceneGraph-2.4.0
openshot                  1.4.3        01 Jun 2014  http://launchpad.net/openshot/1.4/1.4.3/+download/openshot-1.4.3.tar.gz
v                         2.5.1        03 Mar 2020  https://github.com/OpenShot/openshot-qt/archive/v2.5.1.tar.gz
opensp                    1.5.2        01 Jun 2014  http://ovh.dl.sourceforge.net/sourceforge/openjade/OpenSP-1.5.2.tar.gz
openssh                   9.9p1        20 Sep 2024  https://ftp3.usa.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.9p1.tar.gz
openssl                   3.4.0        24 Nov 2024  https://github.com/openssl/openssl/releases/download/openssl-3.4.0/openssl-3.4.0.tar.gz
openssn                   1.3          01 Dec 2012  http://sourceforge.net/projects/openssn/files/openssn-1.3/openssn-1.3.tar.gz
openttd                   13.1         10 Jun 2023  https://cdn.openttd.org/openttd-releases/13.1/openttd-13.1-source.tar.xz
openvpn                   2.6.13       16 Jan 2025  https://swupdate.openvpn.org/community/releases/openvpn-2.6.13.tar.gz
openvrml                  0.18.9       01 Jun 2014  http://sourceforge.net/projects/openvrml/files/openvrml/0.18.9/openvrml-0.18.9.tar.gz
ophcrack                  3.6.0        01 Jul 2013  http://downloads.sourceforge.net/project/ophcrack/ophcrack/3.6.0/ophcrack-3.6.0.tar.bz2
oprofile                  1.4.0        25 Jul 2020  http://prdownloads.sourceforge.net/oprofile/oprofile-1.4.0.tar.gz
optimist                  3.0.1        06 Jun 2021  https://rubygems.org/downloads/optimist-3.0.1.gem
optipng                   0.7.7        31 Jul 2018  https://sourceforge.net/projects/optipng/files/OptiPNG/optipng-0.7.7/optipng-0.7.7.tar.gz
opus                      1.5.2        14 Apr 2024  https://ftp.osuosl.org/pub/xiph/releases/opus/opus-1.5.2.tar.gz
opusfile                  0.12         01 Jul 2020  https://github.com/xiph/opusfile/releases/download/v0.12/opusfile-0.12.tar.gz
opustools                 0.2          20 Nov 2022  https://ftp.mozilla.org/pub/opus/opus-tools-0.2.tar.gz
orbit2                    2.14.19      01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/ORBit2/2.14/ORBit2-2.14.19.tar.bz2
orc                       0.4.32       16 Dec 2020  https://github.com/GStreamer/orc/archive/orc-0.4.32.tar.gz
orca                      46.2         07 Jul 2024  https://download.gnome.org/sources/orca/46/orca-46.2.tar.xz
ostree                    2014.4       01 Apr 2014  http://ftp.gnome.org/pub/gnome/sources/ostree/2014.4/ostree-2014.4.tar.xz
oxygen                    6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/oxygen-6.2.5.tar.xz
oxygengtk                 1.1.6        01 Aug 2013  http://download.kde.org/stable/oxygen-gtk/1.1.6/src/oxygen-gtk-1.1.6.tar.bz2
oxygenicons               5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/oxygen-icons-5.116.0.tar.xz
oxygensounds              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/oxygen-sounds-6.2.5.tar.xz
p11kit                    0.25.5       07 Jul 2024  https://github.com/p11-glue/p11-kit/releases/download/0.25.5/p11-kit-0.25.5.tar.xz
p7zip                     17.04        17 Aug 2022  https://github.com/jinfeihan57/p7zip/archive/v17.04/p7zip-17.04.tar.gz
packagekit                1.1.11       05 Oct 2018  https://www.freedesktop.org/software/PackageKit/releases/PackageKit-1.1.11.tar.xz
packaging                 24.0         19 Mar 2024  https://files.pythonhosted.org/packages/ee/b5/b43a27ac7472e1818c4bafd44430e69605baefe1f34440593e0332ec8b4d/packaging-24.0.tar.gz
pacman                    v7.0.0       14 Jul 2024  https://gitlab.archlinux.org/pacman/pacman/-/archive/v7.0.0/pacman-v7.0.0.tar.bz2
padrino                   0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-0.15.1.gem
padrinoadmin              0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-admin-0.15.1.gem
padrinocache              0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-cache-0.15.1.gem
padrinocore               0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-core-0.15.1.gem
padrinogen                0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-gen-0.15.1.gem
padrinohelpers            0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-helpers-0.15.1.gem
padrinomailer             0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-mailer-0.15.1.gem
padrinosupport            0.15.1       27 May 2022  https://rubygems.org/downloads/padrino-support-0.15.1.gem
paint                     2.3.0        17 Nov 2022  https://rubygems.org/downloads/paint-2.3.0.gem
paintown                  3.6.0        25 Sep 2019  https://github.com/kazzmir/paintown/releases/download/v3.6.0/paintown-3.6.0.tar.bz2
palapeli                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/palapeli-23.08.5.tar.xz
paludis                   2.6.0        18 Jul 2017  http://paludis.exherbo.org/download/paludis-2.6.0.tar.bz2
panda3d                   1.8.0        01 Dec 2012  http://www.panda3d.org/download/panda3d-1.8.0/panda3d-1.8.0.tar.gz
pandas                    1.4.3        26 Jun 2022  https://files.pythonhosted.org/packages/f4/00/2de395c769335956b8650f990ef2a15e860be83b544c408ff95713446329/pandas-1.4.3.tar.gz
pandoc                    2.7.2        14 Apr 2019  https://hackage.haskell.org/package/pandoc-2.7.2/pandoc-2.7.2.tar.gz
pandora                   0.1.2        01 Jun 2014  https://rubygems.org/downloads/pandora-0.1.2.gem
pango                     1.56.1       20 Jan 2025  https://download.gnome.org/sources/pango/1.56/pango-1.56.1.tar.xz
pangomm                   2.56.1       16 Jan 2025  https://download.gnome.org/sources/pangomm/2.56/pangomm-2.56.1.tar.xz
pangzero                  1.3          01 Dec 2012  http://sourceforge.net/projects/pangzero/files/pangzero/1.3/pangzero-1.3.tar.gz
papyrus                   0.13.3       28 Mar 2016  https://sourceforge.net/projects/libpapyrus/files/papyrus/0.13.3/papyrus-0.13.3.tar.gz
paragui                   05.04.2024   05 Apr 2024  https://github.com/pipelka/paragui/paragui-05.04.2024
parallel                  20240922     24 Sep 2024  https://ftp.gnu.org/gnu/parallel/parallel-20240922.tar.bz2
paratim                   0.2.0.1      05 Apr 2024  https://sourceforge.net/projects/paratim/files/paratim/paratim-0.2.0/paratim-0.2.0.1.tar.bz2
parley                    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/parley-23.08.5.tar.xz
parole                    4.18.0       10 Jun 2023  https://archive.xfce.org/src/apps/parole/4.18/parole-4.18.0.tar.bz2
parser                    3.1.2.0      27 May 2022  https://rubygems.org/downloads/parser-3.1.2.0.gem
parsetree                 3.0.9        01 Aug 2017  https://rubygems.org/downloads/ParseTree-3.0.9.gem
parseyapp                 1.21         07 Aug 2017  https://cpan.metacpan.org/authors/id/W/WB/WBRASWELL/Parse-Yapp-1.21.tar.gz
parted                    3.6          11 Apr 2023  https://ftp.gnu.org/gnu/parted/parted-3.6.tar.xz
partimage                 0.6.9        28 Mar 2016  https://sourceforge.net/projects/partimage/files/stable/0.6.9/partimage-0.6.9.tar.bz2
partitionmanager          23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/partitionmanager-23.08.5.tar.xz
passenger                 6.0.8        15 Apr 2021  https://rubygems.org/downloads/passenger-6.0.8.gem
patch                     2.7.6        06 Feb 2018  https://ftp.gnu.org/pub/gnu/patch/patch-2.7.6.tar.xz
patchelf                  0.18.0       26 Apr 2023  https://github.com/NixOS/patchelf/releases/download/0.18.0/patchelf-0.18.0.tar.gz
patchutils                0.4.2        21 Jul 2020  http://cyberelk.net/tim/data/patchutils/stable/patchutils-0.4.2.tar.xz
pathexpander              1.1.0        28 May 2020  https://rubygems.org/downloads/path_expander-1.1.0.gem
pathlib                   1.0.1        27 Dec 2017  https://pypi.python.org/packages/ac/aa/9b065a76b9af472437a0059f77e8f962fe350438b927cb80184c32f075eb/pathlib-1.0.1.tar.gz
pathlib2                  2.3.5        27 Oct 2019  https://files.pythonhosted.org/packages/94/d8/65c86584e7e97ef824a1845c72bbe95d79f5b306364fa778a3c3e401b309/pathlib2-2.3.5.tar.gz
pathspec                  0.12.1       16 Apr 2024  https://files.pythonhosted.org/packages/ca/bc/f35b8446f4531a7cb215605d100cd88b7ac6f44ab3fc94870c120ab3adbf/pathspec-0.12.1.tar.gz
pavucontrol               5.0          27 Aug 2021  https://freedesktop.org/software/pulseaudio/pavucontrol/pavucontrol-5.0.tar.xz
pavucontrolqt             0.16.0       07 Nov 2020  https://github.com/lxqt/pavucontrol-qt/releases/download/0.16.0/pavucontrol-qt-0.16.0.tar.xz
pawm                      2.3.0        01 Jun 2014  http://www.pleyades.net/david/projects/pawm/pawm-2.3.0.tar.bz2
paxutils                  1.3.6        25 Jan 2023  https://gitweb.gentoo.org/proj/pax-utils.git/snapshot/pax-utils-1.3.6.tar.gz
v                         1.6.0        22 Jan 2025  https://github.com/PacificBiosciences/pbbam/archive/v1.6.0.tar.gz
pbzip2                    1.1.13       21 Sep 2017  https://launchpad.net/pbzip2/1.1/1.1.13/+download/pbzip2-1.1.13.tar.gz
pciutils                  3.13.0       11 Jun 2024  https://mj.ucw.cz/download/linux/pci/pciutils-3.13.0.tar.gz
pcmanfmqt                 0.16.0       07 Nov 2020  https://github.com/lxqt/pcmanfm-qt/releases/download/0.16.0/pcmanfm-qt-0.16.0.tar.xz
pcmciautils               014          01 May 2012  http://kernel.org/pub/linux/utils/kernel/pcmcia/pcmciautils-014.tar.bz2
pcre                      8.45         16 Jun 2021  ftp://ftp.pcre.org/pub/pcre/pcre-8.45.tar.bz2
pcre2                     10.44        11 Jun 2024  https://github.com/PhilipHazel/pcre2/releases/download/pcre2-10.44/pcre2-10.44.tar.bz2
pcsclite                  1.8.18       11 Aug 2016  https://alioth.debian.org/frs/download.php/file/4173/pcsc-lite-1.8.18.tar.bz2
pdal                      1.5.0        21 Sep 2017  http://download.osgeo.org/pdal/PDAL-1.5.0.tar.gz
pdfcore                   0.9.0        15 Apr 2021  https://rubygems.org/downloads/pdf-core-0.9.0.gem
pdfcrack                  0.19         19 Mar 2021  https://sourceforge.net/projects/pdfcrack/files/pdfcrack/pdfcrack-0.19/pdfcrack-0.19.tar.gz
pdfedit                   0.4.5        01 Jun 2014  http://sourceforge.net/projects/pdfedit/files/pdfedit/0.4.5/pdfedit-0.4.5.tar.bz2
pdfgrep                   2.0.1        25 Mar 2017  https://pdfgrep.org/download/pdfgrep-2.0.1.tar.gz
pdfpc                     4.3.0        22 Dec 2018  https://github.com/pdfpc/pdfpc/archive/pdfpc-4.3.0.tar.gz
pdfreader                 2.5.0        13 Jun 2021  https://rubygems.org/downloads/pdf-reader-2.5.0.gem
pdftk                     2.02         10 May 2015  https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/pdftk-2.02.zip
pdl                       2.016        29 Jun 2016  https://sourceforge.net/projects/pdl/files/PDL-2.016/PDL-2.016.tar.gz/
peewee                    3.8.0        18 Dec 2018  https://files.pythonhosted.org/packages/b2/38/fe304c84dd7b771e6c313e2905d54944b9577b212e3247cfa58fac2ac935/peewee-3.8.0.tar.gz
peg                       0.1.18       30 Dec 2018  http://piumarta.com/software/peg/peg-0.1.18.tar.gz
pekwm                     0.1.17       01 Jan 2014  https://www.pekwm.org/projects/pekwm/files/pekwm-0.1.17.tar.bz2
penguincommand            1.6.11       01 Jan 2013  http://user.cs.tu-berlin.de/~karlb/penguin-command/penguincommand-1.6.11.tar.gz
pengupop                  2.1.9        01 Jun 2014  http://www.junoplay.com/files/pengupop-2.1.9.tar.gz
perl                      5.40.1       19 Jan 2025  https://www.cpan.org/src/5.0/perl-5.40.1.tar.xz
gettext                   1.07         05 Jun 2020  https://cpan.metacpan.org/authors/id/P/PV/PVANDRY/gettext-1.07.tar.gz
pg                        1.4.6        27 Feb 2023  https://rubygems.org/downloads/pg-1.4.6.gem
phodav                    3.0          02 Jul 2022  https://download.gnome.org/sources/phodav/3.0/phodav-3.0.tar.xz
phonon                    4.12.0       04 Nov 2023  https://download.kde.org/stable/phonon/4.12.0/phonon-4.12.0.tar.xz
phononbackendgstreamer    4.9.0        13 Oct 2017  http://download.kde.org/stable/phonon/phonon-backend-gstreamer/4.9.0/phonon-backend-gstreamer-4.9.0.tar.xz
phononbackendvlc          0.11.2       07 Feb 2021  https://download.kde.org/stable/phonon/phonon-backend-vlc/0.11.2/phonon-backend-vlc-0.11.2.tar.xz
php                       8.4.3        18 Jan 2025  https://www.php.net/distributions/php-8.4.3.tar.xz
phpbb                     3.2.3        30 Sep 2018  https://www.phpbb.com/files/release/phpBB-3.2.3.zip
phpunit                   6.3          24 Sep 2017  https://phar.phpunit.de/phpunit-6.3.phar
phpwiki                   1.5.5        02 Aug 2017  https://sourceforge.net/projects/phpwiki/files/PhpWiki%201.5%20%28current%29/phpwiki-1.5.5.zip
phylip                    3.68         01 Jun 2014  http://evolution.gs.washington.edu/phylip/download/phylip-3.68.tar.gz
physfs                    3.0.2        27 Mar 2024  https://icculus.org/physfs/downloads/physfs-3.0.2.tar.bz2
picmi                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/picmi-23.08.5.tar.xz
pidgin                    2.14.13      23 Feb 2024  https://downloads.sourceforge.net/pidgin/pidgin-2.14.13.tar.bz2
pigz                      2.4          10 Aug 2020  https://zlib.net/pigz/pigz-2.4.tar.gz
pikepdf                   7.1.1        21 Feb 2023  https://files.pythonhosted.org/packages/bb/22/87185add91a015aaebe82a8f96c60c948a00f1ecf6e750b34cb4c368f162/pikepdf-7.1.1.tar.gz
pillow                    9.4.0        21 Mar 2023  https://files.pythonhosted.org/packages/bc/07/830784e061fb94d67649f3e438ff63cfb902dec6d48ac75aeaaac7c7c30e/Pillow-9.4.0.tar.gz
pimcommon                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/pimcommon-23.08.5.tar.xz
pimdataexporter           23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/pim-data-exporter-23.08.5.tar.xz
pimsieveeditor            23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/pim-sieve-editor-23.08.5.tar.xz
pine                      4.64         01 Jun 2014  ftp://ftp.cac.washington.edu/pine/pine.tar.bz2/pine-4.64.tar.xz
pinentry                  1.3.0        19 Mar 2024  https://www.gnupg.org/ftp/gcrypt/pinentry/pinentry-1.3.0.tar.bz2
pinfo                     0.6.8        01 Jun 2014  http://freshmeat.net/redir/pinfo/8107/url_tgz/pinfo-0.6.8.tar.gz
pingus                    0.7.6        01 Jul 2013  http://pingus.googlecode.com/files/pingus-0.7.6.tar.bz2
pinpoint                  0.1.8        23 Sep 2015  http://ftp.gnome.org/pub/GNOME/sources/pinpoint/0.1/pinpoint-0.1.8.tar.xz
pinta                     2.1.2        25 Oct 2024  https://github.com/PintaProject/Pinta/releases/download/2.1.2/pinta-2.1.2.tar.gz
pioneers                  15.4         01 Nov 2017  https://sourceforge.net/projects/pio/files/Source/pioneers-15.4.tar.gz
pip                       24.3.1       24 Nov 2024  https://files.pythonhosted.org/packages/f4/b1/b422acd212ad7eedddaf7981eee6e5de085154ff726459cf2da7c5a184c1/pip-24.3.1.tar.gz
pipenv                    2018.11.26   12 Feb 2019  https://files.pythonhosted.org/packages/fd/e9/01822318551caa0d62a181ba3b10f0f3757bb1e270da97165bd52db92776/pipenv-2018.11.26.tar.gz
showfilesphp?group        id=198283    01 Dec 2012  http://sourceforge.net/project/showfiles.php?group_id=198283
pipewire                  1.2.7        26 Nov 2024  https://gitlab.freedesktop.org/pipewire/pipewire/-/archive/1.2.7/pipewire-1.2.7.tar.bz2
pitivi                    2023.03      27 Mar 2023  https://download.gnome.org/sources/pitivi/2023/pitivi-2023.03.tar.xz
pixman                    0.44.2       04 Dec 2024  https://www.cairographics.org/releases/pixman-0.44.2.tar.gz
pjproject                 1.14.2       01 Jun 2014  http://www.pjsip.org/release/1.14.2/pjproject-1.14.2.tar.bz2
pkgconfig                 0.29.2       26 Mar 2017  https://pkgconfig.freedesktop.org/releases/pkg-config-0.29.2.tar.gz
pl                        6.2.2        01 Jun 2014  http://www.swi-prolog.org/download/stable/src/pl-6.2.2.tar.gz
plankplayer               5.27.9       24 Oct 2023  https://download.kde.org/stable/plasma/5.27.9/plank-player-5.27.9.tar.xz
plasma5support            6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma5support-6.2.5.tar.xz
plasmaactivities          6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-activities-6.2.5.tar.xz
plasmaactivitiesstats     6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-activities-stats-6.2.5.tar.xz
plasmabigscreen           5.27.9       24 Oct 2023  https://download.kde.org/stable/plasma/5.27.9/plasma-bigscreen-5.27.9.tar.xz
plasmabrowserintegration  6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-browser-integration-6.2.5.tar.xz
plasmadesktop             6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-desktop-6.2.5.tar.xz
plasmadialer              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-dialer-6.2.5.tar.xz
plasmadisks               6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-disks-6.2.5.tar.xz
plasmafirewall            6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-firewall-6.2.5.tar.xz
plasmaframework           5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/plasma-framework-5.116.0.tar.xz
plasmaintegration         6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-integration-6.2.5.tar.xz
plasmamobile              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-mobile-6.2.5.tar.xz
plasmanano                6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-nano-6.2.5.tar.xz
plasmanm                  6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-nm-6.2.5.tar.xz
plasmapa                  6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-pa-6.2.5.tar.xz
plasmaphonecomponents     5.24.0       08 Feb 2022  https://download.kde.org/stable/plasma/5.24.0/plasma-phone-components-5.24.0.tar.xz
plasmaremotecontrollers   5.27.9       24 Oct 2023  https://download.kde.org/stable/plasma/5.27.9/plasma-remotecontrollers-5.27.9.tar.xz
plasmasdk                 6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-sdk-6.2.5.tar.xz
plasmasystemmonitor       6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-systemmonitor-6.2.5.tar.xz
plasmatests               5.27.3       23 Mar 2023  https://download.kde.org/stable/plasma/5.27.3/plasma-tests-5.27.3.tar.xz
plasmathunderbolt         6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-thunderbolt-6.2.5.tar.xz
plasmatube                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/plasmatube-23.08.5.tar.xz
plasmavault               6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-vault-6.2.5.tar.xz
plasmawaylandprotocols    1.14.0       16 Sep 2024  https://download.kde.org/stable/plasma-wayland-protocols/plasma-wayland-protocols-1.14.0.tar.xz
plasmawelcome             6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-welcome-6.2.5.tar.xz
plasmaworkspace           6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-workspace-6.2.5.tar.xz
plasmaworkspacewallpapers 6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plasma-workspace-wallpapers-6.2.5.tar.xz
plocate                   1.1.23       04 Dec 2024  https://plocate.sesse.net/download/plocate-1.1.23.tar.gz
pl                        232src       01 Jun 2014  http://ploticus.sourceforge.net/download/pl232src.tar.gz
pluma                     1.28.0       21 Feb 2024  https://pub.mate-desktop.org/releases/1.28/pluma-1.28.0.tar.xz
plumaplugins              1.26.0       20 Aug 2021  https://pub.mate-desktop.org/releases/1.26/pluma-plugins-1.26.0.tar.xz
ply                       3.11         22 Nov 2018  https://files.pythonhosted.org/packages/e5/69/882ee5c9d017149285cab114ebeab373308ef0f874fcdac9beb90e0ac4da/ply-3.11.tar.gz
plymouth                  0.9.4        04 Dec 2018  https://www.freedesktop.org/software/plymouth/releases/plymouth-0.9.4.tar.xz
plymouthkcm               6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/plymouth-kcm-6.2.5.tar.xz
plzip                     1.12         22 Jan 2025  http://download.savannah.gnu.org/releases/lzip/plzip/plzip-1.12.tar.gz
pmacct                    0.14.1       01 Jun 2014  http://www.pmacct.net/pmacct-0.14.1.tar.gz
pmk                       0.10.4       01 Jun 2014  http://prdownloads.sourceforge.net/pmk/pmk-0.10.4.tar.gz
png++                     0.2.9        30 Jul 2016  http://download.savannah.gnu.org/releases/pngpp/png++-0.2.9.tar.gz
pngcrush                  1.7.41       01 Dec 2012  http://sourceforge.net/projects/pmt/files/pngcrush/1.7.41/pngcrush-1.7.41.tar.xz
pngquant                  2.18.0       25 Feb 2023  https://pngquant.org/pngquant-2.18.0.tar.gz
pnmixer                   v0.7.2       10 Sep 2018  https://github.com/nicklan/pnmixer/releases/download/v0.7.2/pnmixer-v0.7.2.tar.gz
podofo                    0.9.8        27 May 2022  https://sourceforge.net/projects/podofo/files/podofo/0.9.8/podofo-0.9.8.tar.gz
poetry                    1.0.0        28 Dec 2019  https://files.pythonhosted.org/packages/59/b9/79e078a303b6b813aa16c1f9cffe22e048c6edfef9a31574670193d09fc4/poetry-1.0.0.tar.gz
poke                      4.1          02 Jun 2024  https://ftp.gnu.org/gnu/poke/poke-4.1.tar.gz
polari                    43.0         12 Oct 2022  https://download.gnome.org/sources/polari/43/polari-43.0.tar.xz
policykit                 0.9          01 Jun 2014  PolicyKit-0.9
polkit                    126          16 Jan 2025  https://github.com/polkit-org/polkit/archive/126/polkit-126.tar.gz
polkitgnome               0.105        01 Jun 2014  ftp://ftp.gnome.org/pub/gnome/sources/polkit-gnome/0.105/polkit-gnome-0.105.tar.xz
polkitkdeagent1           6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/polkit-kde-agent-1-6.2.5.tar.xz
v                         0.114        03 Mar 2015  https://github.com/KDE/polkit-qt-1/archive/refs/tags/v0.114.0.tar.gz
poltergeist               1.18.1       13 Aug 2018  https://rubygems.org/downloads/poltergeist-1.18.1.gem
polyglot                  0.3.5        14 Oct 2018  https://rubygems.org/downloads/polyglot-0.3.5.gem
polylib                   5.22.5       01 Nov 2012  http://icps.u-strasbg.fr/polylib/polylib_src/polylib-5.22.5.tar.gz
poppler                   25.01.0      10 Jan 2025  https://poppler.freedesktop.org/poppler-25.01.0.tar.xz
popplerdata               0.4.12       04 Feb 2023  http://poppler.freedesktop.org/poppler-data-0.4.12.tar.gz
popt                      1.19         18 Sep 2022  http://ftp.rpm.org/popt/releases/popt-1.x/popt-1.19.tar.gz
porcupine                 0.0.7        01 Jun 2014  http://www.innoscript.org/component/option,com_remository/Itemid,33/porcupine-0.0.7
porg                      0.10         01 Sep 2016  https://sourceforge.net/projects/porg/files/porg-0.10.tar.gz
pa                        stable_v190700_20210406 21 Jan 2025  https://files.portaudio.com/archives/pa_stable_v190700_20210406.tgz
postfix                   3.9.0        10 Mar 2024  http://ftp.porcupine.org/mirrors/postfix-release/official/postfix-3.9.0.tar.gz
postgis                   2.3.1        18 Mar 2017  http://download.osgeo.org/postgis/source/postgis-2.3.1.tar.gz
postgresql                17.1         14 Nov 2024  https://ftp.postgresql.org/pub/source/v17.1/postgresql-17.1.tar.bz2
postr                     0.13.1       05 Sep 2014  http://ftp.gnome.org/pub/GNOME/sources/postr/0.13/postr-0.13.1.tar.xz
potrace                   1.16         21 Sep 2019  https://potrace.sourceforge.net/download/1.16/potrace-1.16.tar.gz
povlinux                  3.6          05 Jun 2021  http://www.povray.org/redirect/www.povray.org/ftp/pub/povray/Official/Linux/povlinux-3.6.tgz
powerassert               2.0.3        19 Jun 2023  https://rubygems.org/downloads/power_assert-2.0.3.gem
powerdevil                6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/powerdevil-6.2.5.tar.xz
powermanga                0.93         10 Oct 2017  http://linux.tlk.fr/games/Powermanga/download/powermanga-0.93.tgz
powerpack                 0.1.3        15 Apr 2021  https://rubygems.org/downloads/powerpack-0.1.3.gem
v                         2.15         01 Oct 2022  https://github.com/fenrus75/powertop/archive/refs/tags/v2.15.tar.gz
poxml                     23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/poxml-23.08.5.tar.xz
ppl                       1.2          18 Feb 2016  ftp://ftp.cs.unipr.it/pub/ppl/releases/1.2/ppl-1.2.tar.gz
ppp                       2.5.2        31 Dec 2024  https://www.samba.org/ftp/ppp/ppp-2.5.2.tar.gz
prawn                     2.4.0        15 Apr 2021  https://rubygems.org/downloads/prawn-2.4.0.gem
prefixsuffix              0.6.9        08 Dec 2015  http://ftp.gnome.org/pub/GNOME/sources/prefixsuffix/0.6/prefixsuffix-0.6.9.tar.xz
presentproto              1.1          18 Mar 2017  https://www.x.org/releases/individual/proto/presentproto-1.1.tar.bz2
primer3                   2.3.7        12 Apr 2016  https://sourceforge.net/projects/primer3/files/primer3/2.3.7/primer3-2.3.7.tar.gz
printmanager              6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/print-manager-6.2.5.tar.xz
printproto                1.0.5        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/proto/printproto-1.0.5.tar.bz2
prison                    5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/prison-5.116.0.tar.xz
processing                0111         01 Jun 2014  http://processing.org/download/processing-0111.tgz
procinfong                2.0.304      18 Apr 2019  https://sourceforge.net/projects/procinfo-ng/files/procinfo-ng/2.0.304/procinfo-ng-2.0.304.tar.bz2
procps                    v3.3.15      21 Aug 2019  https://gitlab.com/procps-ng/procps/-/archive/v3.3.15/procps-v3.3.15.tar.bz2
procpsng                  4.0.4        21 Jan 2024  https://sourceforge.net/projects/procps-ng/files/Production/procps-ng-4.0.4.tar.xz
v                         2.6.3        26 Jun 2020  https://github.com/hyattpd/Prodigal/archive/v2.6.3.tar.gz
v                         1.3.7        10 Apr 2017  https://github.com/proftpd/proftpd/archive/v1.3.7a.tar.gz
progsreiserfs             0.3.0.4      01 Jun 2014  http://reiserfs.osdn.org.ua/progsreiserfs-0.3.0.4.tar.gz
protobuf                  28.2         22 Nov 2024  https://github.com/protocolbuffers/protobuf/releases/download/v28.3/protobuf-28.2.tar.gz
protobufcpp               3.21.12      14 Dec 2022  https://github.com/protocolbuffers/protobuf/releases/download/v21.12/protobuf-cpp-3.21.12.tar.gz
protocol                  2.0.0        28 May 2020  https://rubygems.org/downloads/protocol-2.0.0.gem
protoeditor               1.0          09 Feb 2018  https://sourceforge.net/projects/protoeditor/files/protoeditor/1.0/protoeditor-1.0.tar.gz
proxymngr                 1.0.4        18 Apr 2015  http://xorg.freedesktop.org/releases/individual/app/proxymngr-1.0.4.tar.gz
pry                       0.14.1       15 Apr 2021  https://rubygems.org/downloads/pry-0.14.1.gem
prybyebug                 3.9.0        28 May 2020  https://rubygems.org/downloads/pry-byebug-3.9.0.gem
pscpug                    030          01 Jun 2014  http://www.diablonet.net/~mercadal/projects/pscpug/pscpug030.tgz
psftools                  1.0.12       25 Mar 2019  http://www.seasip.info/Unix/PSF/psftools-1.0.12.tar.gz
psmisc                    23.7         17 Mar 2024  https://sourceforge.net/projects/psmisc/files/psmisc/psmisc-23.7.tar.xz
pspp                      2.0.1        22 Mar 2024  https://ftp.gnu.org/pub/gnu/pspp/pspp-2.0.1.tar.gz
pssh                      2.3.1        09 Aug 2020  https://files.pythonhosted.org/packages/60/9a/8035af3a7d3d1617ae2c7c174efa4f154e5bf9c24b36b623413b38be8e4a/pssh-2.3.1.tar.gz
psych                     3.3.1        15 Apr 2021  https://rubygems.org/downloads/psych-3.3.1.gem
pth                       2.0.7        01 Jun 2014  ftp://ftp.gnu.org/gnu/pth/pth-2.0.7.tar.gz
ptlib                     2.10.11      07 Nov 2015  http://ftp.gnome.org/pub/GNOME/sources/ptlib/2.10/ptlib-2.10.11.tar.xz
publicsuffix              4.0.6        15 Apr 2021  https://rubygems.org/downloads/public_suffix-4.0.6.gem
pugixml                   1.11         04 Jan 2021  http://github.com/zeux/pugixml/releases/download/v1.11/pugixml-1.11.tar.gz
pulseaudio                17.0         14 Jan 2024  https://freedesktop.org/software/pulseaudio/releases/pulseaudio-17.0.tar.xz
puma                      5.6.0        16 Sep 2022  https://rubygems.org/downloads/puma-5.6.0.gem
puppet                    7.23.0       28 Feb 2023  https://rubygems.org/downloads/puppet-7.23.0.gem
purpose                   5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/purpose-5.116.0.tar.xz
putty                     0.73         02 Oct 2019  https://the.earth.li/~sgtatham/putty/latest/putty-0.73.tar.gz
py2cairo                  1.10.0       25 Apr 2015  http://cairographics.org/releases/py2cairo-1.10.0.tar.bz2
pyatspi                   2.46.0       22 Sep 2022  https://download.gnome.org/sources/pyatspi/2.46/pyatspi-2.46.0.tar.xz
pybliographer             1.4.0        05 Apr 2018  http://ftp.gnome.org/pub/gnome/sources/pybliographer/1.4/pybliographer-1.4.0.tar.xz
pycairo                   1.27.0       08 Sep 2024  https://github.com/pygobject/pycairo/releases/download/v1.27.0/pycairo-1.27.0.tar.gz
pycdio                    2.1.0        05 Dec 2019  http://ftp.gnu.org/pub/gnu/libcdio/pycdio-2.1.0.tar.gz
pycha                     0.7.0        01 Jun 2014  https://pypi.python.org/packages/source/p/pycha/pycha-0.7.0.tar.gz
pycp                      8.0.6        18 Jan 2019  https://files.pythonhosted.org/packages/fa/2c/b01afb9eacaaeef8c9a733080da62c68545bdbced3c07ae78faad95d3af0/pycp-8.0.6.tar.gz
pycryptodome              3.10.4       28 Sep 2021  https://files.pythonhosted.org/packages/f8/8e/14a8238190bcf1bab3d58432cd795c859edbc2f5abd8460f80438046a799/pycryptodome-3.10.4.tar.gz
pycups                    2.0.1        01 May 2020  https://files.pythonhosted.org/packages/0c/bb/82546806a86dc16f5eeb76f62ffdc42cce3d43aacd4e25a8b5300eec0263/pycups-2.0.1.tar.gz
pycurl                    7.44.1       19 Aug 2021  https://files.pythonhosted.org/packages/47/f9/c41d6830f7bd4e70d5726d26f8564538d08ca3a7ac3db98b325f94cdcb7f/pycurl-7.44.1.tar.gz
pydisplay                 0.2.0        01 Jun 2014  http://downloads.sourceforge.net/pydisplay/pydisplay-0.2.0.tar.gz
pyensembl                 1.9.1        24 Apr 2021  https://files.pythonhosted.org/packages/39/65/0dbcaebd1f376a0907ba625ba86b3aa04b57dff7b026ab41dd062975b7ba/pyensembl-1.9.1.tar.gz
pygame                    1.9.6        20 Sep 2019  https://www.pygame.org/ftp/pygame-1.9.6.tar.gz
pyglet                    1.0          01 Nov 2012  http://pyglet.googlecode.com/files/pyglet-1.0.tar.gz
pygments                  2.19.1       18 Jan 2025  https://files.pythonhosted.org/packages/7c/2d/c3338d48ea6cc0feb8446d8e6937e1408088a72a39937982cc6111d17f84/pygments-2.19.1.tar.gz
pygobject                 3.48.2       06 Apr 2024  https://download.gnome.org/sources/pygobject/3.48/pygobject-3.48.2.tar.xz
pygraphviz                0.22         01 Jun 2014  http://surfnet.dl.sourceforge.net/sourceforge/networkx/pygraphviz-0.22.tar.gz
pygtksourceview           2.10.1       12 May 2019  http://ftp.gnome.org/pub/gnome/sources/pygtksourceview/2.10/pygtksourceview-2.10.1.tar.gz
pygui                     2.2          01 Jun 2014  http://www.cosc.canterbury.ac.nz/greg.ewing/python_gui/PyGUI-2.2.tar.gz
pymol                     v2.3.4       14 Jan 2019  https://sourceforge.net/projects/pymol/files/pymol/2/pymol-v2.3.4.tar.bz2
pymysql                   0.9.2        05 Dec 2018  https://files.pythonhosted.org/packages/d9/ab/bc9be64876fc9b1590d8ae62b471981b4489c23d6c3f0319af570748d408/PyMySQL-0.9.2.tar.gz
pyorbit                   2.24.0       01 Jun 2014  http://ftp.gnome.org/pub/GNOME/sources/pyorbit/2.24/pyorbit-2.24.0.tar.bz2
pyparsing                 3.1.2        08 Mar 2024  https://files.pythonhosted.org/packages/46/3a/31fd28064d016a2182584d579e033ec95b809d8e220e74c4af6f0f2e8842/pyparsing-3.1.2.tar.gz
pype                      2.9.4        01 Jan 2013  https://sourceforge.net/projects/pype/files/pype/PyPE%202.9.4/PyPE-2.9.4.zip
pypy39                    v7.3.9       28 Aug 2022  https://downloads.python.org/pypy/pypy3.9-v7.3.9.tar.bz2
pyqt5                     5.15.4       12 Jun 2021  https://files.pythonhosted.org/packages/8e/a4/d5e4bf99dd50134c88b95e926d7b81aad2473b47fde5e3e4eac2c69a8942/PyQt5-5.15.4.tar.gz
pyrex                     0.9.9        06 Oct 2017  http://www.cosc.canterbury.ac.nz/greg.ewing/python/Pyrex/Pyrex-0.9.9.tar.gz
showfilesphp?group        id=46487&package_id=39324&release_id=346093 01 Jan 2013  http://sourceforge.net/project/showfiles.php?group_id=46487&package_id=39324&release_id=346093
pysimplegui               4.33.0       14 Jan 2021  https://files.pythonhosted.org/packages/3e/8e/3def3ea64635d5d3fb31022f917cdea50ab2fbc39051b9320d55bb9ebcb8/PySimpleGUI-4.33.0.tar.gz
pysqlite                  2.8.3        27 Nov 2019  https://files.pythonhosted.org/packages/42/02/981b6703e3c83c5b25a829c6e77aad059f9481b0bbacb47e6e8ca12bd731/pysqlite-2.8.3.tar.gz
pytesseract               0.3.8        17 Jul 2021  https://files.pythonhosted.org/packages/a3/c9/d6e8903482bd6fb994c32722831d15842dd8b614f94ad9ca735807252671/pytesseract-0.3.8.tar.gz
pytest                    8.3.3        15 Sep 2024  https://files.pythonhosted.org/packages/8b/6c/62bbd536103af674e227c41a8f3dcd022d591f6eed5facb5a0f31ee33bbc/pytest-8.3.3.tar.gz
python                    3.11.11      15 Jan 2025  https://www.python.org/ftp/python/3.11.11/Python-3.11.11.tar.xz
pythoncaja                1.28.0       23 Feb 2024  https://pub.mate-desktop.org/releases/1.28/python-caja-1.28.0.tar.xz
pythondbusmock            0.28.1       30 Jun 2022  https://github.com/martinpitt/python-dbusmock/releases/download/0.28.1/python-dbusmock-0.28.1.tar.gz
pythonefl                 1.20.0       06 Jul 2018  https://download.enlightenment.org/rel/bindings/python/python-efl-1.20.0.tar.xz
pytone                    3.0.3        01 Jun 2014  http://www.luga.de/pytone/download/PyTone-3.0.3.tar.gz
pywal                     3.3.0        24 May 2020  https://files.pythonhosted.org/packages/57/55/2b694e4f42837653258540f7bbf7206e4fac651960aba69ce4fbb5427875/pywal-3.3.0.tar.gz
pyxdg                     0.27         07 Dec 2020  https://files.pythonhosted.org/packages/6f/2e/2251b5ae2f003d865beef79c8fcd517e907ed6a69f58c32403cec3eba9b2/pyxdg-0.27.tar.gz
pyxml                     0.8.4        01 Jun 2014  http://mesh.dl.sourceforge.net/sourceforge/pyxml/PyXML-0.8.4.tar.gz
pyyaml                    6.0.2        22 Sep 2024  https://files.pythonhosted.org/packages/54/ed/79a089b6be93607fa5cdaedf301d7dfb23af5f25c398d5ead2525b063e17/pyyaml-6.0.2.tar.gz
qalculategtk              3.7.0        29 Feb 2020  https://github.com/Qalculate/qalculate-gtk/releases/download/v3.7.0/qalculate-gtk-3.7.0.tar.gz
qbittorrent               5.0.3        18 Dec 2024  https://downloads.sourceforge.net/qbittorrent/qbittorrent-5.0.3.tar.xz
qca                       2.3.9        01 Oct 2024  https://download.kde.org/stable/qca/2.3.9/qca-2.3.9.tar.xz
qcaqt5                    2.1.0.3      22 Sep 2017  https://download.kde.org/stable/qca-qt5/2.1.0.3/src/qca-qt5-2.1.0.3.tar.xz
qdbm                      1.8.77       12 Sep 2015  http://sourceforge.net/projects/qdbm/files/qdbm/1.8.77/qdbm-1.8.77.tar.gz
qdvdauthor                2.3.1        01 Sep 2014  http://sourceforge.net/projects/qdvdauthor/files/qdvdauthor/2.3.1/qdvdauthor-2.3.1.tar.gz
qemu                      9.2.0        11 Dec 2024  https://download.qemu.org/qemu-9.2.0.tar.xz
qgis                      2.18.13      20 Sep 2017  http://qgis.org/downloads/qgis-2.18.13.tar.bz2
qimageblitz               0.0.6        01 Jan 2014  http://download.kde.org/stable/qimageblitz/qimageblitz-0.0.6.tar.bz2
v                         0.8.4        25 Sep 2019  https://github.com/easymodo/qimgv/archive/v0.8.4.tar.gz
qjackctl                  0.3.11       01 Apr 2014  https://downloads.sourceforge.net/qjackctl/qjackctl-0.3.11.tar.gz
qmlkonsole                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/qmlkonsole-23.08.5.tar.xz
qpdf                      11.9.1       11 Jun 2024  https://github.com/qpdf/qpdf/releases/download/v11.9.1/qpdf-11.9.1.tar.gz
qps                       2.2.0        07 Nov 2020  https://github.com/lxqt/qps/releases/download/2.2.0/qps-2.2.0.tar.xz
qqc2breezestyle           6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/qqc2-breeze-style-6.2.5.tar.xz
qqc2desktopstyle          5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/qqc2-desktop-style-5.116.0.tar.xz
v                         1.3.4        09 Aug 2015  https://github.com/stephenostermiller/qqwing/archive/v1.3.4.zip
qrencode                  4.1.1        22 Dec 2021  https://fukuchi.org/works/qrencode/qrencode-4.1.1.tar.bz2
qsynth                    0.5.5        16 Mar 2019  https://downloads.sourceforge.net/qsynth/qsynth-0.5.5.tar.gz
qteverywhere              6.8.1        02 Dec 2024  https://download.qt.io/archive/qt/6.8/6.8.1/single/qt-everywhere-6.8.1.tar.xz
qterminal                 1.1.0        30 Aug 2022  https://github.com/lxqt/qterminal/releases/download/1.1.0/qterminal-1.1.0.tar.xz
qtermwidget               1.4.0        02 Feb 2024  https://github.com/lxqt/qtermwidget/releases/download/1.4.0/qtermwidget-1.4.0.tar.xz
qtgstreamer               1.2.0        10 Jul 2015  http://gstreamer.freedesktop.org/src/qt-gstreamer/qt-gstreamer-1.2.0.tar.xz
qtwebengine               5.15.13      10 Mar 2023  https://anduin.linuxfromscratch.org/BLFS/qtwebengine/qtwebengine-5.15.13.tar.xz
quadkonsole               2.0.3        01 Jun 2014  http://nomis80.org/quadkonsole/quadkonsole-2.0.3.tar.bz2
quadrapassel              40.2         11 Jun 2021  https://download.gnome.org/sources/quadrapassel/40/quadrapassel-40.2.tar.xz
quagga                    1.2.2        02 Jan 2018  http://download.savannah.gnu.org/releases/quagga/quagga-1.2.2.tar.gz
v                         1.3          24 Apr 2022  https://github.com/stachenov/quazip/archive/refs/tags/v1.3.tar.gz
quelcom                   0.4.0        01 Nov 2012  http://www.etse.urv.es/~dmanye/quelcom/src/quelcom-0.4.0.tar.gz
quodlibet                 3.9.1        28 Jul 2017  https://github.com/quodlibet/quodlibet/releases/download/release-3.9.1/quodlibet-3.9.1.tar.gz
quota                     4.05         01 Apr 2019  https://sourceforge.net/projects/linuxquota/files/quota-tools/4.05/quota-4.05.tar.gz
quvi                      0.9.5        09 Jun 2016  https://sourceforge.net/projects/quvi/files/0.9/quvi/quvi-0.9.5.tar.xz
qwt                       6.0.2        01 Dec 2012  http://sourceforge.net/projects/qwt/files/qwt/6.0.2/qwt-6.0.2.tar.bz2
r                         4.4.2        15 Jan 2025  https://cran.r-project.org/src/base/R-4/R-4.4.2.tar.xz
rabbit                    3.0.0        28 May 2020  https://rubygems.org/downloads/rabbit-3.0.0.gem
racc                      1.5.2        15 Apr 2021  https://rubygems.org/downloads/racc-1.5.2.gem
rack                      3.0.7        06 Apr 2023  https://rubygems.org/downloads/rack-3.0.7.gem
rackaccept                0.4.5        04 Aug 2018  https://rubygems.org/downloads/rack-accept-0.4.5.gem
rackcomponent             0.5.0        18 Feb 2019  https://rubygems.org/downloads/rack-component-0.5.0.gem
rackprotection            2.1.0        15 Apr 2021  https://rubygems.org/downloads/rack-protection-2.1.0.gem
racktest                  1.1.0        13 Oct 2018  https://rubygems.org/downloads/rack-test-1.1.0.gem
rackup                    2.1.0        17 Sep 2024  https://rubygems.org/downloads/rackup-2.1.0.gem
rage                      0.4.0        08 Feb 2022  https://download.enlightenment.org/rel/apps/rage/rage-0.4.0.tar.xz
ragel                     6.10         27 Mar 2017  http://www.colm.net/files/ragel/ragel-6.10.tar.gz
rails                     7.0.1        05 Feb 2022  https://rubygems.org/downloads/rails-7.0.1.gem
railsdomtesting           2.0.3        13 Oct 2018  https://rubygems.org/downloads/rails-dom-testing-2.0.3.gem
railshtmlsanitizer        1.3.0        28 May 2020  https://rubygems.org/downloads/rails-html-sanitizer-1.3.0.gem
railties                  6.1.3.1      15 Apr 2021  https://rubygems.org/downloads/railties-6.1.3.1.gem
rainbow                   3.0.0        09 Jan 2018  https://rubygems.org/downloads/rainbow-3.0.0.gem
rak                       1.0.21       01 Jun 2014  rak-1.0.21.tar.xz
rake                      13.0.6       24 Sep 2022  https://rubygems.org/downloads/rake-13.0.6.gem
randrproto                1.5.0        17 May 2015  https://xorg.freedesktop.org/releases/individual/proto/randrproto-1.5.0.tar.gz
rant                      0.5.7        01 Dec 2017  https://rubygems.org/downloads/rant-0.5.7.gem
raptor2                   2.0.16       04 Mar 2023  https://download.librdf.org/source/raptor2-2.0.16.tar.gz
rarian                    0.8.1        01 Jan 2013  http://ftp.gnome.org/pub/gnome/sources/rarian/0.8/rarian-0.8.1.tar.bz2
rasmol                    2.7.5.2      01 Jun 2014  http://www.rasmol.org/software/RasMol_2.7.5.2.tar.gz
rasqal                    0.9.30       01 Jan 2014  http://download.librdf.org/source/rasqal-0.9.30.tar.gz
ratpoison                 1.4.9        31 Jul 2018  http://download.savannah.nongnu.org/releases/ratpoison/ratpoison-1.4.9.tar.xz
rawrec                    0.9.991      01 Jun 2014  https://sourceforge.net/projects/rawrec/files/rawrec/rawrec-0.9.991/rawrec-0.9.991.tar.gz
rbcat                     0.2.1        25 Nov 2019  https://rubygems.org/downloads/rbcat-0.2.1.gem
rcairo                    1.17.13      30 Jan 2024  https://www.cairographics.org/releases/rcairo-1.17.13.tar.gz
rcov                      1.0.0        31 Oct 2017  https://rubygems.org/downloads/rcov-1.0.0.gem
rdesktop                  1.9.0        12 Oct 2019  https://github.com/rdesktop/rdesktop/releases/download/v1.9.0/rdesktop-1.9.0.tar.gz
rdtool                    0.6.38       17 Jan 2019  https://rubygems.org/downloads/rdtool-0.6.38.gem
re2c                      4.0.2        13 Dec 2024  https://github.com/skvadrik/re2c/releases/download/4.0.2/re2c-4.0.2.tar.xz
readedid                  2.0.0        01 Jun 2014  http://polypux.org/projects/read-edid/read-edid-2.0.0.tar.gz
readline                  8.2          27 Sep 2022  https://ftp.gnu.org/pub/gnu/readline/readline-8.2.tar.gz
recode                    3.7.1        12 May 2019  https://github.com/rrthomas/recode/releases/download/v3.7.1/recode-3.7.1.tar.gz
recordmydesktop           0.3.8.1      01 Jun 2014  http://sourceforge.net/projects/recordmydesktop/files/recordmydesktop/0.3.8.1/recordmydesktop-0.3.8.1.tar.gz
recordproto               1.14.2       01 Jun 2014  http://xorg.freedesktop.org/releases/individual/proto/recordproto-1.14.2.tar.bz2
recutils                  1.9          17 Apr 2022  https://ftp.gnu.org/pub/gnu/recutils/recutils-1.9.tar.gz
redcloth                  4.3.2        21 Sep 2017  https://rubygems.org/downloads/RedCloth-4.3.2.gem
redis                     5.0.7        14 Oct 2023  https://rubygems.org/downloads/redis-5.0.7.gem
redland                   1.0.17       01 Aug 2015  http://download.librdf.org/source/redland-1.0.17.tar.gz
redmine                   3.4.10       23 Apr 2019  http://www.redmine.org/releases/redmine-3.4.10.tar.gz
reft                      0.11         01 Jun 2014  http://www.rafkind.com/jon/tar/reft-0.11.tar.gz
reiserfsprogs             3.6.27       03 Aug 2017  https://www.kernel.org/pub/linux/kernel/people/jeffm/reiserfsprogs/v3.6.27/reiserfsprogs-3.6.27.tar.xz
rendercheck               1.6          07 Jul 2024  https://xorg.freedesktop.org/releases/individual/app/rendercheck-1.6.tar.bz2
renderproto               0.11         01 Jun 2014  http://xorg.freedesktop.org/releases/individual/proto/renderproto-0.11.tar.bz2
reportbuilder             1.9          17 Jun 2020  https://rubygems.org/downloads/report_builder-1.9.gem
reportlab                 1_20         01 Jun 2014  http://www.reportlab.org/ftp/ReportLab_1_20.tgz
requests                  2.29.0       28 Apr 2023  https://files.pythonhosted.org/packages/4c/d2/70fc708727b62d55bc24e43cc85f073039023212d482553d853c44e57bdb/requests-2.29.0.tar.gz
rerun                     0.14.0       02 Apr 2024  https://rubygems.org/downloads/rerun-0.14.0.gem
resmgr                    1.0          01 Jun 2014  ftp://ftp.lst.de/pub/people/okir/resmgr/resmgr-1.0.tar.bz2
resourceproto             1.2.0        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/proto/resourceproto-1.2.0.tar.bz2
rest                      0.9.1        19 Jun 2022  https://download.gnome.org/sources/rest/0.9/rest-0.9.1.tar.xz
restclient                2.1.0        28 May 2020  https://rubygems.org/downloads/rest-client-2.1.0.gem
retrogtk                  1.0.1        30 Nov 2020  https://download.gnome.org/sources/retro-gtk/1.0/retro-gtk-1.0.1.tar.xz
rex                       1.4.0        09 Mar 2016  http://www.cpan.org/authors/id/F/FE/FERKI/Rex-1.4.0.tar.gz
rexml                     3.2.5        05 May 2021  https://rubygems.org/downloads/rexml-3.2.5.gem
rezound                   0.13.1beta   31 Oct 2017  https://sourceforge.net/projects/rezound/files/ReZound/0.13.1beta/rezound-0.13.1beta.tar.gz
rfkill                    0.5          24 Oct 2018  https://mirrors.edge.kernel.org/pub/software/network/rfkill/rfkill-0.5.tar.xz
rfpdf                     153g         01 Jun 2014  http://zeropluszero.com/software/fpdf/rfpdf153g.tar.gz
rgb                       1.1.0        31 Oct 2022  https://xorg.freedesktop.org/releases/individual/app/rgb-1.1.0.tar.xz
rhythmbox                 3.4.7        16 Apr 2023  https://download.gnome.org/sources/rhythmbox/3.4/rhythmbox-3.4.7.tar.xz
rili                      2.0.1        28 May 2015  https://sourceforge.net/projects/ri-li/files/Ri-li%20Linux_Unix/Ri-li%20V2.0.1/Ri-li-2.0.1.tar.bz2
rinruby                   2.1.0        19 Aug 2018  https://rubygems.org/downloads/rinruby-2.1.0.gem
ripoff                    0.8.3        01 Jan 2013  https://sourceforge.net/projects/ripoffc/files/RipOff/0.8.3/ripoff-0.8.3.tar.gz
rison                     2.1.0        01 Dec 2017  https://rubygems.org/downloads/rison-2.1.0.gem
ristretto                 0.13.1       17 May 2023  https://archive.xfce.org/src/apps/ristretto/0.13/ristretto-0.13.1.tar.bz2
rkward                    0.7.1        23 Feb 2020  https://download.kde.org/stable/rkward/0.7.1/src/rkward-0.7.1.tar.gz
rlog                      1.3.7        01 Jun 2014  http://www.arg0.net/rlog/rlog-1.3.7.tar.gz
rlwrap                    0.37         01 Jun 2014  http://utopia.knoware.nl/~hlub/uck/rlwrap/rlwrap-0.37.tar.gz
rmagick                   4.2.2        15 Apr 2021  https://rubygems.org/downloads/rmagick-4.2.2.gem
rman                      3.2          01 Jun 2014  http://switch.dl.sourceforge.net/sourceforge/polyglotman/rman-3.2.tar.gz
rmovie                    0.5.1        01 Dec 2017  https://rubygems.org/downloads/rmovie-0.5.1.gem
rnalila                   0.2          26 Jan 2018  https://github.com/mtw/RNAlila/releases/RNAlila-0.2.tar.xz
rnaz                      2.1          29 Jun 2017  https://www.tbi.univie.ac.at/software/RNAz/RNAz-2.1.tar.gz
rocs                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/rocs-23.08.5.tar.xz
roo                       2.8.3        28 May 2020  https://rubygems.org/downloads/roo-2.8.3.gem
rope                      0.11.0       24 Dec 2018  https://files.pythonhosted.org/packages/52/8d/2ebe70e55742a46813952493f8fc86ff2800ccc105e2dfcb25298f7eeb72/rope-0.11.0.tar.gz
rosegarden                20.12        10 Jan 2021  https://sourceforge.net/projects/rosegarden/files/rosegarden/20.12/rosegarden-20.12.tar.bz2
rouge                     3.26.1       26 Sep 2021  https://rubygems.org/downloads/rouge-3.26.1.gem
roxclib                   2.1.10       21 Feb 2016  http://www.kerofin.demon.co.uk/rox/ROX-CLib-2.1.10.tar.gz
showfilesphp?group        id=7023      01 Jun 2014  http://sourceforge.net/project/showfiles.php?group_id=7023
rpcbind                   1.2.7        27 Jul 2024  https://sourceforge.net/projects/rpcbind/files/rpcbind/1.2.7/rpcbind-1.2.7.tar.bz2
rpcsvcproto               1.4.2        01 Jul 2020  https://github.com/thkukuk/rpcsvc-proto/releases/download/v1.4.2/rpcsvc-proto-1.4.2.tar.xz
rpm                       4.18.0       07 Nov 2022  http://ftp.rpm.org/releases/rpm-4.18.x/rpm-4.18.0.tar.bz2
rrdtool                   1.2.11       01 Jun 2014  http://people.ee.ethz.ch/~oetiker/webtools/rrdtool/pub/rrdtool-1.2.11.tar.gz
rspec                     3.10.0       15 Apr 2021  https://rubygems.org/downloads/rspec-3.10.0.gem
rspeccore                 3.10.1       15 Apr 2021  https://rubygems.org/downloads/rspec-core-3.10.1.gem
rspecexpectations         3.10.1       15 Apr 2021  https://rubygems.org/downloads/rspec-expectations-3.10.1.gem
rspecsupport              3.10.2       15 Apr 2021  https://rubygems.org/downloads/rspec-support-3.10.2.gem
rst2html5                 2.0.1        02 Mar 2024  https://files.pythonhosted.org/packages/bb/82/a85a9664cced2dce7b9d8895f6cace6c2501072a320342d7edcb6d7ce295/rst2html5-2.0.1.tar.gz
rstart                    1.0.6        04 Apr 2022  https://www.x.org/releases/individual/app/rstart-1.0.6.tar.xz
rsync                     3.4.1        16 Jan 2025  https://rsync.samba.org/ftp/rsync/rsync-3.4.1.tar.gz
rtorrent                  01.10.2017   30 Sep 2017  https://github.com/rakshasa/rtorrent-01.10.2017
rtranscoder               0.1.3        01 Dec 2017  https://rubygems.org/downloads/rtranscoder-0.1.3.gem
ruamelyaml                0.17.9       13 Jun 2021  https://files.pythonhosted.org/packages/ea/7f/4bcd7276603b4324ac12839a949b3e58f03cda1d87218c89a8a1efe31c1a/ruamel.yaml-0.17.9.tar.gz
v                         1.8.1        17 Apr 2018  https://github.com/falkTX/rubberband/archive/v1.8.1.tar.gz
rubinius                  5.0          24 Apr 2021  https://github.com/rubinius/rubinius/releases/download/v5.0/rubinius-5.0.tar.bz2
rubocop                   1.13.0       24 Apr 2021  https://rubygems.org/downloads/rubocop-1.13.0.gem
ruby                      3.4.1        25 Dec 2024  https://ftp.ruby-lang.org/pub/ruby/3.4/ruby-3.4.1.tar.xz
ruby2d                    0.9.2        21 Feb 2022  https://rubygems.org/downloads/ruby2d-0.9.2.gem
ruby2js                   4.1.7        05 Oct 2021  https://rubygems.org/downloads/ruby2js-4.1.7.gem
rubygame                  2.6.4        01 Dec 2017  https://rubygems.org/downloads/rubygame-2.6.4.gem
rubygems                  3.6.3        22 Jan 2025  https://rubygems.org/rubygems/rubygems-3.6.3.tgz
rubygr                    0.0.26       15 Apr 2021  https://rubygems.org/downloads/ruby-gr-0.0.26.gem
rubygraphviz              1.2.5        28 May 2020  https://rubygems.org/downloads/ruby-graphviz-1.2.5.gem
rubyinline                3.12.5       28 May 2020  https://rubygems.org/downloads/RubyInline-3.12.5.gem
rubyjs                    0.8.0        01 Dec 2017  https://rubygems.org/downloads/rubyjs-0.8.0.gem
rubyprogressbar           1.11.0       15 Apr 2021  https://rubygems.org/downloads/ruby-progressbar-1.11.0.gem
rubypwsh                  0.8.0        15 Apr 2021  https://rubygems.org/downloads/ruby-pwsh-0.8.0.gem
rubyrc4                   0.1.5        22 Jul 2020  https://rubygems.org/downloads/ruby-rc4-0.1.5.gem
rubystats                 0.3.0        26 Nov 2019  https://rubygems.org/downloads/rubystats-0.3.0.gem
rubytorrent               0.3          02 Dec 2017  https://rubygems.org/downloads/rubytorrent-0.3.gem
ruby                      xosd         01 May 2014  https://rubyforge.org/projects/ruby-xosd/
rubyzip                   2.3.0        28 May 2020  https://rubygems.org/downloads/rubyzip-2.3.0.gem
runit                     2.1.2        19 Oct 2019  http://smarden.org/runit/runit-2.1.2.tar.gz
ruport                    1.7.1        01 Dec 2017  https://rubygems.org/downloads/ruport-1.7.1.gem
ruvi                      0.4.12       01 Jun 2014  https://rubygems.org/downloads/ruvi-0.4.12.gem
rxvt                      2.7.10       01 May 2014  https://sourceforge.net/projects/rxvt/files/rxvt-dev/2.7.10/rxvt-2.7.10.tar.gz
rxvtunicode               9.31         05 Jan 2023  http://dist.schmorp.de/rxvt-unicode/rxvt-unicode-9.31.tar.bz2
rygel                     0.42.2       01 Apr 2023  https://download.gnome.org/sources/rygel/0.42/rygel-0.42.2.tar.xz
sakura                    3.8.7        02 Apr 2024  https://launchpad.net/sakura/trunk/3.8.7/+download/sakura-3.8.7.tar.gz
samba                     4.21.3       07 Jan 2025  https://us1.samba.org/samba/ftp/stable/samba-4.21.3.tar.gz
samtools                  1.21         15 Jan 2025  https://sourceforge.net/projects/samtools/files/samtools/1.21/samtools-1.21.tar.bz2
sanebackends              1.3.1        20 May 2024  https://gitlab.com/sane-project/backends/uploads/110fc43336d0fb5e514f1fdc7360dd87/sane-backends-1.3.1.tar.gz
sanefrontends             1.0.14       25 Aug 2020  https://gitlab.com/sane-project/frontends/uploads/14e5c5a9205b10bd3df04501852eab28/sane-frontends-1.0.14.tar.gz
sash                      3.7          01 May 2014  http://members.tip.net.au/~dbell/programs/sash-3.7.tar.gz
sass                      3.7.4        03 May 2019  https://rubygems.org/downloads/sass-3.7.4.gem
sassrails                 6.0.0        28 May 2020  https://rubygems.org/downloads/sass-rails-6.0.0.gem
sawfish                   1.12.0       27 Dec 2021  https://download.tuxfamily.org/sawfish/sawfish_1.12.0.tar.xz
sbc                       1.5          10 Dec 2020  https://mirrors.edge.kernel.org/pub/linux/bluetooth/sbc-1.5.tar.xz
schroedinger              1.0.11       19 Jan 2016  https://launchpad.net/schroedinger/trunk/1.0.11/+download/schroedinger-1.0.11.tar.gz
scintilla                 553          04 Dec 2024  https://www.scintilla.org/scintilla553.tgz
scipy                     1.0.0        26 Oct 2017  https://github.com/scipy/scipy/releases/download/v1.0.0/scipy-1.0.0.tar.xz
scite                     414          10 Mar 2019  https://www.scintilla.org/scite414.tgz
scons                     4.8.1        06 Sep 2024  https://downloads.sourceforge.net/scons/SCons-4.8.1.tar.gz
scourge                   2            01 Dec 2012  http://scourge.svn.sourceforge.net/viewvc/scourge/scourge2.tar.gz
screen                    5.0.0        29 Aug 2024  https://ftp.gnu.org/gnu/screen/screen-5.0.0.tar.gz
screengrab                2.4.0        30 Aug 2022  https://github.com/lxqt/screengrab/releases/download/2.4.0/screengrab-2.4.0.tar.xz
scribus                   1.6.3        24 Dec 2024  https://downloads.sourceforge.net/scribus/scribus-1.6.3.tar.xz
scripts                   1.0.1        01 May 2014  http://xorg.freedesktop.org/releases/individual/app/scripts-1.0.1.tar.bz2
scrnsaverproto            1.2.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/proto/scrnsaverproto-1.2.2.tar.bz2
scrollkeeper              0.3.14       01 Mar 2014  ftp://ftp.gnome.org/pub/GNOME/sources/scrollkeeper/0.3/scrollkeeper-0.3.14.tar.bz2
scrot                     0.8          01 May 2014  http://linuxbrit.co.uk/downloads/scrot-0.8.tar.gz
scruffy                   0.2.6        02 Dec 2017  https://rubygems.org/downloads/scruffy-0.2.6.gem
scummvm                   2.8.1        01 Apr 2024  https://downloads.scummvm.org/frs/scummvm/2.8.1/scummvm-2.8.1.tar.xz
sddm                      0.21.0       27 Feb 2024  https://github.com/sddm/sddm/releases/download/v0.21.0/sddm-0.21.0.tar.xz
sddmkcm                   6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/sddm-kcm-6.2.5.tar.xz
sdl                       1.2.15       01 Jun 2013  https://www.libsdl.org/release/SDL-1.2.15.tar.gz
sdl12compatrelease        1.2.68       28 Mar 2024  https://github.com/libsdl-org/sdl12-compat/archive/refs/tags/release-1.2.68/sdl12-compat-release-1.2.68.tar.gz
sdl2                      2.30.9       02 Nov 2024  https://www.libsdl.org/release/SDL2-2.30.9.tar.gz
sdl2gfx                   1.0.4        01 Nov 2018  https://sourceforge.net/projects/sdl2gfx/files/SDL2_gfx-1.0.4.tar.gz
sdl2image                 2.8.3        20 Dec 2024  https://github.com/libsdl-org/SDL_image/releases/download/release-2.8.3/SDL2_image-2.8.3.tar.gz
sdl2mixer                 2.8.0        25 Feb 2024  https://github.com/libsdl-org/SDL_mixer/releases/download/release-2.8.0/SDL2_mixer-2.8.0.tar.gz
sdl2net                   2.2.0        20 Sep 2022  https://www.libsdl.org/projects/SDL_net/release/SDL2_net-2.2.0.tar.gz
sdl2ttf                   2.20.2       09 Feb 2023  https://github.com/libsdl-org/SDL_ttf/releases/download/release-2.20.2/SDL2_ttf-2.20.2.tar.gz
sdlgfx                    2.0.22       01 May 2014  http://www.ferzkopp.net/Software/SDL_gfx-2.0/SDL_gfx-2.0.22.tar.gz
sdlimage                  1.2.12       01 May 2014  http://www.libsdl.org/projects/SDL_image/release/SDL_image-1.2.12.tar.gz
sdlmixer                  1.2.12       01 Dec 2012  https://www.libsdl.org/projects/SDL_mixer/release/SDL_mixer-1.2.12.tar.gz
sdlmm                     0.1.8        05 Apr 2024  https://sourceforge.net/projects/sdlmm/files/SDLmm/0.1.8/SDLmm-0.1.8.tar.bz2
sdlnet                    1.2.8        01 Nov 2012  https://www.libsdl.org/projects/SDL_net/release/SDL_net-1.2.8.tar.gz
sdlperl                   2.2.0        16 Jul 2016  http://backpan.perl.org/authors/id/D/DG/DGOEHRIG/SDL_Perl-2.2.0.tar.gz
sdltty                    05.04.2024   05 Apr 2024  https://github.com/Grumbel/SDL_tty/SDL_tty-05.04.2024.tar.xz
sdoc                      2.1.0        15 Apr 2021  https://rubygems.org/downloads/sdoc-2.1.0.gem
sdparm                    1.11         23 Apr 2021  http://sg.danny.cz/sg/p/sdparm-1.11.tar.xz
seaborn                   0.8.1        28 Jun 2018  https://files.pythonhosted.org/packages/10/01/dd1c7838cde3b69b247aaeb61016e238cafd8188a276e366d36aa6bcdab4/seaborn-0.8.1.tar.gz
seahorse                  42.0         23 May 2022  https://download.gnome.org/sources/seahorse/42/seahorse-42.0.tar.xz
seahorsenautilus          3.11.92      01 Mar 2014  http://ftp.gnome.org/pub/GNOME/sources/seahorse-nautilus/3.11/seahorse-nautilus-3.11.92.tar.xz
seamonkey                 2.53.1       28 Feb 2020  https://ftp.mozilla.org/pub/seamonkey/releases/2.53.1/source/seamonkey-2.53.1.source.tar.xz
seatd                     21.02.2024   20 Feb 2024  https://github.com/kennylevinsen/seatd/releases/download/3.2.2/seatd-21.02.2024.tar.xz
seatris                   0.0.14       01 May 2014  http://www.earth.li/projectpurple/files/seatris-0.0.14.tar.gz
sed                       4.9          06 Nov 2022  https://ftp.gnu.org/gnu/sed/sed-4.9.tar.xz
segatex                   8.650        05 Apr 2024  https://sourceforge.net/projects/segatex/files/segatex/8.650/segatex-8.650.tar.gz
semanticpuppet            1.0.3        15 Apr 2021  https://rubygems.org/downloads/semantic_puppet-1.0.3.gem
semantik                  1.0.3        15 Jun 2018  https://waf.io/semantik-1.0.3.tar.bz2
sendmail                  8.17.1       17 Aug 2021  ftp://ftp.sendmail.org/pub/sendmail/sendmail.8.17.1.tar.gz
sensorsapplet             3.0.0        19 Feb 2024  http://downloads.sourceforge.net/sensors-applet/sensors-applet-3.0.0.tar.gz
v                         1.3          02 Jan 2020  https://github.com/lh3/seqtk/archive/v1.3.tar.gz
sequel                    5.79.0       20 Apr 2024  https://rubygems.org/downloads/sequel-5.79.0.gem
serf                      1.3.9        03 Jan 2019  https://archive.apache.org/dist/serf/serf-1.3.9.tar.bz2
sessreg                   1.1.3        31 Oct 2022  https://xorg.freedesktop.org/releases/individual/app/sessreg-1.1.3.tar.xz
setconf                   0.7.6        24 Dec 2018  https://files.pythonhosted.org/packages/e2/13/e3d15834bdbbfcfff4a93639541817be2db36263bfb92868187e2db35df2/setconf-0.7.6.tar.gz
setuptools                75.3.0       10 Nov 2024  https://files.pythonhosted.org/packages/ed/22/a438e0caa4576f8c383fa4d35f1cc01655a46c75be358960d815bfbb12bd/setuptools-75.3.0.tar.gz
setxkbmap                 1.3.4        20 May 2023  https://www.x.org/releases/individual/app/setxkbmap-1.3.4.tar.xz
sg3utils                  1.47         17 Nov 2021  http://sg.danny.cz/sg/p/sg3_utils-1.47.tar.xz
sgmlcommon                0.6.3        10 Jul 2015  ftp://sources.redhat.com/pub/docbook-tools/new-trials/SOURCES/sgml-common-0.6.3.tgz
shaderc                   2024.3       15 Jan 2025  https://github.com/google/shaderc/archive/v2024.3/shaderc-2024.3.tar.gz
shadow                    4.16.0       01 Jul 2024  https://github.com/shadow-maint/shadow/releases/download/4.16.0/shadow-4.16.0.tar.xz
shapelib                  1.3.0        01 Nov 2013  http://download.osgeo.org/shapelib/shapelib-1.3.0.tar.gz
shareddesktopontologies   0.11.0       01 Oct 2013  http://sourceforge.net/projects/oscaf/files/shared-desktop-ontologies/0.11.0/shared-desktop-ontologies-0.11.0.tar.bz2
sharedmimeinfo            2.4          11 Feb 2024  https://gitlab.freedesktop.org/xdg/shared-mime-info/-/archive/2.4/shared-mime-info-2.4.tar.gz
sharutils                 4.15.2       21 Jun 2020  https://ftp.gnu.org/gnu/sharutils/sharutils-4.15.2.tar.xz
shed                      1.15         01 May 2014  https://sourceforge.net/projects/shed/files/shed/shed%201.15/shed-1.15.tar.gz
shepherd                  0.9.2        12 Sep 2022  https://ftp.gnu.org/pub/gnu/shepherd/shepherd-0.9.2.tar.gz
shim                      15.8         22 Jan 2024  https://github.com/rhboot/shim/releases/download/15.8/shim-15.8.tar.bz2
shindo                    0.3.10       15 Apr 2021  https://rubygems.org/downloads/shindo-0.3.10.gem
shine                     21.05.2015   20 May 2015  shine-21.05.2015
shipping                  1.6.0        01 Dec 2017  https://rubygems.org/downloads/shipping-1.6.0.gem
shishi                    1.0.3        09 Aug 2022  https://ftp.gnu.org/pub/gnu/shishi/shishi-1.0.3.tar.gz
shmux                     1.0.2        01 Nov 2012  http://web.taranis.org/shmux/dist/shmux-1.0.2.tgz
shntool                   3.0.7        01 May 2014  http://www.etree.org/shnutils/shntool/dist/src/shntool-3.0.7.tar.gz
shorturl                  1.0.0        01 May 2014  https://rubygems.org/downloads/shorturl-1.0.0.gem
shotcut                   18.12.23     18 Jan 2019  https://github.com/mltframework/shotcut/archive/shotcut-18.12.23.tar.gz
shotgun                   0.9.2        02 Aug 2017  https://rubygems.org/downloads/shotgun-0.9.2.gem
shotwell                  0.32.9       27 Oct 2024  https://download.gnome.org/sources/shotwell/0.32/shotwell-0.32.9.tar.xz
showfont                  1.0.6        01 Sep 2022  https://www.x.org/releases/individual/app/showfont-1.0.6.tar.xz
v                         1.33         02 Feb 2020  https://github.com/najoshi/sickle/archive/v1.33.tar.gz
sidekiq                   6.2.1        15 Apr 2021  https://rubygems.org/downloads/sidekiq-6.2.1.gem
signonkwalletextension    23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/signon-kwallet-extension-23.08.5.tar.xz
sigopt                    1.3.0        04 Apr 2016  https://pypi.python.org/packages/source/s/sigopt/sigopt-1.3.0.tar.gz#md5=ccd67d5c514c1a20659f6217686f0782
silk                      3.10.1       08 Mar 2015  https://tools.netsa.cert.org/releases/silk-3.10.1.tar.gz
simplescan                46.0         16 Apr 2024  https://download.gnome.org/sources/simple-scan/46/simple-scan-46.0.tar.xz
simsoup                   0.3          01 May 2014  http://www.simsoup.info/SimSoup-0.3.tar.gz
sinatra                   2.1.0        01 Feb 2021  https://rubygems.org/downloads/sinatra-2.1.0.gem
sinatraactiverecord       2.0.22       15 Apr 2021  https://rubygems.org/downloads/sinatra-activerecord-2.0.22.gem
sinatracontrib            2.1.0        15 Apr 2021  https://rubygems.org/downloads/sinatra-contrib-2.1.0.gem
sinatrareloader           1.0          26 Dec 2018  https://rubygems.org/downloads/sinatra-reloader-1.0.gem
sip                       6.9.1        13 Dec 2024  https://files.pythonhosted.org/packages/e2/83/b23f610ef99fa23aa3c8dcd2ff8536c37b943654405ff4f45f3230327a40/sip-6.9.1.tar.gz
sipwitch                  1.9.1        01 May 2014  http://ftp.gnu.org/gnu/sipwitch/sipwitch-1.9.1.tar.gz
sisiya                    0.4-21       01 May 2014  http://ovh.dl.sourceforge.net/sourceforge/sisiya/sisiya-0.4-21.tar.gz
sitemapgenerator          6.1.2        17 Jun 2020  https://rubygems.org/downloads/sitemap_generator-6.1.2.gem
sithwm                    1.2.3        01 May 2014  http://sithwm.darkside.no/sn/sithwm-1.2.3.tgz
six                       1.16.0       06 May 2021  https://files.pythonhosted.org/packages/71/39/171f1c67cd00715f190ba0b100d606d440a28c93c7714febeca8b79af85e/six-1.16.0.tar.gz
skanlite                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/skanlite-23.08.5.tar.xz
skanpage                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/skanpage-23.08.5.tar.xz
skijump                   0.2.0        01 May 2014  http://prdownloads.sourceforge.net/skijump/skijump-0.2.0.tar.gz
slang                     2.3.3        28 Nov 2022  https://www.jedsoft.org/releases/slang/slang-2.3.3.tar.bz2
slaptget                  0.10.2f      01 May 2014  http://software.jaos.org/source/slapt-get/slapt-get-0.10.2f.tar.gz
slave                     0.2.0        01 May 2014  http://www.codeforpeople.com/lib/ruby/slave/slave-0.2.0.tgz
sleepypenguin             3.5.2        28 May 2020  https://rubygems.org/downloads/sleepy_penguin-3.5.2.gem
slib                      3b5          16 Dec 2018  http://groups.csail.mit.edu/mac/ftpdir/scm/slib-3b5.tar.gz
slim                      4.1.0        28 May 2020  https://rubygems.org/downloads/slim-4.1.0.gem
slocate                   3.1          01 Jan 2014  http://slocate.trakker.ca/files/slocate-3.1.tar.gz
sloccount                 2.26         01 May 2014  http://sourceforge.net/projects/sloccount/files/sloccount-2.26.tar.gz
slop                      4.8.2        15 Apr 2021  https://rubygems.org/downloads/slop-4.8.2.gem
v                         1.1.0        22 Feb 2019  https://github.com/emersion/slurp/archive/v1.1.0.tar.gz
smake                     1.2.5        07 Feb 2016  http://sourceforge.net/projects/s-make/files/smake-1.2.5.tar.gz
smalltalk                 3.2.5        01 May 2023  https://ftp.gnu.org/gnu/smalltalk/smalltalk-3.2.5.tar.xz
smartmontools             7.3          07 Mar 2022  https://downloads.sourceforge.net/smartmontools/smartmontools-7.3.tar.gz
smc                       1.9          01 Dec 2012  http://downloads.sourceforge.net/smclone/smc-1.9.tar.bz2
smf                       0.15.15      26 Nov 2019  https://rubygems.org/downloads/smf-0.15.15.gem
smokeqt                   4.14.3       15 Jul 2015  ftp://gd.tuwien.ac.at/kde/stable/4.14.3/src/smokeqt-4.14.3.tar.xz
smpeg                     18.11.2011   01 Mar 2013  http://www.3ddownloads.com/linuxgames/loki/open-source/smpeg/smpeg-18.11.2011.tar.gz
smplayer                  21.1.0       17 Jan 2021  https://downloads.sourceforge.net/smplayer/smplayer-21.1.0.tar.bz2
smproxy                   1.0.7        16 Oct 2022  https://www.x.org/releases/individual/app/smproxy-1.0.7.tar.xz
smuxi                     0.8.11       01 May 2014  https://www.smuxi.org/jaws/data/files/smuxi-0.8.11.tar.gz
snack                     2.2.10       01 Sep 2013  http://www.speech.kth.se/snack/dist/snack2.2.10.tar.gz
snake3d                   0.7          01 May 2014  http://surfnet.dl.sourceforge.net/sourceforge/worms3d/snake3d-0.7.tgz
snappy                    1.0          01 Apr 2014  http://ftp.gnome.org/pub/gnome/sources/snappy/1.0/snappy-1.0.tar.xz
snapscreenshot            1.0.14.3     01 Mar 2014  http://bisqwit.iki.fi/src/arch/snapscreenshot-1.0.14.3.tar.bz2
snd                       19.8         27 Nov 2019  https://ccrma.stanford.edu/software/snd/snd-19.8.tar.gz
snes9x                    1.53         08 Sep 2017  http://files.ipherswipsite.com/snes9x/snes9x-1.53.tar.bz2
snogray                   20.04.2008   01 May 2014  http://download.savannah.gnu.org/releases/snogray/snogray-20.04.2008.tar.gz
snort                     2.9.17       19 Nov 2020  https://www.snort.org/downloads/snort/snort-2.9.17.tar.gz
soappy                    0.12.22      02 Aug 2017  https://pypi.python.org/packages/78/1b/29cbe26b2b98804d65e934925ced9810883bdda9eacba3f993ad60bfe271/SOAPpy-0.12.22.zip
solid                     5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/solid-5.116.0.tar.xz
sonik                     1.0.0        01 May 2014  http://prdownloads.sourceforge.net/sonik/sonik-1.0.0.tar.gz
sonnet                    5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/sonnet-5.116.0.tar.xz
soprano                   2.9.4        01 Dec 2013  http://sourceforge.net/projects/soprano/files/Soprano/2.9.4/soprano-2.9.4.tar.bz2
sortversions              1.5          26 Aug 2017  http://search.cpan.org/CPAN/authors/id/E/ED/EDAVIS/Sort-Versions-1.5.tar.gz
soundconverter            2.1.4        20 Dec 2014  https://launchpad.net/soundconverter/trunk/2.1.4/+download/soundconverter-2.1.4.tar.xz
soundjuicer               3.40.0       19 Jun 2023  https://download.gnome.org/sources/sound-juicer/3.40/sound-juicer-3.40.0.tar.xz
soundtouch                2.3.3        02 Apr 2024  https://www.surina.net/soundtouch/soundtouch-2.3.3.tar.gz
sourceinstall             2.5rc2       01 May 2014  ftp://ftp.gnu.org/gnu/sourceinstall/sourceinstall-2.5rc2.tar.gz
sox                       14.4.2       12 Apr 2015  https://sourceforge.net/projects/sox/files/sox/14.4.2/sox-14.4.2.tar.gz
spacebar                  6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/spacebar-6.2.5.tar.xz
spdf                      0.2          01 May 2014  http://users.tkk.fi/~jitulkki/spdf/spdf-0.2.tar.gz
spectacle                 23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/spectacle-23.08.5.tar.xz
spectrographic            0.9.3        28 Dec 2019  https://files.pythonhosted.org/packages/6c/cd/5ef7c8e1d53b1d0df1a6507b267ca3bbca889bcb83395d5ddb72d6b7080e/spectrographic-0.9.3.tar.gz
speechd                   26.11.2013   01 Nov 2013  http://git.freebsoft.org/?p=speechd.git/speechd-26.11.2013.tar.gz
speedcrunch?content=      16696        01 May 2014  http://www.kde-apps.org/content/show.php/SpeedCrunch?content=16696
speex                     1.2.0        16 Feb 2017  https://downloads.xiph.org/releases/speex/speex-1.2.0.tar.gz
speexdsp                  1.2.0        10 Jun 2019  https://ftp.osuosl.org/pub/xiph/releases/speex/speexdsp-1.2.0.tar.gz
sphinx                    2.2.0        15 Oct 2019  https://files.pythonhosted.org/packages/76/42/a4465a0080e545cd152f7d3f16229d5e1300183fcb1067e4ec7e639b8605/Sphinx-2.2.0.tar.gz
spice                     0.12.8       08 Aug 2016  http://www.spice-space.org/download/releases/spice-0.12.8.tar.bz2
spiceprotocol             0.12.12      08 Aug 2016  http://spice-space.org/download/releases/spice-protocol-0.12.12.tar.bz2
spidermonkey              08.01.2007   01 Jun 2013  spidermonkey-08.01.2007
spirvheaders              1.4.304.0    18 Jan 2025  https://github.com/KhronosGroup/SPIRV-Headers/archive/vulkan-sdk-1.4.304.0/SPIRV-Headers-1.4.304.0.tar.gz
spirvllvmtranslator       19.1.0       23 Sep 2024  https://github.com/KhronosGroup/SPIRV-LLVM-Translator/archive/v19.1.0/SPIRV-LLVM-Translator-19.1.0.tar.gz
spirvtools                1.3.283.0    07 Jul 2024  https://github.com/KhronosGroup/SPIRV-Tools/archive/vulkan-sdk-1.3.283.0/SPIRV-Tools-1.3.283.0.tar.gz
splint                    3.1.1        01 May 2014  http://www.splint.org/downloads/splint-3.1.1.tgz
splitvt                   1.6.5        01 May 2014  http://www.devolution.com/~slouken/projects/splitvt/splitvt-1.6.5.tar.gz
spreadsheet               1.3.1        07 Apr 2024  https://rubygems.org/downloads/spreadsheet-1.3.1.gem
spring                    2.1.1        15 Apr 2021  https://rubygems.org/downloads/spring-2.1.1.gem
sprockets                 4.0.2        17 Jun 2020  https://rubygems.org/downloads/sprockets-4.0.2.gem
sprocketsrails            3.2.2        15 Apr 2021  https://rubygems.org/downloads/sprockets-rails-3.2.2.gem
sqlite3                   1.6.1        06 Mar 2023  https://rubygems.org/downloads/sqlite3-1.6.1.gem
sqliteautoconf            3470200      08 Dec 2024  https://www.sqlite.org/2024/sqlite-autoconf-3470200.tar.gz
sqlitebrowser             3.11.0       05 Feb 2019  https://github.com/sqlitebrowser/sqlitebrowser/archive/sqlitebrowser-3.11.0.tar.gz
sqliteruby                2.2.3        28 Nov 2017  https://rubygems.org/downloads/sqlite-ruby-2.2.3.gem
squid                     6.12         13 Oct 2024  http://www.squid-cache.org/Versions/v6/squid-6.12.tar.gz
sshfs                     3.7.3        29 May 2022  https://github.com/libfuse/sshfs/releases/download/sshfs-3.7.3/sshfs-3.7.3.tar.xz
sslscan                   1.8.2        01 Jan 2014  http://downloads.sourceforge.net/project/sslscan/sslscan/sslscan-1.8.2.tgz
ssysm                     0.2.4.3      01 May 2014  http://freshmeat.net/redir/ssysm/65338/url_tgz/ssysm-0.2.4.3.tar.gz
st                        0.8.1        19 Sep 2018  https://dl.suckless.org/st/st-0.8.1.tar.gz
stager                    2.0.1        01 May 2014  http://software.uninett.no/stager/download/Stager_2.0.1.tar.gz
startupnotification       0.12         01 Jun 2013  http://www.freedesktop.org/software/startup-notification/releases/startup-notification-0.12.tar.gz
statifier                 1.7.1        01 May 2014  http://sourceforge.net/projects/statifier/files/statifier/1.7.1/statifier-1.7.1.tar.gz
stella                    3.7.2        01 May 2014  http://sourceforge.net/projects/stella/files/stella/3.7.2/stella-3.7.2.tar.gz
step                      23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/step-23.08.5.tar.xz
stfl                      0.23         03 Oct 2017  http://www.clifford.at/stfl/stfl-0.23.tar.gz
stlport                   5.2.1        24 May 2020  https://sourceforge.net/projects/stlport/files/STLport/STLport-5.2.1/STLport-5.2.1.tar.bz2
stoken                    0.92         18 Jan 2025  https://sourceforge.net/projects/stoken/files/stoken-0.92.tar.gz
stow                      2.4.0        10 Sep 2024  http://ftp.gnu.org/pub/gnu/stow/stow-2.4.0.tar.gz
strace                    6.12         15 Jan 2025  https://github.com/strace/strace/releases/download/v6.12/strace-6.12.tar.xz
strasheela                0.8          01 Mar 2013  http://heanet.dl.sourceforge.net/sourceforge/strasheela/strasheela-0.8.tar.gz
stratagus                 2.2.5.5.orig 01 Nov 2012  http://launchpad.net/stratagus/trunk/2.2.5.5/+download/stratagus_2.2.5.5.orig.tar.gz
streamripper              1.64.6       01 May 2014  http://sourceforge.net/projects/streamripper/files/streamripper%20%28current%29/1.64.6/streamripper-1.64.6.tar.gz
streamtuner               0.99.99      01 Apr 2014  http://savannah.nongnu.org/download/streamtuner/streamtuner-0.99.99.tar.gz
strigi                    0.7.8        01 Jan 2014  http://www.vandenoever.info/software/strigi/strigi-0.7.8.tar.bz2
strongswan                4.6.4        01 Jun 2012  http://download.strongswan.org/strongswan-4.6.4.tar.bz2
stunnel                   5.73         11 Sep 2024  https://www.stunnel.org/downloads/stunnel-5.73.tar.gz
subtitlecomposer          05.04.2024   10 Oct 2017  https://github.com/maxrd2/subtitlecomposer/subtitlecomposer-05.04.2024.tar.xz
subtitleeditor            0.40.0       01 May 2014  http://download.gna.org/subtitleeditor/0.40/subtitleeditor-0.40.0.tar.gz
subtle                    0.11.3224-xi 01 Nov 2012  https://subforge.org/attachments/download/81/subtle-0.11.3224-xi.tbz2
subversion                1.14.5       10 Dec 2024  https://www.apache.org/dist/subversion/subversion-1.14.5.tar.bz2
sudo                      1.9.13       16 Feb 2023  https://www.sudo.ws/dist/sudo-1.9.13.tar.gz
1                         Source       15 Dec 2019  https://github.com/SuperTux/supertux/releases/download/v0.6.1/SuperTux-v0.6.1-Source.tar.gz
supybot                   0.83.1       01 May 2014  https://surfnet.dl.sourceforge.net/sourceforge/supybot/Supybot-0.83.1.tar.bz2
sushi                     46.0         06 Apr 2024  https://download.gnome.org/sources/sushi/46/sushi-46.0.tar.xz
suspendutils              1.0          01 Sep 2013  http://sourceforge.net/projects/suspend/files/suspend/suspend-1.0/suspend-utils-1.0.tar.bz2
svgalib                   1.9.25       01 Aug 2013  http://www.svgalib.org/svgalib-1.9.25.tar.gz
svgpart                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/svgpart-23.08.5.tar.xz
sway                      1.8.1        16 Mar 2023  https://github.com/swaywm/sway/releases/download/1.8.1/sway-1.8.1.tar.gz
swbis                     1.13.1       06 Nov 2018  https://ftp.gnu.org/pub/gnu/swbis/swbis-1.13.1.tar.gz
sweep                     0.9.1        01 May 2014  http://www.metadecks.org/software/sweep/download/sweep-0.9.1.tar.bz2
sweeper                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/sweeper-23.08.5.tar.xz
swellfoop                 46.0         16 Mar 2024  https://download.gnome.org/sources/swell-foop/46/swell-foop-46.0.tar.xz
swig                      4.3.0        23 Oct 2024  https://downloads.sourceforge.net/swig/swig-4.3.0.tar.gz
swinput                   0.7.4        04 May 2019  http://download.savannah.nongnu.org/releases/swinput/swinput-0.7.4.tar.gz
swipl                     7.6.0-rc2    30 Sep 2017  http://www.swi-prolog.org/download/stable/src/swipl-7.6.0-rc2.tar.gz
swpkg                     0.51         01 May 2014  http://web.taranis.org/swpkg/dist/swpkg-0.51.tgz
syck                      1.4.1        22 Sep 2022  https://rubygems.org/downloads/syck-1.4.1.gem
sylpheed                  3.7.0        01 Feb 2018  http://sylpheed.sraoss.jp/sylpheed/v3.7/sylpheed-3.7.0.tar.xz
sympa                     6.1.25       26 Nov 2017  http://www.sympa.org/distribution/sympa-6.1.25.tar.gz
sympy                     1.8          12 Jun 2021  https://github.com/sympy/sympy/releases/download/sympy-1.8/sympy-1.8.tar.gz
synaptic                  0.84.2       10 Mar 2017  http://ftp.debian.org/debian/pool/main/s/synaptic/synaptic_0.84.2.tar.xz
syndication               5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/syndication-5.116.0.tar.xz
synfig                    1.2.1        08 Oct 2017  https://sourceforge.net/projects/synfig/files/releases/1.2.1/source/synfig-1.2.1.tar.gz
syntax                    1.2.2        01 Dec 2017  https://rubygems.org/downloads/syntax-1.2.2.gem
syntaxhighlighting        5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/syntax-highlighting-5.116.0.tar.xz
sysklogd                  2.2.0        10 Mar 2021  https://github.com/troglobit/sysklogd/releases/download/v2.2.0/sysklogd-2.2.0.tar.gz
syslinux                  6.03         16 Nov 2018  https://mirrors.edge.kernel.org/pub/linux/utils/boot/syslinux/syslinux-6.03.tar.xz
sysprof                   46.0         29 Jun 2024  https://download.gnome.org/sources/sysprof/46/sysprof-46.0.tar.xz
sysstat                   12.7.2       08 Jul 2024  http://pagesperso-orange.fr/sebastien.godard/sysstat-12.7.2.tar.xz
systemd                   256.4        08 Sep 2024  https://github.com/systemd/systemd/archive/v256.4/systemd-256.4.tar.gz
systemsettings            6.2.5        01 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/systemsettings-6.2.5.tar.xz
systemtap                 3.2          01 Nov 2017  https://sourceware.org/systemtap/ftp/releases/systemtap-3.2.tar.gz
systemtoolsbackends       2.10.2       01 Feb 2014  http://ftp.gnome.org/pub/GNOME/sources/system-tools-backends/2.10/system-tools-backends-2.10.2.tar.gz
systemu                   2.6.5        25 Nov 2019  https://rubygems.org/downloads/systemu-2.6.5.gem
sysuname                  1.2.1        28 May 2020  https://rubygems.org/downloads/sys-uname-1.2.1.gem
sysvinit                  3.13         11 Jan 2025  https://github.com/slicer69/sysvinit/releases/download/3.13/sysvinit-3.13.tar.xz
tabbed                    0.6          30 Jun 2015  http://dl.suckless.org/tools/tabbed-0.6.tar.gz
tabble                    0.45         06 Oct 2020  http://www.rillion.net/tabble/tabble-0.45.tar.gz
taglib                    2.0.2        04 Sep 2024  https://github.com/taglib/taglib/releases/download/v2.0.2/taglib-2.0.2.tar.gz
taglibruby                1.1.0        27 Jan 2021  https://rubygems.org/downloads/taglib-ruby-1.1.0.gem
tali                      40.5         01 Feb 2022  https://download.gnome.org/sources/tali/40/tali-40.5.tar.xz
talib                     0.4.10       30 Jul 2017  https://pypi.python.org/packages/c0/13/3d92ed6c09ed585ed58aeab5da6361ff2aeff7757d8224089bd574945716/TA-Lib-0.4.10.tar.gz
talloc                    2.4.0        21 Jan 2023  https://www.samba.org/ftp/talloc/talloc-2.4.0.tar.gz
tangoicontheme            0.7.2        01 May 2014  http://tango-project.org/releases/tango-icon-theme-0.7.2.tar.gz
tar                       1.35         21 Jan 2024  https://ftp.gnu.org/gnu/tar/tar-1.35.tar.xz
tartan                    0.2.1        01 Nov 2012  https://rubygems.org/downloads/tartan-0.2.1.gem
tasks                     3.3.90       01 Nov 2013  http://ftp.gnome.org/pub/GNOME/sources/tasks/3.3/tasks-3.3.90.tar.xz
tasque                    0.1.12       24 Sep 2014  http://ftp.gnome.org/pub/GNOME/sources/tasque/0.1/tasque-0.1.12.tar.xz
tcc                       0.9.26       01 Aug 2013  http://download.savannah.gnu.org/releases/tinycc/tcc-0.9.26.tar.bz2
tcl                       9.0.0        27 Sep 2024  https://prdownloads.sourceforge.net/tcl/tcl9.0.0.tar.gz
tcpdump                   4.99.5       31 Aug 2024  https://www.tcpdump.org/release/tcpdump-4.99.5.tar.gz
tcsh                      6.24.13      16 Jun 2024  https://astron.com/pub/tcsh/tcsh-6.24.13.tar.gz
tdb                       1.4.11       31 Jul 2024  https://www.samba.org/ftp/tdb/tdb-1.4.11.tar.gz
tea                       44.1.0       18 Jul 2017  http://semiletov.org/tea/dloads/tea-44.1.0.tar.bz2
ted                       2.21         01 May 2014  ftp://ftp.nluug.nl/pub/editors/ted/ted-2.21.tar.gz
telegnome                 0.3.6        30 Aug 2021  https://download.gnome.org/sources/telegnome/0.3/telegnome-0.3.6.tar.xz
telepathyglib             0.24.1       16 Sep 2014  http://telepathy.freedesktop.org/releases/telepathy-glib/telepathy-glib-0.24.1.tar.gz
telepathylogger           0.8.0        01 Oct 2013  http://telepathy.freedesktop.org/releases/telepathy-logger/telepathy-logger-0.8.0.tar.bz2
telepathymissioncontrol   5.16.6       12 Feb 2022  https://telepathy.freedesktop.org/releases/telepathy-mission-control/telepathy-mission-control-5.16.6.tar.gz
telepathyqt               0.9.8        30 May 2020  https://github.com/TelepathyIM/telepathy-qt/releases/download/telepathy-qt-0.9.8/telepathy-qt-0.9.8.tar.gz
tellico                   3.4.4        19 Feb 2022  https://tellico-project.org/files/tellico-3.4.4.tar.xz
tellyskout                23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/telly-skout-23.08.5.tar.xz
templateglib              3.36.2       14 Mar 2024  https://download.gnome.org/sources/template-glib/3.36/template-glib-3.36.2.tar.xz
temple                    0.8.2        28 May 2020  https://rubygems.org/downloads/temple-0.8.2.gem
tepl                      6.9.0        19 Feb 2024  https://download.gnome.org/sources/tepl/6.9/tepl-6.9.0.tar.xz
termanimation             2.6          13 Oct 2018  https://cpan.metacpan.org/authors/id/K/KB/KBAUCOM/Term-Animation-2.6.tar.gz
termansicolor             1.7.1        18 Feb 2019  https://rubygems.org/downloads/term-ansicolor-1.7.1.gem
termcap                   1.3.1        01 May 2014  ftp://ftp.gnu.org/gnu/termcap/termcap-1.3.1.tar.gz
terminator                2.1.1        11 May 2021  https://github.com/gnome-terminator/terminator/releases/download/v2.1.1/terminator-2.1.1.tar.gz
terminology               1.13.0       14 Apr 2024  https://download.enlightenment.org/rel/apps/terminology/terminology-1.13.0.tar.xz
termit                    3.9.0        11 Sep 2017  https://rubygems.org/downloads/termit-3.9.0.gem
termreadkey               2.38         28 Jun 2021  https://cpan.metacpan.org/authors/id/J/JS/JSTOWE/TermReadKey-2.38.tar.gz
testdifferences           0.66         03 Sep 2018  https://cpan.metacpan.org/authors/id/D/DC/DCANTRELL/Test-Differences-0.66.tar.gz
testdisk                  6.11.3       01 May 2014  http://www.cgsecurity.org/testdisk-6.11.3.tar.bz2
testunit                  3.3.6        17 Jun 2020  https://rubygems.org/downloads/test-unit-3.3.6.gem
tetex                     3.0          01 May 2014  ftp://tug.ctan.org/tex-archive/systems/unix/teTeX/3.0/distrib/tetex-3.0.tar.gz
tevent                    0.16.0       19 Oct 2023  https://www.samba.org/ftp/tevent/tevent-0.16.0.tar.gz
texi2html                 1.82         01 Jan 2013  http://savannah.inetbridge.net/texi2html/texi2html-1.82.tar.bz2
texinfo                   7.2          29 Dec 2024  https://ftp.gnu.org/gnu/texinfo/texinfo-7.2.tar.xz
texmaker                  5.0.3        02 Nov 2018  http://www.xm1math.net/texmaker/texmaker-5.0.3.tar.bz2
textbibtex                0.88         19 Apr 2021  https://cpan.metacpan.org/authors/id/A/AM/AMBS/Text-BibTeX-0.88.tar.gz
textcsv                   2.00         12 May 2019  https://cpan.metacpan.org/authors/id/I/IS/ISHIGAKI/Text-CSV-2.00.tar.gz
thesilversearcher         2.2.0        13 Sep 2018  http://geoff.greer.fm/ag/releases/the_silver_searcher-2.2.0.tar.gz
thin                      1.7.2        31 Jul 2017  https://rubygems.org/downloads/thin-1.7.2.gem
thoggen                   0.7.1        01 Apr 2014  http://prdownloads.sourceforge.net/thoggen/thoggen-0.7.1.tar.gz
thor                      1.1.0        25 Apr 2021  https://rubygems.org/downloads/thor-1.1.0.gem
threadsafe                0.3.6        07 Dec 2017  https://rubygems.org/downloads/thread_safe-0.3.6.gem
threadweaver              5.116.0      21 May 2024  https://download.kde.org/stable/frameworks/5.116/threadweaver-5.116.0.tar.xz
thunar                    4.20.1       31 Dec 2024  https://archive.xfce.org/src/xfce/thunar/4.20/thunar-4.20.1.tar.bz2
thunarvolman              4.15.0       25 Aug 2020  https://archive.xfce.org/src/xfce/thunar-volman/4.15/thunar-volman-4.15.0.tar.bz2
tidy                      1.1.2        05 Mar 2017  https://rubygems.org/downloads/tidy-1.1.2.gem
tiff                      4.7.0        19 Sep 2024  https://download.osgeo.org/libtiff/tiff-4.7.0.tar.xz
tightvnc                  1.3.10-unixsrc 01 May 2014  https://sourceforge.net/projects/vnc-tight/files/TightVNC-unix/1.3.10/tightvnc-1.3.10_unixsrc.tar.bz2
tilda                     14.01.2018   14 Jan 2018  https://github.com/lanoxx/tilda/tilda-14.01.2018
tilt                      2.0.10       28 May 2020  https://rubygems.org/downloads/tilt-2.0.10.gem
timelimit                 1.9.0        18 Sep 2018  https://devel.ringlet.net/files/sys/timelimit/timelimit-1.9.0.tar.xz
timidity++                2.14.0       01 May 2014  https://sourceforge.net/projects/timidity/files/TiMidity++/TiMidity++-2.14.0/TiMidity++-2.14.0.tar.xz
tintin                    2.02.20      07 Dec 2022  https://github.com/scandum/tintin/releases/download/2.02.20/tintin-2.02.20.tar.gz
tf                        50b8         01 Feb 2013  https://sourceforge.net/projects/tinyfugue/files/tinyfugue/5.0%20beta%208/tf-50b8.tar.gz
tinyvm                    03.07.2011   01 May 2014  https://github.com/GenTiradentes/tinyvm/tinyvm-03.07.2011
tinyxml                   2_6_2        01 May 2014  http://sourceforge.net/projects/tinyxml/files/tinyxml/2.6.2/tinyxml_2_6_2.tar.gz
tioga                     1.19.1       10 Feb 2016  https://rubygems.org/downloads/tioga-1.19.1.gem
tk                        8.6.15       22 Sep 2024  https://sourceforge.net/projects/tcl/files/Tcl/8.6.15/tk8.6.15.tar.gz
tmux                      3.5a         06 Oct 2024  https://github.com/tmux/tmux/releases/download/3.5a/tmux-3.5a.tar.gz
tmuxinator                2.0.1        28 May 2020  https://rubygems.org/downloads/tmuxinator-2.0.1.gem
tokodon                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/tokodon-23.08.5.tar.xz
tomboy                    1.15.9       16 Jul 2017  http://ftp.gnome.org/pub/gnome/sources/tomboy/1.15/tomboy-1.15.9.tar.xz
toml                      0.10.2       02 Dec 2021  https://files.pythonhosted.org/packages/be/ba/1f744cdc819428fc6b5084ec34d9b30660f6f9daaf70eead706e3203ec3c/toml-0.10.2.tar.gz
toppler                   1.1.6        12 Dec 2015  http://sourceforge.net/projects/toppler/files/toppler/1.1.6/toppler-1.1.6.tar.gz
torsmo                    0.18         01 May 2014  https://sourceforge.net/projects/torsmo/files/torsmo/torsmo%200.18/torsmo-0.18.tar.gz
totem                     42.0         17 Jul 2022  https://download.gnome.org/sources/totem/42/totem-42.0.tar.xz
totemplparser             3.26.6       26 Jun 2021  https://download.gnome.org/sources/totem-pl-parser/3.26/totem-pl-parser-3.26.6.tar.xz
toybox                    0.8.12       22 Jan 2025  https://www.landley.net/toybox/downloads/toybox-0.8.12.tar.gz
toycars                   0.3.10       01 Dec 2012  http://sourceforge.net/projects/toycars/files/toycars/0.3.10/toycars-0.3.10.tar.gz
tpm2tss                   4.1.3        07 Jul 2024  https://github.com/tpm2-software/tpm2-tss/releases/download/4.1.3/tpm2-tss-4.1.3.tar.gz
trac                      1.4.3        11 Nov 2022  https://download.edgewall.org/trac/Trac-1.4.3.tar.gz
traceroute                2.1.6        16 Sep 2024  https://sourceforge.net/projects/traceroute/files/traceroute/traceroute-2.1.6/traceroute-2.1.6.tar.gz
trackballs                1.1.4        03 Apr 2016  https://sourceforge.net/projects/trackballs/files/trackballs/1.1.4/trackballs-1.1.4.tar.gz
tracker                   3.7.2        01 May 2024  https://download.gnome.org/sources/tracker/3.7/tracker-3.7.2.tar.xz
trackerminers             3.7.1        27 Mar 2024  https://download.gnome.org/sources/tracker-miners/3.7/tracker-miners-3.7.1.tar.xz
trackfs                   0.0.7        01 May 2014  http://www.mr511.de/software/trackfs-0.0.7.tar.gz
tramp                     2.5.4        02 Dec 2022  https://ftp.gnu.org/pub/gnu/tramp/tramp-2.5.4.tar.gz
translateshell            0.9.7.1      21 Mar 2023  translate-shell-0.9.7.1
transmission              4.0.6        07 Oct 2024  https://github.com/transmission/transmission/releases/download/4.0.6/transmission-4.0.6.tar.xz
transset                  1.0.4        06 Dec 2024  https://www.x.org/releases/individual/app/transset-1.0.4.tar.xz
tree                      2.0.1        05 Jan 2022  ftp://mama.indstate.edu/linux/tree/tree-2.0.1.tgz
treetop                   1.6.12       04 Mar 2023  https://rubygems.org/downloads/treetop-1.6.12.gem
03                        3            11 Jul 2020  https://github.com/julian-klode/triehash/archive/debian/0.3-3.tar.gz
trimines                  1.3.0        01 May 2014  http://www.freewebs.com/trimines/trimines-1.3.0.tar.gz
trix                      0.93         01 May 2014  http://www.kde-apps.org/content/show.php/trix?content=49417/trix-0.93
trtl                      0.0.2        07 Oct 2017  https://rubygems.org/downloads/trtl-0.0.2.gem
tslib                     1.22         06 Mar 2023  https://github.com/libts/tslib/releases/download/1.22/tslib-1.22.tar.xz
tsombie                   1.1.2        01 Dec 2012  http://mathias-kettner.com/download/tsombie-1.1.2.tar.gz
ttfunk                    1.6.2.1      28 May 2020  https://rubygems.org/downloads/ttfunk-1.6.2.1.gem
ttycolor                  0.5.1        28 May 2020  https://rubygems.org/downloads/tty-color-0.5.1.gem
ttycursor                 0.7.1        28 May 2020  https://rubygems.org/downloads/tty-cursor-0.7.1.gem
ttyprogressbar            0.17.0       28 May 2020  https://rubygems.org/downloads/tty-progressbar-0.17.0.gem
ttyscreen                 0.8.0        17 Jun 2020  https://rubygems.org/downloads/tty-screen-0.8.0.gem
tulip                     6.0.0        21 Jan 2025  https://sourceforge.net/projects/auber/files/tulip/tulip-6.0.0/tulip-6.0.0_src.tar.gz
tumbler                   4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/tumbler/4.20/tumbler-4.20.0.tar.bz2
tunapie                   2.1.19       27 Nov 2019  https://sourceforge.net/projects/tunapie/files/tunapie/2.1/tunapie-2.1.19.tar.gz
turbolinks                5.2.1        28 May 2020  https://rubygems.org/downloads/turbolinks-5.2.1.gem
turbovnc                  1.2.1        01 Jul 2014  http://sourceforge.net/projects/turbovnc/files/1.2.1/turbovnc-1.2.1.tar.gz
tuxrip                    099beta6     01 May 2014  http://tuxrip.free.fr/tuxrip/tuxrip099beta6.tar.bz2
tv                        0.5.1        24 Nov 2017  http://darwin.zoology.gla.ac.uk/~rpage/treeviewx/download/0.5/tv-0.5.1.tar.gz
tvtime                    1.0.1        01 May 2014  http://prdownloads.sourceforge.net/tvtime/tvtime-1.0.1.tar.gz
twilioruby                5.37.0       17 Jun 2020  https://rubygems.org/downloads/twilio-ruby-5.37.0.gem
twind                     1.1.0        01 May 2014  http://puzzle.dl.sourceforge.net/sourceforge/twind/twind-1.1.0.tar.gz
twisted                   18.9.0       18 Oct 2018  https://files.pythonhosted.org/packages/5d/0e/a72d85a55761c2c3ff1cb968143a2fd5f360220779ed90e0fadf4106d4f2/Twisted-18.9.0.tar.bz2
twitux                    0.69         29 Jul 2016  https://sourceforge.net/projects/twitux/files/twitux/0.69/twitux-0.69.tar.bz2
twm                       1.0.12       04 Apr 2022  https://www.x.org/releases/individual/app/twm-1.0.12.tar.xz
twolame                   0.4.0        25 Sep 2022  https://sourceforge.net/projects/twolame/files/twolame/0.4.0/twolame-0.4.0.tar.gz
txr                       206          18 Jan 2019  http://www.kylheku.com/cgit/txr/snapshot/txr-206.tar.bz2
typespeed                 0.6.5        27 Mar 2017  http://typespeed.sourceforge.net/typespeed-0.6.5.tar.gz
tzinfo                    2.0.2        28 May 2020  https://rubygems.org/downloads/tzinfo-2.0.2.gem
uchardet                  0.0.8        12 Dec 2022  https://www.freedesktop.org/software/uchardet/releases/uchardet-0.0.8.tar.xz
uclibc                    0.9.32       01 May 2014  http://www.uclibc.org/downloads/uClibc-0.9.32.tar.bz2
ucview                    0.16         01 May 2014  http://www.unicap-imaging.org/downloads/ucview-0.16.tar.gz
udev                      182          01 Aug 2013  http://www.kernel.org/pub/linux/utils/kernel/hotplug/udev-182.tar.xz
udftools                  1.0.0b3      01 May 2014  http://downloads.sourceforge.net/linux-udf/udftools-1.0.0b3.tar.gz
udisks                    2.9.4        24 Oct 2021  https://github.com/storaged-project/udisks/releases/download/udisks-2.9.4/udisks-2.9.4.tar.bz2
ufoai                     2.5          09 Aug 2015  http://sourceforge.net/projects/ufoai/files/UFO_AI%202.x/2.5/ufoai-2.5-source.tar.bz2
uglifier                  4.2.0        28 May 2020  https://rubygems.org/downloads/uglifier-4.2.0.gem
uhttpmock                 0.10.0       08 Mar 2024  https://tecnocode.co.uk/downloads/uhttpmock/uhttpmock-0.10.0.tar.xz
uim                       1.5.4        01 May 2014  http://uim.googlecode.com/files/uim-1.5.4.tar.bz2
umbrello                  23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/umbrello-23.08.5.tar.xz
umockdev                  0.19.0       30 Dec 2024  https://github.com/martinpitt/umockdev/releases/download/0.19.0/umockdev-0.19.0.tar.xz
unbound                   1.21.0       17 Aug 2024  https://nlnetlabs.nl/downloads/unbound/unbound-1.21.0.tar.gz
uncrustify                0.65         02 Aug 2017  https://sourceforge.net/projects/uncrustify/files/uncrustify/uncrustify-0.65/uncrustify-0.65.tar.gz
unf                       0.1.4        21 Sep 2017  https://rubygems.org/downloads/unf-0.1.4.gem
unfext                    0.0.8.2      25 Jan 2023  https://rubygems.org/downloads/unf_ext-0.0.8.2.gem
dxp                       2532         01 May 2014  http://www.geocities.com/dxp2532/
unicodecollate            1.28         24 Sep 2020  http://search.cpan.org/CPAN/authors/id/S/SA/SADAHIRO/Unicode-Collate-1.28.tar.gz
unicorn                   6.1.0        26 Aug 2022  https://rubygems.org/downloads/unicorn-6.1.0.gem
unieject                  6            01 May 2014  http://prdownloads.sourceforge.net/unieject/unieject-6.tar.bz2
unifdef                   2.12         18 Mar 2023  https://dotat.at/prog/unifdef/unifdef-2.12.tar.xz
union                     1.0.6        06 Nov 2018  https://rubygems.org/downloads/union-1.0.6.gem
unionfsfuse               0.25         01 May 2014  http://podgorny.cz/unionfs-fuse/releases/unionfs-fuse-0.25.tar.bz2
units                     2.22         06 Sep 2022  https://ftp.gnu.org/pub/gnu/units/units-2.22.tar.gz
unity                     7.4.0        20 Nov 2019  https://launchpad.net/unity/7.4/7.4.0/+download/unity-7.4.0.tar.bz2
unixodbc                  2.3.7        07 Nov 2019  ftp://ftp.unixodbc.org/pub/unixODBC/unixODBC-2.3.7.tar.gz
unoconv                   0.7          15 Jan 2019  http://dag.wieers.com/home-made/unoconv/unoconv-0.7.tar.gz
unotools                  0.3.3        10 Jan 2020  https://files.pythonhosted.org/packages/12/35/443632cfa43f82efd363bc8104b3ae67a887a248c6c6baec34c7b9edbcb5/unotools-0.3.3.tar.gz
unrtf                     0.21.10      25 Nov 2018  http://ftp.gnu.org/pub/gnu/unrtf/unrtf-0.21.10.tar.gz
unzip                     60           17 Dec 2015  http://downloads.sourceforge.net/infozip/unzip60.tar.gz
upower                    v1.90.6      19 Sep 2024  https://gitlab.freedesktop.org/upower/upower/-/archive/v1.90.6/upower-v1.90.6.tar.bz2
upstart                   1.6.1        01 Jan 2013  http://upstart.ubuntu.com/download/1.6.1/upstart-1.6.1.tar.gz
uri                       5.31         10 Nov 2024  https://cpan.metacpan.org/authors/id/O/OA/OALDERS/URI-5.31.tar.gz
uriparser                 0.9.4        12 Nov 2020  https://sourceforge.net/projects/uriparser/files/Sources/0.9.4/uriparser-0.9.4.tar.xz
urlgrabber                4.0.0        27 Nov 2019  http://urlgrabber.baseurl.org/download/urlgrabber-4.0.0.tar.gz
urllib3                   1.26.8       13 Jan 2022  https://files.pythonhosted.org/packages/b0/b1/7bbf5181f8e3258efae31702f5eab87d8a74a72a0aa78bc8c08c1466e243/urllib3-1.26.8.tar.gz
urpkg                     1.4          01 May 2014  http://www.svasey.org/urpkg/urpkg-1.4.tar.bz2
usbutils                  018          23 Oct 2024  https://kernel.org/pub/linux/utils/usb/usbutils/usbutils-018.tar.xz
usermanager               5.18.6       29 Sep 2020  https://download.kde.org/stable/plasma/5.18.6/user-manager-5.18.6.tar.xz
userspacercu              0.12.1       12 Aug 2020  https://lttng.org/files/urcu/userspace-rcu-0.12.1.tar.bz2
v                         2.6.0        25 Nov 2020  https://github.com/JuliaStrings/utf8proc/archive/v2.6.0.tar.gz
utfcpp                    10.03.2024   10 Mar 2024  https://github.com/nemtrif/utfcpp/utfcpp-10.03.2024
utillinux                 2.40.4       14 Jan 2025  https://www.kernel.org/pub/linux/utils/util-linux/v2.40/util-linux-2.40.4.tar.xz
utilmacros                1.20.2       16 Nov 2024  https://www.x.org/releases/individual/util/util-macros-1.20.2.tar.xz
uuid                      1.6.2        01 Jul 2013  ftp://ftp.ossp.org/pkg/lib/uuid/uuid-1.6.2.tar.gz
v4lutils                  1.28.1       17 Jan 2025  https://www.linuxtv.org/downloads/v4l-utils/v4l-utils-1.28.1.tar.xz
vala                      0.56.17      19 Apr 2024  https://download.gnome.org/sources/vala/0.56/vala-0.56.17.tar.xz
valadoc                   0.36.2       04 Mar 2019  http://ftp.gnome.org/pub/gnome/sources/valadoc/0.36/valadoc-0.36.2.tar.xz
valencia                  0.8.0        01 Aug 2014  http://ftp.gnome.org/pub/GNOME/sources/valencia/0.8/valencia-0.8.0.tar.xz
valgrind                  3.24.0       03 Nov 2024  https://sourceware.org/pub/valgrind/valgrind-3.24.0.tar.bz2
valknut                   0.3.23       01 Nov 2017  https://sourceforge.net/projects/wxdcgui/files/valknut/0.3.23/valknut-0.3.23.tar.bz2
valkyrie                  2.0.0        17 Dec 2017  http://valgrind.org/downloads/valkyrie-2.0.0.tar.bz2
r                         65           19 Feb 2024  https://github.com/vapoursynth/vapoursynth/archive/refs/tags/R65.tar.gz
vc                        0.7.3        09 Aug 2015  http://code.compeng.uni-frankfurt.de/attachments/download/174/Vc-0.7.3.tar.gz
vcinit                    1.2          01 May 2014  http://www.dervishd.net/Projects/VCinit-1.2.tar.gz
veejay                    1.5.51       21 Apr 2019  https://sourceforge.net/projects/veejay/files/veejay-1.5/veejay-1.5.51.tar.bz2
vera                      1.23         10 Feb 2016  http://ftp.gnu.org/pub/gnu/vera/vera-1.23.tar.gz
vertris                   0.3.2        01 May 2014  http://vertris.googlecode.com/files/vertris-0.3.2.zip
vice                      1.19         01 May 2014  http://www.zimmers.net/anonftp/pub/cbm/crossplatform/emulators/VICE/vice-1.19.tar.gz
videocut?content=         60826        01 May 2014  http://www.kde-apps.org/content/show.php/VideoCut?content=60826
videoproto                2.3.3        12 Mar 2016  http://xorg.freedesktop.org/releases/individual/proto/videoproto-2.3.3.tar.gz
videotrans                1.6.1        01 May 2014  http://sourceforge.net/projects/videotrans/files/videotrans/1.6.1/videotrans-1.6.1.tar.bz2
viennarna                 2.7.0        15 Jan 2025  https://www.tbi.univie.ac.at/RNA/download/sourcecode/2_7_x/ViennaRNA-2.7.0.tar.gz
viewres                   1.0.7        16 Oct 2022  https://www.x.org/releases/individual/app/viewres-1.0.7.tar.xz
viewvc                    1.1.15       01 May 2014  http://viewvc.tigris.org/files/documents/3330/49223/viewvc-1.1.15.tar.gz
vim                       9.0          15 Jan 2025  https://ftp.vim.org/pub/vim/unix/vim-9.0.tar.bz2
vino                      3.22.0       20 Nov 2019  http://ftp.gnome.org/pub/GNOME/sources/vino/3.22/vino-3.22.0.tar.xz
virtualenv                20.17.1      21 Jan 2023  https://files.pythonhosted.org/packages/7b/19/65f13cff26c8cc11fdfcb0499cd8f13388dd7b35a79a376755f152b42d86/virtualenv-20.17.1.tar.gz
vitetris                  0.57         01 Nov 2012  http://www.victornils.net/tetris/vitetris-0.57.tar.gz
vlc                       3.0.21       08 Jun 2024  https://download.videolan.org/pub/videolan/vlc/3.0.21/vlc-3.0.21.tar.xz
vlock                     2.1          01 May 2014  http://cthulhu.c3d2.de/~toidinamai/vlock/vlock-2.1.tar.gz
vmd                       1.9.3        16 Jun 2017  https://www.ks.uiuc.edu/Research/vmd/vmd-1.9.3/files/final/vmd-1.9.3.tar.gz
vnstat                    2.2          30 Apr 2019  https://humdi.net/vnstat/vnstat-2.2.tar.gz
vobcopy                   1.1.0        01 May 2014  http://vobcopy.org/download/vobcopy-1.1.0.tar.bz2
volleyball                0.82         01 May 2014  http://www.losersjuegos.com.ar/juegos/volleyball/descargas/volleyball-0.82.tar.gz
volumekey                 0.3.12       12 Oct 2018  https://releases.pagure.org/volume_key/volume_key-0.3.12.tar.xz
vor                       0.5.6        11 Jun 2023  https://qualdan.com/vor/vor-0.5.6.tar.bz2
vorbistools               1.4.2        26 Jan 2021  https://downloads.xiph.org/releases/vorbis/vorbis-tools-1.4.2.tar.gz
vpnc                      0.5.3        01 May 2013  https://www.unix-ag.uni-kl.de/~massar/vpnc/vpnc-0.5.3.tar.gz
vrpe                      0.31         01 May 2014  http://vrpe.sytes.net/VRPE/VRPE-0.31.tar.bz2
vsftpd                    3.0.5        09 Aug 2021  https://security.appspot.com/downloads/vsftpd-3.0.5.tar.gz
vte                       0.78.2       24 Nov 2024  https://download.gnome.org/sources/vte/0.78/vte-0.78.2.tar.xz
vulkanheaders             1.3.301      15 Jan 2025  https://github.com/KhronosGroup/Vulkan-Headers/archive/v1.3.301/Vulkan-Headers-1.3.301.tar.gz
vulkanloader              1.3.301      15 Jan 2025  https://github.com/KhronosGroup/Vulkan-Loader/archive/v1.3.301/Vulkan-Loader-1.3.301.tar.gz
w3m                       0.5.3        30 Dec 2018  https://sourceforge.net/projects/w3m/files/w3m/w3m-0.5.3/w3m-0.5.3.tar.gz
wacomtablet               6.2.5        02 Jan 2025  https://download.kde.org/stable/plasma/6.2.5/wacomtablet-6.2.5.tar.xz
waf                       2.0.22       26 Apr 2021  https://waf.io/waf-2.0.22.tar.bz2
waon                      0.10         01 Mar 2014  https://sourceforge.net/projects/waon/files/waon/0.10/waon-0.10.tar.gz
warden                    1.2.8        30 Apr 2019  https://rubygems.org/downloads/warden-1.2.8.gem
warmux                    11.04.1      01 Jul 2014  http://sourceforge.net/projects/warmux/files/warmux-11.04.1.tar.bz2
warzone2100               3.1.2        26 Jul 2015  http://sourceforge.net/projects/warzone2100/files/releases/3.1.2/warzone2100-3.1.2.tar.xz
watir                     6.17.0       28 Oct 2020  https://rubygems.org/downloads/watir-6.17.0.gem
wavbreaker                0.12         21 Nov 2019  http://downloads.sourceforge.net/wavbreaker/wavbreaker-0.12.tar.gz
wavpack                   5.7.0        02 Mar 2024  https://github.com/dbry/WavPack/releases/download/5.7.0/wavpack-5.7.0.tar.xz
wayland                   1.23.1       26 Aug 2024  https://gitlab.freedesktop.org/wayland/wayland/-/releases/1.23.1/downloads/wayland-1.23.1.tar.xz
waylandprotocols          1.39         30 Dec 2024  https://gitlab.freedesktop.org/wayland/wayland-protocols/-/releases/1.39/downloads/wayland-protocols-1.39.tar.xz
wcd                       6.0.1-beta3  08 Aug 2017  https://waterlan.home.xs4all.nl/wcd/wcd-6.0.1-beta3.tar.gz
wdiff                     1.2.2        05 Mar 2021  https://ftp.gnu.org/gnu/wdiff/wdiff-1.2.2.tar.gz
webkit                    r189384      22 Dec 2015  http://builds.nightly.webkit.org/files/trunk/src/WebKit-r189384.tar.bz2
webkitgtk                 2.46.5       18 Jan 2025  https://webkitgtk.org/releases/webkitgtk-2.46.5.tar.xz
webmin                    2.111        14 Apr 2024  https://downloads.sourceforge.net/webadmin/webmin-2.111.tar.gz
webrick                   1.8.1        13 Mar 2023  https://rubygems.org/downloads/webrick-1.8.1.gem
websocketdriver           0.7.2        28 May 2020  https://rubygems.org/downloads/websocket-driver-0.7.2.gem
weechat                   3.1          02 Jun 2021  https://weechat.org/files/src/weechat-3.1.tar.xz
wesnoth                   1.18.3       29 Nov 2024  https://sourceforge.net/projects/wesnoth/files/wesnoth-1.18/wesnoth-1.18.3/wesnoth-1.18.3.tar.bz2
weston                    14.0.1       15 Jan 2025  https://gitlab.freedesktop.org/wayland/weston/-/releases/14.0.1/downloads/weston-14.0.1.tar.xz
wget                      1.25.0       12 Nov 2024  https://ftp.gnu.org/gnu/wget/wget-1.25.0.tar.gz
wget2                     2.2.0        24 Nov 2024  https://ftp.gnu.org/gnu/wget/wget2-2.2.0.tar.gz
wheel                     0.43.0       14 Mar 2024  https://files.pythonhosted.org/packages/b8/d6/ac9cd92ea2ad502ff7c1ab683806a9deb34711a1e2bd8a59814e8fc27e69/wheel-0.43.0.tar.gz
whenever                  1.0.0        22 May 2020  https://rubygems.org/downloads/whenever-1.0.0.gem
which                     2.21         17 Jun 2015  http://ftp.gnu.org/gnu/which/which-2.21.tar.gz
whois                     5.5.7        06 Oct 2020  http://ftp.debian.org/debian/pool/main/w/whois/whois_5.5.7.tar.xz
wicd                      1.7.4        25 Jan 2016  https://launchpad.net/wicd/1.7/1.7.4/+download/wicd-1.7.4.tar.gz
wimlib                    1.14.4       15 Mar 2024  https://wimlib.net/downloads/wimlib-1.14.4.tar.gz
windowmaker               0.96.0       07 Aug 2023  https://github.com/window-maker/wmaker/releases/download/wmaker-0.96.0/WindowMaker-0.96.0.tar.gz
windowswmproto            1.0.3        01 May 2014  https://www.x.org/releases/individual/proto/windowswmproto-1.0.3.tar.bz2
wine                      10.0         21 Jan 2025  https://dl.winehq.org/wine/source/10.0/wine-10.0.tar.xz
wings                     2.2.6.1      28 May 2020  https://sourceforge.net/projects/wings/files/wings/2.2.6.1/wings-2.2.6.1.tar.bz2
winzig                    1.73         01 May 2014  http://freshmeat.net/redir/winzig/20777/url_tgz/winzig.1.73.tar.gz
wirble                    0.1.3        23 Mar 2017  https://rubygems.org/downloads/wirble-0.1.3.gem
wireguard                 0.0.20170918 19 Sep 2017  https://git.zx2c4.com/WireGuard/snapshot/WireGuard-0.0.20170918.tar.xz
wireless                  tools.29     01 May 2014  http://www.hpl.hp.com/personal/Jean_Tourrilhes/Linux/wireless_tools.29.tar.gz
wireshark                 4.4.3        09 Jan 2025  https://www.wireshark.org/download/src/wireshark-4.4.3.tar.xz
wmctrl                    1.07         01 May 2014  http://www.sweb.cz/tripie/utils/wmctrl/dist/wmctrl-1.07.tar.gz
wmii                      20071003     01 May 2014  http://www.suckless.org/snaps/wmii-20071003.tgz
woff2                     1.0.2        22 Dec 2021  https://github.com/google/woff2/archive/v1.0.2/woff2-1.0.2.tar.gz
wordpress                 6.5          03 Apr 2024  https://wordpress.org/wordpress-6.5.tar.gz
worker                    4.7.0        10 Mar 2021  http://www.boomerangsworld.de/cms/worker/downloads/worker-4.7.0.tar.bz2
wpasupplicant             2.11         23 Jul 2024  https://w1.fi/releases/wpa_supplicant-2.11.tar.gz
wpebackendfdo             1.14.2       03 Nov 2024  https://wpewebkit.org/releases/wpebackend-fdo-1.14.2.tar.xz
wput                      0.6.1        01 May 2014  http://prdownloads.sourceforge.net/wput/wput-0.6.1.tgz
writeexcel                1.0.5        12 Aug 2017  https://rubygems.org/downloads/writeexcel-1.0.5.gem
wv                        1.2.9        01 Aug 2013  http://www.abisource.com/downloads/wv/1.2.9/wv-1.2.9.tar.gz
wv2                       0.4.2        01 May 2014  https://sourceforge.net/projects/wvware/files/wv2-0.4.2.tar.bz2
wxgtk                     2.6.3        01 May 2014  http://mesh.dl.sourceforge.net/sourceforge/wxwindows/wxGTK-2.6.3.tar.bz2
wxpython                  4.1.0        25 Nov 2020  https://files.pythonhosted.org/packages/cb/4f/1e21d3c079c973ba862a18f3be73c2bbe2e6bc25c96d94df605b5cbb494d/wxPython-4.1.0.tar.gz
wxsvg                     1.1.2        01 May 2014  http://sourceforge.net/projects/wxsvg/files/wxsvg/1.1.2/wxsvg-1.1.2.tar.bz2
wxwidgets                 3.2.5        19 Nov 2024  https://github.com/wxWidgets/wxWidgets/releases/download/v3.2.5/wxWidgets-3.2.5.tar.bz2
x11perf                   1.6.1        17 Mar 2019  https://www.x.org/releases/individual/app/x11perf-1.6.1.tar.gz
x264                      20240812     15 Jan 2025  https://anduin.linuxfromscratch.org/BLFS/x264/x264-20240812.tar.xz
x265                      20240216     18 Mar 2024  https://anduin.linuxfromscratch.org/BLFS/x265/x265-20240216.tar.xz
x86info                   1.30         01 May 2014  http://codemonkey.org.uk/projects/x86info/x86info-1.30.tgz
xapianbindings            1.4.22       05 Feb 2023  https://oligarchy.co.uk/xapian/1.4.22/xapian-bindings-1.4.22.tar.xz
xapiancore                1.4.25       10 Mar 2024  https://oligarchy.co.uk/xapian/1.4.25/xapian-core-1.4.25.tar.xz
xapianomega               1.4.9        11 Jan 2019  https://oligarchy.co.uk/xapian/1.4.9/xapian-omega-1.4.9.tar.xz
xarchiver                 0.5.4.23     03 Mar 2024  https://github.com/ib/xarchiver/archive/0.5.4.23/xarchiver-0.5.4.23.tar.gz
xaric                     0.13.7       06 Dec 2015  http://xaric.org/software/xaric/releases/xaric-0.13.7.tar.gz
xaudio                    0.6.1        01 Apr 2014  http://www.chaoticmind.net/~hcb/murx/xaudio/xaudio-0.6.1.tar.gz
xauth                     1.1.3        04 Mar 2024  https://www.x.org/releases/individual/app/xauth-1.1.3.tar.gz
xautomation               1.02         01 May 2014  http://hoopajoo.net/static/projects/xautomation-1.02.tar.gz
xawtv                     3.95         01 May 2014  http://dl.bytesex.org/releases/xawtv/xawtv-3.95.tar.gz
xbacklight                1.2.3        16 Jul 2019  http://xorg.freedesktop.org/releases/individual/app/xbacklight-1.2.3.tar.bz2
xbak                      0.1.5        01 May 2014  http://sourceforge.net/projects/xbak/files/xbak/xbak-0.1.5/xbak-0.1.5.tar.bz2
xbiff                     1.0.5        01 Feb 2024  https://www.x.org/releases/individual/app/xbiff-1.0.5.tar.xz
xbindkeys                 1.8.5        01 May 2014  http://www.nongnu.org/xbindkeys/xbindkeys-1.8.5.tar.gz
xbitmaps                  1.1.3        03 Mar 2023  https://www.x.org/releases/individual/data/xbitmaps-1.1.3.tar.xz
xboard                    4.9.1        08 Aug 2016  http://ftp.gnu.org/gnu/xboard/xboard-4.9.1.tar.gz
xcalc                     1.1.2        06 May 2023  https://www.x.org/releases/individual/app/xcalc-1.1.2.tar.xz
xcbproto                  1.17.0       16 Apr 2024  https://xcb.freedesktop.org/dist/xcb-proto-1.17.0.tar.xz
xcbutil                   0.4.1        23 Jan 2023  https://xcb.freedesktop.org/dist/xcb-util-0.4.1.tar.xz
xcbutilcursor             0.1.5        18 Nov 2023  https://www.x.org/releases/individual/lib/xcb-util-cursor-0.1.5.tar.xz
xcbutilerrors             1.0.1        19 Oct 2022  https://www.x.org/releases/individual/lib/xcb-util-errors-1.0.1.tar.xz
xcbutilimage              0.4.1        19 Oct 2022  https://www.x.org/releases/individual/lib/xcb-util-image-0.4.1.tar.xz
xcbutilkeysyms            0.4.1        19 Oct 2022  https://www.x.org/releases/individual/lib/xcb-util-keysyms-0.4.1.tar.xz
xcbutilrenderutil         0.3.10       19 Oct 2022  https://www.x.org/releases/individual/lib/xcb-util-renderutil-0.3.10.tar.xz
xcbutilwm                 0.4.2        19 Oct 2022  https://www.x.org/releases/individual/lib/xcb-util-wm-0.4.2.tar.xz
xcbutilxrm                1.0          22 Nov 2016  https://github.com/Airblader/xcb-util-xrm/releases/download/v1.0/xcb-util-xrm-1.0.tar.bz2
xchat                     2.8.8        01 Apr 2014  http://xchat.org/files/source/2.8/xchat-2.8.8.tar.bz2
xchm                      1.27         22 Mar 2019  https://github.com/rzvncj/xCHM/releases/download/1.27/xchm-1.27.tar.gz
xclip                     10.03.2023   01 May 2014  https://downloads.sourceforge.net/project/xclip/xclip/0.12/xclip-10.03.2023.tar.xz
xclipboard                1.1.5        28 Aug 2024  https://www.x.org/releases/individual/app/xclipboard-1.1.5.tar.xz
xclock                    1.1.0        05 Apr 2022  https://www.x.org/pub/individual/app/xclock-1.1.0.tar.xz
xcm                       0.5.3        01 May 2014  https://sourceforge.net/projects/oyranos/files/Xcm/xcm-0.5.3.tar.bz2
xcmiscproto               1.2.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/proto/xcmiscproto-1.2.2.tar.bz2
xcmsdb                    1.0.7        15 Oct 2024  https://www.x.org/releases/individual/app/xcmsdb-1.0.7.tar.xz
xcompmgr                  1.1.8        26 Mar 2019  https://www.x.org/releases/individual/app/xcompmgr-1.1.8.tar.gz
xconsole                  1.1.0        30 Apr 2024  https://www.x.org/releases/individual/app/xconsole-1.1.0.tar.xz
xcursorgen                1.0.8        04 Dec 2022  https://www.x.org/releases/individual/app/xcursorgen-1.0.8.tar.xz
xcursorthemes             1.0.6        16 Feb 2019  https://www.x.org/releases/individual/data/xcursor-themes-1.0.6.tar.bz2
xdbedizzy                 1.0.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/app/xdbedizzy-1.0.2.tar.bz2
xdg                       4.1.0        28 May 2020  https://rubygems.org/downloads/xdg-4.1.0.gem
xdgdbusproxy              0.1.5        20 Jan 2024  https://github.com/flatpak/xdg-dbus-proxy/releases/download/0.1.5/xdg-dbus-proxy-0.1.5.tar.xz
xdgdesktopportal          1.18.4       22 Jan 2025  https://github.com/flatpak/xdg-desktop-portal/releases/download/1.18.4/xdg-desktop-portal-1.18.4.tar.xz
xdgdesktopportalgnome     46.0         27 Mar 2024  https://download.gnome.org/sources/xdg-desktop-portal-gnome/46/xdg-desktop-portal-gnome-46.0.tar.xz
xdgdesktopportalkde       5.27.9       24 Oct 2023  https://download.kde.org/stable/plasma/5.27.9/xdg-desktop-portal-kde-5.27.9.tar.xz
xdguserdirs               0.17         20 Mar 2018  https://user-dirs.freedesktop.org/releases/xdg-user-dirs-0.17.tar.gz
xdguserdirsgtk            0.11         14 Oct 2022  https://download.gnome.org/sources/xdg-user-dirs-gtk/0.11/xdg-user-dirs-gtk-0.11.tar.xz
xdgutils                  1.1.3        26 Jan 2019  https://portland.freedesktop.org/download/xdg-utils-1.1.3.tar.gz
xdialog                   2.3.1        01 May 2014  http://xdialog.free.fr/Xdialog-2.3.1.tar.bz2
xditview                  1.0.7        20 Feb 2024  https://www.x.org/releases/individual/app/xditview-1.0.7.tar.xz
xdm                       1.1.16       05 Apr 2024  https://www.x.org/releases/individual/app/xdm-1.1.16.tar.xz
xdotool                   3.20210903.1 17 Oct 2021  https://github.com/jordansissel/xdotool/releases/download/v3.20210903.1/xdotool-3.20210903.1.tar.gz
xdpyinfo                  1.3.4        29 Apr 2023  https://www.x.org/releases/individual/app/xdpyinfo-1.3.4.tar.xz
xdriinfo                  1.0.5        18 Apr 2015  https://xorg.freedesktop.org/releases/individual/app/xdriinfo-1.0.5.tar.gz
xdtv                      2.4.0        31 Aug 2017  https://sourceforge.net/projects/xawdecode/files/01%20-%20XdTV/2.4.0/xdtv-2.4.0.tar.gz
xedit                     1.2.4        25 Mar 2024  https://www.x.org/releases/individual/app/xedit-1.2.4.tar.xz
xemacs                    21.5.35      01 Apr 2024  http://ftp.xemacs.org/pub/xemacs/xemacs-21.5/xemacs-21.5.35.tar.gz
xercesc                   3.2.3        23 Jul 2020  http://www-us.apache.org/dist//xerces/c/3/sources/xerces-c-3.2.3.tar.xz
xev                       1.2.6        05 Mar 2024  https://www.x.org/releases/individual/app/xev-1.2.6.tar.xz
xextproto                 7.2.99.901   01 Nov 2013  https://xorg.freedesktop.org/releases/individual/proto/xextproto-7.2.99.901.tar.bz2
xf86bigfontproto          1.2.0        01 May 2014  http://xorg.freedesktop.org/releases/individual/proto/xf86bigfontproto-1.2.0.tar.bz2
xf86dga                   1.0.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/app/xf86dga-1.0.2.tar.bz2
xf86dgaproto              2.1          01 Jan 2013  http://xorg.freedesktop.org/releases/individual/proto/xf86dgaproto-2.1.tar.bz2
xf86driproto              2.1.1        01 May 2014  http://xorg.freedesktop.org/releases/individual/proto/xf86driproto-2.1.1.tar.bz2
xf86inputaiptek           1.0.0.5      01 May 2014  https://www.x.org/releases/individual/driver/xf86-input-aiptek-1.0.0.5.tar.bz2
xf86inputelographics      1.4.4        01 Mar 2024  https://www.x.org/releases/individual/driver/xf86-input-elographics-1.4.4.tar.xz
xf86inputevdev            2.11.0       18 Oct 2024  https://www.x.org/releases/individual/driver/xf86-input-evdev-2.11.0.tar.xz
xf86inputevtouch          0.8.8        01 May 2012  http://www.conan.de/touchscreen/xf86-input-evtouch-0.8.8.tar.bz2
xf86inputfpit             1.4.0        22 Dec 2015  https://www.x.org/releases/individual/driver/xf86-input-fpit-1.4.0.tar.gz
xf86inputhyperpen         1.4.1        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-input-hyperpen-1.4.1.tar.bz2
xf86inputjoystick         1.6.3        24 Nov 2016  https://www.x.org/releases/individual/driver/xf86-input-joystick-1.6.3.tar.bz2
xf86inputkeyboard         2.1.0        16 Jan 2025  https://www.x.org/releases/individual/driver/xf86-input-keyboard-2.1.0.tar.xz
xf86inputlibinput         1.5.0        18 Oct 2024  https://www.x.org/releases/individual/driver/xf86-input-libinput-1.5.0.tar.xz
xf86inputmouse            1.9.3        02 Jul 2018  https://www.x.org/releases/individual/driver/xf86-input-mouse-1.9.3.tar.gz
xf86inputmutouch          1.2.1        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-input-mutouch-1.2.1.tar.bz2
xf86inputpenmount         1.4.0        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-input-penmount-1.4.0.tar.bz2
xf86inputsynaptics        1.10.0       16 Jan 2025  https://www.x.org/releases/individual/driver/xf86-input-synaptics-1.10.0.tar.xz
xf86inputvmmouse          13.2.0       16 Oct 2022  http://xorg.freedesktop.org/releases/individual/driver/xf86-input-vmmouse-13.2.0.tar.gz
xf86inputvoid             1.4.1        14 Dec 2018  https://www.x.org/releases/individual/driver/xf86-input-void-1.4.1.tar.bz2
xf86inputwacom            1.2.2        16 Apr 2024  https://github.com/linuxwacom/xf86-input-wacom/releases/download/xf86-input-wacom-1.2.2/xf86-input-wacom-1.2.2.tar.bz2
xf86miscproto             0.9.3        01 May 2014  http://xorg.freedesktop.org/releases/individual/proto/xf86miscproto-0.9.3.tar.bz2
xf86rushproto             1.1.2        01 May 2014  https://www.x.org/releases/individual/proto/xf86rushproto-1.1.2.tar.bz2
xf86videoamd              2.7.7.7      13 Mar 2008  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-amd-2.7.7.7.tar.bz2
xf86videoamdgpu           23.0.0       26 Feb 2023  https://www.x.org/releases/individual/driver/xf86-video-amdgpu-23.0.0.tar.xz
xf86videoapm              1.3.0        08 Feb 2019  https://www.x.org/releases/individual/driver/xf86-video-apm-1.3.0.tar.bz2
xf86videoark              0.7.4        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-ark-0.7.4.tar.bz2
xf86videoast              1.2.0        16 Jan 2025  https://www.x.org/releases/individual/driver/xf86-video-ast-1.2.0.tar.xz
xf86videoati              19.1.0       15 Oct 2019  https://www.x.org/releases/individual/driver/xf86-video-ati-19.1.0.tar.gz
xf86videochips            1.5.0        25 Mar 2024  https://www.x.org/releases/individual/driver/xf86-video-chips-1.5.0.tar.xz
xf86videocirrus           1.6.0        20 Aug 2022  https://www.x.org/releases/individual/driver/xf86-video-cirrus-1.6.0.tar.xz
xf86videodummy            0.4.0        20 Aug 2022  https://www.x.org/releases/individual/driver/xf86-video-dummy-0.4.0.tar.xz
xf86videofbdev            0.5.1        16 Jan 2025  https://www.x.org/releases/individual/driver/xf86-video-fbdev-0.5.1.tar.xz
xf86videofreedreno        1.4.0        12 Oct 2015  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-freedreno-1.4.0.tar.bz2
xf86videogeode            2.11.20      22 Sep 2019  https://www.x.org/releases/individual/driver/xf86-video-geode-2.11.20.tar.gz
xf86videoglide            1.2.2        02 Apr 2019  ftp://ftp.x.org/pub/individual/driver/xf86-video-glide-1.2.2.tar.gz
xf86videoi128             1.4.0        01 May 2014  https://www.x.org/releases/individual/driver/xf86-video-i128-1.4.0.tar.gz
xf86videoi740             1.4.0        01 May 2014  https://www.x.org/releases/individual/driver/xf86-video-i740-1.4.0.tar.gz
xf86videoi810             1.7.4        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-i810-1.7.4.tar.bz2
xf86videointel            2.99.917     11 Apr 2015  ftp://ftp.x.org/pub/individual/driver/xf86-video-intel-2.99.917.tar.bz2
xf86videomach64           6.9.7        20 Aug 2022  https://www.x.org/releases/individual/driver/xf86-video-mach64-6.9.7.tar.xz
xf86videomga              2.1.0        18 Oct 2024  https://www.x.org/releases/individual/driver/xf86-video-mga-2.1.0.tar.xz
xf86videomodesetting      0.9.0        01 Jun 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-modesetting-0.9.0.tar.gz
xf86videoneomagic         1.3.0        27 Dec 2018  https://www.x.org/releases/individual/driver/xf86-video-neomagic-1.3.0.tar.gz
xf86videonewport          0.2.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-newport-0.2.2.tar.bz2
xf86videonouveau          1.0.18       16 Jan 2025  https://www.x.org/releases/individual/driver/xf86-video-nouveau-1.0.18.tar.xz
xf86videonv               2.1.23       25 Mar 2024  https://www.x.org/releases/individual/driver/xf86-video-nv-2.1.23.tar.xz
xf86videoomap             0.4.5        24 May 2020  https://www.x.org/releases/individual/driver/xf86-video-omap-0.4.5.tar.gz
xf86videoopenchrome       0.6.0        08 Mar 2017  https://www.x.org/releases/individual/driver/xf86-video-openchrome-0.6.0.tar.bz2
xf86videoopentegra        0.7.0        01 Jul 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-opentegra-0.7.0.tar.xz
xf86videoqxl              0.1.5        31 Dec 2016  https://www.x.org/releases/individual/driver/xf86-video-qxl-0.1.5.tar.bz2
xf86videoradeonhd         1.3.0        01 Nov 2017  https://www.x.org/releases/individual/driver/xf86-video-radeonhd-1.3.0.tar.bz2
xf86videorendition        4.2.7        23 May 2018  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-rendition-4.2.7.tar.gz
xf86videos3               0.7.0        27 Jul 2019  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-s3-0.7.0.tar.bz2
xf86videos3virge          1.11.0       09 Feb 2019  https://www.x.org/releases/individual/driver/xf86-video-s3virge-1.11.0.tar.gz
xf86videosavage           2.4.1        25 Mar 2024  https://www.x.org/releases/individual/driver/xf86-video-savage-2.4.1.tar.xz
xf86videosiliconmotion    1.7.0        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-siliconmotion-1.7.0.tar.bz2
xf86videosis              0.12.0       04 Dec 2019  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-sis-0.12.0.tar.bz2
xf86videosisusb           0.9.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-sisusb-0.9.2.tar.bz2
xf86videotdfx             1.5.0        17 Feb 2019  https://www.x.org/releases/individual/driver/xf86-video-tdfx-1.5.0.tar.bz2
xf86videotga              1.2.1        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-tga-1.2.1.tar.bz2
xf86videotrident          1.4.0        27 Oct 2016  https://xorg.freedesktop.org/archive/individual/driver/xf86-video-trident-1.4.0.tar.xz
xf86videotseng            1.2.5        05 Apr 2024  https://www.x.org/releases/individual/driver/xf86-video-tseng-1.2.5.tar.gz
xf86videov4l              0.3.0        16 Aug 2018  https://www.x.org/releases/individual/driver/xf86-video-v4l-0.3.0.tar.gz
xf86videovboxvideo        1.0.1        25 Mar 2024  https://www.x.org/releases/individual/driver/xf86-video-vboxvideo-1.0.1.tar.xz
xf86videovesa             2.6.0        12 Dec 2022  https://www.x.org/releases/individual/driver/xf86-video-vesa-2.6.0.tar.xz
xf86videovia              0.2.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-via-0.2.2.tar.bz2
xf86videovmware           13.4.0       29 Jan 2023  https://www.x.org/releases/individual/driver/xf86-video-vmware-13.4.0.tar.gz
xf86videovoodoo           1.2.2        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-voodoo-1.2.2.tar.bz2
xf86videowsfb             0.4.0        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-wsfb-0.4.0.tar.bz2
xf86videoxgi              1.6.1        22 Aug 2015  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-xgi-1.6.1.tar.bz2
xf86videoxgixp            1.8.0        01 May 2014  http://xorg.freedesktop.org/releases/individual/driver/xf86-video-xgixp-1.8.0.tar.bz2
xfburn                    0.7.1        16 Jul 2024  https://archive.xfce.org/src/apps/xfburn/0.7/xfburn-0.7.1.tar.bz2
xfce4appfinder            4.16.1       26 Apr 2021  https://archive.xfce.org/src/xfce/xfce4-appfinder/4.16/xfce4-appfinder-4.16.1.tar.bz2
xfce4clipmanplugin        1.6.2        05 May 2021  https://archive.xfce.org/src/panel-plugins/xfce4-clipman-plugin/1.6/xfce4-clipman-plugin-1.6.2.tar.bz2
xfce4cpufreqplugin        1.2.0        19 Sep 2018  http://archive.xfce.org/src/panel-plugins/xfce4-cpufreq-plugin/1.2/xfce4-cpufreq-plugin-1.2.0.tar.bz2
xfce4devtools             4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/xfce4-dev-tools/4.20/xfce4-dev-tools-4.20.0.tar.bz2
xfce4eyesplugin           4.5.0        25 Sep 2017  http://archive.xfce.org/src/panel-plugins/xfce4-eyes-plugin/4.5/xfce4-eyes-plugin-4.5.0.tar.bz2
xfce4genmonplugin         4.0.0        26 Sep 2017  http://archive.xfce.org/src/panel-plugins/xfce4-genmon-plugin/4.0/xfce4-genmon-plugin-4.0.0.tar.bz2
xfce4mpcplugin            0.5.0        30 Aug 2017  http://archive.xfce.org/src/panel-plugins/xfce4-mpc-plugin/0.5/xfce4-mpc-plugin-0.5.0.tar.bz2
xfce4notifyd              0.8.2        03 Mar 2023  https://archive.xfce.org/src/apps/xfce4-notifyd/0.8/xfce4-notifyd-0.8.2.tar.bz2
xfce4panel                4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/xfce4-panel/4.20/xfce4-panel-4.20.0.tar.bz2
xfce4panelprofiles        1.0.10       06 Feb 2020  https://archive.xfce.org/src/apps/xfce4-panel-profiles/1.0/xfce4-panel-profiles-1.0.10.tar.bz2
xfce4powermanager         4.18.3       03 Dec 2023  https://archive.xfce.org/src/xfce/xfce4-power-manager/4.18/xfce4-power-manager-4.18.3.tar.bz2
xfce4pulseaudioplugin     0.4.7        03 Jun 2023  https://archive.xfce.org/src/panel-plugins/xfce4-pulseaudio-plugin/0.4/xfce4-pulseaudio-plugin-0.4.7.tar.bz2
xfce4sampleplugin         0.1.2        25 Sep 2017  http://archive.xfce.org/src/panel-plugins/xfce4-sample-plugin/0.1/xfce4-sample-plugin-0.1.2.tar.gz
xfce4screensaver          4.18.3       06 Mar 2024  https://archive.xfce.org/src/apps/xfce4-screensaver/4.18/xfce4-screensaver-4.18.3.tar.bz2
xfce4screenshooter        1.9.8        13 Mar 2022  http://archive.xfce.org/src/apps/xfce4-screenshooter/1.9/xfce4-screenshooter-1.9.8.tar.bz2
xfce4session              4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/xfce4-session/4.20/xfce4-session-4.20.0.tar.bz2
xfce4settings             4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/xfce4-settings/4.20/xfce4-settings-4.20.0.tar.bz2
xfce4statusnotifierplugin 0.2.0        26 Sep 2017  http://archive.xfce.org/src/panel-plugins/xfce4-statusnotifier-plugin/0.2/xfce4-statusnotifier-plugin-0.2.0.tar.bz2
xfce4systemloadplugin     1.2.3        18 Aug 2019  https://archive.xfce.org/src/panel-plugins/xfce4-systemload-plugin/1.2/xfce4-systemload-plugin-1.2.3.tar.bz2
xfce4taskmanager          1.4.0        30 Dec 2020  https://git.xfce.org/apps/xfce4-taskmanager/snapshot/xfce4-taskmanager-1.4.0.tar.gz
xfce4terminal             1.1.3        01 Mar 2024  https://archive.xfce.org/src/apps/xfce4-terminal/1.1/xfce4-terminal-1.1.3.tar.bz2
xfce4whiskermenuplugin    2.6.2        16 Nov 2021  https://archive.xfce.org/src/panel-plugins/xfce4-whiskermenu-plugin/2.6/xfce4-whiskermenu-plugin-2.6.2.tar.bz2
xfce4xkbplugin            0.8.1        26 Sep 2017  http://archive.xfce.org/src/panel-plugins/xfce4-xkb-plugin/0.8/xfce4-xkb-plugin-0.8.1.tar.bz2
xfconf                    4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/xfconf/4.20/xfconf-4.20.0.tar.bz2
xfd                       1.1.3        11 Mar 2019  https://xorg.freedesktop.org/releases/individual/app/xfd-1.1.3.tar.bz2
xfdesktop                 4.20.0       15 Dec 2024  https://archive.xfce.org/src/xfce/xfdesktop/4.20/xfdesktop-4.20.0.tar.bz2
xfe                       1.33         01 May 2014  http://sourceforge.net/projects/xfe/files/xfe/1.33/xfe-1.33.tar.gz
xfig                      3.2.8        01 Apr 2023  https://sourceforge.net/projects/mcj/files/xfig-3.2.8.tar.xz
xfindproxy                1.0.4        18 Apr 2015  https://xorg.freedesktop.org/releases/individual/app/xfindproxy-1.0.4.tar.gz
xfmedia                   0.9.2        30 Jul 2022  https://www.spurint.org/files/xfmedia/xfmedia-0.9.2.tar.bz2
xfontsel                  1.1.1        05 Mar 2024  https://www.x.org/releases/individual/app/xfontsel-1.1.1.tar.xz
xforge                    0.2.2        01 May 2014  http://users.tkk.fi/~phonkane/xforge/xforge-0.2.2.tar.gz
xfs                       1.2.2        28 Jul 2024  https://www.x.org/releases/individual/app/xfs-1.2.2.tar.xz
xfsdump                   3.1.12       14 Jun 2023  https://mirrors.edge.kernel.org/pub/linux/utils/fs/xfs/xfsdump/xfsdump-3.1.12.tar.xz
xfsinfo                   1.0.7        24 Oct 2022  https://www.x.org/releases/individual/app/xfsinfo-1.0.7.tar.xz
xfsprogs                  6.12.0       03 Dec 2024  https://www.kernel.org/pub/linux/utils/fs/xfs/xfsprogs/xfsprogs-6.12.0.tar.xz
xft                       2.1.2        01 Dec 2012  http://xlibs.freedesktop.org/release/xft-2.1.2.tar.gz
xfwm4                     4.20.0       20 Dec 2024  https://archive.xfce.org/src/xfce/xfwm4/4.20/xfwm4-4.20.0.tar.bz2
xfwp                      1.0.3        01 May 2014  https://xorg.freedesktop.org/releases/individual/app/xfwp-1.0.3.tar.bz2
xgamma                    1.0.7        01 Apr 2023  https://xorg.freedesktop.org/releases/individual/app/xgamma-1.0.7.tar.xz
xgc                       1.0.6        16 Oct 2022  https://xorg.freedesktop.org/releases/individual/app/xgc-1.0.6.tar.gz
xhost                     1.0.9        15 Dec 2022  https://www.x.org/releases/individual/app/xhost1.0.9.tar.xz
xinelib                   1.2.13       30 May 2023  https://sourceforge.net/projects/xine/files/xine-lib/1.2.13/xine-lib-1.2.13.tar.xz
xinetd                    2.3.15       01 May 2014  http://www.xinetd.org/xinetd-2.3.15.tar.gz
xineui                    0.99.14      11 Jan 2023  https://sourceforge.net/projects/xine/files/xine-ui/0.99.14/xine-ui-0.99.14.tar.xz
xinit                     1.4.3        14 Jan 2025  https://www.x.org/releases/individual/app/xinit-1.4.3.tar.xz
xinput                    1.6.4        29 Apr 2023  https://xorg.freedesktop.org/releases/individual/app/xinput-1.6.4.tar.xz
xisxwayland               2            01 Sep 2022  https://www.x.org/releases/individual/app/xisxwayland-2.tar.xz
xkbcomp                   1.4.7        20 Feb 2024  https://www.x.org/releases/individual/app/xkbcomp-1.4.7.tar.xz
xkbdata                   1.0.1        01 May 2014  https://www.x.org/releases/individual/data/xkbdata-1.0.1.tar.gz
xkbevd                    1.1.5        16 Nov 2022  https://www.x.org/releases/individual/app/xkbevd-1.1.5.tar.xz
xkbprint                  1.0.7        15 Oct 2024  https://www.x.org/releases/individual/app/xkbprint-1.0.7.tar.xz
xkbutils                  1.0.6        20 Feb 2024  https://www.x.org/releases/individual/app/xkbutils-1.0.6.tar.xz
xkeyboardconfig           2.43         05 Oct 2024  https://www.x.org/pub/individual/data/xkeyboard-config/xkeyboard-config-2.43.tar.xz
xkill                     1.0.6        16 Nov 2022  https://www.x.org/releases/individual/app/xkill-1.0.6.tar.xz
xload                     1.2.0        25 Mar 2024  https://www.x.org/releases/individual/app/xload-1.2.0.tar.xz
xlogo                     1.0.7        14 Nov 2024  https://www.x.org/releases/individual/app/xlogo-1.0.7.tar.xz
xlsatoms                  1.1.3        21 Feb 2019  http://xorg.freedesktop.org/releases/individual/app/xlsatoms-1.1.3.tar.gz
xlsclients                1.1.5        16 Nov 2022  https://www.x.org/releases/individual/app/xlsclients-1.1.5.tar.xz
xlsfonts                  1.0.8        04 Mar 2024  https://www.x.org/releases/individual/app/xlsfonts-1.0.8.tar.xz
xmag                      1.0.8        15 Oct 2024  https://www.x.org/releases/individual/app/xmag-1.0.8.tar.xz
xmahjongg                 3.7          01 May 2014  http://www.lcdf.org/xmahjongg/xmahjongg-3.7.tar.gz
xman                      1.2.0        25 Mar 2024  https://www.x.org/releases/individual/app/xman-1.2.0.tar.xz
xmedcon                   0.21.0       05 Apr 2021  https://prdownloads.sourceforge.net/xmedcon/xmedcon-0.21.0.tar.bz2
xmessage                  1.0.7        05 Mar 2024  https://www.x.org/releases/individual/app/xmessage-1.0.7.tar.xz
xmh                       1.0.5        23 Mar 2024  https://xorg.freedesktop.org/releases/individual/app/xmh-1.0.5.tar.xz
xmlcopyeditor             1.0.7.7      01 May 2014  http://puzzle.dl.sourceforge.net/sourceforge/xml-copy-editor/xmlcopyeditor-1.0.7.7.tar.gz
xmldom                    1.46         21 Jan 2025  https://cpan.metacpan.org/authors/id/T/TJ/TJMATHER/XML-DOM-1.46.tar.gz
xmllibxmlsimple           1.01         17 Jan 2020  https://cpan.metacpan.org/authors/id/M/MA/MARKOV/XML-LibXML-Simple-1.01.tar.gz
xmlnamespacesupport       1.12         14 Feb 2019  https://cpan.metacpan.org/authors/id/P/PE/PERIGRIN/XML-NamespaceSupport-1.12.tar.gz
xmlparser                 2.47         21 Jan 2024  https://cpan.metacpan.org/authors/id/T/TO/TODDR/XML-Parser-2.47.tar.gz
xmlsax                    1.02         23 Jun 2020  https://cpan.metacpan.org/authors/id/G/GR/GRANTM/XML-SAX-1.02.tar.gz
xmlsaxbase                1.09         28 Dec 2017  http://search.cpan.org/CPAN/authors/id/G/GR/GRANTM/XML-SAX-Base-1.09.tar.gz
xmlsec1                   1.3.3        23 Feb 2024  https://www.aleksey.com/xmlsec/download/xmlsec1-1.3.3.tar.gz
xmlsimple                 2.25         26 Mar 2018  http://www.cpan.org/authors/id/G/GR/GRANTM/XML-Simple-2.25.tar.gz
xmlsmart                  1.78         12 Apr 2019  https://cpan.metacpan.org/authors/id/T/TM/TMHARISH/XML-Smart-1.78.tar.gz
xmlto                     0.0.28       11 Aug 2016  https://releases.pagure.org/xmlto/xmlto-0.0.28.tar.bz2
xmltv                     0.5.70       28 Jun 2021  https://sourceforge.net/projects/xmltv/files/xmltv/0.5.70/xmltv-0.5.70.tar.bz2
xmodmap                   1.0.11       12 Jul 2022  https://www.x.org/releases/individual/app/xmodmap-1.0.11.tar.xz
xmore                     1.0.4        20 Feb 2024  https://www.x.org/releases/individual/app/xmore-1.0.4.tar.xz
xmp                       4.1.0        28 Jul 2018  https://sourceforge.net/projects/xmp/files/xmp/4.1.0/xmp-4.1.0.tar.gz
xorgcffiles               1.0.6        25 Dec 2015  https://xorg.freedesktop.org/releases/individual/util/xorg-cf-files-1.0.6.tar.gz
xorgproto                 2024.1       27 Mar 2024  https://www.x.org/releases/individual/proto/xorgproto-2024.1.tar.xz
xorgserver                21.1.15      18 Dec 2024  https://www.x.org/archive/individual/xserver/xorg-server-21.1.15.tar.xz
xorgsetup                 0.9.7        22 Dec 2015  ftp://ftp.slackware.org.uk/slacky/slackware-12.1/hardware/x.org-setup/0.9.7/src/xorgsetup-0.9.7.tar.bz2
xorgsgmldoctools          1.12         03 Feb 2024  https://www.x.org/archive//individual/doc/xorg-sgml-doctools-1.12.tar.gz
xorriso                   1.5.6        14 Jun 2023  https://ftp.gnu.org/pub/gnu/xorriso/xorriso-1.5.6.tar.gz
xosd                      2.2.14       01 Dec 2013  https://sourceforge.net/projects/libxosd/files/libxosd/xosd-2.2.14/xosd-2.2.14.tar.gz
xournal                   0.4.8.2016   08 Aug 2020  https://sourceforge.net/projects/xournal/files/xournal/0.4.8.2016/xournal-0.4.8.2016.tar.gz
xpad                      4.5.0        22 Apr 2015  https://launchpad.net/xpad/trunk/4.5/+download/xpad-4.5.0.tar.bz2
xpaint                    3.1.4        08 Oct 2021  https://sourceforge.net/projects/sf-xpaint/files/sf-xpaint/xpaint-3.1.4/xpaint-3.1.4.tar.bz2
xpdf                      4.04         04 Dec 2023  https://dl.xpdfreader.com/xpdf-4.04.tar.gz
xphelloworld              1.0.1        01 May 2014  https://xorg.freedesktop.org/releases/individual/app/xphelloworld-1.0.1.tar.bz2
xplsprinters              1.0.1        01 May 2014  https://xorg.freedesktop.org/releases/individual/app/xplsprinters-1.0.1.tar.bz2
xpr                       1.2.0        05 Mar 2024  https://www.x.org/releases/individual/app/xpr-1.2.0.tar.xz
xpra                      2.3.3        24 Aug 2018  https://www.xpra.org/src/xpra-2.3.3.tar.xz
xprehashprinterlist       1.0.1        01 May 2014  https://xorg.freedesktop.org/releases/individual/app/xprehashprinterlist-1.0.1.tar.bz2
xprop                     1.2.8        14 Nov 2024  https://www.x.org/releases/individual/app/xprop-1.2.8.tar.xz
xproto                    7.0.31       08 Oct 2016  https://www.x.org/releases/individual/proto/xproto-7.0.31.tar.gz
xpyb                      1.3.1        09 Jan 2018  https://xcb.freedesktop.org/dist/xpyb-1.3.1.tar.gz
xrandr                    1.5.3        10 Nov 2024  https://www.x.org/releases/individual/app/xrandr-1.5.3.tar.xz
xrdb                      1.2.2        05 Jun 2023  https://www.x.org/releases/individual/app/xrdb-1.2.2.tar.xz
xrefresh                  1.1.0        05 Mar 2024  https://www.x.org/releases/individual/app/xrefresh-1.1.0.tar.xz
xrender                   0.8.3        01 Nov 2012  http://freeware.sgi.com/source/xrender/xrender-0.8.3.tar.bz2
xrick                     021212       01 May 2014  http://www.bigorno.net/xrick/xrick-021212.tgz
xrx                       1.0.4        16 Apr 2019  https://www.x.org/releases/individual/app/xrx-1.0.4.tar.gz
xsane                     0.999        28 Dec 2017  http://anduin.linuxfromscratch.org/BLFS/xsane/xsane-0.999.tar.gz
xscope                    1.4.4        05 Jun 2023  https://www.x.org/releases/individual/app/xscope-1.4.4.tar.xz
xscreensaver              6.08         24 Feb 2024  https://www.jwz.org/xscreensaver/xscreensaver-6.08.tar.gz
xsel                      1.2.0        15 Mar 2024  http://www.vergenet.net/~conrad/software/xsel/download/xsel-1.2.0.tar.gz
xset                      1.2.5        04 Dec 2022  https://www.x.org/releases/individual/app/xset-1.2.5.tar.xz
xsetmode                  1.0.0        01 Jan 2014  https://www.x.org/releases/individual/app/xsetmode-1.0.0.tar.bz2
xsetpointer               1.0.0        01 May 2014  https://xorg.freedesktop.org/releases/individual/app/xsetpointer-1.0.0.tar.bz2
xsetroot                  1.1.3        31 Oct 2022  https://xorg.freedesktop.org/releases/individual/app/xsetroot-1.1.3.tar.xz
xsm                       1.0.6        07 Mar 2024  https://www.x.org/releases/individual/app/xsm-1.0.6.tar.xz
xstdcmap                  1.0.5        04 Dec 2022  https://www.x.org/releases/individual/app/xstdcmap-1.0.5.tar.xz
xterm                     397          15 Jan 2025  https://invisible-mirror.net/archives/xterm/xterm-397.tgz
xtermcontrol              3.7          28 Mar 2019  http://thrysoee.dk/xtermcontrol/xtermcontrol-3.7.tar.gz
xtrace                    0.0.5        01 May 2014  https://xtrace.sourceforge.net/xtrace-0.0.5.tar.gz
xtrans                    1.5.2        12 Nov 2024  https://www.x.org/releases/individual/lib/xtrans-1.5.2.tar.xz
xtrap                     1.0.3        18 Apr 2018  https://www.x.org/releases/individual/app/xtrap-1.0.3.tar.bz2
xu4                       1.0beta3     01 Oct 2013  http://xu4.sourceforge.net/xu4-1.0beta3.tar.bz2
xulrunner                 41.0b9       05 Jan 2016  http://ftp.mozilla.org/pub/xulrunner/releases/41.0b9/source/xulrunner-41.0b9.source.tar.xz
xvidcap                   1.1.7        01 Oct 2013  https://sourceforge.net/projects/xvidcap/files/xvidcap/1.1.7/xvidcap-1.1.7.tar.gz
xvidcore                  1.3.7        01 Jan 2020  http://downloads.xvid.com/downloads/xvidcore-1.3.7.tar.gz
xvidtune                  1.0.4        06 Feb 2023  https://www.x.org/releases/individual/app/xvidtune-1.0.4.tar.xz
xvinfo                    1.1.5        04 Dec 2022  https://xorg.freedesktop.org/releases/individual/app/xvinfo-1.1.5.tar.xz
xvoice                    0.9.6        01 Apr 2014  http://prdownloads.sourceforge.net/xvoice/xvoice-0.9.6.tar.gz
xwallpaper                0.6.2        09 Jul 2019  https://github.com/stoeckmann/xwallpaper/releases/download/v0.6.2/xwallpaper-0.6.2.tar.xz
xwayland                  24.1.3       05 Oct 2024  https://www.x.org/pub/individual/xserver/xwayland-24.1.3.tar.xz
xwd                       1.0.9        05 Jun 2023  https://www.x.org/releases/individual/app/xwd-1.0.9.tar.xz
xwininfo                  1.1.6        12 Apr 2023  https://xorg.freedesktop.org/releases/individual/app/xwininfo-1.1.6.tar.gz
xwud                      1.0.7        15 Oct 2024  https://www.x.org/releases/individual/app/xwud-1.0.7.tar.xz
v                         0.8.1        09 Jun 2023  https://github.com/Cyan4973/xxHash/archive/refs/tags/v0.8.1.tar.gz
xz                        5.6.3        15 Jan 2025  https://github.com/tukaani-project/xz/releases/download/v5.6.3/xz-5.6.3.tar.gz
yabause                   0.9.14       17 Feb 2017  https://sourceforge.net/projects/yabause/files/yabause/0.9.14/yabause-0.9.14.tar.gz
yaf                       2.12.2       01 May 2014  https://tools.netsa.cert.org/releases/yaf-2.12.2.tar.gz
yafc                      1.3.7        14 Sep 2017  http://www.yafc-ftp.com/downloads/yafc-1.3.7.tar.xz
yakuake                   3.0.5        22 Sep 2018  https://download.kde.org/stable/yakuake/3.0.5/src/yakuake-3.0.5.tar.xz
yaml                      0.2.5        02 Jun 2020  https://pyyaml.org/download/libyaml/yaml-0.2.5.tar.gz
yamlnet                   parser       01 Feb 2013  https://sourceforge.net/projects/yaml-net-parser/files/yaml-net-parser/
yard                      0.9.26       26 Jul 2021  https://rubygems.org/downloads/yard-0.9.26.gem
yasm                      1.3.0        06 Sep 2014  http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
yaz                       5.27.1       12 Feb 2019  http://ftp.indexdata.dk/pub/yaz/yaz-5.27.1.tar.gz
yelp                      42.2         29 Dec 2022  https://download.gnome.org/sources/yelp/42/yelp-42.2.tar.xz
yelptools                 42.1         31 Oct 2022  https://download.gnome.org/sources/yelp-tools/42/yelp-tools-42.1.tar.xz
yelpxsl                   42.1         23 Feb 2024  https://download.gnome.org/sources/yelp-xsl/42/yelp-xsl-42.1.tar.xz
youtubedl                 2021.12.17   22 Dec 2021  https://github.com/ytdl-org/youtube-dl/releases/download/2021.12.17/youtube-dl-2021.12.17.tar.gz
ypserv                    2.19         01 May 2014  ftp://ftp.kernel.org/pub/linux/utils/net/NIS/ypserv-2.19.tar.bz2
yptools                   4.2.2        06 Dec 2017  http://www.linux-nis.org/download/yp-tools/yp-tools-4.2.2.tar.bz2
yum                       3.4.3        15 Nov 2019  http://yum.baseurl.org/download/3.4/yum-3.4.3.tar.gz
z3                        4.8.8        19 Jun 2020  https://github.com/Z3Prover/z3/archive/z3-4.8.8.tar.gz
zabbix                    4.4.7        06 Mar 2021  https://sourceforge.net/projects/zabbix/files/ZABBIX%20Latest%20Stable/4.4.7/zabbix-4.4.7.tar.gz
zanshin                   23.08.5      21 Feb 2024  https://download.kde.org/stable/release-service/23.08.5/src/zanshin-23.08.5.tar.xz
zatacka                   0.1.8        01 May 2014  https://sourceforge.net/projects/zatacka/files/zatacka/0.1.8/zatacka-0.1.8_src.tar.gz
zathura                   0.4.7        18 Apr 2021  https://pwmt.org/projects/zathura/download/zathura-0.4.7.tar.xz
zee                       0.6          01 May 2014  https://sourceforge.net/projects/zee/files/zee/0.6/zee-0.6.tar.gz
zenity                    4.0.2        07 Jul 2024  https://download.gnome.org/sources/zenity/4.0/zenity-4.0.2.tar.xz
zeroconfioslave           22.04.3      07 Jul 2022  https://download.kde.org/stable/release-service/22.04.3/src/zeroconf-ioslave-22.04.3.tar.xz
zeromq                    4.3.4        02 Feb 2021  https://github.com/zeromq/libzmq/releases/download/v4.3.4/zeromq-4.3.4.tar.gz
zfs                       2.3.0        14 Jan 2025  https://github.com/openzfs/zfs/releases/download/zfs-2.3.0/zfs-2.3.0.tar.gz
zile                      2.6.2        06 May 2021  https://ftp.gnu.org/pub/gnu/zile/zile-2.6.2.tar.gz
zim                       0.70         13 Apr 2019  https://zim-wiki.org/downloads/zim-0.70.tar.gz
release                   3.0.5        21 Jan 2025  https://github.com/sekrit-twc/zimg/archive/refs/tags/release-3.0.5.tar.gz
zip                       3.0          15 Feb 2023  https://downloads.sourceforge.net/infozip/zip-3.0.tar.gz
zipruby                   0.3.6        03 Dec 2017  https://rubygems.org/downloads/zipruby-0.3.6.gem
zix                       07.04.2024   07 Apr 2024  https://download.kde.org/stable/zix-07.04.2024.tar.xz
zlib                      1.3.1        23 Jan 2024  https://www.zlib.net/zlib-1.3.1.tar.xz
zope                      4.4.2        22 May 2020  https://files.pythonhosted.org/packages/04/e6/288b653d2e42dd5608093a14b954164d1d85f742577330e973c756ec4864/Zope-4.4.2.tar.gz
zopeinterface             6.1          28 Oct 2023  https://files.pythonhosted.org/packages/87/03/6b85c1df2dca1b9acca38b423d1e226d8ffdf30ebd78bcb398c511de8b54/zope.interface-6.1.tar.gz
zsh                       5.9          07 Apr 2024  https://www.zsh.org/pub/zsh-5.9.tar.xz
zstd                      1.5.6        08 Sep 2024  https://github.com/facebook/zstd/releases/download/v1.5.6/zstd-1.5.6.tar.gz
zsync                     0.6.2        26 Jan 2019  http://zsync.moria.org.uk/download/zsync-0.6.2.tar.bz2
zutils                    1.10         06 Mar 2021  http://download.savannah.gnu.org/releases/zutils/zutils-1.10.tar.lz
zvbi                      0.2.35       28 Sep 2015  https://sourceforge.net/projects/zapping/files/zvbi/0.2.35/zvbi-0.2.35.tar.bz2
v                         2.3.0        10 Feb 2023  https://github.com/zxing-cpp/zxing-cpp/archive/refs/tags/v2.3.0.tar.gz
zziplib                   0.13.68      18 Mar 2024  https://sourceforge.net/projects/zziplib/files/zziplib13/0.13.68/zziplib-0.13.68.tar.bz2
BLFS - Beyond Linux From Scratch (and also LFS)
class SoftwareManager::Actions::Cookbooks::Blfs has "support" for BLFS in that it will ... simply show a URL to the remote BLFS page of a given package, if it exists, on the commandline.

If you only need to know the URL to the BLFS homepage, to embed it for display into a webpage perhaps or to merely display it onto the commandline, you can use the following simple toplevel-API for this:

SoftwareManager.blfs(:name_of_the_program_goes_in_here)
SoftwareManager.blfs(:openssl) # => "https://www.linuxfromscratch.org/blfs/view/8.1/postlfs/openssl.html"

# You can also use a String as input to the method rather than a Symbol:
SoftwareManager.blfs('ruby') # => 'https://www.linuxfromscratch.org/blfs/view/svn/general/ruby.html'

This will return the remote BLFS page, as URL (a String, actually), if it exists, and if it has been registered in the corresponding .yml file (cookbook file for the program at hand; in this example the file openssl.yml). If no BLFS entry has been registered then nil will be returned by this method.

You can also use class SoftwareManager::LFS to build/create a LFS from scratch. This is presently (as of the year 2019) quite incomplete, though, and will not work. Lots of different things have to be done in the correct sequence before a LFS can be built from scratch successfully, and possible errors have to be checked before the build can continue. In the long run, for the future, this will eventually work, one day - but for now it is an incomplete stub and not recommended for testing much at all. It is simpler to use the slow, semi-manual approach for now.

If you wish to quickly paste the BLFS page of a given program onto the commandline, for display purpose, then you can do something like this:

ry php --parseblfs

Since as of September 2021 a bin/ script exists for this functionality. This was already planned back in August 2019, but I finally had a good use case (support for my older laptop) to add it. The executable is called paste_blfs and it can be used in the following way via the commandline:

paste_blfs ruby
paste_blfs htop

Since as of December 2019, it is now also possible to display, on the commandline, any registered BLFS entry. This will help in the event that you can not access the xorg-server at the moment, yet still require information for different packages (I faced exactly this problem, so I then added that functionality to the SoftwareManager project).

In order for this to work, you evidently need to have a working internet connection available - but in principle, the information stored in the BLFS project could be zipped up and distributed too, within the SoftwareManager project.

For the time being, though, only pasting is supported - there is no plan to bundle the information into the SoftwareManager project at this point in time.

Usage example for this functionality follows:

ry --parse-blfs-page-for=llvm

In December 2019 another feature was added - the ability to make use of the BLFS instructions directly, and run them.

The idea here is that the user may wish to compile some program from source, but may not access a working xorg-server, as you are working on the commandline only.

Invocation example for this:

ry kerberos --use-blfs-instructions # Compile kerberos in the BLFS way.

Since LFS/BLFS is such a useful project, the SoftwareManager suite has some more support in this regard. This is rather unsurprising, as the Linux From Scratch (LFS) Project has instructions on how to compile programs from source. So it has a somehwat similar meta-goal, like the SoftwareManager project.

These instructions are immensely useful. A lot of the information from LFS/BLFS was integrated into the SoftwareManager project.

Many individual cookbook yaml files can point towards the LFS/BLFS page, which will often contain additional useful information in case you get stuck with something.

You can try to show this URL by doing this:

blfs curl

This would show the BLFS remote URL of the program called curl.

You can also directly paste the content of the remote BLFS webpage onto the terminal, if there is a remote BLFS page associated with a given program.

The way to do this is as follows:

ry gcc --paste-blfs-page

If you want to show all programs that have a blfs entry, use the following command:

ry --show-programs-with-a-blfs-entry

If you want to obtain an Array of all programs containing a BLFS entry, you can use the following toplevel API:

SoftwareManager.return_all_blfs_entries

This is currently rather slow; I'll look into making it faster in the future, making use of grep.

If you want to find out how many BLFS entries are registered in the RSM project, use this toplevel method:

SoftwareManager.n_BLFS_entries? # => 776 - as of March 2025

Minimal chroot environment and improved chroot environments
On my home system I tend to have a working backup system at /depot/chroot/.

In this directory I tend to have all important programs "duplicated", so that I can safely re-compile when necessary.

Which compile-chain may be used to build up a minimal chroot?

I will try to list the options here, as I use this on my system (or a new system):

ry make --static --chroot
ry bash --static --chroot # <- This may already suffice.
ry mpfr --chroot
ry gmp --chroot
ry mpc --chroot
ry sed --static --chroot
ry grep --static --chroot
ry coreutils --static --chroot
ry ruby --dontuseconfigureoptions --chroot
ry python --chroot
ry gawk --chroot --static
ry nano --static --chroot
# Generate my custom rc-files next:
rcfiles --populate-this-dir=/depot/chroot/AUTOGENERATED/
rcfiles
rcfiles --populate-this-dir=/AUTOGENERATED/
ry gcc --chroot --dont-symlink
ry ncurses --dontuseconfigureoptions --chroot
ry zlib --chroot
ry nano --chroot
ry bison --static --chroot
ry m4 --chroot
ry curl --chroot
ry isl --chroot
ry flex --chroot
ry nasm --chroot
ry gc --chroot
ry tar --chroot
ry texinfo --chroot
ry openssl --chroot
ry libelf --chroot
ry elfutils --chroot
ry ccache --chroot
ry lzma --chroot
ry cmake --chroot
ry file --chroot
ry pkgconfig --chroot
ry gperf --chroot
ry popt --chroot
ry git --chroot
ry pcre1 --chroot
ry pcre2 --chroot
ry libunistring --chroot
ry gettext --chroot
ry check --chroot
ry libxml2 --chroot
ry mtools --chroot
ry gzip --chroot
ry doxygen --chroot
ry strace --chroot
ry valgrind --chroot
ry graphite2 --chroot
ry tcl --chroot
ry expect --chroot
ry dejagnu --chroot
ry patch --chroot
ry libogg --chroot
ry sqlite --chroot
ry libpciaccess --chroot
ry libdrm --chroot
ry pixman --chroot
ry llvm --chroot
ry enchant --chroot
# Compiling glibc should come last here:
ry glibc --chroot --do-not-symlink --do-not-run-ldconfig

Do not forget to also copy a statically compiled version of busybox.

Also, if the chroot-command fails, make sure that you have copied the ld-linux shared objects, such as:

'/lib64/ld-linux.so.2' -> './ld-linux.so.2'
'/lib64/ld-linux-x86-64.so.2' -> './ld-linux-x86-64.so.2'

In March 2025 it was realised that the above steps are a bit brittle, so it made sense to think of an alternative approach: by making use of SoftwareManager to enable the individual early steps used by LFS. These early steps will properly set up a chrooted environment, which is then used to compile the rest of a Linux system. Thus, SoftwareManager will be adjusted to allow for these individual steps as well.

Auto-resolve compile-related problems via the SoftwareManager project
Since as of September 2017, the SoftwareManager scripts can also give some advice how to resolve some compile-related problems.

This is currently fairly minimal, but it will be extended subsequently in the future. It was also a reason why the namespace Libtool has been integrated into SoftwareManager, as SoftwareManager::Libtool. This allows us to auto-correct some libtool-related problems and keep all libtool-related code within that namespace.

Module programs: making use of the extended cookbook dataset
Module programs refers to the ability to access all registered programs that are reigstered in the SoftwareManager project, at the toplevel "SoftwareManager." namespace. In other words, we can "copy" the program names onto the SoftwareManager top-level module "namespace".

For instance, htop would be available at

SoftwareManager.htop

if it is called as a "module program".

Take note that this is NOT the default behaviour. By default, the above way to call the method .htop() on SoftwareManager would not work.

As another example: the program binutils would, as a module program, be available at:

SoftwareManager.binutils

And so on, and so forth. No "-" characters are allowed there, as these are not allowed in methods written in ruby.

Note that this will return a sanitized Hash that holds all the information required to compile the given program from source - this dataset thus describes these programs. That information may still have to be interpreted by a program, of course.

Simply require the necessary file for this:

require 'software_manager/module_programs'

After that, the above methods will work. Let's see a few more examples for this:

SoftwareManager.wayland
SoftwareManager.ruby
SoftwareManager.php
SoftwareManager.gnomemimedata
SoftwareManager.evince

Note that reader methods via '?' character are available too, although this could be toggled. The '?' will access the datastructure as a Hash rather than a specialized object.

Invoke it like so:

SoftwareManager.htop? # => {"archive_size"=>323560, "short_description"=>"ncurses-based interactive process viewer.", "github"=>nil, "set_env_variables"=>{"LDFLAGS"=>"-ltinfow"}, "homepage"=>"http://hisham.hm/htop/", "binaries"=>["htop"], "headers"=>[], "libraries"=>[], "postinstall"=>[], "pre_make_commands"=>[], "required_deps_on"=>["glibc => 2.3.2", "ncurses => 5.6"], "tags"=>[], "pkgconfig_files"=>[], "program_path"=>"/home/x/src/htop/htop-2.0.2.tar.xz", "program_full_name"=>"htop-2.0.2.tar.xz" ... and so forth }

This functionality exists primarily because it may be easier for you to use the above API calls. Internally though, the SoftwareManager project will prefer API calls such as SoftwareManager::Cookbooks::Cookbook.new :htop instead.

Note that the above works without any '-' as part of the name, so there will not be SoftwareManager.gnome-mime-data() - it would be SoftwareManager.gnomemimedata().

When may you want to use this? Well, it could be used if you do want to use a simpler API. It is very easy to remember, too: just use 'SoftwareManager.' followed by the name of the program, without any '-' or '_' characters there.

How to register all programs
It may be that you want to register all programs in the yaml database that is supported by the SoftwareManager project.

In order to do so, run this command:

ry --register_all_programs
ry --rap

Batch-updating all registered gems in the SoftwareManager project
You can batch-update all registered gems via

rsm --update-all-gems

Note that this will only work if the corresponding .gem is part of the Cookbooks submodule of this project.

Updating all registered rubygems
It is possible to batch-update all registered rubygems via:

ry --update-all-gems

The boolean constant ALSO_AUTOMATICALLY_INSTALL_THE_UPDATED_GEM, defined in class SoftwareManager::Action::SoftwareManager, controls whether the updated gems will automatically be installed by the SoftwareManager project. If set to true then the SoftwareManager project will not only download the respective gem, but also install it. Since not everyone may want this, the constant handles this case for the time being - I personally like this constant set to true, since it saves me some keystrokes in the long run.

If you do not want to, or can not, update all registered .gem entries, yet you still want to install the .gem files that are registered, then you can use this commandline invocation variant:

ry --install-all-rubygems

This will install all registered .gem files, in an alphabetical manner. Dependencies will not be checked that way, but in principle if all .gem have been properly registered in the Cookbooks submodule, then this batch-gem installation should work just fine.

Modifying the cookbook-dataset via a webpage
In the past, already since as of 13.09.2007, it was possible to modify the cookbook-dataset via webpages - usually via a .cgi webpage.

As of March 2025 this is no longer possible, but it may be brought back one day. Stay tuned in this regard.

Patches to the cookbook-dataset and applying those patches automatically
Some programs sometimes need a patch, in order for successful compilation.

The entry that is to be used here, in a corresponding .yml file, used to be apply_patch:. Since as of 2025, this is now simply called patch:.

It is to be used like this:

patch:
- https://connie.slackware.com/~alien/slackbuilds/id3lib/build/id3lib-3.8.3_gcc4.diff

This feature is not that important, but sometimes a program may need a patch, so the feature had to be added, in order to allow users to automatically download and apply such patches, before compilation begins in earnest.

The functionality of class ProgramInformation
ProgramInformation is a fairly small class. The motto behind it is to do one thing well.


The two methods .first and .last, part of class ProgramInformation
It is possible to obtain the name of a given program at hand, and the associated version, like so:

require 'software_manager/requires/program_information.rb'

x = SoftwareManager::Actions::ProgramInformation.new('ruby-2.5.0')
puts x.first # => ruby
puts x.last

Note that other toplevel variants exist as well, such as:

SoftwareManager.return_name()
SoftwareManager.return_version()

The idea here is to be as flexible as possible, so that the user cherry-pick whatever is preferred. Personally I usually use the toplevel method variants, as these are shorter to type.

Tests for SoftwareManager::Actions::ProgramInformation
class ProgramInformation is very important to automatically determine the program-name and the program-version of a given String.

As this class is very important, it also comes with a test suite. These tests are stored mostly in the file called software_manager/test/testing_program_information.rb, which will test the Array called SoftwareManager::ProgramInformation::ARRAY_TEST_THIS_AS_INPUT.

Not all tests may work perfectly well, but for the most likely input it should work fine. So like 98% of all use cases that a user or developer may ever encounter. It should do well.

module SoftwareManager::Actions::Validations
module SoftwareManager::Actions::Validations has been added in January 2025. The main objective of this module is to gather all validations in one place. Validations in this regard means quality-control code - in other words, code that can be used to improve the quality of the whole codebase in the SoftwareManager suite.

Handling errors in the SoftwareManager project
Some problems and errors may happen during compilation or installation of programs.

Before this statement is further explained, let's briefly review how to disable checking for errors in the SoftwareManager project automatically. You can use the following commandline flag for disabling error checking:

ry --no-error-checking

This allows the user to bypass error-checking for the current compilation run. Let's have a look at the feature to autocorrect some errors.

In the past, the SoftwareManager suite handled errors mostly via the toplevel area, that is, into the namespace module SoftwareManager, as well as an ad-hoc registry of common errors and problems. These were handled via one, or several, toplevel instance variables.

This approach was changed in 2025. Specifically we now first register all known errors and problems; this step is not yet reasonably complete. Once it has progressed more, the SoftwareManager suite will attempt to handle some of these errors automatically - or at the least give the user an option to decide on his or her own how to handle these errors and problems.

In the past, we registered the problems into the toplevel, and then allowed a toplevel-API such as:

SoftwareManager.error_message?

to show these.

Note that this used to be reset to nil, its default value, whenever a new program is compiled.

In April 2024 this was changed. First, we will finish by detecting all errors and problems. Once that is done, we will add code that will try to fix such problems.

Sometimes it may not be wanted to stop on errors, in particular if the error reported is not a real error. Thus, the user needs to have a way to overrule this behaviour, which can be done by the following commandline flag:

ry --dont-stop-on-errors
ry --ignore-errors

The latter commandline flag still works in 2025 by the way; I sometimes have to use it to bypass wrongfully reported errors.

This would instruct SoftwareManager::Action::SoftwareManager to not stop on errors and instead (try to) continue.


Autocorrecting some errors in regards to the SoftwareManager project
The various SoftwareManager scripts can attempt to auto-correct some errors. This is currently a feature that is to be tested more thoroughly; it may not work flawlessly yet.

The errors that the SoftwareManager project can auto-correct are mostly related to libtool right now.

If the file use_autofixing.yml contains a String called t or true, meaning "enable autofixing", then auto-correcting will be used.

There is another option that has to be set to true though, which is the "is_on_roebe?" settings, which indicates my home directory use.

Presently (Sep 2017) this option does not work too reliably, so it is still tested. You can test it too though if you enable "is_on_roebe".

In 2025 I decided to adjust the above a little; auto-fixing issues will still be a desired feature, but first we really need to document all existing errors and problems, and handle these. Only then should we begin to extend the auto-fixing part of the SoftwareManager project.

Querying the prefix to be used
If you have the need to find out the prefix that will be used, you can issue the following query:

ry htop prefix?
ry htop --prefix?

class SoftwareManager::Actions::Gitty
class SoftwareManager::Gitty is a helper class. It ultimately wraps over the "--gitty" commandline instruction class SoftwareManager::Action::SoftwareManager uses.

The SoftwareManager::Actions::Gitty class can be used to quickly checkout the git sources of a remote project.

So, for instance, if you want to check out the latest source code of mesa, you could do this via the commandline:

gitty mesa

This will download mesa and repackage it to today's timestamp, in dd.mm.yyyy notation.

You can download any program that is registered in the SoftwareManager namespace, if it has a corresponding entry called git_url in the .yml file. This is how I maintain this list: I simply change the associated git URL, such as for github or gitlab, in the corresponding .yml file.

Since as of March 2024 gitty was slightly extended, in that it can now work on any arbitrary remote URL that you pass into this class.

For instance:

gitty https://github.com/kennylevinsen/seatd

This would download the current git sources from that remote URL, and repackage it automatically for you, using a dd.mm.yyyy notation. This functionality was added because I sometimes found new programs and just wanted to quickly test whether I can compile it, so I can now just copy/paste this URL onto the commandline, as input for gitty.

SoftwareManager::Action::SoftwareManager
class SoftwareManager::Action::SoftwareManager was added in March 2024. It has replaced the older class called Installer.

The key idea behind SoftwareManager, aside from fixing a few smaller bugs and issues, will be to transition into an action-based system.

This means that everything that is important, to be done by class SoftwareManager, should be handled through the method called SoftwareManager.action() primarily.

The advantage of this approach is that we can easily change aspects of class SoftwareManager, without having to modify the class directly.

This replacement effort is complete as of 2025.

class SoftwareManager::Action::SoftwareManager: feedback the URL of various programs at the same time
Say that you wish to find out the URL to various programs at the same time.

You can use this the following commandline invocation for this:

ry --url-for-these-programs=php,ruby,python,m4,php

This will show a result such as:

Entries that are included more than once - such as php in the above made-up example - will only be shown once.

class SoftwareManager::Action::SoftwareManager - the commandline option --compile-the-first-entry-in-the-current-working-directory
On my computer system I have done the following alias:

this → rbt --compile-the-first-entry-in-the-current-working-directory

So, I can then navigate to a directory containing a source archive that can be compiled. Then I type "this" and hit enter. SoftwareManager::Action::SoftwareManager will proceed to compile the first entry of this directory.

This allows the user to be lazy and efficient. The above instruction will simply compile the very first entry in any given directory, without having to type the name or even write the beginning of the name and use '*' as substitution character, such as via:

ry php*.xz
ry tar*.xz

I recommend that users of the SoftwareManager suite also use an alias such as "this", but of course this is up to that user, not to me. For me, personally, just typing "this" is very convenient should I need it.

Internals of class SoftwareManager::Action::SoftwareManager
class SoftwareManager::Action::SoftwareManager is the main class of the SoftwareManager-project, as it allows the user to compile or install a program from source.

It is, more or less, the central entry point to the SoftwareManager project - even though you can invoke the other .rb files directly, SoftwareManager::Action::SoftwareManager often provides a simpler convenience access means to the disparate functions that constitute the SoftwareManager-project. This class integrates other classes, sort of like a glue.

This subsection here will explain a few key decisions made, in the long run; and will also explain a few internals used by this class.

Do not worry if this may be a bit confusing at first - it is meant primarily for me, and for others who may want to extend the SoftwareManager-project one day.

The method set_configure_base_directory(), is used to determine the directory where the configure script ought to reside. That way we can enter a separate build directory yet still be able to invoke configure (or cmake) at the right directory target.

Some commandline options for class SoftwareManager::Action::SoftwareManager explained
As was already written down somewhere else in this document, you can pass in several commandline options to class SoftwareManager::Action::SoftwareManager.

You can also use commandline options that include -- and these options can overrule other configure options, on an ad-hoc basis.

For instance, let's assume that the configure options in a given yaml file, such as weechat.yml, include an option such as --enable-gtk. But you don't want any GTK GUI for Weechat. So either you modify the .yml file, or you decide to override this flag, on the commandline, by doing:

ry weechat --disable-gtk

This will now overrule the setting in the weechat.yml file for the given compilation run, and compile weechat with gtk disabled. That way you can leave weechat.yml unmodified, yet you are still able to get the desired change. (For a permanent change you would have to modify the .yml file of course.)

If you want to get a quick overview, issue:

ry --overview

This should give a brief overview over the available utility scripts.

Sometimes you may not want to pick up the configure options from a specific .yml file. In this event, you can pass in the option --dont-use-configure-options to avoid doing so.

Usage examples for this approach:

ry htop --dont-use-configure-options
ry glib --user-home-prefix --use-meson --no-configure-options
ry ruby --do-not-use-any-configure-options

This allows the user to just use the "barebones" compilation mode; it may be helpful for users who encounter errors due to the use of some configure options or some combination of configure options used.

Working with meson
You can specifically enable the meson-build by issuing this:

ry glib --use-meson

Check for software that is locally available
You can check for the latest packages locally via

ry htop latest

configure_command_to_use
This short subsection is to briefly explain the option called configure_command_to_use that is available for individual cookbook .yml files.

By default, this command can be omitted, and will then default to ./configure - aka GNU configure. This should cover most programs.

There are some programs that do not use GNU configure, though, in particular cmake-based projects, scons, waf, meson/ninja and so forth. These are handled separately for the most part.

There are, however had, also programs that use their own custom and unique build system, often defaulting to some shell script. The SoftwareManager-project thus has to be flexible here and allow for custom shell scripts to be used as well.

An example for this can be seen in the file discount.yml for the program discount. If you look at that file you can see the following line:

configure_command_to_use: ./configure.sh

It thus specifies that SoftwareManager::Action::SoftwareManager will make use of that configure.sh shell scripts rather than the more generic configure invocation run.

Some other programs use other configure_command_to_use settings; a recursive grep can find these, should a user be interested in that.

class SoftwareManager::Actions::SimplifyRootEntriesInThisMakefile
This very specialized class will replace all root:root entries in a file, such as a Makefile, with 0:0 entries.

The reason why this small class was added was primarily because sometimes the host system may be partially broken, and only display 0:0 rather than root. This may happen more frequently in a chrooted environment.

Oddly enough, this may then lead to errors during compilation or installation, which was the primary reason why this class was created: I wanted to ignore these issues.

Since I was tired of such Makefiles failing on me, this class was written in August of 2019. It is not a very important or commonly used class, though.

