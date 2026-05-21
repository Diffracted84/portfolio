---  
title: "Projects"
layout: archive
permalink: /projects/
collection: projects
author_profile: true
show_excerpts: true
header:
  image: /assets/images/projects/header.jpg
  alt: Abstract dot pattern background image
  caption: Image by [Tomislav Jakupec](https://pixabay.com/users/tommyvideo-3092371/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5719221) from [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5719221)
---
{% capture written_label %}'None'{% endcapture %}

{% for collection in site.collections %}
  {% unless collection.output == false or collection.label == "posts" %}
    {% capture label %}{{ collection.label }}{% endcapture %}
    {% if label != written_label %}
      <h2 id="{{ label | slugify }}" class="archive__subtitle">{{ label }}</h2>
      {% capture written_label %}{{ label }}{% endcapture %}
    {% endif %}
  {% endunless %}
  {% for post in collection.docs %}
    {% unless collection.output == false or collection.label == "posts" %}
      {% include archive-single.html %}
    {% endunless %}
  {% endfor %}
{% endfor %}