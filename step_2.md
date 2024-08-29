# Step 2 - Git & GitHub

## Create a GitHub account

* Even if you don't want to open source your code, or even share your plugin with anyone, it is
  still a good idea to manage your code in a Git repository. And even if you want to keep your
  source code local and private, a GitHub account lets you use GitHub Desktop and can be used as a
  login for various other online tools, or to interact with other VCV Rack Developers and their
  plugins, which can be a valuable support resource.
* Create yourself a [GitHub account](https://github.com/signup) and setup your profile how you like.

## Install GitHub Desktop

[GitHub Desktop](https://github.com/apps/desktop) makes using Git nice and simple, you don't need to
learn any command line stuff, your basic use-case will be: `edit code > commit in GitHub Desktop`.
You will also be able to clone existing plugin repositories with it should you want to.

* Use Scoop:
  * `scoop bucket add extras`
  * `scoop install extras/github`
* Run the app and login to GitHub.

## Test the plugin build process

The [VCV Fundamental](https://github.com/VCVRack/Fundamental) modules are a plugin that is open
source in a GitHub repository. This means we can clone that repo and use it to test our VCV Rack
plugin build environment.

* First, create a new folder `rack-dev` in your MSys2 home directory to house any Git repos.
* In GitHub Desktop, `File > Clone repository...`
  * Click on the `URL` tab, paste the VCV Fundamental GitHub repo URL of
    `https://github.com/VCVRack/Fundamental` in the first box and select the `rack-dev` folder in
    the second box.
  * When you click the `Clone` button it will download the repo, this will also appear in the
    workspace so you can have a browse of these files in VSCode.

* In `MSys2.mingw64`, go to the new repo:
  * `cd ~/rack-dev/Fundamental`
  * The `makefile` defines a number of tasks for building the plugin, you can use tab completion to
    see all the tasks available
    * type `make [tab][tab]`
  * To build the plugin run `make dist`
    * If the build environment is all setup correctly, when the build completes you should get a
      `dist` folder that contains a `Fundamental-2.6.0-win-x64.vcvplugin` file.
      * If you copy-paste this file into your VCV Rack `plugins-win-x64` folder, this will install
        the plugin when you next run the VCV Rack application, however this will be no different to
        the included version. The `make install` task will build the plugin and copy it to the right
        folder for you.

* You might want to clone other open source plugin repos, to look at their code, maybe make some
  tweaks and see the results.
  * Any plugin that is listed as
    [`Open Source` in the VCV Rack library](https://library.vcvrack.com/plugins?query=&sort=buildTimestamp&license=open)
    should have a `</> Source code` link to the GitHub repo, simply copy this URL and clone the repo
    as you did for Fundamental.

## Create your own private repo

* When you initially create a new plugin, it makes sense to do it in a private repository, you can
  easily make it public later if you want to.
* You have two options:
  * Create a local repo using GitHub Desktop (and push it to github.com if you want to)
  * Or what I find easier is to create a new private repo on the site and then clone it locally
    * Using this method also means GitHub can automatically create a `README.md`, `LICENSE.md` and
      `C++ .gitignore` file for you.
    * You should edit your `.gitignore` file to add:
      ```
        # Rack
        /build
        /dist
        /plugin.so
        /plugin.dylib
        /plugin.dll
        .DS_Store
      ```

---

## [<< Back to Step 1](step_1.md) - [On to Step 3 >>](step_3.md)
