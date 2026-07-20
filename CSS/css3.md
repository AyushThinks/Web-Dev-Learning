# Day 5 Learning Notes

## Flexbox

Flexbox is a one-dimensional layout system used to align and distribute items either in a row or a column. It makes it easier to build responsive layouts without using floats or positioning.

### Common Flexbox Properties

| Property | Purpose |
|----------|---------|
| `display: flex;` | Creates a flex container |
| `flex-direction` | Sets row or column layout |
| `justify-content` | Aligns items horizontally |
| `align-items` | Aligns items vertically |
| `gap` | Adds spacing between flex items |
| `flex-wrap` | Allows items to wrap to the next line |
| `flex-grow` | Allows an item to grow |
| `flex-shrink` | Allows an item to shrink |

### Example

```css
.container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
}
```

---

# CSS Grid

CSS Grid is a two-dimensional layout system used to create rows and columns simultaneously. It is ideal for complex page layouts.

### Common Grid Properties

| Property | Purpose |
|----------|---------|
| `display: grid;` | Creates a grid container |
| `grid-template-columns` | Defines columns |
| `grid-template-rows` | Defines rows |
| `gap` | Space between rows and columns |
| `grid-column` | Controls column span |
| `grid-row` | Controls row span |

### Example

```css
.container {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 20px;
}
```

---

# Flexbox vs Grid

| Flexbox | Grid |
|---------|------|
| One-dimensional layout | Two-dimensional layout |
| Works with rows **or** columns | Works with rows **and** columns |
| Best for components | Best for complete page layouts |
| Easier for alignment | Better for complex layouts |
| Navigation bars, cards, buttons | Dashboards, galleries, web pages |

### When to Use Flexbox

Use Flexbox when:

- Aligning items in a row
- Creating navigation bars
- Building cards
- Centering content
- Sidebar layouts
- Small UI components

### Example

```css
nav {
    display: flex;
    justify-content: space-between;
}
```

---

### When to Use Grid

Use Grid when:

- Creating complete page layouts
- Building image galleries
- Dashboard layouts
- Magazine-style layouts
- Multiple rows and columns

### Example

```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
}
```

---

# Media Queries

Media Queries allow CSS to apply different styles depending on the screen size or device.

They help create responsive websites that work properly on desktops, tablets, and mobile phones.

## Syntax

```css
@media (max-width: 768px) {

}
```

---

## Why Use Media Queries?

Without Media Queries:

- Layout breaks on small screens.
- Images overflow.
- Text becomes difficult to read.
- Horizontal scrolling appears.

With Media Queries:

- Layout adapts automatically.
- Images resize correctly.
- Font sizes become readable.
- Better user experience across all devices.

---

## Common Breakpoints

| Device | Width |
|---------|-------|
| Desktop | Above 992px |
| Tablet | 768px – 992px |
| Mobile | Below 768px |
| Small Mobile | Below 480px |

---

## Example 1

Desktop

```css
.Resume-container {
    display: flex;
}
```

Mobile

```css
@media (max-width:768px){

    .Resume-container{
        flex-direction:column;
    }

}
```

---

## Example 2

Desktop

```css
aside{
    width:20%;
}

main{
    width:70%;
}
```

Mobile

```css
@media(max-width:768px){

    aside,
    main{
        width:100%;
    }

}
```

---

## Example 3

Desktop

```css
img{
    width:220px;
}
```

Mobile

```css
@media(max-width:480px){

    img{
        width:150px;
    }

}
```

---

## Author

**Ayush Joshi**