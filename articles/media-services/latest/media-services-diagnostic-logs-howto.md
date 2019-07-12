---
title: Media Services tanılama günlükleri, Azure İzleyici ile izleme | Microsoft Docs
description: Bu makalede, Azure İzleyici aracılığıyla tanılama günlükleri görüntülemek ve gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 233b043ffdc295fe94ed2e3ba837d4229848df22
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795849"
---
# <a name="monitor-media-services-diagnostic-logs"></a>Media Services tanılama günlüklerini izleyin

[Azure İzleyici](../../azure-monitor/overview.md) ölçümlerini izleme ve yardımcı olacak tanılama günlüklerini anlama, uygulamalarınızın performansını sağlar. Bu ayrıntılı açıklaması için özellik ve neden Azure Media Services ölçümleri ve tanılama günlüklerini kullanmak istiyorsunuz görmek için bkz: [İzleyici Media Services ölçümleri ve tanılama günlüklerini](media-services-metrics-diagnostic-logs.md).

Bu makalede veri depolama hesabına Yönlendirme ve ardından verileri görüntülemeyi gösterir. 

## <a name="prerequisites"></a>Önkoşullar

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).
- Gözden geçirme [İzleyici Media Services ölçümleri ve tanılama günlüklerini](media-services-metrics-diagnostic-logs.md).

## <a name="route-data-to-the-storage-account-using-the-portal"></a>Portalı kullanarak depolama hesabı için rota verilerini

1. [https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.
1. Media Services hesabınıza gidin ve tıklayın **tanılama ayarları** altında **İzleyici**. Burada, aboneliğinizde Azure İzleyici ile izleme verileri oluşturan tüm kaynakların bir listesini görürsünüz. 

    ![Tanılama ayarları bölümü](media/media-services-diagnostic-logs/logs01.png)

1. Tıklayın **tanılama ayarı ekleme**.

   Kaynak tanılama ayarı, belirli bir kaynaktan *hangi* izleme verilerinin yönlendirilmesi gerektiğine ve bu izleme verilerinin *nereye* gideceğine ilişkin bir tanımdır.

1. Görüntülenen bölümde, ayarınıza bir **ad** verin ve **Bir depolama hesabında arşivle** kutusunu işaretleyin.

    Günlükleri ve ENTER tuşuna göndermek istediğiniz depolama hesabını seçin **Tamam**.
1. **Günlük** ve **Ölçüm** altındaki tüm kutuları işaretleyin. Kaynak türüne bağlı olarak, bu seçeneklerden yalnızca birini kullanabilirsiniz. Bu onay kutuları, seçtiğiniz hedefe (bu örnekte bir depolama hesabına) ilgili kaynak türü için kullanılabilen günlük ve ölçüm verileri kategorilerinden hangilerinin gönderildiğini denetler.

   ![Tanılama ayarları bölümü](media/media-services-diagnostic-logs/logs02.png)
1. **Bekletme (gün)** kaydırıcısını 30’a ayarlayın. Bu kaydırıcı, depolama hesabında izleme verilerinin tutulacağı gün sayısını ayarlar. Azure İzleyici, belirtilen gün sayısından daha eski verileri otomatik olarak siler. Bekletme günü sayısının sıfır olması verileri süresiz olarak depolar.
1. **Kaydet**’e tıklayın.

Kaynağınızdaki izleme verileri artık depolama hesabına akar.

## <a name="route-data-to-the-storage-account-using-the-cli"></a>CLI kullanarak depolama hesabı için rota verilerini

Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için aşağıdaki çalıştıracağınız `az monitor diagnostic-settings` CLI komutunu: 

```cli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Örneğin:

```cli
az monitor diagnostic-settings create --name amsv3diagnostic \
    --storage-account storageaccountforams  \
    --resource "/subscriptions/00000000-0000-0000-0000-0000000000/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount" \
    --resource-group "amsResourceGroup" \
    --logs '[{"category": "KeyDeliveryRequests",  "enabled": true, "retentionPolicy": {"days": 3, "enabled": true }}]'
```

## <a name="view-data-in-the-storage-account-using-the-portal"></a>Portalı kullanarak depolama hesabındaki görünümü verileri

Yukarıdaki adımları izlediyseniz, veriler depolama hesabınıza akmaya başlamıştır.

Olayın depolama hesabında görünmesi için beş dakikaya kadar beklemeniz gerekebilir.

1. Portalda, sol gezinti çubuğundaki **Depolama Hesapları** bölümüne gidin.
1. Önceki bölümde oluşturduğunuz depolama hesabını belirleyin ve tıklayın.
1. Tıklayarak **Blobları**, sonra etiketli kapsayıcıya **ınsights günlükleri keydeliveryrequests**. Günlüklerinizi içerdiğinden kapsayıcıdır. İzleme verilerini kapsayıcılarına kaynak Kimliğine göre daha sonra tarih ve saat tarafından ayrılmıştır.
1. Kaynak kimliği, tarih ve saat için kapsayıcılara tıklayarak PT1H.json dosyasına gidin. PT1H.json dosyasına ve **İndir**’e tıklayın.

 Artık depolama hesabında depolanmış JSON olayını görüntüleyebilirsiniz.

### <a name="examples-of-pt1hjson"></a>PT1H.json örnekleri

#### <a name="clear-key-delivery-log"></a>Şifresiz anahtar teslim günlüğü

```json
{
  "time": "2019-05-21T00:07:33.2820450Z",
  "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/amsResourceGroup/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/AMSACCOUNT",
  "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
  "operationVersion": "1.0",
  "category": "KeyDeliveryRequests",
  "resultType": "Succeeded",
  "resultSignature": "OK",
  "durationMs": 253,
  "identity": {
    "authorization": {
      "issuer": "myIssuer",
      "audience": "myAudience"
    },
    "claims": {
      "urn:microsoft:azure:mediaservices:contentkeyidentifier": "e4276e1d-c012-40b1-80d0-ac15808b9277",
      "nbf": "1558396914",
      "exp": "1558400814",
      "iss": "myIssuer",
      "aud": "myAudience"
    }
  },
  "level": "Informational",
  "location": "westus2",
  "properties": {
    "requestId": "fb5c2b3a-bffa-4434-9c6f-73d689649add",
    "keyType": "Clear",
    "keyId": "e4276e1d-c012-40b1-80d0-ac15808b9277",
    "policyName": "SharedContentKeyPolicyUsedByAllAssets",
    "tokenType": "JWT",
    "statusMessage": "OK"
  }
}
```

#### <a name="widevine-encrypted-key-delivery-log"></a>Widevine şifrelenmiş anahtar teslim günlüğü

```json
{
  "time": "2019-05-20T23:15:22.7088747Z",
  "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/amsResourceGroup/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/AMSACCOUNT",
  "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
  "operationVersion": "1.0",
  "category": "KeyDeliveryRequests",
  "resultType": "Succeeded",
  "resultSignature": "OK",
  "durationMs": 69,
  "identity": {
    "authorization": {
      "issuer": "myIssuer",
      "audience": "myAudience"
    },
    "claims": {
      "urn:microsoft:azure:mediaservices:contentkeyidentifier": "0092d23a-0a42-4c5f-838e-6d1bbc6346f8",
      "nbf": "1558392430",
      "exp": "1558396330",
      "iss": "myIssuer",
      "aud": "myAudience"
    }
  },
  "level": "Informational",
  "location": "westus2",
  "properties": {
    "requestId": "49613dd2-16aa-4595-a6e0-4e68beae6d37",
    "keyType": "Widevine",
    "keyId": "0092d23a-0a42-4c5f-838e-6d1bbc6346f8",
    "policyName": "DRMContentKeyPolicy",
    "tokenType": "JWT",
    "statusMessage": "OK"
  }
}
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure İzleyici ölçümleri](../../azure-monitor/platform/data-platform.md)
* [Azure İzleyici tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Toplama ve Azure kaynaklarınızdan günlük verilerini kullanma](../../azure-monitor/platform/diagnostic-logs-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[İzleyici ölçümleri](media-services-metrics-howto.md)
