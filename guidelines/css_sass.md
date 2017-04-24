CSS and Sass
============

## Indentation

* 2 spaces
* No tabs
* Add whitespaces around operators and `{}`

## Code Style

* Keep a whitespace after property names (like `color: #000`) and before the opening bracket (as in `.selector {`)
* Write hexadecimal colors using downcase letters
* Classes: Use `dash-case`: `users-wrapper`, `main-box`, etc
* IDs: Use `underscore_case`: `user_1`, `user_photo`, etc
* Separate each selector by a newline to improve readability
* Always use semicolon `;`

```css
.users-wrapper,
.user-info,
.user-name {
  overflow-x: hidden;
  border: 0;
}
```

* Single property declaration can be used in a single-line format

```css
.user-avatar { border-radius: 5px; }
```

## Property Organization

```css
.vehicle-info {
 /* Imports/Exports */
 @import info-box;

 /* Box model properties */
 width: 100px;
 height: 200px;
 display: block;

 /* Spacing */
 margin: 10px;
 padding: 20px;

 /* Tipography */
 font-size: 1.4em;
 color: #d4d4d4;

 /* Background and borders */
 background: #c5c5c5;
 border: 1px solid #ccc;

 /* Additional properties */
 border-radius: 2px;
 transition: opacity .4s ease;
}
```
