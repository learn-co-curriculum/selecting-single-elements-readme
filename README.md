## Selecting Content Readme

### Objectives

Learn how to:

* Use the developer tools to identify a particular element
* Use JavaScript's selector methods to select particular pieces of the DOM
* Learn about CSS selectors and use them to select elements by id, class or HTML Tag.

### Introduction

For this lesson we want to look a little deeper at how to retrieve information about a webpage by using JavaScript's [document object model API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model).  By working with JavaScript's DOM API, we will be able to select and manipulate any content on the browser.

### Get Started

Let's work with the "Ada Lovelace" Wikipedia entry. We have copied the HTML
from Wikipedia and simplified it a bit.  Open [the Ada Lovelace][static-lovelace-file]
document in a new tab. You'll be moving between that tab as well as this
document.

First things first, let's identify the title of the webpage. The title is
usually just the first header in the web page. In HTML, headers are called `h1`
tags and look like this: `<h1>This Is My Title</h1>`. In our
[Ada Lovelace page][static-lovelace-file], it looks like the title is "Ada
Lovelace".

### Identify The Correct Piece Of HTML In The Console

To set up clarity of the next instructions, let's define some vocabulary.  At
the moment your browser is presenting information about Ada Lovelace in its
_content pane_. Naturally enough this is where the content is displayed. In
this next operation we're going to open up the "Developer Tools." These tools
exist in their own pane, the _developer tools pane_ or _inspector_. The
_developer pane_ has multiple _headings_. That should be enough vocabulary.

1. From the browser, right click (or two fingers click) in the "content pane"
2. Click on **Inspect** from the dropdown menu.
3. Your _developer tools pane_ may come up on the right or the bottom
4. Select the icon two icons to the left of the "Elements" Header. It should look like this: ![element inspector icon](http://web-dev-readme-photos.s3.amazonaws.com/js/elementinspector-icon.png). This is the "Element Selector." After you click that...
5. Click on the "Ada Lovelace" title in the _content pane_.
6. The result of this click should be shown in the _developer tools pane_. The
   "Elements" header should be active and the HTML entity for the title should
   be selected ("highlighted")
7. See the `id` "attribute" is equal to `firstHeading`
8. Double-click on the `id` attribute's value and Chrome will text-select it
   for you for ease of copy-and-paste. Press &#x2318;+c on Mac or Ctrl+c on
   Windows to copy this `id` attribute name. This is a very common pattern for
   developers to use since `id`s can be quite nasty to type out.

**Aside**: You'll notice that this `h1` tag has an `id` attribute that matches the `class` attribute. This is relatively uncommon. An `id` is
usually obviously unique like `mostImportantHeader` while class might be a list
of class names like `header pretty-purple women-tech-pioneer`. We'll focus more
on this later, but don't make the mistake of thinking these two attributes are
required to be the same.

Let's use that captured `id` attribute value with `document.querySelector` to
select the correct element from our document. Select the "Console" header
within the _developer tools pane_ and fill in `document.querySelector()`'s
parentheses with ", #, _paste_, " so that the full line looks like:

```javascript
document.querySelector("#firstHeading")
```

You should see returned:

```text
<h1 id="firstHeading" class="firstHeading" lang="en">Ada Lovelace</h1>
```

Congratulations, you've used the `id` to find an element on a page with
JavaScript! Let's try a more general application by simply looking up based on
the tag.

### Select The Title With The QuerySelector Method

Now, from the Console, type in `document.querySelector('h1')`, and you'll see that this selects the _first_ appropriate element.

```js
document.querySelector('h1')
// <h1 id="firstHeading" class="firstHeading" lang="en">Ada Lovelace</h1>
```

The "tag name" of this element is `h1`. To select something by the tag name we simply "pass" through the "tag name" to `document.querySelector`. Be sure to recognize that the `document.querySelector` method  will return the _first match_. One can easily imagine HTML pages with multiple `<p>` tags, however. We will cover multiple match scenario in a later lesson, but if you'd like to grow your self-education muscles, search the Web for the **MDN** documentation on **`document.querySelectorAll`**.

### Other CSS Selectors

Using JavaScript we can "target" elements using the same syntax we "target" element in CSS. You've just used the first occurrence of an HTML tag match to "select" an HTML element.  Let's see some other ways to "select" elements.  For the moment, we'll step away from the `lovelace.html` document and look at a simple HTML snippet as a thought experiment. Beneath the snippet, find a table displaying different ways to select the paragraph element.

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


So you can see that we prepend the `#` sign to the `id` attribute name to select an item by its `id.` We prepend the `.` to the class attribute name to select an item by its class name.  And we prepend nothing when selecting by tag name.

We can also select elements using other methods.  Let's add in those other methods to our chart.

| Attribute     | CSS Selector  | querySelector Code |Alternative Method |
| ------------- |:-------------:| -----:| -----:|
| id     	   | `#` 			  | `document.querySelector('#content')`|`document.getElementById('content')`|
| class      	   | `.`     		  |  `document.querySelector('.red')` |`document.getElementsByClassName('red')`|
| html tag      | 	         |    `document.querySelector('p')` | `document.getElementsByTagName('p')`|

> Notice that when we use a method like `getElementById` we do not need to start with a # sign.  This is because JavaScript already knows that we are selecting by the `id` attribute by virtue of using a method that only accepts an id.  Query selectors take different types of attributes, so there we do need to specify the type of attribute we are selecting by.

Let's turn back to our `lovelace.html` file.

### Selecting By Ancestry - And Solving a Problem

Let's look at some more content: links.

Links are defined in HTML with an `a` tag. Let's use what we know so far to try to access the link for the word "Marylebone" inside the "info box" under Ada's image. Inspect this element.  You'll see it has no `id` or `class` attribute.  To select it we can select the infobox and _then_ select a unique span with a class name and _then_ the first link within that span. That is, we will "chain" our calls to `document.querySelector`. Try this:

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

By calling the selected element's `constructor` property, we can see that `maryleboneAnchor` is an "instance" of an `HTMLAnchorElement`.  Many methods on this instance correspond to the potential attributes of an HTML anchor.  For example, `maryleboneAnchor.href` returns `"/wiki/Marylebone"`, `maryleboneAnchor.text` returns `"Marylebone"`.

> **The magic of guessing:**
> Programmers guess a lot more than you might think.  The reason why is because the consequences guessing incorrectly are really low, and you can often quickly find out if you are right or wrong.  So if you're unsure if some code will work, just guess and try it.  The consequence of guessing incorrectly is that you learn something new about the language.

The methods we explored thus far like `href` and `text` make a lot of sense when we have an instance of an `HTMLAnchorElement`.  There are other methods you can reliably call on any instance of an element (or any HTML DOM node, to be technical) that you select.  We already saw a couple of them when exploring the DOM in previous lessons.  These methods can allow us to find information about other related nodes, like `children`, or `nextSibling`.  Some tell us attributes of element, like `attributes` to see attributes of the element, `classList` to see a list of classes associated with the element, and `style` which returns the associated CSS styles of a selected element. Explore these on the `maryleboneAnchor` element.

If `maryleboneAnchor` isn't providing anything interesting, use the "chained" `querySelector` method to find something more interesting. Don't forget the magic of guessing either, if there's a `next....` something there's probably also a `previous....` something. If a method doesn't return a String, what _does_ it return? This process of exploration and guess-and-check, optimized for speed, is how programmers avoid having to know books worth of knowledge: they don't memorize all the facts, they ask the _computer_ to teach them micro-lessons.

### Summary

In this section, we learned how to use JavaScript and our developer console to ask questions about our HTML. Here is our process:

1. From the browser, click inspect element on the HTML element you are interested in to find an identifying attribute to select the HTML.
2. Depending on the type of attribute, you can then use `document.querySelector()` method, and in the parentheses pass through the proper CSS selector in combination with the attribute name to select that element **OR** we can use a different JavaScript method like `document.getElementsByClassName()` to select the correct element.
3. Once you have properly targeted the element, you can then call other methods to identify attributes of that element e.g.`document.querySelector('a').text`

### Resources

[MDN Element](https://developer.mozilla.org/en-US/docs/Web/API/Element)

<p class='util--hide'>View <a href='https://learn.co/lessons/selecting-single-elements-readme'>Selecting Single Elements Readme</a> on Learn.co and start learning to code for free.</p>

[static-lovelace-file]: https://learn-co-curriculum.github.io/selecting-single-elements-readme/lovelace.html
