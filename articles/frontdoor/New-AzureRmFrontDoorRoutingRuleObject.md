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
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/new-azurermfrontdoorroutingruleobject
schema: 2.0.0
ms.openlocfilehash: 7dc7eff0120041406b8bbad1ccfe4b042f4006f9
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048305"
---
# Yeni AzureRmFrontDoorRoutingRuleObject
Ön kapı oluşturmak için bir PSRoutingRuleObject oluşturma

## SÖZ DİZİMİ

```
New-AzureRmFrontDoorRoutingRuleObject -ResourceGroupName <String> -FrontDoorName <String> -Name <String>
 -FrontendEndpointName <String[]> -BackendPoolName <String> [-AcceptedProtocol <PSProtocol[]>]
 [-PatternToMatch <String[]>] [-CustomForwardingPath <String>] [-ForwardingProtocol <PSForwardingProtocol>]
 [-EnableCaching <Boolean>] [-QueryParameterStripDirective <PSQueryParameterStripDirective>]
 [-DynamicCompression <PSEnabledState>] [-EnabledState <PSEnabledState>]
 [-DefaultProfile <IAzureContextContainer>] [<CommonParameters>]
```

## AÇIKLAMA
Ön kapı oluşturmak için bir PSRoutingRuleObject oluşturma

## ÖRNEKLER

### Örnek 1: bir PSRoutingRuleObject ön kapı oluşturmak için Oluştur
```powershell
PS C:\> New-AzureRmFrontDoorRoutingRuleObject -Name $routingRuleName -FrontDoorName $frontDoorName -ResourceGroupName  -FrontendEndpointName "frontendEndpoint1" -BackendPoolName "backendPool1" 

FrontendEndpointIds          : {/subscriptions/{subid}/resourceGroups/{rgname}/pro
                               viders/Microsoft.Network/frontDoors/{frontdoorname}/FrontendEndpoints/frontendEndpoint1}
AcceptedProtocols            : {Http, Https}
PatternsToMatch              : {/*}
ForwardingProtocol           : MatchRequest
CustomForwardingPath         :
QueryParameterStripDirective : StripAll
DynamicCompression           : Enabled
HealthProbeSettings          :
BackendPoolId                : /subscriptions/{subid}/resourceGroups/{rgname}/prov
                               iders/Microsoft.Network/frontDoors/{frontdoorname}/BackendPools/backendPool1
EnableCaching                : Disabled
EnabledState                 : Enabled
ResourceState                :
Id                           :
Name                         : routingrule1
Type                         :
```

Ön kapı oluşturmak için bir PSRoutingRuleObject oluşturma

## PARAMETRELER

### -AcceptedProtocol
Bu kural ile eşleştirilecek protokol şemaları.
Varsayılan değer: {Https ve Http}

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSProtocol[]
Parameter Sets: (All)
Aliases:
Accepted values: Http, Https

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -BackendPoolName
Bu kural yönlendirir BackendPool kaynak kimliği

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

### -CustomForwardingPath
Bu kural tarafından eşleşen kaynak yolları yeniden yazmak için kullanılan özel yol.
Gelen yolu kullanmak için boş bırakın.

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

### -DynamicCompression
Dinamik sıkıştırma için önbelleğe alınmış içeriği önbelleğe alırken etkinleştirilip etkinleştirilmeyeceğini etkinleştirilir.
Varsayılan değer etkin

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

### -EnableCaching
Bu rota için önbelleğe alma'ı etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan değer false'tur

```yaml
Type: System.Boolean
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnabledState
Bu kural kullanımını etkinleştirilip etkinleştirilmeyeceğini belirtir.
Varsayılan değer etkin

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

### -ForwardingProtocol
Arka uçları varsayılan değer trafiği MatchRequest olduğunda bu kuralı protokolü kullanır.

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSForwardingProtocol
Parameter Sets: (All)
Aliases:
Accepted values: HttpOnly, HttpsOnly, MatchRequest

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FrontDoorName
Bu yönlendirme kuralı ait olduğu ön kapısı adı.

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

### -FrontendEndpointName
Bu kuralla ilişkili ön uç uç nokta adları

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Ad
RoutingRule adı.

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

### -PatternToMatch
Kuralın yol desenleri herhangi olmamalıdır * dışındaki büyük olasılıkla en son sonra / yolun sonuna. Varsayılan değer: / *

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -QueryParameterStripDirective
URL sorgu işleme, önbellek anahtarı oluşturma, koşulları.
Varsayılan değer StripAll:

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSQueryParameterStripDirective
Parameter Sets: (All)
Aliases:
Accepted values: StripNone, StripAll

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceGroupName
RoutingRule oluşturulacağı kaynak grubu adı.

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

### CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable. Daha fazla bilgi için bkz. about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
