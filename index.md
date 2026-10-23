---
layout: default
title: Home
---

{::nomarkdown}
<div class="box">
{% assign posts = site.posts | where_exp: "item", "item.tags contains 'highlight'" | sort: "sortby"  %}
{% for post in posts limit:3 %}
<div class="entry">
<a href="{{ post.url }}">
<img src="{{ post.image }}" width=240 height=160 />
<h2>{{ post.title }}<h2>
</a>
</div>
{% endfor %}
</div>
{:/nomarkdown}

[sumo]: {% post_url /project/2025-08-01-sumo-digital %}
[braise]: {% post_url /project/2024-10-27-braise-satan %}
[qr]: {% post_url /project/2026-05-09-honours %}
[heldritch]: {% post_url /project/2026-09-01-heldritch-rush %}
[risingstar]: {% post_url /project/2024-01-24-aurionnaut %}
[ensure]: {% post_url /blog/2026-10-23-ensure %}

# Hi, It's Me, Finlay
My interest in programming and game development began early, first with GameMaker and then with Python and C++ in high school in Edinburgh. This culminated in me entering the Computer Games Technology course at Abertay. 

I have made mods, participated in game jams, the Grads in Games [Rising Star Programming challenge][risingstar], and Abertay's [DARE Academy][braise]. From 2024 to 25 I worked as a Placement Programmer at [Sumo Digital][sumo], and from 2025 to 26 I went back to university to [finish my degree][qr] while working on [Heldritch Rush][heldritch] in the background. 

Some games that inspire me today are: Outer Wilds, Deltarune, Tunic, and the Half-Life series.

## Unreal Engine 5
Unreal Engine 5 is the platform with which I have the most experience. 
I worked with it professionally at [Sumo][sumo] for a year, working on multiplayer networking, UI logic, and gameplay systems. 
Outside of work, we used it at Abertay to create [Braise Satan][braise]. During my final year of university, I continued to use Unreal Engine for the development of [Heldritch Rush][heldritch], where I worked on all areas of game programming.
## C++
C++ has been my language of choice throughout my programming journey. 
I started using it in high school and have continued to hone my skills with it through university and my placement.
If you want to see some of my code, [look here](https://github.com/CyborgCabbage/yournespal).
## Collaboration
I have experience working in a large and small teams. I really enjoyed my first collaborative experience working on [Braise Satan][braise] in a team of 8, and I was also fortunate to work in larger team of around 70 at [Sumo][sumo] ([Thomas Chase](https://www.linkedin.com/in/thomas-chase-1674518), a game designer I worked with at Sumo was kind enough to leave a recommendation on my [LinkedIn](https://www.linkedin.com/in/finlay-laidlaw-3302ab2b1/)).

<br>
<details>
  <summary>All Posts</summary>
  <ul> 
    {% for post in site.posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a> <span style="color: lightgrey; font-style: italic;">{{ post.date | date_to_string }}</span></li>
    {% endfor %}
  </ul>
</details>