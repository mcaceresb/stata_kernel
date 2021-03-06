# stata_kernel

`stata_kernel` is a Jupyter kernel for Stata; It works on Windows, macOS, and
Linux.

- [Comparison with ipystata](#comparison-with-ipystata)
- [Installation](#installation)
    - [Prerequisites](#prerequisites)
    - [Package Install](#package-install)
- [Configuration](#configuration)
- [Using the Stata kernel](#using-the-stata-kernel)
    - [Jupyter Notebook](#jupyter-notebook)
    - [Atom](#atom)
    - [Console](#console)
- [Limitations](#limitations)
- [Troubleshooting](#troubleshooting)
- [To do](#to-do)

Atom             |  Jupyter Notebook
:-------------------------:|:-------------------------:
![Atom](./img/atom.png)    |  ![Jupyter Notebook](./img/jupyter_notebook.png)


## Features

`stata_kernel` is undergoing active development. It works now, but will be more
polished in a week.

- [x] Supports Windows, macOS, and Linux.
- [x] Works with Jupyter Notebook and Atom's Hydrogen package
- [x] Allows for inline and block comments.
- [x] Display graphs (still working out limitations)
- [x] Work with a remote session of Stata
- [ ] Special shorthand "magics" to aid with, for example benchmarking code.
- [ ] Documentation website.
- [ ] Autocompletions as you type based on the variables, macros, and return objects currently in memory.
- [ ] Mata interactive support
- [ ] `#delimit ;` blocks
- [ ] Cross-session history file
- [ ] Easily pull up help files within the kernel.

### Comparison with [ipystata](https://github.com/TiesdeKok/ipystata)

Except for Windows, this package takes a different approach to communication
with Stata. On macOS and Linux, this controls the Stata console directly, which
prevents speed degradation with larger data sets. In contrast, `ipystata` saves
the data set to disk after each code cell, slowing execution.

This is also a pure Jupyter kernel, whereas `ipystata` is a Jupyter "magic" within the Python kernel. This means that you don't need to have any knowledge of Python* and don't need to have packages like Pandas installed.

\* Python is amazing language, and if you want to move on to bigger data, I highly recommend learning Python.

## Installation

### Prerequisites

- **Python**. In order to install the kernel, Python >= 3.5 needs to be installed. I suggest installing the [Anaconda distribution](https://www.anaconda.com/download/), which doesn't require administrator privileges and is simple to install. If you want to use less disk space, install [Miniconda](https://conda.io/miniconda.html), which includes few packages other than Python.
- **Windows only:**
    - Install [pywin32](https://github.com/mhammond/pywin32/releases/latest), which lets Python talk to Stata. Choose the version of Python you have installed:
        - [Python 3.5](https://github.com/mhammond/pywin32/releases/download/b223/pywin32-223.win-amd64-py3.5.exe)
        - [Python 3.6](https://github.com/mhammond/pywin32/releases/download/b223/pywin32-223.win-amd64-py3.6.exe)
        - [Python 3.7](https://github.com/mhammond/pywin32/releases/download/b223/pywin32-223.win-amd64-py3.7.exe)
    - [Link the Stata Automation library](https://www.stata.com/automation/#install). The Stata executable is most likely in the folder `C:\Program Files (x86)\Stata15`.

        1. In the installation directory, right-click on the Stata executable, for example, StataSE.exe. Choose "Create Shortcut".
        2. Right-click on the newly created "Shortcut to StataSE.exe", choose "Property", and change the Target from "C:\Program Files\Stata13\StataSE.exe" to "C:\Program Files\Stata13\StataSE.exe" /Register. Click "OK".
        3. Right-click on the updated "Shortcut to StataSE.exe"; choose "Run as administrator"

### Package Install

To install the kernel, run:

```
$ pip install stata_kernel
$ python -m stata_kernel.install
```


## Configuration

The configuration file is named `.stata_kernel.conf` and is located in your home directory. On Windows, this attempts to find the path to your Stata executable. Otherwise, you need to set the path to your Stata executable before running the kernel.

- `stata_path`: The path to your Stata executable.

    On Mac, the default installation is in a place like
    ```
    /Applications/Stata/StataSE.app/Contents/MacOS/
    ```

    There are two executables: `StataSE` and `stata-se`. The former opens the GUI
    and should be used if you choose `automation` mode, while the latter opens the
    console and should be used if you choose `console` mode.

- `execution_mode`: This can be set to `automation` or `console`, and is the manner in which this kernel talks to Stata. `automation` uses Stata Automation to communicate to Stata while `console` controls the console version of Stata. `automation` is only available on Windows or macOS. `console` is only available on macOS or Linux. On macOS, `automation` is the preferred option, though may have more bugs at the moment than `console`.
- `cache_directory`: This is the directory for the kernel to store temporary log files and graphs. By default, this is `~/.stata_kernel_cache`, where `~` means your home directory.
- `graph_format`: This is the format to export and display graphs. By default this is `svg`, but if you're on an older version of Stata, you could switch to `png`. There is also some support for `pdf` if using Jupyter Notebook.

## Using the Stata kernel

The main ways to use this are through Jupyter notebook, Atom, or an enhanced console.

### Jupyter Notebook

You can start the Jupyter Notebook server by running `jupyter notebook` in your terminal or command prompt. The *New* menu in the notebook should show an option for a Stata notebook.

### Atom

Download the [Atom text editor](https://atom.io) and install the [Hydrogen package](https://atom.io/packages/hydrogen), and [language-stata](https://atom.io/packages/language-stata) syntax highlighting package.

Open a `.do` file and run <kbd>Ctrl</kbd>-<kbd>Enter</kbd> to start the Stata kernel.

### Console

To use it as a console, run:
```
$ jupyter console --kernel stata
```

Example:

<img style="max-width: 500px; height: auto; " src="./img/jupyter_console.png" />

## Limitations

- If you make multiple graphs within the same block of code, you need to give them different names with the `name()` argument, or only one will show up.
- Currently can only make one image per minute on Windows/Mac unless you give different names for each graph with `name()`. This will be fixed soon.

## Troubleshooting

If the `pip install` step gives you an error like "DEPRECATION: Uninstalling a distutils installed project (pexpect) has been deprecated", try
```
$ pip install git+https://github.com/kylebarron/stata_kernel --ignore-install pexpect
```

## To do

- [ ] Support autocompletions based on the variables, macros, and return objects currently in memory.
- [ ] Improve syntax highlighting
