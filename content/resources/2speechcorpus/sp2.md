---
title: Create query scripts
linktitle: 2. Creating query scripts
toc: true
type: docs
date: "2021-12-15T00:00:00+01:00"
draft: false
menu:
  speechcorpus:
    parent: Speech Corpus Query
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

The second step is to search in the assembled text file and to create subsets of the tabular data that only contains rows of desired speech units. There are many ways to search and filter a text file, and here I present two methods. They are illustrated through our demo task.

## Goal of demo task
Suppose that I hope to find all Mandarin phrases in which the second syllable is the functional particle "*de*" 的, which is commonly used as a possessive modifiers or nominaliser, in the corpus.

## Awk snippets

The first method to achieve our goal is to utilise Awk command in Unix and to create a small snippet of search code.

The Awk command enables us to define, search, and process text patterns. It scans a file line by line, splits each line into fields by whitespace character and stores them in the `$n` variables. It then compares the fields to pattern, and performs action(s) on matched lines.

I created a text file `find_de.txt` containing the following snippet of code. `$0` represents the entire current line, while `$1` represents the first field, which is the first item in a line. From the previous sample output, we know that the Chinese characters are in the first column in the text file. This snippet searches through the first column of the text file and if the character is *de* 的, it prints out the previous line and the current line on a row.

```
{if ($1 == "的") 
	{print prev, $0
	prev = $0}
else
prev = $0
}
```

Since Awk works with text files delimited by whitespace, we need to change our `.csv file` that is delimited by comma. This can be accomplished by using Sed command in Unix Shell replacing the comma with space. Then we execute the Awk command:

```
sed "s/,/ /g" words.csv > words.txt
awk -f find_de.txt words.txt > de_phrases.txt
```
The flag `-f` indicates that the awk command reads from the program file instead of from the first command line argument. The output is a text file that contains all the targeted phrases, as shown below.

```
大 0.0125 0.1225 0.1100 b08_1_114a 的 0.1225 0.2060 0.0835 b08_1_114a
搭 0.2351 0.3725 0.1374 b02_1_119q 的 0.3725 0.4638 0.0913 b02_1_119q
回 0.1015 0.2397 0.1383 b03_3_118a 的 0.2397 0.3125 0.0728 b03_3_118a
...
```

In the output text, we have the relevant syllables and their information on a line. This file will be our basis to create the trimming script for audio files.

## Python snippets

We can achieve the same results by writing a Python script.

```
# query.py C. Xu 2021.12.12
# This script is for extracting 2 syllable phrases in a text file
# in which we know the last syllable.

# -------------------------------------------

import sys

# -------------------------------------------

if len(sys.argv) < 2:
    print("Usage:", sys.argv[0], '<string> <filename>')
    exit()
s = sys.argv[1]
fname = sys.argv[2]

# -------------------------------------------

selection = ''
# we want to put all syllables in a phrase on one line.
# realines() retains the \n at the end for everyline;
# hence we are using splitlines() here
with open(fname, 'r') as f:
    lines = f.read().splitlines()
    for i, line in enumerate(lines):
        if str(s) in line:
            row = " ".join(lines[i-1:i+1]) + '\n'
            selection += row

with open(s + '.txt', 'w') as text:
    text.write(selection)

f.close()
text.close()

print("finished")
```

Both methods introduced above can be very flexible. You can modify them to make it suitable to your desired patterns. In my Github [repository](https://github.com/chenchenzi/textgrid2table), I included a few Python script that searches different text patterns. The `README.md` will take you from there.