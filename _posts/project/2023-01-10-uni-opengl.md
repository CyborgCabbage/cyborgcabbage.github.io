---
layout: post
title: OpenGL Mirrors & Shadow Volumes
---

Old OpenGL
Mirrors using stencil and clipping plane
Shadow volumes calculated manually
Quite slow, modern methods for shadow volumes use a geometry shader to create the hull
The extremely high fragment/pixel count makes them unappealing, shadow maps are superior (especially with cascades to support sharper shadows)