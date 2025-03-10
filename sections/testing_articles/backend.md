---
title: Backend Testing
layout: default
nav_order: 2
---

# Common Tests for REST APIs

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>


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



---
Testing checklist for REST APIs

URI validation
Keys from body
    Schema validation
Error Handling
Status code
Mandatory elements
    Header key
    Body keys
    Params
Authentication
Response time
Database validation
Response validation
    Pagination
    Specific Key-value validations
    Logic validations
Interoperatibility tests

---
Principales códigos de estado HTTP

✅ Respuestas exitosas (2xx)

    200 OK → Todo salió bien y el servidor devuelve la respuesta esperada.
    201 Created → Se creó un nuevo recurso correctamente.
    204 No Content → La petición fue exitosa, pero no hay contenido en la respuesta (por ejemplo, después de un DELETE).

⚠️ Errores del cliente (4xx)

    400 Bad Request → La petición está mal formada o tiene datos incorrectos.
    401 Unauthorized → Falta autenticación (por ejemplo, no se envió un token válido).
    403 Forbidden → Tienes autenticación, pero no permisos para acceder a ese recurso.
    404 Not Found → El recurso solicitado no existe.

🚨 Errores del servidor (5xx)

    500 Internal Server Error → Algo falló en el servidor.
    502 Bad Gateway → El servidor recibió una respuesta inválida de otro servidor.
    503 Service Unavailable → El servidor está temporalmente fuera de servicio.
---
