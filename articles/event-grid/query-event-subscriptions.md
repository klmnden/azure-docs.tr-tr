---
title: Sorgu Azure olay kılavuz abonelikleri
description: Azure olay kılavuz abonelikleri listesinde açıklar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: tomfitz
ms.openlocfilehash: 2b46cde4a352e647ee97669f116a6c1926879fa0
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34302424"
---
# <a name="query-event-grid-subscriptions"></a>Olay kılavuz abonelikleriyle sorgusu 

Bu makalede, Azure aboneliğinizde olay kılavuz abonelikleri listesinde açıklar. Var olan olay kılavuz aboneliklerinizi sorgularken farklı türde abonelik anlamak önemlidir. Almak istediğiniz aboneliği türüne göre farklı parametreler belirtin.

## <a name="resource-groups-and-azure-subscriptions"></a>Kaynak grupları ve Azure abonelikleri

Azure Abonelikleriniz ve kaynak gruplarınız Azure kaynaklarını değil. Bu nedenle, olay kılavuz abonelikleri kaynak gruplarına veya Azure abonelikleri olay kılavuz abonelikleri Azure kaynakları için aynı özellikleri yok. Kaynak grupları için olay kılavuz abonelikleri ya da Azure abonelik genel olarak kabul edilir.

Olay kılavuz abonelikleri Azure aboneliği ve kaynak gruplarına almak için herhangi bir parametre sağlamanız gerekmez. Sorgulamak istediğiniz Azure aboneliği seçtiğinizden emin olun. Aşağıdaki örnekler özel konular veya Azure kaynakları için olay kılavuz abonelikler elde etmezsiniz.

Azure CLI için şunu kullanın:

```azurecli-interactive
az account set -s "My Azure Subscription"
az eventgrid event-subscription list
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Set-AzureRmContext -Subscription "My Azure Subscription"
Get-AzureRmEventGridSubscription
```

Bir Azure aboneliğine yönelik kılavuz denetlemesi almak için konu türüne göre sağlamak **Microsoft.Resources.Subscriptions**.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.Subscriptions"
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.Subscriptions"
```

Olay kılavuz abonelikleri Azure aboneliği içindeki tüm kaynak gruplarında almak için konu türüne göre sağlamak **Microsoft.Resources.ResourceGroups**.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.ResourceGroups"
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.ResourceGroups"
```

Belirtilen kaynak grubu için olay kılavuz abonelikleri almak için parametre olarak kaynak grubu adını sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --resource-group myResourceGroup
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -ResourceGroupName myResourceGroup
```

## <a name="custom-topics-and-azure-resources"></a>Özel konular ve Azure kaynakları

Olay kılavuz özel Azure kaynaklarını konulardır. Bu nedenle, olay kılavuz abonelikleri özel konular ve Blob storage hesabı gibi diğer kaynaklar için aynı şekilde sorgu. Olay kılavuz abonelikleri özel konular için almak için kaynak tanımlamak veya kaynak konumunu tanımlamak parametreler sağlamanız gerekir. Bu, Azure aboneliğinizin arasında kapsamlı sorgu olay kılavuz abonelikleri kaynaklar için mümkün değil.

Olay kılavuz abonelikleri özel konular ve diğer kaynakları için bir konumda almak için konumun adını sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --location westus2
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -Location westus2
```

Özel konular için abonelikler için bir konum almak için konumu ve konu türüne göre sağlayın **Microsoft.EventGrid.Topics**.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.EventGrid.Topics" --location "westus2"
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.EventGrid.Topics" -Location westus2
```

Depolama hesapları abonelikleri için bir konum almak için konum ve konu türüne göre sağlamak **Microsoft.Storage.StorageAccounts**.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type "Microsoft.Storage.StorageAccounts" --location westus2
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Storage.StorageAccounts" -Location westus2
```

Özel bir konu için olay kılavuz abonelikleri almak için özel konu adını ve kaynak grubu adını sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-name myCustomTopic --resource-group myResourceGroup
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicName myCustomTopic -ResourceGroupName myResourceGroup
```

Belirli bir kaynak için olay kılavuz abonelikleri almak için kaynak kimliğini sağlayın

Azure CLI için şunu kullanın:

```azurecli-interactive
resourceid=$(az resource show -n mystorage -g myResourceGroup --resource-type "Microsoft.Storage/storageaccounts" --query id --output tsv)
az eventgrid event-subscription list --resource-id $resourceid
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
$resourceid = (Get-AzureRmResource -Name mystorage -ResourceGroupName myResourceGroup).ResourceId
Get-AzureRmEventGridSubscription -ResourceId $resourceid
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).
* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
