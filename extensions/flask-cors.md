`Flask-CORS` is a Flask extension that simplifies the process of handling Cross-Origin Resource Sharing (CORS) in your Flask applications. CORS is a security feature implemented by web browsers to restrict web pages from making requests to a different domain than the one that served the web page. This security measure helps prevent potential security vulnerabilities, but it can also pose challenges when you need to make cross-origin requests from your web application.

When you use Flask to build a web API or server, you might encounter CORS issues when your frontend (hosted on a different domain) tries to make requests to your backend. `Flask-CORS` helps you overcome these issues by adding the necessary CORS headers to your responses.

Here's a basic example of how you can use `Flask-CORS`:

1. Install Flask-CORS:

   ```bash
   pip install Flask-CORS
   ```

2. Use it in your Flask application:

   ```python
   from flask import Flask, jsonify
   from flask_cors import CORS

   app = Flask(__name__)
   CORS(app)  # This will enable CORS for all routes

   @app.route('/api/data', methods=['GET'])
   def get_data():
       data = {'key': 'value'}
       return jsonify(data)

   if __name__ == '__main__':
       app.run(debug=True)
   ```

In this example, the `CORS` object is created and passed the `app` instance. This enables CORS for all routes. You can also customize CORS settings based on your requirements.

```python
# Example with specific CORS configurations
from flask_cors import CORS

# ...

CORS(app, resources={r"/api/*": {"origins": "https://example.com"}})
```

This code snippet specifies that only requests from `https://example.com` are allowed to access routes under `/api/`.

`Flask-CORS` provides various configuration options to control which origins are allowed, what methods are allowed, which headers are allowed, etc. Refer to the official documentation for more details: [Flask-CORS Documentation](https://flask-cors.readthedocs.io/)

Here's an extended example with more features and explanations:

```python
from flask import Flask, jsonify, request
from flask_cors import CORS

app = Flask(__name__)
CORS(app, resources={r"/api/*": {"origins": "https://example.com"}})

# Simple endpoint with CORS enabled
@app.route('/api/data', methods=['GET'])
def get_data():
    data = {'key': 'value'}
    return jsonify(data)

# Endpoint with CORS and handling different HTTP methods
@app.route('/api/resource', methods=['GET', 'POST', 'PUT', 'DELETE'])
def handle_resource():
    if request.method == 'GET':
        data = {'message': 'GET request received'}
    elif request.method == 'POST':
        data = {'message': 'POST request received'}
    elif request.method == 'PUT':
        data = {'message': 'PUT request received'}
    elif request.method == 'DELETE':
        data = {'message': 'DELETE request received'}
    else:
        data = {'message': 'Unsupported method'}

    return jsonify(data)

# Endpoint with custom headers and handling CORS preflight requests
@app.route('/api/custom', methods=['GET'])
def custom_headers():
    response_data = {'message': 'Custom headers example'}
    response = jsonify(response_data)
    response.headers['Custom-Header'] = 'Custom Value'
    return response

if __name__ == '__main__':
    app.run(debug=True)
```

Explanation:

1. **CORS Configuration:** The `CORS` object is created with specific configurations. In this example, it allows requests from `https://example.com` to access resources under `/api/`. The resources parameter allows you to specify which paths should have CORS enabled.

2. **Simple Endpoint:** The `/api/data` endpoint is a simple example that returns JSON data. CORS is enabled for this endpoint by default.

3. **Handling Different HTTP Methods:** The `/api/resource` endpoint handles multiple HTTP methods (GET, POST, PUT, DELETE). This demonstrates how CORS can be applied to different types of requests.

4. **Custom Headers:** The `/api/custom` endpoint shows how to include custom headers in the response. This is useful for scenarios where you need to include additional information in the response headers.

5. **Running the App:** The `if __name__ == '__main__':` block ensures that the Flask app is only run when the script is executed directly, not when it's imported as a module.

Remember to adapt the CORS configurations and examples to match your specific use case and security requirements. The Flask-CORS documentation provides detailed information on the available options and their meanings: [Flask-CORS Documentation](https://flask-cors.readthedocs.io/).
