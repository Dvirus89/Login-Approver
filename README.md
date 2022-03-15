# Login-Approver
# Approver in the middle
This document provides a high level overview of the solution


### Why do we need this solution?

## The Issue
Today it's very popular in many organizations to allow contractors / external users to have remote access to some of the organization internal services.
(with SSLVPN, Portals, etc.)
The issue / challange with this approach is that you can't assure if the external login is justified or pre-approved.


## The Solution
Add another step into the authentication process, in this step someone from the organization can approve in realtime the specific login for the contractors / external users.
![Alt text](Login_Approver_Flow.png?raw=true "Solution Diagram")

for every authenticaion, a dedicated GUID (OTP) will be generated and sent as a clickable link to the "approver".

## How to use it?
My assumption here is that you are already familiar with BIGIP APM.
As a begining we will need two VS, the 1st is for the real service (the one that you already have and want to add this extra security step), and the 2nd is for the solution - "approval page".
1. Create the "extra" VS for the solution, HTTP Profile is needed, no pool. assosicate this [iRule - Login-Approver-Page-iRule](Login-Approver-Page-iRule).
2. Assosicate this [iRule - Login-Approver-VPE-iRule](Login-Approver-VPE-iRule) to the 1st VS - the one with the real service (the one that you already have and want to add this extra security step)
3. Modify your APM policy using the VPE, should look similiar to this:
![Alt text](VPE-Flow-Login-Approver.png?raw=true "Solution Diagram")
for the last "approved" branch, add this [expression](approval_expression)
4. select your prefered method to send the approval message to the "approver", SMS/Email/Whatsapp/Teams/Slack/etc., and send a message that contains the link for the "2nd" VS - approval page with generated GUID (OTP), the link should be something like this: https://approvalpage.com/%{session.custom.user_guid}
5. TEST :)
