## Dynamic WebVTT for HTML5 Videos in Internet Explorer

If you are a Front End Developer, you know how much we all love debugging IE bugs. I wanted to save you some time, and write this post in case you are trying to be fancy with the WebVTT.

### Lets get started!

Recently, I was trying to dynamically load a .vtt file, and enable custom CC controls on my video player, if the text tracks were available. I added an event listener to the track element, to listen to the successful loading of the text tracks. The code worked beautifully in all browsers but IE. I dug through the <track> element's documentation on MSDN for hours to find a weird difference in other browsers vs IE when it comes to the textTracks Object.

I'm going to use the following video as an example.

```javascript
<video id="demo" width="50%" height="50%" controls crossorigin="anonymous">
	<source src="demo.mp4" type="video/mp4">
</video>
```

First I'm going to create a track element and append it to the video. For simplicity I am not going to AJAX the .vtt file in this post. Here's what the code looks like:

```javascript
var videoEl = document.getElementById("demo");

var tt = document.createElement('track');

tt.setAttribute('src', 'tt-demo.vtt');
tt.setAttribute('id', 'tt-en-demo');
tt.setAttribute('label', 'English');
tt.setAttribute('kind', 'captions');
tt.setAttribute('srclang', 'en');

videoEl.appendChild(tt);
```

Now if you have an event listener bound to the textTracks object that does something fancy it will not work in IE . 

```javascript
var ttObj = videoEl.textTracks;
ttObj.addEventListener('addtrack', function (evt) {
		  console.log('Do something fancy...');
});
```

In IE the video will have both textTracks object and `audioTracks` object, the event listener will work when bound to the `audioTracks` object.

```javascript
var atObj = videoEl.audioTracks;
atObj.addEventListener('addtrack', function (evt) {
		  console.log('Do something fancy...');
});
```
