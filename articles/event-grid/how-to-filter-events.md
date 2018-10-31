---
title: Olayları Azure Event Grid için filtreleme
description: Olayları Filtrele Azure Event Grid abonelikleri oluşturma işlemi gösterilmektedir.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: tomfitz
ms.openlocfilehash: 8bf7ac9daf928c35a3d6efcac528d3372fa87c8a
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252148"
---
# <a name="filter-events-for-event-grid"></a>Event Grid için olayları filtreleyin

Bu makalede, olayları bir Event Grid Abonelik oluşturulurken filtre gösterilmektedir. Olay filtreleme seçenekleri hakkında bilgi edinmek için bkz. [anlayın olay için Event Grid abonelikleri filtreleme](event-filtering.md).

## <a name="filter-by-event-type"></a>Olay türüne göre filtrele

Event Grid aboneliği oluşturulduğu sırada belirtebileceğiniz [olay türleri](event-schema.md) uç noktasına göndermesi için. Bu bölümdeki örneklerde, bir kaynak grubu için olay abonelikleri oluşturma ancak gönderilen olayları sınırlandırmak `Microsoft.Resources.ResourceWriteFailure` ve `Microsoft.Resources.ResourceWriteSuccess`. Daha fazla esneklik olayları olay türlerine göre filtreleme yaparken ihtiyacınız varsa bkz [Gelişmiş işleçler ve veri alanlarını olarak filtre](#filter-by-advanced-operators-and-data-fields).

PowerShell için şunu kullanın `-IncludedEventType` abonelik oluştururken parametresi.

```powershell
$includedEventTypes = "Microsoft.Resources.ResourceWriteFailure", "Microsoft.Resources.ResourceWriteSuccess"

New-AzureRmEventGridSubscription `
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

Olayları olay verileri içindeki bir konuya göre filtreleyebilirsiniz. Başında ve sonunda konu için eşleşen bir değer belirtebilirsiniz. Konuya göre olaylar filtrelerken daha fazla esneklik gerekiyorsa bkz [Gelişmiş işleçler ve veri alanlarını olarak filtre](#filter-by-advanced-operators-and-data-fields).

Aşağıdaki PowerShell örneği filtreleyen bir olay aboneliği başına tarafından konu oluşturun. Kullandığınız `-SubjectBeginsWith` belirli bir kaynak için olayları sınırlandırmak için parametre. Bir ağ güvenlik grubu kaynak Kimliğini geçirmeniz.

```powershell
$resourceId = (Get-AzureRmResource -ResourceName demoSecurityGroup -ResourceGroupName myResourceGroup).ResourceId

New-AzureRmEventGridSubscription `
  -Endpoint <endpoint-URL> `
  -EventSubscriptionName demoSubscriptionToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -SubjectBeginsWith $resourceId
```

Sonraki PowerShell örneği, bir blob depolama alanı için bir abonelik oluşturur. İle biten bir konu ettiklerinizi olayları sınırlayan `.jpg`.

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName myResourceGroup -AccountName $storageName).Id

New-AzureRmEventGridSubscription `
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

Gelişmiş filtreleme özelliğini kullanmak üzere Azure CLI için Önizleme uzantısını yüklemeniz gerekir. Kullanabileceğiniz [CloudShell](/azure/cloud-shell/quickstart) veya Azure CLI'yı yerel olarak yükleyin.

### <a name="install-extension"></a>Uzantıyı yükleme

CloudShell içinde:

* Uzantıyı daha önce yüklediyseniz bu güncelleştirme `az extension update -n eventgrid`
* Uzantıyı daha önce yüklemediyseniz yükleyin `az extension add -n eventgrid`

Yerel bir yüklemesi için:

1. Azure CLI'yi yerel olarak kaldırın.
1. Yükleme [en son sürümü](/cli/azure/install-azure-cli) Azure CLI'ın.
1. Komut penceresini başlatın.
1. Uzantı'nın önceki sürümlerini kaldırma `az extension remove -n eventgrid`
1. Uzantıyı yükleme `az extension add -n eventgrid`

Artık Gelişmiş filtreleme kullanmaya hazırsınız.

### <a name="subscribe-with-advanced-filters"></a>Gelişmiş Filtreler ile abone olun

İşleçler ve Gelişmiş filtreleme için kullanabileceğiniz anahtarlar hakkında bilgi edinmek için bkz. [Gelişmiş filtreleme](event-filtering.md#advanced-filtering).

Aşağıdaki örnek, özel bir konu oluşturur. Özel konuya abone olur ve veri nesnesi içinde bir değere göre filtreler. Mavi renk özelliği olayları, kırmızı veya yeşil gönderilen abonelik.

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
  --expiration-date "2018-11-30"
```

Abonelik için bir sona erme tarihi ayarlandığına dikkat edin. Olay aboneliği otomatik olarak bu tarihten sonra süresi doldu. Bir süre sonu yalnızca sınırlı bir süre için gerekli olan olay abonelikleri için ayarlayın.

### <a name="test-filter"></a>Test filtresi

Filtreyi test etmek için ayarlamak için yeşil renk alanı ile bir olay gönderin.

```azurecli-interactive
topicEndpoint=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name $topicName -g gridResourceGroup --query "key1" --output tsv)

event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "green"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

Olay uç noktanıza gönderilir.

Olayın nerede gönderilen değil bir senaryoyu test etmek için renk alanı sarı olarak ayarlanmış bir olay gönderin.

```azurecli-interactive
event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "yellow"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

Sarı için olay aboneliğiniz için teslim olmayan Aboneliklerde belirtilen değerlerden biri değil.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimat izleme hakkında daha fazla bilgi için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz: [Event Grid güvenliğini ve kimlik doğrulaması](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
