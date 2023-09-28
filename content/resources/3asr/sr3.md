---
title: ASR from Scratch
linktitle: 3. ASR from scratch
toc: true
type: docs
date: "2023-9-27T00:00:00+01:00"
draft: false
menu:
  speechrecognition:
    parent: Speech Recognition
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

<br>

In this chapter, I demonstrate how to train acoustic models from scratch using a classic HMM-GMM model (Kaldi) and a neural-network-based model.

<br>

## 3.1 Kaldi Installation

The **Kaldi** download and installation is documented in the official [Kaldi](http://www.kaldi-asr.org/doc/install.html) website. [Eleanor Chodroff's tutorial](https://eleanorchodroff.com/tutorial/kaldi/installation.html) also provided the steps in detail. Here is a recap of the general downloading and installation instructions.

If you are a MacOS user with M1 chip, feel free to jump to [Section 1.1.1](#mac-m1) for more details.

**Prerequisites**

Kaldi is now hosted on [Github](https://github.com/kaldi-asr/kaldi) for development and distribution. You will need to install [**Git**](https://git-scm.com/downloads) {{< icon name="github" pack="fab" >}}, the version control system, on your machine. 

Software Carpentry has a nice [tutorial](https://swcarpentry.github.io/git-novice/) on Git for beginners, which includes installation of Git across various operating systems.

**Downloading**

Navigate to the working directory where you would like to install Kaldi (in my case: `~/Work/`), and download the Kaldi toolkit via `git clone`.

```bash
cd ~/Work
git clone https://github.com/kaldi-asr/kaldi.git kaldi --origin upstream
```

**Installation**

Follow the instructions in the file `INSTALL` in the downloaded directory `kaldi/` to complete the build of the toolkit. It should involve the following steps: 

```bash
cd kaldi/tools  
extras/check_dependencies.sh  
make

cd ../src  
./configure  
make depend  
make
```
### 3.1.1 Installing Kaldi on a Mac with M1 chip{#mac-m1}

I have encountered many challenges in installing Kaldi on my Mac (Ventura 13.1) with an M1 chip (updated 27 Sept, 2023) and spent a long time debugging. Here I would like to share some tips for those who have similar laptops and builds to assist you in this installation process 🫶. 

The steps below were tested on macOS Ventura (13.1), but may also work for other recent versions with Apple silicon as well.

**Prerequisites**

❶ You will need Xcode's Command Line Tools. Install Xcode:

```bash
xcode-select --install
```
You will be prompted to start the installation, and to accept a software license. Then the tools will download and install automatically.

❷* [Homebrew](https://brew.sh/), one of the best free and open-source software package management systems for MacOS (and Linux), is recommended. You can install it using the following code.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

❸* [Anaconda](https://www.anaconda.com/download) or miniconda is recommended for creating environments which allows for isolating project-specific dependencies. Follow the link to download the installer.

**Switching Terminal Architecture**

Set up a development environment can be a frustrating process given the updates of different packages and their compatibility. Some applications and tools, for instance, have not yet offer full native support for Apple's M1/M2 architecture.

To install Kaldi, we should use the `x86_64` architecture (up to this date) instead of the Apple native `arm64` architecture.

There is a [blog post](https://www.courier.com/blog/tips-and-tricks-to-setup-your-apple-m1-for-development/) by Chris Gradwohl that introduces **Rosetta Terminal** {{< icon name="terminal" pack="fas" >}}, the `x86_64` emulator. With the translation layer [Rosetta2](https://developer.apple.com/documentation/apple-silicon/about-the-rosetta-translation-environment) by Apple, we can download and compile applications that were built for `x86_64` and run them on Apple silicon. Unfortunately the instructions in the blog post are no longer working for MacOS **Ventura** where the option to easily duplicate the Terminal.App is disabled. 

But there is a quick way to switch your terminal default architectures to enable maintaining separate libraries and environments for the `arm64` and `x86_64` architectures.

Locate the `.zshrc` file and append the following lines to the end. 

```bash
alias arm="env /usr/bin/arch -arm64 /bin/zsh --login"

alias intel="env /usr/bin/arch -x86_64 /bin/zsh --login" 
```
*Source*: [https://developer.apple.com/forums/thread/718666](https://developer.apple.com/forums/thread/718666)

The Zsh shell configuration file, `.zshrc`, is usually located in your home directory `~/.zshrc` and you can use `vim` to edit it in the terminal. Alternatively, you can reveal the hidden files by pressing `⌘ + shift + .` and edit the `.zshrc` file in your preferred editor.

In fact, the `.zshrc` file does not exist by default. If you don't have this file, you can create one by `nano ~/.zshrc`, add in these two lines, and hit `control + X`, then `Y` after the prompt, and `return` to save the changes.

These commands create aliases `arm` and `intel` for the architectures `arm64` and `x86_64` respectively. For Kaldi installation, we need the `x86_64` architecture, so we only need to type `intel` in the terminal to invoke it. 

```
intel
arch
```
To confirm the switch, you can type `arch`. If the output is `i386`, then it is successful.

**Creating a Specified Python Environment**

Before compiling Kaldi, you can utilise Homebrew (aka `brew`) to install the necessary additional packages.

```bash
brew install automake autoconf wget sox gfortran libtool subversion
```

Python2.7 is also needed somehow, although very much sunsetted. You can create a separate Python2 environment. The following code creates a Python 2.7 environment named 'kaldi' and activates it.

```bash
conda create -n kaldi python=2.7
conda activate kaldi
```
**Downloading Kaldi**

Navigate to the working directory where you would like to install Kaldi (in my case: `~/Work/`), and download the Kaldi toolkit via `git clone`.

```bash
cd ~/Work
git clone https://github.com/kaldi-asr/kaldi.git kaldi --origin upstream
```

**Installing Tools**

Navigate to the `kaldi/tools/` directory and check if all required dependencies are installed.

```bash
cd kaldi/tools
extras/check_dependencies.sh
```

*OpenBLAS*

It is likely that you receive an error message as follows: 
{{% alert warning %}}
```
OpenBLAS not detected. Run extras/install_openblas.sh
 ... to compile it for your platform, or configure with --openblas-root= if you
 ... have it installed in a location we could not guess. Note that packaged
 ... library may be significantly slower and/or older than the one the above
 ... would build.
 ... You can also use other matrix algebra libraries. For information, see:
 ...   http://kaldi-asr.org/doc/matrixwrap.html
```
{{% /alert %}}

As suggested, we can install `OpenBLAS`：
```
extras/install_openblas.sh
```

It is likely that you find the following error message in the Terminal output:
{{% alert warning %}}
```
mv: rename xianyi-OpenBLAS-* to OpenBLAS: No such file or directory
```
{{% /alert %}}

This is due to the fact that [OpenBLAS](https://github.com/OpenMathLib/OpenBLAS) has updated and the downloaded and unzipped directory has a different name. We can modify the script of `install_openblas.sh` to make it work.

So in the `extras/` directory, we can find the script `install_openblas.sh` and open it in an editor.

We can add the following two lines below the shebang line `#!/usr/bin/env bash`.

```
OPENBLAS_VERSION=0.3.20
MACOSX_DEPLOYMENT_TARGET=11.0
```
Here you can change the `MACOSX_DEPLOYMENT_TARGET` to match your MacOS system. `11.0` worked for my laptop.

Then we locate and replace the error line `mv xianyi-OpenBLAS-* to OpenBLAS` in the script with:
```
mv OpenMathLib-OpenBLAS-0b678b1 OpenBLAS
```

In the terminal, re-run the modified script:
```
extras/install_openblas.sh
```
In this way, OpenBLAS should be installed successfully.

*Intel Math Kernel Libraries (MKL)*

Having re-run `extras/check_dependencies.sh`, it is likely that you receive another error message as follows: 
{{% alert warning %}}
```
extras/check_dependencies.sh: Intel MKL does not seem to be installed.
 ... Download the installer package for your system from: 
 ...   https://software.intel.com/mkl/choose-download
 ... You can also use other matrix algebra libraries. For information, see:
 ...   http://kaldi-asr.org/doc/matrixwrap.html
```
{{% /alert %}}

To install MKL, you can download the MKL standalone offline installer from the [official website](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-download.html) and follow the instructions of the installer to complete the installation of MKL libraries. Note down the path where MKL is installed. By default, it should be located somewhere in the `/opt/intel/` directory. On my laptop, the path of the `mkl.h` file is `/opt/intel/oneapi/mkl/2023.2.0/include/mkl.h`.

Then we edit the `check_dependencies.sh` script. Locate the lines that point to the path of the file `mkl.h`and update it accordingly:

```
    MKL_ROOT="${MKL_ROOT:-/opt/intel/oneapi/mkl}"
       # Check the well-known mkl.h file location.
    if ! [[ -f "${MKL_ROOT}/2023.2.0/include/mkl.h" ]] &&
```

In this way, the Intel MKL package is installed successfully.

Now we run `extras/check_dependencies.sh` the third time. You should be able to receive the `all OK` message.🤗

You can now finally install the tools required by Kaldi using the following code:
```
make -j 4
```

The parameter `4` here indicates the number of CPUs. The `-j` option enables a parallel build to speed up the process. To find out the number of CPU cores on a Mac, you can use the following code:

```
sysctl -n hw.ncpu
```

{{% alert note %}}
If you had other (failed) attempts of `make`, make sure to clean up the resulting downloaded directories such as `openfst-1.7.2` before running the Makefile again.
{{% /alert %}}

**Installing Source**

Navigate to the `kaldi/src/` directory, run the configuration and the Makefiles as follows:

```
cd ../src/
./configure --shared 
make depend -j 4
make -j 4
```
In the same way we enable the multi-CPU build by supplying the `-j` option. Then you can just wait till it finishes. 

Hopefully you will see `Done` in your terminal output and the Kaldi installation is successful.😎

### 3.1.2 Dataset
...to be continued.