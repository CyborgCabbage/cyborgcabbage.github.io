---
layout: default
title: Home
---
# Highlights

{::nomarkdown}
<div class="box">
{% assign posts = site.posts | where_exp: "item", "item.tags contains 'highlight'" %}
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

# About
My interest in programming and game development began early, first with GameMaker and then with Python and C++ in high school in Edinburgh. This culminated in me entering the Computer Games Technology course at Abertay. 

I have made mods, participated in game jams, the Grads in Games Rising Star Programming challenge, and Abertay's DARE Academy. From 2024 to 25 I worked as a Placement Programmer at Sumo Digital, and from 2025 to 26 I went back to university to finish my degree while working on Heldritch Rush in the background. 

Some games that inspire me today are: Outer Wilds, Deltarune, Tunic, and the Half-Life series.

<!-- Email obfuscation, some spans are removed with CSS by display:none -->
<div class="email"><span>finlay</span><span>jericho</span><span>laidlaw@</span><span>zemp.</span><span>gm</span><span>clink.</span><span>ail.</span><span>kirkle.</span><span>com</span></div>

[LinkedIn](https://uk.linkedin.com/in/finlay-laidlaw-3302ab2b1)

# Skills
## Unreal Engine 5
Unreal Engine 5 is the platform with which I have the most experience. 
I worked with it professionally at Sumo for a year, working on multiplayer networking, UI logic, and gameplay systems. 
Outside of work, we used it at Abertay to create Braise Satan. During my final year of university, I continued to use Unreal Engine for the development of Heldritch Rush, where I worked on all areas of game programming.
## C++
C++ has been my language of choice throughout my programming journey.
I started using it in high school and have continued to hone my skills with it through university and my placement.
## Soft Skills
I have worked in large and small teams and firmly believe that good games can only be made when there is a spirit of collaboration on the personal, intra- and inter- department level.
## All Posts
{::nomarkdown}
<ul> 
  {% for post in site.posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> <span style="color: lightgrey; font-style: italic;">{{ post.date | date_to_string }}</span></li>
  {% endfor %}
</ul>
{:/nomarkdown}