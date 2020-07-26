---
title: Laravel Blade Components Quirks and Limitations
description: 
published: true
date: 2020-07-26T09:29:41.355Z
tags: 
editor: markdown
---

Laravel Blade components looks like it is trying to do what VueJS components is doing. You could:

- Create custom tags.
- Pass string or data in the form of class attributes.
- Create slots for rendering custom HTML in the component.

This enhances the reusability and encapsulation of the component.

However, before jumping into it, do take note of the limitation and quirks of Blade components.

1. Slots HTML are rendered before the component contents are rendered.
2. Slots are only rendered once for every component.
3. Using double qoutes inside class attributes will cause "endif"
