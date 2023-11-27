# 1. JavaScript (node.js)

```javascript
const axios = require('axios');

const apiUrl = 'https://api.example.com/data';
const authToken = 'your_auth_token';

// Basic Authorization Header
const headers = {
  'Authorization': `Bearer ${authToken}`,
  'Content-Type': 'application/json', // Adjust content type as needed
};

// Making GET request
axios.get(apiUrl, { headers })
  .then(response => {
    // Handle the API response
    console.log(response.data);
  })
  .catch(error => {
    // Handle errors
    console.error('Error:', error.message);
  });

```
