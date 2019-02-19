---
title: Azure Media Services - CLI ile video dosyaları Stream | Microsoft Docs
description: Yeni bir Azure Media Services hesabı oluşturmak, bir dosyayı kodlamak ve Azure Media Player’da akışa almak için bu hızlı başlangıcın adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: ''
ms.date: 02/15/2019
ms.author: juliako
ms.openlocfilehash: c0b1f3fb854f4ca553d24ed601749cf91c2b5f28
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56339813"
---
# <a name="quickstart-stream-video-files---cli"></a>Hızlı Başlangıç: Stream video dosyaları - CLI

Bu hızlı başlangıçta, Azure Media Services kullanarak çok çeşitli tarayıcı ve cihazda videoları kodlamanın akışa almaya başlamanın ne kadar kolay olduğu size gösterilmektedir. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak girdi içeriği belirtilebilir.
Bu konu başlığındaki örnek, bir HTTPS URL’si aracılığıyla erişilebilir hale getirdiğiniz içerikleri kodlar. AMS v3 şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklememektedir.

Hızlı başlangıcın sonunda bir videoyu akışa alabileceksiniz.  

![Videoyu yürütme](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Şifreleme, kodlama, çözümleme, yönetme ve azure'da medya içeriği akışı başlatmak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı, bir veya daha fazla depolama hesapları ile ilişkili olması gerekiyor.

Media Services hesabı ve tüm ilişkili depolama hesapları aynı Azure aboneliğinde olması gerekir. Depolama hesapları Media Services hesabıyla aynı konumda ek gecikme süresi ve veri kullanım maliyetleri önlemek için önemle tavsiye edilir.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

```azurecli
az group create -n amsResourceGroup -l westus2
```

### <a name="create-an-azure-storage-account-general-purpose-v2-standard-ragrs"></a>Bir azure depolama hesabı, genel amaçlı v2, standart RAGRS oluşturma

```azurecli
az storage account create -n amsstorageaccount --kind StorageV2 --sku Standard_RAGRS -l westus2 -g amsResourceGroup
```

### <a name="create-an-azure-media-service-account"></a>Bir azure medya hizmeti hesabı oluştur

```azurecli
az ams account create --n amsaccount -g amsResourceGroup --storage-account amsstorageaccount -l westus2
```

```
{
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "location": "West US 2",
  "mediaServiceId": "8b569c2e-d648-4fcb-9035-c7fcc3aa7ddf",
  "name": "amsaccount",
  "resourceGroup": "amsResourceGroupTest",
  "storageAccounts": [
    {
      "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Storage/storageAccounts/amsstorageaccount",
      "resourceGroup": "amsResourceGroupTest",
      "type": "Primary"
    }
  ],
  "tags": null,
  "type": "Microsoft.Media/mediaservices"
}
```

## <a name="start-streaming-endpoint"></a>Akış uç noktasını başlatın

Aşağıdaki CLI varsayılan başlar **akış uç noktası**.

```azurecli
az ams streaming-endpoint start  -n default -a amsaccount -g amsResourceGroup
```

Başlatıldıktan sonra bir yanıt şuna benzer olursunuz:

```
az ams streaming-endpoint start  -n default -a amsaccount -g amsResourceGroup
{
  "accessControl": null,
  "availabilitySetName": null,
  "cdnEnabled": true,
  "cdnProfile": "AzureMediaStreamingPlatformCdnProfile-StandardVerizon",
  "cdnProvider": "StandardVerizon",
  "created": "2019-02-06T21:58:03.604954+00:00",
  "crossSiteAccessPolicies": null,
  "customHostNames": [],
  "description": "",
  "freeTrialEndTime": "2019-02-21T22:05:31.277936+00:00",
  "hostName": "amsaccount-usw22.streaming.media.azure.net",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingendpoints/default",
  "lastModified": "2019-02-06T21:58:03.604954+00:00",
  "location": "West US 2",
  "maxCacheAge": null,
  "name": "default",
  "provisioningState": "Succeeded",
  "resourceGroup": "amsResourceGroup",
  "resourceState": "Running",
  "scaleUnits": 0,
  "tags": {},
  "type": "Microsoft.Media/mediaservices/streamingEndpoints"
}
```

Akış uç noktasını zaten çalışıyorsa, Al

```
(InvalidOperation) The server cannot execute the operation in its current state.
```

## <a name="create-a-transform-for-adaptive-bitrate-encoding"></a>Uyarlamalı bit hızlı kodlama için dönüşüm oluşturma

Oluşturma bir **dönüştürme** kodlama veya videoları analiz için ortak görevler yapılandırmak için. Bu örnekte, Uyarlamalı bit hızlı kodlama yapmak istiyoruz. Ardından, gönderir bir **iş** altında oluşturduğunuz Dönüştür. İş giriş belirli bir video veya ses içeriğini dönüştürme uygulamak için gerçek Media Services'e isteğidir.

```azurecli
az ams transform create --name testEncodingTransform --preset AdaptiveStreaming --description 'a simple Transform for Adaptive Bitrate Encoding' -g amsResourceGroup -a amsaccount
```

Bir yanıt şuna benzer alın:

```
{
  "created": "2019-02-15T00:11:18.506019+00:00",
  "description": "a simple Transform for Adaptive Bitrate Encoding",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform",
  "lastModified": "2019-02-15T00:11:18.506019+00:00",
  "name": "testEncodingTransform",
  "outputs": [
    {
      "onError": "StopProcessingJob",
      "preset": {
        "odatatype": "#Microsoft.Media.BuiltInStandardEncoderPreset",
        "presetName": "AdaptiveStreaming"
      },
      "relativePriority": "Normal"
    }
  ],
  "resourceGroup": "amsResourceGroup",
  "type": "Microsoft.Media/mediaservices/transforms"
}
```

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma

Bir çıkış oluşturur **varlık** kodlama işinin çıktı olarak kullanılır.

```azurecli
az ams asset create -n testOutputAssetName -a amsaccount -g amsResourceGroup
```

Bir yanıt şuna benzer alın:

```
{
  "alternateId": null,
  "assetId": "96427438-bbce-4a74-ba91-e38179b72f36",
  "container": null,
  "created": "2019-02-14T23:58:19.127000+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/assets/testOutputAssetName",
  "lastModified": "2019-02-14T23:58:19.127000+00:00",
  "name": "testOutputAssetName",
  "resourceGroup": "amsResourceGroup",
  "storageAccountName": "amsstorageaccount",
  "storageEncryptionFormat": "None",
  "type": "Microsoft.Media/mediaservices/assets"
}
```

## <a name="start-job-with-https-input"></a>HTTPS giriş ile işi başlatma

Media Services v3 sürümünde kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş videosunun nerede bulacağını söylemeniz gerekir. Seçeneklerden birini (Bu örnekte gösterildiği gibi) giriş işi bir HTTPS URL'si belirtmek içindir. 

```azurecli
az ams job start --name testJob001 --transform-name testEncodingTransform --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' --files 'Ignite-short.mp4' --output-assets testOutputAssetName= -a amsaccount -g amsResourceGroup 
```

> [!TIP]
> İşin label özelliği ayarlamazsanız bile "=" çıkış varlıklar adına eklemek zorunda bildirimi çıkarır.

Bir yanıt şuna benzer alın:

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

### <a name="check-status"></a>Durumu kontrol etme

5 dakika içinde işinin durumunu denetleyin. Bu "tamamlanması". Bu değil, birkaç dakika içinde kontrol edin. "Bittikten sonra" sonraki adıma gidin ve oluşturma bir **akış Bulucu**.

```azurecli
az ams job show -a amsaccount -g amsResourceGroup -t testEncodingTransform -n testJob001
```

## <a name="create-streaming-locator-and-get-path"></a>Akış Bulucusu oluşturmak ve yolu

Kodlama tamamlandıktan sonra sonraki adım video çıktı varlığı kullanılabilir kayıttan yürütme için istemcilere olmasını sağlamaktır. Bunu iki adımda gerçekleştirebilirsiniz: ilk olarak, oluşturun bir **akış Bulucu**ve ikinci olarak, istemcilerin kullandığı akış URL'leri oluşturun.

### <a name="create-a-streaming-locator"></a>Akış Bulucusu oluşturma

```azurecli
az ams streaming-locator create -n testStreamingLocator --asset-name testOutputAssetName --streaming-policy-name Predefined_ClearStreamingOnly  -g amsResourceGroup -a amsaccount 
```

Bir yanıt şuna benzer alın:

```
{
  "alternativeMediaId": null,
  "assetName": "output-3b6d7b1dffe9419fa104b952f7f6ab76",
  "contentKeys": [],
  "created": "2019-02-15T04:35:46.270750+00:00",
  "defaultContentKeyPolicyName": null,
  "endTime": "9999-12-31T23:59:59.999999+00:00",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingLocators/testStreamingLocator",
  "name": "testStreamingLocator",
  "resourceGroup": "amsResourceGroup",
  "startTime": null,
  "streamingLocatorId": "e01b2be1-5ea4-42ca-ae5d-7fe704a5962f",
  "streamingPolicyName": "Predefined_ClearStreamingOnly",
  "type": "Microsoft.Media/mediaservices/streamingLocators"
}
```

### <a name="get-streaming-locator-paths"></a>Akış Bulucusu yolları alın

```azurecli
az ams streaming-locator get-paths -a amsaccount -g amsResourceGroup -n testStreamingLocator
```

Bir yanıt şuna benzer alın:

```
{
  "downloadPaths": [],
  "streamingPaths": [
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)"
      ],
      "streamingProtocol": "Hls"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=mpd-time-csf)"
      ],
      "streamingProtocol": "Dash"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest"
      ],
      "streamingProtocol": "SmoothStreaming"
    }
  ]
}
```

Hls yoluna kopyalayın. Bu durumda: `/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)`.

## <a name="build-url"></a>Yapı (URL) 

### <a name="get-streaming-endpoint-host-name"></a>Akış uç noktası ana bilgisayar adı

```azurecli
az ams streaming-endpoint list -a amsaccount -g amsResourceGroup -n default
```

Kopyalama `hostName` değeri. Bu durumda: `amsaccount-usw22.streaming.media.azure.net`.

### <a name="assemble-url"></a>URL bir araya getirin

"https:// " + &lt;hostName value&gt; + &lt;Hls path value&gt;

#### <a name="example"></a>Örnek

`https://amsaccount-usw22.streaming.media.azure.net/7f19e783-927b-4e0a-a1c0-8a140c49856c/ignite.ism/manifest(format=m3u8-aapl)`

## <a name="play-back-with-azure-media-player"></a>Azure Media Player ile kayıttan yürütme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir web tarayıcısı açın ve [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/) sayfasına gidin.
2. İçinde **URL:** kutusunda, önceki bölümde oluşturulan URL'yi yapıştırın. 
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıçta oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa kaynak grubunu silin.

Aşağıdaki CLI komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [CLI örnekleri](cli-samples.md)
