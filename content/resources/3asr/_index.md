---
# Course title, summary, and position.
linktitle: Utilising ASR in Linguistic Research
summary: A step-by-step guide to apply automatic speech recognition technology in linguistic research. #<i class="fas fa-terminal"></i> Unix Shell <i class="fab fa-python"></i> Python Sox
weight: 1

# Page metadata.
title: Utilising ASR in Linguistic Research
date: "2023-06-20T12:00:00Z"
lastmod: "2023-06-21T12:00:00Z"
tags: ["speech processing", "ASR", "speech to text", "speech recognition"]
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
reading_time: true # does not work here?
share: true # does not work here?
profile: true # does not work here?
editable: true # does not work here?

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  speechrecognition:
    name: Speech Recognition
    weight: 1
---

Automatic Speech Recognition (ASR), or Speech-to-text (TTS), maps a sequence of audio inputs to a sequence of text outputs. Not only is it the core in applications such as voice assistants, video captioning, and minutes-taking, but it can also facilitate linguistic fieldwork and speech data preprocessing.  

There are many open-source resources that can empower us to integrate ASR into our linguistic research workflows. This tutorial aims to help you understand the basic concepts in ASR and guide you step-by-step to utilise ASR in your own linguistic research. 

The tutorial starts with employing state-of-the-art pre-trained ASR models to generate transcripts for audio recordings: [1. Applying large pre-trained models](https://chenzixu.rbind.io/resources/3asr/sr1/). The subsequent chapters will release soon and cover how to fine tune and train ASR models from scratch using PyTorch. Please stay tuned!

<!---
## Classical ASR architecture
1. Feature Extraction
2. Acoustic Model
3. Language Model
4. Decoding

## End-to-end attention-based ASR architecture
1. Speech Augmentation
2. Feature Extraction
3. Speech Recognizer
4. Beamsearch
--->

> **Forced-alignment tutorial**
>
> Automatic Forced Alignment is also based on ASR models. If you don't know how to use a forced aligner, please check out my another [online tutorial](https://chenzixu.rbind.io/resources/1forcedalignment/) about how to get time-aligned transcription files automatically.
>

The main audience expected for this online tutorial is linguistic researchers and students. All scripts were tested on my MacBook Air (M1 2020). Please click on the chapters in the Table of Contents (left side) to [**START**](https://chenzixu.rbind.io/resources/3asr/sr1/).

`{{< icon name="terminal" pack="fas" >}} Unix Shell` `{{< icon name="python" pack="fab" >}} Python` `SoX`


>**DISCLAIMER**
>
>Feel free to leave a comment if you have a question or issue by emailing me, but I'm probably unable to offer personal assistance to your problems. In short, this website is not responsible for any troubles.
>**Good luck!**

Thank you for reading. Feel free to share this tutorial! {{< icon name="facebook" pack="fab" >}} {{< icon name="twitter" pack="fab" >}} {{< icon name="whatsapp" pack="fab" >}} {{< icon name="weibo" pack="fab" >}} {{< icon name="weixin" pack="fab" >}}