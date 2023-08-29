# CHESS <!-- omit in toc -->

_CSS and HTML Encompassing Style System_ (CHESS) is a set of style and design rules for frontend component-based web development.

The goals of CHESS include:

- Facilitate component-based development.
- Provide a complete solution without requiring style preprocessors, JavaScript, compilation, or a frontend framework.
- Create universal formatting standards.
- Be easy to learn and be easy to use.
- Create predictable CSS specificity.
- Enforce consistency.

_This document focuses on more than merely aesthetic issues like formatting. It also provides design guidance — styling with a purpose._

## Copyright <!-- omit in toc -->

Jake Knerr © Ardisia Labs LLC

---

## Table of Contents <a id="toc" name="toc"></a> <!-- omit in toc -->

- [Why develop CHESS? Why not use a pre-existing solution?](#why-develop-chess-why-not-use-a-pre-existing-solution)
- [Document Structure](#document-structure)
- [Code Analyzers](#code-analyzers)
  - [Prettier](#prettier)
  - [Linters](#linters)
- [Shared Rules for both CSS and HTML](#shared-rules-for-both-css-and-html)
  - [Formatting](#formatting)
  - [Filenames](#filenames)
  - [Character Encoding](#character-encoding)
  - [Comments](#comments)
  - [General Design](#general-design)
- [HTML](#html)
  - [HTML Formatting](#html-formatting)
  - [HTML General Design](#html-general-design)
- [CSS](#css)
  - [CSS Suggested Review](#css-suggested-review)
  - [CSS Formatting](#css-formatting)
  - [CSS General Design](#css-general-design)
  - [Style Rule Sections and Section Order](#style-rule-sections-and-section-order)
  - [Imports](#imports)
  - [Fonts](#fonts)
  - [Theme](#theme)
  - [Resets](#resets)
  - [Globals](#globals)
  - [Vendor](#vendor)
  - [Utilities](#utilities)
  - [Components](#components)
  - [Components - Fragments](#components---fragments)
  - [Components - States](#components---states)
  - [Components - Composites](#components---composites)
  - [Components - Extensions](#components---extensions)
  - [Media Queries](#media-queries)
  - [Components - Documenting Components](#components---documenting-components)
  - [Components - Discussion](#components---discussion)
  - [App Overrides](#app-overrides)
  - [Animations](#animations)
- [Miscellaneous](#miscellaneous)
  - [F.A.Q.](#faq)
  - [Comparison To BEM](#comparison-to-bem)
  - [Comparison to Tailwind.](#comparison-to-tailwind)
- [Epilogue](#epilogue)

---

## Why develop CHESS? Why not use a pre-existing solution?

When developing a naming/design system, one has to decide if the system will emphasize writing CSS classes, classes in HTML, styles, or a combination of these priorities. For example, Tailwind maximizes writing classes directly in HTML but minimizes the creation of custom CSS classes and styling.

```html
<!-- Tailwind example - lots of predefined classes in HTML -->
<input
  class="bg-gray-50 border-gray-300 focus:ring-3 focus:ring-blue-300 h-4 w-4 rounded"
/>
```

BEM maximizes writing custom CSS classes and styles, and BEM also requires writing lots of classes in HTML. BEM avoids specificity conflict but at the cost of creating significant overhead for developers.

```html
<!-- BEM example - lots of custom CSS classes and lots of classes in HTML -->
<figure class="photo">
  <img class="photo__img" src="me.jpg" />
  <figcaption class="photo__caption">Look at me!</figcaption>
</figure>
```

```css
/* BEM */
.photo {
}

.photo__img {
}

.photo__caption {
}
```

CHESS aims for a compromise between these different priorities.

Also, most CSS/HTML systems avoid the CSS cascade entirely. The downsides of avoiding all style cascading are that (1) developers must constantly create names for classes (naming is hard), and (2) writing components with repeating elements (like a list) is very clunky because each repeated element requires custom classes. CHESS embraces the idea that the CSS cascade can make life easier and development faster.

Finally, some CSS/HTML systems do not focus on "components." A component is a bundle of styles and HTML nodes that are applied together in a web document. CHESS fully embraces the concept of components and componentizing a web document.

## Document Structure

This guide can roughly be broken up into two parts: (1) a series of style rules that are generally aimed at consistency and formatting, and (2) a design system for component-based development.

Feel free to jump ahead to the part of the guide focused on component-based development [here](#components).

## Code Analyzers

### Prettier

#### Use Prettier.

This guide does not restate formatting rules that Prettier already enforces.

> Why Prettier? The Prettier website puts it best: "By far the biggest reason for adopting Prettier is to stop all the on-going debates over styles. It is generally accepted that having a common style guide is valuable for a project and team but getting there is a very painful and unrewarding process. People get very emotional around particular ways of writing code and nobody likes spending time writing and receiving nits.<br><br>So why choose the 'Prettier style guide' over any other random style guide? Because Prettier is the only 'style guide' that is fully automatic. Even if Prettier does not format all code 100% the way you'd like, it's worth the "sacrifice" given the unique benefits of Prettier, don't you think?"

#### Use Prettier's default configuration.

> Why? The default configuration is reasonable and the easiest to set up.

### Linters

#### (Optional) Consider using HTML and CSS linters to catch code quality warnings and errors. Leave formatting to Prettier.

Linters can help enforce conventions and consistency. However, if you do decide to use a linter, don't let it boss you around because sometimes otherwise good conventions should be violated.

## Shared Rules for both CSS and HTML

### Formatting

#### Use lowercase for all language-dependent strings (strings with special meaning in CSS or HTML) when uppercase and lowercase are both permissible options.

In other words, use lowercase when defining style rules, selectors, declarations, properties, tags, attributes, etc.

> Why? Mixing uppercase and lowercase is very dissonant.

```css
/* avoid */
.Btn {
  color: RED;
}

/* good */
.btn {
  color: red;
}

/* 
  acceptable - "SuffiX" is not language-dependent (it has no special meaning to 
  the CSS language) 
*/
.btn::after {
  content: "SuffiX";
}
```

```html
<!-- avoid -->
<div data-Custom="xyz"></div>
<div style="COLOR: RED"></div>

<!-- good -->
<div data-custom="xyz"></div>
<div style="color: red"></div>

<!-- 
  acceptable - uppercase is allowed in user-specified strings because they are 
  not language-dependent
-->
<div data-custom="A Lovely Name"></div>
```

**[⬆ Table of Contents](#toc)**

---

### Filenames

#### Use the proper file extension for each file type.

- HTML files — extension is .html
- CSS files — extension is .css

#### When naming files:

- **Use only lowercase letters, numbers, and hyphens.**
- **The first character of a filename is a letter.**
- **Use train-case to separate words.**

Use descriptive names.

> Why? Alphanumeric file names ensure the widest compatibility and is simple.

> Why train-case and no underscores? Underlined filenames and links can obscure the underscore, making it easy to miss.

```
# avoid
Index.html
pageSummary.html
toggleButton.css
toggle_button.css

# good
index.html
page-summary.html
toggle-button.css
toggle-button.css
```

**[⬆ Table of Contents](#toc)**

---

### Character Encoding

#### Use UTF-8.

- HTML files — include the `<meta charset="utf-8">` meta element.
- CSS files — nothing is required because UTF-8 is assumed.

> Why UTF-8? Because it is compatible with ASCII, compact, and fully supports Unicode.

#### Prefer using Unicode characters instead of escape sequences.

If an escape sequence is required, leave a comment describing the character.

> Why? Escape sequences are less clear than the actual character.

```css
/* discouraged */
.icon {
  content: "\20AC";
}

/* preferred */
.icon {
  content: "€";
}
```

#### Avoid the byte order mark (BOM).

> Why? The BOM typically does not cause any problems, but it can trigger unexpected issues on older browsers or when parsing string content. Also, the Unicode standard does not recommend using it, "Use of a BOM is neither required nor recommended for UTF-8 ..." Unicode Version 5.0 http://www.unicode.org/versions/Unicode5.0.0/ch02.pdf

**[⬆ Table of Contents](#toc)**

---

### Comments

In this section, _code_ refers to HTML markup and CSS code.

#### Use comments to provide context and documentation for the code.

> Why? Comments are an excellent way to break up code sections into discrete parts and provide guidance to other developers in understanding complex or unintuitive code.

#### Write comments using a natural language.

> Why? Comments complement code; they do not restate code. Since code uses a non-natural language, comments use a natural language to avoid merely repeating the code.

#### Unless an exception is clearly stated in this guide, prefer to write comments:

- **In lowercase only.**
- **Use punctuation only when necessary for clarity.**
- **Sentence fragments are permissible if the meaning of the fragment is clear.**
- **Separate sentences and sentence fragments with semicolons.**

**Otherwise, use formal grammar, e.g., proper verb conjugation, subject-verb agreement, etc. Focus on clarity.**

This rule only applies if it makes sense with regard to one's natural language. This author intends no English language chauvinism.

> Why simplify grammar? Simplifying grammar frees developers from many grammatical debates and decisions. Also, grammatical simplification means that a developer does not have to drift as far from the coding mindset when writing a comment as one would if formal grammar was required. In other words, thinking about formal grammar can throw one's mind out of a coding groove.

```css
/* discouraged */
/* 
  The CSS rule below adds a red outline to a document HTML node. Also, this 
  rule is not required for Electron apps.
*/
.red-outline {
}

/* preferred */
/* adds a red outline; not required for electron apps */
.red-outline {
}
```

```html
<!-- discouraged -->
<!-- The DIV below is used to indicate the author of the article herein. -->
<div>By Jake</div>

<!-- preferred -->
<!-- author goes here -->
<div>By Jake</div>
```

#### Do not be afraid to use technical terms in comments.

Use a vocabulary that is understood by the intended audience.

```css
/* fixing ancient ie — web developers know what ie is referring to */
.ie-fix {
}
```

#### Choose a style guide to resolve any natural language style questions.

Also, if one's natural language has different national varieties, pick one to resolve differences between them. For example, if using English, decide to use American English, British English, Canadian English, or any other national variety to resolve differences.

For example, Wikipedia's style guide for English is here: [Wikipedia:Manual of Style [English]](https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style)

#### Single-line comments should have a single space at the beginning and the end of the comment.

> Why? A comment enclosed in spaces is much easier to read.

```css
/* spaces are on each side of this comment */
```

```html
<!-- spaces are on each side of this comment -->
```

#### Write multi-line comments in the following way:

- **Place a newline after the opening `<!––` (HTML) or `/*` (CSS).**
- **Indent each line of the comment's text content.**
- **For comments that overflow the line width, break after the last word that fits on the line, and continue on the next line.**
- **Place a newline before the closing `-->` (HTML) or `*/` (CSS).**

```css
/*
  lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor
  incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis

  line x
  line y
  line z
*/
```

```html
<!--
  lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor
  incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis 

  line x
  line y
  line z
-->
```

#### Break single-line comments that overflow the line width into a multi-line comment.

> Why? This ensures that the comment is visible regardless of the word-wrapping setting on the reader's text editor.

```css
/* avoid */
/* lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor incididunt ut labore */

/* good */
/* 
  lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor 
  incididunt ut labore 
*/
```

```html
<!-- avoid -->
<!--
  lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor incididunt ut labore
-->

<!-- good -->
<!--
  lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor 
  incididunt ut labore
-->
```

#### Place comments immediately above the subject of the comments.

Also, prefer to not place comments on lines that also contain code. In other words, prefer comments on their own line.

> Why immediately above the subject? This convention makes it clear to what code a comment refers to.

> Why discourage comments inline with code? Because comments and code mixed within a single line are a distraction that hurts code comprehension.

```css
/* avoid */

/* simple red class */

.red {
  color: red;
}

/* good */

/* simple red class */
.css {
  color: red;
}

/* discouraged */

.red {
  color: /* i like red */ red; /* shows error state */
}

/* preferred */

.red {
  /* i like red; shows error state */
  color: red;
}
```

```html
<!-- avoid -->

<!-- author byline -->

<div>
  <div class="author">Jake</div>
</div>

<!-- good -->

<!-- author byline -->
<div>
  <div class="author">Jake</div>
</div>
```

#### Place an empty line above a comment unless it's on the first line of a block.

> Why prefer an empty line? Without a blank line, code becomes cramped.

```css
/* avoid */

.btn {
  color: blue;
  /* my favorite font */
  font: Arial;
}
/* error class */
.error {
  color: red;
}

/* good */

.btn {
  color: blue;

  /* my favorite font */
  font: Arial;
}

/* error class */
.error {
  color: red;
}
```

```html
<!-- avoid -->

<div></div>
<!-- below is for buttons -->
<div class="btn"></div>

<!-- good -->

<div></div>

<!-- below is for buttons -->
<div class="btn"></div>
```

#### For comments that are used to delineate sections (section dividers):\*\*

- **The comment is on a single line and does not overflow the line width.**
- **Starting from the left after the opening characters `<!--` (HTML) or `/*` (CSS), place a single space.**
- **Place two consecutive equal signs `==` (HTML) or two hyphens `--` (CSS).**
- **Add another space.**
- **Next, place the comment's text content. The text content uses title case.**
- **Add another space.**
- **Place two consecutive equal signs `==` (HTML), or 10 hyphens `----------` (CSS), and add another space before closing the comment with the closing characters `-->` (HTML) or `*/` (CSS).**
- **Place blank lines above and below the divider, if possible.**

Section divider comments are not encouraged or discouraged. Usage of them is left up to a developer's discretion. This is simply a guide as to how they should be formatted if used.

> Why equal signs in HTML dividers? Although hyphens would be preferable for consistency, double hyphens create potential XHTML issues.

```css
.btn {
}

/* -- Document and Button States ---------- */

.state-one {
}
```

```html
<div>Content</div>

<!-- == Header == -->

<header>Content</header>
```

#### Use a _TODO_ comment to signify follow-up work.

> Why? _TODO_ is the most common convention for action comments.

```css
/* TODO add hover transitions */
.btn {
}
```

```html
<!-- TODO add a logo -->
<div></div>
```

#### (Optional) A file can, at a developer's discretion, have a top-level comment with a description of the file, its purpose, authors, licenses, copyrights, and any other pertinent top-level information. In addition, the top-level comment has the following requirements:

- **Be placed on the first line of the file.**
- **Have a blank line following it.**
- **Unlike other comments in the document that use simplified grammar, the top-level comment uses formal grammar, i.e., complete sentences, proper noun capitalization, punctuation, etc.**

A reader should be able to comprehend the top-level comment without directly referring to the code.

```css
/*
  This file contains the styling for the contact forms.

  Author: Jake Knerr
  All rights reserved.
*/

.btn {
}
```

```html
<!--
  This file contains the markup for the contact forms.

  Author: Jake Knerr
  All rights reserved.
-->

<!DOCTYPE html>
```

**[⬆ Table of Contents](#toc)**

---

### General Design

#### Prefer valid HTML and CSS.

Exceptions to valid CSS and HTML are very narrowly prescribed.

> Why? Valid code can be expected to work correctly across clients and is easier to maintain.

#### (Optional) Consider HTML and CSS validators to check for validity.

For example, https://validator.w3.org/ (HTML) and https://jigsaw.w3.org/css-validator/ (CSS).

**[⬆ Table of Contents](#toc)**

---

## HTML

### HTML Formatting

#### Rules for how to arrange multiple values:

- **When a node has multiple attributes, prefer to arrange them in alphabetical order.**
- **When an attribute has multiple values, prefer to arrange them in alphabetical order.**

These are defaults. Other rules in this document may specify more specific attribute ordering.

> Why? Alphabetical order provides a predictable and searchable structure.

```html
<!-- discouraged -->
<div disabled data-custom="xyz" alt="xyz"></div>

<!-- preferred -->
<div alt="xyz" data-custom="xyz" disabled></div>

<!-- discouraged -->
<div
  class="zeta alpha omega"
  style="z-index: 1; color: red; background: black;"
></div>

<!-- preferred -->
<div
  class="alpha omega zeta"
  style="background: black; color: red; z-index: 1;"
></div>
```

#### Use the valueless option for boolean attributes.

> Why? The valueless option is concise without any loss of clarity.

```html
<!-- avoid -->
<button type="submit" disabled="disabled"></button>
<button type="submit" disabled=""></button>
<button type="submit" disabled="disabled"></button>
<button type="submit" disabled="disabled"></button>

<!-- good -->
<button type="submit" disabled></button>
```

#### Avoid the _type_ attribute for script tags and avoid the _type_ attribute for link tags that point to external style sheets.

> Why? Because the HTML5 specification and browsers assume JavaScript and CSS for these tags.

```html
<!-- avoid -->
<link href="styles.css" rel="stylesheet" type="text/css" />
<script src="scripts.js" type="text/javascript"></script>

<!-- good -->
<link href="styles.css" rel="stylesheet" />
<script src="scripts.js"></script>
```

#### Include optional tags.

> Why? The omission of optional tags can be confusing.

```html
<!-- avoid - head and body omitted -->
<!DOCTYPE html>
<title>Page</title>
<p>Content

<!-- good -->
<!DOCTYPE html>
  <head>
    <title>Page</title>
  </head>
  <body>
    <p>Content</p>
  </body>
</html>
```

#### The use of HTML entities is discouraged.

However, one must use HTML entities for characters that possess special meaning in HTML, e.g., <, >, &, etc.

> Why? Entities are more verbose and require the developer to memorize what they represent. Also, most entities are unnecessary because documents should be encoded in UTF-8.

```html
<!-- discouraged -->
<div>&copy; &cent; &reg;</div>

<!-- preferred -->
<div>© ¢ ®</div>
```

#### Quote attribute values.

In other words, avoid unquoted attribute values.

> Why? Consistency, fewer rules to memorize, greater editor support, and quoted attributes are easier to maintain because values can be edited without worrying about breaking them because quotes are missing.

```
<!-- avoid -->
<div class=foo>

<!-- good -->
<div class="foo">
```

#### Avoid writing attribute values that merely restate the default value.

> Why? There are a lot of default values, and restating them is extremely redundant and verbose.

```html
<!-- avoid -->
<div draggable="auto"></div>

<!-- good -->
<div></div>
```

#### When Prettier gives one the option:

- **Prefer placing block elements on a new line.**
- **Prefer placing inline elements (inline as defined by HTML, not CSS) on the same line as their parent element if everything fits within the column width.**

However, the most important issue is clarity; the chosen format should be clear.

```html
<!-- discouraged - block elements should be on new lines -->
<header><div>Hello World</div></header>

<!-- preferred -->
<header>
  <div>Hello World</div>
</header>

<!-- discouraged - inline element not on the same line as the parent -->
<div>
  <span>Hello World</span>
</div>

<!-- preferred -->
<div><span>Hello World</span></div>
```

**[⬆ Table of Contents](#toc)**

---

### HTML General Design

#### Use the HTML5 doctype.

> Why? HTML5 is more capable than other doctypes, and browser adoption of HTML5 is high.

```html
<!DOCTYPE html>
```

#### Prefer semantic elements where applicable.

> Why? HTML5's semantic elements add additional context to the document. Also, they can improve search engine results and accessibility.

```html
<!-- preferred -->
<article></article>
```

#### Avoid inline styles.

> Why? Inline styles do not separate the content from its styling. This lack of separation makes styling management more difficult. Also, inline styles bloat the HTML because they can not be reused like style rules defined in style sheets.

```html
<!-- avoid -->
<div style="display: flex; color: red;">
  <button>Click Me</button>
</div>

<!-- good -->
<div class="alert-btn">
  <button>Click Me</button>
</div>
```

#### Avoid unnecessary parent elements.

> Why? Complexity should serve a purpose, and extra containers hurt performance.

```html
<!-- avoid - div serves no purpose -->
<header>
  <div>
    <span>Foo bar</span>
  </div>
</header>

<!-- good -->
<header>
  <span>Foo bar</span>
</header>
```

#### Prefer fallbacks (alternative content) for images, videos, and any other media.

Fallbacks include `alt` tags, captions, and any content that will be displayed if the media is not supported by the client.

> Why? Fallbacks are necessary for accessibility.

```html
<!-- discouraged -->
<img src="red-balloon.jpg" />

<!-- preferred -->
<img src="red-balloon.jpg" alt="Red Balloon" />
```

**[⬆ Table of Contents](#toc)**

---

## CSS

### CSS Suggested Review

Since one of CHESS's primary motivations is managing CSS specificity, it is recommended that readers possess a firm grasp of how [CSS specificity is resolved and calculated](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity).

**[⬆ Table of Contents](#toc)**

---

### CSS Formatting

#### Prefer to arrange declaration properties in alphabetical order within a declaration block.

This is a soft preference.

> Why? Sorted properties provide a predictable and searchable structure. Also, the rule is simple and easy to follow. There is no requirement to memorize what attributes should come first, last, or any particular order.

```css
/* discouraged */
.btn {
  text-align: center;
  box-shadow: 1px #000;
  background: red;
}

/* preferred */
.btn {
  background: red;
  box-shadow: 1px #000;
  text-align: center;
}
```

#### Prefer to place vendor-prefixed styles at the top of the declaration block, sorted alphanumerically.

> Why? This way, they are easier to notice, find, and remove as browsers evolve.

```css
/* discouraged */
.btn {
  color: red;
  display: block;
  -webkit-background-clip: text;
  -ms-hyphens: auto;
}

/* preferred */
.btn {
  -ms-hyphens: auto;
  -webkit-background-clip: text;
  color: red;
  display: block;
}
```

#### Prefer shorthand CSS properties.

> Why? Shorthand properties are more concise and still very clear.

```css
/* discouraged */
.btn {
  padding-bottom: 10px;
  padding-left: 10px;
  padding-right: 5px;
  padding-top: 10px;
}

/* preferred */
.btn {
  padding: 10px 5px 10px 10px;
}
```

#### Avoid units for properties with a value of 0.

> Why? They are unnecessary noise.

```css
/* avoid */
.btn {
  margin: 0px;
}

/* good */
.btn {
  margin: 0;
}
```

#### Prefer hexadecimal notation to describe color unless an alpha channel is required, in which case use `rgba()` format.

> Why? Hexadecimal notation is the standard and is more concise. However, using hexadecimal to describe an alpha channel value of 0-100 is too tricky for creatures with ten fingers. In other words, using base-16 to describe base-10 numbers (0-100) is incredibly unintuitive. Thus, when an alpha value is needed, use `rgba()`.

```css
/* discouraged */
.btn {
  color: rgb(25, 10, 10);
}

/* preferred */
.btn {
  color: #190a0a;
}

/* discouraged */
.btn {
  color: #00ff00cc;
}

/* preferred */
.btn {
  color: rgba(0, 255, 0, 0.8);
}
```

#### Prefer three-character hexadecimal notation over six-character notation when possible.

> Why? Three character notation is more concise and still very clear.

```css
/* discouraged */
.btn {
  color: #555555;
}

/* preferred */
.btn {
  color: #555;
}
```

#### Use train-case for class selector names and custom property names.

```css
/* avoid */
.hugetogglebutton {
}

:root {
  --primaryBackground: brown;
}

/* good */
.huge-toggle-button {
}

:root {
  --primary-background: brown;
}
```

#### Each style rule is followed by a blank line.

```css
/* avoid */
* {
}
body {
}
.btn {
}

/* good */
* {
}

body {
}

.btn {
}
```

**[⬆ Table of Contents](#toc)**

---

### CSS General Design

#### Prefer class names to be as short as possible but as long as necessary. Aim for intuitive names that are also descriptive.

Clarity is king when naming class selectors.

> Remember that readers may not be fluent in English. Do not show off your dictionary/thesaurus skills.

> What is an intuitive name? It is a name that other developers will recognize and know its meaning.

> What is a descriptive name? It is a name that provides adequate detail.

```css
/* discouraged - not intuitive */
.pulchritudinous-button {
}

/* preferred */
.pretty-btn {
}

/* discouraged - not descriptive */
.asset {
}

/* preferred */
.icon {
}
```

#### Prefer abbreviations if they are clear.

Be phonetic and see if you can drop any letters without a loss of clarity. Think about it from another person's perspective: what might another person imagine the shortened name means?

Think: `l (large)`, `md (medium)`, `sm (small)`, `btn (button)`, `sum (summary)`, `el (element)`, `a (link)`, `ico (icon)`, `txt (text)`.

```css
/* discouraged */
.button-large {
}

/* preferred */
.btn-l {
}

/* discouraged - very large array; readers probably do not know this acronym */
.vla {
}

/* acceptable - readers know NASA */
.nasa {
}

/* acceptable */
.button {
}

/* also acceptable - still clear */
.btn {
}
```

#### Prefer semantic names over comparative names when possible.

In other words, prefer using names that describe the purpose of the class instead of how it compares to another state (how it looks).

> Why? Semantic names are more clear to the reader and more reusable.

```css
/* discouraged - just states how it looks */
.btn-red {
}

/* preferred - states the purpose of the button */
.btn-error {
}
```

#### If a comparative name is required, prefer a name where the difference is postfixed to the default name. Use `x`, `xx`, `xxx`, etc. to provide additional strength to the comparison.

```css
.btn {
}

/* smaller than default */
.btn-s {
}

/* even smaller */
.btn-xs {
}

/* even smaller */
.btn-xxs {
}

/* larger than default */
.btn-l {
}

/* even larger */
.btn-xl {
}

/* even larger */
.btn-xxl {
}
```

#### Prefer to use `REM` for font sizes (or anything that should scale with fonts) and use `px` for everything else.

Sometimes, it may be useful to use `REM` for styles that are related to displayed text. For example, the padding around text may be specified in `REM` units.

Do not change the browser's default `font-size`.

> Why? This way, users can change the font size of your document without affecting the layout.

```css
/* avoid */
html,
body {
  font-size: 16px;
}

/* good */
html,
body {
  font-size: 100%;
}
```

#### Avoid using the `@layer` at-rule.

> Why? CHESS already specifies the order of different sections of a document's styles, making `@layer` redundant.

#### For the time being, avoid the Shadow DOM.

Shadow DOM is a great concept, but implementation considerations argue against its use.

> Why? The shadow DOM requires JavaScript, which is unsuitable for many content-focused projects.

> I am building a SPA. Why not use the Shadow DOM? Every element using a shadow DOM requires a style tag or a link to a preloaded stylesheet. Parsing a stylesheet for every component has negative performance implications. A solution named 'adopted stylesheets' aims to fix this problem, but Safari has not implemented it.

#### Avoid becoming obsessive over DRY and reusability with CSS.

These principles taken too far will cause madness.

**[⬆ Table of Contents](#toc)**

---

### Style Rule Sections and Section Order

#### Prefer to split up a document's style rules into the following sections and order:

1. Imports.
1. Fonts
1. Theme.
1. Resets.
1. Globals.
1. Vendor.
1. Utilities.
1. Components.
1. App Overrides

#### The style rules can all exist in the same style sheet or multiple style sheets. Prefer to preserve the desired section order.

```html
<!-- discouraged -->
<link rel="stylesheet" type="text/css" href="vendor.css" />
<link rel="stylesheet" type="text/css" href="app-overrides.css" />
<link rel="stylesheet" type="text/css" href="globals.css" />
<link rel="stylesheet" type="text/css" href="theme.css" />
<link rel="stylesheet" type="text/css" href="fonts.css" />
<link rel="stylesheet" type="text/css" href="resets.css" />
<link rel="stylesheet" type="text/css" href="utilities.css" />
<link rel="stylesheet" type="text/css" href="imports.css" />
<link rel="stylesheet" type="text/css" href="components.css" />

<!-- preferred -->
<link rel="stylesheet" type="text/css" href="imports.css" />
<link rel="stylesheet" type="text/css" href="fonts.css" />
<link rel="stylesheet" type="text/css" href="theme.css" />
<link rel="stylesheet" type="text/css" href="resets.css" />
<link rel="stylesheet" type="text/css" href="globals.css" />
<link rel="stylesheet" type="text/css" href="vendor.css" />
<link rel="stylesheet" type="text/css" href="utilities.css" />
<link rel="stylesheet" type="text/css" href="components.css" />
<link rel="stylesheet" type="text/css" href="app-overrides.css" />
```

**[⬆ Table of Contents](#toc)**

---

### Imports

#### Place `@import` rules in this section.

> Why? `@import` rules must precede other rule types.

```css
@import "my.css";
```

**[⬆ Table of Contents](#toc)**

---

### Fonts

#### Place `@font-face` rules in this section.

> Why? Including font definitions towards the top will start the asynchronous download of the font files sooner rather than later.

```css
@font-face {
  font-family: Inter var;
  src: url("/fonts/Inter.var.woff2") format("woff2");
}
```

**[⬆ Table of Contents](#toc)**

### Theme

This section is for defining default values for CSS variables (sometimes known as custom properties) that are used throughout the document.

#### Use CSS variables to create a default visual theme.

> Why? Custom properties work well for creating a default theme, are natively supported, and browser support for them is excellent.

#### CSS variables are useful for styles that are reused across many different components.

> Why? They can help prevent the developer from typing the same styles over and over.

#### CSS variables are particularly useful for styles that do not cause side effects.

Side effects are styles that when changed force changes in other styles. An example of styles that do not typically cause side effects are "colors", and styles that typically cause side effects are sizing styles like "width".

> Why are CSS variables so useful for styles without side effects? Without side effects, variables can be adjusted in isolation without requiring other changes, which is a perfect use case for CSS variables and theming.

```css
:root {
  background-color: white;
  color: black;
}

.dark-theme {
  background-color: black;
  color: white;
}
```

#### Use the _:root_ selector to declare variables.

> Why use the _:root_ selector? This way, variables cascade down for use by the entire document.

```css
/* bright color theme */
:root {
  --accent-color: #0000ff;
  --code-font: Courier;
  --small-border: 2px solid blue;
}

.btn {
  border-color: var(--accent-color);
  font-family: var(--code-font);
}
```

#### If multiple themes must be available at runtime, append a descriptive class to the _:root_ selector for each additional theme.

Add the theme to the document by adding the theme class to the `html` element.

> Why? Users may be able to change themes at runtime.

```css
/* default theme */
:root {
  --accent-color: red;
}

.dark:root {
  --accent-color: blue;
}
```

```html
<html class="dark">
  <head></head>
  <body></body>
</html>
```

#### (Optional) Consider adding a namespace prefix to a CSS variable property name to help avoid naming collisions.

> Why? Other libraries in your document using CSS variables are unlikely to use the same name and the same prefix.

```css
/* no namespace prefixes */
:root {
  --primary-color: red;
  --secondary-color: #0000ff;
}

/*
  namespace prefix b- — risk of name collisions with other libraries has been
  reduced
*/
:root {
  --b-primary-color: red;
  --b-secondary-color: #0000ff;
}
```

**[⬆ Table of Contents](#toc)**

---

### Resets

This section is for style rules that equalize, or _reset_, the default styles among clients.

#### A reset style rule, or _reset_, normalizes the display across different browsers/clients. In other words, a reset fixes an inconsistent default style among browsers/clients.

A popular set of reset style rules is [normalize.css](https://github.com/necolas/normalize.css).

```css
/* not a reset - body.backgroundColor is not inconsistent among browsers */
body {
  background-color: red;
}

/* is a reset - body.margin is inconsistent among browsers */
body {
  margin: 0;
}
```

**[⬆ Table of Contents](#toc)**

---

### Globals

This section contains style rules that are applied to elements across the entire document.

#### A global style rule, or _global_ has the specificity of a single simple selector:

- **A single [universal](https://developer.mozilla.org/en-US/docs/Web/CSS/Universal_selectors) selector; or**
- **A single [type](https://developer.mozilla.org/en-US/docs/Web/CSS/Type_selectors) selector.**

**Global styles are applied to elements across the entire document.**

A global is not intended to be applied to specific content. When creating styling intended for specific content, use components, which are detailed later in the section titled "Components."

Globals are useful to prevent declaring the same styles repeatedly.

> Why only permit type, or universal selectors in globals? Since globals apply default styling document-wide, more specific styling should easily override them. Type, attribute, and universal selectors have low CSS specificity; class based rules defined later in the document can easily override them.

> Why allow type and attribute selectors when other CSS naming schemas do not? To avoid adding a large number of style classes and rules to the document merely to follow the component abstraction. Avoid blindly adhering to abstractions.

```css
/* avoid - specificity too high */
a.btn {
}

/* good */
* {
  box-sizing: border-box;
}

a {
  text-decoration: underline;
}

div {
  box-sizing: border-box;
}
```

#### Global rules can use additional selectors, but they must be contained within a `:where` pseudo-class function. The specificity of a global rule must not be greater than a single type selector.

> Why? Global rules can be useful to prevent redefining the same styles over and over, but their specificity must remain low so components can override them.

```css
/* avoid */
div.error {
}

input[type="email"] {
}

div#logo {
}

/* acceptable */
div:where(.error) {
}

input:where([type="email"]) {
}

div:where(#logo) {
}
```

**[⬆ Table of Contents](#toc)**

---

### Vendor

#### Include styles required by third-party libraries in this location.

> Why in this location? Component CSS (explained in a later section below) should override conflicts with vendor CSS.

```css
/* vendor rules */
.highlight-js {
}
```

**[⬆ Table of Contents](#toc)**

---

### Utilities

**.\_\_utility**<br>
Example: `.__error`

This section describes style rules that are not tied to specific content.

- **Utilities are rules that use a single class selector and can be applied to any content in the document.**
- **Prefer to define utilities by adding two underscores (`__`) as a prefix to a nounal identifier for a single class selector.**

> Why use utilities? They are a useful technique to avoid repeating the same styles over and over. Also, they can be useful to make small changes to a component without having to create a composite.

```css
.__no-sel {
  user-select: none;
}

.__err {
  font-weight: bold;
  color: red;
}
```

Example - Using a utility to nudge a button. Without the utility moving the button would require a composite or extension.

```css
.__pad-top-16 {
  padding-top: 16px;
}
```

```html
  <div class="grid">
    <button class="btn">
    <button class="btn">
    <button class="btn __pad-top-16">
  </div>
```

#### Utilities are used to add styles, not override.

Since utilities only use a single class selector and are defined before components, their specificity will not be enough to override conflicts in components. Only use them to add styles that will not conflict with component rules.

```html
<a class="btn __red">Click Me</a>
```

```css
/* avoid - utility is overriding a rule */
.btn {
  color: blue;
}

.__red {
  color: red;
}
```

#### Do not make utilities required classes in a component.

Only use utilities to make small changes to components.

> Why not make utilities required? The greatest advantage of components is the mental mapping between the component's classes and the corresponding HTML. Requiring outside styles, like utilities, breaks down this mental mapping, and using components becomes clunky.

**[⬆ Table of Contents](#toc)**

---

### Components

**.(component)**<br>
Example: `.btn-huge`

This section is typically the most substantial portion of an application's styles.

#### A component is a set of related style rules that are applied as a group to document content.

> Why the name _component_? CHESS's concept of components is well suited for documents built using a view-component-based design since each view-component object can be coupled with a CHESS component to encapsulate styling.

#### Components consist of four types of style rules:

- **The defining rule.**
- **Component fragments, which apply default styling to child content.**
- **Component states, which apply dynamic styling to content.**
- **Component extensions, which apply styling to nested components.**

These types will be described later in this document.

#### The _defining rule_:

- **Each component has at least one style rule, the _defining rule_.**
- **The defining rule uses a class selector with a unique name.**
- **The unique name is the _component name_.**
- **The component name should be nounal.**
- **The defining rule is applied to document content to trigger component styling.**
- **When listing the style rules for a component, list the defining rule first.**

For the component name, the use of gerunds, infinitives, and participles are discouraged because even though they can function as nouns/adjectives in a sentence, they are typically perceived as verbs.

> Why is the _component name_ unique? The component name serves as a namespace to group all of the component's style rules, and its uniqueness helps reduce name collisions with other style rules.

> Why prefer nounal names? Components are _things_, just like nouns. They represent content on the page.

```css
/* defining rule for component */
.btn {
}
```

#### A component is _applied_ to document content by adding the component's defining rule to the content's class attribute.

The defining rule must be applied to content, even if the defining rule doesn't apply any styling.

```css
/* component - defining rule */
.btn {
}
```

```html
<!-- component is being applied to the content below -->
<div class="button">Click</div>
```

#### A content element can only have a single component applied to it.

In other words, multiple components cannot be applied to the same content.

> Why? The composition of components leads to confusion.

```css
.btn {
}

.btn-error {
}
```

```html
<!-- avoid - multiple components applied -->
<div class="btn btn-error">
  <svg></svg>
</div>
```

#### Breaking up a document into components is an excellent way to reduce styling complexity and encourage style reuse.

```html
<!-- component - form-container -->
<div class="form-container">
  <div>Welcome to my form.</div>

  <!-- component - email-field -->
  <input class="email-field" />

  <!-- component - address-field -->
  <input class="address-field" />
</div>
```

#### All of a component's style rules are defined together in the same place in a style sheet.

A component must keep all of its rules in a single file.

> Why? By grouping all of a component's style rules, a developer can more easily locate and understand a component's purpose. Also, the order of rules matters and keeping all rules together keeps the order clear and accessible.

#### When naming related components, consider a naming system where adjectives follow the noun.

For example, instead of `red-button` or `user-name-input`, consider `button-red` or `input-user-name`.

> Why? Related components will appear together when sorted alphabetically, making it much easier to scan related components in a file tree or a stylesheet.

```css
.btn-huge /* instead of huge-btn */ {
}

.btn-lg /* instead of lg-btn */ {
}

.btn-sm /* instead of sm-btn */ {
}
```

#### When the defining rule has no styles, there is no need to define it.

However, it still must be applied to HTML content.

> Why apply a non-existent CSS rule? Even if the defining rule does not exist in CSS, it still functions as a target for selectors used by other component rules.

```css
/* component "btn" - no defining rule */

/* fragment */
.btn a {
}
```

```html
<!-- defining rule still applied - necessary to target the `a` fragment -->
<div class="btn">
  <a></a>
</div>
```

#### (Optional) Consider adding a namespace prefix to a component's class names to help avoid style collisions with other components, libraries, and any other styling present in the document.

Namespacing via a prefix is particularly useful for component libraries since the styles in a library may be used across large numbers of documents.

```css
/* no namespace prefix */
.btn {
}

/* namespace prefix b- — risk of style collisions have been reduced */
.b-button {
}
```

#### (Optional) Consider creating a file for each component.

> Why? Putting each component in a separate file is an excellent way to emphasize how components are isolated and decoupled.

#### When creating related components that are not [composites](#components---composites), write the unique adjective after the primary descriptor.

> Why? This avoids confusion with composite components (more later).

```css
/* avoid */
.fancy-btn {
}

.btn {
}

/* good */
.btn-fancy {
}

.btn {
}
```

**[⬆ Table of Contents](#toc)**

---

### Components - Fragments

Fragments are rules that style a component's child content.

#### Fragments style a component's child/inner content. By default, start a fragment with the component's name and then use additional selectors to select the child content.

Prefer to use the simplest selectors possible that will not overmatch. Each rule should only use one class selector, which is typically the defining rule class selector.

> Isn't this madness? The prevailing wisdom is that using child combinators leads to specificity problems and unmanageable CSS. In my experience, as long as components are well designed, selecting inner content via type simple selectors, combinators, and pseudo-classes results in far less work, code, and complexity. The risk of overmatching is real. But, since CSS gives us a huge array of selectors, we can use them to be very specific as to what we select. Instead of ignoring all of the tools CSS gives us, we should embrace and use them.

```html
<div class="btn">
  <span><svg></svg></span>
</div>
```

```css
.btn {
}

/* fragment - type selector and combinator to target the svg */
.btn svg:first-of-type {
}
```

#### If overmatching is a problem, use specific type selectors, child/sibling combinators, and/or pseudo-classes.

Being very precise with type selectors, combinators, and pseudo-classes can negate overmatching.

```html
<table class="footer">
  <tbody>
    <tr>
      <td>Target</td>
      <td></td>
    </tr>
  </tbody>
</table>
```

```css
.footer {
}

/* fragment - very specific to match exactly - uses a single class selector */
.footer > tbody > tr > td:first-of-type {
}
```

#### A single fragment rule can target one or many document elements.

There does not need to be a mapping between a fragment rule and a single document element.

```html
<table class="footer">
  <tbody>
    <tr>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
```

```css
.footer {
}

/* fragment - matches five elements */
.footer > tbody > tr > td {
}
```

#### Alternatively, a fragment may be defined with a single class selector formed by combining (1) the component name, (2) two underscores (\_\_), and (3) a fragment identifier.

Such fragments are called "named fragments."

> Why use a named fragment? Sometimes, it may not be possible or convenient to select sub-structure using child selectors.

```html
<button class="fancy-button">
  <span class="fancy-button__icon"><svg></svg></span>
</button>
```

```css
.fancy-button {
}

/* named fragment */
.fancy-button__icon {
}
```

#### Named fragments can use additional selectors to select the child content.

In this way, a fragment is a type of sub-component.

> Why? It can be very convenient.

```html
<button class="fancy-button">
  <span class="fancy-button__icon"><svg></svg></span>
</button>
```

```css
.fancy-button {
}

.fancy-button__icon {
}

/* acceptable */
.fancy-button__icon > svg {
}
```

#### Pseudo-elements are child content and are styled by fragments.

```css
.btn {
}

/* fragment - using a pseudo-element selector */
.btn::before {
}

/* fragment - using a pseudo-element selector */
.btn > div::after {
}

/* fragment - using a pseudo-element selector */
.btn table a::first-letter {
}
```

#### Prefer to define fragments in the order they target the component's HTML structure.

```css
.btn {
}

/* avoid - fragments defined out of order */
.btn span:last-child {
}

.btn span:first-child {
}

/* good - fragments defined in the same order they appear in the html */
.btn span:first-child {
}

.btn span:last-child {
}
```

```html
<div class="button">
  <span></span>
  <span>Click Me</span>
</div>
```

#### Fragments can be defined but not used. Fragments can be optional.

In other words, the existence of a fragment does not mean it must be applied to document content. Fragments may be created to accommodate different content structure scenarios for a component.

```css
/* component */
.btn {
}

/* fragment - optional; only applied if the button has an icon */
.btn svg {
}
```

```html
<!-- 
  notice that the fragment was not applied because the button does not have
  an icon 
-->
<div class="button">
  <svg></svg>
</div>
```

#### Prefer to apply a single fragment rule to a content element.

In other words, applying multiple fragments to the same content is discouraged.

> Why? The composition of fragments leads to confusion and specificity issues.

```html
<div class="btn">
  <svg></svg>
</div>
```

```css
/* discouraged - multiple fragments will target the same content */
.btn svg {
}

.btn > * {
}
```

#### If it is difficult to select child content due to deep nesting or any other issue, consider breaking up the component into smaller components.

> Why? Smaller components are easier to comprehend.

**[⬆ Table of Contents](#toc)**

---

### Components - States

This section describes the portion of a component's style rules that are states.

**.--(component)-(state)**<br>
Example: `.--btn-error`

#### States:

- **States are style rules that change the appearance, behavior or any other aspect of a component or a component's child content.**
- **Prefer to define states by using a single class selector formed by combining (1) two hyphens (--), (2) the component name, (3) another hyphen (-), and (4) a state identifier. The state identifier should be a verbal or adjectival word.**
- **Rules that use dynamic pseudo-classes (like `:hover`) are considered state rules.**

States can be applied to any content in a component. This includes content already targeted by another rule, or any content not already targeted by another rule.

Prefer to use the simplest selectors possible that will not overmatch.

> Why prefer verbs and adjectives for state identifiers? States are _actions_ or _modifiers_, just like verbs and adjectives. They represent change on the page.

```css
.btn {
}

/* state */
.--btn-selected {
}

/* state */
.btn:focus {
}
```

#### States are applied to content for styling that may change during runtime.

They should not be used to style default or static styling.

```css
.btn {
}

/* state - added or removed based on whether the button is selected */
.--btn-selected {
}
```

```html
<div class="btn --btn-selected">Click to select</div>
```

#### States rules have the `!important` keyword appended to all property values.

> Why? Since states need higher specificity than composites and extensions (more on these later).

> Isn't `!important` evil? Usage of `!important` within the context of CHESS is safe because the risk of overmatching is contained.

```css
.btn {
}

/* state */
.--btn-selected {
  color: red !important;
}
```

#### States that use pseudo-classes do not require a unique class name.

> Why? An additional class does not help target the content.

```css
/* component */
.btn {
}

/* avoid */
.--btn-focus:focus

/* good - states using pseudo-classes do not require a unique class name */
.btn:focus {
}
```

#### When applying a state to a component's inner content, states can be applied alongside the defining rule and then use selectors to match the inner content, or they can be applied directly to the child content.

Use whatever is more convenient. Sometimes applying the state directly to inner content will be necessary.

Example - Applying the state alongside the defining rule and targeting inner content with selectors.

```css
.--btn-selected svg {
}
```

```html
<button class="btn --btn-selected">
  <svg></svg>
</button>
```

Example - Applying the state directly to the child content.

```css
.--btn-selected {
}
```

```html
<button class="btn">
  <svg class="--btn-selected"></svg>
</button>
```

#### Multiple states can be applied to the same content.

> Specificity problems when using multiple states? Consider creating a new state instead.

#### Define states below fragments.

> Why? Since states can modify fragments or target unstyled content, it can be tricky to determine where to locate them. For simplicity, put them below fragments.

```css
.btn {
}

/* fragment */
.btn svg {
}

/* state */
.btn:focus {
}

/* state */
.--btn-error svg {
}
```

#### In HTML, prefer to write state classes after other classes. If multiple states are applied, sort them by the name of the states alphanumerically, left-to-right.

```css
/* component */
.btn {
}

/* state */
.--btn-selected {
}

/* state */
.--btn-error {
}
```

```html
<!-- 
  avoid - states listed before defining rule and --btn-selected listed before
  --btn-error
-->
<div class="--btn-selected --btn-error btn"></div>

<!-- good -->
<div class="btn --btn-error --btn-selected"></div>
```

**[⬆ Table of Contents](#toc)**

---

### Components - Composites

**.(composite-component)-(composed-component)**<br>
Example: `.toggle-btn`

#### A "composite component" — or simply "composite" — inherits the styling of the component being composed (known as the super component or the composed component).

Composites can add new rules or override existing composed component rules.

- **Define a composite class selector by combining (1) a descriptive name for the composite, (2) a single hyphen, and (3) the component name for the super/composed component.**
- **Start each rule with the composite class selector and the composed component's defining rule.**
- **Apply the class selectors for the composite and the composed component together to HTML content. Write the composite class after the composed component class in HTML.**

See the examples below.

> Why use composites? When creating a composite prevents defining significant amounts of redundant styling. Do not abuse the concept for the same reasons we avoid treacherous class hierarchies when programming.

> How are composites different from states? States are used to change styling at runtime due to user interaction or application state changes. Composites are used to create new components that inherit styling from other components.

````css
```css
/* composed component */
.btn {
}

.btn > span:first-child {
}

.btn__icon {
}

.--btn-hover {
}

/* composite component */
.fancy-btn.btn {
}

/* new composite rule - not overriding */
.fancy-btn::before {
}

/* overriding fragment */
.fancy-btn.btn > span:first-child {
}

/* overriding named fragment */
.fancy-btn .btn__icon {
}

/* overriding state */
.fancy-btn.--btn-hover {
}
````

```html
<!-- apply composite and composed classes -->
<button class="fancy-btn btn"><span></span></button>
```

```css
/* avoid */
.btn.fancy-btn {
}

/* good - composite class selector before composed component's defining rule */
.fancy-btn.btn {
}
```

**[⬆ Table of Contents](#toc)**

---

### Components - Extensions

This section concerns rules where outer components style inner components.

#### Extensions add or override styles for nested components' defining rules.

Create an extension by using the parent component's defining rule to target a child component's defining rule. Extensions can only target an child/inner component's defining rule.

Such rules will typically have two class selectors.

> Why use extensions? A common use case is the parent component supplying layout styles to inner components, or making small styling adjustments.

```css
/* component */
.inner {
}

/* component */
.outer {
}

/* extension */
.outer .inner {
}
```

```html
<div class="outer">
  <div class="inner"></div>
</div>
```

#### Prefer to define extensions in the order they target HTML structure.

Fragments and extensions are both defined in the order they target HTML structure.

States are defined below fragments and extensions.

```html
<div class="parent">
  <span></span>
  <div class="child"></div>
</div>
```

```css
/* avoid - extension should be below the fragment */
.parent {
}

.parent .child {
}

.parent > span {
}

/* good */
.parent {
}

.parent > span {
}

.parent .child {
}
```

#### Do not overmatch content in inner, nested components.

Extensions may only target the defining rule for nested components.

> Why? Breaking a component's encapsulation leads to nondeterministic specificity and styling.

```html
<div class="parent">
  <span></span>
  <div class="child">
    <span></span>
  </div>
</div>
```

```css
/* avoid - fragment overmatches span in the child component */
.parent span 

/* good */
.parent > span;
```

#### Extensions that target inner composites include the class selectors from the composite and the composed component.

> Why? This ensures the necessary specificity.

```css
/* avoid */
.parent > .toggle-button {
}

/* good */
.parent > .toggle-button.button {
}
```

**[⬆ Table of Contents](#toc)**

---

### Media Queries

#### Media break-points are defined at the bottom of a component's styles. Use a mobile-first strategy by writing break-points in ascending `min-width:` order.

Logical break-points are `640px` `768px` `1024px` `1280px` and `1440px`.

> Why mobile-first? Writing break-points in ascending order is more intuitive than descending order.

```css
.btn {
}

.btn:focus {
}

/* after all component rules define break-points */

/* lowest resolution break-point */
@media (min-width: 640px) {
  .btn {
  }

  .btn:focus {
  }
}

@media (min-width: 768px) {
  .btn {
  }

  .btn:focus {
  }
}

@media (min-width: 1024px) {
  .btn {
  }

  .btn:focus {
  }
}

/* ascending order */
@media (min-width: 1280px) {
  .btn {
  }

  .btn:focus {
  }
}
```

#### Prefer to define non-break-point media query rules in the order they are applied to the component's HTML structure.

Also, if they override styles from another rule, define them below the rule they are overriding.

```css
.btn {
}

/* define below the rule */
@media (hover: hover) {
  .btn:hover {
    background: yellow;
  }
}

.btn > div a {
}

/* define below the rule being overridden */
@media only screen and (orientation: landscape) {
  .btn > div a {
  }
}

.btn svg {
}
```

#### When overriding a style rule in a media query, write the overridden rule exactly as it appeared earlier.

> Why? This ensures that the specificity of the media query rule will be greater than the rule it is overriding.

```css
.btn div > div > div:first-of-type {
}

@media (min-width: 1024px) {
  /* write the rule being overridden exactly as above */
  .btn div > div > div:first-of-type {
  }
}
```

**[⬆ Table of Contents](#toc)**

---

### Components - Documenting Components

#### (Optional) Consider documenting a component above its defining rule by writing out its HTML structure and style classes.

Place possible style classes on the nodes that they appear. Only write the opening tag and leave out any attributes. Nested elements can go on the same line or next line.

This step is purely optional.

> Why document? It can quickly provide instruction to the reader on how to use the component.

```css
/* 
  <div hero --hero-selected>
    <div>
      <a --hero-selected>
        <img>
*/
```

#### When documenting a component with inner components, only write the inner component's top-level node and defining rule.

```css
/* 
  <div homepage>
    <div>
      <div hero> --> notice no inner structure or states documented for the nested hero component
    <div>
    <footer>
*/
```

#### Prefer to use regular expressions syntax `*?+{}()|` to denote repeating structure or possible structure.

```css
/*
  <div grid>
    <table>
      <tbody>
        ?<thead> --> 0 or 1 thead
          *<tr> --> tr can be repeated 0 or more times
            <td>
              *<*> --> 0 or more nodes of any tag type
            <td>
              *<img|svg> --> 0 or more img or svg here
        <tbody>
          *<tr><td><td> --> ok to write nested structure on the same line
    {2}<div> --> 2 divs
*/
```

#### Do not stress over the documentation describing all possible HTML states for the component.

It can be very difficult to fully describe all possibilities.

#### Add additional code comments when the purpose of a rule is not clear.

This is often the case with rules that have pseudo-classes.

```css
.btn {
}

/* ripple effect */
.btn:before {
}
```

**[⬆ Table of Contents](#toc)**

---

### Components - Discussion

#### Do not overmatch. Fragments and states must not match content in nested components.

Components are encapsulated. Only a component's defining rule is exposed to external classes for modification.

#### Steps when creating a component:

1. Write the defining class rule.
1. Write the fragment and extension classes if the order they appear in the HTML structure.
1. Write the state classes below the fragments/extensions.

#### When to create extensions or state rules, or a composite:

When the proposed styles are strongly related to a particular parent component and make only minor changes to the child component, create an extension rule for the parent component.

For example, since a rule that positions component A inside a particular container component B is strongly related to the container component and only makes minor styling changes, an extension rule for parent B is appropriate.

Otherwise, create a state rule for the styled component or a new composite component.

#### When to create state rules or create a composite:

If a rule is dynamic and applied indeterminately — it could be due to user interaction like a click — then a state is appropriate.

If there are multiple rules, or if the rules effectively create a fundamentally different component, create a composite.

**[⬆ Table of Contents](#toc)**

---

### App Overrides

This section is for defining style rules that are designed to add or override style properties that were defined earlier. They `override` the earlier-defined styles at an application level.

#### App overrides are intended to be used by the people assembling an application, not for libraries or default styles. In other words, app overrides are application-specific.

Do not place default styling in this section.

> Why not? Default styling often requires changes. Since styling in this section is not intended to be overridden by another project, default styling in this section should be avoided.

#### App overrides can be useful to change variable values defined earlier in the variables section.

> Why? This can be useful to create application-specific themes without changing default variable values. Perhaps, the developer cannot change the default values.

```css
/* default variables */
:root {
  --button-color: red;
}

.btn {
  color: var(--button-color);
}

/* app overrides */

/* new blue sub-theme */
:root {
  --button-color: blue;
}
```

#### App overrides can override existing styles or add additional styles. In other words, style rules in this section use the same selectors as the rule they are overriding and apply new styles by either overriding existing styles or adding new ones.

App overrides in this section can change the styling for any section that came before: resets, globals, components, utilities, and everything else are available for overriding in this section. App overrides can also be used to theme an application.

> When are app overrides appropriate? When one cannot change the HTML or CSS for a document or library, or creating new components is not an option. In such a case, app overrides are the only way to change the styling of the document. Library code with hard-coded HTML and CSS is a good use case for when app overrides are appropriate.

> Why not just use CSS custom properties? While custom properties work to create themes, it can become oppressive, verbose, and inflexible to add a custom property for every style that may need to change. Also, CSS custom properties can not be used to add styles that are not already defined, unlike app overrides that can add styles that were not anticipated earlier.

```css
/* component: button */
.btn {
  color: red;
}

/* app overrides section */
.btn {
  color: blue; /* overriding color */
  text-align: center; /* app overrides can also add new styles like text-align */
}
```

**[⬆ Table of Contents](#toc)**

---

### Animations

#### Prefer to give an animation the same name as the class that uses it. Define the animation immediately following the rule that uses it. If more than one rule uses the animation then define the animation after the first rule that uses it.

Many rules that use animations will be component states.

```css
/* discouraged */
@keyframes spinning {
}

.--btn-spinning {
  animation-name: spinning;
}

/* 
  preferred - animation has the same name and defined after the class that uses 
  it
*/
.-btn-spinning {
  animation-name: btn-spinning;
}

@keyframes btn-spinning {
}
```

**[⬆ Table of Contents](#toc)**

---

## Miscellaneous

### F.A.Q.

> Is CHESS flexible with regards to its rules?

Yes, be as flexible as necessary. An inflexible approach towards CSS is doomed to fail.

> What about id selectors, e.g., `#image`?

Avoid id selectors for styling because their specificity is so high that CHESS's class-selector-based methodology falls apart.

> How does one solve CSS specificity problems?

If in a pinch, an easy way to raise CSS specificity is by repeating class selectors.

```css
/* component */
.btn {
}

/* simple technique to increase specificity - repeat class selector */
.btn.btn {
}

.btn.btn--focused {
}
```

> What about selector performance?

Do not concern yourself with selector performance. The runtime difference between optimized and unoptimized CSS is nearly always inconsequential.

### Comparison To BEM

Prominent differences between CHESS and BEM:

- BEM frowns on resets.
- BEM discourages elements, attribute selectors, and global selectors.
- BEM leads to larger style sheets due to its more atomic naming schema.

Initially, CHESS used the BEM terms _block_ and _element_. Ultimately, CHESS dropped these terms because they were confusing since block and element already have special meaning in CSS, HTML, and JavaScript. See block ([HTML](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements), [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Syntax#CSS_declaration_blocks), [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block)) and element ([HTML](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)).

### Comparison to Tailwind.

Tailwind is a utility-first paradigm that eschews component-based design.

I like Tailwind, and I understand the motivation. However, I prefer thinking of documents as being composed of composable blocks rather than building up structure with loads of small classes. Especially for SPA projects.

**[⬆ Table of Contents](#toc)**

---

## Epilogue

Perhaps the single most crucial point to take from any style guide is to be consistent. When an abrupt style change occurs, it can cause a cognitive break that throws a developer out of their groove. Try to be consistent!

I hope you found this guide to be helpful. Even if you do not agree with a single statement contained herein, I am willing to bet it got the juices flowing.

Thanks for reading!

\- Jake Knerr

**[⬆ Table of Contents](#toc)**

---
