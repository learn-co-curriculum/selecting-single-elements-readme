## Selecting Content Readme

### Objectives
Learn how to: 

* Use the developer tools to identify a particular element 
* Use Javascript's selector methods to select particular pieces of the DOM
* Learn about CSS selectors to select elements by id, class or HTML Tag.

Ok, you may not feel like it yet, but you're a pretty good developer.  So it's time to have you start building a search engine.  

I'm serious.  

Let me explain.  As you may know, search engines are built by looking at web pages and inferring page relevancy based on HTML. In 2005, Google acquired a small search engine called Aardvark, which answered users questions by connecting them with a chatbot to those they thought might have an answer.  

![aardvark](https://s3.amazonaws.com/learn-verified/aardvark.jpg)

In building Aardvark, the team had a problem: it wanted to connect not just question and answerers from the same subject but also related subjects.  For example, it needed to know that if a user had a question about cooking, but no one was available, that someone who knew about cuisine may have the answer.

How did it do that?  It retrieved data from the web. Specifically Aardvark often used the topic's Wikipedia page to find what was related. Yes, not a perfect solution, but good enough to get the job done (and sell to Google for 50 million dollars).

In this lesson, we'll also use information from a significantly simplified Wikipedia to find out the topic header and a related article.

### Get Started

We have copied the HTML from a page on Ada Lovelace from Wikipedia and simplified it a bit.  You can view it below.

<iframe height='530' scrolling='no' title='simplified-ada' src='//codepen.io/joemburgess/embed/NjEMOd/?height=530&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/joemburgess/pen/NjEMOd/'>simplified-ada</a> by Joe Burgess (<a href='http://codepen.io/joemburgess'>@joemburgess</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>

First thing's first, our website reader is going to need to read the title of our webpage. Thankfully, the title is usually just the first header in the web page. In HTML, headers are called `h1` tags and look like this: `<h1>This Is My Title</h1>`. In our Ada Lovelace page, it looks like the title is "Ada Lovelace".

Let's confirm that the code wraps the big Ada Lovelace with `h1` tags.

Well, remember that if we open up the console (right click on the page, click inspect), and type in `document` then we will see a current representation of the Document Object Model.  However, that contains everything - the entire page.  We want to scope this down so that we just select the "Ada Lovelace" title.

So we'll use our `document.querySelector()` code just like we did in the last section.  But how do we know what to select?  

# Exploring different selector methods

### Identify the correct piece of HTML in the console

From the browser, right click (or two fingers click) on the Ada Lovelace title, and then click on inspect from the dropdown menu. Your Inspector may come up on the right or the bottom. Select the icon two icons to the left of the "Elements" Header. It should look like this: ![element inspector icon](http://web-dev-readme-photos.s3.amazonaws.com/js/elementinspector-icon.png). This is the Element Selector. Once you click that, select the "Ada Lovelace" title of the Wikipedia page. Then click on the `id` attribute, you should see the `id` is equal to `header`. Press command+c on Mac or Ctrl+c on Windows to copy the `id` attribute.  Ultimately, we'll use that `id` attribute with our query selector to select the correct element from our document.  
  
### Select the title with the querySelector method

Now that we have the identifier for the title, we can place that into our query selector method. First, we need to change our Console to caring only about the CodePen. To do this, you need to click on the Dropdown that says `Top` and change it to the CodePen Preview. Yours may say something slightly different, but it should start with "CodePen Preview". Here is a GIF explaining it a bit better:

![Codepen](https://web-dev-readme-photos.s3.amazonaws.com/js/select-code-pen.gif)

Now, from the Console, type in `document.querySelector('#header')`, and you'll see that this selects the appropriate element.

![select](http://web-dev-readme-photos.s3.amazonaws.com/js/selectHeader.gif)

You may be wondering where the # came from in `document.querySelector('#header')`.  It's there because `header` is the **id** attribute of the element we want to select.  To tell the query selector method that we are selecting an element by its `id` attribute, we use the pound sign.  

### Other CSS Selectors

Now that we have covered that the CSS selector for selecting by an `id` attribute is the # sign, let's cover some of the others.  Below is some HTML content, and afterward is a table displaying different ways to select the paragraph element.  

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
| class      	   | `.`     		  |  `document.querySelector('.red')` |`document.getElementByClassName('red')`|
| html tag      | 	         |    `document.querySelector('p')` | `document.getElementByTagName('p')`|

> Notice that when we use a method like `getElementById` we do not need to start with a # sign.  This is because Javascript already knows that we are selecting by the `id` attribute by virtue of using a method that only accepts an id.  Query selectors take different types of attributes, so there we do need to specify the type of attribute we are selecting by.

### Selecting by ancestry - and solving a problem

Moving back to our Ada Lovelace Wikipedia page, we have a problem.  We can use the `document.querySelector('#header')` to select the header. That's super useful, but if we are going to write a search engine made to find connections, we are going to need to learn how to grab the links on a page.

Links (those blue underlined words you click on) are defined in HTML with an `a` tag. If we use our inspecting elements skills from above, and select the first link ("Charles Babbage") you'll see it has no `id` or `class` attribute. To select it we need to revert to the HTML tag style of selecting. As seen in the table above this looks like: `document.querySelector('p')`. That's close, but instead of grabbing a `p` tag, we want to grab an `a` tag. 

To grab the first link in our web page we need to call `document.querySelector('a')`. That's it. Open up your Console again, make sure to change to the CodePen context by selecting the dropdown at the top that says `top` and selecting CodePen Preview. Then, type in `document.querySelector('a')`. You should see the first link be returned.


### Retreiving Attributes

Now that we have selected the proper HTML element, we can ask Javascript about specific attributes of that element. When thinking about a link, the two most important attributes are what the URL it links to is and what the link text is.

To review, we selected the first link in our simplified wikipedia page by typing:

```javascript
document.querySelector('a')  
```

You should see the following in return

```
<a href="https://en.wikipedia.org/wiki/Charles_Babbage">Charles Babbage</a>
```

Now to get the text out of that element we just append a `.text` onto a code statement

```javascript
document.querySelector('a').text
```

You'll get:

```
"Charles Babbage"
```

Great, you now know what the text of the link is. This is quite valuable when starting to build a search engine! The next question to answer is _where_ does that link go to? If our users wanted more information on Charles Babbage, where should they go?

The website that a link goes to is called it's `href`. You can see it in the HTML code itself:

```
<a href="https://en.wikipedia.org/wiki/Charles_Babbage">Charles Babbage</a>
```

How can we grab the `href` from our querySelector.

> **The magic of guessing:** 
> Programmers guess a lot more than you might think.  The reason why is because the consequences are really low, and you can often quickly find out if you are right or wrong.  So if you're unsure if some code will work, just guess and try it.  The consequence of guessing incorrectly is that you learn something new about the language.

Ok, so if grabbing the `text` was just adding `.text` to the end of our querySelector. Let's try just adding `.href`.

```javascript
document.querySelector('a').href
```

which returns

```
"https://en.wikipedia.org/wiki/Charles_Babbage"
```

You did it! As a reward for your hard work, here is a GIF of a pancake of Ada Lovelace. The internet is a magical place.

![pancake-lovelace](https://web-dev-readme-photos.s3.amazonaws.com/js/lovelace-pancake.gif)

### Summary

In this section, we learned how to use Javascript and our developer console to ask questions about our HTML. Here is our process:

1. From the browser, click inspect element on the HTML element you are interested in to find an identifying attribute to select the HTML.
2. Depending on the type of attribute, you can then use `document.querySelector()` method, and in the parentheses pass through the proper CSS selector in combination with the attribute name to select that element.

	> Or you can use a different Javascript method like `document.getElementByClassName()` to select the correct element.
3. Once you have properly targeted the element, you can then call other methods to identify attributes of that element.  Eg.`document.querySelector('a').text`

<p class='util--hide'>View <a href='https://learn.co/lessons/selecting-single-elements-readme'>Selecting Single Elements Readme</a> on Learn.co and start learning to code for free.</p>
