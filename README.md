# Flask REST API

## Overview

This project demonstrates building a fully functional RESTful API using **Flask**, incorporating additional improvements and error handling. It now includes template rendering, enhanced search functionality, and extended CRUD operations with robust validation.

---

## Prerequisites

* Python 3.7+
* Flask (`pip install flask`)
* API testing tool like Postman or cURL
* run the flask server with (`python3.11 server.py`)
---

## Project Files

* `server.py`: Main Flask application containing all routes and logic.
* `templates/`: Folder containing `home.html`, `about.html`, `contact.html`, and `profile.html`.
* `images/`: Screenshots (if any).

---

## Implemented Features

### 1. **Homepage and Templates**

* `GET /` renders `home.html` and injects a name variable.
* Added `/about`, `/contact`, and `/users/<username>` routes rendering their respective templates.

```python
@app.route("/")
def index():
    name = "Ayoub CHAIEB"
    return render_template("home.html", name=name)
```

---

### 2. **HTTP Responses and Custom Status Codes**

* `/no_content`: Returns a 204 No Content message.
* `/exp`: Uses `make_response` for explicit JSON responses.

---

### 3. **Data Handling and Validation**

* In-memory dataset of sample persons.
* `/data`: Confirms dataset availability.
* `/count`: Returns the number of items.
* Robust exception handling for missing data.

---

### 4. **Search and Query Parameters**

* `/name_search`: Searches case-insensitively by first name.
* `/name_search0`: Deprecated version kept for reference.
* Handles:

  * Missing `q` → 400
  * Invalid `q` → 422
  * Not found → 404

Example:

```bash
curl "localhost:5000/name_search?q=Tanya"
```

---

### 5. **Dynamic Routes and CRUD**

* `GET /person/<uuid>`: Retrieve person by ID.
* `DELETE /person/<uuid>`: Delete a record.
* `POST /person`: Create a new person with strict validation of all fields and types.

Validation logic ensures:

* All keys are present.
* Values match expected types (string, int, etc.).

---

### 6. **Error Management**

* Global 404 and 500 error handlers return JSON responses.
* Generic `Exception` handler captures and returns detailed messages during development.

```python
@app.errorhandler(404)
def api_not_found(error):
    return {"message": "API not found"}, 404
```

---

### 7. **Additional Enhancements**

* Custom exception handling with `werkzeug.exceptions.NotFound`.
* Example placeholders for future testing routes (e.g., `/test500`).
* Code now uses `render_template` for better UI support.

---

## Testing

Use Postman or cURL to test endpoints:

* `GET /`
* `GET /about`
* `GET /contact`
* `GET /users/<username>`
* `GET /no_content`
* `GET /exp`
* `GET /data`
* `GET /name_search?q=Name`
* `GET /count`
* `GET /person/<uuid>`
* `DELETE /person/<uuid>`
* `POST /person`

---

## Future Work

* Replace the dataset with a persistent database.
* Add authentication and authorization.
* Containerize with Docker and add CI/CD pipeline.
* Replace in-memory `data` with a database (e.g., MongoDB, SQLite).
* Add JWT authentication.
* Implement input validation and schemas (e.g., Marshmallow).
* Containerize the app using Docker.

---

## Author

Developed and documented by **Ayoub CHAIEB**, extended with additional functionality for better usability and production readiness.
