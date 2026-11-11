---
layout: default
title: Home
---

[sumo]: {% post_url /project/2025-08-01-sumo-digital %}
[braise]: {% post_url /project/2024-10-27-braise-satan %}
[qr]: {% post_url /project/2026-05-09-honours %}
[heldritch]: {% post_url /project/2026-09-01-heldritch-rush %}
[risingstar]: {% post_url /project/2024-01-24-aurionnaut %}

<div id="bio-panel" markdown="1">
<div id="bio">
<img src="/assets/images/me.jpg">
<section markdown="1">

# Hi, I'm Finlay
I'm a games programmer who believes in writing clean code and making my code *work* for others. I enjoy making good tools, fixing tricky bugs, finding new methods, and working with other skilled professionals to bring ideas into reality.

I am confident in my C++ and Unreal Engine 5 skills, with professional experience at Sumo Digital and 1st class honours in BSc (Hons) Computer Games Technology at Abertay. I have made mods, participated in game jams, the Grads in Games Rising Star Programming challenge, and Abertay’s DARE Academy.

I like indie games, reading sci-fi/fantasy, and learning about physics and science. Some games that inspire me today: Deltarune, Sifu, Outer Wilds, Celeste, and Hardspace: Shipbreaker.
</section>
</div>
[⇓ Download CV ⇓](assets/downloads/about/FinlayLaidlaw_CV.pdf){:.button-link}
</div>

<div class="skills-panel">
<div class="skills">
<section markdown="1">

# Experiences
* Heldritch Rush / Programmer (UE5)
* Sumo Digital / Placement Programmer (UE5)
* Braise Satan (DARE Academy) / Programmer (UE5)
* AURIONNAUT / Developer (C++)
* Game Jams / Developer (Unity/Godot/UE5)
* Undertale for the NES / Developer (6502)

## BSc (Hons) Computer Games Technology
* Creating many game projects
* Multi-disciplinary collaboration
* Engine programming: scene/object management and creating usable APIs
* Graphics Programming: DirectX11, OpenGL, shadow maps, compute shaders
* Network programming: 
* Mathematics: rigid-body physics, splines, collisions, and intersections.
* Multiplayer networking
* Primarily C++, but also TypeScript and Python

</section>
<section markdown="1">

# Skills

## C++
* Modern C++, up to C++23
* Using the standard library
* Safe memory management with smart pointers
* Error handling with expected or exceptions 
* Clean coding practices (encapsulation, DRY, good names, etc.)
* Learning and integrating new libraries as needed
* If you want to see some of my code, [look here](https://github.com/CyborgCabbage/yournespal).

## Unreal Engine 5
* 3Cs: Enhanced Input and Mover
* UI programming: Common UI, UMG, Editor Menu Extensions, and Slate
* Tools programming: implementing undo/redo support, integrating with RewindDebugger, and various minor debug features
* Multiplayer: C++ Game Framework (PlayerState, GameMode, etc.), RPCs, fixing networking bugs
* Architecture: Using Subsystems,

## Soft Skills
* Getting up to speed with an existing project and processes quickly
* Researching and learning new skills independently as needed
* Communicating with others to solve problems on large teams
* Using professional workflows: Jira, CI/CD, code review, and adhering to coding standards 
* Using a variety of resources to solve problems: documentation, blog-posts, reading source-code, learning new debug tools, testing to confirm expected behaviour

## Engine Level
* DirectX11 and OpenGL
* Scene and object management
* Low level networking
* Using physics, audio, input, and windowing libraries

## Miscellaneous
* Some experience with Godot and Unity in game jams
* HTML, CSS, TypeScript and WebAssembly

</section>
</div>
<figure class="quote">

<blockquote markdown="1">
Finlay is a fantastic programmer to work with. He's got a great attitude, is extremely fast and capable at prototyping, and isn't afraid to speak up and make suggestions to improve the features he is working on. He's also really engaged and delivers work extremely quickly. 

I'd happily work with Finlay again in the future, and have no doubt he has a bright future ahead of him in the games industry.

  —Thomas Chase, Senior Principle 3Cs Designer at Sumo Digital Sheffield
</blockquote>
</figure>
</div>

<div id="project-list">
{% assign posts = site.posts | where_exp: "item", "item.tags contains 'highlight'" | sort: "sortby"  %}
{% for post in posts %}
<div class="project-background" style="background-image: url({{ post.background }});">
<div class="project-outer">
<h1>{{ post.title }}</h1>
<div class="project-panel">
<section class="project-thumbnail">
<img src="{{ post.image }}" />
<a href="{{ post.url }}">Go to project page</a>
</section>
<section markdown="1" class="project-summary">
{{ post.summary }}
</section>
</div>
</div>
</div>
{% endfor %}
</div>