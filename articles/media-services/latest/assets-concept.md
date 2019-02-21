---
title: Varlıklar Media Services - Azure | Microsoft Docs
description: Bu makalede, varlıklar nedir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/19/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 2ec2ddbac5d0368aaf1b46208c9ebb44bf12a622
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447319"
---
# <a name="assets"></a>Varlıklar

Azure Media Services, bir [varlık](https://docs.microsoft.com/rest/api/media/assets) dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta veriler içerir. Dijital dosyalar bir varlığa karşıya yüklendikten sonra medya kodlama, akış, içerik iş akışları çözümleme Hizmetleri'nde kullanılabilir. Daha fazla bilgi için [varlıklarına dijital dosyaları karşıya yükleme](#upload-digital-files-into-assets) bölümüne bakın.

Bir varlık, bir blob kapsayıcısını eşlendi [Azure depolama hesabı](storage-account-concept.md) ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Media Services hesabı genel amaçlı v2 kullandığında Blob katmanları destekler (GPv2) depolama. GPv2 ile dosyalarını taşıyabilirsiniz [seyrek erişimli veya arşiv depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Arşiv** depolama (örneğin, bunlar kodlanmış sonra) artık gerekli olmadığında kaynak dosyalarını arşivlemek için uygundur.

**Arşiv** kodlama iş çıktısı bir çıkış blob kapsayıcısında koyulmuş ve depolama katmanı yalnızca zaten kodlanmış çok büyük kaynak dosyaları için önerilir. Bir varlık ve akış veya içeriğinizi çözümlemek için kullanım ile ilişkilendirmek istediğiniz çıkış kapsayıcıdaki blobları bulunması gereken bir **etkin** veya **seyrek erişimli** depolama katmanı.

> [!NOTE]
> Datetime türündeki varlığın özellikleri her zaman UTC biçimindedir.

## <a name="upload-digital-files-into-assets"></a>Dijital dosyalar varlıklarına karşıya yükleme

Genel medya Hizmetleri iş akışlarını karşıya yükleme, kodlama ve bir dosya akışı biridir. Bu bölümde, genel adımlar anlatılmaktadır.

1. Yeni bir "Giriş" varlık oluşturmak için Media Services v3 API'sini kullanın. Bu işlem, Media Services hesabınızla ilişkili depolama hesabında bir kapsayıcı oluşturur. API, kapsayıcı adı döndürür (örneğin, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Bir varlık ile ilişkilendirmek istediğiniz bir blob kapsayıcısı zaten varsa, bir varlık oluştururken, kapsayıcı adı belirtebilirsiniz. Media Services şu anda yalnızca BLOB kapsayıcı kök ve değil dosya adında yolları destekler. Bu nedenle, bir kapsayıcı "input.mp4" dosya adıyla birlikte çalışır. Ancak, "videos/inputs/input.mp4" dosya adıyla bir kapsayıcı çalışmaz.

    Doğrudan herhangi bir depolama hesabı ve aboneliğinizde haklarınız kapsayıcı yüklemek için Azure CLI'yı kullanabilirsiniz. <br/>Kapsayıcı adı benzersiz olmalı ve depolama adlandırma yönergeleri izleyin. Ad, biçimlendirme medya Hizmetleri varlık kapsayıcı adı (varlık GUID'si) izleyin gerekmez. 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Varlık kapsayıcısının içine dijital dosyalar karşıya yüklemek için kullanılacak olan okuma-yazma izinlerine sahip bir SAS URL'sini alın. Media Services API'sine kullanabileceğiniz [varlık kapsayıcı URL'lerin listesi](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Azure depolama API veya SDK'larını (örneğin, [depolama REST API'si](../../storage/common/storage-rest-api-auth.md), [JAVA SDK'sı](../../storage/blobs/storage-quickstart-blobs-java-v10.md), veya [.NET SDK'sı](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) dosyaları varlık kapsayıcıya yüklemek için. 
4. Media Services v3 API'ler, Dönüşüm ve bir iş Varlığınızı "Giriş" işlemi oluşturmak için kullanın. Daha fazla bilgi için [dönüşümler ve işler](transform-concept.md).
5. "Çıktı" varlığına içerikten Stream.

Gösteren tam bir .NET örneği için nasıl yapılır: varlık oluşturma, depolama kapsayıcısını varlığın yazılabilir bir SAS URL'si almak, SAS URL'sini kullanarak depolama kapsayıcısına dosya karşıya yükleme, bkz [yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Yeni varlık oluşturma

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

REST örnek için bkz: [REST ile bir varlık oluşturun](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) örnek.

Bu örnek nasıl oluşturulacağını gösterir **istek gövdesi** açıklaması, kapsayıcı adı, depolama hesabı ve diğer bilgileri gibi yararlı bilgileri belirtebileceğiniz.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Tam bir örnek için bkz: [yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md). Media Services v3 sürümünde bir işin girdisini de HTTPS URL'lerden oluşturulabilir (bkz [bir HTTPS URL'si iş girdisi oluşturma](job-input-from-http-how-to.md)).

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

Bekleyen veri varlıklarınızı korumanın varlıklar tarafından depolama tarafı şifrelemesi şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifrelemesi Media Services'de nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services'ı depolama şifrelemesi|Media Services tarafından yönetilen AES-256 şifreleme anahtarı|Desteklenen<sup>(1)</sup>|Desteklenmeyen<sup>(2)</sup>|
|[Bekleyen veriler için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifrelemesi, Azure Depolama tarafından sunulan anahtarı Azure tarafından veya müşteri tarafından yönetilen|Desteklenen|Desteklenen|
|[Depolama istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar Kasası'nda müşteri tarafından yönetilen bir kiracı anahtarı tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|Desteklenmiyor|

<sup>1</sup> sırada Media Services, işleme içeriğinin desteklemez, açıkta/herhangi bir biçimde şifreleme olmadan, bunu yapmanız bu nedenle önerilmez.

<sup>2</sup> , Media Services v3 (AES-256 şifreleme) depolama şifrelemesi, yalnızca varlıklarınızı Media Services v2 ile oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [Bir bulut DVR kullanma](live-event-cloud-dvr.md)
* [Medya arasındaki farklar Hizmetleri v2 ve v3](migrate-from-v2-to-v3.md)
