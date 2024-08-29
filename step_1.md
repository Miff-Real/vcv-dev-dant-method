# Step 1 - Install & Configure

## Install VCV Rack

* Can be **Rack 2 Free** or **Rack 2 Pro**, but Pro is required to test your plugin in DAWs, the
  [download page](https://vcvrack.com/Rack#get) has the latest version. When there is an update,
  simply download the new version and [install](https://vcvrack.com/manual/Installing) on top of the
  previous version.
* You will need to [create a VCV Rack account](https://vcvrack.com/login) and login to use Pro or to
  subscribe to plugins (including your own if you choose to publish it in the official library).
* Run the VCV Rack app and check that it works correctly, remember a patch needs to have an Audio
  module with a selected audio device, many modules and/or controls will not function when there is
  `No device`.

## Install Windows Terminal

* I use [Windows Terminal](https://aka.ms/terminal) to interact with
  [PowerShell](https://en.wikipedia.org/wiki/PowerShell) (for Scoop) and MSys2.
* Once installed, the only config you need to do for now is to set the `Appearance` and
  `Color Schemes` settings, I use the `Light` Application theme and the `One Half Dark` Color
  scheme. You can set the font face and size in the
  `Profiles > Defaults > Additional settings > Appearance` section.

## Install Scoop

* [Scoop](https://scoop.sh/) is a package and application manager for Windows. I prefer to use Scoop
  for anything that needs to be installed on Windows if it is available, because it makes
  installing, updating and uninstalling easy.
* Just using the default install method will be fine, so all you need to do is open Windows
  Terminal, check you are in a PowerShell session (it should be the default). Then type in the
  commands as per the website Quickstart.
* You should check for updates regularly:
  * first run `scoop update`
  * then to see what updates are available run `scoop status`
  * to apply an update run `scoop update <Name>`
* You can see what you have installed using `scoop list`

## Install Sublime Text

* Some of our configuration is going to need a text editor, when I don't need to use a full-featured
  IDE, I like [Sublime Text](https://www.sublimetext.com/) because it is simple, configurable and
  fast.
* Sublime Text is in the Scoop `extras` bucket, so to install it:
  * first run `scoop bucket add extras`
  * then install using `scoop install extras/sublime-text`
* Configuration and licensing of Sublime Text is beyond the scope of this guide :-P you could just
  use `Notepad` instead if you really want to.

## Install MSys2

* [MSys2](https://www.msys2.org/) is the command-line environment that we use to build our VCV Rack
  plugin, specifically we use the `MSys2.mingw64` environment because it provides an `x86_64`
  architecture with the `gcc` toolchain, `msvcrt` C library and `libstdc++` C++ library.
* MSys2 is in the Scoop `main` bucket, so you can simply run `scoop install msys2` to get it. But be
  sure to follow any `Notes` that are presented in the output.
* Now we can create a Windows Terminal profile for `MSys2.mingw64`:
  * Find the location where Scoop installed MSys2 and look for `msys2_shell.cmd`, right-click that
    file and select `Copy as path`
  * Under `Settings`, click `Add a new profile`
  * Paste the copied path into `Command line` and then add
    ` -defterm -here -no-start -mingw64 -shell bash`
    * My full Command line is
      `C:\Users\dant\scoop\apps\msys2\2024-05-07\msys2_shell.cmd -defterm -here -no-start -mingw64 -shell bash`
  * Find the location of the msys2 filesystem, and then location your home directory within it,
    right-click the directory and select `Copy as path`
    * It is also useful to right-click your home directory and select `Pin to Quick access` so that
      you have a direct link to it in file explorer.
  * Paste the copied path into `Starting directory`
    * My full Starting directory is
      `C:\Users\dant\scoop\persist\msys2\home\dant`
  * Once you save the profile, you should be able to open up a new tab in Windows Terminal for
    `MSys2.mingw64`, you can verify this: run `uname` to get something like `MINGW64_NT-10.0-22631`

### Configure `MSys2.mingw64`

* In your `MSys2.mingw64` home directory you will find a `.bashrc` file, you can open this file in
  Sublime Text to edit it. Add any aliases or other config you might want and save it. When you make
  a change to your `.bashrc` file close your current terminal tab and open a new one for it to take
  effect.
  * What sort of things to put in your `.bashrc` is a large topic, Google it...
    * Two specific aliases I use heavily are `alias ..="cd .."` & `alias l="ls -lF"`

* Now we need to install all the Rack dependencies using [`pacman`](https://devhints.io/pacman)
  * First update `pacman -Syu`
  * Check which packages are installed `pacman -Qqe`
  * If you need to free some space `pacman -Scc`
    * Install or update everything we need to build Rack:
      * This [list in the VCV Rack docs](https://vcvrack.com/manual/Building#Windows)
      * `pacman -Syu git wget make tar unzip zip mingw-w64-x86_64-gcc mingw-w64-x86_64-gdb mingw-w64-x86_64-cmake autoconf automake libtool mingw-w64-x86_64-jq python zstd mingw-w64-x86_64-pkgconf`

### Updating MSys2

* Every time you install or update MSys2 you will also need to:
  * Update the `Command line` path in the Windows Terminal profile
  * Re-install all the Rack dependencies as above
  * Re-install Starship prompt, see below

## Install a NerdFont

* [NerdFonts](https://www.nerdfonts.com/) are specific fonts that are commonly used in text editors
  and IDEs that have been patched to include many icons that are good for enhancing your terminal.
  * I use the font `RobotoMono` in my text editor and IDEs.
  * You can simply download the NerdFont zip you want to use, unzip and double click it to install
    in Windows.
  * Or you can install it with Scoop:
    * `scoop bucket add nerd-fonts`
    * `scoop install nerd-fonts/RobotoMono-NF`
  * Once installed, remember to set the font face in both your text editors, IDEs and Windows
    Terminal.

## Install Starship prompt

* [Starship](https://starship.rs/) is a drop-in replacement for the shell prompt with loads of
  helpful features.
  * In `MSys2.mingw64` first run `mkdir /usr/local/bin` to ensure the directory exists
  * Then run `curl -sS https://starship.rs/install.sh | sh`
  * Edit your `.bashrc` with Sublime Text to add the line `eval "$(starship init bash)"` and restart
    the terminal.

## Install Visual Studio Code

* [Visual Studio Code](https://code.visualstudio.com/) is my preferred IDE for Rack development, it
  is quite powerful and highly customizable, so it can get quite complex. But for rack dev you only
  need a few extensions to create a nice C++ editing environment.
  * Install it using Scoop:
    * `scoop bucket add extras`
    * `scoop install extras/vscode`

### Configure VSCode

* There are a lot of ways to customize VSCode, for the purposes of rack dev the main things you need
  to do are setup a workspace and install some extensions, the rest you can Google when you want to
  take it further.
* Extensions to install:
  * [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
  * [C/C++ Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools-extension-pack)
  * [C/C++ Themes](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools-themes)
  * [Makefile Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.makefile-tools)
  * [Better C++ Syntax](https://marketplace.visualstudio.com/items?itemName=jeff-hykin.better-cpp-syntax)
* Creating a [Workspace](https://code.visualstudio.com/docs/editor/workspaces):
  * `File > Open Folder...` find and select your home directory in the MSys2 file tree.
  * You should now have a `dant.code-workspace` file in your home directory.

## Get the Rack SDK

* The final thing we need to do before moving on is get the Rack SDK,
  [download the latest Windows x64 SDK from the Rack docs](https://vcvrack.com/manual/Building#Building-Rack-plugins).
  * Move the SDK zip file to your MSys2 home directory and unzip, if you don't want to unzip from
    the command line, use Scoop to install 7Zip and do it in Windows file explorer.
  * Now you should have a `Rack-SDK` folder inside your MSys2 home directory, this folder should
    also have appeared in your VSCode workspace so you can have a browse of these files in VSCode.
  * The documentation for the SDK can be found at https://vcvrack.com/docs-v2/annotated
