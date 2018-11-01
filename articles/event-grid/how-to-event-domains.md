---
title: Azure Event Grid konuları büyük kümeleri yönetmek ve olayları olay etki alanı kullanarak bunları yayımlama
description: Oluşturup Azure Event Grid konuları yönetmek ve olayları olay etki alanı kullanarak bunları yayımlama işlemi gösterilmektedir.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 48a5356b03e38e864ba76f048febdb0b040893f5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634313"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Konuları Yönet ve olay etki alanları kullanarak olay yayımlama

Bu makale nasıl yapılır:

* Bir Event Grid etki alanı oluşturma
* Konularına abone olma
* Anahtarları Listele
* Bir etki alanına olayları yayımlama

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-event-domain"></a>Bir olay etki alanı oluşturma

Aracılığıyla bir olay etki alanı oluşturma yapılabilir `eventgrid` uzantısı [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Bir etki alanı oluşturduktan sonra konu büyük kümesini yönetmek için kullanabilirsiniz.

```azurecli-interactive
# if you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name>
  -l <location>
```

Oluşturma başarılı aşağıdaki döndürür:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Not `endpoint` ve `id` gibi etki alanı olayları yayımlamak için gerekir.

## <a name="create-topics-and-subscriptions"></a>Konuları ve abonelikleri oluşturma

Event Grid hizmet otomatik olarak oluşturur ve etki alanı tabanlı bir etki alanı konu için bir olay aboneliği oluşturmak için çağrıda karşılık gelen konusunda yönetir. Bir etki alanındaki bir konu oluşturmak için ayrı hiçbir adım yok. Benzer şekilde, bir konu için son olay aboneliği silindiğinde, konu de silinir.

Bir etki alanı içindeki bir konuya abone olma tüm diğer Azure kaynakları için aynıdır:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/<my-topic>" \
  --endpoint https://contoso.azurewebsites.net/api/f1?code=code
```

Belirtilen kaynak kimliği, daha önce etki alanı oluştururken, döndürülen aynı kimliğidir. Abone olmak istediğiniz konu belirtmek için `/topics/<my-topic>` sonuna kadar kaynak kimliği.

Etki alanındaki tüm olayları alır bir etki alanı kapsamı olay aboneliği oluşturmak için etki alanı olarak vermek `resource-id` herhangi bir konuda örneğin belirtmeden `/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>`.

Olaylarınızı abone olmak için bir test uç noktası gerekiyorsa, her zaman dağıtabileceğiniz bir [önceden oluşturulmuş bir web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) , gelen olayları görüntüler. Olaylarınızı, test sitenize gönderebilirsiniz `https://<your-site-name>.azurewebsites.net/api/updates`.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Bir konu için ayarladığınız izinler, Azure Active Directory'de depolanır ve açıkça silinmelidir. Bir olay aboneliği siliniyor, bir konu üzerinde yazma erişimi varsa olay aboneliği oluşturmak için bir kullanıcı erişimi iptal olmaz.

## <a name="manage-access-to-topics"></a>Konuları erişimi yönetme

Konuları Yönetimi aracılığıyla yapılır [rol ataması](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-cli). Rol ataması, belirli bir kapsamda yetkili kullanıcılara Azure kaynaklarına işlemleri sınırlandırmak için rol tabanlı erişim denetimi kullanır.

Event Grid, belirli bir kullanıcı bir etki alanı içinde çeşitli konularda erişimi atamak için kullanabileceğiniz iki yerleşik role sahiptir. Bu roller `EventGrid EventSubscription Contributor (Preview)`, oluşturma ve abonelikleri silinmesini sağlayan ve `EventGrid EventSubscription Reader (Preview)`, olay abonelikleri listesi için yalnızca sağlar.

Aşağıdaki komutu sınırlandırır `alice@contoso.com` oluşturma ve olay abonelikleri yalnızca konuyla ilgili silme için `foo`:

```azurecli-interactive
az role assignment create --assignee alice@contoso.com --role "EventGrid EventSubscription Contributor (Preview)" --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/foo
```

Bkz: [Event Grid güvenliğini ve kimlik doğrulaması](./security-authentication.md) hakkında daha fazla bilgi:

* Yönetim erişim denetimi
* İşlem türleri
* Özel rol tanımları oluşturma

## <a name="publish-events-to-an-event-grid-domain"></a>Olayları bir Event Grid etki alanına yayımlama

Bir etki alanına olayları yayımlama aynıdır [özel bir konu başlığında yayımlamaya](./post-to-custom-topic.md). Tek fark, her bir olay gitmek istediğiniz konu belirtmeniz gerekiyor. Aşağıdaki olaylar dizisi olay ile sonuçlanır `"id": "1111"` konuya `foo` olay ile çalışırken `"id": "2222"` konu başlığına gönderilen `bar`:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Almak için bir etki alanı için anahtarları kullanın:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

Ve Event Grid etki alanınıza olaylarınızı yayımlamak için bir HTTP POST yapma sık kullandığınız yöntemi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Olay etki alanları ve neden yararlı üst düzey kavramları hakkında daha fazla bilgi için bkz. [olay etki alanlarına kavramsal genel bakış](./event-domains.md).