## Injection
Web applications commonly invoke shell commands via system() or execve()
```
system("TZ=MST date")
execve("/bin/sh", ["sh", "-c", "TZ=MST date"], {...})
execve("/usr/bin/date", ["date"], {"TZ": "MST"})
```
This is great for web applications. Furthermore this is great for hackers too! The backtick operator allows the program to execute commands in those commands first to return the value and to use it as the input. E.g.
```
system("TZ=`whoami` date)
execve("/bin/sh", ["sh", "-c", "TZ=`whoami` date"], {...})
execve("/usr/bin/whoami", ["whoami"], {...})
execve("/usr/bin/date", ["date"], {"TZ": "root"})

This gives root as the timezone
```

### HTML Injection
A website may ask the user for their username. If the user is in any kind of html response then the injection will happen. Therefore websites shouldn't allow certain characters for their usernames.
```
html_response("<p>Hello, User!</p>")
```
With injection:
```
html_response("<p>Hello, <script>alert(1)</script>!</p>")
```
This example isn't really dangerous because the only thing that is being hacked is the user client. Therefore no harm is done. But the reason why certain characters should be disallowed and usernames should be checked for security reasons are that this might lead to Cross Site Scripting [XSS](https://owasp.org/www-community/attacks/xss/). What does XSS do? Well e.g. if the user with a username that runs js code posts a comment on a website where everybody can see his username. This specific username will run the js code on the other clients and might cause trouble.

### SQL Injection
E.g. a database stores users with passwords in the users table.
```
execute ("Select * FROM users WHERE
	username = 'user' AND
	password = 'password123'")
```
Then this will happen if a user tries to log on to this system and the credentials are checked for any valid user.
What if the user is a hacker and tries to inject malicious input data?
Then something like this might happen:
```
execute ("Select * FROM users WHERE
	username = 'user' AND
	password = '' OR 1=1 --'")
```
The hacker inputs the password as: 
```
' OR 1=1 --
```
This does following in the sql query:
```
username = 'user'
AND
password = '' <-- empty
OR
1=1 -- '
```
because 1=1 is always true, the whole condition is true and the hacker gets access to the system where he shouldn't. The -- comments out the rest in the SQL statement and therefore cancels out the last original tick.