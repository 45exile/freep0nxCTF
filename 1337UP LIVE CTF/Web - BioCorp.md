**Web - BioCorp**

Summary
During this challenge, a .zip file containing the website's source code was provided. Through analysis, we identified vulnerabilities that allowed access to sensitive functionalities and ultimately led to flag extraction.

Details
Initial Discovery
The source code revealed a panel.php file requiring a specific HTTP header for access:

![test](Images/20241116212236.png)



![test](Images/20241116222648.png)

Once authenticated, a "Control Panel" option becomes available, redirecting to the following page:

![test](Images/20241116222746.png)

Source Code Analysis
Inspecting the page source highlighted additional elements, prompting further investigation:

![test](Images/20241116222820.png)

XXE Exploitation
The identified patterns suggested a potential XML External Entity (XXE) vulnerability. Exploitation was attempted as follows:


![test](Images/20241116223440.png)

Outcome
The attack successfully allowed access to the target resource, and the flag was retrieved:


![test](Images/20241116223522.png)

