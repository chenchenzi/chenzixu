---
title: Create audio-trimming scripts
linktitle: 3. Creating trim scripts
toc: true
type: docs
date: "2021-12-15T00:00:00+01:00"
draft: false
menu:
  speechcorpus:
    parent: Speech Corpus Query
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

Now that we have picked out the relevant syllables from the assembled text file, the third step is to create a trim script to cut the audio files by employing the information in the text file.

## SoX for audio trimming
Here we make use of `SoX` (Sound eXchange) to achieve the audio extraction. SoX is a collection of handy sound processing utilities. You can download it [here](http://sox.sourceforge.net/). 

The syntax for trimming audio file is:
```
$ sox input_path output_path trim starting_time(s) =ending_time(s)
```
We will create a audio trimming script that extracts all the targeted phrases in the corpus in one go. Thus we need to arrange the information according to the sox syntax. This can be done in the Terminal or Unix shell, using the Awk command again.

### Demo task

Let's continue to work on the task of extracting all Mandarin phrases in which the second syllable is the functional particle "*de*" 的. Imagine we put the trim script in the directory `/audios` (Pwd: working directory), and we create a new output directory for the extracted audio intervals inside the directory of `/speech_corpus`, i.e. `../audios_de/`.

```
cat de_phrases.txt | awk '{print "sox " $5 ".wav ../audios/" $5 $1 $6 "s" $7 "e" $8 ".wav trim " $2 " =" $8}' > de_trim.txt
```
Here `$i` means the ith number of field in a line or the ith column in a line.They don't need quotation marks, while other strings need to be wrapped in quotation marks. `$5` is the filename in `de_phrases.txt`. Thus the input file path is `$5 ".wav"`. We adopted a rather complicated output filename (path), i.e. `../audios_de/" $5 $1 $6 "s" $7 "e" $8 ".wav"`, which contains the audio filename, the characters, the starting, and ending times of the syllable *de* 的. It is information-rich and easy to create. Of course, you can have many other options for naming the output audio intervals. `$2` is the starting time in second for the first syllable and `$8` is the ending time in second for the *de* syllable.

The output trim script `de_trim.txt` is shown below. Let's put it in the directory of audio files `/audios`.

```
sox b08_1_114a.wav ../audios_de/b08_1_114a大的s0.1225e0.2060.wav trim 0.0125 =0.2060
sox b02_1_119q.wav ../audios_de/b02_1_119q搭的s0.3725e0.4638.wav trim 0.2351 =0.4638
sox b03_3_118a.wav ../audios_de/b03_3_118a回的s0.2397e0.3125.wav trim 0.1015 =0.3125
...
```

To execute the trim scipt, we do:
```
chmod +x de_trim.txt
./de_trim.txt
```
The command `chmod +x` enables the trim script to be executable. Then we run the trim script in the Terminal. In the output directory `/audios_de`, we should be able to see the extracted speech intervals.

If you could successfully follow this demo task and "play with" some of your own data, you have already mastered the skill of making speech corpus queries. Hooray!
More acoustic measurements and further analyses can be based on these extracted speech intervals.