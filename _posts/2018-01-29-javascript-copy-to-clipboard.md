---
title: Copy to Clipboard Using Javascript
header: null
description: How to copy a text to clipboard?
og_image: null
published: true
---

<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/font-awesome/4.1.0/css/font-awesome.min.css">
Ever wanted to copy something to the clipboard programmatically using javascript? Here’s how to do it for few different use cases.

## Simple Use Cases

We can use following simple function to copy any variable to clipboard.

```javascript
  function CopyToClipBoardHandler(value) {
    const copyListener = event => {
      document.removeEventListener('copy', copyListener, true);
      event.preventDefault();
      const clipboardData = event.clipboardData;
      clipboardData.clearData();
      clipboardData.setData('text/plain', value);
    };
    document.addEventListener('copy', copyListener, true);
    document.execCommand('copy');
  }
}
```

### Demo

 <div class="wrapper">
  <button class="button" id="copy1">
  Click to copy
  </button>
</div>

## Advanced Use Cases

We can use [clipboardjs](https://clipboardjs.com/) to implement advanced requirements. You can set it up with few lines of code, it doesn’t have any external dependencies, and it’s only 3KB.

```html
    <!-- 1. Define some markup -->
    <div id="btn" data-clipboard-text="1">
        <span>Copy</span>
    </div>

    <!-- 2. Include library -->
    <script src="clipboard.min.js"></script>

    <!-- 3. Instantiate clipboard by passing a HTML element -->
    <script>
    var btn = document.getElementById('btn');
    var clipboard = new Clipboard(btn);
    clipboard.on('success', function(e) {
        console.log(e);
    });
    clipboard.on('error', function(e) {
        console.log(e);
    });
    </script>
```
More examples can be found on [github](https://github.com/zenorocha/clipboard.js/tree/master/demo).

### Demo
 <div class="wrapper">
  <button class="button" id="copy2" data-clipboard-text="Hello Clip Board!! I am from clipboardJS.">
  Click to copy
  </button>
</div>

## Important

- Copy commands triggered from document.execCommand() will only work if the event is dispatched from an event that is trusted and triggered by the user.

- Browser compatibility

  | <img src="http://www.lankapura.net/assets/images/browsers/chrome.png" width="48px" height="48px" alt="Chrome logo"> | <img src="http://www.lankapura.net/assets/images/browsers/edge.png" width="48px" height="48px" alt="Edge logo"> | <img src="http://www.lankapura.net/assets/images/browsers/firefox.png" width="48px" height="48px" alt="Firefox logo"> | <img src="http://www.lankapura.net/assets/images/browsers/ie.png" width="48px" height="48px" alt="Internet Explorer logo"> | <img src="http://www.lankapura.net/assets/images/browsers/opera.png" width="48px" height="48px" alt="Opera logo"> | <img src="http://www.lankapura.net/assets/images/browsers/safari.png" width="48px" height="48px" alt="Safari logo"> |
  |:---:|:---:|:---:|:---:|:---:|:---:|
  | 42+ | 12+ | 41+ | 9+ | 29+ | 10+ |

<script src="https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/1.7.1/clipboard.min.js"></script>

<script type="text/javascript">

function CopyToClipBoardHandler(text) {
  const copyListener = event => {
    document.removeEventListener("copy", copyListener, true);
    event.preventDefault();
    const clipboardData = event.clipboardData;
    clipboardData.clearData();
    clipboardData.setData("text/plain", text);
  };
  document.addEventListener("copy", copyListener, true);
  document.execCommand("copy");
}

var uc1 = 'Hello Clip Board!! I am from a varaible.';
var button1 = document.getElementById("copy1");

button1.addEventListener("click", function(e) {
  e.preventDefault();
  CopyToClipBoardHandler(uc1);
});

var btn = document.getElementById('copy2');
var clipboard = new Clipboard(btn);
clipboard.on('success', function(e) {
    console.log(e);
});
clipboard.on('error', function(e) {
    console.log(e);
});
</script>
