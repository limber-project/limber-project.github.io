# Requests
Several components are involved when processing a request, each are discussed below.

## Routes
Routes can be found under the **routes** folder, with each module containing a set of routes.
A new route can be established inside a route module, as described in the [FastAPI docs](https://fastapi.tiangolo.com/tutorial/first-steps/#step-3-create-a-path-operation).

### Create a New Routes Module
To establish a new set of routes, create a new route module in the *routes* folder.
Inside the route module, create a new [APIRouter](https://fastapi.tiangolo.com/pt/advanced/custom-request-and-route/#custom-apiroute-class-in-a-router) instance.

_limber.routes.web_:
```python
from fastapi import APIRouter

router = APIRouter()

@router.get('/')
def root(request: Request):
    return 'Hello, World!'
```

Next, register the APIRouter with **Application** in **main.py**.

```python
from limber.routes.web import router as web_router
...

# Register routes
app.include_router(api_router)
app.include_router(web_router)

...
```

## Controllers
Controllers can be created to handle processing a request and can be found in **app/http/controllers**.
These controllers can be used inside a route as follows:

```python
from fastapi import APIRouter, Request
from limber.app.http.controllers import welcome_controller

router = APIRouter()

@router.get('/')
def welcome(request: Request):
    return welcome_controller.get()
```

## Middleware
Middleware can be used to handle a request before any processing is performed and handle a response before being sent to the client.
Middleware can be found in **app/http/middleware**.
These middleware can be register with **Application** in **main.py** as follows:

```python
from limberframework.database.middleware import DatabaseSessionMiddleware
...
app.add_middleware(DatabaseSessionMiddleware)
...
```