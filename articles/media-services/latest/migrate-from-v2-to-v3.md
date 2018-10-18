---
title: Azure geçişi Media Services v3 v2 | Microsoft Docs
description: Bu makalede, Azure Media Services v3 sürümünde yapılan değişiklikleri açıklar ve iki sürümü arasındaki farklar gösterilmektedir.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: a17bad5beb651aaa085c626296c200a00c30647e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376371"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Geçiş Media Services v3 v2

Bu makalede, Azure Media Services (AMS) v3 sürümünde yapılan değişiklikleri açıklar ve iki sürümü arasındaki farklar gösterilmektedir.

## <a name="why-should-you-move-to-v3"></a>Neden v3 taşımalısınız?

### <a name="api-is-more-approachable"></a>API daha ulaşılabilir

*  Azure Resource Manager üzerinde oluşturulmuş hem yönetim hem de işlemler işlevselliği kullanıma sunma birleştirilmiş bir API yüzeyi v3 dayanır. Azure Resource Manager şablonları, dönüşüm, akış uç noktalarını, LiveEvents ve daha fazlasını oluşturmak ve dağıtmak için kullanılabilir.
* Açık API (diğer adıyla Swagger) belirtimi belgesi.
* Mevcut .net,.Net Core SDK'ları, Node.js, Python, Java, Ruby.
* Azure CLI tümleştirmesi.

### <a name="new-features"></a>Yeni Özellikler

* Artık kodlama HTTPS (Url tabanlı giriş) alın.
* Dönüşümler v3 sürümünde yenidir. Dönüşüm yapılandırmaları paylaşmak, Azure Resource Manager şablonları oluşturmak ve belirli müşteri veya Kiracı için kodlama ayarlarını yalıtmak için kullanılır. 
* Bir varlık, farklı dinamik paketleme ve dinamik şifreleme ayarları ile birden çok StreamingLocators olabilir.
* İçerik koruma, birden çok anahtar özelliklerini destekler. 
* Dinamik paketleme ile dinamik şifrelemeden Livestream Önizleme destekler. Bu önizleme yanı sıra DASH ve HLS paketleme içerik koruması sağlar.
* LiveOuput eski programı varlık basittir. 
* Varlıklar üzerinde RBAC desteği eklendi.

## <a name="changes-from-v2"></a>V2 değişiklikleri

* Media Services v3 sürümünde, yalnızca depolama şifrelemesi (AES-256 şifreleme) olan varlıklarınızı Media Services v2 ile oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

    Media Services v3 ile varlıkları oluşturulan için destekler [Azure depolama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) sunucu tarafı depolama şifrelemesi.
    
* Medya Hizmetleri depolama SDK'sı hakkında daha fazla denetime izin veren depolama SDK'sı gelen ayrılmış SDK kullanılır ve sürüm oluşturma sorunları önler. 
* V3 sürümünde, tüm kodlama hızları bit / saniye cinsindendir. Bu, Media Encoder Standard hazır ayarları REST v2 farklıdır. Örneğin, bit hızı v2'de 128 belirtilmesi, ancak v3 sürümünde 128000 olacaktır. 
* AssetFiles, AccessPolicies, IngestManifests v3 sürümünde mevcut değil.
* ContentKeys artık StreamingLocator özelliği bir varlık değildir.
* Event Grid destek NotificationEndpoints değiştirir.
* Bazı varlıklar yeniden adlandırıldı

  * Görevi, artık işi yalnızca parçası JobOutput değiştirir.
  * Livestream kanal değiştirir.
  * Program LiveOutput değiştirir.
  * Bulucu StreamingLocator değiştirir.

## <a name="code-changes"></a>Kod değişiklikleri

### <a name="create-an-asset-and-upload-a-file"></a>Bir varlık oluşturun ve bir dosyayı karşıya yükleyin 

#### <a name="v2"></a>v2

```csharp
IAsset asset = context.Assets.Create(assetName, storageAccountName, options);

IAssetFile assetFile = asset.AssetFiles.Create(assetFileName);

assetFile.Upload(filePath);
```

#### <a name="v3"></a>v3

```csharp
Asset asset = client.Assets.CreateOrUpdate(resourceGroupName, accountName, assetName, new Asset());

var response = client.Assets.ListContainerSas(resourceGroupName, accountName, assetName, permissions: AssetContainerPermission.ReadWrite, expiryTime: DateTime.Now.AddHours(1));

var sasUri = new Uri(response.AssetContainerSasUrls.First());
CloudBlobContainer container = new CloudBlobContainer(sasUri);

var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));
blob.UploadFromFile(fileToUpload);
```

### <a name="submit-a-job"></a>Bir iş gönderdiniz

#### <a name="v2"></a>v2

```csharp
IMediaProcessor processor = context.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

IJob job = jobs.Create($"Job for {inputAsset.Name}");

ITask task = job.Tasks.AddNew($"Task for {inputAsset.Name}", processor, taskConfiguration);

task.InputAssets.Add(inputAsset);

task.OutputAssets.AddNew(outputAssetName, outputAssetStorageAccountName, outputAssetOptions);

job.Submit();
```

#### <a name="v3"></a>v3

```csharp
client.Assets.CreateOrUpdate(resourceGroupName, accountName, outputAssetName, new Asset());

JobOutput[] jobOutputs = { new JobOutputAsset(outputAssetName)};

JobInput jobInput = JobInputAsset(assetName: assetName);

Job job = client.Jobs.Create(resourceGroupName,
accountName, transformName, jobName,
new Job {Input = jobInput, Outputs = jobOutputs});
```

### <a name="publish-an-asset-with-aes-encryption"></a>Bir varlık AES şifreleme ile yayımlama 

#### <a name="v2"></a>v2

1. ContentKeyAuthorizationPolicyOption oluşturma
2. ContentKeyAuthorizationPolicy oluşturma
3. AssetDeliveryPolicy oluşturma
4. Varlık oluşturma ve içerik işi veya Gönder yüklemek ve çıktı varlığına kullanın
5. AssetDeliveryPolicy varlıkla ilişkilendirme
6. ContentKey oluşturma
7. Varlık için ContentKey ekleme
8. AccessPolicy oluşturma
9. Bulucu

#### <a name="v3"></a>v3

1. İçerik anahtarı ilkesi oluşturma
2. Varlık oluşturma
3. İçerik yüklemek veya varlık JobOutput kullanın
4. StreamingLocator oluşturma

## <a name="next-steps"></a>Sonraki adımlar

Video dosyalarını kodlamaya ve akışa almaya başlamanın ne kadar kolay olduğunu görmek için [Dosyaları akışa alma](stream-files-dotnet-quickstart.md) bölümüne göz atın. 

