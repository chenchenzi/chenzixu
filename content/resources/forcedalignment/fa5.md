---
title: Post-alignment Options
linktitle: 5. Post-alignment Options
author: ~
toc: true
type: docs
date: '2020-04-28T21:30:55+01:00'
draft: false
menu:
  forcedalignment:
    parent: Forced Alignment
    weight: 5

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5
---

## 5.1 Check the Alignment Results
When you have obtained the automatically time-aligned `.Textgrid` files using P2FA or MFA, it is always good to check them together with the corresponding `.wav` files. You can take a sample and open them in Praat, and listen to the aligned intervals to see if the alignment results are good. My corpus isn't very big, so I checked every single file. Occasionally you'll find some alignment errors, you can manully correct them but do keep a log of the changes!

## 5.2 Converting `.Textgrid` to Table Format
When you have a finalised set of `.Textgrid` files, you might want to extract the temporal information from the alignment. `.Textgrid` format, especially the long form, isn't very reader-friendly. So I wrote a few Python scripts to convert the `.Textgrid` to `.txt` or `.csv` with information presented in a more readable table format.