---
title: Azure Media Services olayları özel web uç noktasına rota | Microsoft Docs
description: Media Services iş durum değişiklik olayı için abone olmak için Azure olay kılavuzunu kullanın.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 6a098f43819bb6581b2c5978fbcc4a378a8514c1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638509"
---
# <a name="route-azure-media-services-events-to-a-custom-web-endpoint-using-cli"></a>CLI kullanarak bir özel web uç noktası için rota Azure Media Services olayları

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Azure Media Services iş durum değişikliği olayları için abone olmak için Azure CLI kullanın ve sonuçları görüntülemek için olay tetikler. 

Normalde olayları olaya yanıt veren bir uç noktaya (web kancası veya Azure İşlevi gibi) gönderirsiniz. Bu öğretici nasıl oluşturulacağı ve bir Web kancası ayarlamak gösterir.

Bu makalede açıklanan adımları tamamladıktan sonra olay verilerinin bir uç noktaya gönderildiğini görürsünüz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](http://portal.azure.com) oturum açın ve **CloudShell** başlatarak sonraki adımlarda gösterildiği gibi CLI komutlarını yürütün.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI sürüm 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

Media Services hesap adını, depolama ad ve kaynak adı için kullanılan değerleri anımsaması emin olun.

## <a name="enable-event-grid-resource-provider"></a>Olay kılavuz kaynak sağlayıcısını etkinleştir

Yapmanız gereken ilk şey, olay kılavuz kaynak sağlayıcısı, aboneliğinizde etkin olduğundan emin olun ' dir. 

İçinde **Azure** portal aşağıdakileri yapın:

1. Abonelikleri gidin.
2. Aboneliğinizi seçin.
3. Ayarlar altında kaynak sağlayıcılarını seçin.
4. "EventGrid" arayın.
5. Olay kılavuz kayıtlı olduğundan emin olun. Aksi takdirde, basın **kaydetmek** düğmesi.  

## <a name="create-a-generic-azure-function-webhook"></a>Genel bir Azure işlevi Web kancası oluşturma 

### <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Olay kılavuz makalesine abone önce bunları görüntüleyebilmeniz iletileri toplayan bir uç nokta oluşturun.

Bölümünde açıklandığı gibi genel bir Web kancası tarafından tetiklenen bir işlev oluşturun [Genel Web kancası](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function) makalesi. Bu öğreticide **C#** kodu kullanılır.

Web kancası oluşturulduktan sonra tıklayarak URL'yi kopyalayın *Al işlevi URL* en üstündeki bağlantı **Azure** portal penceresi. URL son parçası gerekmez (*& ClientID = varsayılan*).

![Bir Web kancası oluşturma](./media/job-state-events-cli-how-to/generic_webhook_files.png)

### <a name="validate-the-webhook"></a>Web kancası doğrula

Kendi Web kancası bitiş noktası içeren olay kılavuz kaydettiğinizde, uç nokta sahipliği kanıtlamak için basit bir doğrulama kodu içeren bir POST isteği gönderir. Uygulamanızın geri doğrulama kodu Yankı tarafından yanıt vermesi gerekir. Olay kılavuz doğrulamayı geçen henüz Web kancası Uç noktalara olayları teslim değil. Daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](https://docs.microsoft.com/azure/event-grid/security-authentication). Bu bölümde, doğrulamanın başarılı tanımlanmalıdır iki bölümden tanımlar.

#### <a name="update-the-source-code"></a>Kaynak kodunu güncelleştirin

Web kancası oluşturduktan sonra **run.csx** dosyası tarayıcıda görünür. Varsayılan kodu aşağıdaki kodla değiştirin. 

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

Sağ tarafındaki **Azure** iki sekme gördüğünüz portal penceresinin: **dosyaları görüntüleyebilir** ve **Test**. **Test** sekmesini seçin. İçinde **istek gövdesinde**, aşağıdaki json yapıştırın. Tüm değerleri değiştirmek için gerekli olduğu gibi yapıştırabilirsiniz.

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

Tuşuna **kaydedip çalıştırın** pencerenin üstündeki.

![İstek gövdesi](./media/job-state-events-cli-how-to/generic_webhook_test.png)

## <a name="register-for-the-event-grid-subscription"></a>Olay kılavuz abonelik için kaydolun 

İzlemek istediğiniz olayları olay kılavuz bildirmek için makaleye abone olun. Aşağıdaki örnek, Media Services hesabı oluşturulur ve olay bildirimi için uç noktaya olarak oluşturulan Azure işlevi Web kancası URL'si geçirir abone olur. 

Değiştir `<event_subscription_name>` olay aboneliğiniz için benzersiz bir ada sahip. `<resource_group_name>` ve `<ams_account_name>` için daha önce oluşturduğunuz değerleri kullanın.  İçin `<endpoint_URL>` , uç nokta URL'sini yapıştırın. Kaldırma *& ClientID = varsayılan* URL'den. Abone olurken bir uç nokta belirttiğinizde, Event Grid olayların bu uç noktaya yönlendirilmesini sağlar. 

```cli
amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $amsResourceId \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

Media Services hesabı kaynak kimliği değeri şuna benzer:

/Subscriptions/81212121-2f4f-4b5d-a3dc-ba0015515f7b/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amstestaccount

## <a name="test-the-events"></a>Olayları test etme

Bir kodlama işi çalıştırın. Örneğin, açıklandığı gibi [akış video dosyaları](stream-files-dotnet-quickstart.md) hızlı başlangıç.

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Daha önce oluşturduğunuz Web kancası göz atın. Tıklatın **İzleyici** ve **yenileme**. İş durumu değişir olayları Bkz: "Sıraya alınan", "Zamanlanmış", "İşlem", "Tamamlandı", "Error", "İptal", "İptal".  Daha fazla bilgi için bkz: [Media Services olay şemaları](media-services-event-schemas.md).

Örneğin:

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu depolama hesabı ve olay aboneliğiyle çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutu kullanarak bu makalede oluşturduğunuz kaynakları silin.

`<resource_group_name>` değerini yukarıda oluşturduğunuz kaynak grubuyla değiştirin.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Sonraki adımlar

[Olaylarına tepki](reacting-to-media-services-events.md)## Ayrıca bkz.

## <a name="see-also"></a>Ayrıca bkz.

[CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
