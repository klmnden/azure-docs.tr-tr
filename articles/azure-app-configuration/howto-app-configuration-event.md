---
title: Bir web uç noktasına olayları göndermek Azure uygulama yapılandırması ayarlanıyor için öğretici | Microsoft Docs
description: Bu öğreticide, bir web uç noktası için anahtar-değer değişikliği olayları göndermek için Azure uygulama yapılandırması olay abonelikleri Ayarla öğrenin.
services: azure-app-configuration
documentationcenter: ''
author: jimmyca
manager: yegu
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 2cb9ad28a21842987f8c0f7c75151ab8c7fe6fa0
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735363"
---
# <a name="quickstart-route-azure-app-configuration-events-to-a-web-endpoint-with-azure-cli"></a>Hızlı Başlangıç: Azure uygulama yapılandırma olaylarını Azure CLI ile bir web uç noktasına yönlendirme

Azure uygulama yapılandırması kullanıcıları, anahtar-değer değiştirdiğinde yayılan iş olaylarına abone olabilirsiniz. Web kancaları, Azure işlevleri, Azure depolama kuyrukları veya tarafından desteklenen herhangi bir olay işleyici bu olayları tetikleyebilir [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/event-handlers). Bu makalede, Azure uygulama yapılandırma olaylarına abone olmak için Azure CLI'yı kullanmayı öğreneceksiniz.

Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bununla birlikte, bu makaleyi basitleştirmek için olayları iletilerin toplandığı ve görüntülendiği bir web uygulamasına gönderirsiniz.

Bu makalede açıklanan adımları tamamladıktan sonra olay verilerinin web uygulamasına gönderildiğini görürsünüz.

![Abonelik olayını görüntüleme](./media/quickstarts/event-grid/view-results.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI'nin en son sürümünü çalıştırıyorsanız gerekir (2.0.24 veya sonraki). Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. 

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur `<resource_group_name>` içinde *westus* konumu.  `<resource_group_name>` değerini kaynak grubunuz için benzersiz bir adla değiştirin.

```azurecli-interactive
az group create --name <resource_group_name> --location westus
```

## <a name="create-an-app-configuration"></a>Uygulama yapılandırması oluşturma

Değiştirin `<appconfig_name>` , uygulama yapılandırması için benzersiz bir ada sahip ve `<resource_group_name>` daha önce oluşturduğunuz kaynak grubu ile. Bir DNS adı olarak kullanıldığından adı benzersiz olmalıdır.

```azurecli-interactive
az appconfig create \
  --name <appconfig_name> \
  --location westus \
  --resource-group <resource_group_name>
```

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu hızlı başlangıcı basitleştirmek için, olay iletilerini görüntüleyin bir [önceden oluşturulmuş web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtırsınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

`<your-site-name>` değerini web uygulamanız için benzersiz bir adla değiştirin. Web uygulaması adı bir DNS girdisinin parçası olduğundan benzersiz olmalıdır.

```azurecli-interactive
$sitename=<your-site-name>

az group deployment create \
  --resource-group <resource_group_name> \
  --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
  --parameters siteName=$sitename hostingPlanName=viewerhost
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

Şu anda iletilerin görüntülenmediği siteyi görüyor olmalısınız.

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-your-app-configuration"></a>Uygulama yapılandırmanız için abone olun

Event Grid’e hangi olayları izlemek istediğinizi ve bu olayların nereye gönderileceğini bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, oluşturduğunuz ve URL'sini web uygulamanızdan olay bildirimi için uç nokta olarak geçirir. Uygulama yapılandırması abone olur. Olay aboneliğiniz için `<event_subscription_name>` öğesini bir ad ile değiştirin. `<resource_group_name>` ve `<appconfig_name>` için daha önce oluşturduğunuz değerleri kullanın.

Web uygulamanızın uç noktası `/api/updates/` sonekini içermelidir.

```azurecli-interactive
appconfigId=$(az appconfig show --name <appconfig_name> --resource-group <resource_group_name> --query id --output tsv)
endpoint=https://$sitename.azurewebsites.net/api/updates

az eventgrid event-subscription create \
  --resource-id $appconfigId \
  --name <event_subscription_name> \
  --endpoint $endpoint
```

Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Göz simgesini seçerek olay verilerini genişletin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Web uygulaması aboneliği doğrulamak için kod içerir.

![Abonelik olayını görüntüleme](./media/quickstarts/event-grid/view-subscription-event.png)

## <a name="trigger-an-app-configuration-event"></a>Uygulama yapılandırması olay tetikleme

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. Kullanarak bir anahtar-değer oluşturma `<appconfig_name>` daha önce gelen.

```azurecli-interactive
az appconfig kv set --name <appconfig_name> --key Foo --value Bar --yes
```

Olayı tetiklediniz ve Event Grid, iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Az önce gönderdiğiniz olayı görmek için web uygulamanızı görüntüleyin.

```json
[{
  "id": "deb8e00d-8c64-4b6e-9cab-282259c7674f",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/eventDemoGroup/providers/microsoft.appconfiguration/configurationstores/{appconfig-name}",
  "subject": "https://{appconfig-name}.azconfig.io/kv/Foo",
  "data": {
    "key": "Foo",
    "etag": "a1LIDdNEIV6wCnfv3xaip7fMXD3"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T18:59:54Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]

```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu uygulama yapılandırması ve olay aboneliği ile çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutu kullanarak bu makalede oluşturduğunuz kaynakları silin.

`<resource_group_name>` değerini yukarıda oluşturduğunuz kaynak grubuyla değiştirin.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Sonraki adımlar

Konu ve olay abonelikleri oluşturma işlemini artık bildiğinize göre anahtar-değer olayları ve ne Event Grid yapmanıza yardımcı olabileceğini hakkında daha fazla bilgi:

- [Anahtar-değer olaylara tepki verme](concept-app-configuration-event.md)
- [Event Grid Hakkında](../event-grid/overview.md)