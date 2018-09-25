---
title: Azure ön kapısı hizmeti - PowerShell başvurusu | Microsoft Docs
description: Bu başvuru Azure ön kapısı Service için desteklenen farklı PowerShell cmdlet'lerinin ayrıntıları
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2018
ms.author: sharadag
Module Name: AzureRM.FrontDoor
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/set-azurermfrontdoorfirewallpolicy
schema: 2.0.0
ms.openlocfilehash: 8fbfe3d5306038297306016385a4b7a70c28961e
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048315"
---
# Set-AzureRmFrontDoorFireWallPolicy
WAF ilkesini güncelleştirme

## SÖZ DİZİMİ

### ByFieldsParameterSet (varsayılan)
```
Set-AzureRmFrontDoorFireWallPolicy -ResourceGroupName <String> -Name <String> [-EnabledState <PSEnabledState>]
 [-Mode <PSMode>] [-Customrule <PSCustomRule[]>] [-ManagedRule <PSManagedRule[]>]
 [-DefaultProfile <IAzureContextContainer>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByObjectParameterSet
```
Set-AzureRmFrontDoorFireWallPolicy -InputObject <PSPolicy> [-EnabledState <PSEnabledState>] [-Mode <PSMode>]
 [-Customrule <PSCustomRule[]>] [-ManagedRule <PSManagedRule[]>] [-DefaultProfile <IAzureContextContainer>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ResourceIdParameterSet
```
Set-AzureRmFrontDoorFireWallPolicy -ResourceId <String> [-EnabledState <PSEnabledState>] [-Mode <PSMode>]
 [-Customrule <PSCustomRule[]>] [-ManagedRule <PSManagedRule[]>] [-DefaultProfile <IAzureContextContainer>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## AÇIKLAMA
**Kümesi AzureRmFrontDoor** cmdlet'i, varolan bir WAF ilkesini güncelleştirir. Giriş parametresi sağlanmazsa, var olan WAF ilkesinden eski parametreleri kullanılır.

## ÖRNEKLER

### Örnek 1: varolan bir WAF ilkesini güncelleştir
```powershell
PS C:\> Set-AzureRmFrontDoorFireWallPolicy -Name $name -ResourceGroupName $resourceGroup -Customrule $customRule -ManagedRule $managedRule -EnabledState $enabledState -Mode $node

PolicyMode         : Prevention
PolicyEnabledState : Enabled
CustomRules        : {Rule1}
ManagedRules       : {Microsoft.Azure.Commands.FrontDoor.Models.PSAzureManagedRule}
Etag               :
ProvisioningState  : Succeeded
Tags               :
Id                 : /subscriptions/{subid}/resourcegroups/{resourceGroupName}/providers/Micr
                     osoft.Network/frontdoorwebapplicationfirewallpolicies/{Name}
Name               : {Name}
Type               :
```

Mevcut bir WAF ilkesini güncelleştirme

### Örnek 2: var olan bir WAF ilkeyi güncelleştirme
```powershell
PS C:\> Set-AzureRmFrontDoorFireWallPolicy -InputObject $policy1 -Customrule $customRule -ManagedRule $managedRule -EnabledState $enabledState -Mode $mode

PolicyMode         : Prevention
PolicyEnabledState : Enabled
CustomRules        : {Rule1}
ManagedRules       : {Microsoft.Azure.Commands.FrontDoor.Models.PSAzureManagedRule}
Etag               :
ProvisioningState  : Succeeded
Tags               :
Id                 : /subscriptions/{subid}/resourcegroups/{resourceGroupName}/providers/Micr
                     osoft.Network/frontdoorwebapplicationfirewallpolicies/{Name}
Name               : {Name}
Type               :
```

Mevcut bir WAF ilkesini güncelleştirme

### Örnek 3: varolan bir WAF ilkesini güncelleştir
```powershell
PS C:\> Set-AzureRmFrontDoorFireWallPolicy -ResourceId $resourcdId -Customrule $customRule -ManagedRule $managedRule -EnabledState $enabledState -Mode $mode

PolicyMode         : Prevention
PolicyEnabledState : Enabled
CustomRules        : {Rule1}
ManagedRules       : {Microsoft.Azure.Commands.FrontDoor.Models.PSAzureManagedRule}
Etag               :
ProvisioningState  : Succeeded
Tags               :
Id                 : /subscriptions/{subid}/resourcegroups/{resourceGroupName}/providers/Micr
                     osoft.Network/frontdoorwebapplicationfirewallpolicies/{Name}
Name               : {Name}
Type               :
```

Mevcut bir WAF ilkesini güncelleştirme

### Örnek 4: $resourceGroup, tüm WAF ilkeleri güncelleştirmek
```powershell
PS C:\> Get-AzureRmFrontDoorFireWallPolicy -ResourceGroupName $resourceGroup | Set-AzureRmFrontDoorFireWallPolicy -Customrule $customRule -ManagedRule $managedRule -EnabledState $enabledState -Mode $mode
```

$resourceGroup, tüm WAF ilkeleri güncelleştirmek

## PARAMETRELER

### -Customrules
İlke içinde özel kurallar

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSCustomRule[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DefaultProfile
Kimlik, hesap, Kiracı ve Azure ile iletişim için kullanılan abonelik.

```yaml
Type: Microsoft.Azure.Commands.Common.Authentication.Abstractions.IAzureContextContainer
Parameter Sets: (All)
Aliases: AzureRmContext, AzureCredential

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnabledState
İlke olup durumu etkin veya durumu devre dışı.
Olası değerler şunlardır: 'Disabled', 'Enabled'

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSEnabledState
Parameter Sets: (All)
Aliases:
Accepted values: Enabled, Disabled

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Inputobject
Güncelleştirilecek FireWallPolicy nesne.

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSPolicy
Parameter Sets: ByObjectParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ManagedRule
İlke içinde kuralları

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSManagedRule[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Modu
Algılama modunda veya önleme modu İlkesi düzeyinde olup olmadığını açıklar.
Olası değerler şunlardır: 'Önleme', 'Algılama'

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSMode
Parameter Sets: (All)
Aliases:
Accepted values: Prevention, Detection

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Ad
Güncelleştirilecek FireWallPolicy adı.

```yaml
Type: System.String
Parameter Sets: ByFieldsParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceGroupName
FireWallPolicy ait olduğu kaynak grubu.

```yaml
Type: System.String
Parameter Sets: ByFieldsParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceId
Güncelleştirilecek FireWallPolicy kaynak kimliği

```yaml
Type: System.String
Parameter Sets: ResourceIdParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Onaylayın
Cmdlet'i çalıştırmadan önce onaylamanız istenir.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable. Daha fazla bilgi için bkz. about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
