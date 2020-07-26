---
title: Laravel Blade Components Quirks and Limitations
description: 
published: true
date: 2020-07-26T09:34:11.809Z
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


## Using double qoutes inside class attributes will cause the component to fail.

Be careful when using helper functions inside class attributes of a component tag. Apparently using double qoutes will cause it to fail.

```php
//This is OK
<x-search action="{{ url('/admin/product') }}" search="{{ $search }}"/>

//This is not OK
<x-search action="{{ url("/admin/product") }}" search="{{ $search }}"/>
```

