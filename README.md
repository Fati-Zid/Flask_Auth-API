# Flask API Documentation

## Overview

This API provides user authentication and authorization services. It includes endpoints for user registration, login, and accessing protected resources.

## Base URL

The base URL for the API is:

http://127.0.0.1:5000

## Endpoints

1. ## User Registration

   Endpoint: /register
   Method: POST
   Description: Registers a new user.

   ## Request Body:

   {
   "username": "string",
   "password": "string"
   }

   ## Response:

   - 201 Created
     {
     "message": "User registered successfully"
     }

2. ## User Login

   Endpoint: /login
   Method: POST
   Description: Authenticates a user and returns a JWT token.

   ## Request Body:

   {
   "username": "string",
   "password": "string"
   }

   ## Response:

   - 200 OK
     {
     "access_token": "string"
     }

   - 401 Unauthorized
     {
     "message": "Invalid credentials"
     }

3. ## Protected Route

   Endpoint: /protected
   Method: GET
   Description: Accesses a protected resource. Requires a valid JWT token.

   ## Headers:

   - Authorization: Bearer <access_token>

   ## Response:

   - 200 OK
     {
     "logged_in_as": {
     "username": "string"
     }
     }

   - 401 Unauthorized
     {
     "msg": "Missing Authorization Header"
     }

## Example Usage

1. ## Register a User

   ## Request:

   curl -X POST -H "Content-Type: application/json" -d '{"username": "testuser", "password": "testpassword"}' http://127.0.0.1:5000/register

   ## Response:

   {
   "message": "User registered successfully"
   }

2. ## User Login

   ## Request:

   curl -X POST -H "Content-Type: application/json" -d '{"username": "testuser", "password": "testpassword"}' http://127.0.0.1:5000/login

   ## Response:

   {
   "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
   }

3. ## Access Protected Route

   ## Request:

   ACCESS_TOKEN=$(curl -X POST -H "Content-Type: application/json" -d '{"username": "testuser", "password": "testpassword"}' http://127.0.0.1:5000/login | jq -r '.access_token')

   curl -X GET -H "Authorization: Bearer $ACCESS_TOKEN" http://127.0.0.1:5000/protected

   ## Response:

   {
   "logged_in_as": {
   "username": "testuser"
   }
   }

## Error Handling

1. ## 400 Bad Request

   Occurs when the request body is malformed or missing required fields.

   ## Response:

   {
   "message": "Bad Request"
   }

2. ## 401 Unauthorized

   Occurs when the user provides invalid credentials or the token is missing/invalid.

   ## Response:

   {
   "message": "Invalid credentials"
   }

3. ## 403 Forbidden

   Occurs when the user tries to access a resource without the necessary permissions.

   ## Response:

   {
   "message": "Forbidden"
   }

## Setup Instructions

1. ## Install dependencies:

   pip install Flask Flask-SQLAlchemy Flask-Bcrypt Flask-JWT-Extended

2. ## Run the application:

   python app.py

3. ## Initialize the database:
   python init_db.py

## Conclusion

This documentation provides an overview of the available API endpoints, example requests, and expected responses. Use this as a reference for integrating with the authentication and authorization services provided by the Flask application.
