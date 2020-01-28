+++
author = "Jonathan Van Eenwyk"
categories = ["web"]
date = "2020-01-20"
description = "How to build ES6 with gulp+rollup+babel+eslint+terser"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Using ES6 with gulp"
type = "post"
+++

For [vihanti.com](https://www.vihanti.com), we're using
[gulp](https://gulpjs.com/) as our build system instead of
[hugo](https://gohugo.io/) (which is what this site uses), because it allows us
to take advantage of the latest goodness available through npm.  I really wanted
to use ES6 for this project, but I found it a little tricky to find the best way
to get minified JS with ES6.  The important thing here is that we're using
[rollup](https://www.npmjs.com/package/gulp-better-rollup) to *both* process the
ES6 code and minify it using
[terser](https://www.npmjs.com/package/rollup-plugin-terser). I tried using
[babelify](https://github.com/babel/babelify) and
[uglify](https://www.npmjs.com/package/gulp-uglify), but I ran into trouble with
gulp 4's async task execution model.  I kept getting the dreaded "Did you
forget to signal async completion?" error.  I finally tracked it down to a
problem with [uglify](https://github.com/terinjokes/gulp-uglify/issues/358).

To help whoever comes along, here's the most important part:

**gulpfile.babel.js**
```js
import babel from 'rollup-plugin-babel'
import commonjs from '@rollup/plugin-commonjs'
import eslint from 'gulp-eslint'
import resolve from '@rollup/plugin-node-resolve'
import rollup from 'gulp-better-rollup'
import { terser } from 'rollup-plugin-terser'
import sourcemaps from 'gulp-sourcemaps'

export function scripts () {
  return gulp.src('app/scripts/app.js')
    .pipe(eslint())
    .pipe(eslint.format())
    .pipe(eslint.failAfterError())
    .pipe(sourcemaps.init())
    .pipe(rollup(
      {
        plugins: [
          babel(),
          resolve({ preferBuiltins: true, mainFields: ['browser'] }),
          commonjs(),
          terser()
        ]
      },
      { format: 'umd' }))
    .pipe(sourcemaps.write('.'))
    .pipe(gulp.dest('public'))
}
```

To see the rest of the config, pop over to
[github](https://github.com/vihanti/vihanti.com/blob/master/gulpfile.babel.js).

