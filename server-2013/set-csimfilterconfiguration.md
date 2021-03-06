﻿---
title: Set-CsImFilterConfiguration
TOCTitle: Set-CsImFilterConfiguration
ms:assetid: c2b08cc0-8261-4b8b-b2e9-5efa902ceb40
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Gg412960(v=OCS.15)
ms:contentKeyID: 48185331
ms.date: 07/23/2014
mtps_version: v=OCS.15
---

<div data-xmlns="http://www.w3.org/1999/xhtml">

<div class="topic" data-xmlns="http://www.w3.org/1999/xhtml" data-msxsl="urn:schemas-microsoft-com:xslt" data-cs="http://msdn.microsoft.com/en-us/">

<div data-asp="http://msdn2.microsoft.com/asp">

# Set-CsImFilterConfiguration

</div>

<div id="mainSection">

<div id="mainBody">

<span> </span>

_**Topic Last Modified:** 2013-03-25_

Modifies an existing instant messaging (IM) filter configuration. IM filter settings are used to prevent users from sending instant messages that contain live (clickable) hyperlinks. This cmdlet was introduced in Lync Server 2010.

<div>

## Syntax

    Set-CsImFilterConfiguration [-Identity <XdsIdentity>] <COMMON PARAMETERS>

    Set-CsImFilterConfiguration [-Instance <PSObject>] <COMMON PARAMETERS>

    COMMON PARAMETERS: [-Action <Allow | Block | Warn>] [-AllowMessage <String>] [-BlockFileExtension <$true | $false>] [-Confirm [<SwitchParameter>]] [-Enabled <$true | $false>] [-Force <SwitchParameter>] [-IgnoreLocal <$true | $false>] [-Prefixes <PSListModifier>] [-WarnMessage <String>] [-WhatIf [<SwitchParameter>]]

</div>

<div>

## Examples

<div>

## EXAMPLE 1

The command shown in this example disables URI filtering for the IM filter configuration with the Identity site:Redmond. To carry out this task, the Enabled parameter is specified in the call to the **Set-CsImFilterConfiguration** cmdlet with a value of $False.

    Set-CsImFilterConfiguration -Identity site:Redmond -Enabled $False

</div>

<div>

## EXAMPLE 2

Example 2 adds a new URI prefix (urn:) to the list of prefixes prohibited by the IM filter configuration for site:Redmond. To add the new prefix, the **Get-CsImFilterConfiguration** cmdlet is used to retrieve the configuration for site:Redmond; the returned object representing that configuration is stored in a variable named $x. After the settings have been retrieved, the Add() method is called in line 2 to add urn: to the set of prefixes stored in the Prefixes property. This changes the value of the object reference $x. To change the actual configuration itself, line 3 calls the **Set-CsImFilterConfiguration** cmdlet, passing $x as the sole parameter value.

    $x = Get-CsImFilterConfiguration -Identity site:Redmond
    $x.Prefixes.Add("urn:")
    Set-CsImFilterConfiguration -Instance $x

</div>

<div>

## EXAMPLE 3

Example 3 performs the exact same action as Example 2, but it does it in one line. In this command the Prefixes parameter of the **Set-CsImFilterConfiguration** cmdlet is used to add urn: to the list of prefixes. The add list modifier is used to add this value to the Prefixes list.

    Set-CsImFilterConfiguration -Identity site:Redmond -Prefixes @{add="urn:"}

</div>

<div>

## EXAMPLE 4

In this example the prefix urn: is removed from the list of prefixes blocked by the IM filter configuration for site:Redmond. This example is identical to Example 3 except that rather than calling the add list modifier to add a prefix to the list, the remove modifier is called to remove a prefix.

    Set-CsImFilterConfiguration -Identity site:Redmond -Prefixes @{remove="urn:"}

</div>

</div>

<div>

## Detailed Description

When sending instant messages, users can embed a Uniform Resource Identifier (URI) within the text of that message to refer other participants in the conversation to a particular web site or share. Lync Server can be configured so that hyperlinks with certain prefixes are blocked or are not active. (In other words, the participants can’t simply click the link and go to the site the URI refers to; they must copy and paste the link manually into a browser.)

The **Set-CsImFilterConfiguration** cmdlet allows you to modify a list of URI prefixes that will be filtered, in addition to enabling and disabling filtering altogether, either globally or within a specific site. With this cmdlet you can also update various messages that provide warnings to users.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Set-CsImFilterConfiguration** cmdlet locally: RTCUniversalServerAdmins. To return a list of all the role-based access control (RBAC) roles this cmdlet has been assigned to (including any custom RBAC roles you have created yourself), run the following command from the Windows PowerShell prompt:

Get-CsAdminRole | Where-Object {$\_.Cmdlets –match "Set-CsImFilterConfiguration"}

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
<td><p><em>Action</em></p></td>
<td><p>Optional</p></td>
<td><p>UrlFilterAction</p></td>
<td><p>The value of this parameter determines the action that will be taken when a hyperlink is included in an instant message:</p>
<p>Allow - Hyperlinks are prefixed with an underscore so that the links are no longer active. In addition, if a message is specified in the AllowMessage property a notification message will be inserted at the beginning of each instant message containing hyperlinks.</p>
<p>Block - Delivery of messages containing active hyperlinks are blocked and Lync Server sends an error message to the sender.</p>
<p>Warn - Messages containing active hyperlinks are delivered to the receiving participants, along with a warning message that is inserted at the beginning of those messages. The warning message can be specified using the WarnMessage property. If Warn is specified and no WarnMessage is entered, IM filtering is disabled, although the settings for the BlockFileExtension property will still be honored.</p></td>
</tr>
<tr class="even">
<td><p><em>AllowMessage</em></p></td>
<td><p>Optional</p></td>
<td><p>String</p></td>
<td><p>If a value is specified for this parameter, that string is inserted at the beginning of each message containing hyperlinks when the value of the Action property is set to Allow. You can use this message to notify users of things such as the potential dangers of clicking unknown links or your organization's relevant policies and requirements.</p></td>
</tr>
<tr class="odd">
<td><p><em>BlockFileExtension</em></p></td>
<td><p>Optional</p></td>
<td><p>Boolean</p></td>
<td><p>If this parameter is set to True, a hyperlink that contains a file path with an extension specified by the Extensions property defined in the Microsoft.Rtc.Management.WritableConfig.Settings.ImFilter.FileTransferFilterConfiguration class (retrieved by calling the <strong>Get-CsFileTransferFilterConfiguration</strong> cmdlet) is blocked and an error message is returned to the sender. If this parameter is set to False, no special check is made for file paths and extensions.</p>
<p>Default: True</p></td>
</tr>
<tr class="even">
<td><p><em>Confirm</em></p></td>
<td><p>Optional</p></td>
<td><p>SwitchParameter</p></td>
<td><p>Prompts you for confirmation before executing the command.</p></td>
</tr>
<tr class="odd">
<td><p><em>Enabled</em></p></td>
<td><p>Optional</p></td>
<td><p>Boolean</p></td>
<td><p>Enables or disables this feature. If this parameter is set to True, instant messages will be scanned for hyperlinks and the rules in this configuration will be applied. If this parameter is set to False, messages will not be checked for hyperlinks.</p>
<p>Default: True</p></td>
</tr>
<tr class="even">
<td><p><em>Force</em></p></td>
<td><p>Optional</p></td>
<td><p>SwitchParameter</p></td>
<td><p>Suppresses any confirmation prompts that would otherwise be displayed before making changes.</p></td>
</tr>
<tr class="odd">
<td><p><em>Identity</em></p></td>
<td><p>Optional</p></td>
<td><p>XdsIdentity</p></td>
<td><p>The unique identifier of the IM filter configuration settings you want to modify. This value will either be global or site:&lt;site name&gt;, where &lt;site name&gt; is the site to which the configuration applies, such as site:Redmond.</p></td>
</tr>
<tr class="even">
<td><p><em>IgnoreLocal</em></p></td>
<td><p>Optional</p></td>
<td><p>Boolean</p></td>
<td><p>The value of this parameter controls whether filtering is performed on local Intranet URIs passed in instant messages.If this parameter is set to True, any URI that is defined in the Intranet zone of the local computer is ignored. (The local computer is the Front End Server running the IM Filter application.) If this parameter is set to False, the specified filtering is applied to all hyperlinks.</p>
<p>Default: True</p></td>
</tr>
<tr class="odd">
<td><p><em>Instance</em></p></td>
<td><p>Optional</p></td>
<td><p>ImFilterConfiguration</p></td>
<td><p>Allows you to pass a reference to an object to the cmdlet rather than set individual parameter values. This cmdlet accepts an object of type Microsoft.Rtc.Management.WritableConfig.Settings.ImFilter.ImFilterConfiguration, which can be retrieved by calling the <strong>Get-CsImFilterConfiguration</strong> cmdlet.</p></td>
</tr>
<tr class="even">
<td><p><em>Prefixes</em></p></td>
<td><p>Optional</p></td>
<td><p>PSListModifier</p></td>
<td><p>The list of URI prefixes that will be filtered. Any hyperlink included in an instant message with a prefix matching one of the prefixes in this list will be filtered according to the specified settings.</p>
<p>Default: callto:, file:, ftp., ftp:, gopher:, href, http:, https:, ldap:, mailto:, news:, nntp:, sip:, sips:, tel:, telnet:, www*.</p></td>
</tr>
<tr class="odd">
<td><p><em>WarnMessage</em></p></td>
<td><p>Optional</p></td>
<td><p>String</p></td>
<td><p>This parameter contains the warning message that is inserted at the beginning of each instant message that contains hyperlinks when the value of the Action property is set to Warn. Typically this message would be used for such things as stating the potential dangers of clicking unknown links or referring to your organization's relevant policies and requirements.</p></td>
</tr>
<tr class="even">
<td><p><em>WhatIf</em></p></td>
<td><p>Optional</p></td>
<td><p>SwitchParameter</p></td>
<td><p>Describes what would happen if you executed the command without actually executing the command.</p></td>
</tr>
</tbody>
</table>


</div>

<div>

## Input Types

Microsoft.Rtc.Management.WritableConfig.Settings.ImFilter.ImFilterConfiguration object. Accepts pipelined input of IM filter configuration objects.

</div>

<div>

## Return Types

The **Set-CsImFilterConfiguration** cmdlet does not return a value or object. Instead, the cmdlet configures instances of the Microsoft.Rtc.Management.WritableConfig.Settings.ImFilter.ImFilterConfiguration object.

</div>

<div>

## See Also


[New-CsImFilterConfiguration](new-csimfilterconfiguration.md)  
[Remove-CsImFilterConfiguration](remove-csimfilterconfiguration.md)  
[Get-CsImFilterConfiguration](get-csimfilterconfiguration.md)  
  

</div>

</div>

<span> </span>

</div>

</div>

</div>

