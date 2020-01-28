+++
author = "Jonathan Van Eenwyk"
categories = ["web"]
date = "2020-01-22"
description = "How to animate an expanding title with CSS"
featured = "expanding-letters.gif"
featuredalt = "Expanding Letters Animation"
featuredpath = "date"
linktitle = ""
title = "Expanding letters animation"
type = "post"
+++

For [vihanti.com](https://www.vihanti.com), I really wanted some animations that
would capture our "Creativity Unleashed" slogan.  After exploring
[GSAP](https://greensock.com/gsap/) and
[AniCollection](http://anicollection.github.io/#/) and still not finding
anything, I finally designed my own animation around `letter-spacing` and
`opacity`.  I think it turned out really cool!

I'm using [waypoints.js](http://imakewebthings.com/waypoints/) to activate the
animation on scrolling.  To make it easy to set which page elements to animate,
I set the animations I want in HTML using `data` attributes.  Then, a little bit
of javascript picks up the animation and applies the appropriate class when
needed.

**scss**
```scss
// Expand out the letters until they fade out completely
@mixin keyframes($name) {
  @-webkit-keyframes #{$name} {
    @content;
  }
  @-moz-keyframes #{$name} {
    @content;
  }
  @-ms-keyframes #{$name} {
    @content;
  }
  @keyframes #{$name} {
    @content;
  }
}

@mixin expanding-letters($name) {
  -webkit-animation-name: $name;
  animation-name: $name;
  white-space: nowrap;
  overflow: hidden;

  @include keyframes($name) {
    0% {
      opacity: 1;
    }

    100% {
      letter-spacing: 100px;
      opacity: 0;
    }
  }

  @content;
}

.expanding-letters-fade-out {
  @include expanding-letters(expandingLettersFadeOut);
}

.expanding-letters-fade-in {
  @include expanding-letters(expandingLettersFadeIn);

  -webkit-animation-direction: reverse;
  animation-direction: reverse;
}
```

**html**
```html
<h1 class="title animated"
   data-animated="expanding-letters-fade-out"
   data-animated-reverse="expanding-letters-fade-in"
   data-offset="20">Unleashed</h1>
```

**javascript**
```js
import $ from 'jquery'
window.$ = $
window.jQuery = $

import 'waypoints/lib/jquery.waypoints.js'

$(document).ready(() => {
  $('.animated')
    .css('opacity', function () {
      const animated = $(this).data('animated')
      if (animated.indexOf('In') !== -1) {
        return 0
      }
    })
    .waypoint(function (direction) {
      const animated = $(this.element).data('animated')
      const animatedReverse = $(this.element).data('animated-reverse')

      if (direction === 'down') {
        $(this.element).removeClass(animatedReverse).addClass(animated)
      } else if (direction === 'up' && animatedReverse) {
        $(this.element).removeClass(animated).addClass(animatedReverse)
      }
    }, {
      offset: function () {
        return $(this.element).data('offset') || Math.ceil(this.context.element.innerHeight * 0.9)
      }
    })
})
```

