# Day 4 — CSS Learnings

## What I Learned

Today I learned the basics of CSS and how it is used to make a web page visually attractive. HTML provides the structure of a webpage, while CSS controls its appearance, including colors, spacing, layouts, fonts, and positioning.

### 1. Why CSS is Important

* HTML defines the content and structure.
* CSS improves the presentation and user experience.
* Separating HTML and CSS makes code cleaner and easier to maintain.

---

## Ways to Add CSS

There are three methods of adding CSS:

1. **Inline CSS**

   * Applied directly inside an HTML element using the `style` attribute.
   * Suitable only for small changes.

2. **Internal CSS**

   * Written inside a `<style>` tag in the HTML document.
   * Useful for single-page websites.

3. **External CSS**

   * Written in a separate `.css` file and linked using the `<link>` tag.
   * Best practice because it keeps HTML clean and allows CSS to be reused across multiple pages.

Example:

```html
<link rel="stylesheet" href="./style.css">
```

---

## CSS Selectors

I learned different ways to target HTML elements.

* Element Selector
* Class Selector
* ID Selector
* Grouping Selector

Selectors help CSS identify which elements should receive styling.

---

## Browser Developer Tools

I learned how to inspect HTML elements using the browser's **Developer Tools**.

Developer Tools help to:

* Inspect the DOM
* View applied CSS
* Test styles live without modifying files
* Debug layout issues

---

## File Paths

To connect an external stylesheet correctly, relative paths are used.

Example:

```html
<link rel="stylesheet" href="./style.css">
```

The `./` represents the current folder.

---

## CSS Learned While Styling My Resume

I applied CSS to my resume project and learned the following properties:

### Layout

```css
.Resume-container{
    display:flex;
    gap:30px;
}
```

* `display: flex` creates a flexible two-column layout.
* `gap` adds spacing between the sidebar and main content.

---

### Sidebar Styling

```css
aside{
    width:320px;
    background-color:rgb(60,21,35);
    color:azure;
    padding:20px;
    flex-shrink:0;
}
```

I learned:

* `width`
* `background-color`
* `color`
* `padding`
* `flex-shrink`

This keeps the sidebar fixed while the main content adjusts automatically.

---

### Main Content

```css
main{
    flex-shrink:1;
}
```

The main section can shrink if necessary to fit the available screen space.

---

### Typography

```css
h1{
    font-size:xxx-large;
}

h2{
    font-size:xx-large;
}

h3{
    font-size:larger;
}

p{
    font-size:large;
}

ul{
    font-size:large;
}
```

I learned how different font sizes improve readability and create a visual hierarchy.

---

### Margin

```css
body{
    margin:0;
}
```

Removing the default browser margin allows better control over page layout.

---

### Horizontal Rule

```css
hr{
    margin-bottom:30px;
}
```

Spacing below the horizontal line improves readability between sections.

---

## Key Concepts I Understood

* CSS is responsible for presentation, while HTML provides structure.
* External CSS is the preferred method for styling webpages.
* Flexbox makes creating layouts much easier.
* CSS selectors target specific HTML elements.
* Developer Tools help inspect and debug webpages.
* Relative file paths are important for linking external resources correctly.
* Small CSS properties like `padding`, `margin`, `font-size`, and `background-color` significantly improve the appearance of a webpage.

---
