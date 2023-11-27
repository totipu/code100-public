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
