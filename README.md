# Flask REST API Lab README

## Overview

This project demonstrates building a RESTful API using **Flask**. It covers key backend concepts like creating routes, handling parameters, managing errors, and processing requests/responses. The steps are adapted from the lab instructions and expanded with additional context for clarity.

---

## Prerequisites

* Python 3.7+
* Flask installed (`pip install flask`)
* cURL or an API testing tool (e.g., Postman)

---

## Project Files

* `server.py`: Main Flask application containing all endpoints.
* `requirements.txt` (optional): List of dependencies.
* `images/`: Folder to store screenshots (if any).

---

## Steps Implemented

### 1. **Environment Setup**

1. Create a project directory:

   ```bash
   mkdir lab && cd lab
   ```
2. Create `server.py` file:

   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route("/")
   def index():
       return "hello world"
   ```
3. Run the server:

   ```bash
   flask --app server --debug run
   ```

---

### 2. **Custom HTTP Status Codes**

* Added `/no_content` route returning **204 No Content**:

  ```python
  @app.route("/no_content")
  def no_content():
      return {"message": "No content found"}, 204
  ```
* Added `/exp` route using **make\_response()** for explicit responses.

  ```python
  from flask import make_response

  @app.route("/exp")
  def index_explicit():
      response = make_response({"message": "Hello World"}, 200)
      return response
  ```

---

### 3. **Handling Query Parameters**

* Created `/data` route to validate dataset existence.
* Created `/name_search` route to filter by first name.
* Edge cases handled:

  * Missing `q` → **400 Bad Request**
  * Invalid `q` → **422 Unprocessable Entity**
  * Not found → **404 Not Found**

Example:

```bash
curl "localhost:5000/name_search?q=John"
```

---

### 4. **Dynamic URLs**

* Added endpoints:

  * `/count` → returns number of persons
  * `/person/<uuid>` → fetches person by ID
  * `DELETE /person/<uuid>` → deletes person
* Proper error handling for invalid or missing UUIDs.

---

### 5. **POST Requests (JSON Body)**

* Endpoint to create a new person:

```python
@app.route("/person", methods=['POST'])
def create_person():
    new_person = request.get_json()
    if not new_person:
        return {"message": "Invalid input"}, 422
    data.append(new_person)
    return {"message": new_person['id']}, 200
```

Test using:

```bash
curl -X POST http://localhost:5000/person \
  -H 'Content-Type: application/json' \
  -d '{"id":"uuid","first_name":"John"}'
```

---

### 6. **Global Error Handling**

* JSON-based 404 handler:

```python
@app.errorhandler(404)
def api_not_found(error):
    return {"message": "API not found"}, 404
```

* Global 500 handler:

```python
@app.errorhandler(Exception)
def handle_exception(e):
    return {"message": str(e)}, 500
```

---

### 7. **Testing**

Use cURL or Postman to test all endpoints:

* `GET /`
* `GET /no_content`
* `GET /exp`
* `GET /data`
* `GET /name_search?q=FirstName`
* `GET /count`
* `GET /person/<uuid>`
* `DELETE /person/<uuid>`
* `POST /person`

---

## Future Enhancements

* Replace in-memory `data` with a database (e.g., MongoDB, SQLite).
* Add JWT authentication.
* Implement input validation and schemas (e.g., Marshmallow).
* Containerize the app using Docker.

---

## Author

Adapted and written by **Ayoub CHAIEB** based on IBM's lab materials.
