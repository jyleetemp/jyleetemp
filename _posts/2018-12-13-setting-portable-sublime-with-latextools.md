---
title: "Setting up portable sublime text 3 with LaTexTools for Windows"
date: 2018-12-13 22:42:11 +0900
header:
  teaser: /assets/images/sublime-latextools/sublime-latextools.png
tags: [Windows10, Sublime Text 3, latex, LaTexTools]
toc: true
categories: [Windows10, Sublime Text 3]
---
![sublime-latextools](https://codeslake.github.io/assets/images/sublime-latextools/sublime-latextools.png)

For those of who use LaTeX for writing a paper with LaTexTools, which is a plugin for sublime text (2 or 3),
this article will be useful for making them portable.

# Prerequisites
**Note:** Make sure to download the portable version.
{: .notice--info}

### Sublime Text 3 [(download)](https://www.sublimetext.com/3)
Make sure to install [Package Control](https://packagecontrol.io/installation), after you install sublime text 3.  

### ImageMagick [(download)](https://www.imagemagick.org/script/download.php)
The tool that allows us to see previews for equations or figures.  

### MiKTex [(download)](https://www.imagemagick.org/script/download.php)
The actual back-end tool for building LaTeX files. We can also download plugins for Latex using MikTex.

### SumatraPDF [(download)](https://www.sumatrapdfreader.org/download-free-pdf-viewer.html)
The viewer. SumartraPDF is very light, and it allows us to search selected lines by backward and forward search between the sublime and itself.  

# 1. Installation
![folder-structure](https://codeslake.github.io/assets/images/sublime-latextools/folder-structure.png)

Install the downloaded programs. Each program should have their own folder wrapping them.  
The figure above shows my folder structure.

# 2. Setting up Environment Variables
Now, we have to set environment variables for Windows 10.

For the easier setting, here I share the batch files for both addition and restoration the variables.
### Adding the variables
Create a batch file, which I named it as `addToPath.bat`.
Don't forget to filling it with the right paths for previously installed programs.

```shell
del restorePath.txt
echo %path% >> restorePath.txt
setx path "%path%;%~dp0SumatraPDF;%~dp0Sublime Text 3;%~dp0MiKTeX 2.9\texmfs\install\miktex\bin;%~dp0ImageMagick"
setx sublime "%~dp0Sublime Text 3"

PAUSE
```
If you run the file by double clicking it, it will automatically add the paths to the environment variable, `path`, for Windows 10.

### Restoring the variables
Don't worry about restoring to the original condition.
With a script below, we can alway go back to the original condition.
Create a batch file, which I named it as `restorePath.bat`.

```shell
set /P restore=<./restorePath.txt
setx path "%restore%"

PAUSE
```
If you run the file, it will automatically restore the original path. 

# 3. Checking the system
To check all things are in the right place, we need to first run Sublime Text 3.
Then, type **ctrl+shit+p** to open command pallete.
In the command pallete, type and run **LaTexTools: Check system**.

* If everything is appeared to be OK, we are good to go.
* If there is a problem on the **Variable** or **Program section**, your path in `addToPath.bat` has problem.
  * Run `restorePath.bat`, correct the paths in `addToPath.bat` and run it again.
* If there is a problem on **Packages for equation preview**, you need to manually install them, by running 'MiKTex'.
  * First, open MiKTex console and update the plugins.
  * Second, in **Packages** tab, search packages that are required and install it.
    * If it fails to install, change the repository. I used `mirrors.tuna.tsinghua.edu.cn`

# 4. Backward searching function for SumatraPDF
Before setting up the backward searching function for SumatraPDF, a LaTeX file is needed to be built at least once.
After the compilation, in SumatraPDF, navigate to **Settings | Options**,
and type `"[Path To]\sublime_text.exe" "%f:%l"` in the text-entry field for **inverse-search command line**

**Note:** For me, it is `"E:\utils\Portable Sublime with LaTeXTools\Sublime Text 3\sublime_text.exe" "%f:%l"`
{: .notice--info}

or, you can also open `cmd.exe` and run `setx path "%path%;[Path To]\SumatraPDF"` 

# References
[LaTexTools](https://latextools.readthedocs.io/en/latest/)<br/>

---
