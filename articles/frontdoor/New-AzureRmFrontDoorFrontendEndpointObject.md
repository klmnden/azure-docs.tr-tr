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
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/new-azurermfrontdoorfrontendendpointobject
schema: 2.0.0
ms.openlocfilehash: 6ab27ebb0b87dc7a81e1db03c2de8f679ed69a65
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048281"
---
# Yeni AzureRmFrontDoorFrontendEndpointObject

Ön kapı oluşturmak için bir PSFrontendEndpoint nesnesi oluşturun

## SÖZ DİZİMİ

```
New-AzureRmFrontDoorFrontendEndpointObject -Name <String> -HostName <String>
 [-SessionAffinityEnabledState <PSEnabledState>] [-SessionAffinityTtlInSeconds <Int32>]
 [-WebApplicationFirewallPolicyLink <String>] [-CertificateSource <PSCertificateSource>]
 [-ProtocolType <PSProtocolType>] [-Vault <String>] [-SecretName <String>] [-SecretVersion <String>]
 [-CertificateType <PSCertificateType>] [-DefaultProfile <IAzureContextContainer>] [<CommonParameters>]
```

## AÇIKLAMA
Ön kapı oluşturmak için bir PSFrontendEndpoint nesnesi oluşturun

## ÖRNEKLER

### Örnek 1
```powershell
PS C:\> New-AzureRmFrontDoorFrontendEndpointObject -Name "frontendendpoint1" -HostName $hostName


HostName                         : frontdoor5.azurefd.net
SessionAffinityEnabledState      : Disabled
SessionAffinityTtlSeconds        : 0
WebApplicationFirewallPolicyLink :
Backends                         :
CustomHttpsProvisioningState     :
CustomHttpsProvisioningSubstate  :
CertificateSource                : AzureKeyVault
ProtocolType                     : ServerNameIndication
Vault                            :
SecretName                       :
SecretVersion                    :
CertificateType                  : Shared
ResourceState                    :
Id                               :
Name                             : frontendendpoint1
Type                             :
```

Ön kapı oluşturmak için bir PSFrontendEndpoint nesnesi oluşturun

## PARAMETRELER

### -CertificateSource
SSL sertifikası kaynağı

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSCertificateSource
Parameter Sets: (All)
Aliases:
Accepted values: AzureKeyVault, FrontDoor

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateType
bir frontendEndpoint güvenli bağlantılar için kullanılan sertifika türü

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSCertificateType
Parameter Sets: (All)
Aliases:
Accepted values: Shared, Dedicated

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

### -Ana bilgisayar adı
FrontendEndpoint konak adı.
Bir etki alanı adı olmalıdır.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Ad
Ön uç uç nokta adı.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProtocolType
Güvenli dağıtım için kullanılan TLS uzantısını Protokolü

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSProtocolType
Parameter Sets: (All)
Aliases:
Accepted values: ServerNameIndication, IPBased

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SecretName
Tam Sertifika PFX temsil eden anahtar kasası gizli dizi adı

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SecretVersion
Tam Sertifika PFX temsil eden anahtar kasası gizli dizi sürümü

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SessionAffinityEnabledState
Bu ana bilgisayarda oturum benzeşimi izin verilip verilmeyeceğini.
Varsayılan değer devre dışı bırakıldı

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

### -SessionAffinityTtlInSeconds
Oturum benzeşimi, saniye cinsinden varsa kullanmak için TTL. Varsayılan değer 0'dır

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Vault
SSL sertifikasını içeren anahtar kasası

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WebApplicationFirewallPolicyLink
Kaynak kimliğini (varsa) her konak için Web uygulaması güvenlik duvarı ilkesi

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable. Daha fazla bilgi için bkz. about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
