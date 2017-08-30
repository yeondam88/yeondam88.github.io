---
layout: post
title: Introduction React
category: javascript
tags: [javascript, react, es2015, es6]
comments: true
---

## Hello, React
React is popular JavaScript library made by Facebook. I have been studied Front End Development 2 years now, and at the beginning, I only used HTML, CSS, JavaScript, jQuery and never used any frameworks like Angular, Backbone, Knockout, etc.

I realized that I need to stop using jQuery and start using one of the famous JavaScript frameworks, and when I first time saw the React, I misunderstood about React and somehow I thought I must learn Redux in order to use React. As grow your app, you must use Redux for sure to manage your state easily, but for small size site, or web app do not need to Redux. The first impression of Redux was so confused and hard to understand, so I gave up on React. And I had a chance to work on Angular 2 project as one of the developers to build site. While I was developing the site with Angular, it was really fun and felt easy to work with other developers. Ever since then I kept on using Angular and never thought to learn React.

One day, I saw Udacity’s new nano degree called React Nanodegree, and I decided to give it a shot. Because I have been using Udacity’s free web development courses and really enjoyed it. Somehow to learning and getting new library React was not that hard. Maybe because I’ve learned Angular before I start to learning React. But as I go deeper in React, I felt that it is really just ‘JavaScript’. You do not need to learn a bunch of new APIs that required by frameworks.

Take a look code example.

```js
import React, { Componenet } from 'react';
import ReactDOM from 'react-dom';

class App extends Component {
  render() {
    return (
      <div>Hello, React</div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('app'));

```

To write a React app, you just need to use plain JavaScript ES6 syntax and a little bit about React side syntax, such as JSX(it will compile to JavaScript object).

I really like to use online editors stackblitz or codesandox to prototype it fast with zero configuration. Those are also built with React.

Next post, I will be writing about building small React app. You could [click](http://react-crypto.surge.sh) here to see the demo site