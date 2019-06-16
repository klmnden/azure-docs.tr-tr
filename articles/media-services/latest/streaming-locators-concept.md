---
title: Azure Media Services Bulucular akış | Microsoft Docs
description: Bu makalede, akış Bulucuyu nedir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/26/2019
ms.author: juliako
ms.openlocfilehash: 5897b7df2460257784c40eb974c473573ec4003d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66299159"
---
# <a name="streaming-locators"></a>Akış Bulucuları

Çıkış Varlığınızdaki videoların istemcilerde kayıttan yürütülebilmesi için bir [Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) ve ardından akış URL'si oluşturmanız gerekir. Bir URL oluşturmak için akış uç noktası ana bilgisayar adı ve akış Bulucu yolu birleştirmeniz gerekir. .NET örneği için bkz. [Akış Bulucu edinme](stream-files-tutorial-with-api.md#get-a-streaming-locator).

Oluşturma işlemi bir **akış Bulucu** yayımlama denir. Varsayılan olarak, **akış Bulucu** API çağrılarını hemen sonra geçerli olduğunu ve isteğe bağlı bir başlangıç ve bitiş zamanlarını yapılandırmadığınız sürece silinene kadar sürer. 

Oluştururken bir **akış Bulucu**, belirtmelisiniz bir **varlık** adı ve **akış ilke** adı. Daha fazla bilgi için aşağıdaki konulara bakın:

* [Varlıklar](assets-concept.md)
* [Akış İlkeleri](streaming-policy-concept.md)
* [İçerik Anahtarı İlkeleri](content-key-policy-concept.md)

Ayrıca, akış Kullanıcınızı yalnızca imkan sağlayacak Bulucu, başlangıç ve bitiş saati içerik (örneğin, arasında 5/1/2019 5/5/2019 için') şu saatler arasında yürütmek de belirtebilirsiniz.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* **Akış bulucuları** güncelleştirilebilir değil. 
* Özelliklerini **akış bulucuları** DateTime türü her zaman UTC biçiminde olan.
* Medya hizmeti hesabınız için sınırlı sayıda ilkeleri tasarlayın ve aynı seçeneklere gerektiğinde bunları yeniden için akış Bulucular gerekir. Daha fazla bilgi için [kotaları ve sınırlamaları](limits-quotas-constraints.md).

## <a name="create-streaming-locators"></a>Akış bulucuları oluşturma  

### <a name="not-encrypted"></a>Şifreli değil

Dosya içinde--Temizle (şifrelenmemiş) akışla aktarmak istiyorsanız, önceden tanımlanmış açık akış ilke ayarlama: 'Predefined_ClearStreamingOnly' için (.NET, PredefinedStreamingPolicy.ClearStreamingOnly enum kullanabilirsiniz).

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

İçeriğinizi CENC şifreleme ile şifreleme gerekiyorsa 'Predefined_MultiDrmCencStreaming' ilkenizi ayarlayın. Widevine şifreleme için bir DASH akışında ve PlayReady düzgün şekilde uygulanır. Anahtar yapılandırılmış DRM lisansları temel bir kayıttan yürütme istemciye teslim edilir.

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

Ayrıca, HLS akışı CBCS (FairPlay) ile şifrelemek istiyorsanız, 'Predefined_MultiDrmStreaming' kullanın.

## <a name="associate-filters-with-streaming-locators"></a>Akış bulucuları ile ilişkilendirme filtreleri

Bkz: [filtreleri: akış bulucuları ile ilişkilendirme](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="filter-order-page-streaming-locator-entities"></a>Filtre sırası, sayfa akış Bulucu varlıklar

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="list-streaming-locators-by-asset-name"></a>Liste akış bulucuları varlık adı

İlişkili varlık adını temel alarak Bulucular akış almak için şu işlemleri kullanın:

|Dil|API|
|---|---|
|REST|[liststreaminglocators](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators)|
|CLI|[az ams varlığı liste-streaming-bulucular](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest#az-ams-asset-list-streaming-locators)|
|.NET|[listStreamingLocators](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocators?view=azure-dotnet#Microsoft_Azure_Management_Media_AssetsOperationsExtensions_ListStreamingLocators_Microsoft_Azure_Management_Media_IAssetsOperations_System_String_System_String_System_String_)|
|Java|[AssetStreamingLocator](https://docs.microsoft.com/java/api/com.microsoft.azure.management.mediaservices.v2018_07_01.assetstreaminglocator?view=azure-java-stable)|
|Node.js|[listStreamingLocators](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/assets?view=azure-node-latest#liststreaminglocators-string--string--string--object-)|

## <a name="also-see"></a>Ayrıca bkz:

* [Varlıklar](assets-concept.md)
* [Akış İlkeleri](streaming-policy-concept.md)
* [İçerik Anahtarı İlkeleri](content-key-policy-concept.md)

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: .NET kullanarak videoları karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
