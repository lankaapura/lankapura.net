---
title: "Copy to Clipboard Using Javascript"
header:
description: How to copy a text to clipboard?
og_image: 
---

Ever wanted to copy something to the clipboard programmatically using javascript? Here's how do it for few diffrent use cases.

## Simple Use Cases

We can you following simple function to copy some variable to clip board.

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

We can use [clipboardjs](https://clipboardjs.com/) to implement advanced requirements. You can set it up with few lines of code and it doesn't have any external dependencies and it's only 3KB.

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

  | <i class="fa fa-chrome" aria-hidden="true"></i> | <i class="fa fa-edge" aria-hidden="true"></i> | <i class="fa fa-firefox" aria-hidden="true"></i> | <i class="fa fa-internet-explorer" aria-hidden="true"></i> | <i class="fa fa-opera" aria-hidden="true"></i> | <i class="fa fa-safari" aria-hidden="true"></i> |
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

