---
title: "Azure Event hubs'a Azure API Management olaylarını günlüğe kaydedecek şekilde nasıl | Microsoft Docs"
description: "Azure Event hubs'a Azure API Management'te olayları günlüğe kaydetme hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 066f151aa96b3a57c86515411ba05a982c10aa5f
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Azure Event hubs'a Azure API Management'te olayları günlüğe kaydetme hakkında
Azure Event Hubs, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki verileri işleyip analiz edebilmeniz için saniye başına milyonlarca olayı işleyebilen ileri düzeyde ölçeklenebilir bir veri alım sistemidir. Event Hubs bir olay komut zincirinin "ön kapı" olarak görev yapan ve veriler bir event hub'ına toplandıktan sonra dönüştürülebilir ve tüm gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak saklanır. Event Hubs olay akışı üretimlerini bu olayların tüketilmesinden ayırır, böylece olay tüketicileri olaylara kendi zamanlamalarında erişebilir.

Bu makalede bir yardımcı olan [olay hub'ları ile Azure API Management tümleştirmek](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video ve Azure Event Hubs kullanarak API Management olaylarını günlüğe kaydedecek şekilde açıklar.

## <a name="create-an-azure-event-hub"></a>Bir Azure olay hub'ı Oluştur
Yeni bir olay hub'ı oluşturmak için oturum için açma [Klasik Azure portalı](https://manage.windowsazure.com) tıklatıp **yeni**->**uygulama hizmetleri**->**hizmet veri yolu**  -> **Olay hub'ı**->**hızlı Oluştur**. Bir olay hub'ı adını girin, bölge, bir abonelik seçin ve bir ad seçin. Daha önce bir ad alanı oluşturmadıysanız bir ad yazarak bir tane oluşturabilirsiniz **Namespace** metin kutusu. Tüm özellikleri yapılandırıldıktan sonra tıklatın **yeni bir olay hub'ı oluşturma** olay hub'ı oluşturmak için.

![Olay hub'ı Oluştur][create-event-hub]

Ardından, gitmek **yapılandırma** sekmesinde yeni olay hub'ınız için ve iki oluşturmak **paylaşılan erişim ilkeleri**. Adı ilk **gönderme** ve verin **Gönder** izinleri.

![Gönderme İlkesi][sending-policy]

Ad ikinci **alma**, bu verin **dinleme** izinleri ve tıklatın **kaydetmek**.

![İlke alma][receiving-policy]

Her paylaşılan erişim ilkesinin uygulamaların olayları olay hub'ı gelen ve giden gönderip almasına izin verir. Bu ilkeler için bağlantı dizelerini erişmek için gidin **Pano** sekmesini tıklatın ve olay hub'ı **bağlantı bilgilerini**.

![Bağlantı dizesi][event-hub-dashboard]

**Gönderme** bağlantı dizesi, olaylar, oturum açarken kullanılır ve **alma** bağlantı dizesi, olayların Event Hub'ından indirirken kullanılır.

![Bağlantı dizesi][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Bir API Management Günlükçü oluşturma
Bir Event Hub sahip olduğunuza göre sonraki adıma yapılandırmaktır bir [Günlükçü](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) , API Management hizmeti bu olayları Event Hub'ına oturum açabilir.

API Management günlükçüleri kullanılarak yapılandırılmış olan [API Management REST API](http://aka.ms/smapi). REST API ilk kez kullanmadan önce gözden [Önkoşullar](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) ve olduğundan emin olun [REST API erişim etkin](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Günlükçü oluşturmak için aşağıdaki URL şablonu kullanarak bir HTTP PUT isteği oluşturun.

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* Değiştir `{your service}` API Management hizmet örneği adı.
* Değiştir `{new logger name}` yeni Günlükçü için istenen adı ile. Yapılandırdığınızda bu adı başvurur [günlük eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) İlkesi

Aşağıdaki üst bilgiler isteği ekleyin.

* Content-Type: uygulama/json
* Yetkilendirme: SharedAccessSignature 58...
  * Oluşturma yönergeleri için `SharedAccessSignature` bkz [Azure API Management REST API kimlik doğrulama](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Aşağıdaki şablonu kullanarak istek gövdesini belirtin.

```json
{
  "loggertype" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `loggertype`ayarlanmalıdır `AzureEventHub`.
* `description`günlükçünün isteğe bağlı bir açıklama sağlar ve isterseniz sıfır uzunluğunda bir dize olabilir.
* `credentials`içeren `name` ve `connectionString` Azure olay hub'ınızın.

Yaptığınızda isteği durum kodu Günlükçü oluşturduysanız `201 Created` döndürülür.

> [!NOTE]
> Diğer olası dönüş kodları ve bunların nedenleri için bkz: [Günlükçü oluşturma](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Görmek için nasıl listesi, güncelleştirme ve silme, bkz: gibi diğer işlemleri [Günlükçü](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) varlık belgeleri.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Günlük eventhubs ilkeleri yapılandırma
API Management'te, Günlükçü yapılandırıldıktan sonra günlük eventhubs ilkelerinizi istenen olaylarını günlüğe kaydedecek şekilde yapılandırabilirsiniz. Günlük eventhubs ilkesi gelen ilke bölümünü veya giden ilke bölümünü kullanılabilir.

İlkeleri yapılandırmak için oturum için açma [Azure portal](https://portal.azure.com)API Management hizmetiniz gidin ve tıklayın **yayımcı portalına** yayımcı portalına erişmek için.

![Yayımcı portalı][publisher-portal]

Tıklatın **ilkeleri** sol API Management menüsünde istenen ürün ve API seçin ve tıklayın **ilke Ekle**. Bu örnekte, biz bir ilkeye eklediğiniz **Echo API'si** içinde **sınırsız** ürün.

![İlke ekleme][add-policy]

İmleç Konumlandır `inbound` İlkesi bölümüne ve tıklatın **EventHub günlüğüne** eklemek için ilke `log-to-eventhub` ilke deyimi şablonu.

![İlke düzenleyicisi][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

Değiştir `logger-id` önceki adımda yapılandırdığınız API Management günlükçü adı.

Değeri olarak bir dize döndürür. herhangi bir ifade kullanabileceğiniz `log-to-eventhub` öğesi. Bu örnekte, tarih ve saat, hizmet adı, istek kimliği, istek IP adresi ve işlem adı içeren bir dize günlüğe kaydedilir.

Tıklatın **kaydetmek** güncelleştirilmiş ilke yapılandırmasını kaydetmek için. Kaydedilmeden hemen ilkesi etkindir ve belirlenen olay Hub'ına olayları kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Event Hubs hakkında daha fazla bilgi edinin
  * [Azure Event Hubs ile çalışmaya başlama](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Management ve Event Hubs ile tümleştirme hakkında daha fazla bilgi edinin
  * [Günlükçü varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [Günlük eventhub ilke başvurusu](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Azure API Management, olay hub'ları ve Runscope Apı'lerinizi izleme](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Bir videosu izleme
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Integrate-Azure-API-Management-with-Event-Hubs/player]
>
>

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
