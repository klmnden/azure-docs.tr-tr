---
title: Olayları Azure Event Grid için filtreleme
description: Olayları Filtrele Azure Event Grid abonelikleri oluşturma işlemi gösterilmektedir.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: spelluru
ms.openlocfilehash: 5bb95b80e12c818641e2be2b929cdfd01f8f5b5c
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304221"
---
# <a name="filter-events-for-event-grid"></a>Event Grid için olayları filtreleyin

Bu makalede, olayları bir Event Grid Abonelik oluşturulurken filtre gösterilmektedir. Olay filtreleme seçenekleri hakkında bilgi edinmek için bkz. [anlayın olay için Event Grid abonelikleri filtreleme](event-filtering.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="filter-by-event-type"></a>Olay türüne göre filtrele

Event Grid aboneliği oluşturulduğu sırada belirtebileceğiniz [olay türleri](event-schema.md) uç noktasına göndermesi için. Bu bölümdeki örneklerde, bir kaynak grubu için olay abonelikleri oluşturma ancak gönderilen olayları sınırlandırmak `Microsoft.Resources.ResourceWriteFailure` ve `Microsoft.Resources.ResourceWriteSuccess`. Gelişmiş işleçler ve veri alanlarını göre filtrele, olayları olay türlerine göre filtreleme yaparken daha fazla esneklik gerekiyorsa, bkz.

PowerShell için şunu kullanın `-IncludedEventType` abonelik oluştururken parametresi.

```powershell
$includedEventTypes = "Microsoft.Resources.ResourceWriteFailure", "Microsoft.Resources.ResourceWriteSuccess"

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Endpoint <endpoint-URL> `
  -IncludedEventType $includedEventTypes
```

Azure CLI için şunu kullanın `--included-event-types` parametresi. Azure CLI, aşağıdaki örnek bir Bash kabuğunda kullanır:

```azurecli
includedEventTypes="Microsoft.Resources.ResourceWriteFailure Microsoft.Resources.ResourceWriteSuccess"

az eventgrid event-subscription create \
  --name demoSubToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --included-event-types $includedEventTypes
```

Bir Resource Manager şablon için kullandığınız `includedEventTypes` özelliği.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [
          "Microsoft.Resources.ResourceWriteFailure",
          "Microsoft.Resources.ResourceWriteSuccess"
        ]
      }
    }
  }
]
```

## <a name="filter-by-subject"></a>Konuya göre filtrele

Olayları olay verileri içindeki bir konuya göre filtreleyebilirsiniz. Başında ve sonunda konu için eşleşen bir değer belirtebilirsiniz. Gelişmiş işleçler ve veri alanlarını göre filtrele konuya göre olaylar filtrelerken daha fazla esneklik gerekiyorsa, bkz.

Aşağıdaki PowerShell örneği filtreleyen bir olay aboneliği başına tarafından konu oluşturun. Kullandığınız `-SubjectBeginsWith` belirli bir kaynak için olayları sınırlandırmak için parametre. Bir ağ güvenlik grubu kaynak Kimliğini geçirmeniz.

```powershell
$resourceId = (Get-AzResource -ResourceName demoSecurityGroup -ResourceGroupName myResourceGroup).ResourceId

New-AzEventGridSubscription `
  -Endpoint <endpoint-URL> `
  -EventSubscriptionName demoSubscriptionToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -SubjectBeginsWith $resourceId
```

Sonraki PowerShell örneği, bir blob depolama alanı için bir abonelik oluşturur. İle biten bir konu ettiklerinizi olayları sınırlayan `.jpg`.

```powershell
$storageId = (Get-AzStorageAccount -ResourceGroupName myResourceGroup -AccountName $storageName).Id

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToStorage `
  -Endpoint <endpoint-URL> `
  -ResourceId $storageId `
  -SubjectEndsWith ".jpg"
```

Azure CLI aşağıdaki örnekte filtreleyen bir olay aboneliği başına tarafından konu oluşturun. Kullandığınız `--subject-begins-with` belirli bir kaynak için olayları sınırlandırmak için parametre. Bir ağ güvenlik grubu kaynak Kimliğini geçirmeniz.

```azurecli
resourceId=$(az resource show --name demoSecurityGroup --resource-group myResourceGroup --resource-type Microsoft.Network/networkSecurityGroups --query id --output tsv)

az eventgrid event-subscription create \
  --name demoSubscriptionToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --subject-begins-with $resourceId
```

Sonraki Azure CLI örnek, bir blob depolama alanı için bir abonelik oluşturur. İle biten bir konu ettiklerinizi olayları sınırlayan `.jpg`.

```azurecli
storageid=$(az storage account show --name $storageName --resource-group myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name demoSubToStorage \
  --endpoint <endpoint-URL> \
  --subject-ends-with ".jpg"
```

Aşağıdaki Resource Manager şablonu örnekte filtreleyen bir olay aboneliği başına tarafından konu oluşturun. Kullandığınız `subjectBeginsWith` belirli bir kaynak için olayları sınırlandırmak için özellik. Bir ağ güvenlik grubu kaynak Kimliğini geçirmeniz.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "[resourceId('Microsoft.Network/networkSecurityGroups','demoSecurityGroup')]",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

Sonraki Resource Manager şablonu örneği, bir blob depolama alanı için bir abonelik oluşturur. İle biten bir konu ettiklerinizi olayları sınırlayan `.jpg`.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
    "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/', parameters('eventSubName'))]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectEndsWith": ".jpg",
        "subjectBeginsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

## <a name="filter-by-operators-and-data"></a>İşleçler ve veri göre filtrele

Filtreleme daha fazla esneklik için işleçler ve filtre olayları için veri özelliklerini kullanabilirsiniz.

### <a name="subscribe-with-advanced-filters"></a>Gelişmiş Filtreler ile abone olun

İşleçler ve Gelişmiş filtreleme için kullanabileceğiniz anahtarlar hakkında bilgi edinmek için bkz. [Gelişmiş filtreleme](event-filtering.md#advanced-filtering).

Bu örnekler, özel bir konu oluşturun. Bunlar özel konuya abone olma ve veri nesnesi içinde bir değere göre filtreleyin. Mavi renk özelliği olayları, kırmızı veya yeşil gönderilen abonelik.

Azure CLI için şunu kullanın:

```azurecli-interactive
topicName=<your-topic-name>
endpointURL=<endpoint-URL>

az group create -n gridResourceGroup -l eastus2
az eventgrid topic create --name $topicName -l eastus2 -g gridResourceGroup

topicid=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  -n demoAdvancedSub \
  --advanced-filter data.color stringin blue red green \
  --endpoint $endpointURL \
  --expiration-date "<yyyy-mm-dd>"
```

Abonelik için bir [sona erme tarihi](concepts.md#event-subscription-expiration) belirlendiğine dikkat edin.

PowerShell için şunu kullanın:

```azurepowershell-interactive
$topicName = <your-topic-name>
$endpointURL = <endpoint-URL>

New-AzResourceGroup -Name gridResourceGroup -Location eastus2
New-AzEventGridTopic -ResourceGroupName gridResourceGroup -Location eastus2 -Name $topicName

$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Id

$expDate = '<mm/dd/yyyy hh:mm:ss>' | Get-Date
$AdvFilter1=@{operator="StringIn"; key="Data.color"; Values=@('blue', 'red', 'green')}

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint $endpointURL `
  -ExpirationDate $expDate `
  -AdvancedFilter @($AdvFilter1)
```

### <a name="test-filter"></a>Test filtresi

Filtreyi test etmek için ayarlamak için yeşil renk alanı ile bir olay gönderin. Çünkü filtre değerlerinden yeşil, olay uç noktasına gönderilir.

Azure CLI için şunu kullanın:

```azurecli-interactive
topicEndpoint=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name $topicName -g gridResourceGroup --query "key1" --output tsv)

event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "green"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
$endpoint = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Endpoint
$keys = Get-AzEventGridTopicKey -ResourceGroupName gridResourceGroup -Name $topicName

$eventID = Get-Random 99999
$eventDate = Get-Date -Format s

$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="green"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Olayın nerede gönderilen değil bir senaryoyu test etmek için renk alanı sarı olarak ayarlanmış bir olay gönderin. Sarı için olay aboneliğiniz için teslim olmayan Aboneliklerde belirtilen değerlerden biri değil.

Azure CLI için şunu kullanın:

```azurecli-interactive
event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "yellow"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```
PowerShell için şunu kullanın:

```azurepowershell-interactive
$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="yellow"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimat izleme hakkında daha fazla bilgi için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz: [Event Grid güvenliğini ve kimlik doğrulaması](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
