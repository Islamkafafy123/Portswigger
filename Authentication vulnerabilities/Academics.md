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
- For websites that adopt a password-based login process, users either register for an account themselves or they are assigned an account by an administrator
- This account is associated with a unique username and a secret password, which the user enters in a login form to authenticate themselves.
- he mere fact that they know the secret password is taken as sufficient proof of the user's identity. Consequently, the security of the website would be compromised if an attacker is able to either obtain or guess the login credentials of another user.
# Brute-force attacks
- brute-force attack is when an attacker uses a system of trial and error in an attempt to guess valid user credentials.
- Brute-forcing is not always just a case of making completely random guesses at usernames and passwords
- By also using basic logic or publicly available knowledge, attackers can fine-tune brute-force attacks to make much more educated guesses
- while attempting to brute-force a login page, you should pay particular attention to any differences in:
  - Status codes: During a brute-force attack, the returned HTTP status code is likely to be the same for the vast majority of guesses because most of them will be wrong. If a guess returns a different status code, this is a strong indication that the username was correct. It is best practice for websites to always return the same status code regardless of the outcome, but this practice is not always followed.
    
  - Error messages: Sometimes the returned error message is different depending on whether both the username AND password are incorrect or only the password was incorrect. It is best practice for websites to use identical, generic messages in both cases, but small typing errors sometimes creep in. Just one character out of place makes the two messages distinct, even in cases where the character is not visible on the rendered page.
 
  - Response times: If most of the requests were handled with a similar response time, any that deviate from this suggest that something different was happening behind the scenes. This is another indication that the guessed username might be correct. For example, a website might only check whether the password is correct if the username is valid. This extra step might cause a slight increase in the response time. This may be subtle, but an attacker can make this delay more obvious by entering an excessively long password that the website takes noticeably longer to handle.

