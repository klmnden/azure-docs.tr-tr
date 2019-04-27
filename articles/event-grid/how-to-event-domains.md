---
title: Azure Event Grid olay etki alanları ile konularda büyük kümelerini yönetme
description: Azure Event Grid konuları büyük kümeleri yönetmek ve olayları yayımlarsınız gösterilmektedir olay etki alanlarını kullanma.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 01/17/2019
ms.openlocfilehash: c49044d8bd96efb7e86cf54509c32033900be305
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561779"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Konuları Yönet ve olay etki alanları kullanarak olay yayımlama

Bu makale nasıl yapılır:

* Bir Event Grid etki alanı oluşturma
* Olay ızgarası konu başlıkları için abone olun
* Anahtarları Listele
* Bir etki alanına olayları yayımlama

Olay etki alanları hakkında bilgi edinmek için [Event Grid konuları yönetmek için olay etki alanlarını anlamak](event-domains.md).

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Önizleme özelliğini yükle

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-event-domain"></a>Bir olay etki alanı oluşturma

Konu büyük kümesini yönetmek için bir olay etki alanı oluşturun.

Azure CLI için şunu kullanın:

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name> \
  -l <location>
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain-name> `
  -Location <location>
```

Oluşturma başarılı aşağıdaki değerleri döndürür:

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

Not `endpoint` ve `id` etki alanını yönetmek ve olayları yayımlamak için gerekli.

## <a name="manage-access-to-topics"></a>Konuları erişimi yönetme

Konuları Yönetimi aracılığıyla yapılır [rol ataması](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli). Rol ataması, belirli bir kapsamda yetkili kullanıcıların Azure kaynakları üzerinde işlemler sınırlamak için rol tabanlı erişim denetimi kullanır.

Event Grid, belirli bir kullanıcı bir etki alanı içinde çeşitli konularda erişimi atamak için kullanabileceğiniz iki yerleşik role sahiptir. Bu roller `EventGrid EventSubscription Contributor (Preview)`, oluşturma ve abonelikleri silinmesini sağlayan ve `EventGrid EventSubscription Reader (Preview)`, olay abonelikleri listesi için yalnızca sağlar.

Aşağıdaki Azure CLI komutu sınırlar `alice@contoso.com` oluşturma ve olay abonelikleri yalnızca konuyla ilgili silme için `demotopic1`:

```azurecli-interactive
az role assignment create \
  --assignee alice@contoso.com \
  --role "EventGrid EventSubscription Contributor (Preview)" \
  --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```

Aşağıdaki PowerShell komutu sınırlar `alice@contoso.com` oluşturma ve olay abonelikleri yalnızca konuyla ilgili silme için `demotopic1`:

```azurepowershell-interactive
New-AzureRmRoleAssignment `
  -SignInName alice@contoso.com `
  -RoleDefinitionName "EventGrid EventSubscription Contributor (Preview)" `
  -Scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```

Event Grid işlemleri için erişimi yönetme hakkında daha fazla bilgi için bkz. [Event Grid güvenliğini ve kimlik doğrulaması](./security-authentication.md).

## <a name="create-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri oluşturma

Event Grid hizmet otomatik olarak oluşturur ve etki alanı tabanlı bir etki alanı konu için bir olay aboneliği oluşturmak için çağrıda karşılık gelen konusunda yönetir. Bir etki alanındaki bir konu oluşturmak için ayrı hiçbir adım yok. Benzer şekilde, bir konu için son olay aboneliği silindiğinde, konu de silinir.

Bir etki alanı içindeki bir konuya abone olma tüm diğer Azure kaynakları için aynıdır. Kaynak kaynak kimliği için daha önce etki alanı oluştururken, döndürülen olay etki alanı kimliği belirtin. Abone olmak istediğiniz konu belirtmek için `/topics/<my-topic>` sonuna kadar kaynak kaynak kimliği Etki alanındaki tüm olayları alır bir etki alanı kapsamı olay aboneliği oluşturmak için herhangi bir konuda belirtmeden olay etki alanı kimliği belirtin.

Genellikle, kullanıcı, erişim içinde önceki bölümde abonelik oluşturacak izni. Bu makaleyi basitleştirmek için bir abonelik oluşturun. 

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --source-resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" \
  --endpoint https://contoso.azurewebsites.net/api/updates
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
New-AzureRmEventGridSubscription `
  -ResourceId "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" `
  -EventSubscriptionName <event-subscription> `
  -Endpoint https://contoso.azurewebsites.net/api/updates
```

Olaylarınızı abone olmak için bir test uç noktası gerekiyorsa, her zaman dağıtabileceğiniz bir [önceden oluşturulmuş bir web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) , gelen olayları görüntüler. Olaylarınızı, test sitenize gönderebilirsiniz `https://<your-site-name>.azurewebsites.net/api/updates`.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Bir konu için ayarladığınız izinler, Azure Active Directory'de depolanır ve açıkça silinmelidir. Bir olay aboneliği siliniyor, bir konu üzerinde yazma erişimi varsa olay aboneliği oluşturmak için bir kullanıcı erişimi iptal olmaz.


## <a name="publish-events-to-an-event-grid-domain"></a>Olayları bir Event Grid etki alanına yayımlama

Bir etki alanına olayları yayımlama aynıdır [özel bir konu başlığında yayımlamaya](./post-to-custom-topic.md). Ancak, özel konuya yayımlama yerine, tüm olayları ve etki alanı uç noktasına yayımlayın. JSON olay verilerini, olayları gitmek istediğiniz konu belirtin. Aşağıdaki olaylar dizisi olay ile sonuçlanır `"id": "1111"` konuya `demotopic1` olay ile çalışırken `"id": "2222"` konu başlığına gönderilen `demotopic2`:

```json
[{
  "topic": "demotopic1",
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
  "topic": "demotopic2",
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

Azure CLI ile bir etki alanı uç noktası almak için kullanın

```azurecli-interactive
az eventgrid domain show \
  -g <my-resource-group> \
  -n <my-domain>
```

Bir etki alanı için anahtarları almak için kullanın:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

PowerShell ile bir etki alanı uç noktası almak için kullanın

```azurepowershell-interactive
Get-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```

Bir etki alanı için anahtarları almak için kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridDomainKey `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```

Ve etki alanınızı Event Grid, olayları yayımlamak için bir HTTP POST yapma sık kullandığınız yöntemi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Olay etki alanları ve neden yararlı üst düzey kavramları hakkında daha fazla bilgi için bkz. [olay etki alanlarına kavramsal genel bakış](event-domains.md).
