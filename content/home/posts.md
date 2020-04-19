+++
# A Recent Blog Posts section created with the Pages widget.
# This section displays recent blog posts from `content/post/`.

widget = "pages"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = false  # Activate this widget? true/false
weight = 60  # Order that this section will appear.

title = "Resources"
subtitle = ""

[content]
  # Page type to display. E.g. post, talk, or publication.
  page_type = "post"
  
  # Choose how much pages you would like to display (0 = all pages)
  count = 0
  
  # Choose how many pages you would like to offset by
  offset = 0

  # Page order. Descending (desc) or ascending (asc) date.
  order = "desc"

  # Filter posts by a taxonomy term.
  [content.filters]
    tag = ""
    category = ""
    publication_type = ""
    author = ""
    exclude_featured = false
  
[design]
  # Toggle between the various page layout types.
  #   1 = List
  #   2 = Compact
  #   3 = Card
  #   4 = Citation (publication only)
  view = 2
  
[design.background]
  # Apply a background color, gradient, or image.
  #   Uncomment (by removing `#`) an option to apply it.
  #   Choose a light or dark text color by setting `text_color_light`.
  #   Any HTML color name or Hex value is valid.
  
  # Background color.
  # color = "navy"
  
  # Background gradient.
  # gradient_start = "DeepSkyBlue"
  # gradient_end = "SkyBlue"
  
  # Background image.
  # image = "background.jpg"  # Name of image in `static/img/`.
  # image_darken = 0.6  # Darken the image? Range 0-1 where 0 is transparent and 1 is opaque.

  # Text color (true=light or false=dark).
  # text_color_light = true  
  
[advanced]
 # Custom CSS. 
 css_style = ""
 
 # CSS class.
 css_class = ""
+++

Acoustic analysis programmes such as Praat, ESPS, and Matlab have enabled the large-scale processing and analysis of speech data. For phoneticians, we often work with a corpus of audio files and begin with the preparation of the speech data involving reformatting, transcription, and segmentation. Such pre-processing steps made it convenient for us to focus, retrieve, and extract the target units of our research. Many tools with strong computational power for relatively big data are available now, which automates some processes and greatly boost our efficiency.

In the course of my DPhil, I have accumulated a bunch of scripts that greatly speeded up some processing procedures. Some are directly downloaded online from those who kindly shared their efforts, others are either adapted from others to suit my own purposes or my nascent attempts in a few programming languages. Dipping my toes in scripting was a steep learning curve but worthwhile. Here I would also like to share some useful scripts I had (and brief tutorials of how to use them), to pay it forward to the community where their generous sharing helped me a lot. This also serves as an archive of some of my analyses procedure, making the research open and **reproducible**.

I mainly use {{< icon name="terminal" pack="fas" >}} Terminal,
{{< icon name="python" pack="fab" >}} Python, {{< icon name="r-project" pack="fab" >}} R, and Praat in these posts. So it would make more sense to you if you have some very basic ideas about Shell scripts and Praat.

Prerequisites:
* Bash/Unix Shell Basics in the Terminal
* Basic familiarity with Praat.
* **Googling** or **stackoverflowing** your problems!

The following posts are the topics that are covered so far.
 