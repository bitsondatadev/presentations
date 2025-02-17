# Trino presentations

> **Note**
> These presentations are in draft mode. Please check back when they are ready for full use!

A collection of learning resources about Trino.

## Presenting

Use the URL for the repository to display the overview presentation, and
navigate to the individual presentations.

Alternatively run a local webserver with the runLocalServer.sh or
runLocalServer.cmd scripts, and navigate to http://localhost:9000.

Press ? for keyboard shortcuts, and find more information in the [reveal.js
documentation](https://revealjs.com/).

## More information

- Uses [reveal.js](https://revealjs.com/) version 4.4.0 source in the `reveal`
  folder.
- [reveal.js documentation](https://github.com/hakimel/reveal.js/) for info on
  writing and other details.
- [keyboard shorts](https://github.com/hakimel/reveal.js/wiki/Keyboard-Shortcuts)
  for presenting

## Building and updating the custom theme

Reveal.js has [multiple built-in themes](https://revealjs.com/themes/) but what's better is you can customize your own theme. Trino presentations use our own branding and coloring already, but they can always be improved. Here are the steps to update them.

We've created [trino-theme.scss](/css/trino-theme.scss) from the ```.scss``` files in [/reveal/css/theme/source](/reveal/css/theme/source/). It will be automatically compiled from Sass to CSS (see the [gulpfile](https://github.com/hakimel/reveal.js/blob/master/gulpfile.js)) when you run `npm run build -- css-themes`. To see all exposed variables to use in the scss files, reference the [/reveal/css/theme/template/exposer.scss](/reveal/css/theme/template/exposer.scss) file.

> **Note**
> For more general css changes to the presentation that don't directly apply to the Trino Reveal theme, put that CSS in [/css/trino.css](/css/trino.css)

Each theme file does four things in the following order:

1. **Include [/reveal/css/theme/template/mixins.scss](/reveal/css/theme/template/mixins.scss)**
Shared utility functions.

2. **Include [/reveal/css/theme/template/settings.scss](/reveal/css/theme/template/settings.scss)**
Declares a set of custom variables that the template file (step 4) expects. Can be overridden in step 3.

3. **Override**
This is where you override the default theme. Either by specifying variables (see [settings.scss](/reveal/css/theme/template/settings.scss) for reference) or by adding any selectors and styles you please.

4. **Include [/reveal/css/theme/template/theme.scss](/reveal/css/theme/template/theme.scss)**
The template theme file which will generate final CSS output based on the currently defined variables.
