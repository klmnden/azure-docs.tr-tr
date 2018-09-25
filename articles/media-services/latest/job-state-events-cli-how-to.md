---
title: Azure Media Services olaylarını bir özel web uç noktasına yönlendirme | Microsoft Docs
description: Media Services iş durum değişiklik olayı için abone olmak için Azure Event grid'i kullanın.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/20/2018
ms.author: juliako
ms.openlocfilehash: e7268a066acf41c454de0c66aa21603199d85a60
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034850"
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>CLI kullanarak bir özel web uç noktası için Azure Media Services olaylarını yönlendirme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Azure Media Services iş durum değişikliği olayları abone olmak için Azure CLI'yı kullanın ve sonucu görüntülemek için olayı tetiklersiniz. 

Normalde olayları olaya yanıt veren bir uç noktaya (web kancası veya Azure İşlevi gibi) gönderirsiniz. Bu öğreticide, bir Web kancası oluşturma ve nasıl gösterir.

Bu makalede açıklanan adımları tamamladıktan sonra olay verilerinin bir uç noktaya gönderildiğini görürsünüz.

## <a name="prerequisites"></a>Önkoşullar

- Etkin bir Azure aboneliğine sahip.
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

    Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun.

- Yükleme [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Bu makalede, Azure CLI 2.0 veya sonraki bir sürümünü gerektirir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Ayrıca [Azure Cloud Shell](https://shell.azure.com/bash).

## <a name="enable-event-grid-resource-provider"></a>Event Grid kaynak sağlayıcısını etkinleştirme

Yapmanız gereken ilk şey, Event Grid kaynak sağlayıcısı, aboneliğinizde etkin olduğundan emin olun ' dir. 

İçinde **Azure** portal, şunları yapın:

1. Aboneliklere Git.
2. Aboneliğinizi seçin.
3. Ayarlar altında kaynak sağlayıcılarını seçin.
4. "EventGrid" arayın.
5. Event Grid kayıtlı olduğundan emin olun. Aksi takdirde, basın **kaydetme** düğmesi.  

## <a name="create-a-generic-azure-function-webhook"></a>Genel bir Azure işlevi Web kancası oluştur 

### <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Olay kılavuz makalesine abone olmadan önce bunları görüntüleyebilmeniz için iletileri toplayan bir uç nokta oluşturun.

Bölümünde anlatıldığı gibi bir genel Web kancası tarafından tetiklenen bir işlev oluşturma [Genel Web kancası](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function) makalesi. Bu öğreticide **C#** kod kullanılır.

Web kancası oluşturulduktan sonra üzerine tıklayarak URL'yi kopyalayın *işlev URL'sini Al* en üstündeki bağlantıyı **Azure** portalı penceresi. Son kısmını URL gerekmez (*& ClientID = default*).

![Bir Web kancası oluştur](./media/job-state-events-cli-how-to/generic_webhook_files.png)

### <a name="validate-the-webhook"></a>Web kancası doğrula

Event Grid ile kendi Web kancası uç noktası kaydettiğinizde, uç nokta sahipliği kanıtlamak için basit bir doğrulama kodunu içeren bir POST isteği gönderir. Uygulamanızın bir doğrulama kodu geri Yankı tarafından yanıt vermesi gerekir. Event Grid doğrulamayı geçen henüz Web kancası uç noktalar için olayları teslim değil. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](https://docs.microsoft.com/azure/event-grid/security-authentication). Bu bölümde, doğrulamanın başarılı tanımlanmalıdır iki bölümden tanımlar.

#### <a name="update-the-source-code"></a>Kaynak kodunu güncelleştirme

Web kancanız oluşturduktan sonra **run.csx** dosyası tarayıcıda görüntülenir. Varsayılan kodu aşağıdaki kodla değiştirin. 

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"Webhook was triggered!");

    string jsonContent = await req.Content.ReadAsStringAsync();
    string eventGridValidation = 
        req.Headers.FirstOrDefault( x => x.Key == "Aeg-Event-Type" ).Value?.FirstOrDefault();

    dynamic eventData = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"event: {eventData}");

    if (eventGridValidation != String.Empty)
    {
        if (eventData[0].data.validationCode !=String.Empty && eventData[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent")
        {
            return req.CreateResponse(HttpStatusCode.OK, new 
            {
                validationResponse = eventData[0].data.validationCode
            });
        }
    }
    
    log.Info(jsonContent);

    return req.CreateResponse(HttpStatusCode.OK);
}
```

#### <a name="update-test-request-body"></a>Test istek gövdesi güncelleştir

Sağ tarafındaki **Azure** iki sekme gördüğünüz portalı penceresi: **dosyaları görüntüleyin** ve **Test**. **Test** sekmesini seçin. İçinde **istek gövdesi**, aşağıdaki json yapıştırın. Olduğu gibi herhangi bir değeri değiştirmenize gerek yapıştırabilirsiniz.

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2017-08-06T22:09:30.740323Z"
}
]
```

Tuşuna **Kaydet ve Çalıştır** pencerenin üst kısmındaki.

![İstek gövdesi](./media/job-state-events-cli-how-to/generic_webhook_test.png)

## <a name="register-for-the-event-grid-subscription"></a>Event Grid aboneliği için kaydolun 

Abone olduğunuz bir makalede Event Grid'e hangi olayları izlemek istediğinizi bildirmek için. Aşağıdaki örnek, oluşturduğunuz ve olay bildirimi için uç nokta olarak oluşturduğunuz Azure işlevi Web kancası URL'sini geçirir Media Services hesabına abone olur. 

Değiştirin `<event_subscription_name>` olay aboneliğiniz için benzersiz bir ada sahip. İçin `<resource_group_name>` ve `<ams_account_name>`, Media Services hesabı oluştururken kullanılan değerleri kullanın. İçin `<endpoint_URL>` uç nokta URL'nizi yapıştırın. Kaldırma *& ClientID = default* URL'den. Abone olurken bir uç nokta belirttiğinizde, Event Grid olayların bu uç noktaya yönlendirilmesini sağlar. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

Media Services hesabı kaynak kimliği değeri şuna benzer:

```
/subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amstestaccount
```

## <a name="test-the-events"></a>Olayları test etme

Bir kodlama işi çalıştırın. Örneğin, açıklandığı gibi [Stream video dosyaları](stream-files-dotnet-quickstart.md) hızlı başlangıç.

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Daha önce oluşturduğunuz Web kancası'na göz atın. Tıklayın **İzleyici** ve **Yenile**. İşin durum değişikliği olayları görmek: "Sıraya alınan", "Zamanlandı", "İşleme", "Bitti", "Error", "İptal", "İptal".  Daha fazla bilgi için [Media Services olay şemaları](media-services-event-schemas.md).

Aşağıdaki örnek, JobStateChange olay şeması gösterir:

```json
[{
  "topic": "/subscriptions/<subscription id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job id>",
  "eventType": "Microsoft.Media.JobStateChange",
  "eventTime": "2018-04-20T21:17:26.2534881",
  "id": "<id>",
  "data": {
    "previousState": "Scheduled",
    "state": "Processing"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

![Test olayları](./media/job-state-events-cli-how-to/test_events.png)

## <a name="next-steps"></a>Sonraki adımlar

[Olaylara tepki verme](reacting-to-media-services-events.md)

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
