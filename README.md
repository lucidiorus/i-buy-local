# I BUY LOCAL APPLICATION

## Introduction
In these days of quarantine because of the coronavirus effect, this application arises as an opportunity to put in touch local small business with the people living in the same neighborhood.

The application works as a SAAS, where businesses and customers can create a profile and interact with each other. The idea of the application is that businesses can create profile, upload products, and allow customers to make orders, chat with them, ask for takeaway orders, and much more.

The curren application is not finished but meets the requirements to pass the rubric.


## Roles
There are 2 different roles. Each role has been created in auth0 and have different permissions. For example, a business can see some information from customers, but not the long representation with condifential data, and same for customers.

### Businesses
A business represent a small local business which can create a profile and add products

### Customers
A customer represent a person who wants to buy products from local businesses placed near to him


## CODE STRUCTURE

- `app.py`: contains the basic structure to create the flask application, the endpoints in charge of responding to the frontend requests, and the error handles.

- `model.py`: contains the models of the application. Businesses, Customers, and Products.

- `auth.py`: contains the code to manage the auth0 authentication, looking for JWT tokens and decoding them. It launch an error when there is a problem with the token provided.

- `test_app.py`: includes unittest for the different endpoints of the application

- `capstone-i-buy-local.postman`: collection of test in postman

- `manage.py`: code to carry out the migrations for heroku

- `requirements.txt`: all the needed packages that need to be installed

- `/migrations`: include the migrations of the database

- `setup.sh`: includes the environment variables needed to test the application



## Getting Started

Developers using this project should already have Python3 and pip installed on their local machines.

To create a virtual environment on Windows:

    py -m venv env

To activate it:

    .\env\Scripts\activate


Only the backend has been development so far, so there is no frontend. To check the responses, there is a postman collection with all the possible requests.

From the main folder run
    
    pip install requirements.txt. 

All required packages are included in the requirements file.

If you want to test the code in local you need to create a database in postgres called 'i_buy_local' and set the next enviromental variable with the URL of your database

    $env:DATABASE_URL="postgres://{user}:{password}@localhost:{port}/i_buy_local"


If you are going to run the `test_app.py` with the unittest for the endpoints, you also need to provide the next envoironment variables, provide in the setup.sh
```
$env:CUSTOMER_TOKEN= get it from stup.sh
$env:BUSINESS_TOKEN= get it from stup.sh
$env:AUTH0_DOMAIN= get it from stup.sh
$env:API_AUDIENCE= get it from stup.sh
```

To run the application in a local machine, you need to set the next environment variables
```
$env:FLASK_APP='app'
$env:FLASK_ENV='development'
flask run --reload
```



## Testing the deployed app in Heroku

You can test the application directly in the remote url hosted in heroku. The URL to access the API is 

    https://i-buy-local.herokuapp.com/

The postman collection has been configured to access this url.

It has 2 folders, one called business, with a valid token including all the permissions of the business role. And another one called customer, with a valid token including all the permissions of the customer role.

It is possible to run this collection and check that all the test sucess.

These tests check the same APIs that the file test `test_app.py` does


## API Reference

### Error Handling

Errors are returned as a JSON object with the next format:
```
{
    "error": 400,
    "sucesss": False,
    "messace": "Bad Request"
}
```

The API returns the next error types
- 400: Bad request
- 401: Message related to authorization header or token. (Token expired, token not found,...)
- 403: Permission not found on token
- 404: Resource not found
- 405: Method not allowed
- 422: Unprocessable Entity


### Roles and permissions

- Business:
    - get:businesses
    - post:business
    - delete:business
    - get:business-details
    - get:customers

- Customer:
    - get:customers
    - post:customer 
    - delete:customer 
    - get:customer-details
    - get:businesses


### ENDPOINTS

- ***GET /businesses   (Auth Required - get:businesses)***

Get a list of all the business in the app showing non confidential information

example `/businesses`

response
```
{
  "businesses": [
    {
      "address": "Street 2",
      "email": "business2@business2.com",
      "id": 2,
      "name": "Business2",
      "phone": "666666662"
    },
    {
      "address": "Street 4",
      "email": "business4@business4.com",
      "id": 4,
      "name": "Business4_B",
      "phone": "666666664"
    }],
  "status": 200,
  "success": true
}
```

- ***GET /businesses/<int:id>   (Auth required - get:business-details)***

Get complete information of a business

example `/businesses/2`   GET

response
```
{
    "address": "Street 2",
    "email": "business2@business2.com",
    "id": 2,
    "name": "Business2",
    "phone": "666666662"
}
```

- ***POST /businesses   (Auth required - post:business)***

Create a new business

example `/businesses` POST

POST body
```
{ 
    'id':'10',
    'name': 'business10',
    'address': 'address10',
    'phone':'phone10',
    'cif':'cif10',
    'email':'business10@business10.com'
}
```

response
```
{
  "business": {
    "address": "address10",
    "cif": "cif10",
    "email": "business10@business10.com",
    "id": 10,
    "name": "business10",
    "phone": "phone10"
  },
  "status": 200,
  "success": true
}
```

- ***PATCH /businesses   (Auth required - post:business)***

Edit an existing business

example `/businesses`  PATCH

PATCH body
```
{ 
    'id':'10',
    'name': 'business10_mod',
    'address': 'address10_mod',
    'phone':'phone10',
    'cif':'cif10',
    'email':'business10@business10.com'
}
```

response
```
{
  "business": {
    "address": "address10_mod",
    "cif": "cif10",
    "email": "business10@business10.com",
    "id": 10,
    "name": "business10_mod",
    "phone": "phone10"
  },
  "status": 200,
  "success": true
}
```


- ***DELETE /businesses/<int:id>   (Auth required - delete:business)***

Delete an existing business

example `/businesses/2`  DELETE

response
```
{
  "business": 2,
  "status": 200,
  "success": true
}
```


- ***GET /customers   (Auth Required - get:customers)***

Get a list of all the customers in the app showing non confidential information

example `/customers`

response
```
{
  "customers": [
    {
      "email": "email@email2.com",
      "id": 1,
      "name": "customer 1",
      "phone": "555555555"
    },
    {
      "email": "customer10@customer10.com",
      "id": 10,
      "name": "customer10",
      "phone": "phone10"
    }
  ],
  "status": 200,
  "success": true
}
```

- ***GET /customers/<int:id>   (Auth required - get:customer-details)***

Get complete information of a customer

example `/customers/3`   GET

response
```
{
  "customer": {
    "address": "Street 3",
    "email": "customer3@customer3.com",
    "id": 3,
    "name": "Customers3",
    "phone": "666666663"
  },
  "status": 200,
  "success": true
}
```

- ***POST /customers   (Auth required - post:customer)***

example `/customer` POST

POST body
```
{
	"id": "3",
	"name": "Customers3",
    "address": "Street 3",
    "phone": "666666663",
    "email": "customer3@customer3.com"
}
```

response
```
{
  "business": {
    "address": "Street 3",
    "email": "customer3@customer3.com",
    "id": 3,
    "name": "Customers3",
    "phone": "666666663"
  },
  "status": 200,
  "success": true
}
```

- ***DELETE /customers/<int:id>   (Auth required - delete:customers)***

Delete an existing customer

example `/customers/3`  DELETE

response
```
{
  "customer": 3,
  "status": 200,
  "success": true
}
```