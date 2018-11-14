# linter-gcc2

[![Build Status](https://travis-ci.com/tvincent056/linter-gcc2.svg?branch=master)](https://travis-ci.com/tvincent056/linter-gcc2) [![apm](https://img.shields.io/apm/dm/linter-gcc2.svg?style=flat-square)](https://atom.io/packages/linter-gcc2)

This is a fork of [linter-gcc](https://github.com/hebaishi/linter-gcc), which seems to have been abandoned.

This plugin provides an interface to gcc/g++ for linting and works with either [Linter](https://github.com/AtomLinter/Linter) or [atom-ide-ui](https://github.com/facebook-atom/atom-ide-ui) (through [atom-ide-diagnostics](https://github.com/facebook-atom/atom-ide-ui/tree/master/modules/atom-ide-ui/pkg/atom-ide-diagnostics)). You will need to manually install one or the other before it will function properly.

Used with files with grammar "C", "C++" and ["C++14"](https://atom.io/packages/language-cpp14).

Includes linting **on-the-fly**, though it is only partially tested. Please open an issue if you encounter any problems.

_For instructions on using this with Windows Subsystem for Linux (WSL), checkout the [Wiki](https://github.com/tvincent056/linter-gcc2/wiki)._

## Important info for Mac OSX users!
If you have XCode installed on OSX, the `gcc/g++` commands will both link to `clang`. This can cause issues with the `-fmax-errors` option used by linter-gcc2, which isn't recognised by clang. To properly install GCC, you need to install it with Homebrew (instructions [here](https://github.com/hebaishi/linter-gcc/issues/62)).

## Linter in action!

![linter-gcc screenshot](https://raw.githubusercontent.com/hebaishi/images/master/lintergcc_onthefly.gif)

## Using CMake compile settings
linter-gcc2 can take compile settings from CMake. For example:

```bash
git clone https://github.com/hebaishi/gtf2tab
cd gtf2tab
mkdir build
cd build
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ..
```

Running ```cmake``` with the ```-DCMAKE_EXPORT_COMPILE_COMMANDS``` flag generates a ```compile_commands.json``` file which linter-gcc2 can get the compile settings from. Then you simply open the project in Atom, and enter ```./build/compile_commands.json``` in the Compile Commands File setting of linter-gcc2. Note that if you supply a valid ```compile_commands.json``` file, your include paths and compile flags configuration settings (described below) are ignored.

Unfortunately, CMake does not typically compile header files so the ```compile_commands.json``` file does not include entries for these files. To add them you can use a tool like [compdb](https://github.com/Sarcasm/compdb).

```bash
compdb -p ./ list > compile_commands.json
```

## File/Project-Specific settings

Assuming you have the a file called ```sample.cpp``` open, linter-gcc2 performs the following actions:

1. Looks for file called ```sample.cpp.gcc-flags.json``` in the same directory as your source file (file-specific settings)
2. Looks for a file called ```.gcc-flags.json``` in every subdirectory from the current up to your project root (subdirectory/project-specific settings)
3. If no ```.gcc-flags.json``` is found, the settings in your configuration page are used.

The package takes its settings from the first configuration file that is found.

You can specify your settings in ```.gcc-flags.json```, at any level (file/subdirectory/project) using the following syntax:

```json
{
  "execPath": "/usr/bin/g++",
  "gccDefaultCFlags": "-Wall",
  "gccDefaultCppFlags": "-Wall -std=c++11",
  "gccErrorLimit": 15,
  "gccIncludePaths": ".,./include,./path",
  "gccSuppressWarnings": true
}
```

Note that the include paths need to be separated by commas. If this file is present, it will replace the settings you specified in the settings window. Relative paths (starting with ```.``` or ```..```) are expanded with respect to the root folder. Both ```execPath``` and ```gccIncludePaths``` are expanded.

In order to avoid unwanted behavior associated with having multiple projects open, only the paths within the first project are used, and the package limits its search to 30 levels when looking for a configuration file. You can work with multiple projects, as long as each is open in a separate window. Additionally, within each project, you may have as many file/directory-specific configuration files as you wish.

### Usage notes:
* Add ```-fsyntax-only``` to your C/C++ compilation flags to prevent the generation of ```.gch``` files when linting headers
* Add ```-c``` to your flags to avoid linking errors.

### Plugin installation
Press ctrl and ',' or cmd and ',' , click on 'Packages', search 'linter gcc', or:
```
$ apm install linter-gcc2
```
### Reporting Issues

Please read the [Wiki](https://github.com/tvincent056/linter-gcc2/wiki) before reporting any issues.
