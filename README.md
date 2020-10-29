# restaurant-app-react-client-for-express

This is the frontend part of my React + MongoDB + Express restaurant web app. The backend is at https://github.com/Abel-Slk/restaurant-app-express-server

# Complete App Overview (frontend + backend)
A restaurant website with a React + Redux frontend and a MongoDB + Express backend. 

The website supports logging in, which enables users to add comments to the dishes in the menu, as well as add dishes to their personal list of favorites.

See the screenshots folder for both frontend and backend screens.

# Frontend
Implemented as a single page React app. Navigation is done via React Router. Application state is managed by Redux. 

## Technologies used
- ES6 JavaScript 
- React
- Redux
- Redux-Thunk
- React Router
- Reactstrap (Bootstrap)
- React Transition Group and React Animation Components for animation

# Backend
Implemented using a MongoDB database and an Express server. 

## Technologies used
-	MongoDB (with Mongoose)
-	Node.js
-	REST API
-	Express.js
-	HTTPS Node module
-	Passport Node module
-	Postman


## MongoDB
All data is stored in MongoDB. 

In addition, for consistency in the structure of the data, Mongoose was used to create schemas for different document types.

Some document models store references to other object models in the form of ObjectIds. 
Whenever it was necessary to make the data containing ObjectIds more readable, Mongoose population was used to populate the referencing document with more complete information about the referenced document.

## Express server

The Express app is scaffolded out using the Express generator.

The app is configured to run at a secure HTTPS server. HTTPS is configured using OpenSSL private key and certificate (not included in this repo). The app is set up to automatically redirect any requests using the HTTP protocol towards the HTTPS server.

For carrying out server-side operations I used Postman.

### Logging in
Logging in is done through POST requests to the '/users/login' endpoint. User authentication is implemented using the Passport middleware (the local authentication strategy).  

### Token-based authentication
After a user successfully logs in, a JWT token is issued to that user. In this version of the app I made the token expire in 48 hours.

To carry out a request on the server side, the user includes their bearer token in the authentication header of the request.

### Routes and access management

The following routes are mounted in app.js:
Generated by the Express generator:
-	'/' 
-	'/users'
Custom routes:
-	'/dishes'
-	'/promotions'
-	'/leaders'
-	'/imageUpload'
-	'/favorites'
-	'/comments'

'/' only supports GET requests, which are allowed to everyone.

GET requests to '/users' are allowed to admins only; POST requests to '/users/signup' and '/users/login' and GET requests to '/users/logout' are allowed to everyone.

GET requests to /dishes, /promotions and /leaders are allowed to anyone; POST and DELETE requests are allowed to admin users only. 

'/imageUpload' allows GET, POST, PUT, DELETE operations only to admins.

'/favorites' allows GET, POST, PUT, DELETE requests to all users who are logged in. (The GET operation returns a response with the list of favorite dishes of that particular user, POST adds an item to their personal list of favorites, etc.). 

'/comments' allows GET operations to everyone and POST operations to logged in users. 
Updating or deleting a specific comment: '/comments/:commentId' allows PUT and DELETE requests only to the user who posted that particular comment.


User authentication for allowing or disallowing access to certain routes is done using JSON Web Tokens (JWT).

In addition, the CORS Node module is used for origin-related access control:
-	GET requests are allowed for all origins
-	POST, PUT, DELETE requests are allowed only for certain whitelisted origins. In this version of the app these origins are: 'http://localhost:3000/' (the URL of the HTTP server), 'https://localhost:3443/' (the HTTPS server), 'http://localhost:3001/' (the React client).


# How to use the app locally
First generate the private key and certificate for the HTTPS server. I did it using OpenSSL by navigating to express-server/bin and typing:
- `openssl genrsa 1024 > private.key`    // generate a private key of size 1024 and put this in to a file named private key
- `openssl req -new -key private.key -out cert.csr`
- `openssl x509 -req -in cert.csr -signkey private.key -out certificate.pem`

Note: this creates a self-signed certificate which is not entirely functional. In my version of the app it worked in Firefox and Safari, but Chrome didn’t allow fetching any resources from the server because it considered this certificate to be invalid.

Then run the app locally:

### Start the MongoDB server
-	Create a folder named mongodb on your computer and create a subfolder under it named data.
-	Navigate in the Terminal to the mongodb folder and then start the MongoDB server by typing

  `mongod --dbpath=data --bind_ip 127.0.0.1`

### Start the Express server
- `cd express-server`
- `npm install`
- `npm start`

### Start the React client 
- `cd react-client-for-express`
- `npm install`
- `npm start`
