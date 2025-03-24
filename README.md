# API Testing wih herokuapp

The HerokuApp Hotel Booking Website is a test platform designed for practicing API automation and software testing. It simulates a real-world hotel booking system, allowing users to search make reservations, and manage bookings through API requests.
This project focused on automating API testing to ensure functionality, security, and performance of the booking system.

## POST Request – Logging in to the app
First, we need to authenticate users, as user must have an existing account before logging in. The status code for this is 200. In the response body, a token will be generated.

```POST https://restful-booker.herokuapp.com/booking ```

```json
{
  "username":"admin"
  "password": "password123"           `
}
```
## POST Request – Create a New Hotel Booking

```POST https://restful-booker.herokuapp.com/booking```

### Request Body (JSON):
```json
{
    "firstname" : "Ade",
    "lastname" : "Temi",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}
```
### Expected Response (201 Created):

```json
{
   "Bookingid" : "547",
   "firstname" : "Ade",
    "lastname" : "Temi",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}
```
### Validation in Test Script  
Responses should be validated because we need to ensure API returns accurate data.

```json
   pm.test("status code is 201", () => {
    pm.response.to.have.status(200);
});
var JsonDataa=JSON.parse(responseBody)
pm.environment.set("booking_id",JsonDataa.bookingid);
pm.test("content-Type header is present", () => {    
    pm.response.to.have.header("Content-Type");});
```
## GET Request – Retrieve all Booking

This is used to retrieve data from a server. I am retrieving all the bookings from the server

``` GET https://restful-booker.herokuapp.com/booking ```

## GET Request – Retrieve a Booking through bookingID
I am retrieving the details o the user that we created through the bookingid

``` https://restful-booker.herokuapp.com/booking/547 ```

### Validation in Test Script  
Responses should be validated because we need to assert the values of json fields.

```json
pm.test("values of json fields",()=>
{
var JsonDataa=pm.response.json()
pm.expect(JsonDataa.firstname).to.eql(pm.environment.get("Fname_env"));
pm.expect(JsonDataa.lastname).to.eql(pm.environment.get("Lname_env"));
})
```

## PUT Request – Update a User Booking 
The name of the user needs to be changed but we need an authorization before we can access this API. We also need bookingID to access this.

``` https://restful-booker.herokuapp.com/booking/547 ```

### Autentication:
```json
   username:"admin"
   password: "password123" 
```

### Request Body (JSON):

```json
{
    "firstname" : "Olu",
    "lastname" : "Tade",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}
```
### Response Body (JSON):
```json
{
    "firstname" : "Olu",
    "lastname" : "Tade",
    "totalprice" : 111,
    "depositpaid" : true,
    "bookingdates" : {
        "checkin" : "2018-01-01",
        "checkout" : "2019-01-01"
    },
    "additionalneeds" : "Breakfast"
}
```

## DELETE Request – Delete a User Booking 

``` https://restful-booker.herokuapp.com/booking/547 ```

### Response Body (JSON):
``` 201 Created ```
