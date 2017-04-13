# gulp-s3-publish [![NPM version][npm-image]][npm-url]

> s3 plugin for [gulp](https://github.com/gulpjs/gulp)

## Info
This plugin is a fork of [gulp-s3](https://github.com/nkostelnik/gulp-s3), which was unmaintained when this repository was forked.

This plugin adds support for AWS Signature V4 (which is mandatory for the newer regions), produces Streams2/3 compatible streams and will also error on upload failure, which can be useful in CI/CD environments

Please open an issue for feature requests.

## Usage

First, install `gulp-s3-publish` as a development dependency:

```shell
npm install --save-dev gulp-s3-publish
```

Setup your aws.json file
```javascript
{
  "key": "AKIAI3Z7CUAFHG53DMJA",
  "secret": "acYxWRu5RRa6CwzQuhdXEfTpbQA+1XQJ7Z1bGTCx",
  "bucket": "dev.example.com",
  "region": "eu-west-1"
}
```

Then, use it in your `gulpfile.js`:
```javascript
var s3 = require("gulp-s3-publish");

aws = JSON.parse(fs.readFileSync('./aws.json'));
gulp.src('./dist/**/*')
    .pipe(s3(aws));
```

## API


#### options.headers

Type: `Object`          
Default: `{}`
Options: `CacheControl` | `ContentDisposition` | `ContentEncoding` | `ContentLanguage`, `ContentLength` | `ContentMD5` | `ContentType` | `Expires`

Headers to set to each file uploaded to S3. Note that ContentLenghth is automatically computed for you and ContentType is guessed using the `mime` package.

```javascript
var options = { headers: {'CacheControl': 'max-age=315360000, no-transform, public'} };
gulp.src('./dist/**', {read: false})
    .pipe(s3(aws, options));
```

### options.acl

Type: `String`
Default: `public-read`
Options: `private` | `public-read` | `public-read-write` | `authenticated-read` | `aws-exec-read` | `bucket-owner-read` | `bucket-owner-full-control`

Set the access control for each object uploaded. Defaults to `public-read`.

```javascript
var options = { acl: 'private' };
gulp.src('./dist/**', {read: false})
    .pipe(s3(aws, options));
```

#### options.gzippedOnly

Type: `Boolean`          
Default: `false`

Only upload files with .gz extension, additionally it will remove the .gz suffix on destination filename and set appropriate Content-Type and Content-Encoding headers.

```javascript
var gzip = require("gulp-gzip");
...
var options = { gzippedOnly: true };
gulp.src('./dist/**')
    .pipe(gzip())
    .pipe(s3(aws, options));
});
```

#### options.concurrency

Type: `Number`          
Default: `1`

Change the concurrency at which the upload occurs, useful for projects with many files. Defaults to 1.

```javascript
var options = { concurrency: 5 };
gulp.src('./dist/**')
    .pipe(s3(aws, options));
});
```

## License

[MIT License](http://en.wikipedia.org/wiki/MIT_License)

[npm-url]: https://npmjs.org/package/gulp-s3-publish
[npm-image]: https://badge.fury.io/js/gulp-s3-publish.png
