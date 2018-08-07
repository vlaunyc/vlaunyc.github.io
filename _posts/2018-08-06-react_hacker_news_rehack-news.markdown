---
layout: post
title:      "React + Hacker News = Rehack-News"
date:       2018-08-06 18:09:19 -0400
permalink:  react_hacker_news_rehack-news
---


For my final project I decided to make a hacker news client for viewing top stories, searching stories, and the ability to save stories to read later. I wanted the user interface to be clean, minimal, and easy to read. I basically wanted a prettier [hacker news](https://news.ycombinator.com/) with save and search functionality.  

<img width="700" src="https://i.imgur.com/SVtQngg.png">
<img width="700" src="https://i.imgur.com/PTa6u8t.png">
<img width="700" src="https://i.imgur.com/LocIIHj.png">

**There were a few things I wanted to add in addition to requirements for the project.**
1. Connect to a live API
2. Use [styled-components](https://www.styled-components.com/)
3. Add user auth with JWT
 
…I didn’t get to #3 user auth with JWT, but I’m fetching from 3 API’s and was able to implement styled components :)

My app has three pages the **main search page**, the **top stories page**, and the **saves page**. Each of these pages are fetching data in different ways. The Search page is fetching from the Algolia Hacker News Search API which allows you to query search terms. Top stories is fetching from the Hacker News Firebase API which is a real time API, this API gives an updating array of the top stories. From this array I then make a subsequent request to the Hacker News Algolia search API for the stories info such as title, url, comments. The Saves page is fetching from the backend Rails API.

My backend simply handles the data persistence, creates and destroys 'saves' and has an index of all saves. While my front-end handles the display of data and uses fetch() within actions to GET and POST data from the API. For hooking up a React-Redux front-end to a Rails API backend, I followed this guide: 
[how-to-get-create-react-app-to-work-with-your-rails-api](https://www.fullstackreact.com/articles/how-to-get-create-react-app-to-work-with-your-rails-api/). 

I used [react-router](https://www.npmjs.com/package/react-router-dom) to create 3 routes/pages which I set up in the app component.

```
// rehack-news/rehack-news-client/src/components/App.js

import React, { Component } from 'react';
import { BrowserRouter as Router, Route} from 'react-router-dom';

import NavBar from './common/NavBar'
import SearchContainer from './mainPage/SearchContainer'
import SavesContainer from './savesPage/SavesContainer'
import TopStoriesContainer from './topStoriesPage/TopStoriesContainer'

class App extends Component {

  render() {
    return (
      <Router>
         <div className='App'>
          <NavBar />
          <Route exact path="/" component={SearchContainer}/>
          <Route exact path="/topstories" component={TopStoriesContainer}/>
          <Route path="/saves" component={SavesContainer}/>
        </div>
      </Router>
    )
  }
}

export default App;


```
I'm importing the *smart components* for my Router, which are my *container components* i.e.: **SearchContainer**, **SavesContainer**, and **TopStoriesContainer**. Each of these container components are the main pages to my app and are connected to Redux store.  This is how the Async flow works:  
1. The store dispatches a fetch action which sends an API request
2. When that action receives a successful response from the API it dispatches an action to the store
3. The store forwards that action to the reducers
4. The reducer handles the action and creates a copy of the state
5. This new state is available to container components that are subscribed to the store
7. The *container components* pass props down to my *presentational components (aka dumb components)*. My *presentational components* are only in charge of displaying props and [styled-components](https://www.styled-components.com/). 

Here's a great tutorial for configuring Redux store, action creators and defining reducers: [react-redux-tutorial](https://www.thegreatcodeadventure.com/react-redux-tutorial-part-iii-async-redux/).
<br>

and now I have a confession to make... 
> **I LOVE STYLED COMPONENTS!**  
> 
> Styled-components brings together classic CSS syntax with component scoping and adds conditional styles by passing in props. Styled-components turns CSS into presentational components that can be exported like any other component and imported where needed. 

For implementing styled-component, check out the [docs on getting-started]( https://www.styled-components.com/docs/basics#getting-started
).

<br>

**WHAT'S GREAT ABOUT STYLED COMPONENTS?**

* **Scoped styles while using normal CSS syntax.** 
* **Passed props**
* **Reusability**
* **Extended styles**

Here's an example of ***buttons*** that demonstrate the use of CSS syntax, passing props, reusability, and the extension of styles.

I have a button component
```
// rehack-news/rehack-news-client/src/components/common/Button.js

import React from 'react';
import PropTypes from 'prop-types';

const Button = ({ onClick, className, children }) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

Button.defaultProps = {
  className: '',
};
Button.propTypes = {
  onClick: PropTypes.func.isRequired,
  className: PropTypes.string,
  children: PropTypes.node.isRequired,
};

export default Button;

```

which is imported into styles.js
```
// rehack-news/rehack-news-client/src/components/style.js

import styled from 'styled-components';
import Button from './common/Button';

export const HeartButton = styled(Button)`
  background-color: transparent;
  height: 2rem;
  width: 2rem;
  min-width: 2rem;
  max-width: 2rem;
  line-height: 2rem;
  border: transparent;
  border-radius: 1rem;
  align-items: center;
  outline: none;
  border: none;
  color: #ffb74d;
  font-size: .8rem;
  margin-right: 15px;
  &:hover{
    background-color: #f57c00;
    color: #FFF;
  }
`

export const XButton = styled(HeartButton)`
  color: #424242;
`
```
<img width="300" src="https://i.imgur.com/eSxLXqd.png">

In styles.js my **HeartButton** is a **styled(Button)** and my **XButton** is an extension of the HeartButton!  

These styled buttons are imported and used in all of my presentational components.

```
import React from 'react';
import {
  XButton,
  HeartButton
} from '../style';

const Table = ({ stories, onDismiss, onSave }) =>
  <div className="table">
    {stories.map(item =>
      <Wrapper key={item.objectID}>
      ...
        <HeartButton onClick={() => onSave(item)}>
          <i className="fas fa-heart"></i>
        </HeartButton>
  	...
        <XButton onClick={() => onDismiss(item.objectID)}>
          &#10006;
        </XButton>
  	...
      </Wrapper>
    )}
  </div>
export default Table;

```

**SUPER COOL!  right?** 

Over all I learned a lot from this project. It was my first React-Redux app and really helped me understand Async flow and the manner in which the app retrieves data and makes it available to the components. I really enjoyed learning how to implement styled-components too. 

