---
# Course title, summary, and position.
linktitle: 2. Speech Corpus Querying
summary: A step-to-step guide to extract targeted speech intervals from your own speech corpus. #<i class="fas fa-terminal"></i> Unix Shell <i class="fab fa-python"></i> Python Sox
weight: 1

# Page metadata.
title: Speech Corpus Querying
date: "2021-12-14T12:00:00Z"
lastmod: "2021-12-14T12:00:00Z"
tags: ["speech processing", "Mandarin", "speech corpus", "speech corpus querying"]
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
  speechcorpus:
    name: Speech Corpus Query
    weight: 1
---


A speech corpus is usually a large collection of audio recordings of a spoken language, most often accompanied by text transcription files, and sometimes metadata documents about sources or background information of these files. 

There are many available, open source speech corpora such as TIMIT (English), [LibriSpeech](http://www.openslr.org/12/) (English ~1000 hours). Open speech corpora of other languages are also available, and many of them are fairly large, used for machine learning training. For text corpora, there are many free or commercial corpus analysis toolkits or software that provide a nice interface and powerful concordancer which allows to query corpora in an efficient manner, to generate frequency/n-grams for search tokens, and to automatically perform semantic or grammatical tagging. Such tools include [AntConc](https://www.laurenceanthony.net/software/antconc/) and [Sketch Engine](https://www.sketchengine.eu/) for example. For spoken corpora, however, there aren't many good options. [AIKUMA](http://www.aikuma.org/aikuma-app.html) may be a good choice for recording an audio source and adding translation and metadata; [ELAN](https://archive.mpi.nl/tla/elan) is a good annotation tool for audio and video recordings.

For linguists, we often conduct fieldwork and collect our own, first-hand language data by recording speakers and transcribing their speech. Depending on your linguistic research questions, we might also want to control our speech stimuli and speaker socio-cultural background instead of using large open source database without knowing much information about the speakers and recording contexts. Corpus building skill can be essential. This tutorial will introduce one way to compile a speech corpus and make queries of speech intervals, using the command-line interface.

When we build our own corpus, we will need audio files, and prepare corresponding time-aligned transcripts. The transcript file usually share the same filename with the audio file, except for a different file extention. It would be nice to have another metadata file to log some information about the speakers, recording equipments and environments, and audio file formats.

## General elements for speech corpora
 1. Speech audio files (`.wav` ) 
 2. Time-aligned transcript files (`.txt`/`.lab`/`.TextGrid`)
 3. Metadata files*

We prefer uncompressed audio formats such as WAV in research; sometimes you might encounter lossless compressed audio formats such as FLAC. Here I won't be covering how to record an audio file or how to get a transcription file (assuming that you already have them).


> **Forced-alignment tutorial**
>
> If you don't know how to use a forced aligner, please check out my another [tutorial](https://chenzixu.rbind.io/resources/1forcedalignment/) about how to get time-aligned transcription files automatically.


## General procedure for making a query
1. **Assemble** all time-aligned transcripts
2. Prepare a **query script** that search the text file for targeted sequences
3. Prepare **trim script** for cutting the portions out of relevant audios

In this tutorial, I'll briefly walk through how to **search and extract** syllables or phrases that are the focus or target of research from a speech corpus. All demonstration was tested on My Mac Book (*Big Sur 11.5.1*). Mandarin Chinese data will be used as an example. I'm trying my best to be clear and hope this is helpful for those who want to achieve similar goals, especially for non-programmers and linguistic students.

Courtesy of my supervisor, [Prof. John Coleman](http://www.phon.ox.ac.uk/coleman), who taught me how to query a speech corpus.

`{{< icon name="terminal" pack="fas" >}} Unix Shell` `{{< icon name="python" pack="fab" >}} Python` `SoX`


Click on the chapters in the Table of Contents to [**START**](https://chenzixu.rbind.io/resources/2speechcorpus/sp1/).

>**DISCLAIMER**
>
>Feel free to leave a comment if you have a question or issue by emailing me, but I'm probably unable to offer personal assistance to your problems (I'm in the middle of my dissertation). In short, this website is not responsible for any troubles.
>**Good luck!**

Feel free to share this tutorial! {{< icon name="facebook" pack="fab" >}} {{< icon name="twitter" pack="fab" >}} {{< icon name="whatsapp" pack="fab" >}} {{< icon name="weibo" pack="fab" >}} {{< icon name="weixin" pack="fab" >}}