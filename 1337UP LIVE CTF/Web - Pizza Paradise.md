Web - Pizza Paradise

Summary
This challenge involved uncovering sensitive information via a misconfigured robots.txt, cracking a hashed password, and exploiting a Local File Inclusion (LFI) vulnerability to retrieve the flag.

Details
Initial Discovery
Checking the /robots.txt file revealed a hidden directory:

![test](Images/20241116210409.png)

The hidden directory led to a login page:

![test](Images/20241116210439.png)

Credential Retrieval
Examining the page source code revealed a file, auth.js, containing the username and a hashed password:

![test](Images/20241116210523.png)

The hash was successfully cracked using Crackstation, revealing the plaintext password:

![test](Images/20241116210634.png)

Using the recovered credentials, we logged in and gained access to a page with an image download feature:

![test](Images/20241116211115.png)

Local File Inclusion (LFI) Exploitation
Intercepting the download request with a proxy revealed that the file path was vulnerable to LFI:

![test](Images/20241116211412.png)

Initial attempts to read /etc/passwd were blocked:

![test](Images/20241116211453.png)


Adjusting the file path to include the base directory /assets/images bypassed the restriction:

![test](Images/20241116211559.png)

Flag Retrieval
Inspecting the source code of the current page revealed the flag:

![test](Images/20241116211740.png)
