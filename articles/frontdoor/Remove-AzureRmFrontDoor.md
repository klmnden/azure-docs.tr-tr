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
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/remove-azurermfrontdoor
schema: 2.0.0
ms.openlocfilehash: ddc6a40521b951c8083ac185368621ea792b1c85
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048447"
---
# Remove-AzureRmFrontDoor
Ön Yük Dengeleyiciyi kaldırma

## SÖZ DİZİMİ

### ByFieldsParameterSet (varsayılan)
```
Remove-AzureRmFrontDoor -ResourceGroupName <String> -Name <String> [-PassThru]
 [-DefaultProfile <IAzureContextContainer>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByObjectParameterSet
```
Remove-AzureRmFrontDoor -InputObject <PSFrontDoor> [-PassThru] [-DefaultProfile <IAzureContextContainer>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ResourceIdParameterSet
```
Remove-AzureRmFrontDoor -ResourceId <String> [-PassThru] [-DefaultProfile <IAzureContextContainer>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## AÇIKLAMA
**Remove-AzureRmFrontDoor** cmdlet'i, geçerli abonelik kapsamında bir ön kapısı yük dengeleyici kaldırır

## ÖRNEKLER

### Örnek 1: kaynak grubu "rg1" geçerli bir abonelik altında "frontdoor1" kaldırın.
```powershell
PS C:\> Remove-AzureRmFrontDoor -Name "frontdoor1" -ResourceGroupName "rg1"
```

Geçerli abonelik altındaki kaynak grubu "rg1" içinde "frontdoor1" kaldırın.

### Örnek 2: geçerli abonelik altındaki kaynak grubu "rg1" içinde tüm FrontDoors kaldırın.
```powershell
PS C:\> Get-AzureRmFrontDoor -ResourceGroupName "rg1" | Remove-AzureRmFrontDoor
```

Geçerli abonelik altındaki kaynak grubu "rg1" içinde tüm FrontDoors kaldırın.

### Örnek 3: geçerli abonelik altındaki tüm FrontDoors kaldırın.
```powershell
PS C:\> Get-AzureRmFrontDoor | Remove-AzureRmFrontDoor
```

Geçerli abonelik altındaki tüm FrontDoors kaldırın.

### Örnek 4: geçerli abonelik altında "frontdoor1" adı ile tüm FrontDoors kaldırın.
```powershell
PS C:\> Remove-AzureRmFrontDoor -Name "frontdoor1" | Remove-AzureRmFrontDoor
```

Geçerli abonelik altında "frontdoor1" adı ile tüm FrontDoors kaldırın.

## PARAMETRELER

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

### -Inputobject
Silmek için ön kapı nesne.

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSFrontDoor
Parameter Sets: ByObjectParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Ad
Silmek için ön kapı adı.

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

### -PassThru
Nesne (belirtilmişse) döndürür.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceGroupName
Ön kapısı ait olduğu kaynak grubu.

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
Silmek için ön kapı kaynak kimliği

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
