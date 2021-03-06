---
date: 2016-08-15T15:23:17+02:00
title: LinedUp
language: JavaScript
project_url: https://github.com/kevingimbel/LinedUp
---
LinedUp is an app/website I build to get into Node.js, MongoDB and ExpressJS. It's a
collection of concerts I've been to that I'm aiming to update so it soon holds
all concerts I've ever been to. Getting together all information is quite a task.

<!--more-->

[Live Demo](http://concerts.kevingimbel.me)

### Stack
LinedUp is written in Node.js - the underlying Server is Express. The database I
used is MongoDB which I enjoy working with so far. For the Front-End I'm using
an self-made Ajax Function to fetch data and a `RenderEngine` to create the HTML
- both of these should never ever be used in production so don't copy them to
  your thingy! Better use some real stuff.

### API

There's a basic API build with Express to get the data. The endpoints are as
follows.

|      Endpoint     | Type  | Return                                                       |
|:------------------|:-----:|:-------------------------------------------------------------|
| /api/concerts     | GET   | List of all concerts, sorted by: Date -> Venue -> Name       |
| /api/concerts/:id | GET   | Get only the data of a specific concert by ID¹               |
| /api/concerts/filters | GET | Get fields but only once. Used for navigation*             |
* ¹ ID is not exposed yet

* The `/api/concerts/filters` Endpoint returns a reduced dataset. You'll get all the venues, dates, bands, etc. sorted by keys but every band, city and venue is only added once. The data then looks as follows:

```js
{ "venue":
    ["Jahrhunderthalle","Schlachthof","ZOOM",...],
  "city":
    ["Frankfurt","Wiesbaden","Mayence",...],
  "artists":
    ["Beatsteaks","Kraftklub","Bloody Beetroots",...]
}
```

### Concert Model
The Concert Data Model currently only consists of some meta data.

```js
{
  name: String,
  venue: String,
  city: String,
  country: String,
  date: String
}
```
|      Key          | Value                    |
|:------------------|--------------------------|
| name              | Band/Artist name         |
| venue             | Concert hall or festival |
| city              | City the event was in    |
| country           | Country the event was    |
| date              | Date of the event; currently only year |

### Additional stuff

##### renderer.js
Renderer.js is a script that renders HTML from plain JSON. It takes a junk of data, a template String and replaces every occurrence of a key inside the string with the value from the JSON.

**Usage**
```js
var template = '<div>Hello {name}</div>';
var data = {
  name: 'Kevin'
}
var Renderer = new RenderEngine();
Renderer.setData(data).setTemplate(template);
var output = Renderer.render();
console.log(output); // => <div>Hello Kevin</div>
```

##### Ajax.js
Wrapper around [`XMLHttpRequest`](https://developer.mozilla.org/de/docs/Web/API/XMLHttpRequest) to handle POST and GET calls to the API endpoints.

**Usage**
```js
Ajax.get('http://localhost:1337/api/concerts', function(data) {
  var content = document.querySelector('#content');  
  var Renderer = new RenderEngine();
  // Configure the renderer.
  Renderer.setTemplate(template);
  // Set the render data
  Renderer.setData(JSON.parse(data.response));
  // Render the HTML
  var htmlStr = Renderer.render();
  content.innerHTML = htmlStr;
});
```
