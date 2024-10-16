# CHANGES IN litedown VERSION 0.3

- Added a new engine `md` to output Markdown text both verbatim and as-is, which can be useful for showing Markdown examples, e.g.,

  ````md
  ```{md}
  You can see both the _source_ and _output_ of
  this `md` chunk.
  ```
  
  You can also use `{md} the engine **inline**`.
  ````

- Added a new engine `mermaid` to generate Mermaid diagrams, e.g.,

  ````md
  ```{mermaid, fig.cap='A nice flowchart.'}
  graph TD;
      A-->B;
      A-->C;
      B-->D;
      C-->D;
  ```
  ````

- Added helper functions `pkg_desc()`, `pkg_news()`, `pkg_citation()`, `pkg_code()`, and `pkg_manual()` to get various package information for building the full package documentation as a single-file book (thanks, @jangorecki @llrs #24, @TimTaylor #22).

- LaTeX math environments such as equations can be numbered and cross-referenced now (thanks, @hturner, #32).

- Section headings containing the class name "unlisted" will be excluded in the table of contents.

- Provided a way to write `<span>` with attributes based on empty links, i.e., `[text](){.class #id ...}`. The empty URL here tells `mark()` to treat the link as a `<span>` instead of `<a>`.

- Added back/forward/refresh/print buttons to the toolbar in the `litedown::roam()` preview interface.

- Changed the behavior of `.Rmd` and `.R` file links in the `litedown::roam()` interface: previously, clicking on an `.Rmd` or `.R` filename will execute them; now it will only show their content, because fully executing the code may be expensive or even dangerous (especially when the files were not authored by you). A new "Run" button has been provided in the interface, on which you can click on to run a file in memory and preview it (i.e., the old behavior of clicking on filenames). You should use this button only if you trust the file.

- Added the JS asset [`@mathjax-config`](https://github.com/yihui/misc.js/blob/main/js/mathjax-config.js) to enable equation numbering by default when the JS math library is set to MathJax (thanks, @hturner, #32).

- Set `options(bitmapType = 'cairo')` in `fuse()` if `capabilities('cairo')` is TRUE, which will generate smaller bitmap plot files (e.g., `.png`) than using `quartz` or `Xlib`, and is also a safer option for `fuse()` to be executed in parallel (rstudio/rmarkdown#2561).

- Added a new vignette engine `litedown::book` to make it possible to build multiple vignettes into a book. To use this engine, declare `\VignetteEngine{litedown::book}` only in the book index file (e.g., `index.Rmd`) but not in other book chapter files.

- Added support for an array of multiple authors in the YAML metadata (thanks, @AlbertLei, #28). If the `author` field in YAML is an array of length > 1, each author will be written to a separate `<h2>` in HTML output, or concatenated by `\and` in LaTeX output. Note that you can also write multiple authors in a single string (e.g., `author: "Jane X and John Y"`) instead of using an array (`author: ["Jane X", "John Y"]`), in which case the string will be treated as a single author (they will be put inside a single `<h2>` in HTML output).

- Fixed the bug that the leading `-`, `+`, or `*` in a LaTeX math expression was recognized as the bullet list marker, which would invalidate the math expression (thanks, @hturner, #33).

- Changed the first `-` to `:` in automatically generated element IDs, including section, figure, and table IDs, e.g., the ID `sec-intro-methods` is changed to `sec:intro-methods`, and `fig-nice-plot` is changed to `fig:nice-plot`. You can still use `-` when manually assigning IDs to elements, e.g., `# Intro Methods {#sec-intro-methods}`. For backward compatibility, cross-references using `-` will be resolved if the `:` version of the ID can be found, e.g., `@sec-intro-methods` will be resolved to `@sec:intro-methods` if the former cannot be found but the latter can.

- Fixed a bug that when LaTeX math environments are written in raw LaTeX blocks (i.e., ```` ```{=latex}````), `mark()` will not load the math JS library such as MathJax or KaTeX unless `$ $` or `$$ $$` expressions are present in the document.

- As-is output accepts attributes via the chunk option `attr.asis` now. If provided, as-is output will be wrapped in a fenced Div with these attributes.

- Numeric output from inline code will no longer be formatted if the value is wrapped in `I()`.

- The prefix for the automatic IDs of `h1` headings has been changed from `sec:` to `chp:`. For other levels of headings, the prefix is still `sec:`.

- Provided a new option `embed_cleanup` to clean up plot files that have been embedded in HTML output (thanks, @TimTaylor, #16).

- `fuse()` supports the output format `litedown::markdown_format` now, which generates the intermediate Markdown from R Markdown without further rendering Markdown to other formats. Using this output format is equivalent to `fuse(..., output = '.md')` or `fuse(..., output = 'markdown')` (thanks, @mikmart, #35).

# CHANGES IN litedown VERSION 0.2

- A data frame (or matrix/tibble) wrapped in `I()` is fully printed to a table now by default. Without `I()`, data objects are truncated to 10 rows by default when printing to tables.

- When `options(litedown.fig.alt = TRUE)` and the chunk option `fig.alt` is unset, `fuse()` will emit reminders about the missing alt text for code chunks containing plots (thanks, @TimTaylor, #23). Providing alt text can improve the accessibility of images in HTML output. To avoid omitting the alt text inadvertently, you can set the option `litedown.fig.alt` in your `.Rprofile`.

- Added the meta variable `plain-title` for HTML output, which is the plain version of the document title (i.e., without HTML tags), and used in the `<title>` tag.

- Check boxes from `- [ ] ...` are no longer disabled in HTML output.

- The implicit latest version of jsdelivr resources will be resolved to an explicit version, e.g., `https://cdn.jsdelivr.net/npm/@xiee/utils/css/default.css` will be resolved to `https://cdn.jsdelivr.net/npm/@xiee/utils@X.Y.Z/css/default.css`, where `X.Y.Z` is the current latest version. This will make sure the HTML output containing jsdelivr resources is stable.

# CHANGES IN litedown VERSION 0.1

- Initial CRAN release.
