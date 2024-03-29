How to add 4D code highlight to GitHub Pages.

# Introduction

It's really easy to publish a public repository as a [GitHub Pages site](https://pages.github.com).

As described in the official [Quickstart](https://docs.github.com/en/pages/quickstart), you just need to 

1. Click **Settings**
2. Click Code and automation > **Pages**
3. Select **main** branch, **/ (root)** path, and save

<img src="https://github.com/miyako/4d-tip-github-pages/assets/1725068/0a7ceb19-09a9-420c-bd29-630639c1b982" height="40" />

…and that's it.

To add a [Jekyll Theme](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll) you just need to

1. Add a file named `_config.yml` to the root of the repository
2. Add a line to the file that specifies one of the [supported themes](https://pages.github.com/themes/), e.g.

```yml
theme: jekyll-theme-minimal
```

You can also add 4D code to the page using markdown, but here is the problem; syntax highlighting is not applied.

This is because the default syntax highlighter does not support the 4D language.

# Solution

1. Go to [highlightjs/highlight.js](https://github.com/highlightjs/highlight.js) and clone the repository. This library supports the 4D language

2. Follow the [instruction](https://highlightjs.readthedocs.io/en/latest/building-testing.html) to build `highlight.js`

3. Open Terminal at the root of the repository. You can specify the languages you want to include in the build, e.g. 

```
node tools/build.js 4d css javascript xml json sql yaml php
```

4. Add an `assets/js` folder to the root of the repository

5. Copy the generated `highlight.min.js` to this folder

6. Add an `assets/css` folder to the root of the repository

7. Copy the styles in `src/styles` to this folder

8. Add a `_includes/head-custom.html` file to the root of the repository. The HTML snippet in this file is added to the page. 

9. Add references to `highlight.min.js` and the style of your choice, e.g. 

```html
<link rel="stylesheet" href="{{ 'assets/css/base16/gigavolt.css' | relative_url }}">
<script type="text/javascript" src="{{ '/assets/js/highlight.min.js' | relative_url }}"></script>
<script>hljs.highlightAll();</script>
```

10. Add lines to `_config.yml` to disable the default syntax highlighter

```yml
kramdown:
  syntax_highlighter_opts:
    disable : true
```

# Result 

```4d
$param:=New object
// Close the process after 2s if not already terminated
$param.timeout:=2
// Start the system worker with the parameter defined above
$sys:=4D.SystemWorker.new("git --version";$param)

// Wait for the end or the process and return the response from Git
ALERT($sys.wait().response)
```
