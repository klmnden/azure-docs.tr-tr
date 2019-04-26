---
title: İzleme portalını kullanarak Event Grid ile Azure Media Services olaylarını | Microsoft Docs
description: Bu makalede, Event Grid için Azure Media Services olaylarını izlemek için abone olunacağı gösterilmektedir.
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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: d4592c93cb7969c45a107d7365a1b9dabf11f412
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326539"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak Event Grid ile Media Services olaylarını izleme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu hizmetin kullandığı [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için. Media Services olaylarını verilerinizdeki değişiklikleri yanıtlamak için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.Media" ile başladığından bir Media Services olayı belirleyebilirsiniz. Daha fazla bilgi için [Media Services olay şemaları](media-services-event-schemas.md).

Bu makalede, Azure Media Services hesabınız için olaylara abone için Azure portalını kullanın. Ardından, sonucu görüntülemek için olayları tetikleyin. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Makalede, biz toplayan ve iletileri görüntüleyen bir web uygulaması için olayları gönderirsiniz.

İşiniz bittiğinde, olay verilerinin web uygulamasına gönderildiğini görürsünüz.

## <a name="prerequisites"></a>Önkoşullar 

* Etkin bir Azure aboneliğine sahip.
* [Bu hızlı başlangıçta](create-account-cli-quickstart.md) açıklandığı gibi yeni bir Azure Media Services hesabı oluşturun.

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Media Services hesabı için olaylara abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu makalede, dağıttığınız bir [önceden oluşturulmuş bir web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) , olay iletileri görüntüler. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

"Azure Event Grid Görüntüleyicisi" sitesine geçerseniz, henüz hiçbir olay sahip bakın.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="subscribe-to-media-services-events"></a>Media Services olaylarına abone olma

Event Grid’e hangi olayları izlemek istediğinizi ve olayların nereye gönderileceğini bildirmek için bir konuya abone olursunuz.

1. Portalda, Media Services hesabınızı seçip **olayları**.
1. Olayları görüntüleyici uygulamanıza göndermek için uç noktada bir web kancası kullanın. 

   ![Web kancasını seçme](./media/monitor-events-portal/select-web-hook.png)

1. Olay aboneliği Media Services hesabınız için değerlerle doldurulmuş. 
1. İçin seçin 'Web kancası' **uç noktası türü**.
1. Bu konu başlığında, biz bırakın **tüm olay türlerine abone** işaretli. Ancak, bunu ve belirli olay türleri için Filtre kutusunun işaretini kaldırabilirsiniz. 
1. Tıklayarak **bir uç nokta seçin** bağlantı.

    Web kancası uç noktası için web uygulamanızın URL'sini girin ve ana sayfa URL'sine `api/updates` ekleyin. 

1. Tuşuna **seçimi onaylamanız**.
1. **Oluştur**’a basın.
1. Aboneliğinize bir ad verin.

   ![Günlükleri seçme](./media/monitor-events-portal/create-subscription.png)

1. Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. 

    Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Uç nokta ayarlamak zorunda `validationResponse` için `validationCode`. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](../../event-grid/security-authentication.md). Abonelik doğrulama biçimini görmek için web uygulaması kodunu görüntüleyebilirsiniz.

Şimdi, Event Grid'in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim.

## <a name="send-an-event-to-your-endpoint"></a>Uç noktanıza olay gönderme

Media Services hesabı için olaylara bir kodlama işi tetikleyebilirsiniz. İzleyebileceğiniz [Bu hızlı başlangıçta](stream-files-dotnet-quickstart.md) dosya kodlamak ve olayları göndermeye başlayın. Tüm olaylara abone, aşağıdakine benzer bir ekran görürsünüz:

> [!TIP]
> Göz simgesini seçerek olay verilerini genişletin. Tüm olayları görüntülemek istiyorsanız, sayfa yenilenmez.

![Abonelik olayını görüntüleme](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama ve akışını](stream-files-tutorial-with-api.md)
