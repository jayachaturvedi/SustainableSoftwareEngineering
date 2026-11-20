# Practical: Turn the Pythagoras Calculator into a Flask API

## Aim

In this practical, you will turn the Pythagoras calculation you have already written into a small web service.

Instead of asking a user to enter values directly into a Python program, the service will accept an HTTP request containing JSON, calculate the hypotenuse, and return a JSON response.

A client written in another language (for example, JavaScript) can use the service without importing or running the Python code itself.

## Learning objectives

By the end, you should be able to:

- Explain the purpose of an HTTP API.
- Describe the request–response model.
- Create a simple API using Flask.
- Send JSON data to an API and receive a JSON response.
- Handle invalid requests and return suitable HTTP status codes.
- Explain how a service can improve interoperability and reuse.

## 1. Set up your project

Create a project folder called `pythagoras-api`.

Your project will eventually look like this:

```text
pythagoras-api/
├── app.py
├── pythagoras.py
├── requirements.txt
└── test_api.py          # optional extension
```

Open the folder in VS Code or GitHub Codespaces.

### Create a virtual environment (recommended)

A virtual environment keeps this practical's Python packages separate from other projects.

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows PowerShell:**

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

If activation is blocked by your system's PowerShell policy, you can use the Codespaces terminal or follow your institution's Python environment guidance.

### Install Flask

Create `requirements.txt` containing:

```text
Flask
```

Install the dependency:

```bash
python -m pip install -r requirements.txt
```

Check that Flask installed:

```bash
python -m pip show Flask
```

## 2. Keep the calculation separate

Create `pythagoras.py`:

```python
import math


def calculate_hypotenuse(side_a, side_b):
    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    return math.sqrt(side_a**2 + side_b**2)
```

This function contains the calculation. It does not know anything about HTTP, Flask, browsers or JSON.

Keeping the calculation separate makes it easier to reuse and test.

## 3. Create a Flask application

Create `app.py`.

Start with this code:

```python
from flask import Flask, request, jsonify
from pythagoras import calculate_hypotenuse

app = Flask(__name__)


@app.route("/")
def home():
    return "Pythagoras API is running"


if __name__ == "__main__":
    app.run(debug=True)
```

Run the server:

```bash
python app.py
```

You should see Flask report that it is running, usually at:

```text
http://127.0.0.1:5000
```

Open that address in your browser. You should see:

```text
Pythagoras API is running
```

**Important:** Keep the terminal running while you use the API. To stop the server, press `Ctrl + C`.

If you are using GitHub Codespaces, open the **Ports** tab, find port `5000`, and open the forwarded address in your browser. If the port is not detected automatically, add port `5000` in the Ports panel.

## 4. Add an API endpoint

An API endpoint is a URL that a client can send a request to.

We will create an endpoint:

```text
POST /hypotenuse
```

It will accept JSON containing two side lengths:

```json
{
    "side_a": 3,
    "side_b": 4
}
```

It should return:

```json
{
    "hypotenuse": 5.0
}
```

**Your task:** Add a `/hypotenuse` route to `app.py`.

Hints:

- Use `@app.route("/hypotenuse", methods=["POST"])`.
- Read the incoming JSON using `request.get_json()`.
- Get the values using `data.get("side_a")` and `data.get("side_b")`.
- Call `calculate_hypotenuse(side_a, side_b)`.
- Return the result using `jsonify(...)`.

Do not put the calculation itself into the route. Reuse the function from `pythagoras.py`.

## 5. Test your API

Keep the Flask server running. Open a **second terminal** to send requests.

### Option A: Use curl

Run:

```bash
curl -X POST http://127.0.0.1:5000/hypotenuse \
  -H "Content-Type: application/json" \
  -d '{"side_a": 3, "side_b": 4}'
```

Expected JSON response:

```json
{
  "hypotenuse": 5.0
}
```

The order and spacing of the JSON response may differ.

### Option B: Use the Codespaces forwarded URL

If you are using Codespaces, replace `http://127.0.0.1:5000` in the command with the forwarded port-5000 URL provided by Codespaces.

### What is happening?

1. `curl` sends an HTTP POST request.
2. The request contains JSON data.
3. Flask receives the request.
4. The route calls the Python calculation function.
5. Flask returns the result as JSON.

## 6. Handle invalid requests

A useful API should not simply fail when a client sends invalid data.

Improve your endpoint to handle these situations:

- The request does not contain JSON.
- One or both side lengths are missing.
- A side length is a string rather than a number.
- A side length is zero or negative.

Your API should return a helpful error message and an appropriate HTTP status code.

Useful status codes:

| Status code | Meaning | When to use it |
|---|---|---|
| `200` | OK | Calculation succeeded |
| `400` | Bad Request | The client sent invalid or incomplete data |
| `415` | Unsupported Media Type | The request is not sent as JSON |

**Hint:** Flask's `jsonify` can return both a JSON response and a status code:

```python
return jsonify({"error": "An explanatory message"}), 400
```

Remember that in Python, `bool` is a subclass of `int`. Decide whether `true` and `false` should be accepted as side lengths; for this practical, they should be rejected.

## 7. Try the API with different requests

Once your error handling is complete, test the following cases.

| Request data | Expected outcome |
|---|---|
| `{"side_a": 3, "side_b": 4}` | Success; hypotenuse 5 |
| `{"side_a": 5, "side_b": 12}` | Success; hypotenuse 13 |
| `{"side_a": 0, "side_b": 4}` | Error: side lengths must be positive |
| `{"side_a": -3, "side_b": 4}` | Error: side lengths must be positive |
| `{"side_a": 3}` | Error: missing side |
| `{"side_a": "three", "side_b": 4}` | Error: values must be numbers |
| No JSON body | Error: JSON request required |

You can change the JSON in your `curl` command to try each case.

## 8. Extension: use the API from JavaScript

The API is written in Python, but a JavaScript client can communicate with it using HTTP.

Create a file called `client.js`:

```javascript
async function calculateHypotenuse() {
    const response = await fetch("http://127.0.0.1:5000/hypotenuse", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify({
            side_a: 3,
            side_b: 4
        })
    });

    const result = await response.json();
    console.log(result);
}

calculateHypotenuse();
```

This example is intended to run in an environment where JavaScript's `fetch` is available and the API address is reachable. If running the client in a browser on a different origin, you may also need to configure CORS on the Flask server.

**Think about it:** The client is written in JavaScript, the service is written in Python, and they communicate using HTTP and JSON. The client does not need to know how the Python calculation is implemented.

## 9. Reflection: service-based architectures and sustainability

Discuss these questions:

1. How is using an API different from downloading and importing a Python library?
2. Why can an API make software easier to use from another programming language?
3. What are the advantages of hosting the service remotely?
4. What might happen if the service is unavailable?
5. What responsibilities does the service developer have when changing an API that other people use?

Consider the sustainability benefits:

- **Interoperability:** different languages and applications can communicate through a common interface.
- **Usability:** users can access functionality without installing all the underlying software locally.
- **Reusability:** multiple clients can use the same service.
- **Extensibility:** the service can be extended while keeping a consistent interface.

## 10. Useful commands

| Command | Purpose |
|---|---|
| `python -m venv .venv` | Create a virtual environment |
| `source .venv/bin/activate` | Activate it on macOS/Linux |
| `python -m pip install -r requirements.txt` | Install dependencies |
| `python app.py` | Start the Flask server |
| `Ctrl + C` | Stop the server |

**Key takeaway:** A service-based architecture exposes software functionality through a defined interface. Other programs can use that functionality over HTTP, even if they are written in different programming languages.
