	# `stratic-truncate-indexes`

[Gulp][1] plugin to truncate [Stratic][2] indexes to a specific number of posts

This is particularly useful in combination with [`stratic-indexes-to-rss`][].

## Installation

    npm install stratic-truncate-indexes

## Usage

All you need to do is pipe Stratic indexes to this module and BAM! It'll truncate them to a certain number of posts - 15 by default.

You may also pass a number to the module to override the number of posts to truncate to. The complete example below shows this.

## Examples

Minimal `gulpfile.js` for this module to work:

```js
var gulp = require('gulp');
var frontmatter = require('gulp-gray-matter');
var straticDateInPath = require('stratic-date-in-path');
var addsrc = require('gulp-add-src');
var straticPostsToIndex = require('stratic-posts-to-index');
var straticTruncateIndexes = require('stratic-truncate-indexes');

gulp.task('rss', function() {
    gulp.src('*.md')
        .pipe(straticParseHeader())
        .pipe(straticDateInPath())
        .pipe(addsrc('src/blog/index.jade'))
        .pipe(straticPostsToIndex('index.jade'))
        .pipe(straticTruncateIndexes());
});
```

Complete example `gulpfile.js`:

```js
var gulp = require('gulp');
var frontmatter = require('gulp-gray-matter');
var remark = require('gulp-remark');
var remarkHtml = require('remark-html');
var straticDateInPath = require('stratic-date-in-path');
var addsrc = require('gulp-add-src');
var straticPostsToIndex = require('stratic-posts-to-index');
var straticTruncateIndexes = require('stratic-truncate-indexes');
var straticIndexesToRss = require('stratic-indexes-to-rss');
var rename = require('gulp-rename');

gulp.task('rss', function() {
    gulp.src('*.md')
        .pipe(frontmatter())
        .pipe(remark().use(remarkHtml))
        .pipe(straticDateInPath())
        .pipe(addsrc('src/blog/index.jade'))
        .pipe(straticPostsToIndex('index.jade'))
        .pipe(straticTruncateIndexes(10)) // Override the default number of posts to truncate to
        .pipe(straticIndexesToRss({title: 'Blag!'}, 'https://example.com/blog/'))
        .pipe(rename({ extname: '.rss' }))
        .pipe(gulp.dest('dist/blog'));
});
```

## License

LGPL 3.0+

## Author

AJ Jordan <alex@strugee.net>

 [1]: http://gulpjs.com/
 [2]: https://github.com/strugee/generator-stratic
 [`stratic-indexes-to-rss`]: https://github.com/straticjs/stratic-indexes-to-rss
