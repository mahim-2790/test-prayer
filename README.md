# PayerURL Integration for Node.js Applications

This guide provides step-by-step instructions for integrating PayerURL into your Node.js application, using Express.js for demonstration purposes.

## Prerequisites

1. Obtain your API public key and secret key from the [PayerURL website](https://www.payerurl.com).

## Request Setup

### Step 1: Install Node.js and Set Up Express.js

Follow the instructions at [Express.js](https://expressjs.com/) to set up your Express.js application.

### Step 2: Define Payment Route

Define a route for payment requests (e.g., "request" route).

### Step 3: Define Response Route

Define a response route to handle successful payment responses.

### Step 4: Prepare Request Object

Prepare an object with payment details:

```javascript
const paymentRequest = {
  order_id: 1922658446,
  amount: 123,
  items: [
    {
      name: "Order item name",
      qty: "1",
      price: "123",
    },
  ],
  currency: "usd",
  billing_fname: "Mohi Uddin",
  billing_lname: "Mahim",
  billing_email: "mahim@gmail.com",
  redirect_to: "http://localhost:3000/success",
  notify_url: "http://localhost:4000/response",
  cancel_url: "http://localhost:3000/cancel",
  type: "php",
};
```

Make sure the request object is containing all the parameters.
			 
You need to provide the success page and cancel page url of your frontend application as shown.
Make sure to send the response url (In which you will get the response from payerURL about the payment.) of your backend application as shown.

### Step 5: Write a View for the Request Route

In your Express.js application, create a view for the payment request route. Sort the object keys, build the query string, create an HMAC signature, and encode it in base64.

```javascript
// Sort the object keys in ascending order
const sortedArgsKeys = {};
Object.keys(paymentRequest)
  .sort()
  .forEach((key) => {
    sortedArgsKeys[key] = paymentRequest[key];
});

// Build the required query string
function buildQueryString(obj, prefix) {
  let queryString = [];
  for (const key in obj) {
    if (Object.prototype.hasOwnProperty.call(obj, key)) {
      const value = obj[key];
      const propName = prefix ? `${prefix}[${key}]` : key;
      if (value !== null && typeof value === "object") {
        queryString.push(buildQueryString(value, propName));
      } else {
        queryString.push(`${propName}=${encodeURIComponent(value)}`);
      }
    }
  }
  queryString = queryString.join("&");
  const argsString = new URLSearchParams(queryString).toString();
  return argsString;
}

// Create HMAC signature with sha256 hash
const argsString = buildQueryString(sortedArgsKeys);
const signature = crypto
  .createHmac("sha256", securityKey)
  .update(argsString)
  .digest("hex");

// Create auth string in base64 format
const authStr = btoa(`${publicKey}:${signature}`);
```

### Step 6: Prepare Request Headers and Properties

In this step, you will set up the necessary request headers and make the POST request to PayerURL.

```javascript
// Set up the request headers
const url = "https://test.payerurl.com/api/payment";
const headers = {
  "Content-Type": "application/x-www-form-urlencoded;charset=UTF-8",
  Authorization: `Bearer ${authStr}`,
};

// Prepare the request options
const requestOptions = {
  method: "POST",
  headers: headers,
  body: argsString,
};

// Make the request using fetch or axios
const response = await fetch(url, requestOptions);
const reData = await response.json();

### Step 7: Get Redirect URL

After making the payment request to PayerURL, you will need to extract the "redirectTO" from the response (`reData`) and send it to the frontend to redirect the user to this URL.


