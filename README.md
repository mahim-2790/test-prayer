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

Make sure the request object is containing all the parameters.
			 
You need to provide the success page and cancel page url of your frontend application as shown.
Make sure to send the response url (In which you will get the response from payerURL about the payment.) of your backend application as shown.
