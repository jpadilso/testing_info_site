# Common Tests for REST APIs

## Table of Contents
- [GET Requests](#get-requests)
- [POST Requests](#post-requests)
- [PUT Requests](#put-requests)
- [DELETE Requests](#delete-requests)
- [Test Checklist](#test-checklist)

## GET Requests

**GET requests** are used to retrieve data from a server. Common tests include:

### 1. **Schema Validation**
Ensures the response matches the expected JSON schema.

**Example:**
```python
import jsonschema
from jsonschema import validate

response = {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com"
}

schema = {
    "type": "object",
    "properties": {
        "id": {"type": "integer"},
        "name": {"type": "string"},
        "email": {"type": "string"}
    },
    "required": ["id", "name", "email"]
}

validate(instance=response, schema=schema)
```

### 2. **Pagination**
Verifies correct handling of pagination.

**Example:**
```python
import requests

response = requests.get("https://api.example.com/items?page=1&limit=10")
data = response.json()

assert len(data["items"]) == 10
assert "next" in data["links"]
assert "prev" in data["links"]
```

### 3. **Data Integrity**
Ensures the data returned is accurate and consistent.

**Example:**
```python
import requests

response = requests.get("https://api.example.com/items/1")
data = response.json()

assert data["id"] == 1
assert data["name"] == "Item 1"
```

## POST Requests

**POST requests** are used to create new resources. Common tests include:

### 1. **Schema Validation**
Ensures the response matches the expected JSON schema.

**Example:**
```python
import jsonschema
from jsonschema import validate

response = {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com"
}

schema = {
    "type": "object",
    "properties": {
        "id": {"type": "integer"},
        "name": {"type": "string"},
        "email": {"type": "string"}
    },
    "required": ["id", "name", "email"]
}

validate(instance=response, schema=schema)
```

### 2. **Error Handling**
Checks that the API returns appropriate error messages and status codes for invalid requests.

**Example:**
```python
import requests

response = requests.post("https://api.example.com/items", json={"invalid": "data"})
assert response.status_code == 400
assert response.json()["error"] == "Invalid request data"
```

### 3. **Authentication and Authorization**
Ensures that the API endpoints are protected and only accessible to authorized users.

**Example:**
```python
import requests

headers = {"Authorization": "Bearer valid_token"}
response = requests.post("https://api.example.com/protected", headers=headers)
assert response.status_code == 200

headers = {"Authorization": "Bearer invalid_token"}
response = requests.post("https://api.example.com/protected", headers=headers)
assert response.status_code == 401
```

## PUT Requests

**PUT requests** are used to update existing resources. Common tests include:

### 1. **Schema Validation**
Ensures the response matches the expected JSON schema.

**Example:**

>!
```python
import jsonschema
from jsonschema import validate

response = {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com"
}

schema = {
    "type": "object",
    "properties": {
        "id": {"type": "integer"},
        "name": {"type": "string"},
        "email": {"type": "string"}
    },
    "required": ["id", "name", "email"]
}

validate(instance=response, schema=schema)
```

### 2. **Data Integrity**
Ensures the data returned is accurate and consistent after the update.

**Example:**
```python
import requests

response = requests.put("https://api.example.com/items/1", json={"name": "Updated Item"})
data = response.json()

assert data["id"] == 1
assert data["name"] == "Updated Item"
```

### 3. **Error Handling**
Checks that the API returns appropriate error messages and status codes for invalid update requests.

**Example:**
```python
import requests

response = requests.put("https://api.example.com/items/1", json={"invalid": "data"})
assert response.status_code == 400
assert response.json()["error"] == "Invalid request data"
```

## DELETE Requests

**DELETE requests** are used to delete resources. Common tests include:

### 1. **Successful Deletion**
Ensures the resource is successfully deleted.

**Example:**
```python
import requests

response = requests.delete("https://api.example.com/items/1")
assert response.status_code == 204
```

### 2. **Error Handling**
Checks that the API returns appropriate error messages and status codes for invalid delete requests.

**Example:**
```python
import requests

response = requests.delete("https://api.example.com/items/999")
assert response.status_code == 404
assert response.json()["error"] == "Resource not found"
```

### 3. **Data Integrity**
Ensures the resource is no longer available after deletion.

**Example:**
```python
import requests

requests.delete("https://api.example.com/items/1")
response = requests.get("https://api.example.com/items/1")
assert response.status_code == 404
```

## Test Checklist

### GET Requests
- Schema Validation
- Pagination
- Data Integrity

#### Common Defects:
- Incorrect data format
- Missing or extra fields
- Incorrect pagination links
- Inconsistent data

### POST Requests
- Schema Validation
- Error Handling
- Authentication and Authorization

#### Common Defects:
- Incorrect data format
- Missing or extra fields
- Incorrect pagination links
- Unauthorized access

### PUT Requests
- Schema Validation
- Data Integrity
- Error Handling

#### Common Defects:
- Incorrect data format
- Missing or extra fields
- Improper error messages
- Data not updated correctly

### DELETE Requests
- Successful Deletion
- Error handling
- Data integrity

#### Common Defects:
- Resource not deleted 
- Improper error messages
- Resource still available