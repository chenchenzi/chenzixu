---
# Course title, summary, and position.
linktitle: Forced Alignment Using P2FA and MFA
summary: A step-to-step guide to automatic forced alignment of your own corpus using P2FA and Montreal Forced Aligner. #<i class="fas fa-terminal"></i> Terminal <i class="fab fa-python"></i> Python
weight: 1

# Page metadata.
title: Forced Alignment Using P2FA and MFA
date: "2020-04-19T12:00:00Z"
lastmod: "2020-04-19T12:00:00Z"
tags: ["speech processing", "Mandarin", "forced alignment", "P2FA", "MFA", "speech recognition"]
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
reading_time: true
share: true
profile: true
editable: true

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  forcedalignment:
    name: Forced Alignment
    weight: 1
---

Acquiring a large amount of speech data can be 'cheap' and relatively easy today. The traditional way of manually transcribing and segmenting audios is, however, very time-consuming and 'expensive'. Algorithms of automatic speech recognition (ASR) can be extremely helpful in automatic transcription through speech-to-text, as well as allow for automatic alignment and synchronisation of speech signals to phonetic units.

A forced alignment system usually takes an audio file and its corresponding transcript as input and returns a text file, which is time-aligned at the phone and word levels. I employed two forced alignment systems: the **Penn Forced Aligner (P2FA)** and the **Montreal Forced Aligner (MFA)**. The former is built with the HTK speech recognition toolkit, while the latter with a similar system Kaldi ASR toolkit. Many other aligners are based on one of these two toolkits. I'll briefly walk through how to use them from data preparation and installation to post-aligning processing, pooling relevant online resources (instead of reinventing the wheels) and adding in some of my own snippets of code. 

`{{< icon name="terminal" pack="fas" >}} Terminal` `{{< icon name="python" pack="fab" >}} Python`

## General procedure
1. Prepare the `.wav` files
2. Prepare the transcript files (`.txt`/`.lab`/`.TextGrid`)
3. Obtain a pronunciation dictionary with canonical phonetic transcription for words/characters
4. Run the aligner with pre-trained acoustic models

## Post-alignment options
5. Convert .Textgrid files into readable table format with temporal information

Here I basically describe how I managed to acquire automatic time-aligned .Textgrids using open-source softwares and tools on my Mac (*Mojave 10.14.6*) in details. I will first introduce how to prepare input data including `.wav` files and transcript files and then how to work with the Penn Forced Aligner and Montreal Forced Aligner respectively. I'm trying my best to be clear and hope this is helpful for those who want to achieve similar goals, especially for non-programmers and linguistic students.


>**DISCLAIMER**
>
>Feel free to leave a comment if you have a question or issue, but I'm probably unable to offer personal assistance to your problems (I'm in the middle of my dissertation). In short, this website is not responsible for any troubles.
>**Good luck!**
