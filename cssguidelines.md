##Goal of CSS Guidelines
Below are Best Practices and Guidelines that the Enterprise software UX group intends to use going forward. The goal of these guidelines is to keep our stylesheets scalable and maintainable. This is a live document and is open to suggestions and change. Content borrowed with love from [Code Guide by Mark Otto](http://codeguide.co/) and [CSS Guidelines from Harry Roberts](http://cssguidelin.es/) and elsewhere. Check those out for more great CSS guidelines.
 
>“Every line of code should appear to be written by a single person, no matter the number of contributors” - Mark Otto (the guy who made bootstrap)
 
###General Guidelines
**Syntax**
- 4 space tabs soft tabs (2) spaces (edited)
- When grouping selectors, place individual selectors on their own line
```css 
.class1,
.class2 { 
}
```
- Include one space before open brace, closing on new line.
`.class {
    /* code here */
}``
- Put one space after colons `:` for each declaration and close all declarations with a semi-colon `;`
```css
.selector {
    color: #fff;
}
```
- Each declaration should be on it’s own line, unless there is only one. This is very helpful in debugging.
```css 
.class1 {
  font-size: 1em;
  color: #fff;
}
.class2 { color: #fff; }
```
- Comma separated property values get space between them, color values do not
```css 
.selector {
  background-color: rgba(0,0,0,.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;`
}
```

- Never use leading 0’s on values, (ex `0.5px`)
- Use lowercase hex values for colors. It’s easier to read. Also, use shorthand when possible. (bad: `#FAFEDC`, good: `#fafedc`, or `#fff`)
- Put quotes around attributes values in selectors `input[type="text"]`
- Don’t specify units for zero values (bad: `0px`, good: `0`)

**Comments**  
Write descriptive comments that convey context and purpose

**Specificity**  
The key to solving a lot of CSS tangle and headaches is keeping specificity of your selectors as flat as possible. Overly specific selectors can cause a lot of damage.
 - Don’t use IDs in CSS
 - Don’t nest selectors - Using descendent and child selectors increase specificity. In general if you can avoid using them, don’t use them. Nested selectors can help with providing scope, and need to be used for scoping app/module styles, but an alternative in other scope cases is to use the BEM naming convention mentioned later.
 - Be aware that chaining selectors and qualifying classes to an element increase their specificity and reduce reusability.
   - `input.error` is more specific and less reusable than `.error`


**!important**

Use `!important` proactively, not reactively. Use it to make sure nothing overwrites a declaration. i.e. `display: none !important;` If you find yourself having to use it to overwrite other styles something is probably wrong.

**Shorthand**

Try to limit use of shorthand declarations to instances where you must explicitly set all available values. Setting unnecessary values in shorthand leads to unnecessary overrides and unintended side effects.
```css
/* Bad example */
.element {
  margin: 0 0 10px;
  background: red;
}

/* Good example */
.element {
  margin-bottom: 10px;
  background-color: red;
}

```

**Prefixed properties**

Indent prefixed properties so declaration values line up:
```css
.selector {
 -webkit-flex: ;
    -moz-flex: ; 
         flex: ;
}
```

**Declaration Order**

Group property declarations in following order:
1. Positioning
2. Box Model
3. Typographic
4. Visual

Positioning comes first, it can override box-model styles.<br/>
Box Model next as it sets dimensions and placement.<br/>
The others affect things inside component so they come last.

**Nesting**

Limit nesting to when you must scope styles to a parent and if there are multiple items to be nested. (Exception, scoping for app/module styles)

**Media Query Placement**

When possible, place media queries as close to their relevant rule sets as possible. Keeping them together makes it harder to miss them.

###Organizing your CSS
**Section titles**

Major sections of CSS should start with a title. Title should begin with `#` to help with targeted searches. 
Titles should be followed by a return to space out following CSS code, and preceded by 5 returns when not the first title in a document.
```css
/*---------------#SECTION TITLE---------------*/
.selector {}
```

###Classes
**Overview**
 - Write classes in lowercase
 - Use dashes to separate words, don’t use underscores or camelCase
 - Class names should be meaningful. Use structural over presentational names. Instead of focusing on semantics, look more closely at sensibility and longevity. Choose names based on ease of maintenance, not for their perceived meaning. Tying class name semantics to nature of content reduces the ability of architecture to scale or be easily put to use by other devs. TODO( Example )
    - Augment classes with data-ui-component attribute to house a more specific name. ex: 
    ```html 
    <ul class=“tabbed-nav” data-ui-component=“Main Nav”>
    ```
    - use `.js-*` classes to hook into for behavior. Keep these out of stylesheets though.

**Naming Conventions**  
Based on BEM methodology (https://en.bem.info/)  
BEM naming conventions help make css and HTML markup clear to developers by associating elements and modifiers with the blocks (component) they belong to.
The trick with BEM is knowing when something falls into a relevant category. Just because something happens to live inside a block, doesn’t always mean it is actually a BEM element. It’s important to know when BEM scope starts and stops. As a rule, BEM applies to self-contained, discrete parts of the UI.

**Block:** The sole root of the component  
**Element:** A component part of the Block
 - use double underscores to separate block name and element name: `block-name__element-name`
**Modifier:** A variant or extension of the block
 - use double dashes to separate block name and modifier name: `block-name—modifier-name`
 
**Examples:**
```css
  .btn {} /* Block */  
  .btn__content {} /* Element */  
  .btn--big {} /* Modifier */
```



###Selectors
**Selector Intent**  
Poor selector intent is one of the biggest reasons for headaches in CSS projects. Using generic selectors to apply very specific treatments causes unexpected side effects and leads to tangled stylesheets. Instead use explicit and unambiguous selectors.
`Bad: header ul {}
Good: .site-nav {}`
 
**Reusability**  
With a component based approach to building the UI, reusability is paramount. Everything you choose, from the right type of selector to it’s name should lend itself toward being reused.
 
**Selector Performance**  
In general. The longer a selector is (the more component parts) the slower it is. `body.home div.header ul {}` is a lot slower than `.primary-nav {}`