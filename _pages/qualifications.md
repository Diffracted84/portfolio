---
title: My Qualifications
layout: single
permalink: /qualifications/
#collection: qualifications

author: Professional
author_profile: true

classes: wide
show_excerpts: false
entries_layout: list
sort_by: index
sort_order: forward

header:
  #overlay_filter: rgba(0,0,0,0.07)
  overlay_image: /assets/images/qualifications/header.jpg
  alt: Abstract pattern background image
  caption: Image by [edith lüthi](https://pixabay.com/users/eluela31-4894494/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2681039) from [Pixabay](https://pixabay.com)
---
## University Level

<span style="font-size: 0.7em;">University qualifications</span>

<div class="grid__wrapper">
  <style>
    .grid__wrapper .archive__item-teaser {
      width: 100% !important;
      display: flex !important;
      justify-content: center !important;
      align-items: center !important;
      height: auto !important;
      max-height: 140px !important;
    }
    .grid__wrapper .archive__item-teaser img{
      max-width: 100% !important;
      max-height: 140px !important;
      object-fit: contain !important;
    }
    .grid__wrapper .archive__item-excerpt {
      width: 100% !important;
      align-self: stretch !important;
    }
  </style>
  {% for post in site.university %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
</div>

## Diploma Level

<span style="font-size: 0.7em;">Diplomas and other higher qualifications</span>

<div class="grid__wrapper">
  <style>
    .grid__wrapper .archive__item-teaser {
      width: 100% !important;
      display: flex !important;
      justify-content: center !important;
      align-items: center !important;
      height: auto !important;
      max-height: 140px !important;
    }
    .grid__wrapper .archive__item-teaser img{
      max-width: 100% !important;
      max-height: 140px !important;
      object-fit: contain !important;
    }
    .grid__wrapper .archive__item-excerpt {
      width: 100% !important;
      align-self: stretch !important;
    }
  </style>
  {% for post in site.diplomas %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
</div>

## Certificates

<span style="font-size: 0.7em;">Industry certifications</span>

<div class="grid__wrapper">
  <style>
    .grid__wrapper .archive__item-teaser {
      width: 100% !important;
      display: flex !important;
      justify-content: center !important;
      align-items: center !important;
      height: auto !important;
      max-height: 140px !important;
    }
    .grid__wrapper .archive__item-teaser img{
      max-width: 100% !important;
      max-height: 140px !important;
      object-fit: contain !important;
    }
    .grid__wrapper .archive__item-excerpt {
      width: 100% !important;
      align-self: stretch !important;
    }
  </style>
  {% for post in site.certificates %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
</div>

## Credentials

<span style="font-size: 0.7em;">Micro-credentials and other qualifications</span>

<div class="grid__wrapper">
  <style>
    .grid__wrapper .archive__item-teaser {
      width: 100% !important;
      display: flex !important;
      justify-content: center !important;
      align-items: center !important;
      height: auto !important;
      max-height: 140px !important;
    }
    .grid__wrapper .archive__item-teaser img{
      max-width: 100% !important;
      max-height: 140px !important;
      object-fit: contain !important;
    }
    .grid__wrapper .archive__item-excerpt {
      width: 100% !important;
      max-width: none !important;
    }
  </style>
  {% for post in site.credentials %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
</div>