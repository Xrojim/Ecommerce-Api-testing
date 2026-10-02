In this hands-on Postman Mini Ecommerce API Testing:
✅ Extract values from API responses and store them in variables
✅ Use variables dynamically across multiple API requests
✅ Automate complete API test flows using Postman Collection Runner
✅ Validate responses with assertions and environment variables
✅ Run  entire test suite with a single click

Scenario Overview:
simple User Management API
The test flow involves:
Login → extract token
Create a user → use token  → extract user ID
Update user using the extracted ID
Delete the user
run all these steps as one automated flow using Postman Runner

Steps:

Step 1 - Create new Environment
Step 2 - Add a variable:  baseUrl → https://reqres.in
     token → (empty)
     userId → (empty)
Step 3 - Save and select the environment
Step 4 - Click “New” → Collection → Name it User Flow Automation
Step 5 - Create a new Post request Login (Extract Token)  {{baseUrl}}/api/login
Step 6 - Add body

{
   "email": "eve.holt@reqres.in",
   "password": "cityslicka"
}

Step 7 - In Tests tab add:
const response = pm.response.json();
pm.environment.set("token", response.token);
pm.test("Token is received", () =＞ {
   pm.expect(response.token).to.not.be.undefined;
});

Step 8 - Create Request 2 — Create User POST  {{baseUrl}}/api/users
Step 9 - Add body:

{
   "name": "John Doe",
   "job": "QA Engineer"
}

Step 10 - Headers: Authorization: Bearer {{token}}
Step 11 - Tests tab:

const response = pm.response.json();
pm.environment.set("userId", response.id);
pm.test("User ID is stored", () =＞ {
   pm.expect(response.id).to.not.be.undefined;
});

Step 12 - Request 3 — Update User UPDATE   {{baseUrl}}/api/users/{{userId}}  
Step 13 - Add body and headers as needed
Step 14 - Tests tab:

pm.test("User retrieved successfully", () =＞ {
   pm.response.to.have.status(200);
});

Step 15 - Request 4 — Delete User DELETE  {{baseUrl}}/api/users/{{userId}}
Step 16 - Headers: Authorization: Bearer {{token}}
Step 17 - Tests tab:

pm.test("User deleted", () =＞ {
   pm.response.to.have.status(204);
});

Step 18 - Add a Pre-request Script in “Login” to print timestamp: (Optional)

pm.environment.set("runStartTime", new Date().toISOString());
console.log("Run started at:", pm.environment.get("runStartTime"));

Step 19 - Open Collection Runner
Use Collection: User Flow Automation
Environment: (your environment)
Step 20 - Run the Collection and observe
Token gets stored
User created
User fetched
User deleted
All tests run automatically
✅ Expected Output:
All 4 requests executed in sequence
Tests show “Passed”
Token and userId handled dynamically
