# Love-and-Paris
The updated website files for Love and Paris Shopify Websites. This repo includes all files that have been added, updated or changed. 

***General Notes*** 

Understanding the Hashtag (#) Syntax

In CSS, the hashtag (`#`) represents an **ID Selector**. It is used to target a single, unique HTML element on the page.
In Shopify Liquid templates, this is commonly combined with dynamic code to create **Scoped IDs**:
`#Name-{{ section.id }}`

Breakdown:
1. **`#` (ID Selector):** Tells the browser to style a specific element based on its `id` attribute.
2. **`{{ section.id }}` (Dynamic Liquid):** A placeholder that Shopify replaces with a unique alphanumeric string for that specific section instance.

Why is this used?
This pattern ensures **style isolation**. 
If you add the same section (e.g., a Slideshow) to a page three times, each instance gets a unique ID (e.g., `#Slider-123`, `#Slider-456`). This allows the CSS to apply only to the intended section without breaking the layout of the others.


*** STORE FRONT CUSTOM CSS ***
NOTE: These are the full limitations of the storefront custom css section if changes are needed:
These limitations apply to the "Custom CSS" fields found in two places:
- Section-Level: Inside a specific section (e.g., a "Rich Text" or "Product Information" section).
- Theme-Level: Under Theme Settings > Custom CSS (meant for global changes).

Here is a breakdown of the specific limitations for each.
- Hard Character LimitsThe most immediate roadblock users hit is the character count.
Note: Spaces and line breaks count toward this limit. If you hit the limit, you will need to minify your CSS (remove spaces) or move the code to the theme's assets files.

Scoping & Selector Limitations
Shopify attempts to "scope" your CSS so it doesn't accidentally ruin other parts of your site.
- The selector Keyword: In Section-Specific CSS, you can use the keyword selector to target the wrapper of that specific section.
Example: selector { padding-top: 20px; } applies only to that section instance.
- Targeting "Outside" Elements: You generally cannot style elements outside the section you are editing. If you are in a "Featured Product" section, you cannot write CSS to change the "Header" navigation.
- Checkout Page: The Custom CSS field does not apply to the Checkout page. The Checkout is locked down for security (unless you are on Shopify Plus and use Checkout Extensibility).

Syntax & "At-Rule" Restrictions
The editor does not support the full breadth of CSS features found in a standard .css file.
- Prohibited At-Rules: You cannot use @import, @charset, or @namespace.
These will often result in a saving error or be stripped out.
Allowed At-Rules: You can use @media (for mobile responsiveness), @container, @layer, and @supports.
- No SCSS/Sass: You cannot use nesting (e.g., parent { child { color: red } }). You must use standard CSS syntax (e.g., parent child { color: red }).

The content Property Bug (JSON Conflict)
This is a common "gotcha" for developers. If you try to use pseudo-elements like ::before or ::after with the content property (e.g., content: "";), the editor may fail to save.
- The Reason: The theme settings are saved as a JSON file in the backend. Sometimes, the quotation marks in content: "value" conflict with the JSON formatting, causing the save to fail.
- The Fix: You usually have to move this specific code to your main theme stylesheet (Edit Code > Assets > base.css or theme.css) rather than using the editor's Custom CSS box.

Performance Implications
While convenient, relying heavily on these boxes can create "technical debt.
- "Rendering: CSS added here is often rendered inline or in a separate block that loads after the main stylesheet. If you have 50 different sections all with custom 500-character CSS blocks, it can bloat the DOM and make future debugging difficult.

Summary of Best Practices
- Use the Editor for: Quick, isolated fixes (e.g., "Make this specific button blue").
- Use Theme Files for: Anything involving @font-face, complex animations, pseudo-elements (::before/::after), or code exceeding 500 characters.
