<img width="1459" height="816" alt="image" src="https://github.com/user-attachments/assets/7a336cfe-874d-475e-bdd6-6eea5632f1dd" />

<h1>Active Directory Homelab</h1>
Active Directory is Microsoft's centralized directory system used for managing users, systems and other resources within a network. It organizes these objects in a domain based structure where it can enforce authentication, authorization and group policies.
<h2>Description</h2>
In this lab, we will continue our Active Directory homelab by exploring some of the features like groups, OUs and GPOs. Groups are an object on AD that is used to assign permissions and access. Organizational Units are the smallest administrative unit in Active Directory and used to assign administrative prilviledges. We will even create a powershell script to automate the creation of a user and placement into an OU.
<br />


<h2>Technologies Used</h2>

- <b>VirtualBox</b> 
- <b>Active Directory Domain Services</b>
- <b>Windows 11</b>
- <b>Windows Server 2022</b> 

<h2>Lab walk-through:</h2>
Creating Organizational Units 

1. Boot up your DC and navigate to Server Manager > Tools > Active Directory Users and Computers. We will simulate an enterprise enviroment by creating an OU for each department. We will create 3 OUs called "IT", "Management" and "Engineering". To do this right click on the page > Select "New" > Click Organizational Unit. Enter the name for the OU, click Enter and it will show up on the left menu. Repeat this until you have made 3 OUs.
 <br/>
<img width="756" height="528" alt="image" src="https://github.com/user-attachments/assets/490cf831-91e0-49b1-8077-41301f2fe811" />
<br />
<br />

2. Now we will create users to place into the OUs. We should already have 3 user from the previous lab walkthrough where we set up Active Directory. We will create an additional 3 users for a total of 6. Right-click on Active Directory Users and Computers Page > Click New > Click User. Create 3 new users with any name that you want. Once we have 6 users on total we will place 2 in each OU. You can do this by dragging the user and dropping over the desired OU. 
<br/>
<img width="752" height="536" alt="image" src="https://github.com/user-attachments/assets/73158047-606a-4a8a-a2c8-0e02b1486654" />
<br />
<br /> 

Enforcing GPOs

1. We have created OUs that represent a deparment in a company. First we will enforce a GPO on the whole domain. A typical GPO you may see is a password policy and an account lockout policy. A user must create a passwords that meet certain requirements for better security. A user will also only have a certain number of attempts before their account is locked out for a duration of time. Theses policies are used to prevent unauthorized access and bruteforce attacks. We will enforce them on the whole domain. On Server Manager navigate to Tools > Group Policy Management > Click on your domain name > Right-click
 on "Default Domain Policy" > Click Edit.
<br/>
<img width="594" height="419" alt="image" src="https://github.com/user-attachments/assets/3dd38cdd-a161-493a-8e59-65aa2476c42f" />
<br />
<br />

2. On left menu navigate to Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy. We will set "Account lockout duration" and "Reset account lockout counter after" to 5 minutes. Set Account lockout threshold to 3 attempts. After policy is set, open command prompt and run "gpudate /force"  to update policy. In a real world environment we would set these durations to be longer. In the account policies section we can also set a password policy which might include rules such as password age, length, complexity, etc based on company policy.
<br/>
<img width="620" height="446" alt="image" src="https://github.com/user-attachments/assets/64b539a6-ec9f-4f65-8006-e1f791c7ceff" />
<img width="620" height="446" alt="image" src="https://github.com/user-attachments/assets/7a173a15-62c8-4071-b7b5-6e4cfd6dac74" />

<br />
<br />

3. Now that we have this policy set we can test it by going to our Windows 11 machine that is connected to the domain. Login and open command prompt to run "gpudate /force" to update policy. Log out and try to sign in but type in your password wrong three times and your account should be locked out.
<br/>
<img width="807" height="613" alt="image" src="https://github.com/user-attachments/assets/bd87eb41-29ab-484b-a100-1a55c3090475" />
<br />
<br />

4. If you wanted to unlcok the account you can log back on to the DC. Navigate to Server Manager > Active Directory Users and Computers > Right-click on user with account locked out > Click Properties > Account > Click on box that says "Unlock Account"
<br/>
<img width="407" height="538" alt="image" src="https://github.com/user-attachments/assets/5e01ec1e-e8a5-4725-8c2c-7296106a029b" />
<br />
<br />

5. If a user forgets a their password, you can reset it by navigating to Active Directory Users and Computer > Right Click on User's Name > Select "Reset Password". Enter a temporary password but check on the box that says "User must change password at next logon" so they can set their new password.
<br/>
<img width="752" height="529" alt="image" src="https://github.com/user-attachments/assets/86a6c0f9-ce6f-4611-a67e-511504587cc8" />
<br />
<br />

6. Let's say that you want to apply a policy to a certain department. We will assign a policy to the Engineering Department that disables their used of the control panel so
they can't change any system configurations on the computer. First on the DC, naviagte to Server Manager > Tools > Group Policy Management > Right Click "Group Policy Object" > Click New > Name it "Disbale Control Panel".
<br/>
<img width="598" height="417" alt="image" src="https://github.com/user-attachments/assets/e4610dc3-c1c3-48b2-ac59-73a37269d2ff" />
<br />
<br />

7. Right click on GPO > click Edit. Navigate to User Configuration > Polices > Administrative Templates > Control Panel > Prohibit Access to Control Panel and PC settings. Click on it and "Enable" the policy.
<br />
<img width="623" height="447" alt="image" src="https://github.com/user-attachments/assets/f97c8434-341a-4f89-acac-c9af75ee63da" />
<br />
<br />

8. On the main menu of Group Policy Manangemnt, Drag the "Disable Control Panel" and drag it into the Engineering OU. Open command prompt and run  "gpudate /force" to update policy.
<br />
<img width="593" height="421" alt="image" src="https://github.com/user-attachments/assets/60671d79-0f8a-4dff-8f64-fdd5f1881b7f" />
<br />
<br />

9. Open Windows 11 VM and sign in to user in Engineering Department. Open command prompt and run "gpudate /force" to update policy. Now, if you try to open Control Panel on an Engineering account you will be denied.
<br />
<img width="808" height="604" alt="image" src="https://github.com/user-attachments/assets/57d9c428-3b0c-44f3-ab41-2a3fee3dc2bb" />
<br />
<br />
