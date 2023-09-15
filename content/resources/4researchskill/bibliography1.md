---
title: Let's talk about reference management
linktitle: 1. Let's talk about reference management
toc: true
type: docs
date: "2023-6-20T00:00:00+01:00"
draft: false
menu:
  researchhacks:
    parent: Research Hacks
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

<br>

When you write an academic article or dissertation, how do you cite other sources? Have you ever used the following approaches to create your bibliography or reference list?

**A. Copy and Paste** approach: You copy citations from Google Scholar {{< icon name="quote-right" pack="fas" >}}, or existing publications {{< icon name="newspaper" pack="far" >}};

**B. Ad hoc Referencing Generator** approach:  You use a (mostly browser-based) Referencing Generator freely available online or provided by a university to automatically generate formatted academic references in a certain style. Or you use the basic referencing function in Word and add new sources by manually filling the boxes. {{< icon name="laptop-code" pack="fas" >}}

**C. Manual Typing** approach: You are probably a fast-typer and know the rules of citation style by heart. You create the reference texts and adjust the formatting from scratch. {{< icon name="keyboard" pack="fas" >}}

You will greatly benefit from this tutorial if (1) you are still using one of the ways described above to create your bibliography, (2) you want to have a head start on reference management, or (3) you have never tried [Zotero](https://www.zotero.org/), a free reference management software (this is not an advertisement XD).

This guide will show you the importance of structured citations, introduce you to one of the best tools, and provide strategies to optimise your research pipeline. (Feel free to jump to [Section 1.4](#zotero-setup) if you are already convinced to try Zotero.)


<br>

## 1.1 Why do we cite?

Briefly speaking, citing sources is a win-win practice for both you and the original author(s) who produced the sources. On their end, citations give their work credit and may reward them with career advancement (in the same line of thinking, you would want others to cite your original ideas or creation too). On your end, with citations you engage in academic conversations, contextualise your topic, and establish your credibility. Most importantly, you **avoid plagiarism**, which can have serious ethical and professional consequences.

<br>

## 1.2 What is reference management?

The core of reference management is to (1) **collect**, (2) **organise**, and (3) **format** bibliographic citations. Effective reference management is conducive to seamless academic writing and research, and a good reference management software is the unsung hero of academic and professional research. 

Let's first look at the **drawbacks** of the approaches I mentioned at the beginning.

**A. Consistency and Accuracy** 

Is the citation style consistent? Are the citations accurate? Copy and paste may be a quick option in the short term, but the web is inundated with false information. Often the citation options provided by Google Scholar do not strictly follow the latest guide of a citation style. Google Scholar also provides you multiple, sometimes **inconsistent** or **inaccurate**, options.
Below are some Google Scholar citation options of a seminal paper with tracked 88761 references (number based on first search result, as of September 13, 2023 23:00 BST).

>**MLA** Vaswani, Ashish, et al. "Attention is all you need." *Advances in neural information processing systems* 30 (2017).
>
>**APA** Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. *Advances in neural information processing systems, 30*.

Here is another APA option from the second search result (as of September 13, 2023 23:00 BST). We can see that the first author surname `Vaswani` was wrongly spelt.
>**APA** Waswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A., ... & Polosukhin, I. (2017). Attention is all you need. In NIPS.

This paper in fact was from the 31st International Conference on Neural Information Processing Systems (NIPS) in 2017. Here is another APA option with relatively good APA conference paper format, having skimmed many Google Scholar search results.
> **APA** Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017, December). Attention is all you need. In *Proceedings of the 31st International Conference on Neural Information Processing Systems* (pp. 6000-6010).

**B. Information Input and Re-use** 

It is extremely **time-consuming** and painfully **tedious** to manually type your reference list or fill in the boxes of a reference generator. We should probably avoid this as much as possible and divert our efforts on more important things. Ad hoc reference generators may be good for one-time formatting, but not sustainable nor efficient for the long run. If you are going to cite the same paper in many of your writing pieces, you do not want to repeat the same procedure again and again.

**C. Style Shifting?** 

Popular citation styles include `APA`, `MLA`, `Chicago`, `IEEE`, `Harvard`, etc. If a journal or conference requires a different citation style, it would be an extensive rework if you manually curated the bibliography or copied each source from elsewhere.

A reference management software {{< icon name="folder-open" pack="fas" >}} is essentially a platform for building **personalised, searchable databases** of source metadata, with capability to output formatted references. The database component records the accumulation of knowledge or information, and enables easy access for previously marked or cited works. **Automating** the process of generating formatted references minimises human error, enables consistent and replicable output, and greatly increases the efficiency.

<br>

## 1.3 Reference management software **TIER LIST**

Over the past years, I have garnered some experience in various reference management software. The figure below shows my reference management software tier list. The **S** tier (the best) software is **Zotero**, A tier Mendeley, and B tier EndNote, Paperpile, and RefWorks. 

All the software on the B tier is paywalled (which is why they stopped at the B tier despite their respective strengths), although they may be freely available if you are a student or researcher of a partner university. But what if you graduate or leave the university? You might not want to lose your carefully-built research databases, unless you start to purchase a subscription (potentially forever)!

{{< figure library="true" src="biblio.png" title="Tier List of Reference Management Software (personal opinion)" >}}

**Disclaimer**: The rankings are based on my subjective user experience and asynchronous comparisons, and some software may have already improved over time.

Both Mendeley and Zotero are **FREE**, but with charges for adding additional cloud storage space. Cloud storage is mandatory for Mendeley, but optional for Zotero. There is a workaround in Zotero to avoid the additional cloud storage charges (I will introduce this in a later section). Despite the support for Macintosh, Windows, and Linux system, Mendeley's Mac desktop client was not the best -- it crashed or froze many times on my MacBook Pro a few years ago. That was the time I switched to Zotero.

Zotero is **open-source** and has an active development community. Robust community support and contributions (e.g. plugins) make Zotero versatile and powerful. The main strengths of Zotero include:

- **Compatibility**: Zotero can work as a Firefox extension or as a standalone desktop version compatible with Mac, Windows, and Linux with a connector to all major web browsers.

- **Metadata scraping**: The browser plugin is excellent at importing metadata from a variety of sources ranging from traditional formats such as catalogue records, books, and articles to webpages and YouTube videos. 

{{% alert warning %}}
Always double-check the entries when possible to ensure complete details required by the citation rules have been imported.
{{% /alert %}}

- **Collaboration**: You can create unlimited private or public groups and share references and documents, which facilitates research collaboration and teamwork.

- **Language Support**: "Zotero's Unicode support allows you to import, store, and cite items in any language. There is an unofficial multilingual version of Zotero, [Juris-M](https://juris-m.github.io/), which supports storage of item metadata in more than one language (transliterations and translations)." More specifically for Chinese literature, there is a plugin, [Jasminum](https://github.com/l0o0/jasminum), which enhances Chinese metadata retrieval. There is also translation plugin.

- **Note-taking**: You can directly annotate PDFs in Zotero and take notes as you read.

- **Writing integration**: Zotero works with Word (sometimes a bit slow), LibreOffice and Google Docs. It also works with LaTeX through BibTeX.

- **Multi-device Sync**: By syncing your Zotero account (a mobile site through zotero.org) and local versions, you can achieve syncing across multiple devices (e.g. your work laptop and personal laptop). There's an official iPhone and iPad app, and third-party Android apps (More info [here](https://www.zotero.org/support/mobile)). 

<br>

## 1.4 Recommended **Zotero** Setup{#zotero-setup}

The installation is straightforward if you follow the official [online guide](https://www.zotero.org/support/installation). ...to be continued.

### 1.4.1 Must-have Plug-ins

[**Zotero Connector**](https://www.zotero.org/download/connectors)

[**ZotFile**](http://zotfile.com/)


[**Better BibTex for Zotero**](https://retorque.re/zotero-better-bibtex/), if you are a Latex user.

**Zotero Word for Mac Integration**, if you are a Word user.

### 1.4.2 More Plug-ins



## 1.5 **Zotero** User Cases


The official Zotero guide is available [here](https://www.zotero.org/support/).

More coming soon...