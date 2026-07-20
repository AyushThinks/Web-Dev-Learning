# Day 5 — CSS Learnings

## What I learned today

### 1. Margin & Padding
Today I understood the core box model concepts properly:
- **Margin** — space outside an element, between it and other elements.
- **Padding** — space inside an element, between its content and its border.
- Used both to fix spacing issues in my resume — like the gap between sections, and around my profile photo.

### 2. Centering an Image
I learned how to center an image properly using CSS instead of it being randomly aligned. Used `text-align: center` on the parent `<figure>` tag, and set the image as a block element with `margin: 0 auto` to center it within its container.

### 3. Applying CSS to My Resume
Took my existing resume HTML and added real styling to it — spacing, layout structure (sidebar + main content), borders, and hover effects. This made the resume look much more professional compared to plain unstyled HTML.

### 4. Enhancv (Reference Website)
We discussed Enhancv, a resume-building website, as a reference for what good resume layouts and design choices look like — clean sections, good spacing, clear hierarchy. Useful to compare against while styling my own resume.

### 5. Google Fonts
Learned how to add custom fonts using Google Fonts instead of relying on default system fonts.

**Steps to add a Google Font:**
1. Go to [fonts.google.com](https://fonts.google.com) and pick a font.
2. Copy the `<link>` tag provided.
3. Paste it inside the `<head>` of the HTML file:
   ```html
   <link href="https://fonts.googleapis.com/css2?family=FontName&display=swap" rel="stylesheet">
   ```
4. Apply it in CSS using `font-family`:
   ```css
   body {
     font-family: 'FontName', sans-serif;
   }
   ```

### 6. Image Size & Page Load Speed
Discussed how large image files slow down webpage loading time. A bigger profile photo file size = slower load, especially on slower internet connections.

**Fix — used WebP format:**
- WebP compresses images much better than JPG/PNG while keeping good quality.
- Reduced my profile photo's file size using WebP, which improved load time.
- Also used `width` and `height` attributes on the `<img>` tag so the browser reserves space for the image before it fully loads (avoids layout shift).

## Summary
Today was mainly about applying real CSS to my resume project — spacing with margin/padding, centering elements properly, using custom fonts via Google Fonts, and optimizing image size with WebP for better performance.