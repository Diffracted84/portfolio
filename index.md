---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: splash
author_profile: false

header:
  overlay_color: "#000"
  overlay_filter: "0.05"
  overlay_image: /assets/images/splash/splash-header.jpg
  actions:
    - label: "Download CV"
      url: /files/
  caption: "Image by [Axel Sifuentes](https://pixabay.com/users/axelsteven2000-20170836/) from [Pixabay](https://pixabay.com)"
  excerpt: "Welcome to my Portfolio Site."

intro:
  - excerpt: "Please select from the following:"

feature_row:
  - image_path: /assets/images/splash/splash-about.png
    alt: "Image of a man working at a desk."
    title: "About"
    excerpt: "My work history is just a click away."
    url: /about/
    btn_label: "Find Out More"
    btn_class: "btn--danger"
  - image_path: /assets/images/splash/splash-project.png
    alt: "Image of man planting a tree."
    title: "Projects"
    excerpt: "Look at my side projects."
    url: /projects/
    btn_label: "Read More"
    btn_class: "btn--success"
  - image_path: /assets/images/splash/splash-qualification.png
    alt: "Image of man graduating university."
    title: "Qualifications"
    excerpt: "Coming Soon" #"Dive into my education and qualifications."
  #  url: /qualifications/
  #  btn_label: "Investigate"
  #  btn_class: "btn--warning"
  #- image_path: /assets/images/splash/splash-aviation.png
  #  alt: "Image of a pilot with a plane flying overhead."
  #  title: "Aviation"
  #  excerpt: "An overview of my aviation record."
  #  url: /aviation/
  #  btn_label: "Check It Out"
  #  btn_class: "btn--info"
---
    {% include feature_row id="intro" type="center" %}
    {% include feature_row %}