---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: splash
author_profile: false

header:
  overlay_color: "#000"
  overlay_filter: "0.0"
  overlay_image: /assets/images/splash-header.jpg
  actions:
    - label: "Download CV"
      url: /files/
  caption: "Image by [Axel Sifuentes](https://pixabay.com/users/axelsteven2000-20170836/) from [Pixabay](https://pixabay.com)"
  excerpt: "Welcome to my Portfolio Site."

intro:
  - excerpt: "Please select from the following:"

feature_row:
  - image_path: /assets/images/splash-about.jpg
    alt: "Image of a lego person at a desk."
    title: "About"
    excerpt: "My work history is just a click away."
    url: /about
    btn_label: "Find Out More"
    btn_class: "btn--inverse"
  - image_path: /assets/images/splash-project.jpg
    alt: "Image of lego people constructing."
    title: "Projects"
    excerpt: "Look at the projects I have been involved with."
    url: /projects
    btn_label: "Read More"
    btn_class: "btn--inverse"
  - image_path: /assets/images/splash-qualification.jpg
    alt: "Image of a lego person sitting on a book."
    title: "Qualifications"
    excerpt: "Dive into my education and qualifications."
    url: /credentials
    btn_label: "Investigate"
    btn_class: "btn--inverse"
---
    {% include feature_row id="intro" type="center" %}
    {% include feature_row %}

Images by [kirill_makes_pics](https://pixabay.com/users/kirill_makes_pics-5203613/) from [Pixabay](https://pixabay.com/)