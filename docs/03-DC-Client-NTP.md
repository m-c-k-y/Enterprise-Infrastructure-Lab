# Domain Controller + Windows Client time sync 

### Goal:
   - Establish synchronization of time between the Domain Controller and client machine.

---

**Purpose of time synchronization in a *Active Directory* environment:**

> Excessive time drift between a Domain Controller and client machine can cause:
>> - Kerberos failures
>> - External service failures
>> - Log timestamp inaccuracy
>
> The implementation of ***NTP*** is necessary for providing secure identity verification through ***Kerberos*** tickets as well as ensuring reliability of log data.
> 
> - **Kerberos** tickets are time-limited, meaning if there is a mismatch in time between a Domain Controller and client, the ticket may no longer be usable even if it should be.

**NTP flow plan:**

> <img src="../images/NTP/ntp-flow-plan.png" width="500">
> 
> - The **Domain Controller** synchronizes its time with an external **NTP** source. **Domain clients** then synchronize their time with the **Domain Controller**. This ensures that all client machines within the same domain will be synced to the same time source, greatly reducing the likelihood of time drift between machines.
---
<br>

### <mark>Step 1</mark>: Provide the Domain Controller with an external NTP source:

<br>

**External NTP server that will be used:**
>
> **time.windows.com**
>
> - This is Microsoft's public **NTP** server


**Configure the Domain Controller as a reliable time source:**
>
> <img src="../images/NTP/reliable-time-source.png" width="600">
>
> - ***manualpeerlist* =** External NTP server
> - ***syncfromflags:manual* =** Use manually specified peers
> - ***reliable:yes* =** Advertise this DC as a trusted time source to domain clients
> - ***update* =** Apply configuration changes
> - ***0x8* =** Synchronize time from this external NTP server

**Restart of the Windows Time service:**

> <img src="../images/NTP/restart-time-service.png" width="600">

**Force synchronization:**

> <img src="../images/NTP/force-sync.png" width="600">

**Verification of synchronization to external time source:**

> <img src="../images/NTP/time-source-verify.png" width="600">
---

<br>

### <mark>Step 2</mark>: Set the client's time source as the Domain Controller:

<br>

#### 🟥 Problem:

**The client was unable to synchronize to the Domain Controller because difference in time exceeded the Windows Time Service synchronization threshold:**
>
> <img src="../images/NTP/failed-sync.png" width="600">

### 🟩 Solution:

**1. Manually set the client's clock "close enough" to the Domain Controller's time so that it falls within the synchronization threshold.**
      
**2. Configured the client to use the Active Directory domain hierachy:**
>      
> <img src="../images/NTP/client-domain-hierachy.png" width="600">
>
**3. Established the client as a non-reliable time source:**
> 
> <img src="../images/NTP/client-non-reliable.png" width="600">
>
> - **Only the Domain Controller should be configured as reliable time source.**
>
**4. Restarted the Windows Time service:**
>
> <img src="../images/NTP/client-restart-time-service.png" width="600">
>
**5. Rediscovered time sources and synchronized:**
>
> <img src="../images/NTP/rediscover-sync.png" width="600">
>
**6. Verified client's time source:**
>
> <img src="../images/NTP/client-verify.png" width="600">


