<h3>Workflow</h3>

* First, user Logged in using Username and Password.
* When logged-in successful, we store user **identity** in their browser for further authorization on pages and keep user logged-in in site.<br/>
But this doesn’t mean: we will always keep user logged-in, it will be dangerous, so we will keep the identity of user for short period of time.
After that time user will again need to authenticate using username and password.

<h2>How we really Implement this in modern web applications using MERN Stack
</h2>
We implement these tasks of Authentication and Authorization using Register Form, Login Form and JWT token at backend.

**First user account creates then user became able to login and access the site resources.**

<h3>We implement security using:</h3>

* Register API
* Login API
* Password hashing with bcrypt
* Token-based authentication using JWT
* Protected routes on backend & frontend

<h3>User Registration Flow</h3>

**At Frontend**

* User fills the registration form.
* Form data is sent to backend using a POST /register request.

**At Backend**  

* Receive user data.
* Check if username or email already exists.
* If exists → return error: "User already exists".
* If not:

* Hash the password using bcrypt.
* Save user data in MongoDB.
* Return success: "User registered successfully".

<h3>User Login (Authentication)</h3>

**At Frontend**

* User enters username & password.
* Data is sent to POST /login endpoint.

**At Backend**

* Find user by username.
* If not found → "Invalid username or password" response from API.
* If user exist: Compare submitted password with stored hashed password using bcrypt.compare().
* If mismatch → "Invalid username or password" response from API.
* If match → User is authenticated.

<h3>Now, After user successful authentication:</h3>

<h2>JWT Authentication Flow</h2>

**After successful login:**

**At Backend**

We generate a JWT token using user unique data payload and JWT secret Key.
We store JWT secret key securely in our backend env.

<code>
  const token = jwt.sign(
  { userId: user._id, role: user.role },
  process.env.JWT_SECRET,
  { expiresIn: "1d" }
);
</code>

then, **Backend sends:**

* login success message
* JWT token
* (optional) user details

  in response.

**At Frontend:**

We store the token in localStorage or httpOnly cookie
(localStorage is common but httpOnly cookie is more secure).


<h2>Using JWT and 2 layer check for Protected Routes (Authorization)</h2>

**At Frontend:**

When opening a protected page:

* Check if token exists in localStorage.
* If not → redirect to login.
* If token exist: using authcheck API, we send token to server for verification.
* If server verify token is correct, then allow access to page else redirect to login page.

**At Backend: 2 layer check**

Check token correctness using **jwt.verify(token,JWT_SECRET).**
* if failed: Return 401 Unauthorized, Message: "Invalid or expired token."
* Else: using decoded token, we access the user id and role.
* we check if user exist.
* if user not exit then we return → **"message: Access denied.", "status code: 401", and status: Failed** in response 
* If user exist then we check if role has required permission and role is correct as stored in db.
* If all correct, then return → **"Authorization successful" and status: success** in response. 




<h3>For Login Page/Route</h3>

On login page/register page route, we check if user is authenticated then we will redirect user to homepage with message **"You are already Authenticated"**
