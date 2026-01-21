# Love and London/Paris Shopify Website Guide

## Overview

This document serves as a guide for the updated website files for the Love and London/Paris Shopify websites. This repository includes all files that have been added, updated, or changed.

While this repository holds the code, the notes below are designed to help you understand what you are looking at within the Shopify Theme Editor and the code files themselves.

---

## 1. Decoding the "Code" (A Glossary)

If you find yourself looking at the raw code, you might see symbols or tags that look confusing. Here is a cheat sheet on what they mean and what they do.

### The difference between `<style>` and `<script>`

Think of a webpage like a house. HTML is the bricks and mortar—it provides the structure.

* **`<style>` (CSS):** This is the interior design. It tells the browser what colour the walls are, how big the windows should be, and where the furniture sits. If you see code inside `<style>` tags, it is strictly for **visuals**.
* **`<script>` (JavaScript):** This is the electricity and plumbing. It handles functionality—like what happens when you click a button, how a slider moves, or how a pop-up appears. If you see code inside `<script>` tags, it is strictly for **behaviour**.

### Comments and Notes

Developers leave notes in the code to explain what complex sections do. These notes are invisible to the customer on the live website.

* `{%- comment -%}`: **This is a Liquid Comment.** Anything between these tags is completely ignored by Shopify. We use these to leave instructions or to temporarily "hide" a piece of code without deleting it.
* `/* Text */`: **This is a CSS Comment.** You will see this inside `<style>` blocks. It usually explains what a specific block of styling is doing (e.g., `/* Adjusts mobile padding */`).

---

## 2. Understanding the Hashtag (#) Syntax

You will often see the hashtag symbol in the code or settings. In CSS (styling), the hashtag represents an **ID Selector**.

It is used to target a single, unique HTML element on the page. In Shopify Liquid templates, this is commonly combined with dynamic code to create **Scoped IDs**:

`#Name-{{ section.id }}`

### Breakdown:

* **`#` (ID Selector):** Tells the browser to style a specific element based on its unique `id` attribute.
* **`{{ section.id }}` (Dynamic Liquid):** A placeholder that Shopify automatically replaces with a unique alphanumeric string for that specific section instance.

### Why is this used?
This pattern ensures **style isolation**. If you add the same section (e.g., a Slideshow) to a page three times, each instance gets a unique ID (e.g., `#Slider-123`, `#Slider-456`). This allows the CSS to apply only to that specific slideshow instance without breaking the layout of the others.

---

## 3. Using "Custom Liquid" in the Editor

In the Shopify Theme Editor (Customise), you have the option to add a section called **Custom Liquid**.

### What is it for?
This section is a "blank canvas" that allows you to drop code snippets directly onto a page without hiring a developer.

### Common Use Cases:
* **App Embeds:** Adding code for a booking form, a newsletter popup, or a third-party review widget.
* **Liquid Logic:** Displaying dynamic data, such as the current product title: `{{ product.title }}`.

### Best Practices:
* **Don't overcomplicate it:** If you are pasting hundreds of lines of code here, it belongs in the theme files, not the editor.
* **Check Mobile:** Content added here might look different on mobile devices. Always toggle the mobile view in the editor to double-check.

---

## 4. Storefront Custom CSS: Limitations & Guide

> **Note:** These are the full limitations of the storefront custom CSS section if changes are needed.

These limitations apply to the "Custom CSS" fields found in two places:
1.  **Section-Level:** Inside a specific section (e.g., a "Rich Text" or "Product Information" section).
2.  **Theme-Level:** Under *Theme Settings > Custom CSS* (meant for global changes).

### Hard Character Limits
The most immediate roadblock users hit is the character count.
* **Note:** Spaces and line breaks count toward this limit.
* **Solution:** If you hit the limit, you will need to minify your CSS (remove spaces/newlines) or move the code to the theme's asset files.

### Scoping & Selector Limitations
Shopify attempts to "scope" your CSS so it does not accidentally ruin other parts of your site.

* **The `selector` Keyword:** In Section-Specific CSS, you can use the keyword `selector` to target the wrapper of that specific section.
    * *Example:* `selector { padding-top: 20px; }` applies padding only to that section instance.
* **Targeting "Outside" Elements:** You generally *cannot* style elements outside the section you are editing. For example, if you are in a "Featured Product" section, you cannot write CSS to change the "Header" navigation.
* **Checkout Page:** The Custom CSS field does not apply to the Checkout page. The Checkout is locked down for security (unless you are on Shopify Plus and use Checkout Extensibility).

### Syntax & "At-Rule" Restrictions
The editor does not support the full breadth of CSS features found in a standard `.css` file.

* **Prohibited At-Rules:** You cannot use `@import`, `@charset`, or `@namespace`. These will often result in a saving error or be stripped out.
* **Allowed At-Rules:** You can use `@media` (for mobile responsiveness), `@container`, `@layer`, and `@supports`.
* **No SCSS/Sass:** You cannot use nesting (e.g., `parent { child { color: red } }`). You must use standard CSS syntax (e.g., `parent child { color: red }`).

### The `content` Property Bug (JSON Conflict)
This is a common "gotcha" for developers and editors alike. If you try to use pseudo-elements like `::before` or `::after` with the content property (e.g., `content: "";`), the editor may fail to save.

* **The Reason:** The theme settings are saved as a JSON file in the backend. Sometimes, the quotation marks in `content: "value"` conflict with the JSON formatting, causing the save to fail.
* **The Fix:** You usually have to move this specific code to your main theme stylesheet (*Edit Code > Assets > base.css* or *theme.css*) rather than using the editor's Custom CSS box.

### Performance Implications
While convenient, relying heavily on these boxes can create "technical debt."
* **Rendering:** CSS added here is often rendered inline or in a separate block that loads *after* the main stylesheet.
* **Bloat:** If you have 50 different sections all with custom 500-character CSS blocks, it can bloat the DOM (the page structure) and make future debugging difficult.

---

## Summary of Best Practices

| Action | Context |
| :--- | :--- |
| ✅ **Use the Editor for** | Quick, isolated fixes (e.g., "Make this specific button blue"). |
| ❌ **Use Theme Files for** | Anything involving `@font-face`, complex animations, pseudo-elements (`::before`/`::after`), or code exceeding 500 characters. |
