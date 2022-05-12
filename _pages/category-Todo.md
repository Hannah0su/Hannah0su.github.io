---
title: "Todo"
layout: archive
permalink: categories/Todo
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories.Todo %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}