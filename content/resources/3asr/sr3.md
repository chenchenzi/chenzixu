---
title: "ASR from Scratch I: Training models of Hong Kong Cantonese using the Kaldi recipe"
linktitle: "3. ASR from Scratch I: Training models of Hong Kong Cantonese using the Kaldi recipe"
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

It is surprising that mainstream forced aligner tools such as MFA, WebMAUS, CLARIN, FAVE, P2FA, and Charsiu have not yet had any pre-trained models of Cantonese (Yue Chinese), native to approximately 82.4 million speakers (Wikipedia), at the time of this blog (2 Oct 2023). Hence, in this chapter I demonstrate how to train acoustic models of Hong Kong Cantonese from scratch using a classic HMM-GMM model through [Kaldi](https://kaldi-asr.org/doc/about.html), a state-of-the-art ASR toolkit.

This tutorial is built on Eleanor Chodroff's awesome [tutorial](https://eleanorchodroff.com/tutorial/kaldi/index.html) on Kaldi and the Kaldi [official guide](https://kaldi-asr.org/doc/kaldi_for_dummies.html), with enriched implementation details. A dozen of Python snippets were created to prepare the datasets and acquire the forced alignment outputs in the TextGrid format. 
For more details on the explanations of certain steps, please refer to their tutorials. 

All the Python scripts will be available on my Github {{< icon name="github" pack="fab" >}} [to be updated]. This tutorial is very long. Feel free to navigate through the menu on the right on a computer screen.

> The **Montreal Forced Aligner (MFA)** is built upon Kaldi ASR and provides much straightforward commands for model training and forced alignment. This Kaldi tutorial manifests the inner workings of MFA. For the theory of statistical speech recognition and the MFA approach, check out my [slides](https://chenzixu.rbind.io/slides/asr/asr_talk#/title-slide).  

<br>

## 3.1 Kaldi Installation

The **Kaldi** download and installation is documented in the official [Kaldi](http://www.kaldi-asr.org/doc/install.html) website. Eleanor's [tutorial](https://eleanorchodroff.com/tutorial/kaldi/installation.html) also provided the steps in detail. Here is a recap of the general downloading and installation instructions.

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
### 3.1.1 Installing Kaldi on a Mac with M1 chip or above{#mac-m1}

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

<br>

## 3.2 Setting up Kaldi directories

There is a conventional directory structure for training data and models. We can create a new directory `fa-canto/` under the `kaldi/egs/` directory to house the relevant files for our project of HK Cantonese. The skeleton for `fa-canto/` should be as follows:

```
. 
├── cmd.sh
├── conf
│   └── mfcc.conf
├── data
│   ├── lang
│   ├── local
|   |   └── lang
│   └── train
├── exp
├── mfcc
├── path.sh
├── src -> ../../src
├── steps -> ../wsj/s5/steps
└── utils -> ../wsj/s5/utils
```

❶ The skeleton structure

To achieve the above structure, you can use the following code in a Unix Shell. For more details, please read [here](https://eleanorchodroff.com/tutorial/kaldi/training-acoustic-models.html#prepare-directories). Alternatively, for the most part you can also just right click your mouse/pad, select `New Folder`(Mac), and rename it accordingly.

```bash
cd kaldi/egs
mkdir fa-canto

cd fa-canto
ln -s ../wsj/s5/steps .
ln -s ../wsj/s5/utils .
ln -s ../../src .
                    
cp ../wsj/s5/path.sh .
```
Check and edit the `path.sh` file in the `vim` editor. Alternatively, you can open this file in a text editor and modify it from there.

```bash
vim path.sh
```
Press `i` on your keyboard and change the path line in `path.sh` to:

```bash
export KALDI_ROOT='pwd'/../..
```
Press `esc` and then `:wq` to save and quit.
Then complete the `fa-canto/` skeleton:

```
cd fa-canto
mkdir exp
mkdir conf
mkdir data
                    
cd data
mkdir train
mkdir lang
mkdir local
                    
cd local
mkdir lang
```

{{% alert note %}}
`vim` is a command-line editor. Typing `vim <text file>` in a Unix shell opens the file:
- Press `i` to insert and edit; Press `esc` to exit insert mode;
- Press `:wq` to write and quit; `:q` to quit normally; 
- Press `:q!` to quit forcibly (without saving).
{{% /alert %}}

❷ The `mfcc.conf` file

In the `conf/` directory we create a `mfcc.conf` file containing the parameters for MFCC extraction. Again we can use `vim` editor within the Shell:

```
cd fa-canto/conf
vim mfcc.conf
```
In the insert mode, add the following lines:

```
--use-energy=false  
--sample-frequency=16000
```
The sampling frequency should match that of your audio data. 

❸ The parallelisation wrapper

Kaldi provides a wrapper to implement data processing and training in parallel, taking advantage of the multiple processors/cores. Kaldi’s wrapper scripts include `run.pl`, `queue.pl`, `slurm.pl`, etc. The applicable script and parameters will be specified in a file called `cmd.sh` in our directory. For more details about this, please read Eleanor's [tutorial](https://eleanorchodroff.com/tutorial/kaldi/training-acoustic-models.html#set-the-parallelization-wrapper) and the [official guide](http://www.kaldi-asr.org/doc/queue.html).

Since I am going to train the data on a personal laptop, I used the `run.pl` script and set up the `cmd.sh` as follows:

```
cd fa-canto  
vim cmd.sh
```
Insert the following lines in `cmd.sh`:
```
train_cmd="run.pl"
decode_cmd="run.pl"
```
`run.pl` runs all the jobs you request on the local machine, and does so in parallel if you use a job range specifier, e.g. JOB=1:4. 

{{% alert warning %}}
`run.pl` doesn't keep track of how many CPUs are available or how much memory your machine has. If you use `run.pl` to run scripts that were designed to run with `queue.pl` on a larger grid, you may end up exhausting the memory of your machine or overloading it with jobs (from Kaldi official guide). 
{{% /alert %}}

Lastly, we run the `cmd.sh` file:
```
. ./cmd.sh
```

<br>

## 3.3 The Common Voice Dataset

We will be using the latest `Chinese (Hong Kong)` subset of the publicly available Common Voice corpus `Common Voice Corpus 15.0` updated on 9/14/2023, which contains 108 hours of validated speech with 3001 voices (3.3G `.mp3` files).

You can download the dataset [here](https://commonvoice.mozilla.org/en/datasets) and unzip it to your working directory. The downloaded corpus has the following structure:

```
├── cv-corpus-15.0-2023-09-08
... └── zh-HK
        ├── clip_durations.tsv
        ├── train.tsv
        ├── validated.tsv
        ├── dev.tsv
        ├── test.tsv
        ├── Invalidated.tsv
        ├── other.tsv
        ├── reported.tsv
        ├── times.txt
        └── clips
            ├── common_voice_zh-HK_20096730.mp3
            ├── common_voice_zh-HK_20096731.mp3
            ├── common_voice_zh-HK_20096732.mp3
            ├── ...
            └── ... #118736 items in total
```
Most of the audio clips are short utterances or sentences (fewer than 30 syllables) and have their corresponding transcription in these `.tsv` files. In this tutorial, we will mainly use the training set (see `train.tsv`) which has 8426 recordings.

<br>

## 3.4 Data preprocessing

### 3.4.1 Setting up the environment

I created a new Conda environment named 'acoustics' for acoustic processing. This step is not compulsory, but highly recommend because you can manage a collection of packages and their dependencies related to this project or pipeline in one place.

```bash
conda create -n acoustics
conda activate acoustics
```

The Python packages I used include: `datasets`, `re`, `os`, `pandas`, `csv`, `tqdm`, `subprocess`, `transformers`, `lingpy`, `praatio`. You can install these via Conda or other package management systems. We also need [sox](https://sourceforge.net/projects/sox/), a command-line audio editing software.

I created a working directory for data pre-processing `fa-cantonese/` and moved the data folder here:  `fa-cantonese/cv-corpus-15.0-2023-09-08/`.

```bash
cd ~/Work
mkdir fa-cantonese
```

### 3.4.2 Audio preprocessing: `.mp3` to `.wav`

While the compressed format `.mp3` is storage-friendly, we should use `.wav` for acoustic modeling and training.
Inside the Common Voice directory `cv-corpus-15.0-2023-09-08/`, I created a new directory `clips_wavs/` for converted `.wav` files.

In my working directory `fa-cantonese/`, I wrote a python script to convert the audio format from `.mp3` to `.wav` with 16K sampling rate, using `sox`. The package `subprocess` enables running the external `sox` command in parallel.

```python
import re
import os
from tqdm import tqdm
import subprocess
from tqdm.contrib.concurrent import process_map

path = 'cv-corpus-15.0-2023-09-08/zh-HK/clips'
output = 'cv-corpus-15.0-2023-09-08/zh-HK/clips_wavs'

file_pairs = [(file,re.search(r'(.*?)\.mp3',file).group(1)+'.wav') for file in tqdm(os.listdir(path))]

def convert_and_resample(item):
    command = ['sox', os.path.join(path,item[0]),'-r','16000',os.path.join(output,item[1])]
    subprocess.run(command)

if __name__ == '__main__':
    wavs = process_map(convert_and_resample, file_pairs, max_workers=4, chunksize=1)
```

### 3.4.3 Transcripts preparation: The `text` file

The Kaldi recipe will require a `text` file with the utterance-by-utterance transcript of the corpus, which has the following format (see Elinor's tutorial [here](https://eleanorchodroff.com/tutorial/kaldi/training-acoustic-models.html#text)):

```
utt_id WORD1 WORD2 WORD3 WORD4 …
# utt_id = utterance ID
```
We can achieve this in two steps:

❶ Define an utterance ID

In our Common Voice corpus, we have the `train.tsv` which contains columns such as `client_id` (I assumed this representing a unique speaker), `path` (a unique file name), and `sentence` (the transcript for the audio file). We can define an utterance ID by concatenating the `client_id` and the unique numbers in `path` after removing the prefix `common_voice_zh-HK_` and the file extension `.mp3`.

{{% alert note %}}
The conventional way to create an **utterance ID** is to concatenate the speaker ID and the utterance index, so that an utterance ID embeds the relevant speaker information. 
{{% /alert %}}


❷ Tokenise Cantonese transcript

The Chinese orthography is very useful in identifying syllables. A good way to tokenise Cantonese transcript is to separate each character with a space, so that we will obtain syllable-level alignment of speech and text. Note that in Hong Kong Cantonese, there is often code-switching and thus you will occasionally see English words in the transcripts.

The following Python snippet will help us prepare the `text` file.

```python
from datasets import load_dataset
import re

cv_tsv = load_dataset('csv', 
                      data_files="cv-corpus-15.0-2023-09-08/zh-HK/train.tsv",
                      sep="\t")

cv_tsv = cv_tsv['train']
cv_text = cv_tsv.remove_columns(['up_votes', 'down_votes', 'age', 'gender', 'accents', 'variant', 'locale', 'segment'])

def prepare_text(batch):
  """Function to preprocess the dataset with the .map method"""
  transcription = batch["sentence"]
  utt_id = batch['path']
  spk_id = batch['client_id']
  
  #remove punctuation
  puncs = r'[^\u4e00-\u9FFFa-zA-Z0-9 ]'
  transcription = re.sub(puncs, '', transcription)
  
  #add space between Chinese characters
  transcription = re.sub(r'([\u4e00-\u9fff])', r'\1 ', transcription).strip()
  #add space after an English word followed by a Chinese character
  transcription = re.sub(r'([a-zA-Z0-9_]+)([\u4e00-\u9fff])', r'\1 \2', transcription)
  
  batch["sentence"] = transcription
  batch['client_id']= spk_id+ '-'+ utt_id[19:-4]
  
  return batch

texts = cv_text.map(prepare_text, desc="preprocess text")
texts = texts.remove_columns(['path'])
texts.to_csv('text', sep='\t', header=False,index=False)
```

The output of the above script looks as follows:
```
9a3a...43-22235680	兩 個 人 扯 貓 尾  唔 認 數
9a3a...43-22235715	你 知 唔 知 點 去 中 環 安 慶 台 嗰 間 缽 仔 糕
9a3a...43-22235825	車 站 路
9a3a...43-22235895	陳 師 奶 要 去 深 水 埗 欽 州 街 搵 個 朋 友 一 齊 打 麻 雀
...

# utterance ID is not fully shown given the space here
```

We can then move this `text` file to our kaldi training directory at `kaldi/egs/fa-canto/data/train`. 

### 3.4.4 The dictionary `lexicon.txt`: Cantonese G2P

We will need a Cantonese pronunciation dictionary `lexicon.txt` of the words/characters, in fact, **only** the words, present in the training corpus. This will ensure that we do not train extraneous phones. If we want to use IPA symbols for acoustic models, we should transcribe the words/characters in IPA in this dictionary.

We can download an open Cantonese dictionary from {{< icon name="github" pack="fab" >}} [open-dict-data](https://raw.githubusercontent.com/open-dict-data/ipa-dict/master/data/zh.txt) or {{< icon name="github" pack="fab" >}} [CharsiuG2P](https://raw.githubusercontent.com/lingjzhu/CharsiuG2P/main/dicts/yue.tsv) and utilise the multilingual [CharsiuG2P](https://github.com/lingjzhu/CharsiuG2P) tool with a pre-trained Cantonese model for grapheme-to-phoneme conversion.

The open dictionary has the following format:
```
𠻺	/a:˨/
㝞	/a:˥/
㰳	/a:˥/, /a:˧/
...
䧄	/a:k˥/, /kɔ:k˧/
...
```
Generally, we want ❶ each phone in the dictionary to be separated by a space. ❷ The tone label is always put at the end of an IPA token, which gives an impression of tone being a linearly arranged segment. Tone, however, is suprasegmental. We might want to exclude the tone labels here. ❸ We can have multiple pronunciation entries for a word, which are usually put in different rows. ❹ We need to add the pseudo-word entries such as `<oov>` and `{SL}`, required by the Kaldi training recipe. `<oov>` stands for ‘out of vocabulary’ items including unknown sounds and laughters, `{SL}` for silence.

Therefore, we need to revise the format of a downloaded dictionary. The following python script creates a `lexicon.txt` file using `CharsiuG2P` and their open dictionary.

```python
from transformers import T5ForConditionalGeneration, AutoTokenizer
from tqdm import tqdm
import pandas as pd
from lingpy import *

# load G2P models
model = T5ForConditionalGeneration.from_pretrained('charsiu/g2p_multilingual_byT5_small_100')
tokenizer = AutoTokenizer.from_pretrained('google/byt5-small')
model.eval()

# load pronunciation dictionary
pron = {l.split('\t')[0]:l.split('\t')[1].strip() for l in open('yue.tsv','r',encoding="utf-8").readlines()}

with open('lexicon.txt','w', encoding='utf-8') as output:
    
    rows=[]
    with open('words.txt','r',encoding='utf-8') as f:
        for line in tqdm(f):
            w = line.strip()
            word_pron = ''
            if w in pron:
                word_pron+=pron[w]
            else:
                out = tokenizer(['<yue>: '+w],padding=True,add_special_tokens=False,return_tensors='pt')
                preds = model.generate(**out,num_beams=1,max_length=50)
                phones = tokenizer.batch_decode(preds.tolist(),skip_special_tokens=True)
                word_pron+=phones[0]
            
            rows.append([w,word_pron])
    
    lexicon = pd.DataFrame(rows, columns=['word', 'ipa'])
    lexicon['ipa'] = lexicon['ipa'].str.split(',')
    lexicon = lexicon.explode('ipa')
    
    #remove IPA tones and tokenize IPA-encoded strings
    lexicon['ipa'] = lexicon['ipa'].str.replace(r'[\u02E5-\u02E9]+', '', regex=True)
    lexicon['ipa'] = lexicon['ipa'].apply(lambda x: ' '.join(map(str, ipa2tokens(x))))

    #remove duplicated rows if any
    lexicon.drop_duplicates(inplace=True)
    lexicon.to_csv(output,sep='\t', index=False, header=False)
```

The final dictionary is as follows: 
```
<oov>	oov
{SL}	sil
A	a:
Annual	a: nn ʊ ŋ
Anson	a: n s ɔ: n
B	b i:
Browser	pʰ r ɔ: w s ɐ
...
一	j ɐ t
丁	t s a: ŋ
丁	t ɪ ŋ
丁	t s ɐ ŋ
...
```

We can then move this `lexicon.txt` file to our kaldi training directory at `kaldi/egs/fa-canto/data/local/lang`. 

### 3.4.4 Other text files for Kaldi `data/train` 

To train acoustic models in Kaldi, we also need text files:
- `segments`, 
- `wav.scp`, 
- `utt2spk`,
- `spk2utt`.

We can generate them in our data working directory `fa-cantonese/` and then move them to the kaldi training directory at `kaldi/egs/fa-canto/data/train`. I included some Python snippets to facilitate creating these files. You can also achieve this in your preferred programming language.

❶ The `utt2spk` file

The `utt2spk` file contains the mapping of each utterance to its corresponding speaker, which has the following general form:
```
utt_id spkr
# utt_id: utterance ID
# spkr: speaker ID
```
We can use the script below:

```python
import pandas as pd

cv_tsv = pd.read_csv('cv-corpus-15.0-2023-09-08/zh-HK/train.tsv', sep='\t')

df = cv_tsv[['path','client_id']]
# create utt_id
df['path']=df['path'].str.replace('common_voice_zh-HK_','')
df['path']=df['path'].str.replace('.mp3','')
df['utt_id']=df['client_id'] +'-'+df['path']
# save it as a reference table
df.to_csv('table.csv', index=False, header=False)

df = df[['utt_id', 'client_id']]
df.drop_duplicates(inplace=True)
df.to_csv('utt2spk', sep=' ', index=False, header=False)
```

The output is as follows (the utterance ID is not fully shown):
```
01b0...2e-22244218 01b0...2e
01b0...2e-22391509 01b0...2e
01b0...2e-22391576 01b0...2e
...
```

Apart from outputting the `utt2spk` file, I also made a `table.csv` containing the pairing information of `utt_id`, `client_id`, and `path`(file_id) for later reference.

❷ The `wav.scp` file

`wav.scp` contains the file path for all audio files, which has the following general form:
```
file_id path/file
```
In our case, we **only have one utterance in one audio file**. So the `file_id` is the same as `utt_id`.

```python
import os
import pandas as pd

dir = 'cv-corpus-15.0-2023-09-08/zh-HK/clips_wavs'

table = pd.read_csv('table.csv', names = ['file', 'client_id','utt_id'])

path=[]

for root, dirs, files in os.walk(os.path.abspath(dir)):
    for file in files:
        path.append([file[19:-4], os.path.join(root, file)])

df = pd.DataFrame(path, columns=['file', 'path'])
df['file'] = df['file'].astype('int64')

df_update = pd.merge(df, table, on='file', how='right')
df_update = df_update[['utt_id','path']]
df_update.to_csv('wav.scp', sep=' ', index=False, header=False)
```
The output is as follows (the utterance ID is not fully shown):

```
01b0...2e-22244218 /Users/cx936/Work/fa-cantonese/cv-corpus-15.0-2023-09-08/zh-HK/clips_wavs/common_voice_zh-HK_22244218.wav
01b0...2e-22391509 /Users/cx936/Work/fa-cantonese/cv-corpus-15.0-2023-09-08/zh-HK/clips_wavs/common_voice_zh-HK_22391509.wav
01b0...2e-22391576 /Users/cx936/Work/fa-cantonese/cv-corpus-15.0-2023-09-08/zh-HK/clips_wavs/common_voice_zh-HK_22391576.wav
...
```

❸ The `segments` file

The `segments` file contains the start and end time for each utterance in an audio file, with the following general form:
```
utt_id file_id start_time end_time
```
Again in our case, the first two fields are the same, the start time is always 0, and the end time is the total duration of an audio file. We can make use of the duration information in `clip_durations.tsv` available in the Common Voice dataset.

```python
import pandas as pd

dur = pd.read_csv('cv-corpus-15.0-2023-09-08/zh-HK/clip_durations.tsv', sep='\t', header=0)

dur['file']=dur['clip'].str.replace('common_voice_zh-HK_','')
dur['file']=dur['file'].str.replace('.mp3','')
dur['file'] = dur['file'].astype('int64')

table = pd.read_csv('table.csv', names = ['file', 'client_id','utt_id'])

df = pd.merge(dur, table, on='file', how='right')
df['file_id'] = df['utt_id']
df['start_time'] = 0.0
df['end_time']= df['duration[ms]']/1000
df = df[['utt_id','file_id','start_time','end_time']]

df.to_csv('segments', sep=' ', index=False, header=False)
```
The output is as follows (the utterance ID is not fully shown):

```
01b0...2e-22244218 01b0...2e-22244218 0.0 5.832
01b0...2e-22391509 01b0...2e-22391509 0.0 4.584
01b0...2e-22391576 01b0...2e-22391576 0.0 4.848
...
```

❹ The `spk2utt` file

`spk2utt` is a file that contains the speaker to utterance mapping, with the following general form:
```
spkr utt_id1 utt_id2 utt_id3
```
We can use the Kaldi script below to automatically generate this file given the `utt2spk` file and also check if all required data files are present and in the correct format.

```bash
utils/fix_data_dir.sh data/train
```
The output is as follows (the utterance ID is not fully shown):

```
01b0...2e 01b0...2e-22244218 01b0...2e-22391509 01b0...2e-22391576 01b0...2e-22391635 01b0...2e-22391636 ...  
...
```

### 3.4.5 Other text files for Kaldi `data/local/lang` 

Apart from the `lexicon.txt` file, we should create a few other text files about the language data specific to our training corpus including
- `nonsilence_phones.txt`,
- `optional_silence.txt`,
- `silence_phones.txt`,
- `extra_questions.txt` (optional).

❶ The `nonsilence_phones.txt` file

This file contains a list of all the typical phones that are not silence.

```bash
cut -f 2- lexicon.txt | sed 's/ /\n/g' | sort -u > nonsilence_phones.txt
```
{{% alert note %}}
In `lexicon.txt`, I have used a **tab** between a Cantonese character and its tokenised (space-separated) IPA string, which is the default delimiter for the `cut` command. You can specify the delimiter using the flag `-d '(delimiter)'` if you used a different one.
{{% /alert %}}

The `nonsilence_phones.txt` is displayed below:

```
a:
a:i
a:u
b
d
e
ei
f
h
i:
...
```

❷ The `optional_silence.txt` file

This file only has one line, the `sil` (silence) phone.
```bash
echo 'sil' > optional_silence.txt
```

❸ The `silence_phones.txt` file

This file will contain the special, non-word phones in your dictionary:
```
sil
oov
```
You can use the following code:

```bash
echo "sil\noov" > silence_phones.txt
```

❹ The `extra_questions.txt` file

We can skip this step for now. A Kaldi script will generate a basic `extra_questions.txt` file for you, but in `data/lang/phones`. For more details, read [here](https://eleanorchodroff.com/tutorial/kaldi/training-acoustic-models.html#extra_questions.txt).

### 3.4.6 Creating files for Kaldi `data/lang` 

We then use a script to generate files in  `data/lang`. This shell script takes four arguments: `<dict-dir/>`, `<'oov-word-entry'>`, `<tmp-dir/>`, and `<lang-dir/>`. In our case, we can use the script as follows:

```
cd fa-canto
utils/prepare_lang.sh data/local/lang '<oov>' data/local/ data/lang
```
You will notice new files generated in the `data/lang` directory: `L.fst`, `L_disambig.fst`, `oov.int`, `oov.txt`, `phones.txt`, `topo`, `words.txt`, and `phones/`. It is worth checking out the `extra_questions.txt` file inside the `phones/` directory to see how the model may be learning more about a phone’s contextual information.

If you arrive at this stage, you have completed the data preparation. We can now start training our first model.

<br>

## 3.5 Extracting MFCC features

The first step is to extract the Mel Frequency Cepstral Coefficients (MFCCs) of the speech signals and compute the cepstral mean and variance normalization (CMVN) stats, using shell scripts as follows:

```bash
cd fa-canto

mfccdir=mfcc  
x=data/train  
steps/make_mfcc.sh --cmd "$train_cmd" --nj 4 $x exp/make_mfcc/$x $mfccdir  
steps/compute_cmvn_stats.sh $x exp/make_mfcc/$x $mfccdir
```
The `--nj` option specifies the number of jobs to be sent out and processed in parallel. Since I was training the dataset on my (not-too-powerful) laptop, I kept this number low here.

{{% alert note %}}
Kaldi keeps data from the same speakers together, so you do not want more splits than the number of speakers you have.
{{% /alert %}}

## 3.6 Training and alignment

The training and alignment procedure follows Eleanor's tutorial, which is briefly recapped here. There are a handful of training scripts based on different algorithms. 

{{% alert note %}}
A training script takes the following arguments in order:

- Location of the acoustic data: `data/train` 
- Location of the lexicon: `data/lang`
- Source directory for the model: `exp/lastmodel`
- Destination directory for the model: `exp/currentmodel`
{{% /alert %}}

...to be continued.

### 3.6.1 Monophone training and alignment

```
cd fa-canto  
utils/subset_data_dir.sh --first data/train 8000 data/train_8k

steps/train_mono.sh --boost-silence 1.25 --nj 4 --cmd "$train_cmd" \
data/train_8k data/lang exp/mono_8k
```
...to be continued.

### 3.6.2 Triphone training and alignment

```
steps/train_deltas.sh --boost-silence 1.25 --cmd "$train_cmd" \
2000 10000 data/train data/lang exp/mono_ali exp/tri1 || exit 1;
```
Align delta-based triphones
```
steps/align_si.sh --nj 4 --cmd "$train_cmd" \
data/train data/lang exp/tri1 exp/tri1_ali || exit 1;
```
Train delta + delta-delta triphones
```
steps/train_deltas.sh --cmd "$train_cmd" \
2500 15000 data/train data/lang exp/tri1_ali exp/tri2a || exit 1;
```
Align delta + delta-delta triphones
```
steps/align_si.sh  --nj 4 --cmd "$train_cmd" \
--use-graphs true data/train data/lang exp/tri2a exp/tri2a_ali  || exit 1;
```
Train LDA-MLLT triphones
```
steps/train_lda_mllt.sh --cmd "$train_cmd" \
3500 20000 data/train data/lang exp/tri2a_ali exp/tri3a || exit 1;
Align LDA-MLLT triphones with FMLLR
steps/align_fmllr.sh --nj 4 --cmd "$train_cmd" \
data/train data/lang exp/tri3a exp/tri3a_ali || exit 1;
```
Train SAT triphones
```
steps/train_sat.sh  --cmd "$train_cmd" \
4200 40000 data/train data/lang exp/tri3a_ali exp/tri4a || exit 1;
Align SAT triphones with FMLLR
steps/align_fmllr.sh  --cmd "$train_cmd" \
data/train data/lang exp/tri4a exp/tri4a_ali || exit 1;
```

## 3.7 Forced Alignment

### 3.7.1 Extracting alignment: from CTM output to phone-n-word alignments

### 3.7.2 Creating Praat TextGrids

### Credit

I would like to thank Jian Zhu for his G2P help and Yajie Gu for scripting suggestions.