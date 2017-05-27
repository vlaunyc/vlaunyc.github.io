---
layout: post
title:  Lesser Known Elements
date:   2017-05-27 20:24:42 +0000
---


### Let's talk about the lesser-known elements and features that have been added to HTML5.

#### **The `details` Element**
This element is a collapsible box that has a title, and more info or functions that are hidden away. Normally this kind of widget is created using a combination of HTML and JavaScript, but HTML5 removes the scripting required and simplifies it. <br><br>
Here's how it looks in code:
```
<details>
  <summary>All of the things we will discuss</summary>
  <ul>
    <li>Topic 1</li>
      <p> Information on Topic 1 </p>
    <li>Topic 2</li>
      <p> Information on Topic 2 </p>
    <li>Topic 3</li>
      <p> Information on Topic 3 </p>
  </ul>
</details>
```
Here’s how it looks published:
<details>
  <summary>All of the things we will discuss</summary>
  <ul>
    <li>Topic 1</li>
      <p> Information on Topic 1 </p>
    <li>Topic 2</li>
      <p> Information on Topic 2 </p>
    <li>Topic 3</li>
      <p> Information on Topic 3 </p>
  </ul>
</details>
<br>
In the above example, the contents of the `summary` element will appear to the user, but the remainder of the content is hidden. When you click on `summary` the hidden contents will appear below.

However, if  `details` lacks a defined  `summary`, the browser will define a default summary. In the case where you would want the hidden content to be visible by default, you can use a Boolean  `open` attribute on the  `details`.

Another rule is that the `summary` element must be a child of `details` and it must be the first child used.
<br><br>

#### **The `picture` element**
The `picture` element is a recent addition to HTML5 which lends itself to responsive web design with responsive images. `picture` lets you define multiple image sources, allowing users on smaller devices like mobile browsers to download a low resolution version of the image, while offering a larger image for tablets and desktops. 

The `srcset`, `sizes`, `picture` and  `source` attributes give developers more flexibility in specify image resources. Essentially instead of having one image that is scaled up or down based on the screen width, multiple images can be designed to more appropriately fill the browser viewpoint. 

See below for example :

![Non-Responsive image](https://www.html5rocks.com/en/tutorials/responsive/picture-element/resized-image.png) 
![Responsive Image](https://www.html5rocks.com/en/tutorials/responsive/picture-element/art-direction.png)

By providing multiple image sources, while using the `<picture>` or `<img>` with the `srcset` and `sizes` attribute, the users browser will download the image that matches to the device. Here's what the code may look like:

```
<picture>
	<!-- 16:9 crop -->
	<source
		type="image/webp"
		media="(min-width: 36em)"
		srcset="paddle_boat/detail/large.webp  1920w,
		        paddle_boat/detail/medium.webp  960w,
		        paddle_boat/detail/small.webp   480w" />
	<source
		media="(min-width: 36em)"
		srcset="paddle_boat/detail/large.jpg  1920w,
		        paddle_boat/detail/medium.jpg  960w,
		        paddle_boat/detail/small.jpg   480w" />
	<!-- square crop -->
	<source
		type="image/webp"
		srcset="paddle_boat/square/large.webp   822w,
		        paddle_boat/square/medium.webp  640w,
		        paddle_boat/square/small.webp   320w" />
	<source
		srcset="paddle_boat/square/large.jpg   822w,
		        paddle_boat/square/medium.jpg  640w,
		        paddle_boat/square/small.jpg   320w" />
	<img
		src="quilt_2/detail/medium.jpg"
		alt="Image of man paddle boating with sunset in the background." />
</picture>
```

<br><br>

#### **The `download` attribute**
The `download` attribute allows you to set a separate file download name than the actual link endpoint itself. 

Download the file while clicking on the link instead of navigating to the file. <br><br>
<a href="https://static1.squarespace.com/static/55d115e0e4b03088e37f9e38/t/5929da5720099e2f4b02331b/1495915097157/yay.pdf" download="yay.pdf">Download This Example</a>
<br><br>
Example in code:
```
<!-- will download as "yay.pdf: -->
<a href="https://static1.squarespace.com/static/55d115e0e4b03088e37f9e38/t/5929da5720099e2f4b02331b/1495915097157/yay.pdf" download="yay.pdf">Download This Example</a>

```

Note this attribute is only used if the href attribute is set.
The value of the attribute will be the name of the downloaded file. There are no restrictions on allowed values and the browser will automatically detect the correct file extension an add it the the file.


<br><br>
<hr />
**Source**<br><br>
Chen, P. (September 11, 2014). Built-in Browser Support for Responsive Images. https://www.html5rocks.com/en/tutorials/responsive/picture-element/
<br><br>
Walsh, D. (August 22, 2012). HTML5 Download Attribute. https://davidwalsh.name/download-attribute

