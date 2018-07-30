﻿---
title: Set-CsConfigurationStoreLocation
TOCTitle: Set-CsConfigurationStoreLocation
ms:assetid: 1c69a872-8e78-4c78-ba27-f20f04dce59f
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Gg398258(v=OCS.15)
ms:contentKeyID: 48183558
ms.date: 07/23/2014
mtps_version: v=OCS.15
---

<div data-xmlns="http://www.w3.org/1999/xhtml">

<div class="topic" data-xmlns="http://www.w3.org/1999/xhtml" data-msxsl="urn:schemas-microsoft-com:xslt" data-cs="http://msdn.microsoft.com/en-us/">

<div data-asp="http://msdn2.microsoft.com/asp">

# Set-CsConfigurationStoreLocation

</div>

<div id="mainSection">

<div id="mainBody">

<span> </span>

_**Topic Last Modified:** 2013-03-07_

Sets the Active Directory service control point for the Central Management store. This cmdlet was introduced in Lync Server 2010.

<div>

## Syntax

    Set-CsConfigurationStoreLocation -SqlServerFqdn <Fqdn> [-Confirm [<SwitchParameter>]] [-Force <SwitchParameter>] [-GlobalSettingsDomainController <Fqdn>] [-MirrorSqlInstanceName <String>] [-MirrorSqlServerFqdn <Fqdn>] [-Report <String>] [-SkipLookup <SwitchParameter>] [-SqlInstanceName <String>] [-WhatIf [<SwitchParameter>]]

</div>

<div>

## Examples

<div>

## EXAMPLE 1

The command shown in Example 1 sets the Active Directory service control point for the Lync Server Central Management store. In this example, the SCP points to the computer atl-sql-001.litwareinc.com and to the SQL Server instance Rtc.

    Set-CsConfigurationStoreLocation -SqlServerFqdn atl-sql-001.litwareinc.com -SqlInstanceName Rtc

</div>

<div>

## EXAMPLE 2

The command shown in Example 2 is a variation of the command shown in Example 1. This command also sets the Active Directory SCP for the Lync Server Central Management store. In addition, information regarding the success (or failure) of this operation is logged to the file C:\\Logs\\Store\_Location.html. This log is generated by including the Report parameter followed by the full path to the log file.

    Set-CsConfigurationStoreLocation -SqlServerFqdn atl-sql-001.litwareinc.com -SqlInstanceName Rtc -Report C:\Logs\Store_Location.html

</div>

</div>

<div>

## Detailed Description

Active Directory Domain Services uses service control points (SCP) to help computers locate services. For example, when you install Lync Server, a service control point is created that provides location information for the Central Management store used to maintain Lync Server data. Computers that need access to the database connect to Active Directory and use the information contained in the SCP to help them locate the correct computer and the correct instance of SQL Server.

As noted, when you install Lync Server an SCP that for the Central Management store is automatically created for you. If you need to move that database to another computer or if you need to move that database to a different instance of SQL Server, then you will need to update the corresponding service control point. That’s a task that can be carried out by using the **Set-CsConfigurationStoreLocation** cmdlet. When you run it, the **Set-CsConfigurationStoreLocation** cmdlet searches Active Directory for the computer specified by the SqlServer parameter. The cmdlet then sets the store location to the FQDN of that computer.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Set-CsConfigurationStoreLocation** cmdlet: RTCUniversalServerAdmins.

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
<td><p><em>SqlServerFqdn</em></p></td>
<td><p>Required</p></td>
<td><p>Microsoft.Rtc.Management.Deploy.Fqdn</p></td>
<td><p>Fully qualified domain name (FQDN) of the computer where the Central Management store has been installed. For example: -SqlServer atl-sql-001.litwareinc.com.</p></td>
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
<td><p><em>GlobalSettingsDomainController</em></p></td>
<td><p>Optional</p></td>
<td><p>Microsoft.Rtc.Management.Deploy.Fqdn</p></td>
<td><p>FQDN of a domain controller where global settings are stored. If global settings are stored in the Active Directory System container, then this parameter must point to the root domain controller. If global settings are stored in the Configuration container, then any domain controller can be used and this parameter can be omitted.</p></td>
</tr>
<tr class="odd">
<td><p><em>MirrorSqlInstanceName</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>Name of the SQL Server instance containing the Lync Server mirror database tables and data. For example: -SqlInstanceName &quot;rtc&quot;.</p></td>
</tr>
<tr class="even">
<td><p><em>MirrorSqlServerFqdn</em></p></td>
<td><p>Optional</p></td>
<td><p>Microsoft.Rtc.Management.Deploy.Fqdn</p></td>
<td><p>Fully qualified domain name (FQDN) of the computer where the Central Management store mirror database has been installed. For example: -SqlServer atl-mirror-001.litwareinc.com.</p></td>
</tr>
<tr class="odd">
<td><p><em>Report</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>Enables you to specify a file path for the log file created when the cmdlet runs. For example: -Report &quot;C:\Logs\ConfigurationStore.html&quot;</p></td>
</tr>
<tr class="even">
<td><p><em>SkipLookup</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>If this parameter is included then the <strong>Set-CsConfigurationStoreLocation</strong> cmdlet will not verify that the specified computer and the specified instance of SQL Server are available. Instead, it will simply change the service control point.</p>
<p>If this parameter is not included then both the specified computer and the specified instance of SQL Server must be available before the SCP will be changed.</p></td>
</tr>
<tr class="odd">
<td><p><em>SqlInstanceName</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>Name of the SQL Server instance containing the Lync Server tables and data. For example: -SqlInstanceName &quot;rtc&quot;.</p></td>
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

None. The **Set-CsConfigurationStoreLocation** cmdlet does not accept pipelined input.

</div>

<div>

## Return Types

The **Set-CsConfigurationStoreLocation** cmdlet does not return any objects or values.

</div>

<div>

## See Also


[Get-CsConfigurationStoreLocation](get-csconfigurationstorelocation.md)  
[Remove-CsConfigurationStoreLocation](remove-csconfigurationstorelocation.md)  
  

</div>

</div>

<span> </span>

</div>

</div>

</div>
