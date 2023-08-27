# Authentication vulnerabilities
- Authentication is the process of verifying the identity of a given user or client.
- it involves making sure that they really are who they claim to be
- websites are exposed to anyone who is connected to the internet by design. Therefore, robust authentication mechanisms are an integral aspect of effective web security
- There are three authentication factors into which different types of authentication can be categorized
  - Something you know, such as a password or the answer to a security question. These are sometimes referred to as "knowledge factors".
  - Something you have, that is, a physical object like a mobile phone or security token. These are sometimes referred to as "possession factors".
  - Something you are or do, for example, your biometrics or patterns of behavior. These are sometimes referred to as "inherence factors".
# What is the difference between authentication and authorization?
- Authentication is the process of verifying that a user really is who they claim to be
- authorization involves verifying whether a user is allowed to do something
- authentication determines whether someone attempting to access the site with the username Carlos123 really is the same person who created the account.
- Once Carlos123 is authenticated, his permissions determine whether or not he is authorized
# How do authentication vulnerabilities arise?
- most vulnerabilities in authentication mechanisms arise in one of two ways:
  - The authentication mechanisms are weak because they fail to adequately protect against brute-force attacks.
  - Logic flaws or poor coding in the implementation allow the authentication mechanisms to be bypassed entirely by an attacker. This is sometimes referred to as "broken authentication".
  -  logic flaws will simply cause the website to behave unexpectedly, which may or may not be a security issue
# What is the impact of vulnerable authentication?
- Once an attacker has either bypassed authentication or has brute-forced their way into another user's account, they have access to all the data and functionality that the compromised account has
- If they are able to compromise a high-privileged account, such as a system administrator, they could take full control over the entire application and potentially gain access to internal infrastructure
- Even compromising a low-privileged account might still grant an attacker access to data that they otherwise shouldn't have, such as commercially sensitive business information
- Even if the account does not have access to any sensitive data, it might still allow the attacker to access additional pages, which provide a further attack surface.
- certain high-severity attacks will not be possible from publicly accessible pages, but they may be possible from an internal page. 
# Vulnerabilities in password-based login
- 
