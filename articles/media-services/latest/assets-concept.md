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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60734363"
---
# <a name="assets"></a>Varlıklar

Azure Media Services, bir [varlık](https://docs.microsoft.com/rest/api/media/assets) dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta veriler içerir. Dijital dosyalar bir varlığa karşıya yüklendikten sonra medya kodlama, akış, içerik iş akışları çözümleme Hizmetleri'nde kullanılabilir. Daha fazla bilgi için [varlıklarına dijital dosyaları karşıya yükleme](#upload-digital-files-into-assets) bölümüne bakın.

Bir varlık, bir blob kapsayıcısını eşlendi [Azure depolama hesabı](storage-account-concept.md) ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Media Services hesabı genel amaçlı v2 kullandığında Blob katmanları destekler (GPv2) depolama. GPv2 ile dosyalarını taşıyabilirsiniz [seyrek erişimli veya arşiv depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Arşiv** depolama (örneğin, bunlar kodlanmış sonra) artık gerekli olmadığında kaynak dosyalarını arşivlemek için uygundur.

**Arşiv** kodlama iş çıktısı bir çıkış blob kapsayıcısında koyulmuş ve depolama katmanı yalnızca zaten kodlanmış çok büyük kaynak dosyaları için önerilir. Bir varlık ve akış veya içeriğinizi çözümlemek için kullanım ile ilişkilendirmek istediğiniz çıkış kapsayıcıdaki blobları bulunması gereken bir **etkin** veya **seyrek erişimli** depolama katmanı.

> [!NOTE]
> Datetime türündeki varlığın özellikleri her zaman UTC biçimindedir.

## <a name="upload-digital-files-into-assets"></a>Dijital dosyalar varlıklarına karşıya yükleme

Genel medya Hizmetleri iş akışlarını karşıya yükleme, kodlama ve bir dosya akışı biridir. Bu bölümde, genel adımlar anlatılmaktadır.

1. Media Services v3 API'sini kullanarak yeni bir "input" Varlığı oluşturun. Bu işlem, Media Services hesabınızla ilişkilendirilen depolama hesabında bir kapsayıcı oluşturur. API, kapsayıcı adı döndürür (örneğin, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Varlıkla ilişkilendirmek istediğiniz bir blob kapsayıcınız varsa, Varlığı oluştururken ilgili kapsayıcının adını belirtebilirsiniz. Media Services şu anda blobları yalnızca kapsayıcı kökünde destekler ve dosya adında yol kullanılmasını desteklemez. Bu nedenle "input.mp4" gibi bir dosya adına sahip kapsayıcıları kullanabilirsiniz. Ancak "videos/inputs/input.mp4" dosya adına sahip bir kapsayıcıyı kullanamazsınız.

    Azure CLI'yi kullanarak aboneliğinizde gerekli izinlere sahip olduğunuz tüm depolama hesaplarına ve kapsayıcılara içerik yükleyebilirsiniz. <br/>Kapsayıcı adının benzersiz ve depolama adlandırma kurallarına uygun olması gerekir. Ad için Media Services Varlık kapsayıcısı adı (Varlık-GUID) biçimlendirmesinin kullanılması şart değildir. 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Dijital dosyaları Varlık kapsayıcısına yüklemek için kullanmak üzere okuma-yazma izinlerine sahip bir SAS URL'si alın. Media Services API'sini kullanarak [varlık kapsayıcısı URL'lerini listeleyebilirsiniz](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Azure Depolama API'lerini veya SDK'larını ([Depolama REST API](../../storage/common/storage-rest-api-auth.md), [JAVA SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md) veya [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md) gibi) kullanarak dosyaları Varlık kapsayıcısına yükleyin. 
4. Media Services v3 API'lerini kullanarak "input" Varlığınızı işlemek üzere bir Dönüşüm ve bir İş oluşturun. Daha fazla bilgi için [Dönüşümler ve İşler](transform-concept.md) konusuna bakın.
5. "Çıktı" varlığına içerikten Stream.

Gösteren tam bir .NET örneği için nasıl yapılır: varlık oluşturma, depolama kapsayıcısını varlığın yazılabilir bir SAS URL'si almak, SAS URL'sini kullanarak depolama kapsayıcısına dosya karşıya yükleme, bkz [yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Yeni varlık oluşturma

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

REST örnek için bkz: [REST ile bir varlık oluşturun](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) örnek.

Bu örnekte açıklama, kapsayıcı adı, depolama hesabı ve benzeri gerekli bilgileri belirtebileceğiniz **İstek Gövdesini** oluşturma adımları gösterilmektedir.

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
* [Bulut DVR kullanma](live-event-cloud-dvr.md)
* [Medya arasındaki farklar Hizmetleri v2 ve v3](migrate-from-v2-to-v3.md)
