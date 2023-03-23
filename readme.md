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

```Javascript
// Serve side code
const express = require('express');
const fileUpload = require('express-fileupload');
const app = express();

app.use(express.static('public'));
app.use(express.text());
app.use(fileUpload());
app.use(express.raw({ type: 'image/*', limit: '5mb' }));

const port = 3000;
app.post('/api/single-file', (req, res) => {
    const contentType = req.header('content-type');
    if (contentType.includes('text/plain')) {
        res.set('Content-Type', 'text/plain');
        res.send(req.body);
    } else if (contentType.includes('multipart/form-data')) {
        const f = req.files.myfile;
        res.set('Content-Type', 'text/html');
        f.mv('./uploads/' + f.name);
        res.send(`
            <table>
                <tr><td>Name</td><td>${f.name}</td></tr>
                <tr><td>Size</td><td>${f.size}</td></tr>
                <tr><td>MIME type</td><td>${f.mimetype}</td></tr>
            </table>
        `);
    } else {
        res.set('Content-Type', contentType);
        res.send(req.body);
    }
});
...
```

### Task: Styling File Input Elements

1. Ref <https://app.pluralsight.com/course-player?clipId=2988c1e3-0039-42dc-97f3-5a170666a858>

![html form](/images/html-form-customised.jpg "html form")

```html
<body>
    <form method="post" action="/api/single-file" enctype="multipart/form-data">
      <input
        type="file"
        name="myfile"
        id="myfile"
        style="display: none"
      /><br />
      <!-- Added-->
      <button type="button" id="uploadButton">Choose a file to upload</button>
      <div>Selected filename:<span id="filename"></span></div>
      <button type="submit">Submit</button>
    </form>
    <script>
      const fileTag = document.getElementById('myfile');
      const filenameElem = document.getElementById('filename');
      document.getElementById('uploadButton').addEventListener('click', () => {
        fileTag.click();
      });
      fileTag.addEventListener('change', () => {
        filenameElem.innerHTML = fileTag.files[0].name;
      });
    </script>
  </body>
```