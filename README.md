### For user

* visit https://ant-tool.github.io/


### For developer


1. confirm you are developing in develop branch

2. download dependencies
	
	```bash
	npm i
	```

3. developing local

	```bash
	npm run dev
	```

4. deploy github master

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
  <video id="my-video" class="video-js" controls preload="auto" width="640" height="264"
  poster="MY_VIDEO_POSTER.jpg" data-setup="{}">
  <source src="MY_VIDEO.mp4" type='video/mp4'>
  <p class="vjs-no-js">
    To view this video please enable JavaScript, and consider upgrading to a web browser that
    <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a>
  </p>
  </video>
{% endraw %}
```