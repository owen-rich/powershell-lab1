# Windows Server: PowerShell Active Directory Management

## Lab Mission
Practice PowerShell Active Directory commands and manage objects using the bulk action method.

## Resources
**Environment and Tools:**
- VirtualBox:
  - Windows Server 2016 (Server1)
  - Windows 10 Client
- Extra files:
  - Create-Users.ps1
  - Userlist.csv

---

## Lab Task 1: Manage Active Directory Using PowerShell
On **Server1**, use PowerShell commands to create and manage users for the domain.

1. Create an organizational unit (OU) named **IT** using the command:
   `New-ADOrganizationalUnit IT`

2. Create a user account named **Josep** in the IT OU by using the command:
   `New-ADUser –Name "Jim" –SamAccountName "Jim" –path "OU=IT, DC=Cyber, DC=local"`

3. Set a password for the new user with the command:
   `Set-ADAccountPassword Jim`
   *Hint: When asked for the current password, leave it empty and press Enter.*

4. Enable the **Josep** account by using the command:
   `Enable-ADAccount Jim`

5. Create a global security group named **IT-GRP** in the IT OU using the command:
   `New-ADGroup "IT-GRP" –path "ou=IT, dc=Cyber, dc=local" –GroupScope Global –GroupCategory Security`

6. Add **Josep** to the **IT-GRP** group by using the command:
   `Add-ADGroupMember "IT-GRP" –Members Jim`

7. Add a **Department** parameter to the **Josep** user by running:
   `Set-ADUser Jim –Department "IT"`

8. Verify the changes made to the **Josep** user account by running:
   `Get-ADUser Jim –Properties department`
   *Note: Use the `-Properties` flag with an asterisk (`*`) to see all user properties:*
   `Get-ADUser Jim –Properties *`

9. Verify the members of the **IT-GRP** group using:
   `Get-ADGroupMember IT-GRP`

10. Log in to the **Jim** user account via the Windows 10 client machine.

---

## Lab Task 2: Work with PowerShell Scripts
Use a PowerShell script to add domain users via a `.csv` file.

- **Create-Users.ps1**: A PowerShell script that creates users automatically from a list.
- **Userlist.csv**: A list of users and additional information.

*Note: If the file extensions do not appear, select the following option on the View tab:*

1. On **Server1**, use the SIP to install Guest Additions to drag and drop the extra files into **Server1**.

2. Move the files to the **C:\** drive.

3. Edit **Create-Users.ps1** with PowerShell ISE by right-clicking it and choosing **Edit**.

4. Ensure the path of **Userlist.csv** is correct (leave it open).

5. Edit the **CSV** file with Excel or Notepad and add five users of your choice to the IT department. Then save the changes.
   *Note: Avoid creating users with the same first names or `SamAccountName`, as it may cause errors. Example:*
   `John,Snow,cyber.local,John,"OU=Sales,DC=Cyber,DC=local",(Secure Password),Sales`

6. Run the script by clicking the green play icon or pressing **F5** on your keyboard.
   *Note: Ensure PowerShell is in the correct file path for the program to execute.*

7. Verify the users were created by opening **Active Directory Users and Computers**.

---

## Lab Task 3: Manage User Accounts Using Bulk Actions
Practice using commands to manage multiple users collectively.

1. Create two new users, **Test1** and **Test2**, in the IT OU via PowerShell with no additional parameters:
   `New-ADUser –Name Test1 –SamAccountName Test1 –path "OU=IT, DC=Cyber, DC=local"`
   `New-ADUser –Name Test2 –SamAccountName Test2 –path "OU=IT, DC=Cyber, DC=local"`

2. Show all users in the IT OU by using:
   `Get-ADUser –Filter * -SearchBase "OU=IT, DC=Cyber, DC=local" –SearchScope subtree`

3. Show all users from the domain where the **Department** parameter equals **IT**. We will not see the two new users.
   `Get-ADUser –Filter "department –eq 'IT'"`
   *Or*
   `Get-ADUser –Filter {department –like "IT"}`

4. To get all users from the IT OU for which the **Department** parameter is empty and set it to **IT** via a single command, run:
   `Get-ADUser –Filter {department –notlike "*"} –SearchBase "ou=IT, dc=Cyber, dc=local" | Set-ADUser –Department "IT"`

5. Repeat Step 3. You should now see the new users.
