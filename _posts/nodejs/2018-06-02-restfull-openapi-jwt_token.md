---
title: Open API Web Token 
date: 2018-06-02 09:00:00 +0900
layout: post
tags: [nodejs, "웹토큰", "JWT Toekn", "JSON Web Token"]
categories: [Programming]
---

## Open api key

API keys는 사용자 인증, 요청 API 호출을 위해 필요하다. 

보통 RESTFull API 호출시 API keys 와 인증 스킴을 사용한다. 주요 차이는:

 - API keys : 앱 혹은 사이트 호출을 식별해서 API 호출을 만다.
 - Auth tokens : 앱 혹은 사이트 사용자를 식별한다. 

이런 인증에 두가지 방식을 사용한다.

 - Session-based Authentication
 - Token-based Authentication

### Token based Authentication

토큰 기반 인증은

1. Stateless Authentication 이 큰 이유이다. 클라이언트 측 로컬 스토리지 (혹은 세션, 쿠키 가능)에 저장된다.
2. 좋은 예는 싱글 페이지 앱, Web ApIs, IoT가 있다
3. 앱이 확장성이 있고 분리되야 한다면, Token 기반이 좋은 사례가 된다.


> Stateless Authentication: 인증에 대해 서버측이 인증 정보를 보존하지 않는다.


### JWT

**JWT**(JSON Web Tokens)는 REST API를 사용한 간단하고 보안이 적용된 인증 전략이다. 웹 인증을 위한 표준이고 JSON token 요청을 기반으로 한다. 인증 서버에서 발급한 토큰을 클라이언트가 요청시 마다 헤더에 토큰을 넣어 요구해야 한다.

MEAN 스택 앱에 빠르게 인증을 추가하는 것은 [JSON Web Token](http://jwt.io/) 을 사용할 수 있다.



### How JWT Works?
 
[Node Js JWT Authentication Tutorial From Scratch](https://appdividend.com/2018/02/07/node-js-jwt-authentication-tutorial-scratch/)

JSON Web Token is the token; we need to put the header in every request to verify the client. The Architecture of JWT Authentication is pretty darn simple.

1. First user attempt to login with their credentials.
2. After server verifies the credentials, it sends JSON Web Token to the client.
3. A client then saves that token in local storage or any other storage mechanism.
4. Again if a client wants to request a protected route or resource, then it sends JWT in a request header.
5. The server verifies that JWT and if it is correct then return a 200 response with the information, client needs.
6. If the JWT is invalid, then it gives unauthorized access or any other restricted message.
7. Node js JWT Authentication

In this tutorial, we are not using any front-end framework. We will use POSTMAN  to request the server. We will check the auth using token. So let us get started.

#### Step 1: Install node js dependencies.

Create one project folder and go into that folder. Type the following command.

```
npm init
```

Now, install the following dependencies.

```
npm install express jsonwebtoken mongoose body-parser --save
```

It will install the express web framework, jsonwebtoken package to authenticate the user, mongoose schema model, and body-parser middleware.

Also, we need to install the nodemon development server to prevent the stop and restart the server process. So let us do that first.

```
npm install nodemon --save-dev
```

Rest dependencies we will install as our project grows.


#### Step 2: Configure the Node Server.

In the package.json file, change this object to the following.

```json
"scripts": {
    "start": "nodemon server"
},
```

So in the terminal when we type npm start command, we bootstrap the server.js file.
In the root folder, make one file called *server.js*. Configure the node server.

```js
// server.js

const express = require('express');
const app = express();
const bodyParser = require('body-parser');


const PORT = 3000;

app.listen(PORT, function(){
   console.log('Server is running on Port',PORT);
});
```

Now, go to terminal and hit the following instruction.

```
npm start
```

It will start the server, you can see it on a console. So it is ready to consume any request, either web or API.

#### Step 3: Send a request to node server via Postman.

First, we define one route and send the JSON response to the client.

```js
// server.js

app.get('/checking', function(req, res){
   res.json({
      "Tutorial": "Welcome to the Node express JWT Tutorial"
   });
});
```

Open the Postman and send the get request to http://localhost:3000/checking. Postman Tutorial

#### Step 4: Configure the MongoDB Database.

Write the following code to connect the Node.js application to the MongoDB database.

if below mongoose 3.x, 

```js
// server.js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/jwtauth');
```

Then use above mongoose 4.x, prepare *Promise* and connect().

```js
// Mongodb
mongoose.Promise = global.Promise;

mongoose.connect( 
  "mongodb://student:PW@localhost/students",
  { useMongoClient: true }
);
```

Also, write the body-parser middleware to the application.

```js
// server.js

app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());
```


#### Step 5: Create a User model.

Create one new folder inside root called models. In that, create one file called user.model.js file.

```js
// user.model.js
'use strict';

const mongoose = require('mongoose');

const user = mongoose.Schema({
   //_id: mongoose.Schema.Types.ObjectId,
   email: {type: String, required: true},
   password: {type: String, required: true}
});
module.exports = mongoose.model('User', user);
```

We have defined the schema for the User collection.

#### Step 6: Create the routes for users.

In the root, make one folder called routes. In that folder, create one file called user.route.js. Now we need to sign the user up for our application. So let us define the post route to signup the user. We also need a bcrypt module to hash the password. We can not store the plain password. So let us install bcrypt module first.

```
npm i bcrypt
```

Next, write the following code into the *user.route.js* file.

```js
// user.route.js

const express = require('express');
const router = express.Router();
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');
const User = require('../models/user.model');

router.post('/signup', function(req, res) {
   bcrypt.hash(req.body.password, 10, function(err, hash){
      if(err) {
         return res.status(500).json({
            error: err
         });
      }
      else {
         // const user = new User(req.body);
         const user = new User({
            // _id: new  mongoose.Types.ObjectId(),
            email: req.body.email,
            password: hash    
         });
         user.save().then(function(result) {
            console.log(result);
            res.status(200).json({
               success: 'New user has been created'
            });
         }).catch(error => {
            res.status(500).json({
               error: err
            });
         });
      }
   });
});

module.exports = router;
```

What it does is that it tries to hash the incoming request’s password property. If it fails to do so then returns a response with an error in json format. If it successes then it will create a new user and add that to the MongoDB database. Now include this user.route.js file in the server.js file. I am writing the whole file now.

```js
// server.js

const express = require('express');
const app = express();
const bodyParser = require('body-parser');
const user = require('./routes/user.route');
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost/jwtauth');

const PORT = 3000;

app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.get('/checking', function(req, res){
   res.json({
      "Tutorial": "Welcome to the Node express JWT Tutorial"
   });
});

app.use('/user', user);

app.listen(PORT, function(){
   console.log('Server is running on Port',PORT);
});
```

#### Step 7: Send a post request from the Postman.

postman 에서 POST 방식으로 새 사용자를 추가해 보자, POST에서 Body를 Raw 이고 형식은 **json(application/json)** 를 선택하고 작성한다.

```json
{ 
  "firstName":"",
  "lastName":"",
  "email": "",
  "password": ""
}
```

![](/images/nodejs/auth-jwt-signup-postman.png){:width="500"}
<figcaption>[그림. Postman에서 JSON 포스트]</figcaption>



You can see here, I have created user successfully. Now, I am using Studio 3T for MongoDB. So here is the newly created user in the database.

이제 실제 암호화된 비밀번호로 생성된 사용자 정보를 확인할 수 있다.

![](/images/nodejs/auth-jwt-signup-db.png){:width="500"}
<figcaption>[그림. 새 사용자 정보]</figcaption>


MongoDB Tutorial With An Example


#### Step 8: Sign In the User.

사용자가 생성되고 비밀번호로 bcryp로 비밀번호가 생성되면 `bcrypt.compare()` 를 사용해 사용자 비밀번호를 인증할 수 있다.

```js
// user.route.js

router.post('/signin', function(req, res){
   User.findOne({email: req.body.email})
   .exec()
   .then(function(user) {
      bcrypt.compare(req.body.password, user.password, function(err, result){
         if(err) {
            return res.status(401).json({
               failed: 'Unauthorized Access'
            });
         }
         if(result) {
            return res.status(200).json({
               success: 'Welcome to the JWT Auth'
            });
         }
         return res.status(401).json({
            failed: 'Unauthorized Access'
         });
      });
   })
   .catch(error => {
      res.status(500).json({
         error: error
      });
   });;
});
```

사용자 이메일로 허가된 사용자인지 인증하는데, 이메일이 존재하지 않으면 **401 unauthorized access** 에러를 출력한다.

![](/images/nodejs/auth-jwt-signin-postman.png){:width="500"}
<figcaption>[그림. Postman에서 Sign 포스트]</figcaption>

First, I have checked if the user’s email exists or not. If not then return 401 unauthorized access. If email is there then check the password with bcrypted database password if match found then welcome to the JWT auth else 401 unauthorized access.

이메일로 사용자 검색이 되고 비밀번호 인증이 되면 환영 JWT auth를 반환하고 아니면 **401 unauthorized access** 를 반환하게 한다.

 
#### Step 9: Return the JWT, if auth attempt successful.

사용자 증명인 이메일과 패스워드가 유효하면 JWT token을 반환하도록 하자, `jwt.sign()` 함수는 기본으로 HMAC SHA256 해시 코드를 발생한다.

```js
jwt.sign({
  email: user.email,
  _id: user._id
}
```

다른 알고리즘이 필요하면 *algorighm* 속성을 제시해 준다.

```js
jwt.sign({
  email: user.email,
  _id: user._id,
  { algorithm: 'RS256'}
}
```


If the user’s credentials email and password are valid then in response, we need to return a JWT token. So let us generate the token and return to the user.

```js
// user.route.js

const jwt = require('jsonwebtoken');

if(result) {
   const JWTToken = jwt.sign({
        email: user.email,
        _id: user._id
      },
      'secret',
       {
         expiresIn: '2h'
       });
       return res.status(200).json({
         success: 'Welcome to the JWT Auth',
         token: JWTToken
       });
  }
```

JWT Token의 형식은 HEADER, PAYLOAD, SECRETKEY 조합으로 아래 같다:

The format of JWT Token is as following.

**HEADER: ALGORITHM & TOKEN TYPE**

```json
{
“alg”: “HS256”,
“typ”: “JWT”
}
```

**PAYLOAD:DATA**

```json
{
“email”: “krunallathiya10@gmail.com”,
“_id”: “5a7c9bd8fc3e501c94aa6035”,
“iat”: 1518120124,
“exp”: 1518127324
}
```

**VERIFY SIGNATURE**

```json
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

조합으로 생성한 JWT는 아래 같은 JWT Token을 반환한다.

So it is a combination of header, payload, and secretkey, we are providing. Remember, You need to define your secret key in your environment variable file. I have just shown here for the demo purpose. So it will produce the following JWT token.

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImtydW5hbGxhdGhpeWExMEBnbWFpbC5jb20iLCJfaWQiOiI1YTdjOWJkOGZjM2U1MDFjOTRhYTYwMzUiLCJpYXQiOjE1MTgxMjAxMjQsImV4cCI6MTUxODEyNzMyNH0._6qVGQV_KYlonawnaTHG-OhOJLV4tgD-Eob5iRz89AM
```


이 토큰을 제한된 자원에 접근하는 앱에 사용할 수 있다.

Now use this token to access the protected resources to your application and we are done here. So this is how you can generate the JWT Auth Token. 


#### JWT Token verifying

`jwt.verify()`를 사용해 발급한 토큰이 허용되는지 검증할 수 있다.

> [jwt.verify(token, secretOrPublicKey, [options, callback])](https://github.com/auth0/node-jsonwebtoken#jwtverifytoken-secretorpublickey-options-callback)
>  - `token`: 발행된 Json Web Token
>  - `secretOrPublicKey`: 토큰 발행시 제공한 시크릿 키


```js
router.post('/verify', function(req, res) {
    const user = User.findOne({email: req.body.email});

   // check expired?
   jwt.verify(req.body.token, user.secret_key, function(err, decoded) {
     if (err) {
      console.log(err);
      return res.status(200).json(err);
     }
   });
});
```


## 참조

 - [npm:JSONWebToken](https://www.npmjs.com/package/jsonwebtoken)
 - [Node Js JWT Authentication Tutorial From Scratch](https://appdividend.com/2018/02/07/node-js-jwt-authentication-tutorial-scratch/)
 
  - [Why and When to Use API Keys](https://cloud.google.com/endpoints/docs/openapi/when-why-api-key)
  - https://appdividend.com/2018/02/07/node-js-jwt-authentication-tutorial-scratch/
  - [Securing Node.js RESTful APIs with JSON Web Tokens](https://medium.freecodecamp.org/securing-node-js-restful-apis-with-json-web-tokens-9f811a92bb52)