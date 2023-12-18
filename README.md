# MultiCloud Federation — Microsoft Azure and Oracle Cloud Infrastructure using Single Sign-On for Teams Authentication

![picture with the words Cloud Project and icons for the services used](https://miro.medium.com/v2/resize:fit:720/format:webp/0*Kxr51YRtO7Ra_aIa.jpg)

In this project based on a real scenario, I was responsible for designing and implementing a MultiCloud architecture, 
by performing MultiCloud federation of users and groups between Azure Active Directory and OCI IAM.

There were multiple users who needed to authenticate to both Microsoft Azure and Oracle Cloud Infrastructure using 
the same login and password (single sign-on) to access resources.

Once the integration was implemented in Oracle Cloud Infrastructure, the mapping of users and groups was 
directly related to users and groups in Microsoft Azure through the federation service.

![picture of a graph showing the solution architecture](https://miro.medium.com/v2/resize:fit:720/format:webp/0*YyjJH00Z3bJKBfeN.jpg)

The project was split in two parts. On the first part I realized the federation between the two providers and created a Identity Provider on Oracle,
while on the second part I synched the users, cloning the users on Azure Active Directory to Oracle. Both parts where made trough internal 
apps created on the providers themselves.

On Microsoft Azure I created an enterprise app called “AZ-OCI-Fed”, which was responsible for the certification exchange between the two providers, 
attributing the app to a group called “Administrators” (which existed on Oracle too). After that I started the Single Sign-On (SSO) Process,
using the SAML protocol. I uploaded the Oracle XML Certificate to this app, filling the required info. When planning the project, it was decided 
to make the federation using email as the principal parameter, so I’ve adjusted the “Required Claim” panel, attributing the “user.mail” parameter. 
After that I downloaded the federation metadata XML from Azure.

On Oracle Cloud I began the process to create a new Identity Provider, called “Azure AD”, using the SAML protocol again. The process itself is very
straightforward, you just have to upload the federation metadata from Azure on the second step. After that you map the user identity using email as 
the Requested Name ID format. Finishing the process, I edited the default identify provider policy and added Azure AD as an option.

With federation set, I went to part 2 of the project, which was to clone the users from Azure to Oracle. I created a new app on Oracle, using the 
confidential format, and configured it as a client, atributing to it the roles of “User Administrator” and “Identity Policy Administrator”,
required to create new users on the Oracle Cloud. I copied the Client ID and Client Secrets and converted them both to Base64. 
With this data in hands, I returned to Azure and began the Provisioning Process on the same app used for the federation. I just had to
insert the tenancy URL and the token (the Client ID and Secret in Base64) to stabilish the conection. After starting the provisioning, 
the users on the Administrators group on Azure where created on Oracle, under the same group name and with admin privileges. 
I tested the On-Demand Provisioning too, with sucess.

![The Azure provisioning dashboard opened on a browser window](https://miro.medium.com/v2/resize:fit:720/format:webp/0*f4Tv6Z7bp7-ksZik.png)

![Oracle cloud dashboard opened on a browser window showing the user created trough the process](https://miro.medium.com/v2/resize:fit:720/format:webp/0*YEZOjkVr5OMTyKBu.png)

Initially I had problems on the provisioning process. After some days and after redoing the project from zero, I’ve finally found that the problem 
was on one of my Azure account atributes. My Microsoft account is 20 years old, since the Hotmail days, and right around 2013 
I created an alias for my account using a Outlook address. Because of that, my account email was listed on the “other email” section instead of the main “email”, 
which was the required atribute on the federation process. After correcting the field, the provisioning was successful, allowing me to login to 
my Oracle Cloud using my Azure account.

Altought I had problems on the way, I’m glad I had to redo the project several times, this helped me to fully understand how federation works between
the two providers. Repetition is essential to fully grasp all the knowledge you gather along the journey.


