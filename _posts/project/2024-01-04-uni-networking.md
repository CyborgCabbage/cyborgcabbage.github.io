---
layout: post
title: Multiplayer Networking
---

Simple top down shooter
Server authoritative model of networking based on prediction and correction
The players inputs are executed on the client and set to the server
The server sends back inputs that have been confirmed, and also sends back any that were dropped and had to be predicted
When the client receives these, it reruns the player physics using these confirmed inputs as a base
The server predicts inputs simply by assuming the previously held buttons will continue to be held
