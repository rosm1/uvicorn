@@ -32,32 +32,29 @@ Install using `pip`:
$ pip install uvicorn
$ pip install uvicorn
```
```


Create an application, in `app.py`:
Create an application, in `example.py`:


```python
```python
class App():
async def app(scope, receive, send):
    def __init__(self, scope):
    assert scope['type'] == 'http'
        assert scope['type'] == 'http'
        self.scope = scope
    await send({
        'type': 'http.response.start',
    async def __call__(self, receive, send):
        'status': 200,
        await send({
        'headers': [
            'type': 'http.response.start',
            [b'content-type', b'text/plain'],
            'status': 200,
        ],
            'headers': [
    })
                [b'content-type', b'text/plain'],
    await send({
            ],
        'type': 'http.response.body',
        })
        'body': b'Hello, world!',
        await send({
    })
            'type': 'http.response.body',
            'body': b'Hello, world!',
        })
```
```


Run the server:
Run the server:


```shell
```shell
$ unicorn app: App
$ unicorn example: app
```
```


---
---
10 docs/deployment.md
@@ -14,7 +14,7 @@ As a general rule, you probably want to:
Typically you'll run `unicorn` from the command line.
Typically you'll run `unicorn` from the command line.


```bash
```bash
$ uvicorn app:App --reload --port 5000
$ uvicorn example:app --reload --port 5000
```
```


The ASGI application should be specified in the form `path.to.module: instance.path`.
The ASGI application should be specified in the form `path.to.module:instance.path`.
@@ -88,8 +88,10 @@ import uvicorn
class App:
class App:
    ...
    ...
app = App()
if __name__ == "__main__":
if __name__ == "__main__":
    uvicorn.run(App, host="127.0.0.1", port=5000, log_level="info", reload=True)
    uvicorn.run(app, host="127.0.0.1", port=5000, log_level="info", reload=True)
```
```


The set of configuration options is the same as for the command line tool.
The set of configuration options is the same as for the command line tool.
@@ -230,15 +232,15 @@ For local development with https, it's possible to use [mkcert][mkcert]
to generate a valid certificate and private key.
to generate a valid certificate and private key.


```bash
```bash
$ uvicorn app:App --port 5000 --ssl-keyfile=./key.pem --ssl-certfile=./cert.pem
$ uvicorn example:app --port 5000 --ssl-keyfile=./key.pem --ssl-certfile=./cert.pem
```
```


### Running gunicorn worker
### Running gunicorn worker


It also possible to use certificates with unicorn's worker for unicorn
It also possible to use certificates with unicorn's worker for unicorn


```bash
```bash
$ gunicorn --keyfile=./key.pem --certfile=./cert.pem -k uvicorn.workers.UvicornWorker app:App
$ gunicorn --keyfile=./key.pem --certfile=./cert.pem -k uvicorn.workers.UvicornWorker example:app
```
```


[letsencrypt]: https://letsencrypt.org/
