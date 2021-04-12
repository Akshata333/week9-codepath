Project 8 - Pentesting Live Targets

Time spent: 6.5 hours spent in total

Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

## Blue
Vulnerability #1: SQL Injection

The hint provided to us was with a blind ejection example to attempt 'OR SLEEP(5)=0--' at three separate sites (I increased to 10 for effect). The first challenge was to see that it could be used. I was on the Find a vendor page already, and it continued to take a while to get the injector correctly. Nothing occurred when it was used on the red and green pages. 


![1)SQL](https://user-images.githubusercontent.com/55360840/114341501-f0263e00-9b27-11eb-9e9f-a3a15c1d6189.gif)


![2)SQL blue site blind works](https://user-images.githubusercontent.com/55360840/114341521-fc120000-9b27-11eb-815e-70908033ee15.gif)


Vulnerability #2: Session Hijacking/Fixation

The blue site is vulnerable to session fixation and hijacking attacks. Using the supplied public/hacktools/change_session_id.php tool, a session created in one browser was successfully used in a different.

![2 vuln](https://user-images.githubusercontent.com/55360840/114341743-75a9ee00-9b28-11eb-83a6-01d4c935bfc7.gif)


## Green
Vulnerability #1: Username Enumeration
The green site's login mechanism leaks information which indicates if a username exists in the database. If a username is not found in the database, the following HTML is returned:

<span class="failed">Log in was unsuccessful.</span>
If a username is found in the database, an invalid login attempt returns the following HTML:

<span class="failure">Log in was unsuccessful.</span>


![green1](https://user-images.githubusercontent.com/55360840/114341879-bf92d400-9b28-11eb-8cd0-34af32c75383.gif)


Vulnerability #2: Cross-site Scripting

The green site's contact form (found at /green/public/contact.php) allows arbitrary JavaScript to be submitted and later executed by authenticated users in the administration interface (found at /green/public/staff/feedback/index.php).

For example, submitting the following JavaScript in the "Feedback" field of the contact form will lead to a JavaScript alert appearing when feedback is viewed by an authenticated user.

<script>alert('zmh68 - XSS');</script>

![2 2vuln](https://user-images.githubusercontent.com/55360840/114342069-231d0180-9b29-11eb-9cc1-989ac1647444.gif)


## Red

Vulnerability #1:IDOR

After clicking on the name of salespeople and seeing the profiles in the url, I was hoping that on other profiles something cool would appear or the numbers would run. 

![red1](https://user-images.githubusercontent.com/55360840/114342194-6aa38d80-9b29-11eb-835c-c3ac27d74ae6.gif)


Vulnerability #2: Cross-Site Request Forgery (CSRF)

For this site it did not require a valid CSRF token to be submitted when updating a salesperson's information via the URL /red/public/staff/salespeople/edit.php?id=, where id is the salesperson's ID. This enables covert editing of data through hiding requests to this endpoint in other pages an authenticated user might visit.

For eg, if an authenticated user loads a page containing the following HTML sample, the seller's phone number with ID 5 is changed without the user's visibility of the operation.

![red2](https://user-images.githubusercontent.com/55360840/114342337-af2f2900-9b29-11eb-8c31-e9b4441b8edd.gif)

## Notes
No major challenges were encountered while completing this assignment.It was fun performing these vulnerabilities. 



