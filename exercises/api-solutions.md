# Solutions: Pythagoras Flask API

**Suggested approach:** Build the API incrementally. First demonstrate a successful request, then introduce validation and discuss the sustainability implications.

## 1. Why Flask?

Flask is a good fit for this introductory exercise:

- It is lightweight and has a small amount of setup.
- Its route functions make the relationship between URL, HTTP method and Python function clear.
- It lets students focus on the request–response model without introducing a larger framework's additional concepts.
- It is widely used for small Python web applications and APIs.

FastAPI is also a strong option, especially when teaching type hints, automatic API documentation and asynchronous endpoints. For this class, Flask is likely the simpler starting point. The example below is synchronous; it does not attempt to teach asynchronous programming.

## 2. Expected project structure

```text
pythagoras-api/
├── app.py
├── pythagoras.py
└── requirements.txt
```

`requirements.txt`:

```text
Flask
```

Install:

```bash
python -m pip install -r requirements.txt
```

For a reproducible teaching environment, you can pin the Flask version after checking the version used in your course environment. The unpinned requirement above is simpler for the first exercise.

## 3. Calculation module: `pythagoras.py`

```python
import math


def calculate_hypotenuse(side_a, side_b):
    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    return math.sqrt(side_a**2 + side_b**2)
```

Teaching point: keep the mathematical calculation separate from the web layer. This function can be unit-tested independently and reused by other parts of the application.

## 4. Minimal Flask app: first milestone

Start with:

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

Run:

```bash
python app.py
```

Open `http://127.0.0.1:5000/` locally. In Codespaces, open the forwarded port 5000 from the Ports panel.

Explain:
- `Flask(__name__)` creates the application.
- `@app.route("/")` maps a URL path to a Python function.
- The function's return value becomes the HTTP response.
- `app.run()` starts the development server.
- `debug=True` is convenient during local teaching, but should not be used for a publicly deployed production service.

## 5. Complete API solution: `app.py`

This solution accepts JSON, validates it, calls the calculation function, and returns JSON.

```python
from flask import Flask, request, jsonify
from pythagoras import calculate_hypotenuse

app = Flask(__name__)


@app.route("/")
def home():
    return "Pythagoras API is running"


@app.route("/hypotenuse", methods=["POST"])
def hypotenuse():
    if not request.is_json:
        return jsonify({
            "error": "Request body must be JSON"
        }), 415

    data = request.get_json(silent=True)

    if not isinstance(data, dict):
        return jsonify({
            "error": "Request body must be a JSON object"
        }), 400

    if "side_a" not in data or "side_b" not in data:
        return jsonify({
            "error": "Both side_a and side_b are required"
        }), 400

    side_a = data["side_a"]
    side_b = data["side_b"]

    # Reject booleans explicitly: bool is a subclass of int in Python.
    if (
        isinstance(side_a, bool)
        or isinstance(side_b, bool)
        or not isinstance(side_a, (int, float))
        or not isinstance(side_b, (int, float))
    ):
        return jsonify({
            "error": "Side lengths must be numbers"
        }), 400

    if side_a <= 0 or side_b <= 0:
        return jsonify({
            "error": "Side lengths must be positive"
        }), 400

    result = calculate_hypotenuse(side_a, side_b)

    return jsonify({
        "hypotenuse": result
    }), 200


if __name__ == "__main__":
    app.run(debug=True)
```

### Why these checks?

1. `request.is_json` checks that the client sent a JSON content type. `415` indicates an unsupported media type.
2. `get_json(silent=True)` avoids raising an exception for malformed JSON; the result is checked.
3. The dictionary check ensures the JSON body is an object with key/value pairs.
4. The required-key check gives a clear error when a side is missing.
5. The type check rejects strings, arrays, objects and booleans.
6. The positive-value check enforces the mathematical requirement.
7. The calculation remains in `pythagoras.py`, separate from HTTP handling.

For a teaching exercise, `400` is a reasonable response for malformed or invalid request data. More elaborate APIs may distinguish different validation failures in greater detail.

## 6. Test the successful request

Start the server in one terminal:

```bash
python app.py
```

In a second terminal:

```bash
curl -X POST http://127.0.0.1:5000/hypotenuse \
  -H "Content-Type: application/json" \
  -d '{"side_a": 3, "side_b": 4}'
```

Expected response:

```json
{
  "hypotenuse": 5.0
}
```

Another example:

```bash
curl -X POST http://127.0.0.1:5000/hypotenuse \
  -H "Content-Type: application/json" \
  -d '{"side_a": 5, "side_b": 12}'
```

Expected response:

```json
{
  "hypotenuse": 13.0
}
```

JSON property order and whitespace are not significant.

## 7. Test invalid requests

Examples to demonstrate:

### Missing value

```bash
curl -X POST http://127.0.0.1:5000/hypotenuse \
  -H "Content-Type: application/json" \
  -d '{"side_a": 3}'
```

Expected status: `400`.

### String instead of a number

```bash
curl -X POST http://127.0.0.1:5000/hypotenuse \
  -H "Content-Type: application/json" \
  -d '{"side_a": "three", "side_b": 4}'
```

Expected status: `400`.

### Zero side

```bash
curl -X POST http://127.0.0.1:5000/hypotenuse \
  -H "Content-Type: application/json" \
  -d '{"side_a": 0, "side_b": 4}'
```

Expected status: `400`.

### No JSON content type

Send a request without the JSON content type. Expected status: `415`.

## 8. Optional: automated API tests

If time permits, introduce Flask's built-in test client. It allows requests to be made against the application without manually starting a server.

Create `test_api.py`:

```python
import unittest
from app import app


class TestPythagorasAPI(unittest.TestCase):

    def setUp(self):
        self.client = app.test_client()

    def test_hypotenuse_success(self):
        response = self.client.post(
            "/hypotenuse",
            json={"side_a": 3, "side_b": 4}
        )

        self.assertEqual(response.status_code, 200)
        self.assertAlmostEqual(
            response.get_json()["hypotenuse"],
            5
        )

    def test_missing_side(self):
        response = self.client.post(
            "/hypotenuse",
            json={"side_a": 3}
        )

        self.assertEqual(response.status_code, 400)

    def test_zero_side(self):
        response = self.client.post(
            "/hypotenuse",
            json={"side_a": 0, "side_b": 4}
        )

        self.assertEqual(response.status_code, 400)


if __name__ == "__main__":
    unittest.main()
```

Run:

```bash
python -m unittest -v
```

This is a useful link to the previous unit-testing practical: students can test not only the calculation function, but also the API interface.

## 9. Optional JavaScript client

A simple JavaScript example using `fetch`:

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

Expected result in the console:

```javascript
{ hypotenuse: 5 }
```

Important teaching notes:
- The JavaScript client does not import the Python function.
- It sends an HTTP request and reads a JSON response.
- If the browser page and API have different origins, the browser's same-origin policy applies. Configure Flask-CORS (or an equivalent appropriate CORS policy) if cross-origin browser access is required.
- A Node.js client or a browser page served from the same origin can avoid some cross-origin setup issues, depending on the environment.

## 10. Codespaces notes

- Start Flask with `python app.py`.
- Open the Ports panel.
- Make port `5000` visible and open its forwarded address.
- Use that forwarded URL in browser or client requests.
- Keep the server process running while testing.
- Stop it with `Ctrl + C`.

Do not assume that `127.0.0.1` in a student's browser refers to the Codespace. In Codespaces, use the forwarded URL when accessing the service from outside the Codespace terminal.

## 11. Troubleshooting

| Problem | Likely cause / response |
|---|---|
| `ModuleNotFoundError: No module named 'flask'` | Activate the virtual environment and run `python -m pip install -r requirements.txt`. |
| `ModuleNotFoundError: No module named 'pythagoras'` | Check that `pythagoras.py` is in the same project folder as `app.py`. |
| `Address already in use` | Another server is using port 5000. Stop it or choose another port. |
| Browser shows `404 Not Found` | Check the URL path. The calculation endpoint is `/hypotenuse`. |
| `405 Method Not Allowed` | The endpoint expects POST, not GET. |
| `415 Unsupported Media Type` | Include the `Content-Type: application/json` header. |
| Connection refused | Check that the Flask server is running and that the correct local or forwarded URL is used. |
| Browser reports a CORS error | Configure an appropriate CORS policy if the client is on a different origin. |

## 12. Discussion: service architecture and sustainability

| Benefit | Sustainability connection |
|---|---|
| HTTP provides a common interface | Interoperability |
| Multiple clients can use one service | Reusability |
| Users can access a hosted service | Usability |
| The API can evolve while retaining its interface | Extensibility |
| Services can be separated and independently maintained | Maintainability and potential resilience |

Important nuance: a service-based architecture does not automatically make software resilient. If a service becomes unavailable, clients may lose access to its functionality. Resilience requires deliberate design, such as monitoring, sensible error handling, retries where appropriate, redundancy and fallback behaviour. A single API server can itself become a single point of failure.

Also distinguish a service-based architecture from microservices: this practical creates one small HTTP service. It is not necessary to split this simple calculator into multiple services.



**Important:** The most important learning outcome is not Flask syntax. It is understanding that a Python function can be exposed through a standard HTTP interface, allowing clients written in other languages to use its functionality.
