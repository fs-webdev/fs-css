# fs-css
A repo to hold FamilySearch WebDev's current CSS style guides, both for humans and machines to read, IE: .md or .csslint

## Hey, this (insert pattern/thought here) isn't in the style guide!? 

Then we do not have a guide for it, and you may do whatever makes you work happiest, and best, as long as it doesn't conflict with something in the style guide and follows industry best practices and standards.


## Table of Contents
  1. [Anatomy](#anatomy)
  1. [Specificity](#specificity)
  1. [Selectors](#selectors)
  1. [Properties](#properties)
  1. [Strings](#strings)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Semicolons](#semicolons)
  1. [Naming Conventions](#naming-conventions)
  1. [Vendor Prefixes](#vendor-prefixes)
  1. [Browser Targeting](#browser-targeting)
  1. [BEM](#bem)

## Anatomy

  - Each CSS rule is made up for four parts: the **selector**, the **declaration block**, a list of **properties**, and their **values**.

    ```css
    h1 /* selector */
    {  /* declaration block { } */
      color: #4D4D4A;  /* property: value */
      font-size: 36px; /* property: value */
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Specificity

  - Selector specificity determines which CSS rule is applied to each element. Higher specificities take precedence over lower specificities.

  - The universal selector has a specificity of 0.

    ```css
    /* specificity = 0 */
    * {
      box-sizing: border-box;
    }
    ```

  - Each Element selector and pseudo-element (`::before`, `::after`, etc.) has a specificity of 1.

     ```css
    /* specificity = 1 */
    p {
      margin: 0;
    }
    
    /* specificity = 2 */
    h3 p {
      margin: 0;
    }
    
    /* specificity = 2 */
    p::before {
      margin: 0;
    }
    ```

  - Each Class selector, Attribute selector, and psuedo-class (`:hover`, `:first-child` etc.) has a specificity of 10.

     ```css
    /* specificity = 10 */
    .content {
      margin: 0;
    }
    
    /* specificity = 11 */
    a[href='/'] {
      font-weight: bold;
    }
    
    /* specificity = 20 */
    .link:hover {
      text-decoration: underline;
    }
    ```

  - Each ID selector has a specificity of 100.

    ```css
    /* specificity = 100 */
    #content {
      padding: 0;
    }
    
    /* specificity = 110 */
    #content .nav {
      color: #333331;
    }
    ```

  - An inline style has a specificity of 1000.

    ```html
    <!-- specificity = 1000 -->
    <div style="float: left"></div>
    ```

  - The `!important` rule ignores all specificity and will always override any other declaration made anywhere else in any CSS rule.

    ```css
    /* located in file main.css */
    .nav {
      font-size: 12px !important;
    }
    
    /* located in file my-module.css */
    .nav {
      font-size: 16px;  /* will not be applied since !important was used in main.css */
    }
    ```

  - Avoid the use of the `!important` rule

    ```css
    /* bad */
    .header {
      max-width: 1200px !important;
    }
    
    /* good */
    .header {
      max-width: 1200px;
    }
    ```
  
**[⬆ back to top](#table-of-contents)**

## Selectors

  - Avoid the ID selector whenever possible.

    ```css
    /* bad */
    #container {
      /* ... */
    }
    
    /* good */
    .container {
      /* ... */
    }
    ```

  - Avoid the universal selector. Most of the time it can be rewritten to make use of inheritance.

    ```css
    /* bad */
    .content * {
      font-size: 12px;
    }
    
    /* good */
    .content {
      font-size: 12px;
    }
    ```

  - Keep your selectors short (i.e. less than 3 items long). Be careful when nesting selectors in pre-processors becuase they will produce long selectors without your realizing it.

    ```css
    /* bad */
    .selector .a .b .c .d {
      /* ... */
    }
    
    /* good */
    .selector .a .b {
      /* ... */
    }
    
    /* best */
    .selector {
      /* ... */
    }
    ```

  **CSS pre-processors**

    ```sass
    /* bad */
    /* produces: .selector .a .b .c .d { } */
    .selector {
      /* ... */
      
      .a {
        /* ... */
        
        .b {
          /* ... */
          
          .c {
            /* ... */
            
            .d {
              /* ... */
            }
          }
        }
      }
    }
    ```

  - For more information see [Keep Your CSS Selectors Short](http://csswizardry.com/2012/05/keep-your-css-selectors-short/) by Harry Roberts.

**[⬆ back to top](#table-of-contents)**

## Properties

  - Use shorthand properties whenever possible.

    ```css
    /* bad */
    .selector {
      margin-left: 0;
      margin-right: 0;
      margin-top: 10px;
      margin-bottom: 10px;
    }
    
    /* good */
    .selector {
      margin: 10px 0;
    }
    ```
    
  - For more information see [CSS Shorthand Properties](http://www.webcredible.com/blog-reports/css/css-shorthand-properties.shtml) by Paul McCarthy.
  
  - Use `px` for font-size instead of `em`.

    ```css
    /* bad */
    p {
      font-size: 1em;
    }
    
    /* good */
    p {
      font-size: 16px;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  - Use dobule quotes when referencing URLs

    ```css
    /* bad */
    .photo {
      background-image: url(img/photo_background.png);
    }
    
    /* bad */
    .photo {
      background-image: url('img/photo_background.png');
    }
    
    /* good */
    .photo {
      background-image: url("img/photo_background.png");
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  - Use comments to identify various components, sections, etc.

    ```css
    /* Person Actions */
    .tree-person {
      /* ... */
    }
    .name {
      /* ... */
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

  - Each rule should be written on multiple lines.

    ```css
    /* bad */
    .header { margin: 0 auto; padding: 0; }
    
    /* good */
    .header {
      margin: 0 auto;
      padding: 0;
    }
    ```
  
  - Each selector should be written on mutiple lines.

    ```css
    /* bad */
    h1, h2, h3 { color: #4D4D4A; }
    
    /* good */
    h1,
    h2,
    h3 {
      color: #4D4D4A;
    }
    ```

  - Add a whitespace before each opening brace and after each colon.

    ```css
    /* bad */
    p{
      line-height:14px;
    }
    
    /* good */
    p {
      line-height: 14px;
    }
    ```
 
**[⬆ back to top](#table-of-contents)** 
    
## Semicolons

  - Trailing semicolons: **Yep.**

    ```css
    /* bad */
    .nav {
      list-style: none;
      padding: 0
    }
    
    /* bad */
    /* Adding another declaration to the rule can result in an error */
    .nav {
      list-style: none;
      padding: 0
      margin: 0;
    }
    
    /* good */
    .nav {
      list-style: none;
      padding: 0;
    }
    ```
    
**[⬆ back to top](#table-of-contents)** 

## Naming Conventions

  - Do not use presentational class names or IDs.

    ```css
    /* bad */
    .yellow {
      background-color: yellow;
    }
    .left {
      float: left;
    }
    .mediumBlueBox2pxBorderBlack {
      background-color: #0000CD
      border: 2px solid black;
      width: 100px;
    }
    
    /* good */
    .alert {
      background-color: yellow;
    }
    .sidebar {
      float: left;
    }
    .content-box {
      background-color: #0000CD
      border: 2px solid black;
      width: 100px;
    }
    ```

  - Use all lowercase, hyphen-separated values for class names and IDs.

    ```css
    /* bad */
    .navMenu {
      /* ... */
    }
    .nav_menu {
      /* ... */
    }
    .NAVMENU {
      /* ... */
    }
    
    /* good */
    .nav-menu {
      /* ... */
    }
    ```

**[⬆ back to top](#table-of-contents)** 

## Vendor Prefixes

  - Always include the proposed standard property name after any vendor prefixes for future proofing.

    ```css
    /* bad */
    .selector {
      display: -webkit-box;
    }
    
    /* good */
    .selector {
      display: -webkit-box;
      display: flex;
    }
    ```

**[⬆ back to top](#table-of-contents)** 

## Browser Targeting

  - Avoid browser targeted CSS hacks. For the most part, we have deprecated all browsers that would cause you to use a browser CSS hack.

  - You are responsible for removing any CSS browser hacks when you come across them. Examples of browser hacks:

    ```css
    /* bad. Very bad */
    
    .selector {
      color: blue\9;  
    }
    
    .selector {
      color/*\**/: blue\9;
    }
    
    _:-ms-input-placeholder, :root .selector {
      color: blue;
    }
    ```

**[⬆ back to top](#table-of-contents)** 

## BEM

  - BEM stands for **Block, Element, Modifier** and is a methodology for naming you CSS classes.

  - When making components or closely related classes then understand and judiciously apply BEM naming conventions for your classes. This introduces the valid uses of double dashes (--) and double underscores (__).

  - We understand that it is difficult to define "judiciously apply." We want you to use BEM even though it is "ugly." And we want you to keep from going overboard with it because it can easily be overdone. Use BEM for complex things.

  - Resources:
    * CSS Wizardry blog post [MindBEMding: Getting Your Head 'Round BEM Syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/).
    * Appfolio Engineering technical blogpost on [CSS Architecture](http://engineering.appfolio.com/2012/11/16/css-architecture/). While it doesn't mention BEM by name it uses the BEM syntax in its examples of how to organize related classes.
    * The [fs-styles](https://github.com/fs-webdev/fs-styles) project uses BEM. Good place to see how we really use it here. Refer to [this issue](https://github.com/fs-webdev/fs-styles/issues/4) as to why we chose to use BEM in the first place.

