
## Design
### Template Sources
- [Free CSS Templates](http://www.free-css.com/template-categories/portfolio)
- [HTML5 UP](http://html5up.net/)
- [Start Bootstrap](http://startbootstrap.com/)
- [Theme Wagon](https://themewagon.com/top-html-landing-page-templates/)
- [Templatemo](https://templatemo.com/tag/portfolio)
- [One Page Love](https://onepagelove.com/templates/free-templates)

### Icons
- [fontawesome](https://fontawesome.com/)
- [devicon lib](https://konpa.github.io/devicon/)

### Fonts
- https://fontflipper.com
- https://fonts.google.com/

### Assets

- https://www.figma.com/
- https://pigment.shapefactory.co/
- https://duotone.shapefactory.co/
- https://gradient.shapefactory.co/
- https://source.unsplash.com/
- https://www.heropatterns.com/
- https://coolors.co/
## Dev Environment
### VSCode Plugins
- css-peek
- live server
- html css support
- live sass compiler

```json
{
  "liveSassCompile.settings.formats": [
    {
      "format": "expanded",
      "extensionName": ".css", // what kind of file to save the transpilation as
      "savePath": "/assets/css/" // where to save the final transpiled css
    }
  ],
  "liveSassCompile.settings.excludeList": ["**/node_modules/**", ".vscode/**"],
  "liveSassCompile.settings.includeItems": ["assets/sass/main.scss"],
  "liveSassCompile.settings.generateMap": true,
  "liveSassCompile.settings.autoprefix": ["> 1%", "last 2 versions"]
}
```

### Basic Gulp Set Up

1. `npm init -yes`
2. `npm install --save-dev gulp gulp-sass browser-sync`
3. add `.gitignore` file to root dir for `/node_modules`
4. add `gulpfile.js`

```javascript
"use strict";

const gulp = require("gulp");
const sass = require("gulp-sass");
const browserSync = require("browser-sync").create();

// this function will transpile our SCSS into CSS and
// stream any changes to the browser
function style() {
  return (
    gulp
      // 1. indicate the origin SCSS files
      .src("./assets/sass/**/*.scss")
      // 2. invoke the transpiling and log any errors to the console
      .pipe(sass().on("error", sass.logError))
      // 3. indicate the destination directory
      .pipe(gulp.dest("./assets/css"))
      // 4. stream any changes to the browser
      .pipe(browserSync.stream())
  );
}

// this function will initialize our dev browser and watch files for changes
// to trigger live reloads
function watch() {
  browserSync.init({
    server: {
      // indicate the root to be hosted on our dev server
      baseDir: "./"
    }
  });
  // each of these watch callbacks will take a location to watch
  // as a first argument and a callback to invoke when a change occurs
  gulp.watch("./assets/sass/**/*.scss", style);
  gulp.watch("./*.html").on("change", browserSync.reload);
  gulp.watch("./assets/js/**/*.js").on("change", browserSync.reload);
}

// here we export the gulp commands for access in our terminal
exports.style = style;
exports.watch = watch;
```
