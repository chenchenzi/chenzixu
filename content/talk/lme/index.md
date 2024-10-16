---
title: Linear Mixed Effects Models in R
event: Lab Skills Workshop
event_url: 

location: First Floor Lecture Room 1
address: 
  street: 47 Wellington square street
  city: Oxford
  region: 
  postcode: 'OX1 2ER'
  country: United Kingdom

summary: Lab Skills Workshop Week 5 Hilary Term. This workshop introduces you to linear mixed effects models in R. It is decidedly conceptual without too much mathematical proofs or equations involved. We will focus on understanding the model through analysing two datasets and building up linear modeling workflow in R.
abstract: "This workshop introduces you to linear mixed effects models in R. It is decidedly conceptual without too much mathematical proofs or equations involved. We will focus on understanding the model through analysing two datasets and building up linear modeling workflow in R."

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: "2022-02-18T11:00:00Z"
# date_end: "2030-06-01T15:00:00Z"
all_day: false

# Schedule page publish date (NOT talk date).
publishDate: "2022-01-14T00:00:00Z"

authors: [Chenzi Xu]
tags: [stats, Phonetics, lab skills, academic, mixed models, R, statistics, Phonlab]

# Is this a featured talk? (true/false)
featured: false

image:
  caption: ''
  focal_point: 

links:
- icon: desktop
  icon_pack: fas
  name: Slides
  url: https://chenzixu.rbind.io/slides/lme/lmer.html
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides: "example"
# slides: content/slides/praat/praat.html

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
#projects:
#- internal-project

# Enable math on this page?
math: false
---

## Pre-workshop Setup

- Download and install the latest version of [R](https://www.r-project.org/).
- Download and install [RStudio](https://www.rstudio.com/). RStudio is an application (an integrated development environment or IDE) that facilitates the use of R and offers a number of nice additional features. You will need the free Desktop version for your computer.
- Install the following packages. Copy the following line of code and paste it in the Rstudio Console, and press `return` to run the code. 
```
install.packages(c("tidyverse", "lme4", "lmerTest"))
```
- Import the shortened version of dataset in Winter and Grawunder (2012). Copy the following line of code and paste it in the Rstudio Console, and press `return` to run the code. 
```
data = read.csv("http://www.bodowinter.com/tutorial/politeness_data.csv")
```
<br/>

## Prerequisites

The workshop will be easier for you if...

{{< icon name="r-project" pack="fab" >}} You have basic knowledge about R and R studio.

{{< icon name="chart-bar" pack="fas" >}} You have basic inferential statistical knowledge.

<br/>

## Recommended Reading

Winter, B. (2019). Statistics for Linguists: An Introduction Using R (1st ed.). Routledge. https://doi.org/10.4324/9781315165547

Shravan Vasishth, Daniel Schad, Audrey Bürki, Reinhold Kliegl. (on going) Linear Mixed Models in Linguistics and Psychology: A Comprehensive Introduction. [https://vasishth.github.io/Freq_CogSci/](https://vasishth.github.io/Freq_CogSci/)

Barr, Dale J. (2021). Learning statistical models through simulation in R: An interactive textbook. Version 1.0.0. Retrieved from https://psyteachr.github.io/stat-models-v1.

McElreath, R. (2020). Statistical Rethinking: A Bayesian course with examples in R and STAN. CRC Press.