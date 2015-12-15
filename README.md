# Basic modular gulp setup for use in WordPress theme development.

This is the basic modular gulp starter setup that is used for [wpdevelopersclub.com](https://wpdevelopersclub.com) courses.

- __Contributors:__ [Alain Schlesser](http://www.alainschlesser.com)
- __License:__ [GPL-2.0+](http://www.gnu.org/licenses/gpl-2.0.html)

## Features

### General

* Modular structure that is easy to extend, with each task in a separate file.
* Single configuration file that allows a quick adaptation to the folder structure of a project.
* Error handling that pops up notifications and keeps the watch task running on error.

### Styles

* Linting through [Stylelint](http://stylelint.io/) with [WordPress standards](https://github.com/stylelint/stylelint-config-wordpress) preconfigured (overrideable in `config.js`).
* Compilation of Sass styles with [libSass](http://sass-lang.com/libsass).
* Post-processing through [PostCSS](https://github.com/postcss/postcss). Plugins included by default:
	* [Autoprefixer](https://github.com/postcss/autoprefixer) to add browser vendor prefixes;
	* [CSS-MQPacker](https://www.npmjs.com/package/css-mqpacker) to regroup media queries;
	* [Perfectionist](https://github.com/ben-eb/perfectionist) to prettify generated CSS.
* Check browser support of generated CSS against Autoprefixer settings through [doiuse](https://github.com/anandthakker/doiuse).
* Minification through [cssnano](http://cssnano.co/) (both minified & non-minified version are usable).
* Sourcemaps for easy debugging provided by [gulp-sourcemaps](https://www.npmjs.com/package/gulp-sourcemaps).

### Scripts

* Linting through [JSHint](http://jshint.com/) with [WordPress standards](https://develop.svn.wordpress.org/trunk/.jshintrc) preconfigured.
* Minification through [UglifyJS](http://lisperator.net/uglifyjs/)  (both minified & non-minified version are usable).

### Images

* Optimization through [ImageMin](https://github.com/imagemin/imagemin).

## Requirements

The main requirements to be able to use this are:

* [Node.js](https://nodejs.org/) - tested with 2.11.3
* [gulp](http://gulpjs.com/) - tested with 3.9

## Installation

The contents of this repository are meant to be dropped inside the root folder of an existing project, so that the `gulpfile.js` resides at the root of the project.

The file `gulp/config.js` should be modified so that it fits the project's folder structure.

To add the dependencies that are needed for the individual gulp tasks to your project's `package.json` ([which should already exist](https://docs.npmjs.com/cli/init)), run the following command in your project's root folder:

```
cat gulp/dependencies | xargs npm install --save-dev
```

The file `gulp/dependencies` can be safely deleted after this operation, it is no longer needed.

## Usage

To get an overview of the available tasks that are provided with this starter setup, run the following command in your project's root folder:

```
gulp help
```

The default task that will run when no specific option is provided is the `watch` task.

## Special Notes

### style.css

The stylesheet gets compiled to a `style.css` & `style.min.css`, but they do not get moved to the theme root folder.

Although WordPress requires a file called `style.css` inside the theme root folder, this goes against several best practices. That's why this setup will provide both minified & non-minified versions of the stylesheet in the proper assets folder (defaults to `assets/styles/style.css`).

You should provide a stub file inside the theme's root folder with the necessary theme metadata comments, so that WordPress correctly recognizes the theme.

To load the correct stylesheet on the frontend, add the following code to your theme:
```PHP
add_filter( 'stylesheet_uri', 'wpdc_stylesheet_uri', 10, 2 );
/**
 * Modify location of main stylesheet
 * N.B.: style.css in the theme's root is still needed to provide WordPress
 * with the necessary metadata about the theme
 */
function wpdc_stylesheet_uri( $stylesheet_uri, $stylesheet_dir_uri ) {

	// TODO: Adapt this to your theme's folder structure
	$folder = '/assets/styles/';

	// TODO: Adapt this to your theme's filenames
	$name = 'style';

	$suffix = defined( SCRIPT_DEBUG ) ? '.css' : '.min.css';

	return $stylesheet_dir_uri . $folder . $name . $suffix;
}
```

## Known Issues

* `gulp dist` has not been completed yet.

* There currently seems to be a race condition between Sass & PostCSS. Thanks to @iCaspar for the testing & debugging.

* If you are getting errors while pulling in the dependencies as described in the section "Installation", make sure that you are running a current version of `npm`. The latest stable one should always be correct, as of this writing, the code has been tested with version 3.3.9.
To get the current version, type the command `npm -v` at your command-line.
It is also a good idea to clear your npm's cache, as it might contain old versions of the packages we're trying to use. To clear your cache, type `npm cache clean` at your command-line.


## Tasks

The following `gulp` task files are included in the Gulp_Starter folder. 

* build.js - Launch individual tasks that are necessary for a build.

* clean.js - Clean out junk files after build.

* default.js - Default task to get executed when gulp is run without arguments.

* dist-copy.js - Copy all the files to the `$dist` folder.

* dist-images.js - Optimize images in place.

* dist-styles.js - Optimize stylesheets for distribution.

* dist-wipe.js - Totally wipe the contents of the distribution folder after doing a clean build.

* dist.js - Package everything up for distribution.

* help.js - Print a list of available gulp tasks.

* images-optimize.js - Copy and optimize images.

* images.js - Handle images.

* scripts-lint.js - Run the JavaScript files through a linter (jshint). 

* scripts-uglify.js - Minify JavaScript files.

* scripts.js - Handle JavaScript files.

* styles-lint.js - Run CSS through a linter (style lint). 

* styles-postcss.js - Process stylesheets with PostCSS.

* styles-sass.js - Compile sass source files and generate CSS.

* styles.js - Handle stylesheets.

* watch.js - Watch for file changes to relaunch necessary tasks.

