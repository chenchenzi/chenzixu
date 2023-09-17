---
title: Let's talk about reference management and Zotero
linktitle: Let's talk about reference management and Zotero
toc: true
type: docs
date: "2023-9-10T00:00:00+01:00"
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

**B. Ad hoc Referencing Generator** approach:  You use a (mostly browser-based) referencing generator freely available online to automatically generate formatted academic references in a certain style. Or you use the basic referencing function in Word and add new sources by manually filling the boxes. {{< icon name="laptop-code" pack="fas" >}}

**C. Manual Typing** approach: You are probably a fast-typer and know the rules of citation style by heart. You create the reference texts and adjust the format from scratch. {{< icon name="keyboard" pack="fas" >}}

You will greatly benefit from this tutorial if (1) you are still using one of the ways described above to create your bibliography, (2) you want to have a head start on reference management, or (3) you have never tried [Zotero](https://www.zotero.org/), a free reference management software (this is not an advertisement XD).

This guide will show you the importance of structured citations, introduce you to one of the best tools, and provide strategies to optimise your research pipeline. Feel free to jump to [**Section 4 Recommended Zotero Setup**](#zotero-setup) if you are already convinced to try Zotero.

<br>

# 1 Why do We Cite?

Briefly speaking, citing sources is a win-win practice for both you and the original author(s) who produced the sources. On their end, citations give their work credit and may reward them with career advancement (in the same line of thinking, you would want others to cite your original ideas or creation too). On your end, with citations you engage in academic conversations, contextualise your topic, and establish your credibility. Most importantly, you **avoid plagiarism**, which can have serious ethical and professional consequences.

<br>

# 2 What is Reference Management?

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

**B. Information Importing and Re-use** 

It is extremely **time-consuming** and painfully **tedious** to manually type your reference list or fill in the boxes of a reference generator. We should probably avoid this as much as possible and divert our efforts on more important things. Ad hoc reference generators may be good for one-time formatting, but not sustainable nor efficient for the long run. If you are going to cite the same paper in many of your writing pieces, you do not want to repeat the same procedure again and again.

**C. Style Shifting?** 

Popular citation styles include `APA`, `MLA`, `Chicago`, `IEEE`, `Harvard`, etc. If a journal or conference requires a different citation style, it would be an extensive rework if you manually curated the bibliography or copied each source from elsewhere.

A reference management software {{< icon name="folder-open" pack="fas" >}} is essentially a platform for building **personalised, searchable databases** of source metadata, with capability to output formatted references. The database component records the accumulation of knowledge or information, and enables easy access for previously marked or cited works. **Automating** the process of generating formatted references minimises human error, enables consistent and replicable output, and greatly increases the efficiency.

<br>

# 3 Reference Management Software **TIER LIST**

Over the past years, I have garnered some experience in various reference management software. The figure below shows my reference management software tier list. The **S** tier (the best) software is **Zotero**, created at the Roy Rosenzweig Center for History and New Media at George Mason University. **A** tier contains Mendeley, by Elsevier, and **B** tier (from left to right) features EndNote (by Clarivate Analytics), Paperpile (by Paperpile LLC), and RefWorks (by ProQuest). 

{{< figure library="true" src="biblio.png" title="Tier List of Reference Management Software (personal opinion)" >}}

**Disclaimer**: The rankings are based on my subjective user experience and asynchronous comparisons, and some software may have already improved over time.

**Overview of Reference Management Software**

All software listed can fulfill the basic needs of reference management. Paperpile and RefWorks are rather lightweight and entirely web-based. Paperpile is specially well-integrated for Chrome and Google Apps including Google Drive and Docs. A limitation of these products is that offline support is limited (we sometimes work when there is no Wi-Fi access). Both Zotero and Mendeley are compatible with Windows, Mac, and Linux, with iPad, iPhone, and Android apps. EndNote is Windows and Mac only.

All the software on the B tier is **paywalled** (which is why they stopped at the B tier despite their respective strengths), although they may be freely available if you are a student or researcher of a partner university. But what if you graduate or leave the university? You might not want to lose your carefully-built research databases, unless you start to purchase a subscription (potentially forever) or to spend time migrating your collections and learning to use a new software! The university RefWorks subscription usually extends free access to alumni, but free access to EndNote is terminated beyond graduation from the university. 

Both Mendeley and Zotero are **FREE**, **but** with charges for adding additional cloud storage space. Cloud storage is mandatory for Mendeley, but optional for Zotero. There is a **workaround** in Zotero to avoid the additional cloud storage charges (I will introduce this in [Section 4.1.2](#zotfile)). Despite the support for Macintosh, Windows, and Linux system, Mendeley's Mac desktop client was not the best -- it crashed or froze many times on my MacBook Pro a few years ago. That was the time I switched to Zotero.

**Zotero's Strengths**

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

# 4 Recommended **Zotero** Setup{#zotero-setup}

The installation of Zotero is straightforward if you follow the official [online guide](https://www.zotero.org/support/installation). 

**Zotero (v.6.0.27) Interface**

The figure below shows an example Zotero interface on a Mac. In the left panel, you can organise your reference entries (**items**) into different **collections** (i.e. by theme, project, publication etc.) with various hierarchical structures in your **library** or **shared libraries**. Collections are like playlists in Spotify {{< icon name="spotify" pack="fab" >}}: items in collections are aliases (links) to a single copy of the item in the library. An item can be affiliated with multiple collections. 

In the middle panel, you can see all items in a selected collection or library. The type of an item is signaled by the mini icon preceding the title such as a book, book chapter, conference paper, dissertation, webpage, article, letter, software, artwork, audio recording etc. When you click on an item, its metadata information will be displayed in the right panel. An item can have notes, files, and links attached to it. By toggling the grey triangular arrow to the left of an item, attachments can be shown.

{{< figure library="true" src="zotero.png" title="Zotero Interface on a Mac (M1)" >}}

## 4.1 Must-have Plug-ins

Apart from downloading and installing the Zotero desktop app, plugins elevate Zotero to the next-level. The following plugins greatly enhance the user experience, which I considered as must-haves: **Zotero Connector** for reference importing, **ZotFile** for attachment (PDF) management, **Better BibTex** or **Word processor Plugin** for reference exporting.

### 4.1.1 Zotero Connector
[**Zotero Connector**](https://www.zotero.org/download/connectors) is the web browser plugin and available for Chrome, Firefox, or Safari, enabling automatically creating new items and populating the metadata fields using information available on the internet, with **a simple click**. A full-text PDF if available is downloaded together with the click.

{{% alert note %}}
You need to have access to / have subscribed the (non-open access) source databases (e.g. logged-in via university account) to be able to download the full-text DPF with the click.
{{% /alert %}}

The figure below illustrates the Zotero Connector on Chrome, the blue webpage icon. When you hover your mouse on the icon, you will see "Save to Zotero".If you click on this icon, this webpage will be added as an item in the current library and/or collection by default (the one you opened and displayed in the middle panel in your Zotero desktop). You can also select the destination folder in the dialog box `Saving to` popped up right after the click. You need to have Zotero desktop app running when using Zotero Connector.

The icon **changes** along with the potential source document type. Sometimes the webpage icon is grey when the webpages don't provide any information Zotero can recognise. On webpages such as Google Scholar that contain multiple potential sources, the icon will change into a folder {{< icon name="folder" pack="fas" >}}. By clicking the folder, a dialog box `Zotero Item Selector` will pop up from which source references can be selected and saved to Zotero (see the figure in [Section 4.2.2](#jasminum)). In this way you can import multiple items in one go. 

You can click the link provided above to download and install Zotero Connectors.

{{< figure library="true" src="chrome.png" title="Zotero Connector for Chrome" >}}

### 4.1.2 ZotFile{#zotfile}

[**ZotFile**](http://zotfile.com/) is a plugin for attachment management. When you download articles from the web, their filenames are likely to be a string of meaningless numbers and letters, which make the file unsearchable. ZotFile enables automatically renaming your files in a consistent manner using the metadata of the file. It can also automatically move and attach the PDFs to the entries. 

To **install** ZotFile, first click `Download` on the [ZotFile](http://zotfile.com/) website and you will acquire an extension file `zotfile-5.1.2-fx.xpi`. Then in Zotero software, go to `Tools > Add-ons`. In the Add-ons Manager, click the gear icon {{< icon name="gear" pack="fas" >}} on the top-right corner of the window, select `Install Add-on From File`, and navigate to the folder where the `.xpi` file is located (possibly in ` Downloads`) and click `Open` (or `install`). In this way, the ZotFile plugin is installed. Restart Zotero.

{{% alert note %}}
**Plugin Installation**: Most of the plugins below should follow the same procedure of installation as ZotFile (so I shall not repeat the description of installations of plugins). The general procedure:

①  Download the  `.xpi` file of the plugin from a website or Github page;

②  Go to `Tools > Add-ons`, and click gear icon `> Install Add-on From File` to install the `.xpi` file;

③  Restart Zotero.
{{% /alert %}}

To **set up** ZotFile, you can go to `Tools > ZotFile Preferences...`. You can set up the **customised PDF file renaming rules** and file storage path in these tabs. An example of the setup is shown below. 

In the `Renaming Rules` tab, I used the filename rule `{%F_}{%y_}{%t}` for PDFs, where:
- **%F** indicates author’s last name with first letter of first name (e.g. EinsteinA).
- **%y** indicates year (extracted from Date field)
- **%t** indicates title. Usually truncated after : . ? The maximal length of the remaining part of the title can be changed.

These three fields are connected in `_`. For more place holders in formatting filenames, check out the [online guide](http://zotfile.com/#renaming-rules).

{{< figure library="true" src="zotfile.png" title="ZotFile Settings: File Renaming Rules (left) and File Storage Path (right)" style="width: 10%">}}

In the `General Settings` tab, you can specify where you want to store your downloaded PDFs of articles and book chapters. If you store a copy of your PDFs in Zotero, they will be synced to the Zotero server and you can have access to them across devices. The drawback is that this fills up the free Cloud storage space (300 MB) pretty fast. 

A **workaround** to avoid payments is to use `Custom Location` here to move your attachment files (PDFs) to a custom folder and link to that location from Zotero. In the above example I stored neatly-renamed PDFs in a folder called `research_lib` on my laptop. Next, you go to `Zotero > Settings... (or Preferences...) > Advanced > Files and Folders`. In the section `Linked Attachment Base Directory` you can set up the Base directory using the storage path (where the PDFs are) you set up in ZotFile. In this case, it was `/Users/cx936/Documents/research_lib`. Zotero will use relative paths for linked files within the base directory. This means that as long as we have consistent filenames, we can have different base directory on different computers in order to access attachment files, given that the attachment folder, `research_lib` in this example, is copied over to a different machine or you use some Cloud space (e.g. Dropbox and Google Drive) as storage. 

To **use** ZotFile, select an item and right-click. Then in the menu, select `Manage Attachments > Rename and Move`, and you will find that the file is renamed, moved, and linked. You can select all files in a library or collection and right-click to bulk rename all of them (same command) in one go.


### 4.1.3  Better BibTex for Zotero

If you are a Latex / Markdown user, [**Better BibTex for Zotero**](https://retorque.re/zotero-better-bibtex/) facilitates managing bibliographic data. It generates unique citation keys, which are customisable to your liking. It also allows **auto-exports** so that the exported `.bib` file is automatically updated when you changed items in the corresponding collection or library.

To **export** a library or collection, right-click on it in the left Zotero panel and choose `Export Library...` or `Export Collection...`. In the pop=up menu, select one of the Better BibTex's export translators (all start with "Better"", e.g. `Better BibLaTex`). Checking the `Keep updated` option will register the export for automation, which is very helpful when you are revising your draft. After you have completed the current export, any changes to the collection or library will trigger an automatic re-export to update the export file. Click `OK` to start exporting. 

In `Zotero > Settings... (or Preferences...) > Better BibTex > Open Better BibTex preferences...`, you can configure the citation key formula (or just keep the default) and automatic export options. 

#### Linking Overleaf and Zotero

You can link your **Overleaf** account to Zotero, please following the [online instructions](https://www.overleaf.com/learn/how-to/How_to_link_your_Overleaf_account_to_Mendeley_and_Zotero).

### 4.1.4 Word Processor Plugin

If you are a Word user, [**Word Processor Plugins**](https://www.zotero.org/support/word_processor_integration) helps you **cite as you write**. The word processor plugins are bundled with Zotero and should be installed automatically for each supported word processor (i.e. Word, LibreOffice, Google Docs) on your computer when you first start Zotero.
You can check out Zotero integration with your favorite editor [here](https://retorque.re/zotero-better-bibtex/citing/cayw/). 

<br>

## 4.2 More Valuable Plug-ins (Optional)

My overall advice is not to install too many plugins, otherwise the desktop Zotero gets a bit clunky. Feel free to read about or try out the plugins I introduced below to see if you like them (I think they are very cool!): **Zotero Reference** for finding connected references, **Jasminum** for Chinese language support, **Zotero Better Notes** and **ZotCard** for taking notes, and **Zotero Style** for tracking reading progress and interface styling. You can scroll down or navigate using the table of contents to the right of your laptop screen.

{{% alert note %}}
**Zotero Trouble Shooting**: Restarting Zotero seems to solve 99% of the problems.
{{% /alert %}}

### 4.2.1 Zotero Reference

One of the most efficient ways to collect references revolving around a topic or a research area during literature review is to check out the reference list or works cited in a document published in that area. [**Zotero Reference**](https://github.com/MuiseDestiny/zotero-reference) automatically extracts all references in a document and provides a shortcut for adding the interested ones to your collection.

When you open a PDF document, you will notice there is a new tab named `References`. After clicking the `refresh` button, the works cited in this paper are extracted and listed here. When you hover your mouse on one of the listed references, a floating window will appear showing some metadata including the abstract of this reference. This is illustrated in the following screen capture. The small pink dots in the floating window represent four different data sources (click to switch). Clicking the `+` to the right of the reference adds it to your collection / library.

{{< figure library="true" src="reference.png" title="Adding References from a Reference List" >}}

To install, find the `.xpi` file via the provided link. More introduction is available in the Github page.

### 4.2.2 Jasminum{#jasminum}

[**Jasminum**](https://github.com/l0o0/jasminum) adds support for Chinese sources, especially the CNKI (China National Knowledge Infrastructure; 中国知网) database. To set up Jasminum, go to `Zotero > Settings... (or Preferences...) > Jasminum > Unofficial Translators Repository` and click `Update All`. You can occasionally go to `Zotero > Settings... (or Preferences...) > Advanced`, click `Update now` for `Automatically check for updated translators and styles` under the Miscellaneous section.

The figure below demonstrate bulk downloading themed Chinese articles from the CNKI database of journal articles.

{{< figure library="true" src="bulk.png" title="Bulk Save in CNKI Database" >}}

To install, find the `.xpi` file via the provided link. More introduction is available in the Github page.

### 4.2.3 Zotero Better Notes and ZotCard

[**Zotero Better Notes**](https://github.com/windingwind/zotero-better-notes) and [**ZotCard**](https://github.com/018/zotcard) are two plugins for note-taking in Zotero.

**Better Notes** works great if you want to jot down quick notes while reading. It allows linking different annotations and notes from different references inside Zotero, facilitating information organisation for a particular theme. Check out the Github page (link above) for its usage.

**Zotcard** provides customised card templates for note-taking and sorting the information. The figure below demonstrates a note with pre-defined sections themed on research methods and automatically populated metadata information. I can add notes relevant to the sections as I read along.

{{< figure library="true" src="notes.png" title="Note-taking Helper in Zotero" >}}

To set up a note card template, go to `Tools > zotCard Option > Config`. In the window zotCard Option, you can see some read-made cards, as well as `card1` to `card6`. The latter ones are empty cards which you can customise. Click on `card1` for example. You can change the card name by filling in the `Menu` box, keep `Enable` checked, write in the `Template` box (check out the ready-made cards for format), and finally `Save`. Since Zotero notes is HTML-based, some HTML foundation may be required to create a card. There are also many card templates shared among the community, available at the [Discussions](https://github.com/018/zotcard/discussions) in the Github page. The card I used in above figure can be created using the following code (modified from free521521521's [sharing](https://www.freezhao.com/zoterozotcard-template/)):

```html
<h1> <span>Method_Overview_${title}</span></h1>
<h2 style="color: #1B5E20; background-color:#F1F8E9;">💡 Meta Data </h2> 
<table>
  <tr>         
    <th style="background-color:#dbeedd;">
      <p style="text-align: right">Title </p>
    </th>         
    <td style="background-color:#dbeedd;">${title}</td>      
  </tr>
  <tr>
    <th style="background-color:#f3faf4;">
      <p style="text-align: right">Journal</p>
    </th>         
    <td style="background-color:#f3faf4;">${publicationTitle}&nbsp;<em>(<a href="https://doi.org/${DOI}">${DOI}</a>)</em></td>
  </tr> 
  <tr>
    <th style="background-color:#dbeedd;">
      <p style="text-align: right">Authors </p> 
    </th>
    <td style="background-color:#dbeedd;"> ${authors}</td>  
  </tr>
  <tr>
    <th style="background-color:#f3faf4;">
      <p style="text-align: right">Date </p>
    </th> 
    <td style="background-color:#f3faf4;">${date}</td>  
  </tr>
 </table>
<h2 style="color: #E65100; background-color: #FFF8E1;" >📜 Language Variety and Tone System</h2>
<p></p>
<h2 style="color: #2E7D32; background-color: #F1F8E9;" >🗣 Speaker Information</h2>
<p></p>
<h2 style="color: #1565C0; background-color: #E1F5FE;" >📋 Stimuli Information</h2>
<p></p>
<h2 style="color: #4A148C; background-color: #F5F5F5;" >🎙 Recording Information</h2>
<p></p>
<h2 style="color: #006064; background-color: #E0F7FA;" >💻 Acoustic Analysis</h2>
<p></p>
<hr/>
<h3 style="color: #1565C0"font-family: arial,sans-serif;>Other Notes:</h3>
<p></p>

```

To install, find the `.xpi` file via the provided links. More introduction is available in the Github pages.

### 4.2.4 Zotero Style

[**Zotero Style**](https://github.com/MuiseDestiny/zotero-style) provides personalised modification to the columns in the middle panel.

In the figure below, you see shades of pink in the `Title` column, which indicates the reading progress of a reference document -- it is the distribution of your reading time spent on each page of the PDF attached to an item. I also added an extra `Progress` column, which represents the annotation word count of each page of the PDF attached to an item. I enabled these setup to keep track of my literature reading. It is straightforward to see which ones I haven't read yet. 

There are more style-related options including changing colors and adding various tags and stars. You can also automatically track the Impact Factor and ranking for a publication, which may help you set your priority in literature review. Check out the Github page (above link) for more information. Don't spend too much time on the look:D

{{< figure library="true" src="style.png" title="Modified Zotero Columns to show Reading Progress" >}}

To install, find the `.xpi` file via the provided link. More introduction is available in the Github page.

<br>

## 4.3 GPT Meets Zotero...

There are infinite possibilities when we bridge ChatGPT and Zotero. 
You will need to purchase the OpenAI API to use [**Zotero GPT**](https://github.com/MuiseDestiny/zotero-gpt). I haven't fully explored this plugin yet. I will update this when I tested it out.

To install, find the `.xpi` file via the provided link. More introduction is available in the Github page (above link).

{{< figure library="true" src="gpt.png" title="Integrating ChatGPT with Zotero" >}}

# Concluding Remarks

The present guide is based on the official Zotero documentation available [here](https://www.zotero.org/support/). There are a lot more [plugins](https://www.zotero.org/support/plugins) in the Zotero community.

Effective reference management is the backbone of research and academic writing, and Zotero, with its ability to streamline the citation process, emerges as a valuable tool in this endeavor. Zotero may not be the easiest to set up to release its fullest potential. You might not need all of the (additional) functions, but that's the beauty of Zotero -- You can tailor it to suit your unique workflow and preferences. I hope this blog has been a useful and time-saving resource to help you on your path to academic excellence.







