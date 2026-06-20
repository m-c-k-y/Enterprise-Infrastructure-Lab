# Users, Groups, and Permissions


### Goals:
   - Create two users that belong to two different groups (departments).
   - Assign unique permissions for each group.

---

<br>

### <mark>Step 1</mark>: Create Organizational Units and Users:

<br>

**Organizational Units for Computers, Groups, Servers, Service Accounts, and Users created:**
>
> <img src="../images/users-groups-permissions/OUs.png" width="500">
>
> - **Organizational Units** are containers used to organize users, computers, groups and other objects into a logical structure.

**Two users created inside of the "Users" OU:**
>
> <img src="../images/users-groups-permissions/users-created.png" width="500">
>
**Each user would belong to a separate department:**
 - IT
 - HR
---
<br>

### <mark>Step 2</mark>: Create Groups and shares:

<br>

**Global and Domain Local groups added to the "Groups" OU:**

> <img src="../images/users-groups-permissions/groups-created.png" width="500">
> 
> Permission assignment for these groups will be structured following the **AGDLP model**:
>>
>> <img src="../images/users-groups-permissions/agdlp-model.png" width="600">
>> 
>> -  **Users** are put into **global groups**. These groups represent the department.
>> - Permissions are only assigned to the **domain local groups**, NOT **global groups** or **users** directly.
>> - A **global group** is added to a **domain local group** and only then do they inherit those permissions. This will allow for scalability as well as separation of roles from resource access.
>>
>>  This is **Microsoft's** recommended way to assign permissions in Active Directory.

**Configured *IT* and *HR* folders as network shares:**

**Before share configuration:**
>
> <img src="../images/users-groups-permissions/before-share.png" width="500">
>
> <img src="../images/users-groups-permissions/share-folder.png" width="500">

**After share configuration:**
>
><img src="../images/users-groups-permissions/after-share.png" width="500">
---
<br>

### <mark>Step 3</mark>: Assign permissions for shares:

<br>

**Assigned share permissions to *Domain Local* groups:**

> <img src="../images/users-groups-permissions/share-permissions1.png" width="500">
>
> <img src="../images/users-groups-permissions/add-user-permissions.png" width="500">
>
> <img src="../images/users-groups-permissions/domain-local-permissions.png" width="500">
>
>> ***Reminder:** Permissions here are ONLY assigned to Domain Local groups, NOT Global groups or users directly, as per the **AGDLP model**.*

**Administrator access was added to all shares to allow for Administrative troubleshooting:**

> <img src="../images/users-groups-permissions/admin-permission.png" width="500">

#### 🟥 Problem:

**Users where able to access shares belonging to other departments:**
>
> ***HR* user accessing the *IT* share:**
>
> <img src="../images/users-groups-permissions/hr-accessing-it.png" width="500">

#### 🟨 Why:

**The "Everyone" group had read permissions in both shares:**

> <img src="../images/users-groups-permissions/everyone-group.png" width="500">
   
#### 🟩 Solution: 

**Removed the "Everyone" group from *Share Permissions* in both shares:**

> <img src="../images/users-groups-permissions/removed-everyone-group.png" width="500">

### 🏆 Result:

***HR* user attempting to access the *IT* share:**

> <img src="../images/users-groups-permissions/hr-attempt-it.png" width="500">
---
<br>

### <mark>Step 4</mark>: Map drives for each share:

<br>

#### Why:
   - **By mapping drives for each share, this will not only save users time but also ensure they do not mistakenly attempt to access other department's shares.**

**Creating mapped drives:**

> <img src="../images/users-groups-permissions/drive-mapping-policy.png" width="500">
> 
> <img src="../images/users-groups-permissions/drive-maps.png" width="500">
   
**Assigning a unique Drive Letter for each share:**

> <img src="../images/users-groups-permissions/drive-letter.png" width="500">

**Targeting the security group (Domain Local group) for each drive:**

> <img src="../images/users-groups-permissions/item-level-targeting.png" width="500">
> 
> <img src="../images/users-groups-permissions/targeting-domain-local.png" width="500">
> 
> - This ensures that only members of the corresponding **Domain Local group** would receive the mapped drives.

**Mapped drives created for both the *IT* and *HR* shares:**

> <img src="../images/users-groups-permissions/mapped-drives-created.png" width="500">

***HR* user accessing the *HR* share through the mapped drive:**

> <img src="../images/users-groups-permissions/mapped-drive-access.png" width="500">
   
