## Selecting Content Readme

### Objectives
Learn how to:

* Use the developer tools to identify a particular element
* Use Javascript's selector methods to select particular pieces of the DOM
* Learn about CSS selectors to select elements by id, class or HTML Tag.

### Introduction

For this lesson we want to look a little deeper at how to retrieve information about a webpage by using JavaScript's [document object model api](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model).  By working with JavaScript's DOM API, we will be able to select and manipulate any content on the browser.

### Get Started

Let's work with the [Ada Lovelace Wikipedia entry](https://en.wikipedia.org/wiki/Ada_Lovelace). We have copied the HTML from a page on Ada Lovelace from Wikipedia and simplified it a bit.  Open the `lovelace.html` page included in this reading in your web browser. You can browse the "local" system by using &#x2318; + o on a Mac or Control + o or &x2318; on other systems and open the `lovelace.html` file. We'll be working with this file over the next several instructions.

First things first, let's try to read the title of the webpage. The title is usually just the first header in the web page. In HTML, headers are called `h1` tags and look like this: `<h1>This Is My Title</h1>`. In our Ada Lovelace page, it looks like the title is "Ada Lovelace".

### Identify the correct piece of HTML in the console

From the browser, right click (or two fingers click) on the Ada Lovelace title, and then click on inspect from the dropdown menu. Your Inspector may come up on the right or the bottom. Select the icon two icons to the left of the "Elements" Header. It should look like this: ![element inspector icon](http://web-dev-readme-photos.s3.amazonaws.com/js/elementinspector-icon.png). This is the Element Selector. Once you click that, select the "Ada Lovelace" title of the Wikipedia page. Then click on the `id` attribute. You should see the `id` "attribute" is equal to `firstHeading`. Press &#x2318;+c on Mac or Ctrl+c on Windows to copy the `id` attribute.  Let's use that `id` attribute value with our query selector to select the correct element from our document. In doing so we'll perceive the necessity of understanding HTML well: JavaScript can only be used to affect the HTML that has can be "grabbed."

### Select the title with the querySelector method

Now that we have the `id`entifier for the title, we can place that into our query selector method.

Now, from the Console, type in `document.querySelector('h1')`, and you'll see that this selects the appropriate element.

```js
document.querySelector('h1')
// <h1 id="firstHeading" class="firstHeading" lang="en">Ada Lovelace</h1>
```

So `h1` is called the tag name of the element.  And to select something by the tag name we simply pass through the name of the tag. The `document.querySelector` method  will return the _first match_. One can easily imagine multiple `<p>` tags, however. We will cover multiple match scenario in a later lesson, but if you'd like to grow your self-education muscles, search the Web for the **MDN** documentation on **`document.querySelectorAll`**.

### Other CSS Selectors

Using JavaScript we can "target" elements using the same syntax we "target" element in CSS. You've just used the first occurrence of an HTML tag match to "select" an HTML element.  Let's see some other ways to "select" elements.  For the moment, we'll step away from the `lovelace.html` document and look at a simple HTML snippet. Beneath the snippet, find a table displaying different ways to select the paragraph element.

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

| Attribute     | CSS Selector  | querySelector Code |
| ------------- |:-------------:| -----:|
| id      	   | `#` 			  | `document.querySelector('#content')`|
| class      	   |`.`     		  |  `document.querySelector('.red')` |
| html tag      | 	         |    `document.querySelector('p')` |


So you can see that we prepend the `#` sign to the `id` attribute name to select an item by its id.  We prepend the `.` to the class attribute name to select an item by its class name.  And we prepend nothing when selecting by tag name.

We can also select elements using other methods.  Let's add in those other methods to our chart.

| Attribute     | CSS Selector  | querySelector Code |Alternative Method |
| ------------- |:-------------:| -----:| -----:|
| id     	   | `#` 			  | `document.querySelector('#content')`|`document.getElementById('content')`|
| class      	   | `.`     		  |  `document.querySelector('.red')` |`document.getElementsByClassName('red')`|
| html tag      | 	         |    `document.querySelector('p')` | `document.getElementsByTagName('p')`|

> Notice that when we use a method like `getElementById` we do not need to start with a # sign.  This is because Javascript already knows that we are selecting by the `id` attribute by virtue of using a method that only accepts an id.  Query selectors take different types of attributes, so there we do need to specify the type of attribute we are selecting by.

Let's turn back to our `lovelace.html` file.

### Selecting by ancestry - and solving a problem

Let's some more content: links.

Links are defined in HTML with an `a` tag. Let's use what we know so far to try to access the link for the word "Marylebone" inside the "info box" or "gray inset" at the right. Inspect this element.  You'll see it has no `id` or `class` attribute.  To select it we can select the infobox and _then_ select a unique span with a class name and _then_ the first link within that span. Try this:

`document.querySelector(".infobox").querySelector("span.deathplace").querySelector("a")`

This code statement should make sense in English if you read it backward:

"Find the link that's within the span element with the class "deathplace" that's within some element with the class "infobox".

It's our opinion that having the mental flexibility to read backward and forward in different made-up languages might be the reason so many programmers are prone to puns, wordplay, and Lewis Carroll books.

### Retrieving Attributes

Now that we have selected the proper HTML element, let's save it to a variable and examine it "under a microscope."

```js
let maryleboneAnchor = document.querySelector(".infobox").querySelector("span.deathplace").querySelector("a")
maryleboneAnchor.constructor
// ƒ HTMLAnchorElement() { [native code] }
```

By calling the selected element's constructor method, we can see that we `maryleboneAnchor` is an instance of an HTMLAnchorElement.  Many of methods on this instance correspond to the potential attributes of an HTML anchor.  For example, `maryleboneAnchor.href` returns `"/wiki/Marylebone"`, `maryleboneAnchor.text` returns `"Marylebone"`.

> **The magic of guessing:**
> Programmers guess a lot more than you might think.  The reason why is because the consequences guessing incorrectly are really low, and you can often quickly find out if you are right or wrong.  So if you're unsure if some code will work, just guess and try it.  The consequence of guessing incorrectly is that you learn something new about the language.

The methods we explored thusfar like like `href` and `text` make a lot of sense when we have an instance of an HTMLAnchorElement.  There are other methods you can reliably call on any instance of an element (or any HTML DOM node, to be technical) that you select.  We already saw a couple of them when exploring the DOM in previous lessons.  These methods can allow us to find information about other related nodes, like `children`, or `nextSibling`.  Some tell us attributes of element, like `attributes` to see attributes of the element, `classList` to see a list of classes associated with the element, and `style` which returns the associated CSS styles of a selected element.

### Summary

In this section, we learned how to use Javascript and our developer console to ask questions about our HTML. Here is our process:

1. From the browser, click inspect element on the HTML element you are interested in to find an identifying attribute to select the HTML.
2. Depending on the type of attribute, you can then use `document.querySelector()` method, and in the parentheses pass through the proper CSS selector in combination with the attribute name to select that element.

	> Or you can use a different Javascript method like `document.getElementsByClassName()` to select the correct element.

3. Once you have properly targeted the element, you can then call other methods to identify attributes of that element.  Eg.`document.querySelector('a').text`

### Resources

[MDN Element](https://developer.mozilla.org/en-US/docs/Web/API/Element)

<p class='util--hide'>View <a href='https://learn.co/lessons/selecting-single-elements-readme'>Selecting Single Elements Readme</a> on Learn.co and start learning to code for free.</p>
