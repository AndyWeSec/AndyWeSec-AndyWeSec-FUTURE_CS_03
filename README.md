# AndyWeSec-AndyWeSec-FUTURE_CS_03
Repository for the Cyber Security Internship under the Fellowship Program at Future Interns (August 2026 – September 2026).
Task 3: API Security Risk Analysis (postman-echo.com)](#api-security-risk-analysis-postman-echocom

# API Security Risk Analysis: postman-echo.com

**Date of Assessment:** *September 2026*  
**Tools Used:** Postman  
**Classification:** 🟡 **Low–Medium Risk**

---

## 📋 Executive Summary
An API security review was conducted against a public test endpoint (`postman-echo.com`) using Postman to evaluate authentication controls and information disclosure risks. The endpoint was found to be fully accessible without authentication and to leak internal client runtime version information in its response headers — issues that, on a production API, would materially aid an attacker's reconnaissance and increase the risk of abuse by anonymous third parties.

---

## 🚨 Detailed Findings & Risk Breakdown

<img width="1470" height="739" alt="Postman GET request to postman-echo.com showing 200 OK JSON response and exposed headers" src="https://github.com/user-attachments/assets/c0d9577c-b38d-43cc-b9a2-a236dd623f03" />

*Figure 1.1: Automated HTTP GET request evaluation targeting the postman-echo.com server. The request returns a successful 200 OK response with a raw JSON payload, while the response also reveals architectural anomalies including public unauthenticated data routing and client software framework exposure.*

### 1. Unauthenticated Resource Endpoint with Client Software Reflection
* **What is the issue?** The endpoint accepts and processes GET requests with no authorization check of any kind, and its response headers reflect internal client runtime version information (`PostmanRuntime/7.56.1`).
* **Why does it matter?** Allowing arbitrary third parties to query the endpoint anonymously removes any control over who can access or abuse it. Exposing internal runtime versions in response headers hands reconnaissance data to a potential attacker, helping them map out developer environments and identify likely attack surfaces before attempting further exploitation.
* **Risk Level:** 🟡 **Low to Medium** *(severity is context-dependent — rises significantly if the endpoint sits in front of production data or business logic rather than a public test service)*
* **Remediation:** Implement authorization verification (e.g. OAuth2 or JWT validation) before processing incoming HTTP requests. Strip or sanitize descriptive internal client/server headers so they are not echoed back in responses.

---

## 🛠️ Summary Action Roadmap

1. **Immediate:** Confirm whether this endpoint pattern exists on any production API surfaces; if so, restrict public access immediately pending an authentication fix.
2. **Short Term:** Implement OAuth2 or JWT-based request validation on all endpoints intended for authenticated use only.
3. **Ongoing:** Review server and framework configuration to strip version-revealing headers (e.g. `X-Powered-By`, runtime identifiers) from all outbound responses.
