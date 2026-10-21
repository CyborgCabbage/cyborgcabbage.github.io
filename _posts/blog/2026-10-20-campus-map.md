---
layout: post
title: Abertay Campus Map
---

Abertay's online room finder utility often doesn't work, and it is otherwise quite difficult to find a map of the campus online.
I got these PDFs from the [Internet Archive](https://archive.org/), using links of the format "https://www.abertay.ac.uk/media/AbertayLevel4.pdf".

Beware they are from 2010 so out of date but it is better than nothing.

{% for i in (1..5) %}
<a href="/assets/map/AbertayLevel{{ i }}.pdf">Level {{ i }}</a>.
{% endfor %}