+++
author = "Jonathan Van Eenwyk"
categories = ["web"]
date = "2020-01-24"
description = "Put some snazz in a banner using particles"
featured = "particles.gif"
featuredalt = "Particles banner"
featuredpath = "date"
linktitle = ""
title = "Animated particles in page banner"
type = "post"
+++

Wanting to add some movement to our refreshed
[vihanti.com](https://www.vihanti.com) website, I found
[particles.js](https://vincentgarreau.com/particles.js/) which I figured just
might do the trick.  But I didn't want it over the whole site--just in the
header banner.  After tweaking the particles and downloading the json config, it
was actually pretty simple!

The most important part was making sure that the parent element of the particles
`div` was set to `position: relative`.  That way, we can set the width and
height of the particles to match the size of the hero banner.

```js
import $ from 'jquery'
window.$ = $
window.jQuery = $

$(document).ready(() => {
   window.particlesJS.load('particles', 'assets/particles.json')
})
```

```html
<!doctype html>
<html lang="en">
  <head>
    <script src="scripts/particles.js"></script>
  </head>
  <body>
    <section class="header hero has-text-centered is-relative">
      <div id="particles"></div>
      <div class="hero-head">
         {% include "partials/navigation.njk" %}
      </div>
      <div class="hero-body">
        {% block header %} {% endblock %}
      </div>
    </section>
  </body>
</html>
```

```scss
#particles {
  height: 100%;
  position: absolute;
  width: 100%;
}
```

