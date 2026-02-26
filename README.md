# API Chaining – Postman Project

## 📌 Project Overview
This project demonstrates the implementation of API Chaining using Postman.  
It covers dynamic data generation, environment variable usage, token-based authentication, and response validation across dependent APIs.

The workflow simulates a real-world scenario where the response from one API request is used as input for another API request.

---

## 🔗 Objective
To validate end-to-end API workflow by:
- Creating a user via POST request
- Extracting the generated user ID
- Using that ID in a GET request
- Validating response consistency between APIs

---

## 🛠 Tools & Technologies Used
- Postman
- JavaScript (Pre-request & Test Scripts)
- Chai Assertion Library
- Environment Variables
- REST API

API Provider Used:
- GoRest Public API

---

## 🔐 Authentication Handling
- Implemented Bearer Token authentication
- Handled 401 Unauthorized error when token was missing or invalid

---

## 🔄 Workflow Implementation

### 1️⃣ Dynamic Data Generation (Pre-request Script)

To avoid hardcoded values and prevent **422 Unprocessable Entity** errors (duplicate email issue), dynamic data was generated:

```javascript
var random = Math.random().toString(36).substring(2);

var username = "ab" + random;
var useremail = "ab" + random + "@gmail.com";

pm.environment.set("email_env", useremail);
pm.environment.set("user_env", username);

2️⃣ POST Request – Create User

Request Body:

{
  "name": "{{user_env}}",
  "email": "{{email_env}}",
  "gender": "male",
  "status": "active"
}

Post-response script to extract User ID:

var jsonData = pm.response.json();
pm.environment.set("userid_env", jsonData.id);
3️⃣ GET Request – Fetch Created User

Endpoint:

https://gorest.co.in/public/v2/users/{{userid_env}}

The extracted userid_env is dynamically injected into the GET request.

✅ Response Validation

Cross-verification between POST and GET responses:

pm.test("Validate POST and GET entities", () => {

    var jsonData = pm.response.json();

    pm.expect(jsonData.id).to.eql(pm.environment.get("userid_env"));
    pm.expect(jsonData.email).to.eql(pm.environment.get("email_env"));
    pm.expect(jsonData.name).to.eql(pm.environment.get("user_env"));

});

✔ ID Validation
✔ Email Validation
✔ Name Validation
✔ End-to-end data consistency check

📊 Status Codes Covered

201 – Created

200 – OK

401 – Unauthorized

422 – Unprocessable Entity

🎯 Key Highlights

Implemented API Chaining using environment variables

Removed hardcoded values to prevent 422 errors

Worked with token-based authentication

Performed dependent API workflow validation

Executed dynamic data-driven API testing

👤 Author

Pratik Kumar Singh
QA | API Testing | Automation Enthusiast


-
