# NgCssLoadProblem

This project represents a much simplified issue which I encountered on a separate project I have been working on.

In that project there are a number of files, both scss and css, but also some woff2 font files which are imported into one or master scss files using the `@import` statement.

NG-CLI fails to load the css files when attempting to build as it is unable to find the file, even though it is in the same location as the scss file which is requesting it.

Another Angular project in the estate which uses a custom webpack config file does not encounter this problem.

This problem doesn't occur when other scss files are `@import`ed. 

My expectation as a "user" is that if a file is referenced by another one in the same folder, the system should be able to find that file.

I suspect what is happening is that the system picks up and inlines the scss code, and then searches for any non-scss imports, by which time the current working location context of the original `@import` statement is lost. I believe this is a bug and hae created this repository to recreate the issue.

The project is simply a vanilla `ng new` project, with a handful of scss and css files added to te `assets` folder. The default `styles.scss` file `@import`s the `external-styles.scss` file, which in turn `@import`s both the `external-styles.css` and `another-file.scss` file, in that order.

When running ng build or ng serve we get an rror to the effect that `external-styles.css` could not be found. If we change the name of the file to be a scss file (we also need to slightly ament the file name to avoid a naming clash) and then reference that renamed file, the project builds and serves with no problems, and we can observe that the rules defined kick in (the background is red and divs have a green 1px border).