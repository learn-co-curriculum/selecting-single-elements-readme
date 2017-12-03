## Selecting Content Readme

### Objectives
Learn how to:

* Use the developer tools to identify a particular element
* Use Javascript's selector methods to select particular pieces of the DOM
* Learn about CSS selectors to select elements by id, class or HTML Tag.

### Introduction

For this lesson we want to look a little deeper at how to retrieve information about a webpage by using JavaScript's [Document Object Model API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model). The [Document Object Model](https://www.w3schools.com/js/js_htmldom.asp) (DOM), is a way of thinking about a document as a "root" and all other elements being branches or "children" of that root. By working with JavaScript's DOM API, we will be able to select and manipulate any content on the browser. 

### Get Started

In order to learn about the DOM, we'll need a document to play with. Let's work with the [Ada Lovelace Wikipedia entry](https://en.wikipedia.org/wiki/Ada_Lovelace). We have copied the HTML from a page on Ada Lovelace from Wikipedia and simplified it a bit.  You can view it in the file `lovelace.html` in this repo. (if you don't know what a 'repo' is, that's fine, just go ahead and use the "live" copy on wikipedia linked above. Just know that it might have changed and some of the things in the page might have changed since this lesson was last modified)

First things first, let's try to read the title of the webpage. The title is usually just the first header in the web page. In HTML, headers are called `h1` tags and look like this: `<h1>This Is My Title</h1>`. In our Ada Lovelace page, it looks like the title is "Ada Lovelace".

Let's confirm the HTML in the Wikipedia page wraps the big Ada Lovelace with `h1` tags.

### Identify the correct piece of HTML in the console

You can view the HTML content in the browser by [clicking here](https://en.wikipedia.org/wiki/Ada_Lovelace) (or by opening the `lovelace.html` page included in this repo).  Then, right click (or two fingers click) on the Ada Lovelace title, and then click on 'Inspect' from the dropdown menu. What will open is called Chrome Developer Tools (and will likely open in the Elements tab), and it may come up on the right or the bottom of the browser window (or in a separate window altogether). Select the icon two icons to the left of the "Elements" tab header. It should look like this: ![element inspector icon](http://web-dev-readme-photos.s3.amazonaws.com/js/elementinspector-icon.png). This is the Element Selector. Once you click that, click on the "Ada Lovelace" title of the Wikipedia page. Then click on the `id` attribute, you should see the `id` is equal to `header`. Press command+c on Mac or Ctrl+c on Windows to copy the `id` attribute.  Ultimately, we'll use that `id` attribute with our query selector to select the correct element from our document.  

### Select the title with the querySelector method

Now that we have the identifier for the title, we can place that into our query selector method. 

Now, from the Console tab in Chrome Developer Tools, type in `document.querySelector('h1')`, and it should have selected the appropriate element. You should see something like this:

```js
document.querySelector('h1')
// <h1 id="firstHeading" class="firstHeading" lang="en">Ada Lovelace</h1>
```

So `h1` is called the tag name of the element.  And to select something by the tag name we simply pass in the name of the tag in quotes.
 
### Other CSS Selectors

Now that we have selected something by tag name by passing in the name of the tag, let's see some other ways to select elements.  Below is some HTML content, and afterward is a table displaying different ways to select the paragraph element.  

```html
<div>
	<p id="content" class="red"> Select this content!</p>
</div>
<ul>
	<li>Don't </li>
	<li>Select </li>
	<li>This </li>
</ul>
```

| Attribute     | CSS Selector Prefix  | querySelector Code |
| ------------- |:-------------:| -----:|
| id      	   | `#` 			  | `document.querySelector('#content')`|
| class      	   |`.`     		  |  `document.querySelector('.red')` |
| html tag      | 	         |    `document.querySelector('p')` |


So you can see that we prefix the `#` in front of an `id` attribute value to select an item by its id. We prepend the `.` to the class attribute value to select an item by its class.  And we prepend nothing when selecting by tag name.

We can also select elements using other methods.  Let's add in those other methods to our chart.

| Attribute     | CSS Selector Prefix | querySelector Code |Alternative Method |
| ------------- |:-------------:| -----:| -----:|
| id     	   | `#` 			  | `document.querySelector('#content')`|`document.getElementById('content')`|
| class      	   | `.`     		  |  `document.querySelector('.red')` |`document.getElementsByClassName('red')`|
| html tag      | 	         |    `document.querySelector('p')` | `document.getElementsByTagName('p')`|

> Notice that when we use a method like `getElementById` we do not need to start with a # sign.  This is because Javascript already knows that we are selecting by the `id` attribute by virtue of using a method that only accepts an id.  Query selectors take different types of attributes, so there we do need to specify the type of attribute we are selecting by.

### Selecting by ancestry - and solving a problem

Moving back to our Ada Lovelace Wikipedia page, let's see if we can select some more content: links.

Links are defined in HTML with an anchor tag, also known as an `a` tag. Go ahead and use your inspecting elements skills from above, and select the link in the infobox on the right under her picture that says "Maryleborne".  You'll see the `a` element itself has no `id` or `class` attribute. To select it using the DOM API, we need to use the HTML tag style of selecting the infobox, and then selecting the first link inside of the element with a class of `deathplace`: `document.querySelector('.deathplace').querySelector('a')`.

### Retrieving Attributes

Now that we have selected the proper HTML element, let's see what we have on our hands. 

```js
let anchor = document.querySelector('.deathplace').querySelector('a')
// undefined
anchor
//  <a href="/wiki/Marylebone" title="">Marylebone</a>
anchor.constructor
// ƒ HTMLAnchorElement() { [native code] }
```
By calling the selected element's constructor method, we can see that `anchor` is an instance of an HTMLAnchorElement.  Many of methods on this instance correspond to the potential attributes of an HTML anchor.  For example, `anchor.href` returns `"https://en.wikipedia.org/wiki/Marylebone"`, `anchor.text` returns `"Marylebone"`.

> **The magic of guessing:**
> Programmers guess a lot more than you might think.  The reason why is because the consequences of guessing incorrectly are really low, and you can often quickly find out if you are right or wrong.  So if you're unsure if some code will work, just guess and try it.  The consequence of guessing incorrectly is that you learn something new about the language. Just play! You can't break it!

The methods we explored thusfar like like `href` and `text` make a lot of sense when we have an instance of an HTMLAnchorElement.  There are other methods you can reliably call on any instance of an element (or any HTML DOM node, to be technical) that you select.  We already saw a couple of them when exploring the DOM in previous lessons.  These methods can allow us to find information about other related nodes, like `children`, or `nextSibling`.  Some tell us attributes of element, like `attributes` to see attributes of the element, `classList` to see a list of classes associated with the element, and `style` which returns the associated CSS styles of a selected element.  

### Summary

In this section, we learned how to use Javascript and Chrome Developer Tools' Console to ask questions about our HTML. Here is our process:

1. From the browser, choose "Inspect" after right-clicking on the HTML element you are interested in to find an identifying attributes to select the HTML.
2. Depending on the type of attribute, you can then use `document.querySelector()` method, and in the parentheses pass through the proper CSS selector in combination with the attribute name to select that element.

	> Or you can use a different Javascript method like `document.getElementsByClassName()` to select the correct element.

3. Once you have properly targeted the element, you can then call other methods to identify attributes of that element.  Eg.`document.querySelector('a').text`

### Resources

[MDN Element](https://developer.mozilla.org/en-US/docs/Web/API/Element)

<p class='util--hide'>View <a href='https://learn.co/lessons/selecting-single-elements-readme'>Selecting Single Elements Readme</a> on Learn.co and start learning to code for free.</p>
