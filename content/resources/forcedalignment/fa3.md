---
title: Use Penn Forced Aligner (P2FA)
linktitle: 3.P2FA
toc: true
type: docs
date: "2020-04-19T00:00:00+01:00"
draft: false
menu:
  forcedalignment:
    parent: Forced Alignment
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

## 3.1 Installation
The Penn Forced Aligner (P2FA) can be downloaded from [here](https://web.sas.upenn.edu/phonetics-lab/facilities/). They have a version for American English and one for Mandarin Chinese, though there's only scant documentation. The installation of P2FA can be a bit of hassle. Fortunately we have detailed instructions:

- For Mac users: Check [Will Styler's post](http://wstyler.ucsd.edu/posts/p2fa_mac.html). The installation is the same for English and Mandarin versions.

- For Windows users: Check [Cong Zhang's post](https://congzhanglinguist.wordpress.com/2018/09/03/p2fa_chinese_2/).

I rewrite a bit of the python code for the Mandarin version and make it a python 3 script since python 2 is getting outdated. If you *have installed* it successfully, you can check my Github repository [P2FA_Mandarin_py3](https://github.com/chenchenzi/P2FA_Mandarin_py3/tree/master) for an enhanced Python 3 script for **Mandarin** alignment.

There is also [FAVE](https://github.com/JoFrhwld/FAVE), a up-to-date implementation of the P2FA with pre-trained acoustic models of **American English**.

## 3.2 Pronunciation Dictionary
Before running the aligner, we need to make sure that the pronunciation dictionary `/P2FA_Mandarin/run/model/dict` contains all the characters appeared in our transcripts. Again, Bash Shell commands can help us with that (idea from here, adapted for Chinese orthography).
First we obtain a wordlist from our transcripts. Continuing with the above example `list.txt`, we make a copy of it, and in Terminal we navigate to this directory.
```
$ tr ' ' '\n' < list.txt|sort|uniq -c|sed 's/^ *//'|sort -r -n > wordlist.txt
```
{{% alert note %}}
**Tip**: No space in front of sort! Otherwise you might get the error message: "Command not found", since Bash is sensitive to spaces when you're piping.
{{% /alert %}}

This command generates a `wordlist.txt` file in which each unique Chinese character is lining up as a single column. The command also gives you the corresponding frequency count of each character in the first column. Then we want to compare it against the dictionary. We can also make a copy of the dict file `dict copy` (so that we don't ruin it by accident). If the character in `wordlist.txt` is also in the dictionary, then the corresponding dictionary line is extracted.
```
$ cut -d ' ' -f 2 wordlist.txt | sed 's/^/^/'| sed 's/$/ /' >tmp.txt 
```
(`-d ' '`: this flag specifies the delimiter is a space. Put the column of characters into regular expression format for locating the beginning of a line(`^`))
```
$ egrep --file=tmp.txt dict\ copy > words_phones.txt
```

There are some duplicated rows in the dict file. So we could do the following:
```
$ words_phones.txt|uniq -c|sed 's/^ *//' >words_phones2.txt
```
The idea is to sort the Chinese characters the same way in `wordlist.txt` and `words_phones2.txt` so that we can use the join command to see the record(s) that do not match.
```
$ sort -k 2 wordlist.txt >tmp1.txt
```
The problem is that even if we sort the column of characters in the grepped dict file `words_phones2.txt`, the sorting result is influenced by the third field of letters. So we decided to extract only the column of the Chinese characters of `words_phones2.txt` and sort it.
```
$ awk '{print $2}' words_phones2.txt|sort> tmp2.txt
```
Then we find out whether there are any characters in `tmp1.txt` but missing in `tmp2.txt`:
```
$ join -v 1 -1 2 -2 1 tmp1.txt tmp2.txt >missingwords.txt
```
(`-v 1`: this flag displays the non-matching records of the file 1. The following `-1 2 -2 1`: file 1 second column or field; file 2 first column)
This `missingwords.txt` lists the missing Chinese characters and you can manually add them to the original `dict` file in the `/model`.

## 3.3 Running P2FA
Running P2FA is easy when you have all the input files prepared as required. Here is a checklist:

- [x] All `.wav` files are in 16KHz, 16-bit, mono channel
- [x] Each `.wav` file has a `.txt`transcript file with a matching filename
- [x] The pronunciation dictionary in the P2FA model has been updated
- [x] All the files has been put in the same directory `/P2FA_Mandarin/run`

You just need one single line in the Terminal calling the `Calign2textgrid.py` and filling in relevant arguments: `.wav` file path, `.txt` file path, (output) `.Textgrid` file path. This script returns the short form `.Textgrid` file.
If you want to run the aligner for all of the audio files in a directory, you can make use of a loop structure:
```
$ for i in *.wav; do python Calign2textgrid.py $i $i.txt $i.TextGrid; done
```
In the Github repository `/P2FA_Mandarin` there's also `Calign2mlf.py`, which returns the output in `.mlf` with table-like form, as shown in the following example:
```
#!MLF!#
"/tmp/xuchenzi_27944.rec"
0 8500000 sp 3079.143311 sp
8500000 8800000 n -1.651408 你
8800000 9700000 i 73.151802
```
I also made a Python script `mlf2textgrid.py` to convert files in `.mlf` to `.Textgrid` (short form).