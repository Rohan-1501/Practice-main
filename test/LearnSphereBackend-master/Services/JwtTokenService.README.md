# Documentation: JwtTokenService.cs

## Overview
`JwtTokenService.cs` generates the secure "Keys" (tokens) used to keep users logged in.

## Detailed Explanation
It builds a JSON Web Token (JWT) that contains specific "Claims" about the user, such as their Name, Email, and Role. This token is signed with a secret key from the `appsettings.json` file, making it tamper-proof.

## Line-by-Line Explanation: IJwtTokenService.cs

| Line(s) | Explanation |
| :--- | :--- |
| **5** | Defines the interface for the security token issuer. |
| **7** | `CreateToken`: Contract for generating a unique JWT string based on a `User` entity. |

## Line-by-Line Explanation: JwtTokenService.cs

| Line(s) | Explanation |
| :--- | :--- |
| **1-4** | Imports for JWT handling, Security Claims, and Token Validation logic. |
| **10** | Class implementing the `IJwtTokenService`. |
| **12-14** | **Constructor**: Expression-bodied constructor receiving `IConfiguration`. |
| **16** | `CreateToken`: Main logic for building the encrypted user session. |
| **18** | Grabs the "Jwt" section from the configuration file (`appsettings.json`). |
| **20-22** | Extracts the secret Security Key, Issuer, and Audience strings. |
| **24-25** | **Expiration Logic**: Sets a default of 120 minutes, or reads a custom value from settings. |
| **27** | **Security Layer**: Formats the secret key into a format the library understands (`SymmetricSecurityKey`). |
| **28** | Defines the algorithm (`HmacSha256`) used to sign the token signature. |
| **30-37** | **Claims Collection**: Populates the token with the User ID, Email, Name, and Role. |
| **39-45** | **Construction**: Assembles the Token object with the issuer, audience, claims, and credentials. |
| **47** | **Serialization**: Converts the Token object into the final triple-part string (`header.payload.signature`). |

## Program Flow
1. **Claims Collection**: Gathers user ID and role.
2. **Configuration**: Reads the secret key and expiration time (default 120 minutes) from settings.
3. **Signing**: Uses the `HmacSha256` algorithm to sign the token.
4. **Output**: Returns a long string that the frontend includes in every API request.

## Why it is used
It enables "Stateless Authentication". The server doesn't need to remember every logged-in user in its memory; it only needs to check if the token signature is valid.

## Interview Questions
1. **What is a "Claim" in a JWT?**
   *Answer: A Claim is a piece of information about the user (like "Role: admin") that is encoded inside the token. The server trusts these claims because the token is signed with a secret key.*
2. **Why is the secret key stored in `IConfiguration`?**
   *Answer: Security best practices. You should never hardcode secrets. By using configuration, you can use "Environment Variables" or "Azure Key Vault" in production to keep the key safe.*
3. **What happens when the token expires?**
   *Answer: The middleware on the backend will automatically return a `401 Unauthorized` response to the frontend, which typically redirects the user back to the Login page.*
