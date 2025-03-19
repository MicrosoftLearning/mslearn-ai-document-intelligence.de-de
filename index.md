---
title: Azure KI Dokument Intelligenz-Übungen
permalink: index.html
layout: home
---

# Azure KI Dokument Intelligenz-Übungen

Die folgenden Übungen sind eine Ergänzung für die Module in Microsoft Learn.


{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %} {% for activity in labs  %} {% if activity.url contains 'ai-foundry' %} {% continue %} {% endif %}
- [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) {% endfor %}
