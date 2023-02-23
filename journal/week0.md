# Week 0 — Billing and Architecture
## Homework

### Overview 
Cruddur is an ephemeral micro-blogging platform with time-limited content or expiring posts. Cruddur is a platform that will uphold optimal privacy for its user, and cruddur is for anyone that doesn't want to maintain a permanent presence online. Cruddur is a community-focused platform.

### Conceptual Diagram on a Napkin for Cruddur
Conceptual design is simply trying to model the solution or our proposed system as a pure theoretical construct without regard for the practicalisation of trying to implement this on some actual pieces of hardware.

![20230222_144245](https://user-images.githubusercontent.com/99274632/220733248-1ab26e70-b8e8-4fd6-a1bb-2e87aff32d18.jpg)

### Logical Architectual Diagram for Cruddur.
Based on the Requirement gathering meeting, the logical design required for cruddur is to explain in more detail the necessary services to get started with cruddur.

![lucid](https://user-images.githubusercontent.com/99274632/220733204-dbbc009d-e3a6-4cf2-b9b3-27f7f5809f0b.PNG)

[lucidchart logical diagram link](https://lucid.app/lucidchart/ae479679-fbaa-4c1a-b501-cbcf83c8b668/edit?viewport_loc=-32%2C-69%2C2216%2C997%2C0_0&invitationId=inv_ec16a08d-241f-4716-a2d2-e092a17be0e3)

### Installed AWS cli and configure aws-cli auto-prompt with Gitpod.
I had to sort permission error I got while trying to push to github
![permissionssorted](https://user-images.githubusercontent.com/99274632/220951097-ebbae1e1-752e-4cfc-819c-fd12fcda0cd3.PNG)
<br>
This is where I added a code to enable gitpod install aws cli automatically each time I launch gitpod from github.

![prompt](https://user-images.githubusercontent.com/99274632/220951521-9d53903e-a83d-46a7-b20f-0822bcbf9ad4.PNG)


### Created BUDGET and BILLING ALARM VIA AWS CLI using json

This is when I tried to connfirm the env variables I set
![awsidentity](https://user-images.githubusercontent.com/99274632/220949659-71026573-825e-42d4-a839-0cf7cba634aa.PNG)

I previously created billing and budget manually but decided to use code to create via aws cli
![json](https://user-images.githubusercontent.com/99274632/220952329-f6a49156-bcff-4cd1-b595-2c939701e93d.PNG)

Billing alarm created
![biling](https://user-images.githubusercontent.com/99274632/220949615-26a2a837-2a9b-4e8c-acc2-6ed0797d1c2a.PNG)

Amazon sns created
![sns](https://user-images.githubusercontent.com/99274632/220953696-b341b00e-8af3-4c6f-9049-a3df845b8c8d.PNG)

Budget created
![budjet](https://user-images.githubusercontent.com/99274632/220949881-83d3a0b8-ae5a-42eb-abc7-a8d1b39b2311.PNG)

This is the subscription confirmation
![subcon](https://user-images.githubusercontent.com/99274632/220954225-f318cc34-9797-4d3c-b095-88b5d6c0c453.PNG)

Pushed files created to github
![files](https://user-images.githubusercontent.com/99274632/220954687-608053bf-806b-41b0-87de-9768d5cb2923.PNG)

This are some of the commands I used
![com](https://user-images.githubusercontent.com/99274632/220954667-f8b219e4-dd58-43d6-b51d-156c50608455.PNG)
