# Web Development Front End

## HTML

Example:

```html
<!DOCTYPE html>

<html>

<head>
    <title>Page Title</title>
</head>

<body>
    <h1>A Photo of a Beautiful Landscape</h1>
    <a href="https://www.w3schools.com/tags">HTML tags</a>
    <p>Here is the photo</p>
    <img src="photo.jpg" alt="Country Landscape">
</body>

</html>
```

[W3Schools HTML Tags](https://www.w3schools.com/tags/default.asp)

Checking your HTML: It's a good idea to check the validity of your HTML. Here is a website that checks your HTML for syntax errors: [W3C Validator](https://validator.w3.org/#validate_by_input).

### div and span

- `<div>` elements is used to split off large chunks of html into sections. 

- `<span>` elements, on the other hand, are for small chunks of html. You generally use span elements in the middle of a piece of text in order to apply a specific style to that text. 

```html
<div>
   <p>This is an example of when to use a div elements versus a span element. A span element goes around <span>a small chunk of html</span></p>
</div>
```

### ids and classes

- `id` is for marking a specific piece of content. You should only use an ID name one time per html page.

- `class` helps group multiple pieces of content together in order to add the same styling/functionality.

```html
<div id="top">
    <p class="first_paragraph">First paragraph of the section</p>
    <p class="second_paragraph">Second paragraph of the section</p>
</div>

<div id="bottom">
    <p class="first_paragraph">First paragraph of the section</p>
    <p class="second_paragraph">Second paragraph of the section</p>
</div>
```

## CSS

CSS stands for cascading style sheets. The "cascading" refers to how rules trickle down to the various layers of an html tree.

### Different ways to write CSS

1. __in-line__: means that you specify the CSS directly inside of an html tag like so:

   ```html
   <p style="font-size:20px;">This is a paragraph</p>
   ```

2. __stylesheet__: separate from html tag

   1. The stylesheet can go underneath an html `<head>` tag like so:

      ```html
      ...
      <head>
         <style>
             p {font-size: 20px;}
         </style>
      </head>
      ```

   2. The CSS can go into its own separate CSS file (extension .css). Then you can link to the CSS file within the html head tag like so:

      ```html
      <head>
          <link rel="stylesheet" type"text/css" href="style.css">
      </head>
      ```

      where `style.css` is the path to the style.css file. Inside the style.css file would be the style rules such as:

      ```css
      p {
        color:red;
      }
      ```

The [W3 Schools CSS Website](https://www.w3schools.com/css/default.asp) is a good place to find all the different rules you can use.

To select by class name, you use a dot like so:

```css
.class_name {
   color: red;
}
```

more complex selections like "select paragraphs inside the div with id "div_top" :

```css
div#div_top p {
  color: red;
}
```

### Margins and Padding

The difference between margin and padding is a bit tricky. 

- **Margin** rules specify a spatial buffer on the outside of an element. 

- **Padding** specifies an internal spatial buffer.

```html
<div style="border:solid red 1px;margin:40px;padding:40px;">
    Box
</div>
```

### Specifying Size: Pixel vs Percent vs EM units

When you use `px`, you're defining the exact number of pixels an element should use in terms of size. 

```html
<p style="font-size: 12px;">
```

The percent and em units have a similar function. They dynamically change sizing based on a browser's default values.

```html
<p style="font-size: 100%"> 
```

means to use the default browser font size.  Similarly, 1em unit would be 1x default_font. So 2em would be 2 x default font, etc.

Percentages and em units are actually calculating sizes relative to parent elements in the html tree.

```html
<body style="font-size: 20px">
    <p style="font-size:80%">This is a paragraph</p>
...
</body>
```

## JavaScript

### Basic JavaScript Syntax

- a line of code ends with a semi-colon ;
- () parenthesis are used when calling a function much like in Python
- {} curly braces surround large chunks of code or are used when initializing dictionaries
- [] square brackets are used for accessing values from arrays or dictionaries much like in Python

```javascript
function addValues(x) {
  var sum_array = 0;
  for (var i=0; i < x.length; i++) {   
    sum_array += x[i];
  }
  return sum_array;
}

addValues([3,4,5,6]);
```

### jQuery

jQuery is a JavaScript library that makes developing the front-end easier. 

The jQuery library simplifies JavaScript quite a bit.

The dollar sign `$`  is jQuery syntax that says "grab this element, class or id". For example `$("p#first")` means find the paragraph with id="first". 

```javascript
         $(document).ready(function(){
            $("img").click(function(){
                $("h1").text("A Photo of a Breathtaking View");
            });
        });
```

## Bootstrap

Bootstrap is one of the easier front-end frameworks to work with. Bootstrap eliminates the need to write CSS or JavaScript. Instead, you can style your websites with HTML.

[Starter Template](https://getbootstrap.com/docs/4.0/getting-started/introduction/#starter-template)

## Visualization with JavaScript (Plotly) 

[d3.js](https://d3js.org/) is one of the most popular (and complex!) javascript data visualization libraries. 

## Hugo

Hugo is an open-source static site generators (written with Go Lang). 