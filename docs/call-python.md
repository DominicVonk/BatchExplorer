# Calling python from the javascript

There is a rpc server that starts automaticaly when you start the app that allows you to run python code from the browser.

## Step 0: Setup python
Install python > 3.6 **Important: Batch labs will look for python and check the version is more than 3.6 before starting the server**
It will scan for executable in the path called `python3` or `python`. If your python is not in your path you can set the environment variable `BL_PYTHON_PATH` with the path to python 3.6.

## Step 1: Write the python controller
In `python/controllers` open/create a controller file(It will be loaded automatically).

```python
from server.app import app
# Give the name of the procedure in `app.procedure` decorator.
@app.procedure("foo")
def foo(params):
    print("got data", params)
    # Return a dict that can be converted to JSON(The conversion is done automatically)
    return {"what": "it works!"}

```

## Step 2: Call the python procedure from the javascript.

```ts
import {PythonRpcService} from "app/services";

// Inject service in constructor
constructor(pythonRpcService: PythonRpcService) {

}

// Call the python methods
pythonRpcService.call("foo", ["abc", "def"]).subscribe({
    next: (data) => console.log("Got foo", data),
    error: (err) => console.log("Error foo", err),
});
pythonRpcService.call("other", ["abc", "def"]).subscribe({
    next: (data) => console.log("Got other", data),
    // Here it will return an
    error: (err) => console.log("Error other", err),
});
```