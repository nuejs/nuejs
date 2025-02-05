
# Content in Nue

The web platform excels at sharing content, but modern development practices often overcomplicate this fundamental strength. By keeping content separate from presentation, we enable:

1. **Direct content updates**. Change content through Markdown without rebuilding application code.

2. **Clear separation of concerns**. Content teams work with clean Markdown while developers handle implementation.

3. **Content team autonomy**. Update landing pages, documentation, and blog posts directly.

4. **Simpler architecture**. Most content-heavy sites need minimal JavaScript.

5. **Natural scalability**. As content grows, the system stays maintainable without complex state management.

Instead of mixing content with application code or framework-specific markup, Nue provides a Markdown syntax that enables rich, interactive documents while maintaining pure content structure.


### What makes Nue Markdown special

Nue's content system differs from traditional Markdown approaches:

1. **Structural understanding**. The parser automatically recognizes document structure and creates semantic sections from your headings – no manual markup needed. This enables sophisticated styling without polluting content.

2. **Rich layouts without markup**. Create sophisticated grid layouts, stacks, and nested structures using a simple block system that builds on the natural document structure.

3. **Interactive elements through built-in tags**. Add dialogs, tabs, and accordions using Nue's native browser-based components.

4. **Custom components when needed**. Create your own server-side, client-side, or isomorphic tags for advanced functionality like image galleries or forms.


### Key principles

1. Content is wrapped with [layout modules](layout.html) to create the final HTML document structure including headers, footers, and potential sidebars.

2. The generated HTML is [styled](styling.html) with an external design system, keeping presentation completely separate from content.



## Sections
Modern web content often requires sophisticated layouts, but manually wrapping content in divs or custom HTML is messy and hard to maintain. Nue solves this by analyzing your content structure and generating semantic HTML sections.

You can enable automatic sectioning by adding `sections: true` to your page configuration. When enabled, Nue treats each level 2 heading (`##`) as the start of a new section:

```md
---
sections: true
---

## Introduction
The first section content...

## Key features
The second section content...

## Technical details
The third section content...
```

This creates semantic HTML sections:

```html
<article>
  <section>
    <h2>Introduction</h2>
    <p>The first section content...</p>
  </section>

  <section>
    <h2>Key features</h2>
    <p>The second section content...</p>
  </section>

  <section>
    <h2>Technical details</h2>
    <p>The third section content...</p>
  </section>
</article>
```

### Section classes

You can assign classes to sections for styling purposes by providing an array:

```md
---
sections: [hero, features, details]
---
```

This generates sections with corresponding class names:

```html
<article>
  <section class="hero">...</section>
  <section class="features">...</section>
  <section class="details">...</section>
</article>
```

### Manual sections

For more granular control, you can use triple pluses (`+++`) to explicitly create section breaks:

```md
First section content...

+++

Second section content...

+++

Third section content...
```

This is particularly useful for landing pages or marketing content where visual sections may not align with heading structure.


## Blocks, grids, and flex layouts

Blocks are Nue's system for creating rich layouts without HTML markup. They generate clean HTML that your CSS can style:

### Basic blocks

A block is created with square brackets and a class name:

```md
[.note]
  ### Important
  This is a highlighted note with a title
```

This generates semantic HTML:

```html
<div class="note">
  <h3>Important</h3>
  <p>This is a highlighted note with a title</p>
</div>
```

### Grid layouts

Create grid layouts using headings as separators:

```md
[.grid]
  ### First feature
  Description of the first feature

  ### Second feature
  Description of the second feature

  ### Third feature
  Description of the third feature
```

This generates:

```html
<div class="grid">
  <div>
    <h3>First feature</h3>
    <p>Description of the first feature</p>
  </div>
  <div>
    <h3>Second feature</h3>
    <p>Description of the second feature</p>
  </div>
  <div>
    <h3>Third feature</h3>
    <p>Description of the third feature</p>
  </div>
</div>
```

You can also create more flexible grid layouts using triple plus as separator:

```md
[.grid]
  [image feature-1.jpg]
  First feature description

  +++

  [image feature-2.jpg]
  Second feature description

  +++

  [image feature-3.jpg]
  Third feature description
```

This generates:

```html
<div class="grid">
  <div>
    <figure>
      <img src="feature-1.jpg" loading="lazy">
    </figure>
    <p>First feature description</p>
  </div>
  <div>
    <figure>
      <img src="feature-2.jpg" loading="lazy">
    </figure>
    <p>Second feature description</p>
  </div>
  <div>
    <figure>
      <img src="feature-3.jpg" loading="lazy">
    </figure>
    <p>Third feature description</p>
  </div>
</div>
```

The layout is handled entirely through CSS:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}
```

### Stack layouts

Stack layouts vertically arrange elements with consistent spacing:

```md
[.stack]
  ### Design
  Focus on systematic design

  ### Engineering
  Built for performance

  ### Content
  Pure content structure
```

The generated HTML:

```html
<div class="stack">
  <div>
    <h3>Design</h3>
    <p>Focus on systematic design</p>
  </div>
  <div>
    <h3>Engineering</h3>
    <p>Built for performance</p>
  </div>
  <div>
    <h3>Content</h3>
    <p>Pure content structure</p>
  </div>
</div>
```

With CSS handling the vertical spacing:

```css
.stack {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
```

### Nested blocks

Blocks can be nested for more complex layouts:

```md
[.feature]
  ## Main feature
  Feature description

  [.grid]
    ### Sub-feature one
    First sub-feature

    ### Sub-feature two
    Second sub-feature
```

Generates:

```html
<div class="feature">
  <h2>Main feature</h2>
  <p>Feature description</p>

  <div class="grid">
    <div>
      <h3>Sub-feature one</h3>
      <p>First sub-feature</p>
    </div>
    <div>
      <h3>Sub-feature two</h3>
      <p>Second sub-feature</p>
    </div>
  </div>
</div>
```

The parser maintains this clean structure while your CSS handles all visual presentation.


## Basic Markdown support

Nue fully supports standard Markdown, providing a familiar foundation for content creation. While the syntax is straightforward, Nue's implementation ensures your Markdown content remains clean and semantic.

### Common formatting

- **Headings** with `#` symbols (`# H1`, `## H2`, `### H3`)
- **Emphasis** with asterisks (`*italic*`, `**bold**`)
- **Lists** with hyphens or numbers
- **Links** with `[text](url)` syntax
- **Code** with backticks or fenced blocks
- **Blockquotes** with `>` symbol

For example:

```md
# Main heading

This paragraph has **bold** and *italic* text, plus a `code snippet`.

## Second level heading

- First list item
- Second list item
  - Nested item
  - Another nested item

1. Ordered list
2. Second ordered item

> This is a blockquote that can contain multiple paragraphs
> and *formatting*.

[Link to docs](https://docs.example.com)
```

### Code blocks

Fenced code blocks use triple backticks, with optional language hints for syntax highlighting:

```md
\```js
function example() {
  return "Hello world"
}
\```
```

### Why this matters

While these Markdown features are standard, Nue's implementation focuses on generating clean, semantic HTML that serves as a foundation for your design system. There's no framework-specific markup or special processing – just pure content that directly expresses meaning.

For complete Markdown syntax reference, see the [original Markdown specification](https://daringfireball.net/projects/markdown/). Nue builds on this foundation with powerful extensions covered in the following sections.



## Extended Markdown syntax

Nue builds on standard Markdown by supporting common extensions and adding powerful features that enhance content creation while maintaining purity.

### Standard extensions

Standard Markdown lacks certain features needed for complex content. Nue includes the most commonly used extensions out of the box.

Tables help present structured data in a readable format:

```md
| Feature   | Description           | Status  |
|-----------|----------------------|---------|
| Grid      | Layout system        | Done    |
| Sections  | Document structure   | Done    |
| Themes    | Design systems       | Soon    |
```

Footnotes allow you to add references or asides without disrupting content flow:

```md
Here's a statement that needs clarification[^1].

[^1]: This is the footnote content providing more detail.
```

### Enhanced formatting

Nue provides additional formatting options that maintain semantic meaning while providing more precise control:

[table]
  Markdown  | HTML | Example
  ------
  `I'm **bold**`   | `<strong>bold</strong>`    | I'm **bold**
  `I'm __bold__`   | `<strong>bold</strong>`    | I'm __bold__
  `I'm •bold•`     | `<b>bold</b>`              | I'm •bold•
  `I'm *italic*`   | `<em>italic</em>`          | I'm *italic*
  `I'm _italic_`   | `<em>italic</em>`          | I'm _italic_
  `I'm /italic/`   | `<i>italic</i>`            | I'm /italic/
  `I'm \`code\``   | `<code>code</code>`        | I'm `code`
  `I'm ~striked~`  | `<s>striked</s>`           | I'm ~striked~
  `I'm "quoted"`   | `<q>quoted</q>`            | I'm "quoted"
  `I'm |marked|`   | `<mark>marked</mark>`      | I'm |marked|

### Variables

Embed dynamic values directly in your content:

```md
Current version: { site.version }
Last updated: { page.date }
Author: { meta.author }
```

This keeps content dynamic without resorting to template languages or JavaScript.

### Heading extensions

Add IDs and classes to headings for linking and styling:

```md
## Less is More { #less.minimalist }
## Another heading { #custom-id }
```

This enables direct linking to sections while keeping CSS classes separate from content.

### Enhanced footnotes

Reference entire phrases instead of just numbers:

```md
[Separation of Concerns][^soc] is fundamental to Nue.
Progressive Enhancement[^pe] ensures accessibility.

[^soc]: Keeping content, structure and style separate
[^pe]: Building from core functionality up
```

### Why these extensions matter

Each extension serves a specific purpose in creating better content:
- IDs and classes enable styling without HTML pollution
- Variables keep content dynamic and maintainable
- Enhanced formatting provides semantic precision
- Extended footnotes improve documentation clarity

Together, they enable sophisticated content creation while maintaining the purity and portability of your Markdown files.
