# Users, Groups, and Permissions


### Goals:
   - Create two users that belong to two different groups (departments).
   - Assign unique permissions for each group.

---

<br>

### <mark>Step 1</mark>: Create Organizational Units and Users:

<br>

#### Organizational Unit:
 - Organizational Units are containers used to organize users, computers, groups and other objects into a logical structure.

<br>

***Organizational Units* for Computers, Groups, Servers, Service Accounts, and Users created:**
>
> ![](images/users-groups-permissions/OUs.png)

**Two users created inside of the *"Users" OU*:**
>
> ![](images/users-groups-permissions/users-created.png)
>
**Each user would belong to a separate department:**
 - IT
 - HR
---
<br>

### <mark>Step 2</mark>: Create Groups and shares:

<br>

**Global and Domain Local groups added to the *"Groups" OU*:**

> ![](images/users-groups-permissions/groups-created.png)
> 
> Permission assignment for these groups will be structured following the "***AGDLP model***":
>>
>> ![](images/users-groups-permissions/agdlp-model.png)
>> 
>> -  **Users** are put into **global groups**. These groups represent the department.
>> - Permissions are only assigned to the **domain local groups**, NOT **global groups** or **users** directly.
>> - A **global group** is added to a **domain local group** and only then do they inherit those permissions. This will allow for scalability as well as separation of roles from resource access.
>>
>>  This is **Microsoft's** recommended way to assign permissions in ***Active Directory***.

**Configured *IT* and *HR* folders as network shares:**

**Before share configuration:**
>
> ![](images/users-groups-permissions/before-share.png)
>
> ![](images/users-groups-permissions/share-folder.png)

**After share configuration:**
>
>![](images/users-groups-permissions/after-share.png)
---
<br>

### <mark>Step 3</mark>: Assign permissions for shares:

<br>

**Assigned share permissions to *Domain Local* groups:**

>![](images/users-groups-permissions/share-permissions1.png)
>
> ![](images/users-groups-permissions/add-user-permissions.png)
>
> ![](images/users-groups-permissions/domain-local-permissions.png)
>
>> ***Reminder:** Permissions here are ONLY assigned to Domain Local groups, NOT Global groups or users directly, as per the **AGDLP model**.*

**Administrator access was added to all shares to allow for Administrative troubleshooting:**

> ![](images/users-groups-permissions/admin-permission.png)

### Problem:

**Users where able to access shares belonging to other departments:**
>
> ***HR* user accessing the *IT* share:**
>
> ![](images/users-groups-permissions/hr-accessing-it.png)

### Why:

**The "*Everyone*" group had read permissions in both shares:**

> ![](images/users-groups-permissions/everyone-group.png)
   
### Solution: 

**Removed the "*Everyone*" group from *Share Permissions* in both shares:**

> ![](images/users-groups-permissions/removed-everyone-group.png)

### Result:

***HR* user attempting to access the *IT* share:**

> ![](images/users-groups-permissions/hr-attempt-it.png)
---
<br>

### <mark>Step 4</mark>: Map drives for each share:

<br>

#### Why:
   - **By mapping drives for each share, this will not only save users time but also ensure they do not mistakenly attempt to access other department's shares.**

**Creating mapped drives:**

> ![](images/users-groups-permissions/drive-mapping-policy.png)
> 
> ![](images/users-groups-permissions/drive-maps.png)
   
**Assigning a unique Drive Letter for each share:**

> ![](images/users-groups-permissions/drive-letter.png)

**Targeting the security group (Domain Local group) for each drive:**

> ![](images/users-groups-permissions/item-level-targeting.png)
> 
> ![](images/users-groups-permissions/targeting-domain-local.png)
> 
> - *This ensures that only members of the corresponding **Domain Local group** would receive the mapped drives.*

**Mapped drives created for both the *IT* and *HR* shares:**

> ![](images/users-groups-permissions/mapped-drives-created.png)

***HR* user accessing the *HR* share through the mapped drive:**

> ![](images/users-groups-permissions/mapped-drive-access.png)
   
