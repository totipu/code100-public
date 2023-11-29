# API Instructions

## API Base URL
```
https://challenger.code100.dev
```
## Login

In order to request the coding puzzle or submit the solution, you first need to retrieve the JSON Web Token by logging into the API:

### Request
```
POST /login
```
### Body
```
{
  "email": <email here>,
  "password": <password here>
}
```
### Response
```
{
   ...
   "token": <JWT token>
   ...
}
```
The token given in this response needs to be supplied for the Request Puzzle and Submit Solution requests.

## Request Puzzle

### Request
```
GET /getpuzzle
```
### Headers
```
Authorization: Bearer <JWT token>
```
### Response
```
{
  "name": <name of the puzzle>,
  "input": <coding challenge>
}
```
(coding challenge depends from round to round)

## Submit Solution

### Request
```
POST /postanswer
```
### Headers
```
Authorization: Bearer <JWT token>
```
### Body
```
{
  "answer": <your solution to the posted puzzle>
}
```
### Response

There is a 15 seconds delay for you to receive the answer for this request.
```
{
  "message": <information if solution was correct or not>,
}
```

# 1. JavaScript (node.js)

```javascript
const axios = require('axios');

// Step 1: Login
axios.post('https://challenger.code100.dev/login', {
  email: 'any',
  password: 'any'
})
  .then(response => {
    const token = response.data.token;

    // Step 2: Call Authenticated Endpoint
    axios.get('http://challenger.code100.dev/testauthroute', {
      headers: {
        Authorization: `Bearer ${token}`
      }
    })
      .then(authResponse => {
        console.log(authResponse.data);
      })
      .catch(error => {
        console.error(error);
      });
  })
  .catch(error => {
    console.error(error);
  });

```

# 2. Python

```python
import requests

# Step 1: Login
login_data = {"email": "any", "password": "any"}
login_response = requests.post("https://challenger.code100.dev/login", json=login_data)
token = login_response.json()["token"]

# Step 2: Call Authenticated Endpoint
headers = {"Authorization": f"Bearer {token}"}
auth_response = requests.get("http://challenger.code100.dev/testauthroute", headers=headers)
print(auth_response.json())
```

# 3. Java

```java
import java.net.HttpURLConnection;
import java.net.URL;
import java.io.OutputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class RestApiExample {

    public static void main(String[] args) {
        try {
            // Step 1: Login
            URL loginUrl = new URL("https://challenger.code100.dev/login");
            HttpURLConnection loginConnection = (HttpURLConnection) loginUrl.openConnection();
            loginConnection.setRequestMethod("POST");
            loginConnection.setDoOutput(true);
            OutputStream loginOutputStream = loginConnection.getOutputStream();
            loginOutputStream.write("{\"email\":\"any\",\"password\":\"any\"}".getBytes());
            loginOutputStream.flush();
            loginOutputStream.close();

            BufferedReader loginReader = new BufferedReader(new InputStreamReader(loginConnection.getInputStream()));
            String loginResponse = loginReader.readLine();
            loginReader.close();

            String token = loginResponse.split("\"")[3];

            // Step 2: Call Authenticated Endpoint
            URL authUrl = new URL("http://challenger.code100.dev/testauthroute");
            HttpURLConnection authConnection = (HttpURLConnection) authUrl.openConnection();
            authConnection.setRequestMethod("GET");
            authConnection.setRequestProperty("Authorization", "Bearer " + token);

            BufferedReader authReader = new BufferedReader(new InputStreamReader(authConnection.getInputStream()));
            String authResponse = authReader.readLine();
            authReader.close();

            System.out.println(authResponse);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
# 4. PHP
```php
<?php

// Step 1: Login
$loginData = json_encode(["email" => "any", "password" => "any"]);
$loginOptions = [
    "http" => [
        "method" => "POST",
        "header" => "Content-type: application/json",
        "content" => $loginData
    ]
];
$loginContext = stream_context_create($loginOptions);
$loginResult = file_get_contents("https://challenger.code100.dev/login", false, $loginContext);
$token = json_decode($loginResult)->token;

// Step 2: Call Authenticated Endpoint
$authOptions = [
    "http" => [
        "method" => "GET",
        "header" => "Authorization: Bearer $token"
    ]
];
$authContext = stream_context_create($authOptions);
$authResult = file_get_contents("http://challenger.code100.dev/testauthroute", false, $authContext);
echo $authResult;
?>
```
# 5. C++
```cpp
#include <iostream>
#include <curl/curl.h>

int main() {
    // Step 1: Login
    CURL *loginCurl;
    curl_global_init(CURL_GLOBAL_DEFAULT);
    loginCurl = curl_easy_init();

    if (loginCurl) {
        curl_easy_setopt(loginCurl, CURLOPT_URL, "https://challenger.code100.dev/login");
        curl_easy_setopt(loginCurl, CURLOPT_POSTFIELDS, R"({"email": "any", "password": "any"})");

        curl_easy_perform(loginCurl);
        curl_easy_cleanup(loginCurl);
    }

    // Extract the token from the login response and set it as a header for the next request

    // Step 2: Call Authenticated Endpoint
    CURL *authCurl;
    authCurl = curl_easy_init();

    if (authCurl) {
        struct curl_slist *headers = NULL;
        headers = curl_slist_append(headers, "Authorization: Bearer <TOKEN>");

        curl_easy_setopt(authCurl, CURLOPT_URL, "http://challenger.code100.dev/testauthroute");
        curl_easy_setopt(authCurl, CURLOPT_HTTPHEADER, headers);

        curl_easy_perform(authCurl);
        curl_easy_cleanup(authCurl);
    }

    return 0;
}
```

# 6. C#
```c#
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        // Step 1: Login
        using (var client = new HttpClient())
        {
            var loginData = new StringContent("{\"email\": \"any\", \"password\": \"any\"}", Encoding.UTF8, "application/json");
            var loginResponse = await client.PostAsync("https://challenger.code100.dev/login", loginData);
            var token = await loginResponse.Content.ReadAsStringAsync();

            // Step 2: Call Authenticated Endpoint
            client.DefaultRequestHeaders.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token.Trim('"'));
            var authResponse = await client.GetAsync("http://challenger.code100.dev/testauthroute");
            var result = await authResponse.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```
# 7. Ruby
```ruby
require 'net/http'
require 'json'

# Step 1: Login
uri = URI('https://challenger.code100.dev/login')
http = Net::HTTP.new(uri.host, uri.port)
request = Net::HTTP::Post.new(uri.path, {'Content-Type' => 'application/json'})
request.body = { email: 'any', password: 'any' }.to_json
response = http.request(request)
token = JSON.parse(response.body)['token']

# Step 2: Call Authenticated Endpoint
uri = URI('http://challenger.code100.dev/testauthroute')
http = Net::HTTP.new(uri.host, uri.port)
request = Net::HTTP::Get.new(uri.path, {'Authorization' => "Bearer #{token}"})
response = http.request(request)
puts response.body
```
# 8. Rust
```rust
use reqwest;

#[tokio::main]
async fn main() -> Result<(), reqwest::Error> {
    // Step 1: Login
    let login_data = json!({
        "email": "any",
        "password": "any"
    });

    let login_response = reqwest::Client::new()
        .post("https://challenger.code100.dev/login")
        .json(&login_data)
        .send()
        .await?;

    let token = login_response.json::<serde_json::Value>().await?["token"].as_str().unwrap();

    // Step 2: Call Authenticated Endpoint
    let auth_response = reqwest::Client::new()
        .get("http://challenger.code100.dev/testauthroute")
        .header("Authorization", format!("Bearer {}", token))
        .send()
        .await?;

    let auth_result = auth_response.text().await?;
    println!("{}", auth_result);

    Ok(())
}
```
