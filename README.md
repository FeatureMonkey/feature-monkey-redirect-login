# How to setup Feature Monkey login from domain

## Motivation

Your users won't have to login again after being redirected to feature monkey.🙈

## Steps

1. Get your `YOUR_API_SECRET` from feature monkey team page.
2. Generate user data with `full_name`, `email` and `picture`.
3. Use any [HS256](https://jwt.io/) algorithm client and generate `token` with your user data and `SECRET`. 
4. Redirect user to your domain at this url `DOMAIN`/sso/`token`.


## Client-Side JavaScript

```
  <a href="" style="font-size: 5rem;" target="_blank" id="link">url</a>

  <!-- include any hs256 library -->
  <script
    language="JavaScript"
    type="text/javascript"
    src="https://kjur.github.io/jsrsasign/jsrsasign-latest-all-min.js"
  ></script>
  <script>
    var Header = JSON.stringify({ alg: "HS256", typ: "JWT" });

    // add user data in payload
    var user_data = JSON.stringify({
      full_name: "<USER_FULLNAME>",
      email: "<EMAIL>",
      picture: "<PICTURE>",
    });
    //   add your secret
    var secret = "<YOUR_API_SECRET>";
    var token = KJUR.jws.JWS.sign("HS256", Header, user_data, secret);

    document.getElementById("link").href =
      "https://<YOUR_FEATURE_MONKEY_SUMDOMAIN>/sso/" + token;
  </script>
```

## Python

### Install a JWT library
`pip install PyJWT`

### Generate tokens and redirect to url
```
import jwt

secret = '<YOUR_API_SECRET>'
subdomain = '<YOUR_FEATURE_MONKEY_SUMDOMAIN>`

def get_redirect_url(user):
  user_data = {
      full_name: "<USER_FULLNAME>",
      email: "<EMAIL>",
      picture: "<PICTURE>",
  }
  return "http://" + subdomain + "/sso/" + jwt.encode(user_data, secret, algorithm='HS256')
```

## Go

### Install a JWT library

`go get github.com/dgrijalva/jwt-go`

### Generate tokens and redirect to url

```
import (
  "github.com/dgrijalva/jwt-go"
)

secret = '<YOUR_API_SECRET>'
subdomain = '<YOUR_FEATURE_MONKEY_SUMDOMAIN>`

func get_redirect_url(user map[string]interface{}) (string, error) {
  token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
      "full_name": "<USER_FULLNAME>",
      "email": "<EMAIL>",
      "picture": "<PICTURE>",
  })
  return "http://" + subdomain + "/sso/" + jwt.encode(user_data, secret, algorithm='HS256') + token.SignedString([]byte(secret));
}
```

## Node

### Install a JWT library

```
npm install --save jsonwebtoken
```

### Generate tokens and redirect to url

```
var jwt = require('jsonwebtoken');

var secret = '<YOUR_API_SECRET>'
var subdomain = '<YOUR_FEATURE_MONKEY_SUMDOMAIN>`

function get_redirect_url(user) {
  var userData = {
      full_name: "<USER_FULLNAME>",
      email: "<EMAIL>",
      picture: "<PICTURE>",
  };
  return "https://" + subdomain + "/sso/" + jwt.sign(userData, secret, {algorithm: 'HS256'});
}
```

## Java

https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt/0.7.0

```
import java.util.HashMap;

import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

public class FeatureMonkeySSO {
  private static final String Secret = '<YOUR_API_SECRET>'
  private static final String Subdomain = '<YOUR_FEATURE_MONKEY_SUMDOMAIN>`

  public static String getRedirectUrl(User user) throws Exception {
    HashMap<String, Object> userData = new HashMap<String, Object>();
    userData.put("full_name", <USER_FULLNAME>);
    userData.put("email", <EMAIL>;
    userData.put("picture", <PICTURE>);

    return "https://" + Subdomain + "/sso/" + Jwts.builder()
               .setClaims(userData)
               .signWith(SignatureAlgorithm.HS256, PrivateKey.getBytes("UTF-8"))
               .compact();
  }
}
```

