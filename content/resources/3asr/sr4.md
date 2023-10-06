---
title: "ASR from Scratch II: Training models of Hong Kong Cantonese with MFA implementation"
linktitle: "4. ASR from Scratch II: Training models of Hong Kong Cantonese with MFA implementation"
toc: true
type: docs
date: "2023-6-20T00:00:00+01:00"
draft: false
menu:
  speechrecognition:
    parent: Speech Recognition
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---

<br> 

In the previous extensive chapter *ASR from Scratch I*, I have demonstrated how to train acoustic models of Hong Kong Cantonese using (source) Kaldi ASR. This chapter achieves the same goal with the help of the Montreal Forced Aligner (MFA), which is also based on Kaldi but a more streamlined process.

<br> 

## 4.1 MFA Installation

MFA is built on Conda Forge, so we will install MFA via Conda. Install [Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html) or [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/) first if you haven't done it.

Then in your Unix Shell, or Terminal on a Mac, create a new environment and install MFA:
```bash
conda create -n aligner -c conda-forge montreal-forced-aligner
conda activate aligner
```
For more information, see the official installation [guide](https://montreal-forced-aligner.readthedocs.io/en/latest/installation.html).

<br> 

## 4.2 The Common Voice Dataset

We will be using the same dataset as that in the previous chapter, the latest `Chinese (Hong Kong)` subset of the publicly available Common Voice corpus `Common Voice Corpus 15.0`. Please check out [**§ 3.3** in *ASR from Scratch I*](https://chenzixu.rbind.io/resources/3asr/sr3/#33-the-common-voice-dataset) for more details.

<br> 

## 4.3 Data Preprocessing

```


```