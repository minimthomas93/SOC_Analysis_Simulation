# SOC Analysis Simulation
Dive into the heat of a live phishing attack as it unfolds within the corporate network. In this high-pressure scenario, my role is to meticulously analyze and document each phase of the breach as it happens.

How to start?

In the dashboard, we can see multiple alerts. From the multiple alerts Filter the alerts with High severity.

<img width="949" height="533" alt="image" src="https://github.com/user-attachments/assets/2c3fe5f5-9d92-4804-86fa-4b754881dad5" />

## ALERT 1

From the Alert queue tab, I have opened the High severity alert and start analyse the problem.

<img width="955" height="523" alt="image" src="https://github.com/user-attachments/assets/e09b1507-6d1d-483d-a49e-0138d33b6fc5" />

Took ownership of the alert.

<img width="776" height="93" alt="image" src="https://github.com/user-attachments/assets/714323e2-aa92-43e9-a5f3-c5b5a52f4b9b" />

From the alert description I understood that,
- This is coming from firewall
- A user clicked and tried to access a malicious url, http://bit.ly/3sHkX3da12340, which was already added to the blacklist of the organisation's blacklist list,
- Firewall blocked the access to the url using the Firewall rule, Blocked Websites.

Investigate whether the url is malicious in Virus Total.

<img width="940" height="383" alt="image" src="https://github.com/user-attachments/assets/2257a95c-eb6f-447f-8226-5a6dd3bbbbbb" />

Let's try which user tried to access the url. Source IP is given as 10.20.2.17. From the Company information given in the documentation of TryHackMe, it clearly shows the user belongs this ip address.

<img width="947" height="426" alt="image" src="https://github.com/user-attachments/assets/1c5bc740-90c6-42cd-b5d9-42f0a50bd76a" />

When I investigated the users activities in Splunk, I found that the user received an email from the domain amazon.biz and their email content includes the malicious url. This is a phishing email which spoofs legitimate amazon domain. So user being tricked and clicked the url in the email body.

<img width="933" height="347" alt="image" src="https://github.com/user-attachments/assets/38a925f4-dc7a-4832-a7aa-2ec0e7a3f7da" />

So concluding, we found that user clicked the url and firewall blocked. So it is clearly shows this is A True Positive, but no more actions needed as it is already handled by Firewall. So let's start writing case report and close the alert.

<img width="763" height="316" alt="image" src="https://github.com/user-attachments/assets/5733a4f7-c089-4cd5-9db7-e195ad88379f" />

<img width="722" height="443" alt="image" src="https://github.com/user-attachments/assets/0b429243-7f62-48fe-82c6-317517c01368" />

----------------------------------------------------------------

## ALERT 2

Lets look on a medium severity alert.

<img width="746" height="356" alt="image" src="https://github.com/user-attachments/assets/52f8beb2-ffac-40c8-bf4d-f1ba2707a34c" />

Description tells that no-reply@m1crosoftsupport.co send an email to c.allen@thetrydaily.thm. Email body contains an login url,https://m1crosoftsupport.co/login.

TryDetectThis application in TryHackMe says that the domain is malicious.
<img width="671" height="433" alt="image" src="https://github.com/user-attachments/assets/ab09e2e7-5f94-4628-bb91-062548dbd9f4" />

When we read the email we can understand that sender is trying to spoof the legitimate microsoft domain, microsoftsupport.com, to m1crosoftsupport.co which is confusing to recipients. Also, email subject tells Unusual Sign-In Activity on Your Microsoft Account. So this is a phishing email tricks user to click the login url in the email body, asks for login credentials. User probably enter their microsoft account details and these credentials will be stealed by the attacker.


Investigate in the Splunk to know what happens after user receives this email.

Two events exists for this domain, one is the email log and other is the firewall log.

<img width="938" height="523" alt="image" src="https://github.com/user-attachments/assets/575a2552-7873-4fea-90a0-e3225ad2b4e8" />

Firewall log shows that Ip address 10.20.2.25 accessed https://m1crosoftsupport.co/login and Firewall allowed. Ip address 10.20.2.25 belongs to the same person, Charlotte Allen

Further investigated for any malicious connections happened for the Ip address 10.20.2.25 and for the user Charlotte Allen. But no logs for such activities. So it is clear that user clicked the url but not entered any credentials or send any requests to the malicious domain.

So we can close the report as true positive and with all details in case report.

<img width="695" height="416" alt="image" src="https://github.com/user-attachments/assets/a4b9690d-6e98-4a31-b37c-3bc665ae0954" />
<img width="695" height="212" alt="image" src="https://github.com/user-attachments/assets/b281e9b2-8370-44f2-9b87-0b1ee8f4b158" />



