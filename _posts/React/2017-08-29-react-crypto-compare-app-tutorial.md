---
layout: post
title: Create React Crypto App
category: javascript
tags: [React]
comments: true
---

# React Crypto Compage App
> Credit - Scotch.io's Vue.js tutorial [View Original Tutorial](https://scotch.io/tutorials/build-a-cryptocurrency-comparison-site-with-vuejs)

I often go to scotch.io to see the tutorials and I saw the Vue.js tutorial to make crypto compare app. I do not know about Vue.js and I am focusing on React thesedays, so I decided to follow along this tutorial but with React. And also I want to learn by doing, so I am try to not see the how the author implemented the features with Vue.js and tried to come up with my own logics. 

First lets setting our environment.

```sh
$ npm install create-react-app -g
$ create-react-app react-crypto-comapre-app
```
Now `create-react-app` will create a folder called `react-crypto-compare-app` and install all dependencies that need to build React app. We will use `Bulma` css library to make our app more beautiful. so lets install `Bulma.css`

```sh
$ npm install bulma font-awesome --save
```

src/index.js
```js
import React from 'react';
import ReactDOM from 'react-dom';
import '../node_modules/bulma/css/bulma.css'; //import bulma css from node_modules
import '../node_modules/font-awesome/css/font-awesome.min.css'; //import font-awesome from node_modules

ReactDOM.render(<App />, document.getElementById('root'));
```

Lets create a folder called `components` and create `Nav, Hero, Table, Footer` components.

src/components/Nav.js
```js
import React from "react";
import logo from "../images/bitcoin.svg";

const Nav = props =>
  <section className="">
    <nav className="navbar">
      <div className="navbar-brand">
        <a className="navbar-item" href="#">
          <img src={logo} alt="Crypto Currency" width="112" height="35" />
          <span>Crypto Compare</span>
        </a>

        <div className="navbar-burger">
          <span />
          <span />
          <span />
        </div>
      </div>

      <div className="navbar-end">
        <a
          className="navbar-item"
          href="https://github.com/yeondam88/react-crpyto-compare-app"
        >
          <i className="fa fa-github" />&nbsp;Source
        </a>
      </div>
    </nav>
  </section>;

export default Nav;
```

src/components/Hero.js
```js
import React from "react";

const Hero = props =>
  <section className="hero is-warning is-bold is-medium">
    <div className="hero-body">
      <div>
        <h1 className="title has-text-centered">Crypto Compare</h1>
        <p className="has-text-centered description">
          This website indexes the top 10 cryptocurrencies by market cap (how
          much the sum of all coins is collectively worth), and gives you an
          easy way to compare cryptocurrency performance and rank over the last
          week.
        </p>
        <p className="has-text-centered explaination">
          This tutorial app originally written in <strong>Vue</strong> from
          <a href="https://scotch.io/tutorials/build-a-cryptocurrency-comparison-site-with-vuejs">
            <span className="button is-small is-info">Scotch.io</span>
          </a>
          <br />Re-write with <strong>React</strong>
        </p>
      </div>
    </div>
  </section>;

export default Hero;
```

We will make utils folder under `src` directory and create API.js file.

```js
import axios from "axios";

export let CRYPTOCOMPARE_API_URI = "https://www.cryptocompare.com";
export let COINMARKETCAP_API_URI = "https://api.coinmarketcap.com";
export let UPDATE_INTERVAL = 60 * 1000;

export async function getCoins() {
  try {
    const response = await axios.get(
      `${COINMARKETCAP_API_URI}/v1/ticker/?limit=10`
    ); // fetching the data from API
    const coins = response.data;
    return coins;
  } catch (error) {
    console.error(error);
  }
}

export async function getCoinData() {
  try {
    const response = await axios.get(
      `${CRYPTOCOMPARE_API_URI}/api/data/coinlist`
    ); // fetching the data for each coin's symbol images.
    const coinData = response.data.Data;
    return coinData;
  } catch (error) {
    console.error(error);
  }
}
```

src/components/
```js
import React from "react";
import { getCoinData, CRYPTOCOMPARE_API_URI } from "../utils/API";

class Table extends React.Component {
  state = {
    coinData: {}
  };

  componentDidMount() {
    getCoinData().then(coinData => {
      this.setState(state => ({ coinData }));
    });
  }

  getImgUrl = symbol => {
    if (this.state.coinData[symbol]) {
      return this.state.coinData[symbol].ImageUrl;
    } else {
      return null;
    }
  };

  getCoinUrl = symbol => {
    if (this.state.coinData[symbol]) {
      return this.state.coinData[symbol].Url;
    } else {
      return null;
    }
  };

  render() {
    const { isFetching, coins } = this.props;
    const { coinData } = this.state;
    if (isFetching) {
      return <h1>Loading...</h1>;
    }
    return (
      <table className="table">
        <thead>
          <tr>
            <th>Rank</th>
            <th>Name</th>
            <th>Symbol</th>
            <th>Price (USD)</th>
            <th>1H</th>
            <th>1D</th>
            <th>1W</th>
            <th>Market Cap (USD)</th>
          </tr>
        </thead>
        <tbody>
          {coins.map(coin => {
            const imgUrl = `${CRYPTOCOMPARE_API_URI}${this.getImgUrl(
              coin.symbol
            )}`;
            const coinUrl = `${CRYPTOCOMPARE_API_URI}${this.getCoinUrl(
              coin.symbol
            )}`;
            return (
              <tr>
                <td>
                  {coin.rank}
                </td>
                <td>
                  <img src={imgUrl} />
                  <a style={{ color: "#000" }} href={coinUrl} target="_blank">
                    {coin.name}
                  </a>
                </td>
                <td>
                  {coin.symbol}
                </td>
                <td>
                  ${parseFloat(coin.price_usd).toFixed(2)}
                </td>
                <td
                  style={
                    coin.percent_change_1h > 0
                      ? { color: "green" }
                      : { color: "red" }
                  }
                >
                  {coin.percent_change_1h > 0
                    ? "+" + coin.percent_change_1h
                    : coin.percent_change_1h}%
                </td>
                <td
                  style={
                    coin.percent_change_24h > 0
                      ? { color: "green" }
                      : { color: "red" }
                  }
                >
                  {coin.percent_change_24h > 0
                    ? "+" + coin.percent_change_24h
                    : coin.percent_change_24h}%
                </td>
                <td
                  style={
                    coin.percent_change_7d > 0
                      ? { color: "green" }
                      : { color: "red" }
                  }
                >
                  {coin.percent_change_7d > 0
                    ? "+" + coin.percent_change_7d
                    : coin.percent_change_7d}%
                </td>
                <td>
                  ${parseInt(coin.market_cap_usd).toLocaleString()}
                </td>
              </tr>
            );
          })}
        </tbody>
      </table>
    );
  }
}

export default Table;
```

src/components/Footer.js
```js
import React from "react";

const Footer = props => {
  return (
    <footer className="footer">
      <div className="container">
        <div className="content has-text-centered">
          <p>
            <strong>This site made with ❤</strong> by{" "}
            <a href="https://github.com/yeondam88">Lloyd Park</a>. The source
            code is licensed
            <a href="http://opensource.org/licenses/mit-license.php"> MIT</a>.
            The website content is licensed{" "}
          </p>
          <p>
            <a
              className="icon"
              href="https://github.com/yeondam88/react-crpyto-compare-app"
            >
              <i className="fa fa-github" />
            </a>
          </p>
        </div>
      </div>
    </footer>
  );
};

export default Footer;
```

src/App.js
```js
import React, { Component } from "react";
import { Nav, Hero, Table, Footer } from "./components";
import { getCoins, UPDATE_INTERVAL } from "./utils/API";

class App extends Component {
  state = {
    coins: [],
    isFetching: false
  };

  fetchingCoin = () => {
    getCoins().then(coins => {
      if (coins) {
        this.setState({
          coins
        });
      } else {
        this.setState({
          isFetching: true
        });
      }
    });
  };

  componentDidMount() {
    setInterval(this.fetchingCoin(), UPDATE_INTERVAL);
  }

  render() {
    const { coins, coinData, isFetching } = this.state;
    return (
      <div>
        <Nav />
        <div className="container">
          <Hero />
          <section className="container content-area">
            <Table isFetching={isFetching} coins={coins} />
          </section>
          <Footer />
        </div>
      </div>
    );
  }
}

export default App;
```

Here is the page will look like :)

---
<img src="/assets/images/react-crypto-app.png">



