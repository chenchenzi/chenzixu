---
title: Prepare a time-aligned transcription file
linktitle: 1. Time-aligned transcription
toc: true
type: docs
date: "2020-04-19T00:00:00+01:00"
draft: false
menu:
  speechcorpus:
    parent: Speech Corpus Query
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

Different aligners may have different requirements for audio input. Most work well with **16 KHz, 16-bit precision, and mono channel**, so it would be safe to format all the target `.wav` files like that. 

{{% alert warning %}}
The Penn Forced Aligner doesn't work with 24-bit.wav files (I stumbled on this for quite a while trying to debug). 
{{% /alert %}}

## 1.1 Praat
You can reformat your `.wav` files in Praat using its interactive graphical interface. To acquire a single channel, select the `sound > Convert > Convert to mono` or `Extract one channel`; to change the sampling rate, select the `sound > Convert > Resample`; then save it as a new `.wav`. The default is 16-bit in Praat.

## 1.2 SoX
Alternatively, you can also use `SoX` (Sound eXchange) commands in the Terminal. SoX is a collection of handy sound processing utilities. It is also required by P2FA. You can download it [here](http://sox.sourceforge.net/). To reformat the input.wav, we can use the following command in the Terminal having installed SoX.
```
$ sox input.wav -r 16k -b 16 -c 1 output.wav
```
The `$` isn't part of the command. It indicates that this is a Shell script in the Terminal. The flags here: `-r` , `-b`, `-c` define the sampling rate, precision, and number of channel of the output.wav respectively.