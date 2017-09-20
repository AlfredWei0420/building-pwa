# Building a React PWA

## Prerequisite

[NodeJS (npm)](https://nodejs.org/en/)

[Visual Studio Code](https://code.visualstudio.com/)

[Chrome](http://www.google.com/chrome/)

[Optional] - [yarn](https://yarnpkg.com/en/)

## create-react-app

1. Install `create-react-app` globally

    ```javascript
    npm install -g create-react-app
    ```

2. Initialise our React app

    ```javascript
    create-react-app hacker-news
    cd hacker-news
    ```

3. Folder structure

    ```javascript
    hacker-news
    ├── node_modules
    ├── public
    │   └── favicon.ico
    │   └── index.html
    │   └── manifest.json
    ├── src
    │   └── App.css
    │   └── App.js
    │   └── App.test.js
    │   └── index.css
    │   └── index.js
    │   └── logo.svg
    │   └── registerServiceWorker.js
    ├── .gitignore
    ├── package.json
    ├── README.md
    └── yarn.lock
    ```

4. Run app in dev mode

   `npm start` or `yarn start`

5. Open in browser

   Open [http://localhost:3000](http://localhost:3000/) to view initial app.

6. Update `App.js` and observe hot update (Webpack Hot Moduel Replacement)

7. Try to find service worker in Chrome Dev Tools

8. Build app

   `npm build` or `yarn build`

   ​And then serve with a static server

    ```javascript
    npm i -g serve
    serve -s build
    ```

9. Observe service worker and offline mode 🙌


### REST API

hacker news API  
https://hacker-news.firebaseio.com/v0/topstories.json

https://hacker-news.firebaseio.com/v0/item/15066729.json

10. Display posts from hacker news `topstories.json`

  - Install `axios`

       `yarn add axios`

  - Display posts on home page

   ```javascript
   constructor(props) {
       super(props);

       this.state = {
       posts: []
       };
   }
   ```

   ```javascript
    fetchItem(id) {
      axios.get(`https://hacker-news.firebaseio.com/v0/item/${id}.json`)
        .then((res) => {
          this.setState({
            posts: [...this.state.posts, res.data]
          })
        })re
        .catch((err) => {
          console.log("Error getting posts ", err)
        });
    }

    componentDidMount() {
      axios.get('https://hacker-news.firebaseio.com/v0/topstories.json')
        .then((res) => {
          res.data.slice(0, 10).map((id) => this.fetchItem(id));
        })
        .catch((err) => {
          console.log("Error getting posts ", err)
        });
    }
   ```
    ```jsx
        <ul>
          {this.state.posts.map((p) =>
            <li key={p.id}>
              <span>{p.score}</span>
              {p.title}
              <div> by {p.by} - {p.descendants} comments</div>
            </li>
          )}
        </ul>
    ```

11. CSS modules? Service workder cache?

    #### eject from create-react-app

    ```javascript
    yarn eject
    ```

    **Note: This is a one-way operation. Once you **`eject`**, you can’t go back!**

12. CSS modules

    ```javascript
    {
      loader: require.resolve('css-loader'),
      options: {
        ...
        modules: true,
        localIdentName: "[name]__[local]___[hash:base64:5]",
      },
    },
    ```

    ```javascript
    {
      loader: require.resolve('css-loader'),
      options: {
        ...
        modules: true,
      },
    },
    ```

13. `SWPrecacheWebpackPlugin` 

    ```javascript
    staticFileGlobsIgnorePatterns: [/\.map$/, /asset-manifest\.json$/],
    runtimeCaching: [
      {
        urlPattern: /^https:\/\/hacker-news\.firebaseio\.com\//,
        handler: 'cacheFirst'
      }
    ],
    ```

##### App-like look & feel

```html
<!-- Add to home screen for Safari on iOS -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="apple-mobile-web-app-title" content="hacker-news">
```

**Styles**

```css
.postList {
  list-style: none;
  padding: 30px;
  max-width: 800px;
  text-align: left;
  margin: 0 auto;
}

.postItem {
  padding: 10px;
  border: 1px solid lightgrey;
  margin-top: -1px;
}

.postTitle {
  font-weight: bold;
  text-decoration: none;
  color: chocolate;
}

.postBy {
  margin-left: 45px;
  padding-top: 5px;
  color: dimgrey;
}

.postScore {
  margin-right: 15px;
  width: 30px;
  display: inline-block;
  text-align: right;
}
```

```jsx
<ul className={styles.postList}>
  {this.state.posts.map((p) =>
    <li className={styles.postItem} key={p.id}>
      <span className={styles.postScore}>{p.score}</span>
      <span className={styles.postTitle}>
        {p.title}
      </span>
      <div className={styles.postBy}> by {p.by} - {p.descendants} comments</div>
    </li>
  )}
</ul>
```

