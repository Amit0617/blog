---
title: "Vulnerabilities guide for web developers (of digestible length"
datePublished: Tue Mar 14 2023 17:37:47 GMT+0000 (Coordinated Universal Time)
cuid: clf8jdtga00030amqezas5p8k
slug: vulnerabilities-guide-for-web-developers-of-digestible-length
tags: web-development, infosec-cjbi6apo9015yaywu2micx2eo, cybersecurity-1, owasp-top-10

---

### SQL injection

Bypassing through `' OR True #`

1. Securing web applications - using methods given by the framework or libraries you are utilizing for development. There are often opinionated way provided to use user input inside a SQL query to get results from database.
    
2. Mainly you want to prevent concatenating user input directly to SQL queries.
    
3. Use libraries with ORM(object relational mapping) support.
    

### Command Injection

Very similar to previous one but this time we are executing on a shell. Concatenating user input with things like `;`, `&&`, `||`,`|` or simply just being creative maybe you can use redirection( `>` ) to store output in a file and calling a curl to post the output to your server by piping and things like that.

It's a bad idea to use user input with any command and putting them in between. Skip it entirely if possible or escape the user input properly or simply make sure to research for alternative ways to achieve that.

### Broken Authentication

Things which should be kept in mind

1. Exposed Session IDs in URL of client which should be completely server side.
    
2. Rate limiting failed attempts - To prevent
    
    1. brute-forcing (randomly trying all possible combinations of allowed characters up to password length limit) for guessing weak passwords and
        
    2. credential stuffing (using username and passwords from leaked/breached databases).
        
3. Strong Credential Recovery - To prevent account access by wrong user.
    
    1. Weak user identification system (like easy to guess recovery questions).
        
4. Multi-Factor Authentication.
    
5. Not Allowing Weak passwords by users.
    
6. Make sure that none of the software hosted by you(as an organisation) is shipped with default passwords.
    

### Sensitive information disclosure

1. Prevent exposing Software Version Details (which can be used to find possible CVEs)
    
2. Prevent exposing files with sensitive information (like config files for server)
    
3. Prevent exposing logs.
    
4. Prevent exposing Errors very verbosely, as they can also work as a hint to an attacker. Prefer use of try-catch statements. One of the famous examples, handling login failures as "User name and/or password is invalid" rather than throwing errors exactly as coming from databases like password is invalid or username doesn't exists.
    
5. Use of https protocol over http as the encrypted transmission from client to server takes place while in http if you user logs in to the account then username and password are sent in plain text.
    
6. Store sensitive information of user in database as hashes. Things like passwords, user government ID no. etc if stored in *hashes with salting* and strong password policy at the place makes cracking painstakingly difficult.
    
7. Keeping de-compilers and reverse engineering in mind and not storing hard-coded passwords, API keys and anything considered sensitive to be exposed publicly.
    

### Broken Access Control

1. Having a user access control on APIs. Requests like POST, PUT, UPDATE AND DELETE can be dangerous if they are directly forwarded to databases as it is. There should be an access hierarchy for various roles. (General advice: Instead of having a structure where you are giving every access to all roles and then limiting the control to lower user roles, prefer giving zero access to everyone first and then adding more and more controls with increasing level on hierarchy.)
    
2. Verify access control on server side every-time. This is one of the common implementation mistakes which lots of new people in the field do that they blindly believe what's on client side - which can easily be manipulated. Let me make it clear that there are various proxy tools which allow manipulating:
    
    1. a request sent from client to server *annnd...*
        
    2. response coming from server to client.
        

That means you shouldn't be asking server things like `isAdmin` because it can be changed from `false` to `true`. Rather you should be sending requests to server, verify access control server side, perform actions if allowed and then send the response and use client side to render results only.

1. Log access control failures. This can mostly help in identifying malicious attempts.
    

### Security Misconfigurations

1. Misconfigurations related to cloud permissions or IAM related.
    
2. Beware of buffer overflows which can crash server and/or client. Buffer overflows can also cause unauthorized access and command execution etc. Application server and all internet infrastructure should be kept updated and security patches should be applied timely.
    
    1. Review all your application code wherever a user input is sent through a HTTP request.
        
    2. Apply checks to ensure all those functions can handle arbitrarily large user input.
        
    3. Even secured applications should have mitigating steps for large user input as they
        
        1. can certainly increase processing time,
            
        2. cause delay in server response,
            
        3. can eat up high amount of resources or
            
        4. cause dos (Denial of Service).
            
3. Shipping only the required application in production. This happens mostly on big organisational level where
    
    1. unnecessary ports are left open,
        
    2. unnecessary features or software is hosted, (which could increase attack surface with newly disclosed vulnerabilities)
        
    3. unused accounts which could easily get left with under-managed permissions are present.(which could be forgotten to update with changing rules, hierarchy structure or user control access.)
        
4. Throwing too much informative errors, i.e., having debugging mode enabled. Errors, in general, should be handled by displaying information which is explanatory to a normal user but it shouldn't be raw technical error thrown by system as this can mostly tell the flow of code and/or information etc.
    

### XSS (Cross Site Scripting)

1. Never trusting user input. Specially in cases like where you wanna show something which is coming from user input (like displaying text which is given through user input). Make sure to escape it and never use it as code rather consider user input as malicious and dangerous.
    
2. Don't try to be smart by using encoding or regex filtering for sanitizing user input as those can be easily bypassed. The list of filter bypasses will tend to infinite given types of encoding like html, base64, url, hex, ascii, unicode or even UTF-7 characters with double or triple encodings. The good practice is always to create a whitelist of what is allowed rather than creating a blacklist of what isn't allowed.
    

#### References: [https://owasp.org/Top10/](https://owasp.org/Top10/)