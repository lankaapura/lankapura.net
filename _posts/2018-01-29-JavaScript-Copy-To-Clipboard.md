---
title: "Copy to Clipboard in Javascript"
header:
description: How to copy text to clipboard?
og_image: 
---

Ever wanted to copy something to the clipboard programmatically using javascript? Here's how do it for few diffrent use cases.

## Simple Use Case

We can you following simple function to copy some variable to clip board. One important thing to remember is that Copy commands triggered from document.execCommand() will only work if the event is dispatched from an event that is trusted and triggered by the user.

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
 <div class="wrapper">
  <input id="uc1" value="I just copied this with only JavaScript"/>
  <button class="button" id="copy1">Click to copy</button>
</div>




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
</script>
