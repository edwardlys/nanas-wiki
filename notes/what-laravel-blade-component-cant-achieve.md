---
title: Laravel Blade Components Quirks and Limitations
description: 
published: true
date: 2020-10-30T05:30:04.176Z
tags: 
editor: markdown
dateCreated: 2020-07-26T09:15:07.318Z
---

Laravel Blade components looks like it is trying to do what VueJS components is doing. You could:

- Create custom tags.
- Pass string or data in the form of class attributes.
- Create slots for rendering custom HTML in the component.

This enhances the reusability and encapsulation of the component.

However, before jumping into it, do take note of the limitation and quirks of Blade components.

## Slots content are rendered before the component contents are rendered

Why this was significant to me? Because coming from VueJS/Vuetify background, I was used to this:

```javascript
<template v-slot:item.glutenfree="{ item }">
		<v-simple-checkbox v-model="item.glutenfree" disabled></v-simple-checkbox>
</template>
```
Note that we can extract `item` in the code above to be used in your slot content. This means data can be passed from the component as the component contents are being rendered. 

Where as blade component slots are unable to perform such task because slot contents are rendered before the components does and thus making the slot contents static. 

This was not in the documentation. Maybe it is something that should be understood by common sense but I learnt it the hard way.

-- Update 2020-10-30
In Laravel 8 there has been a few new features for Blade templating specifically to dynamically render templates in run time. Perhaps this update will mitigate this issue.

Check it out here: https://laravel-news.com/laravel8

## Blade delimiter does not work as expected inside class attributes

I could not find the reason behind this failure but below are some example. It looks like the Blade delimiter `{{` and `}}` does not work inside the component. Hence causing this issue. 

```php
//This is OK
<x-search action="{{ url('/admin/product') }}" search="{{ $search }}"/>

//This is OK
<x-search action='{{ url("/admin/product") }}' search="{{ $search }}"/>

//This is not OK
<x-search action="{{ url("/admin/product") }}" search="{{ $search }}"/>
```

