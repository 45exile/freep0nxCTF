**Web - Cat Club**

Summary
This challenge provided a .zip file containing the website's source code. Analyzing the code revealed vulnerabilities that allowed server-side template injection (SSTI) exploitation and subsequent flag retrieval.

Details
Initial Discovery
The site offers a login feature and a gallery displaying cat images. The username is reflected on the gallery page, suggesting a potential SSTI vulnerability:

![test](Images/20241116214032.png)
However, the registration process only accepts usernames containing alphanumeric characters. The site uses JSON Web Tokens (JWT), and with the source code, it was possible to forge a custom token.

Endpoint Analysis
An interesting endpoint was identified in the source code:

![test](Images/20241116214439.png) 

and

![test](Images/20241116214456.png)

Using the information gathered, we forged a JWT with the HS256 algorithm, injecting the payload #{9*9} in the username field to test SSTI:

![test](Images/20241116214654.png)

The test was successful:

![test](Images/20241116214859.png)

Exploitation
The source code revealed the use of PugJS as the templating engine. Using a payload from HackTricks, we attempted further exploitation. However, there was no visible response on the web page.

To confirm execution on the server side, we tested the id command:

![test](Images/20241116215217.png)

The returned field was empty:

![test](Images/20241116215239.png)

We then used curl to confirm server-side execution by sending an external response:

![test](Images/20241116215537.png)

The server successfully executed the command, and we retrieved the result:

![test](Images/20241116215517.png)

Flag Retrieval
With confirmed code execution, retrieving the flag was straightforward:

![test](Images/20241116215724.png)
