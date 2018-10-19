---
title: Şablon kullanarak bir Azure Event Hubs ad alanı ve tüketici grubu oluştur | Microsoft Docs
description: Bir olay hub'ı ve Azure Resource Manager şablonlarını kullanarak bir tüketici grubu ile bir Event Hubs ad alanı oluşturma
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: e3a7b2a7ad866fe6c70c638dc5203b9a31c6b5b3
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426643"
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Bir Event Hubs ad alanı bir Azure Resource Manager şablonu kullanarak olay hub'ı ve tüketici grubu oluşturun
Azure Event Hubs saniyede milyonlarca olay alıp işleyebilen, ölçeklenebilirlik yüzeyi yüksek bir veri akışı platformu ve veri alma hizmetidir. Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak bir olay hub'ı oluşturma işlemi gösterilmektedir.

Bir Azure Resource Manager şablonu türünde bir ad alanı oluşturmak için kullandığınız [Event Hubs](event-hubs-what-is-event-hubs.md)bir olay hub'ı ve tek bir tüketici grubu. Makalede nasıl tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen gösterilmektedir. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Tam şablon için bkz: [olay hub'ı ve tüketici grubu şablonunu] [ Event Hub and consumer group template] GitHub üzerinde.

> [!NOTE]
> En yeni şablonları denetlemek için [Azure Hızlı Başlangıç Şablonları][Azure Quickstart Templates] galerisini ziyaret edin ve Event Hubs araması yapın.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Biri yoksa [ücretsiz hesap oluşturun] başlamadan önce [].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Kullanıyorsanız **Azure PowerShell** Resource Manager şablonu dağıtmak için yerel olarak, bu hızlı başlangıcı tamamlamak için PowerShell'in en son sürümünü çalıştırmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell'i Yükleme ve Yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

Yükleyip kullanmayı tercih ederseniz **Azure CLI** Resource Manager şablonu dağıtmak için yerel olarak Bu öğretici, Azure CLI 2.0.4 sürüm gerektirir veya üzeri. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablonu kullanarak bir Event Hubs ad alanı bir olay hub'ı ve bir tüketici grubu ile dağıtın.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, tüm parametre değerlerini içeren `Parameters` adlı bir bölüm içerir. Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama bağlı değişir değerleri için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her bir şablon parametre değeri, dağıtılan kaynakları tanımlar.

Şablon aşağıdaki parametreleri tanımlar:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Oluşturulacak Event Hubs ad alanının adı.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Event Hubs ad alanında oluşturulan olay hub’ının adı.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Oluşturulan olay hub'ı için tüketici grubunun adıdır.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

Şablonun API sürümü.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar

Türünde bir ad alanı oluşturur **EventHubs**bir olay hub'ı ve bir tüketici grubu:

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```azurepowershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```
### <a name="create-a-storage-account-for-event-processor-host"></a>Olay İşleyicisi Ana Bilgisayarı için bir depolama hesabı oluşturma

Olay İşleyicisi Ana Bilgisayarı, olay hub’larına ait denetim noktalarını ve paralel alıcıları yöneterek bu olay hub’larından olay almayı basitleştirir. Olay İşleyicisi Ana Bilgisayarı, denetim noktası için bir depolama hesabına ihtiyaç duyar. Bir depolama hesabı oluşturmak anahtarlarını almak için aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
# Create a standard general purpose storage account 
New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name storage_account_name -Location eastus -SkuName Standard_LRS 
e
# Retrieve the storage account key for accessing it
Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name storage_account_name
```

Olay hub'ınıza bağlanmak ve olayları işlemek için bir bağlantı dizesi gerekir. Bağlantı dizenizi almak için şu komutu çalıştırın:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```


## <a name="azure-cli"></a>Azure CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

### <a name="create-a-storage-account-for-event-processor-host"></a>Olay İşleyicisi Ana Bilgisayarı için bir depolama hesabı oluşturma
```azurecli-interactive
# Create a general purpose standard storage account for Event Processor Host
az storage account create --name storageAccountName --resource-group myResourceGroup --location eastus2 --sku Standard_RAGRS --encryption blob

# List the storage account access keys
az storage account keys list --resource-group myResourceGroup --account-name storageAccountName

# Get namespace connection string
az eventhubs namespace authorization-rule keys list --resource-group myResourceGroup --namespace-name namespaceName --name RootManageSharedAccessKey
```

Bağlantı dizesini kopyalayıp daha sonra kullanmak üzere Not Defteri gibi geçici bir konuma yapıştırın.

## <a name="stream-into-event-hubs"></a>Event Hubs'a akış sağlama

Artık Event Hubs'a akış başlatabilirsiniz. Bu örnekleri [Event Hubs deposundan](https://github.com/Azure/azure-event-hubs) indirebilir veya Git üzerinden kopyalayabilirsiniz

### <a name="ingest-events"></a>Olayları alma

Olay akışını başlatmak için GitHub'dan [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) dosyasını indirin veya aşağıdaki komutu kullanarak [Event Hubs GitHub deposunu](https://github.com/Azure/azure-event-hubs) kopyalayın:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

\azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleSender klasörüne gidin ve SampleSender.sln dosyasını Visual Studio'ya yükleyin.

Ardından [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) Nuget paketini projeye ekleyin.

Program.cs dosyasında aşağıdaki yer tutucuları olay hub'ınızın adı ve bağlantı dizenizle değiştirin:

```C#
private const string EhConnectionString = "Event Hubs connection string";
private const string EhEntityPath = "Event Hub name";

```

Şimdi örneği derleyin ve çalıştırın. Olayların, olay hub'ınıza alındığını görebilirsiniz:

![][3]

### <a name="receive-and-process-events"></a>Olayları alma ve işleme

Şimdi, az önce gönderdiğiniz iletileri alan Olay İşlemcisi Ana Bilgisayarı alıcı örneğini indirin. GitHub'dan [SampleEphReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) dosyasını indirin veya aşağıdaki komutu kullanarak [Event Hubs GitHub deposunu](https://github.com/Azure/azure-event-hubs) kopyalayın:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

\azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleEphReceiver klasörüne gidin ve SampleEphReceiver.sln dosyasını Visual Studio'ya yükleyin.

Ardından [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) ve [Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) Nuget paketlerini projenize ekleyin.

Program.cs dosyasında aşağıdaki sabitleri ilgili değerlerle değiştirin:

```C#
private const string EventHubConnectionString = "Event Hubs connection string";
private const string EventHubName = "Event Hub name";
private const string StorageContainerName = "Storage account container name";
private const string StorageAccountName = "Storage account name";
private const string StorageAccountKey = "Storage account key";
```

Şimdi örneği derleyin ve çalıştırın. Olayların örnek uygulamanıza geldiğini görebilirsiniz:

![][4]

Azure portalda belirli bir Event Hubs ad alanı için olayların işleme hızını aşağıdaki şekilde görüntüleyebilirsiniz:

![][5]


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Event Hubs ad alanı oluşturan ve olay hub'ından olayları alıp göndermek için örnek uygulamaları kullanılır. Olayları (gönderip) olayları bir event hub'ından adım adım yönergeler için aşağıdaki öğreticilere bakın: 

1. **Olay hub'ına olayları gönderme**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js ](event-hubs-node-get-started-send.md), [Git](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
2. **Bir olay hub'ından olay alma**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [ Node.js](event-hubs-node-get-started-receive.md), [Git](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
