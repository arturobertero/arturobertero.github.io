---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

Peer-reviewed articles, book chapters, reports, and research outputs are listed below in reverse chronological order. You can also find my work on [Google Scholar](https://scholar.google.com/citations?hl=en&user=C5ps1gwAAAAJ) and [ORCID](https://orcid.org/0000-0001-6014-1794).

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
