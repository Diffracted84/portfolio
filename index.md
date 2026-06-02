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
  - image_path: /assets/images/splash/splash-about.jpg
    alt: "Image of a lego person at a desk."
    title: "About"
    caption: Image by [kirill_makes_pics](https://pixabay.com/users/kirill_makes_pics-5203613/) from [Pixabay](https://pixabay.com/)
    excerpt: "My work history is just a click away."
    url: /about
    btn_label: "Find Out More"
    btn_class: "btn--inverse"
  - image_path: /assets/images/splash/splash-project.jpg
    alt: "Image of lego people constructing."
    title: "Projects"
    caption: Image by [kirill_makes_pics](https://pixabay.com/users/kirill_makes_pics-5203613/) from [Pixabay](https://pixabay.com/)
    excerpt: "Look at the projects I have been involved with."
    url: /projects
    btn_label: "Read More"
    btn_class: "btn--inverse"
  - image_path: /assets/images/splash/splash-qualification.jpg
    alt: "Image of a lego person sitting on a book."
    title: "Qualifications"
    caption: Image by [facebook827](https://pixabay.com/users/facebook827-3332528/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4924237) from [Pixabay](https://pixabay.com/)
    excerpt: "Dive into my education and qualifications."
    url: /qualifications
    btn_label: "Investigate"
    btn_class: "btn--inverse"
  #- image_path: /assets/images/splash/splash-aviation.jpg
  #  alt: "Image of a lego person sitting on a book."
  #  caption: Image by [Robert](https://pixabay.com/users/s2dent-208425/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=309342) from [Pixabay](<a href=")https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=309342)
  #  title: "Aviation"
  #  excerpt: "An overview of my aviation record."
  #  url: /aviation
  #  btn_label: "Check It Out"
  #  btn_class: "btn--inverse"
---
    {% include feature_row id="intro" type="center" %}
    {% include feature_row %}