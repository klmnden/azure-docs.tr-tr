---
title: PowerShell ile Azure CDN yönetme | Microsoft Docs
description: Azure CDN yönetmek için Azure PowerShell cmdlet'lerini kullanmayı öğrenin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5634ecdec04f023d9eb901c4ad0fb21b13bcfdc1
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="manage-azure-cdn-with-powershell"></a>Azure CDN PowerShell ile yönetme
PowerShell Azure CDN profili ve uç noktaları yönetmek için en esnek yöntemi sağlar.  Yönetim görevlerini otomatikleştirmek için etkileşimli olarak veya komut dosyaları yazarak PowerShell'i kullanabilirsiniz.  Bu öğretici birkaç gösterir Azure CDN profili ve uç noktaları yönetmek için PowerShell ile en yaygın görevleri gerçekleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Azure CDN profili ve uç noktaları yönetmek için PowerShell kullanmak için Azure PowerShell Modülü yüklü olması gerekir.  Azure PowerShell'i yükleyin ve Azure kullanarak bağlanma hakkında bilgi edinmek için `Connect-AzureRmAccount` cmdlet'ini bkz [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

> [!IMPORTANT]
> İle oturum açmanız gerekir `Connect-AzureRmAccount` Azure PowerShell cmdlet'lerini çalıştırmadan önce.
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a>Azure CDN cmdlet'leri listeleme
Tüm Azure CDN cmdlet'leri kullanma listeleyebilirsiniz `Get-Command` cmdlet'i.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Yardım alma
Herhangi bir kullanarak bu cmdlet'leri ile Yardım alabilirsiniz `Get-Help` cmdlet'i.  `Get-Help` Kullanım ve sözdizimi sağlar ve isteğe bağlı olarak örnekler gösterilmektedir.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Liste mevcut Azure CDN profili
`Get-AzureRmCdnProfile` Hiçbir parametre olmadan cmdlet'i tüm mevcut CDN profili alır.

```powershell
Get-AzureRmCdnProfile
```

Numaralandırma için cmdlet'leri yöneltilen bu çıkış.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Profil adı ve kaynak grubu belirterek tek bir profil de döndürebilir.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> Farklı kaynak gruplarında oldukları sürece aynı ada sahip birden çok CDN profili olması mümkündür.  Atlama `ResourceGroupName` parametresi eşleşen bir ada sahip tüm profillerini döndürür.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Liste mevcut CDN uç noktası
`Get-AzureRmCdnEndpoint` tek bir uç nokta veya bir profildeki tüm uç noktaları alabilir.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>CDN profili ve uç noktaları oluşturma
`New-AzureRmCdnProfile` ve `New-AzureRmCdnEndpoint` CDN profili ve uç noktaları oluşturmak için kullanılır.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Uç nokta ad kullanılabilirliğini denetleme
`Get-AzureRmCdnEndpointNameAvailability` bir uç nokta adı olup olmadığını gösteren bir nesne döndürür.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Özel bir etki alanı ekleme
`New-AzureRmCdnCustomDomain` özel etki alanı için mevcut bir uç noktası ekler.

> [!IMPORTANT]
> CNAME açıklandığı gibi DNS sağlayıcınız ile ayarlamalısınız [özel etki alanını içerik teslim ağı (CDN) uç noktasına eşleme nasıl](cdn-map-content-to-custom-domain.md).  Eşleme kullanan uç noktasını değiştirmeden önce sınayabilirsiniz `Test-AzureRmCdnCustomDomain`.
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Bir uç nokta değiştirme
`Set-AzureRmCdnEndpoint` Mevcut bir uç noktası değiştirir.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Temizleme/öncesi-loading CDN varlıklar
`Unpublish-AzureRmCdnEndpointContent` temizler varlıklar, önbelleğe alınmış durumdayken `Publish-AzureRmCdnEndpointContent` desteklenen uç noktasında varlıkları önceden yükler.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Başlatma/durdurma CDN uç noktası
`Start-AzureRmCdnEndpoint` ve `Stop-AzureRmCdnEndpoint` tekil uç noktalarını ya da grupları uç noktaları durdurmak ve başlatmak için kullanılabilir.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN kaynakları silme
`Remove-AzureRmCdnProfile` ve `Remove-AzureRmCdnEndpoint` profilleri ve uç noktaları kaldırmak için kullanılır.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Sonraki Adımlar
Azure CDN’yi [.NET](cdn-app-dev-net.md) veya [Node.js](cdn-app-dev-node.md) ile nasıl otomatik hale getireceğinizi öğrenin.

CDN özellikleri hakkında bilgi edinmek için [CDN'ye genel bakış](cdn-overview.md).

