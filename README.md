## Overview

This is a REST API server for a campground rating and reservation web application. It was developed using Node.js, Express, MongoDB, Mongoose, and Passport.

The app’s principal resources are stored in a NoSQL database on MongoDB. Express routing methods handle get/post/put/delete client requests for all routes and interact with the MongoDB server. Routes are controlled with token-based authentication. Aside from GET requests, all endpoints require some level of authentication. Endpoints with more sensitive resources require admin authentication. All routes have error handling for unauthorized operations, server-side errors, and invalid data submission. File uploading functionality is handled by the Multer middleware library.
<br><br>

## Data Modeling
Mongoose ODM is used to define schemas that enforce structure on documents in the database. Queries and CRUD operations are executed with the Mongoose Model API.

Mongoose Population provides relational functionality in the database. The 'author' field of each Comment subdocument stores an ObjectId that references the author's User document. When a GET request is sent to a route in campsiteRouter.js, the author's name for each rendered Comment is populated from their respective User document. This avoids storage redundancy in the database.
<br><br>

## Security

The app supports secure communication with TLS using HTTPS and a public key. This was developed using OpenSSL and self-signed certificates for testing purposes. All traffic to the HTTP server is redirected to the secure HTTPS server.

The Passport library, along with the Passport-JWT plugin, handles authentication using JSON Web Tokens. The tokens are signed, issued and verified using the jsonwebtoken node module. 

Local strategy and third-party authentication are both supported. The Passport-Local Mongoose plug-in handles the creation of usernames and passwords in User documents for local strategy authentication, as well as providing additional security by hashing and salting passwords. The passport-facebook-token module is used to request access tokens from Facebook's OAuth server so that users can login using their Facebook account.

The CORS middleware is used to configure cross-origin resource sharing on endpoints. The module’s default CORS method, which allows access for all origins, is used for GET requests on most routes. More sensitive endpoints are configured with a custom CORS function that checks the client’s request header origin against a pre-approved whitelist.

