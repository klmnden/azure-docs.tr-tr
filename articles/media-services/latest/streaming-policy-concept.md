---
title: Azure Media Services ilkeleri akış | Microsoft Docs
description: Bu makalede, akış ilkeleri nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/03/2019
ms.author: juliako
ms.openlocfilehash: 10600d8f3ff4e08b8d90f28ec15d3cb0c56bcae0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230905"
---
# <a name="streaming-policies"></a>Akış İlkeleri

Azure Media Services v3 içinde [akış ilkeleri](https://docs.microsoft.com/rest/api/media/streamingpolicies) , akış protokolleri ve şifreleme seçeneklerini tanımlamanıza olanak sağlar, [akış bulucuları](streaming-locators-concept.md). Özel bir ilke oluşturulur veya önceden tanımlanmış akış ilkelerden birini ya da kullanabilirsiniz. Önceden tanımlanmış akış şu anda kullanılabilir ilkeleri şunlardır: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' ve 'Predefined_MultiDrmStreaming'.

> [!IMPORTANT]
> * Özelliklerini **akış ilkeleri** DateTime türü her zaman UTC biçiminde olan.
> * Medya hizmeti hesabınız için sınırlı sayıda ilkeleri tasarlayın ve aynı seçeneklere gerektiğinde bunları yeniden için akış Bulucular gerekir. 

## <a name="examples"></a>Örnekler

### <a name="not-encrypted"></a>Şifreli değil

Dosya içinde--Temizle (şifrelenmemiş) akışla aktarmak istiyorsanız, önceden tanımlanmış açık akış ilke ayarlama: 'Predefined_ClearStreamingOnly' için (.NET, PredefinedStreamingPolicy.ClearStreamingOnly kullanabilirsiniz).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly
    });
```

### <a name="encrypted"></a>Şifreli 

İçerik Zarfı ve cenc şifreleme ile şifreleme gerekiyorsa 'Predefined_MultiDrmCencStreaming' ilkenizi ayarlayın. Bu ilke, bulucuda iki içerik anahtarı (zarf ve CENC) oluşturulmasını ve ayarlanmasını istediğinizi belirtir. Bu nedenle zarf, PlayReady ve Widevine şifrelemeleri uygulanır (anahtar, yapılandırılan DRM lisanslarına göre kayıttan yürütme istemcisine teslim edilir).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = "Predefined_MultiDrmCencStreaming",
        DefaultContentKeyPolicyName = contentPolicyName
    });
```

Akışınız CBCS (FairPlay) ile şifrelemek istiyorsanız, 'Predefined_MultiDrmStreaming' kullanın.

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [AES-128 dinamik şifreleme ve anahtar dağıtım hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslimat hizmeti kullanın](protect-with-drm.md)
