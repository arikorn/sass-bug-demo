Demonstrate error-reporting bug in Sass 1.89.2 (Sass embedded on WSL-Linux, Dart Sass on Windows)
(See [Issue #2598](https://github.com/sass/dart-sass/issues/2598) )

This bug is nearly identical to [Issue #1180](https://github.com/sass/dart-sass/issues/1180) but that one appears to have been resolved.

The top-level file is App.scss
There is an (intentional) typo in the variable name, so it is not found in the imported library. (The correct name is $white.)

Run: `sass App.scss`

Sass reports:
~~~
Error: This module was already loaded, so it can't be configured using "with".
   ┌──> scss/_functions.scss
3  │   @forward "functions/contrast-ratio"; // also loaded in color-contrast
   │   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ new load
   ╵
   ┌──> scss/functions/_color-contrast-variables.scss
1  │   @use "contrast-ratio" as *;
   │   ━━━━━━━━━━━━━━━━━━━━━━━━━━ original load
   ╵
   ┌──> App.scss
8  │ ┌ @use 'scss/coreui' with (
9  │ │    $whte: #fff
10 │ │ );
   │ └─^ configuration
   ╵
  scss/_functions.scss 3:1  @forward
  scss/coreui.scss 2:1      @use
  App.scss 8:1              root stylesheet
~~~

instead of:
~~~
Error: This variable was not declared with !default in the @used module.
  ╷
9 │    $whte: #fff
  │    ^^^^^^^^^^^
  ╵
  App.scss 9:4  root stylesheet
~~~

The example files are taken from CoreUI; I have stripped out about as much as I could to keep it simple.
