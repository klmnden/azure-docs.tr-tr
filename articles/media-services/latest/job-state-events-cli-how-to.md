---
title: CLI kullanarak Event Grid ile Azure Media Services olaylarını izleme | Microsoft Docs
description: Bu makalede, Event Grid için Azure Media Services olaylarını izlemek için abone olunacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 11/09/2018
ms.author: juliako
ms.openlocfilehash: f6243bbc21466361aed7cbb7193f3a7b7c7e539f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322694"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-cli"></a>Oluşturma ve Azure CLI kullanarak Event Grid ile Media Services olaylarını izleme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu hizmetin kullandığı [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için. Media Services olaylarını verilerinizdeki değişiklikleri yanıtlamak için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.Media" ile başladığından bir Media Services olayı belirleyebilirsiniz. Daha fazla bilgi için [Media Services olay şemaları](media-services-event-schemas.md).

Bu makalede, Azure Media Services hesabınız için olaylara abone için Azure CLI'yı kullanın. Ardından, sonucu görüntülemek için olayları tetikleyin. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bu makalede, toplar ve iletileri görüntüleyen bir web uygulaması için olayları gönderirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- Yükleyin ve bu makalede Azure CLI 2.0 veya sonraki bir sürüm gerektirir, CLI'yı yerel olarak kullanın. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda tüm [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) komutlar Azure Cloud Shell içinde çalışır. CLI'yi yerel olarak kullanmak için önerilir.

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

    Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun.

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Media Services hesabı için olaylara abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu makalede, dağıttığınız bir [önceden oluşturulmuş bir web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) , olay iletileri görüntüler. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

"Azure Event Grid Görüntüleyicisi" sitesine geçerseniz, henüz hiçbir olay sahip bakın.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliğini sağlayın. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’e giderek erişimine sahip olduğunuz aboneliklerin listesini görebilirsiniz.

```azurecli
az account set --subscription mySubscriptionId
```

## <a name="subscribe-to-media-services-events"></a>Media Services olaylarına abone olma

Abone olduğunuz bir makalede Event Grid'e hangi olayları izlemek istediğinizi bildirmek için. Aşağıdaki örnek, oluşturduğunuz ve URL'sini olay bildirimi için uç nokta olarak oluşturduğunuz Web sitesinden geçirir Media Services hesabına abone olur. 

Değiştirin `<event_subscription_name>` olay aboneliğiniz için benzersiz bir ada sahip. İçin `<resource_group_name>` ve `<ams_account_name>`, Media Services hesabı oluştururken kullanılan değerleri kullanın. İçin `<endpoint_URL>`, web uygulamanızın URL'sini sağlayın ve ekleme `api/updates` giriş sayfası URL'si. Abone olurken bir uç noktası belirterek, Event Grid olayların Bu uç noktaya yönlendirilmesini sağlar. 

1. Kaynak kimliğini alın

    ```azurecli
    amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)
    ```

    Örneğin:

    ```
    amsResourceId=$(az ams account show --name amsaccount --resource-group amsResourceGroup --query id --output tsv)
    ```

2. Olaylara abone olma

    ```azurecli
    az eventgrid event-subscription create \
    --resource-id $amsResourceId \
    --name <event_subscription_name> \
    --endpoint <endpoint_URL>
    ```

    Örneğin:

    ```
    az eventgrid event-subscription create --resource-id $amsResourceId --name amsTestEventSubscription --endpoint https://amstesteventgrid.azurewebsites.net/api/updates/
    ```    

    > [!TIP]
    > Doğrulama el sıkışması uyarısı alabilirsiniz. Birkaç dakika bekleyin ve anlaşma doğrulamalıdır.

Şimdi, Event Grid'in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim.

## <a name="send-an-event-to-your-endpoint"></a>Uç noktanıza olay gönderme

Media Services hesabı için olaylara bir kodlama işi tetikleyebilirsiniz. İzleyebileceğiniz [Bu hızlı başlangıçta](stream-files-dotnet-quickstart.md) dosya kodlamak ve olayları göndermeye başlayın. 

Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Uç nokta ayarlamak zorunda `validationResponse` için `validationCode`. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](../../event-grid/security-authentication.md). Abonelik doğrulama biçimini görmek için web uygulaması kodunu görüntüleyebilirsiniz.

> [!TIP]
> Göz simgesini seçerek olay verilerini genişletin. Tüm olayları görüntülemek istiyorsanız, sayfa yenilenmez.

![Abonelik olayını görüntüleme](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama ve akışını](stream-files-tutorial-with-api.md)

