# Gulp + Sass + LiveReload

This Gist goes over setting up  a gulp workflow that will:

1. watch for any sass changes, then compiles sass source into css
2. watch for any changes in the public directory, and trigger live-reload
3. serve static content in `public/`

---

## globally install npm

First, make sure you have all the required dependencies. If you do not have npm or gulp installed globally, you need to install them. If you already have these installed, skip to the next step.

_to install npm_
`brew install npm`

_to install gulp_
`npm install -g gulp`

---

## set up gulp + sass + livereload for your project

### set up node modules

1. initialize your project with a `package.json` file

 ````
 npm init
 ````
2. set up your `.gitignore` file to ignore `node_modules`

 ````
 echo "node_modules" >> .gitignore
 ````
3. add and commit your 2 new files to be tracked in git
4. install and save required dependencies using `npm install -D [node_module name]`
  - `gulp`
  - `gulp-sass`
  - `gulp-connect`

### set up the sass directory

5. set up sass source directory `mkdir sass/`
6. set up sass source file `touch sass/styles.scss`
7. set up sass compiled output directory `mkdir -p public/css`

### set up an index file

8. set up root html5 template file `touch public/index.html`
9. generate a base html5 template in `public/index.html`
10. in the index file head, load `css/styles.css`
11. include an `h1` element in the body with the content of `Hello Gulp, Sass, and Livereload`

### set up your gulpfile

12. set up a Gulpfile `touch gulpfile.js`
13. set up the Gulpfile tasks. Contents of gulpfile.js:

```
var gulp = require('gulp');
var sass = require('gulp-sass');
var connect = require('gulp-connect');

gulp.task('connect', function(){
  connect.server({
    root: 'public',
    livereload: true
  });
});

// keeps gulp from crashing for scss errors
gulp.task('sass', function () {
  return gulp.src('./sass/*.scss')
      .pipe(sass({ errLogToConsole: true }))
      .pipe(gulp.dest('./public/css'));
});

gulp.task('livereload', function (){
  gulp.src('./public/**/*')
  .pipe(connect.reload());
});

gulp.task('watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
  gulp.watch('./public/**/*', ['livereload']);
});

gulp.task('default', ['connect', 'watch', 'sass']);
```

### test sass and the gulp task

14. In the `sass/styles.scss` file, write the "Hello World" of Sass (we are checking if variables work. If #333333 loads as the background color, variables are working, and thus, sass is working).

    ````
    $dark-color : #333333;

    body {
      background: $dark-color;
    }
    ````
15. run `gulp` in your terminal
16. open the `styles.css` file in SublimeText (the /public/styles/styles.css file, not the /sass/styles.scss file)
17. check if it is proper css
18. open a browser `open localhost:8080`
19. you should see a dark grey background and your single black h1 text

### test livereload

1. while you terminal, browser, and sublime text are all visible, add the following to your `styles.scss`

 ````
 h1 {
   color: #F1F1F1;
 }
 ````
2. **_note_** livereload will only track files that it _sees_ when you first start the gulp task. including the initial generated css files, after the first time you compile, you'll have to *restart gulp!*
3. once you save this file, your browser should automatically refresh, and your h1 element should be white.

### viewport

In order for the webpage to render and scale properly on mobile devices, add this meta tag to your `public/index.html` file in the the `<head>` element

````
 <meta name="viewport" content="width=device-width, initial-scale=1.0" />
````

---

## commit your project

Once gulp sass and livereload are wired up correctly, commit your new files

While you work, make sure to keep your "gulp" terminal running and visible, if your sass source is invalid, it will crash the watching gulp process, and you will want to see the error message that will output in that terminal. Then you can restart your `gulp` watcher after fixing your errors.

Anytime you add new files to your `public/` directory (like images or javascript), you may need to kill then restart your gulp task