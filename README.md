### For user

* visit https://ant-tool.github.io/


### For developer


1. confirm you are developing in develop branch

2. download dependencies
	
	```bash
	npm i
	```

3. download gitbook plugins
  
  ```bash
  npm run gitbook-install
  ```

4. developing local

	```bash
	npm run dev
	```

5. deploy github master

	```bash
	npm run deploy
	```

### FAQ 

**how to add gitbook plugins**

1. modify book.json

  `book.json`
  
  ```javascript
  
  {
    "plugins": [ "plugin-name" ]
  }
  
  ```
2. install plugin
  
  ```bash
	./node_modules/.bin/gitbook install
	```
 
**plugin `local-video` is already built-in**

You can include the Video.js markup where you want it using raw GitBook tags

```javascript
{% raw %}
  <video id="my-video" class="video-js" controls preload="auto">
    <source src="sd1434876536_2.mp4" type='video/mp4'>
  </video>
{% endraw %}
```