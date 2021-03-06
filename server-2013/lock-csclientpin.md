﻿---
title: Lock-CsClientPin
TOCTitle: Lock-CsClientPin
ms:assetid: 81a9895f-96e3-43c9-9dac-8129358e446a
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Gg398650(v=OCS.15)
ms:contentKeyID: 48184664
ms.date: 07/23/2014
mtps_version: v=OCS.15
---

<div data-xmlns="http://www.w3.org/1999/xhtml">

<div class="topic" data-xmlns="http://www.w3.org/1999/xhtml" data-msxsl="urn:schemas-microsoft-com:xslt" data-cs="http://msdn.microsoft.com/en-us/">

<div data-asp="http://msdn2.microsoft.com/asp">

# Lock-CsClientPin

</div>

<div id="mainSection">

<div id="mainBody">

<span> </span>

_**Topic Last Modified:** 2013-03-06_

Enables an administrator to prevent a user from using personal identification number (PIN) authentication. This cmdlet was introduced in Lync Server 2010.

<div>

## Syntax

    Lock-CsClientPin -Identity <UserIdParameter> [-Confirm [<SwitchParameter>]] [-Force <SwitchParameter>] [-WhatIf [<SwitchParameter>]]

</div>

<div>

## Examples

<div>

## EXAMPLE 1

In Example 1, the **Lock-CsClientPin** cmdlet is used to lock the PIN belonging to the user kenmyer@litwareinc.com.

    Lock-CsClientPin -Identity "kenmyer@litwareinc.com"

</div>

<div>

## EXAMPLE 2

Example 2 uses the **Lock-CsClientPin** cmdlet to lock the PINs for all users who currently have a PIN assigned to them. This is done by using the **Get-CsUser** cmdlet to return a collection of all the users who have been enabled for Lync Server. That collection is piped to the **Get-CsClientPinInfo** cmdlet, which is used, in conjunction with the **Where-Object** cmdlet, to return a collection of users where the IsPinSet property is equal to True. That filtered collection is then piped to the **Lock-CsClientPin** cmdlet, which locks the PIN for each user in the collection.

    Get-CsUser | Get-CsClientPinInfo | Where-Object {$_.IsPinSet -eq $True} | Lock-CsClientPin

</div>

<div>

## EXAMPLE 3

In Example 3, the **Lock-CsClientPin** cmdlet is used to lock the PINs for all the users who have expired PINs. To carry out this task, the **Get-CsUser** cmdlet is first called in order to return a collection of all the users who have been enabled for Lync Server. This collection is then piped to the **Get-CsClientPinInfo** cmdlet, which is used, in conjunction with the **Where-Object** cmdlet, to limit the collection to users who have expired PINs. To determine which users have expired PINs, the **Where-Object** cmdlet checks for accounts where the PinExpirationTime property (which indicates the date that the PIN number expires) is less than the current date. (The current date is retrieved using the **Get-Date** cmdlet.) If the expiration date (for example, September 1, 2010) is less than the current date (for example, September 2, 2010), that means that the PIN has expired. After that, the **Lock-CsClientPin** cmdlet is used to lock each of the expired PINs.

    Get-CsUser | Get-CsClientPinInfo | Where-Object {$_.PinExpirationTime -lt (Get-Date)} | Lock-CsClientPin

</div>

</div>

<div>

## Detailed Description

Lync Server enables users to connect to the system, or to join public switched telephone network (PSTN) conferences by using a telephone. Typically, logging on to the system or joining a conference requires the user to enter a user name or password. However, entering a user name and password can be a problem if you are using a phone that does not have an alphanumeric keypad. Because of that, Lync Server enables you to supply users with numeric-only PINs; when prompted, users can then log on to the system or join a conference by entering the PIN instead of a user name and password.

As a security measure, Lync Server enables you to lock a user’s PIN. When a PIN is locked, the user will no longer be able to use that PIN to access the system or join a conference. (However, that user will still be able to access the system and join conferences by using an application such as Lync 2013 and by supplying a user name and password.) After a PIN has been locked, the only way to restore the PIN (and the user's access privileges) is for an administrator to unlock the PIN. This can be done by using the **Unlock-CsClientPin** cmdlet.

The **Lock-CsClientPin** cmdlet enables administrators to temporarily disable the ability of a user to access the system by using PIN authentication. PINs can also be locked by the system: if a user repeatedly fails to log on to the system, their PIN will automatically be locked and, again, will require unlocking by an administrator.

Note that, by default, the firewall exceptions for SQL Server Express are not enabled when you install the Standard Edition of Lync Server. That means that you will not be able to run the **Lock-CsClientPin** cmdlet from a remote instance of Windows PowerShell; that’s because your command will not be able to traverse the firewall and access the SQL Server Express database. (However, you can still run the cmdlet locally on the Standard Edition server itself.) To run the **Lock-CsClientPin** cmdlet remotely you will need to manually enable the firewall exceptions for SQL Server Express.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Lock-CsClientPin** cmdlet locally: RTCUniversalServerAdmins. To return a list of all the role-based access control (RBAC) roles this cmdlet has been assigned to (including any custom RBAC roles you have created yourself), run the following command from the Windows PowerShell prompt:

Get-CsAdminRole | Where-Object {$\_.Cmdlets –match "Lock-CsClientPin"}

</div>

<div>

## Parameters


<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Required</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>Identity</em></p></td>
<td><p>Required</p></td>
<td><p>Microsoft.Rtc.Management.AD.UserIdParameter</p></td>
<td><p>Identity of the user account for which the PIN should be locked. User Identities can be specified by using one of four formats: 1) the user's SIP address; 2) the user's user principal name (UPN); 3) the user's domain name and logon name, in the form domain\logon (for example, litwareinc\kenmyer); and, 4) the user's Active Directory display name (for example, Ken Myer). You can also reference user Identities by using the user’s Active Directory distinguished name.</p>
<p>In addition, you can use the asterisk (*) wildcard character when using the Display Name as the user Identity. For example, the Identity &quot;* Smith&quot; returns all the users who have a display name that ends with the string value &quot; Smith&quot;.</p></td>
</tr>
<tr class="even">
<td><p><em>Confirm</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Prompts you for confirmation before executing the command.</p></td>
</tr>
<tr class="odd">
<td><p><em>Force</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Suppresses the display of any non-fatal error message that might occur when running the command.</p></td>
</tr>
<tr class="even">
<td><p><em>WhatIf</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Describes what would happen if you executed the command without actually executing the command.</p></td>
</tr>
</tbody>
</table>


</div>

<div>

## Input Types

String value or Microsoft.Rtc.Management.ADConnect.Schema.ADUser object. The **Lock-CsClientPin** cmdlet accepts pipelined input of string values representing the Identity of a user account. The cmdlet also accepts pipelined input of user objects.

</div>

<div>

## Return Types

The **Lock-CsClientPin** cmdlet does not return a value or object. Instead, the cmdlet configures one or more instances of the Microsoft.Rtc.Management.UserPinService.PinInfoDetails object.

</div>

<div>

## See Also


[Get-CsClientPinInfo](get-csclientpininfo.md)  
[Set-CsClientPin](set-csclientpin.md)  
[Unlock-CsClientPin](unlock-csclientpin.md)  
  

</div>

</div>

<span> </span>

</div>

</div>

</div>

