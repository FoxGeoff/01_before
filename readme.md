# PS Uploading Files with a JavaScript REST API

1. ref for security: <https://securityheaders.com>

## Project Introduction

### Task: Simple HTML Form

![html form](/public/html-form.jpg "html form")

```html
<!-- HTML Simple form  -->
<body>
    <form method="post" action="/api/single-file" enctype="multipart/form-data">
      <input type="file" name="myfile" id="myfile"><br />
      <button type="submit">Submit</button>
    </form>
```
