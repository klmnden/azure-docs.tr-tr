---
title: Azure'dan geçiş medya Hizmetleri v3 v2 | Microsoft Docs
description: Bu makalede Azure Media Services v3 sunulan değişiklikleri açıklar ve iki sürümleri arasındaki farklar gösterilmektedir.
services: media-services
documentationcenter: na
author: Juliako
manager: cfowler
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 05/22/2018
ms.author: juliako
ms.openlocfilehash: 4e644db12a74d6ef132a0c8d64ef517a0c2253cc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655784"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Geçiş medya Hizmetleri v3 v2

> [!NOTE]
> En son Azure Media Services sürümü Önizleme aşamasındadır ve v3 olarak adlandırılabilir.

Bu makalede Azure Media Services (AMS) v3 sunulan değişiklikleri açıklar ve iki sürümleri arasındaki farklar gösterilmektedir.

## <a name="why-should-you-move-to-v3"></a>Neden v3 taşımanız gerekir?

### <a name="api-is-more-approachable"></a>API daha kullanılabilir

*  V3 Azure Resource Manager yerleşik yönetimi ve işlemleri işlevselliği kullanıma sunan bir birleşik API yüzeyi temel alır. Azure Resource Manager şablonları oluşturmak ve dönüşümler, akış uç noktaları, LiveEvents ve daha fazlasını dağıtmak için kullanılabilir.
* API açın (diğer adıyla Swagger) belirtim belgesi.
* SDK'ları için .net, .net kullanılabilir çekirdek, Node.js, Python, Java, Ruby.
* Azure CLI tümleştirme.

### <a name="new-features"></a>Yeni Özellikler

* Şimdi destekler kodlama HTTPS (Url tabanlı giriş) alma.
* Dönüşümler v3 yenidir. Bir dönüştürme yapılandırmaları paylaşmak, Azure Resource Manager şablonları oluşturmak ve belirli bir müşteri veya Kiracı kodlama ayarlarını yalıtmak için kullanılır. 
* Bir varlık farklı dinamik paketleme ve dinamik şifreleme ayarları ile birden çok StreamingLocators olabilir.
* İçerik koruma, birden çok anahtar özelliklerini destekler. 
* Dinamik paketleme ve dinamik şifreleme LiveEvent Önizleme destekler. Bu önizleme yanı sıra kısa çizgi ve HLS paketleme içerik koruması sağlar.
* LiveOuput daha eski Program varlığı kullanmak daha basittir. 
* Varlıklar üzerinde RBAC desteği eklendi.

## <a name="changes-from-v2"></a>V2 değişiklikler

* Media Services SDK'ları depolama SDK'sı üzerinde daha fazla denetim imkanı sağlayan depolama SDK ayrılmış kullanılır ve sürüm oluşturma sorunları önler. 
* V3, kodlama bit hızlarını saniyede bit tümü. Bu Medya Kodlayıcısı standart hazır ayarları REST v2'den farklı değildir. Örneğin, v2, bit hızı 128 belirtilmesi, ancak v3 128000 olacaktır. 
* AssetFiles, AccessPolicies, IngestManifests v3 yok.
* ContentKeys artık StreamingLocator özelliğinin bir varlık değildir.
* Olay kılavuz desteği NotificationEndpoints değiştirir.
* Bazı varlıklar yeniden adlandırıldı

  * JobOutput görevi, artık işi yalnızca parçası değiştirir.
  * LiveEvent kanal değiştirir.
  * LiveOutput Program değiştirir.
  * StreamingLocator Bulucu değiştirir.

## <a name="code-changes"></a>Kod değişiklikleri

### <a name="create-an-asset-and-upload-a-file"></a>Bir varlık oluşturun ve bir dosyayı karşıya yüklemek 

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

### <a name="submit-a-job"></a>Bir işi gönderin

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

### <a name="publish-an-asset-with-aes-encryption"></a>AES şifreleme içeren bir varlık yayımlamak 

#### <a name="v2"></a>v2

1. ContentKeyAuthorizationPolicyOption oluşturma
2. ContentKeyAuthorizationPolicy oluşturma
3. AssetDeliveryPolicy oluşturma
4. Varlık oluşturma ve içerik veya gönderme iş yüklemek ve çıkış varlığına kullanın
5. AssetDeliveryPolicy varlıkla ilişkilendirme
6. ContentKey oluşturma
7. Varlık için ContentKey ekleme
8. AccessPolicy oluşturma
9. Bulucu oluşturun

#### <a name="v3"></a>v3

1. İçerik anahtarı ilkesi oluşturma
2. Varlık oluşturma
3. İçerik yüklemek veya varlık JobOutput kullanın
4. Bulucu oluşturun

## <a name="next-steps"></a>Sonraki adımlar

Video dosyalarını kodlamaya ve akışa almaya başlamanın ne kadar kolay olduğunu görmek için [Dosyaları akışa alma](stream-files-dotnet-quickstart.md) bölümüne göz atın. 

