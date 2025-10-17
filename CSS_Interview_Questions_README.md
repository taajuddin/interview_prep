# 🎨 CSS Interview Questions and Answers

A comprehensive collection of **CSS interview questions** with concise explanations — covering beginner to advanced levels.

---

## 🟢 Basic CSS Questions

### 1. What is CSS?
CSS (Cascading Style Sheets) is used to style and layout web pages — controlling colors, fonts, spacing, and responsiveness.

---

### 2. What are the different types of CSS?
1. **Inline CSS** — inside HTML elements using `style` attribute.  
2. **Internal CSS** — inside a `<style>` tag within `<head>`.  
3. **External CSS** — in a separate `.css` file linked via `<link>`.

---

### 3. Difference between `class` and `id` selectors?
- **class**: reusable; starts with `.`  
- **id**: unique; starts with `#`

---

### 4. What does “Cascading” mean in CSS?
It refers to the priority order in which multiple styles are applied: inline → internal → external → browser default. The most specific or latest rule wins.

---

### 5. Explain CSS Positioning.
| Position | Description |
|-----------|--------------|
| **static** | default; normal flow |
| **relative** | positioned relative to its normal position |
| **absolute** | positioned relative to nearest positioned ancestor |
| **fixed** | positioned relative to viewport |
| **sticky** | toggles between relative and fixed |

---

## 🟡 Intermediate CSS Questions

### 6. What are pseudo-classes and pseudo-elements?
- **Pseudo-class:** defines a state, e.g. `:hover`, `:focus`  
- **Pseudo-element:** styles a specific part, e.g. `::before`, `::after`

---

### 7. What are CSS combinators?
- `div p` → descendant  
- `div > p` → direct child  
- `div + p` → next sibling  
- `div ~ p` → general sibling  

---

### 8. What are media queries?
Used for **responsive design** — applying styles based on device width, height, or orientation.
```css
@media (max-width: 768px) {
  body {
    background-color: lightgray;
  }
}
```

---

### 9. Difference between `display: none` and `visibility: hidden`?
- `display: none` → removes element completely from layout.  
- `visibility: hidden` → hides element but retains its space.

---

### 10. What are Flexbox and Grid?
- **Flexbox** → one-dimensional layout (rows or columns).  
- **Grid** → two-dimensional layout (rows + columns).

---

## 🔵 Advanced CSS Questions

### 11. Explain CSS Box Model.
Every element is treated as a box with:  
`content + padding + border + margin`

---

### 12. Difference between `em`, `rem`, `%`, and `px` units?
| Unit | Relative To | Description |
|-------|--------------|-------------|
| px | none | fixed pixel size |
| em | parent’s font size | scalable |
| rem | root’s font size | consistent |
| % | parent element’s size | flexible |

---

### 13. What is Specificity in CSS?
Determines which rule takes priority when multiple selectors target the same element.

Order: Inline > ID > Class/Pseudo-class > Element/Pseudo-element

---

### 14. What is z-index?
Defines the **stacking order** of elements. Higher `z-index` values appear above lower ones. Works only on positioned elements.

---

### 15. What are transitions and animations?
- **Transition:** smooth change between states.  
- **Animation:** allows keyframe-based motion.

---

### 16. What are CSS Variables?
Reusable custom properties that make styling consistent.
```css
:root {
  --main-color: #007bff;
}
button {
  background-color: var(--main-color);
}
```

---

### 17. Difference between inline, block, and inline-block elements?
| Type | Description |
|------|--------------|
| inline | fits content width |
| block | takes full width |
| inline-block | inline layout with set width/height |

---

### 18. Difference between `nth-child()` and `nth-of-type()`?
- `nth-child()` → selects nth child regardless of type.  
- `nth-of-type()` → selects nth element of a given type.

---

### 19. Difference between `opacity` and `rgba()` transparency?
- `opacity` affects entire element including children.  
- `rgba()` affects only background or color transparency.

---

### 20. Explain the CSS Box Sizing property.
`box-sizing: border-box` ensures padding and border are included in total element width/height.

---

📘 **Author:** Md Taaj Uddin  
📅 **Last Updated:** October 2025  
🧑‍💻 **Tech Stack:** React | CSS | JavaScript | Node.js  

> ⭐ Star this repo if you found it helpful!
