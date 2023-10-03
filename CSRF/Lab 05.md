# CSRF where token is tied to non-session cookie
- lab's email change functionality is vulnerable to CSRF
- uses tokens to try to prevent CSRF attacks, but they aren't fully integrated into the site's session handling system
