# Web Application Security Assessment

## Introduction

Web Application Security Assessment is the process of identifying, analyzing, validating, and reporting security vulnerabilities in a web application.

The main objective of a security assessment is to understand:

- How the application works
- How the application is designed
- What components are exposed
- What functionalities are available
- How authentication is implemented
- How authorization is implemented
- How sessions and cookies are managed
- How user input is processed
- Whether business rules can be bypassed
- Whether sensitive information can be accessed without proper authorization

«Important: Security testing should only be performed on applications and systems for which you have explicit authorization.»

---

## Part 1: Web Application Architecture

Before performing a security assessment, it is important to understand how the web application is designed and how different components communicate with each other.

A typical web application may contain:

- Client or browser
- Web server
- Application server
- Database server
- APIs
- Authentication services
- WAF
- Load balancer
- External integrations
- Cache and storage services

Basic Web Application Architecture
```
+------------------+
|      User        |
|     Browser      |
+--------+---------+
         |
         v
+------------------+
|    Web Server    |
|    / WAF / LB    |
+--------+---------+
         |
         v
+------------------+
| Application      |
| Server / APIs    |
| Business Logic   |
+--------+---------+
         |
         v
+------------------+
|     Database     |
| MySQL/PostgreSQL |
| MSSQL/MongoDB    |
+------------------+
```

Presentation Layer

The presentation layer is the part of the application that the user interacts with.

Examples:

- Login page
- Registration page
- Dashboard
- Forms
- Search functionality
- HTML
- CSS
- JavaScript

From a security testing perspective, we can look for:

- Cross-Site Scripting (XSS)
- DOM-based vulnerabilities
- Client-side validation bypass
- Sensitive information in JavaScript
- Insecure client-side logic

Application or Business Logic Layer

The application layer processes the user's requests and implements the application's business logic.

Examples:

- Login validation
- Authentication
- Authorization
- Session creation
- Payment processing
- File processing
- API processing
- Business workflows

Security testing may include:

- Authentication vulnerabilities
- Authorization vulnerabilities
- IDOR
- BOLA
- Business logic vulnerabilities
- SQL Injection
- SSRF
- Command Injection
- File upload vulnerabilities

Database Layer

The database layer stores application information.

Examples:

- User information
- Customer information
- Transactions
- Orders
- Password hashes
- Application configuration

Common databases include:

- MySQL
- PostgreSQL
- Microsoft SQL Server
- Oracle
- MongoDB

Security testing may include:

- SQL Injection
- Excessive database privileges
- Sensitive data exposure
- Weak database configuration
- Improper access control

Two-Tier Architecture

In a two-tier architecture, the client communicates directly with the application or database layer.
```
+-------------+       +----------------------+
|   Client    | <---> | Application/Database |
|   Browser   |       |       Server         |
+-------------+       +----------------------+
```
Potential security concerns include:

- Database credential exposure
- Weak access controls
- SQL Injection
- Excessive database privileges
- Direct database exposure
- Insecure client-side logic

Three-Tier Architecture

A three-tier architecture separates the application into three major layers:

1. Presentation Layer
2. Application Layer
3. Data Layer
```
+-------------------+
| Presentation      |
| Layer             |
| Browser / UI      |
+---------+---------+
          |
          v
+-------------------+
| Application       |
| Layer             |
| Business Logic    |
| APIs              |
+---------+---------+
          |
          v
+-------------------+
| Data Layer        |
| Database          |
| MySQL/PostgreSQL  |
+-------------------+
```
N-Tier Architecture

Modern applications can contain many additional components.
```
User
 |
 v
Browser
 |
 v
WAF
 |
 v
Load Balancer
 |
 v
Web Server
 |
 v
API Gateway
 |
 +------------------+
 |                  |
 v                  v
Service A        Service B
 |                  |
 v                  v
Database          Database
 |
 v
Cache / Storage
```
Modern applications may use:

- Microservices
- API gateways
- Cloud services
- CDN
- Message queues
- Third-party APIs
- Authentication providers
- Object storage
- Redis or other caching technologies

Security Point

The more components an application contains, the more potential entry points may exist.

Therefore, understanding the architecture is an important first step in a security assessment.

---

## Part 2: Attack Surface and Attack Vector

What Is an Attack Surface?

An attack surface is the collection of all entry points, components, interfaces, and functionalities that an attacker could potentially interact with or target.

Common web application attack surfaces include:

- Login
- Registration
- Forgot Password
- Password Reset
- OTP
- MFA
- Search
- File Upload
- File Download
- Profile
- Payment
- Checkout
- APIs
- Admin Panel
- User Management
- Reports
- WebSockets
- Mobile APIs
- Third-party integrations

Easy Definition

«Attack Surface = WHERE can I attack?»

What Is an Attack Vector?

An attack vector is the method or technique used to attack or exploit an entry point.

For example:
```
Attack Surface             Attack Vector

Login Page          -----> Brute Force

File Upload         -----> Malicious File Upload

Search Function     -----> XSS

API Endpoint        -----> IDOR / BOLA

Password Reset      -----> Token Manipulation

Payment Function    -----> Price Manipulation
```
Easy Definition

«Attack Vector = HOW can I attack?»

Attack Surface vs Attack Vector

Attack Surface| Attack Vector
Login Page| Brute Force
File Upload| Malicious File Upload
Search Function| XSS
API Endpoint| IDOR / BOLA
Password Reset| Token Manipulation
Payment Function| Price Manipulation

Easy Memory Trick

«Surface = WHERE
Vector = HOW»

---

## Part 3: Gray-Box Web Application Security Assessment

What Is Gray-Box Testing?

In a gray-box security assessment, the tester has partial information about the application.

The tester may receive:

- Application URL
- Test credentials
- Different user roles
- API documentation
- Application architecture
- Scope information
- Test accounts
- Business workflow information

However, the tester may not have complete internal information such as:

- Full source code
- Complete infrastructure details
- All internal documentation
- Complete database access

Black Box vs Gray Box vs White Box

Testing Type| Information Available
Black Box| Very limited information
Gray Box| Partial information
White Box| Extensive internal information and/or source code

Gray-Box Example

Black Box
```
Tester
  |
  | Very Limited Information
  v
Application
```
```
Gray Box

Tester
  |
  | Partial Information
  | Test Credentials
  | API Documentation
  | User Roles
  v
Application
```
```
White Box

Tester
  |
  | Source Code
  | Architecture
  | Internal Documentation
  v
Application
```
Gray-Box Assessment Approach

A typical gray-box assessment can follow these steps:
```
1. Understand Scope
        |
        v
2. Understand Architecture
        |
        v
3. Map Attack Surface
        |
        v
4. Configure Burp Suite
        |
        v
5. Unauthenticated Testing
        |
        v
6. Authentication Testing
        |
        v
7. Authorization Testing
        |
        v
8. Session & Cookie Testing
        |
        v
9. Input & API Testing
        |
        v
10. Business Logic Testing
        |
        v
11. Validate Findings
        |
        v
12. Report
```
Step 1: Understand the Scope

Before testing, identify what is authorized.

Collect information such as:

- Domains
- Subdomains
- Application URLs
- IP addresses
- APIs
- Mobile APIs
- Test accounts
- User roles
- Testing window
- Out-of-scope assets
- Restricted functionality
- Testing limitations

«Never test systems or assets that are outside the authorized scope.»

Step 2: Understand the Architecture

Try to understand:

- Is the application two-tier or three-tier?
- Is there a WAF?
- Is there a load balancer?
- Is there an API gateway?
- Are microservices used?
- What authentication mechanism is used?
- What user roles exist?
- What external integrations are present?

Step 3: Map the Application

Identify important functionality.
```
Application
 |
 +-- Login
 |
 +-- Registration
 |
 +-- Forgot Password
 |
 +-- OTP
 |
 +-- Dashboard
 |
 +-- Profile
 |
 +-- File Upload
 |
 +-- File Download
 |
 +-- Payment
 |
 +-- Reports
 |
 +-- APIs
 |
 +-- Admin Panel
 |
 +-- Logout
```
This process helps identify the application's attack surface.

---

## Part 4: Burp Suite

What Is Burp Suite?

Burp Suite is a web application security testing platform developed by PortSwigger.

It can be used to:

- Intercept HTTP/HTTPS traffic
- Inspect requests
- Inspect responses
- Modify requests
- Replay requests
- Analyze application behavior
- Test authentication
- Test authorization
- Test APIs
- Test session management
- Test input validation

Official documentation:

https://portswigger.net/burp/documentation

Why Is Burp Suite Important?

Without Burp Suite:

Browser --------------------> Server

With Burp Suite:

Browser ----> Burp Suite ----> Server
                 |
                 |
          Inspect Request
          Modify Request
          Inspect Response

Burp allows a tester to inspect:

- HTTP methods
- URLs
- Parameters
- Cookies
- Authorization headers
- Request headers
- Response headers
- Request body
- Response body
- Status codes
- Session tokens
- API requests

Configuring Burp Suite With a Browser

The purpose of configuring the browser with Burp Suite is to allow Burp to sit between the browser and the application.
```
Browser
   |
   | HTTP/HTTPS Request
   v
Burp Suite
   |
   | Inspect / Modify
   v
Web Server
   |
   | HTTP/HTTPS Response
   v
Burp Suite
   |
   v
Browser
```
A common Burp Proxy listener configuration is:

127.0.0.1:8080

Always verify the actual listener configuration inside Burp Suite.

Burp Proxy

Burp Proxy allows us to intercept HTTP/HTTPS traffic.

Example request:

GET /dashboard HTTP/1.1
Host: example.com
Cookie: sessionid=ABC123

We can inspect:

- HTTP method
- URL
- Headers
- Cookies
- Parameters
- Authentication tokens
- Request body

Burp Intercept

When interception is enabled:
```
Browser
   |
   | HTTP Request
   v
Burp Suite
   |
   | Request Captured
   |
   | Modify / Forward
   v
Server
```
For example:

GET /api/user/123 HTTP/1.1
Host: example.com
Authorization: Bearer TOKEN

A tester can analyze the request and test how the application handles controlled changes.

For example:
```
User ID: 123
     |
     v
User ID: 124
     |
     v
Compare Response
```
When performed against authorized test accounts, this can help identify authorization issues such as IDOR or BOLA.

Burp HTTP History

Burp HTTP History records application traffic.

It can help identify:

- API endpoints
- Hidden requests
- Parameters
- Cookies
- Authentication headers
- Response codes
- Unexpected endpoints
- Application functionality

Burp Repeater

Burp Repeater allows a tester to manually modify and resend requests.

Example:

GET /api/user/123 HTTP/1.1
Host: example.com
Authorization: Bearer TOKEN

The request can be sent to Repeater and controlled changes can be tested.

Repeater is useful for testing:

- Authorization
- IDOR
- BOLA
- Input validation
- Parameter manipulation
- Business logic
- API behavior

---

## Part 5: HTTP Response Status Codes

HTTP status codes indicate the result of an HTTP request.

There are five major groups:

Code Range| Meaning
1xx| Informational
2xx| Success
3xx| Redirection
4xx| Client Error
5xx| Server Error

1xx - Informational

Examples:

100 Continue
101 Switching Protocols

These indicate that the request is being processed or that additional communication is expected.

2xx - Success

Common examples:

200 OK
201 Created
202 Accepted
204 No Content

200 OK

The request was successfully processed.

Example:

HTTP/1.1 200 OK

201 Created

The request successfully created a resource.

202 Accepted

The request has been accepted for processing but may not have completed yet.

204 No Content

The request was successful but there is no response body.

3xx - Redirection

Common examples:

301 Moved Permanently
302 Found
304 Not Modified

These indicate redirection or cached resource behavior.

4xx - Client Errors

Common examples:

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
405 Method Not Allowed
409 Conflict
429 Too Many Requests

400 Bad Request

The server could not correctly process the request.

401 Unauthorized

The request requires valid authentication credentials.

«The HTTP term is "Unauthorized", although in practical security discussions this commonly means that the user is not authenticated or valid authentication credentials were not provided.»

403 Forbidden

The server understood the request but refuses to authorize it.

404 Not Found

The requested resource was not found.

405 Method Not Allowed

The HTTP method is not supported for the requested resource.

409 Conflict

The request conflicts with the current state of the resource.

429 Too Many Requests

The client has sent too many requests within a given period.

This status code is particularly useful when evaluating rate-limiting controls.

5xx - Server Errors

Common examples:

500 Internal Server Error
502 Bad Gateway
503 Service Unavailable

500 Internal Server Error

A generic server-side error occurred.

502 Bad Gateway

A gateway or proxy received an invalid response from an upstream server.

503 Service Unavailable

The server is currently unable to handle the request.

Important Security Point

Do not assume:

200 = Secure
403 = Secure
404 = Secure

A status code alone does not determine whether a vulnerability exists.

Always compare:

- Request
- Response status
- Response body
- Response headers
- Application behavior
- Authentication state
- User role

---

## Part 6: Unauthenticated Attacks

What Is an Unauthenticated Attack?

An unauthenticated attack occurs when a resource or functionality that should require authentication can be accessed or exploited without successfully logging in.

Normal Flow

User
 |
 v
Login
 |
 v
Authentication
 |
 v
Dashboard

Potentially Vulnerable Flow

Attacker
   |
   | Direct Request
   v
/api/user/123
   |
   v
Sensitive Information

If sensitive functionality is accessible without authentication when authentication should be required, it may indicate an access-control or authentication-related vulnerability.

Common Unauthenticated Vulnerabilities

Common examples include:

- Unauthenticated API access
- Unauthenticated admin functionality
- Unauthenticated IDOR
- Unauthenticated BOLA
- Unauthenticated file access
- Unauthenticated file upload
- Authentication bypass
- Password reset flaws
- Sensitive information disclosure
- Exposed debug endpoints
- Exposed management endpoints
- Unauthenticated SSRF
- Unauthenticated SQL Injection
- Publicly exposed sensitive endpoints

Important Testing Question

For every endpoint, ask:
```
Should authentication be required here?
              |
        +-----+-----+
        |           |
       YES          NO
        |           |
   Test whether   Verify that
   authentication it is intentionally
   is enforced    public
```
Not every publicly accessible endpoint is vulnerable.

The important question is whether the endpoint is supposed to be publicly accessible.

---

## Part 7: Authentication

What Is Authentication?

Authentication is the process of verifying the identity of a user.

Easy Definition

«Authentication = Who are you?»

Authentication Flow
```
Username
    +
Password
    +
MFA / OTP
    |
    v
Authentication
    |
 +--+--+
 |     |
Valid Invalid
 |     |
 v     v
Login  Reject
```
Authentication Functions to Test

Important authentication-related functionality includes:

- Login
- Registration
- Logout
- Forgot Password
- Password Reset
- Change Password
- OTP
- MFA
- Email Verification
- Account Recovery
- Remember Me
- SSO
- OAuth

Login Vulnerabilities

Common issues include:

- Brute Force
- Credential Stuffing
- Username Enumeration
- Authentication Bypass
- Weak Password Policy
- Missing Rate Limiting
- Default Credentials
- Weak Login Controls

Password Reset Vulnerabilities

Check for:

- Predictable reset tokens
- Token reuse
- Tokens that do not expire
- Reset token leakage
- Account takeover
- Improper identity verification
- Weak reset workflow
- Missing rate limiting

OTP Vulnerabilities

Check whether:

- OTP can be brute-forced
- OTP can be reused
- OTP does not expire
- Rate limiting is missing
- OTP generation is weak
- OTP verification can be bypassed
- OTP validation can be skipped

MFA Vulnerabilities

Common issues include:

- MFA bypass
- Step skipping
- Weak MFA recovery
- MFA not enforced for sensitive actions
- Improper session handling after MFA
- Weak fallback authentication

---

## Part 8: Authorization

What Is Authorization?

Authorization determines what an authenticated user is allowed to access or perform.

Easy Definition

«Authorization = What are you allowed to do?»

Authorization Flow
```
Authentication
      |
      v
    User
      |
      v
Authorization
      |
   +--+--+
   |     |
Allowed Denied
```
Authentication vs Authorization

Authentication| Authorization
Who are you?| What can you access?
Verifies identity| Verifies permissions
Login| Access control
Password / MFA| Role / ownership
Happens before authorization| Happens after identity is established

Example
```
Username + Password
        |
        v
Authentication
        |
        v
User = User A
        |
        v
Authorization
        |
        +----> Own Profile? YES
        |
        +----> Another User's Profile? NO
        |
        +----> Admin Panel? NO
```
Horizontal Authorization

Horizontal authorization issues occur when one user can access another user's resources at the same privilege level.

Example:
```
User A
   |
   | /api/user/101
   v
User A's Data


User A
   |
   | /api/user/102
   v
User B's Data
```
If User A can access User B's data without proper authorization, this may indicate an IDOR or BOLA vulnerability.

Vertical Authorization

Vertical authorization issues occur when a lower-privileged user can access functionality intended for a higher-privileged user.

Example:
```
Admin
 |
 +----> Admin Panel
 |
 +----> Delete User
 |
 +----> Manage Roles
```
```
Normal User
 |
 +----> Should NOT access Admin Functions
```
If a normal user can access administrative functionality, it may indicate vertical privilege escalation.

Authorization Vulnerabilities

Common authorization vulnerabilities include:

- IDOR
- BOLA
- Broken Function-Level Authorization
- Forced Browsing
- Vertical Privilege Escalation
- Horizontal Privilege Escalation
- API Authorization Flaws
- HTTP Method Authorization Bypass
- Parameter-Based Authorization Bypass
- Multi-Tenant Isolation Issues
- Missing Server-Side Authorization
- Role Manipulation

Authorization Testing Approach

Use authorized test accounts with different:

- Users
- Roles
- Privileges
- Resource ownership

Then compare:

User A -> Resource A -> Allowed

User A -> Resource B -> Should be Denied

Normal User -> Admin Function -> Should be Denied

Always verify that authorization is enforced on the server side.

---

## Part 9: Session and Cookie Security

What Is a Session?

HTTP is stateless.

This means that the server does not automatically remember previous requests.

Web applications therefore use sessions to remember an authenticated user.

Session Flow
```
Browser
   |
   | Username + Password
   v
Web Server
   |
   | Validate Credentials
   v
Session Created
   |
   v
Session Store
   |
   | Session ID
   v
Browser Cookie
   |
   | sessionid=ABC123
   v
Future Requests
   |
   v
Server Identifies User
```
Cookie Example

The server may send:

Set-Cookie: sessionid=ABC123

The browser may send the cookie in future requests:

Cookie: sessionid=ABC123

The server uses the session identifier to identify the user's session.

Important Cookie Attributes

Secure

The "Secure" attribute tells the browser to send the cookie only over HTTPS.

Example:

Set-Cookie: sessionid=ABC123; Secure

HttpOnly

The "HttpOnly" attribute prevents normal JavaScript from accessing the cookie.

Example:

Set-Cookie: sessionid=ABC123; HttpOnly

This can help reduce the ability of client-side scripts to directly read the session cookie.

SameSite

The "SameSite" attribute controls when cookies are sent with cross-site requests.

Common values include:

Strict
Lax
None

Example:

Set-Cookie: sessionid=ABC123; SameSite=Lax

Session and Cookie Vulnerabilities

Common vulnerabilities include:

1. Session Hijacking
2. Session Fixation
3. Session ID in URL
4. Missing Secure Flag
5. Missing HttpOnly Flag
6. Weak SameSite Configuration
7. Session Not Invalidated After Logout
8. Excessive Session Lifetime
9. Weak Session ID Generation
10. Insufficient Session Rotation

Session Fixation

Session fixation may occur when an attacker can cause a victim to use a session identifier known to the attacker and the application does not properly regenerate the session after authentication.

A secure approach is:
```
Before Login
Session ID = A
      |
      v
User Login
      |
      v
Session ID Regenerated
      |
      v
After Login
Session ID = B
```
The session identifier should be properly regenerated after authentication.

Session Invalidation

Before logout:
```
Browser ---> Session ABC123 ---> Server
                     |
                   Valid
```
After logout:
```
Browser ---> Session ABC123 ---> Server
                     |
                  Invalid
```
The old session should no longer provide authenticated access after logout.

Session Testing Checklist

Check:

- Is the session ID unpredictable?
- Does the session ID change after login?
- Does the session ID change after privilege changes?
- Is the session invalidated after logout?
- Does the session expire?
- Is the session lifetime reasonable?
- Is the "Secure" flag enabled?
- Is the "HttpOnly" flag enabled?
- Is "SameSite" configured appropriately?
- Is the session ID exposed in URLs?
- Can an old session be reused?

---

## Part 10: Business Logic Vulnerabilities

What Is a Business Logic Vulnerability?

A business logic vulnerability occurs when an attacker manipulates the intended application workflow or business rules to achieve an unintended result.

These vulnerabilities can be difficult for automated scanners to identify because the tester needs to understand how the application is supposed to work.

Example: Price Manipulation

Suppose a product costs:

Product Price = ₹1000

A normal request may look like:

POST /checkout HTTP/1.1

product=123
quantity=1
price=1000

An attacker may try to modify the request:

POST /checkout HTTP/1.1

product=123
quantity=1
price=1

If the server accepts the transaction for ₹1 instead of calculating the correct price using trusted server-side information, this may indicate a business logic vulnerability.

Secure Approach

The server should not blindly trust the price supplied by the client.

A safer approach is:

Client
 |
 | Product ID = 123
 v
Server
 |
 | Retrieve price
 v
Database
 |
 | Price = ₹1000
 v
Server
 |
 | Calculate final amount
 v
Payment

Common Business Logic Vulnerabilities

Examples include:

- Price Manipulation
- Quantity Manipulation
- Coupon Abuse
- Transaction Limit Bypass
- Workflow Bypass
- Race Conditions
- Duplicate Transactions
- Reward or Points Abuse
- Refund Abuse
- Negative Value Manipulation
- Feature Restriction Bypass
- Multi-Step Process Manipulation
- Payment Workflow Bypass
- Account Limit Bypass

Quantity Manipulation

Suppose a shopping application allows:

Quantity = 1

A tester may check how the server handles unexpected values such as:

Quantity = 0
Quantity = -1

The application should validate whether the quantity is logically valid.

Coupon Abuse

Normal workflow:
```
Apply Coupon
      |
      v
Validate Coupon
      |
      v
Apply Discount
```
Potentially vulnerable workflow:
```
Apply Coupon
      |
      v
Discount Applied
      |
      v
Coupon Reused Multiple Times
```
The server should enforce business rules such as:

- Maximum usage
- User eligibility
- Expiry
- Minimum purchase amount
- One-time use
- Product restrictions

Workflow Bypass

Suppose an application requires:
```
Step 1 -> Step 2 -> Step 3 -> Payment -> Confirmation
```
A tester may check whether Step 3 can be accessed directly without completing the required previous steps.

Normal:
```
Step 1
  |
  v
Step 2
  |
  v
Step 3
  |
  v
Payment
```

Potential Bypass:
```
Step 1 ----X
Step 2 ----X
Step 3
  |
  v
Payment
```
If security-sensitive steps can be skipped, this may indicate a business logic vulnerability.

Race Condition

A race condition may occur when multiple requests are processed at nearly the same time and the application does not correctly handle concurrent operations.

Example:

Account Balance = ₹1000

Request 1 ---> Withdraw ₹1000
Request 2 ---> Withdraw ₹1000

Both requests are processed before
the balance is correctly updated.

The application may incorrectly allow both transactions.

Race-condition testing should only be performed in an authorized testing environment.

---

## Part 11: Practical Web Application Security Assessment Methodology

A practical assessment can be organized into the following phases:
```
+----------------------------+
| 1. Understand Scope        |
+-------------+--------------+
              |
              v
+----------------------------+
| 2. Understand Architecture|
+-------------+--------------+
              |
              v
+----------------------------+
| 3. Map Attack Surface     |
+-------------+--------------+
              |
              v
+----------------------------+
| 4. Unauthenticated Testing|
+-------------+--------------+
              |
              v
+----------------------------+
| 5. Authentication Testing |
+-------------+--------------+
              |
              v
+----------------------------+
| 6. Authorization Testing  |
+-------------+--------------+
              |
              v
+----------------------------+
| 7. Session & Cookie Test  |
+-------------+--------------+
              |
              v
+----------------------------+
| 8. Input & API Testing    |
+-------------+--------------+
              |
              v
+----------------------------+
| 9. Business Logic Testing |
+-------------+--------------+
              |
              v
+----------------------------+
| 10. Validate Findings     |
+-------------+--------------+
              |
              v
+----------------------------+
| 11. Document & Report     |
+----------------------------+
```
---

## Part 12: Quick Assessment Checklist

Architecture

- [ ] Understand application architecture
- [ ] Identify web server
- [ ] Identify application server
- [ ] Identify databases
- [ ] Identify APIs
- [ ] Identify WAF or load balancer
- [ ] Identify third-party integrations

Attack Surface

- [ ] Login
- [ ] Registration
- [ ] Forgot Password
- [ ] OTP
- [ ] MFA
- [ ] File Upload
- [ ] File Download
- [ ] Search
- [ ] Profile
- [ ] Payment
- [ ] APIs
- [ ] Admin Functions
- [ ] WebSockets
- [ ] External Integrations

Authentication

- [ ] Login controls
- [ ] Brute-force protection
- [ ] Credential-stuffing protection
- [ ] Username enumeration
- [ ] Password policy
- [ ] Password reset
- [ ] OTP
- [ ] MFA
- [ ] Account recovery
- [ ] Session creation

Authorization

- [ ] Horizontal authorization
- [ ] Vertical authorization
- [ ] IDOR
- [ ] BOLA
- [ ] Admin access
- [ ] API authorization
- [ ] Function-level authorization
- [ ] Multi-tenant isolation

Session and Cookies

- [ ] Session ID randomness
- [ ] Session rotation
- [ ] Session expiration
- [ ] Logout invalidation
- [ ] Secure flag
- [ ] HttpOnly flag
- [ ] SameSite
- [ ] Session fixation
- [ ] Session ID exposure

Business Logic

- [ ] Price manipulation
- [ ] Quantity manipulation
- [ ] Coupon abuse
- [ ] Workflow bypass
- [ ] Transaction limit bypass
- [ ] Race conditions
- [ ] Duplicate transactions
- [ ] Refund abuse
- [ ] Reward abuse
- [ ] Negative values

---

## Part 13: Important Questions to Ask During an Assessment

While testing a web application, continuously ask the following questions.

Authentication

«Can I access this functionality without logging in?»

Authorization

«Can User A access User B's data?»

Privilege Escalation

«Can a normal user perform an administrative function?»

Session

«What happens if an old session is reused?»

API

«Does the API enforce the same authorization controls as the web application?»

Input Validation

«What happens if I modify this parameter?»

Business Logic

«Can I change the workflow or business rule to achieve an unintended result?»

---

## Part 14: Authentication vs Authorization vs Session

These three concepts are fundamental to web application security.
```
Authentication
      |
      | Who are you?
      v
    User
      |
      v
Authorization
      |
      | What can you do?
      v
Permissions
      |
      v
Session
      |
      | How does the application
      | remember you?
      v
Authenticated Requests
```
Easy Memory Trick

«Authentication = Who are you?»

«Authorization = What can you do?»

«Session = How does the application remember you?»

---

## Part 15: Attack Surface to Vulnerability Mapping

-Functionality| Possible Security Testing

- Login| Brute Force, Enumeration, Authentication Bypass
- Registration| Account Enumeration, Input Validation
- Forgot Password| Token Issues, Account Takeover
- OTP| Brute Force, Reuse, Rate Limiting
- File Upload| File Upload Validation
- File Download| IDOR, Access Control
- Search| XSS, Injection
- Profile| IDOR, Authorization
- Payment| Price Manipulation, Business Logic
- Admin Panel| Privilege Escalation
- API| BOLA, Authentication, Authorization
- Session| Session Fixation, Session Hijacking
- Cookie| Cookie path set to root, Http only and Secure flag, Samesite
- Coupon| Coupon Abuse
- Transaction| Race Condition, Limit Bypass

---

## Part 16: Key Takeaways

A good web application security assessment is not only about running automated scanners.

A security tester should understand:
```
Application
     |
     v
Architecture
     |
     v
Attack Surface
     |
     v
Authentication
     |
     v
Authorization
     |
     v
Session
     |
     v
Input Validation
     |
     v
Business Logic
     |
     v
Manual Validation
     |
     v
Reporting
```
The most important mindset is:

«Understand the application before trying to break it.»

Security tools can help identify and analyze technical behavior, but they cannot replace the tester's understanding of the application and its business workflow.

---

## Part 17: Simple Assessment Mindset

Remember these seven words:
```
UNDERSTAND
     |
     v
MAP
     |
     v
CAPTURE
     |
     v
ANALYZE
     |
     v
MANIPULATE
     |
     v
VALIDATE
     |
     v
REPORT
```
Understand

Understand the application's architecture, functionality, roles, and business workflow.

Map

Identify endpoints, parameters, APIs, functionality, and user roles.

Capture

Use tools such as Burp Suite to capture HTTP/HTTPS requests and responses.

Analyze

Understand how authentication, authorization, sessions, APIs, and business logic work.

Manipulate

In an authorized testing environment, modify requests and parameters to test security controls.

Validate

Confirm whether the observed behavior represents a genuine security vulnerability.

Report

Document the vulnerability with:

- Vulnerability Title
- Severity
- Affected URL or API
- Description
- Steps to Reproduce
- Request and Response Evidence
- Business Impact
- Recommendation
- References

---

## Part 18: Final Reminder

Web application security assessment is a combination of:
```
Technical Knowledge
        +
Application Understanding
        +
Security Testing
        +
Business Logic Understanding
        +
Manual Analysis
        +
Proper Reporting
```
Tools such as Burp Suite are extremely useful for understanding application behavior, but tools alone cannot replace a security tester's analysis.

The goal is not simply to find vulnerabilities.

The goal is to:

1. Understand how the application works.
2. Identify how the application can potentially be abused.
3. Validate the security impact.
4. Clearly document the vulnerability.
5. Provide recommendations that help make the application more secure.

«Understand the application first. Map the attack surface. Test the security controls. Validate the impact. Report the findings.»

---

Practice Environment

For hands-on learning, use intentionally vulnerable applications and authorized security labs.

A useful platform for learning web application security is PortSwigger Web Security Academy:

https://portswigger.net/web-security

«Always perform security testing only against systems where you have explicit authorization.»
