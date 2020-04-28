---
title: Prepare Transcripts files
linktitle: 2. Prepare Transcripts
toc: true
type: docs
date: "2020-04-19T00:00:00+01:00"
draft: false
menu:
  forcedalignment:
    parent: Forced Alignment
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

I'm using **Mandarin Chinese** here as an example. My goal here is to align each Chinese character (each syllable) with its corresponding sound interval. Thus I need to prepare transcript files in which there is a space between any two characters. Besides, there are many audio files in a corpus to be aligned. We would want to name all the transcript files accordingly (matching with the `.wav` filename) so that we can write a loop script to run an aligner for all of the `.wav` files at once. Since `.txt` files work for both aligners, I'll demonstrate how to prepare a `.txt` file (relatively efficiently) for each `.wav` file when you have assembled transcripts such as **pre-designed speech stimuli** in speech production experiments. One way to do this is:

## 2.1. Acquire a List of `.wav` Files
Put all the `.wav` files in a directory and open Terminal and navigate to that directory. In Terminal: 
```
$ ls *.wav >> list.txt 
```
We obtain a .txt file with each .wav filename being a row in one column. 

## 2.2. Add Corresponding Transcripts
Then we add in the corresponding transcript of the audio on the same row following the .wav filename with a space (i.e. we have the second column of transcript texts). 
For my project, I have pre-designed speech stimuli, so I can just copy them over and slightly modify them manually when speakers made any variation. An example of the updated list.txt file is:

```
1_1_101.wav 他们堆的雪人很稳当
1_2_101.wav 他们堆的城堡很稳当
1_3_101.wav 他们堆的台阶很稳当
```

## 2.3. Insert Spaces
Then we can make use of bash shell commands to insert a space between every character:
```
$ sed -e 's/./& /' list.txt
```
So now the list.txt is as follows:
```
1_1_101.wav 他 们 堆 的 雪 人 很 稳 当
1_2_101.wav 他 们 堆 的 城 堡 很 稳 当
1_3_101.wav 他 们 堆 的 台 阶 很 稳 当
```
## 2.4. Generate Individual `.txt` Files
We want the characters in each line to be an independent `.txt` file whose filename is the first field but with `.txt` extension. File-naming is very important because you need to think about the next step.

### 2.4.1 Filenaming for Penn Forced Aligner
To use P2FA for a group of files in a directory, I used a shell script calling the .py in a loop. So it would be easy to use the .wav filename (e.g. `1_1_101.wav`) as a common stem and a variable `$i` so that the corresponding `.txt` filename is `$i.txt` (e.g. `1_1_101.wav.txt`).
```
$ cat list.txt | while read line || [ -n "$line" ]; do echo $line | awk '{$1=""}1'| awk '{$1=$1}1' > $(cut -d " " -f1 <<< $line).txt; done < list.txt
```
The first awk here removes the first field and the second awk removes the leading space (by redefine the beginning as the first string).

### 2.4.2 Filenaming for Montreal Forced Aligner
To use the Montreal aligner we want the paired `.txt` and `.wav` to have exactly the same filename (e.g. `1_1_101.txt` and `1_1_101.wav`). An easier way is probably change the extensions in the first column in `list.txt` first to make them the filenames of our output `.txt` files before splitting it into individual files.
```
$ sed 's/\.wav/\.txt/' list.txt 
```
(you can also use `Find and Replace` in your text editor. **Do whichever is easier!**)
```
$ cat list.txt | while read line || [ -n "$line" ]; do echo $line | awk '{$1=""}1'| awk '{$1=$1}1' > $(cut -d " " -f1 <<< $line); done < list.txt
```
By doing so, our corresponding`.txt` files are ready.


There are many ways to achieve this depending on how you obtain your transcript texts. **How to obtain orthography transcripts efficiently for spontaneous speech?** I think of open-source speech-to-text APIs (e.g. Google's Speech-to-Text or Baidu's DeepSpeech). But there are often some restrictions. I may build a pipeline from speech-to-text to forced alignment in the near future and post it here.