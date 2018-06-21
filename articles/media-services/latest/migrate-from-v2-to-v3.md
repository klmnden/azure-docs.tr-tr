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
ms.date: 06/12/2018
ms.author: juliako
ms.openlocfilehash: a382af644d30f9f0ebb586273c982ef1766f50b0
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295690"
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

* Media Services v3 depolama şifreleme (AES 256 şifreleme) Only'dir varlıklarınızı ile Media Services v2 oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifrelenmiş ancak yenilerini oluşturulmasına izin vermez.

    Varlıklar v3 ile oluşturulan için Media Services destekler [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) sunucu tarafı depolama şifrelemesi.
    
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
4. StreamingLocator oluşturma

## <a name="next-steps"></a>Sonraki adımlar

Video dosyalarını kodlamaya ve akışa almaya başlamanın ne kadar kolay olduğunu görmek için [Dosyaları akışa alma](stream-files-dotnet-quickstart.md) bölümüne göz atın. 

