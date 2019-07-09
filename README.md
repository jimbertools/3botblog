# 3botblog

## How to launch this project in your local environment

Run a gunDB of your choice  
conection strings are currently multiple times in the code  
Look for `Gun("ws://localhost:8000/gun")`

Run the following commands:

``` bash
npm install
npm run serve
```

Then you can access it at [http://localhost:8080](http://localhost:8080).  

Deleting posts is not working!

Dummy data, just paste in console of browser
```javascript
var gun = Gun() // Or Gun("ws://ip:port")
var post1 = {
	id: 1,
	title: "Look at my cat",
	date: new Date().toString(),
	description: "My cat",
	body: "My cat is the best cat. You have never seen such a good cat",
	image: "cat01.jpg"
}
var post2 = {
	id: 2,
	title: "Look at my duck",
	date: new Date().toString(),
	description: "My duck",
	body: "My duck is the best duck. You have never seen such a good duck. A big yellow duck",
	image: "duck01.jpg"
}
var posts = gun.get('posts')
posts.set(post1)
posts.set(post2)

gun.get("headline").put({
	image: "tf.jpg",
	title: "Welcome to my blog",
	description: "This blog is running on 3Bot :D"
})

```


## Project setup
``` bash
npm install
```

### Compiles and hot-reloads for development
``` bash
npm run serve
```

### Compiles and minifies for production
``` bash
npm run build
```


#Template used to create blog

# Prismic Vue.js Example Blog

> [Vue.js](https://vuejs.org) example blog project with content managed in [Prismic](https://prismic.io)

## Learn more about using Prismic with Vue.js

> [Prismic Vue.js Documentation](https://prismic.io/docs/vuejs/getting-started/with-the-vuejs-starter)

## License

This software is licensed under the Apache 2 license, quoted below.

Copyright 2013-2019 Prismic (http://prismic.io).

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this project except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0.

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
