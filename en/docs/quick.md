# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > Quick start

## Quick start

We use JSON format for any data exchange.
Use `POST` to send a request, and put JSON object into [the body of that request](params/post.md).
To use Vozovoz API you need [an authentication token (auth token)](params/auth.md) for every request.
You can put it in [Query](params/get.md) or in a [request body](params/post.md).

Here is an example in JavaScript, you can easily use it in developer console of any web browser:

```javascript
var xhttp = new XMLHttpRequest();
// request to demo server
xhttp.open("POST", "https://vozovoz.org/api/?token=yourToken", false); // replace "yourToken" with your auth token
// xhttp.open("POST", "https://vozovoz.ru/api/?token=yourToken", false); // for a production server
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.send(JSON.stringify({
// Request body, i.e.
    "object": "version",
    "action": "get"
}));
// print the response immediately in the console
xhttp.responseText;
// or on the webpage
//document.body.innerHTML = xhttp.responseText;
```

Same example without commentary:

```javascript
var xhttp = new XMLHttpRequest();
xhttp.open("POST", "https://vozovoz.org/api/?token=", false);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.send(JSON.stringify({
    "object": "version",
    "action": "get"
}));
document.body.innerHTML = xhttp.responseText;
```

***
[▲ Up](#up)