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
<style>
  .grid__wrapper {
    display: flex !important;
    flex-wrap: wrap !important;
  }

  .grid__wrapper .archive__item-teaser {
    width: 100%;
    display: flex !important;
    justify-content: left !important;
    align-items: left !important;
    height: auto !important;
    max-height: 140px !important;
  }
  .grid__wrapper .archive__item-teaser img{
    max-width: 75% !important;
    max-height: 140px !important;
    object-fit: contain !important;
  }
  .grid__wrapper .archive__item-excerpt {
    width: 100% !important;
    align-self: stretch !important;
  }

  .flex-break {
    clear: both !important;
    width: 100% !important;
    height: 0.1rem !important;
    padding: 0 !important;
    margin: 0 !important;
  }
</style>

## University Level

<span style="font-size: 0.7em;">University qualifications</span><br><br>
<span style="strong">Coming Soon</span>

<div class="grid__wrapper">
  {% for post in site.university %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
  <div class="flex-break"></div>
</div>

## Diploma Level

<span style="font-size: 0.7em;">Diplomas and other higher qualifications</span><br><br>
<span style="strong">Coming Soon</span>

<div class="grid__wrapper">
  {% for post in site.diplomas %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
  <div class="flex-break"></div>
</div>

## Certificates

<span style="font-size: 0.7em;">Industry certifications</span>

<div class="grid__wrapper">
  {% for post in site.certificates %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
  <div class="flex-break"></div>
</div>

## Credentials

<span style="font-size: 0.7em;">Micro-credentials and other qualifications</span>

<div class="grid__wrapper">
  {% for post in site.credentials %}
    {% include archive-single.html type="grid" asset_path=post.header.teaser %}
  {% endfor %}
</div>