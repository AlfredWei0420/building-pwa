# Building a Vue PWA

## Prerequisite

[NodeJS (npm)](https://nodejs.org/en/)

[Visual Studio Code](https://code.visualstudio.com/)

[Chrome](http://www.google.com/chrome/)

[Optional] - [yarn](https://yarnpkg.com/en/)

## vue-cli

1. Install `vue-cli` globally

    ```javascript
    npm install -g vue-cli
    ```

2. Initialise our Vue app

    ```javascript
    vue init pwa vue-hacker-news
    cd vue-hacker-news
    ```
    `npm i` or `yarn`

3. Run app in dev mode

   `npm run dev` or `yarn dev`

4. Update `Hello.vue` and observe hot update (Webpack Hot Moduel Replacement)

5. Try to find service worker in Chrome Dev Tools

6. Build app

   `npm build` or `yarn build`

   ​And then serve with a static server

    ```javascript
    npm i -g serve
    serve -s build
    ```

7. Observe service worker and offline mode 🙌


### REST API

hacker news API  
https://hacker-news.firebaseio.com/v0/topstories.json

https://hacker-news.firebaseio.com/v0/item/15066729.json

10. Display posts from hacker news `topstories.json`

  - Install `axios`

       `yarn add axios`

  - Display posts on home page

   ```javascript
    data () {
      return {
        msg: 'Welcome to Vue.js PWA',
        posts: []
      }
    },
    methods: {
      fetchItem: function(id) {
        axios.get(`https://hacker-news.firebaseio.com/v0/item/${id}.json`)
          .then((res) => {
            this.posts.push(res.data);
          })
          .catch((err) => {
            console.log("Error getting posts ", err)
          });
      }
    },
    created() {
      axios.get("https://hacker-news.firebaseio.com/v0/topstories.json")
          .then((res) => {
             res.data.slice(0, 10).map((id) => this.fetchItem(id));
          })
          .catch((err) => {
            console.log("Error getting posts ", err)
          });
    }
   ```
    ```javascript
      <ul>
        <li v-for="post in posts">
          {{ post.title }}
        </li>
      </ul>
    ```

11. `SWPrecacheWebpackPlugin` 

    ```javascript
    runtimeCaching: [
      {
        urlPattern: /^https:\/\/hacker-news\.firebaseio\.com\//,
        handler: 'cacheFirst'
      }
    ],
    ```

**Styles**

```css
.post-list {
  list-style: none;
  padding: 30px;
  max-width: 800px;
  text-align: left;
  margin: 0 auto;
}

.post-item {
  padding: 10px;
  border: 1px solid lightgrey;
  margin-top: -1px;
  display: block;
}

.post-title {
  font-weight: bold;
  text-decoration: none;
  color: chocolate;
}

.post-by {
  margin-left: 45px;
  padding-top: 5px;
  color: dimgrey;
}

.post-score {
  margin-right: 15px;
  width: 30px;
  display: inline-block;
  text-align: right;
}
```

```vue
<ul class="post-list">
  <li v-for="p in posts" class="post-item">
      <span class="post-score">{{p.score}}</span>
      <span class="post-title">
        {{p.title}}
      </span>
      <div class="post-by"> by {{p.by}} - {{p.descendants}} comments</div>
  </li>
</ul>
```

