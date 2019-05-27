---
title: Azure PowerShell kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma | Microsoft Docs
description: Azure PowerShell kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 51e9bc85c2ee843aa096674a25a1f634bd08b838
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66162575"
---
# <a name="tutorial-use-azure-powershell-to-configure-iot-hub-message-routing"></a>Öğretici: IOT Hub ileti yönlendirme yapılandırmak için Azure PowerShell'i kullanma

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="download-the-script-optional"></a>(İsteğe bağlı) betiği indirin

Bu öğreticinin ikinci bölümü, indirin ve IOT Hub'ına ileti göndermek için bir Visual Studio uygulamayı çalıştırın. Azure Resource Manager şablonu ve parametre dosyasını yanı sıra, Azure CLI ve PowerShell betikleri içeren indirme bir klasör bulunur. 

Tamamlanmış betiği görüntülemek istiyorsanız, indirme [Azure IOT C# örnekleri](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Master.zip dosyanın sıkıştırmasını açın. Azure CLI betiği olan olarak//iot-hub/Tutorials/Routing/SimulatedDevice/resources **iothub_routing_psh.ps1**.

## <a name="create-your-resources"></a>Kaynaklarınızı oluşturun

PowerShell ile kaynakları oluşturarak başlayın.

### <a name="use-powershell-to-create-your-base-resources"></a>Temel kaynaklarınızı oluşturmak için PowerShell kullanma

IOT hub'ı adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adları vardır. Bunu kolaylaştırmak için bu kaynak adları adlı rastgele alfasayısal bir değer eklenir *randomValue*. RandomValue komut dosyasının üst kısmında bir kez oluşturulur ve komut dosyası gerektiğinde kaynak adları için eklenmiş. Rastgele olmasını istemiyorsanız, boş bir dize veya belirli bir değere ayarlayabilirsiniz. 

> [!IMPORTANT]
> İlk komut dosyasında değişkenleri de bu nedenle betiği tümünün aynı Cloud Shell oturumda çalıştırılan yönlendirme komut dosyası tarafından kullanılır. Yönlendirmeyi ayarlama için komut dosyasını çalıştırmak için yeni bir oturum açarsanız, birkaç değişkenlerin değerleri eksik olacaktır. 
>

Kopyalama, aşağıdaki komut dosyasını Cloud shell'e yapıştırın ve Enter tuşuna basın. Bu betik bir satır aynı anda çalışır. Betik bu ilk bölümünde, depolama hesabı, IOT hub'ı, Service Bus Namespace ve Service Bus kuyruğu dahil olmak üzere bu öğreticide, temel kaynaklar oluşturacaksınız. Öğreticide ilerlerken, her komut dosyası bloğu kopyalamak ve çalıştırmak için Cloud shell'e yapıştırın.

```azurepowershell-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing rules.
$subscriptionID = (Get-AzContext).Subscription.Id

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves the first 6 digits of a random value.
$randomValue = "$(Get-Random)".Substring(0,6)

# Set the values for the resource names that don't have to be globally unique.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, 
#   so add a random value to the end.
$iotHubName = "ContosoTestHub" + $randomValue
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1 

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random value to the end.
$storageAccountName = "contosostorage" + $randomValue
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
# Retrieve the connection string from the context. 
$storageConnectionString = $storageAccount.Context.ConnectionString
Write-Host "storage connection string = " $storageConnectionString 

# Create the container in the storage account.
New-AzStorageContainer -Name $containerName `
    -Context $storageAccount.Context

# The Service Bus namespace must be globally unique,
#   so add a random value to the end.
$serviceBusNamespace = "ContosoSBNamespace" + $randomValue
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random value to the end.
$serviceBusQueueName  = "ContosoSBQueue" + $randomValue
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName  `
    -EnablePartitioning $False 
```

### <a name="create-a-simulated-device"></a>Sanal cihaz oluşturma

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

Temel kaynaklar ayarlanır, ileti yönlendirme yapılandırabilirsiniz.

## <a name="set-up-message-routing"></a>İleti yönlendirmeyi ayarlama

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

Yönlendirme bir uç nokta oluşturmak için kullanın [Ekle AzIotHubRoutingEndpoint](/powershell/module/az.iothub/Add-AzIotHubRoutingEndpoint). Uç noktası için Mesajlaşma rota oluşturmak için kullanın [Ekle AzIotHubRoute](/powershell/module/az.iothub/Add-AzIoTHubRoute).

### <a name="route-to-a-storage-account"></a>Bir depolama hesabına yönlendirme 

İlk olarak, depolama hesabı için uç nokta ayarlayın, sonra ileti yolu oluşturun.

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

Bu değişkenler ayarlanır:

**ResourceGroup**: Bu alanın iki kez vardır; her ikisi de, kaynak grubunuza ayarlayın.

**Ad**: Bu alan, yönlendirme uygulanacağı IOT Hub'ının adıdır.

**Uçnoktaadı**: Bu alan, uç nokta tanımlayan addır. 

**endpointType**: Bu alan, uç nokta türüdür. Bu değer ayarlanmalıdır `azurestoragecontainer`, `eventhub`, `servicebusqueue`, veya `servicebustopic`. Amacınıza buraya ayarlayın `azurestoragecontainer`.

**Subscriptionıd**: Bu alan Subscriptionıd Azure hesabınız için ayarlanır.

**storageConnectionString**: Bu değer, önceki betikte ayarlanan depolama hesabından alınır. Depolama hesabına erişmek için bu yönlendirme'ı kullanılır.

**containerName**: Depolama hesabındaki verilerin yazılacağı kapsayıcı adı alandır.

**kodlama**: Bu alan aşağıdaki seçeneklerden birine ayarlayın `AVRO` veya `JSON`. Bu, depolanan verilerin biçimini belirler. AVRO varsayılandır.

**Routetablename**: Bu alan, ayarladığınız yol adıdır. 

**Koşul**: Bu alan için bu endpoint gönderilen iletileri için filtre uygulamak için kullanılan sorgu gereklidir. Depolama'ya yönlendirilen iletileri için sorgu koşulu `level="storage"`.

**Etkin**: Bu alan için varsayılan olarak `true`, belirten bir ileti yolu oluşturduktan sonra etkinleştirilmelidir.

Bu betiği kopyalayıp, Cloud Shell penceresine yapıştırın.

```powershell
##### ROUTING FOR STORAGE #####

$endpointName = "ContosoStorageEndpoint"
$endpointType = "azurestoragecontainer"
$routeName = "ContosoStorageRoute"
$condition = 'level="storage"'
```

Sonraki adım, depolama hesabı için yönlendirme uç nokta oluşturmaktır. Ayrıca, sonuçların depolanacağı bir kapsayıcı belirtirsiniz. Depolama hesabı oluşturduğunuzda, kapsayıcı oluşturuldu.

```powershell
# Create the routing endpoint for storage.
# Specify 'AVRO' or 'JSON' for the encoding of the data.
Add-AzIotHubRoutingEndpoint `
  -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EndpointName $endpointName `
  -EndpointType $endpointType `
  -EndpointResourceGroup $resourceGroup `
  -EndpointSubscriptionId $subscriptionId `
  -ConnectionString $storageConnectionString  `
  -ContainerName $containerName `
  -Encoding AVRO
```

Ardından, depolama uç noktasının ileti yolu oluşturun. Sorgu belirtimi uyan iletileri gönderileceği ileti yolu belirtir.

```powershell
# Create the route for the storage endpoint.
Add-AzIotHubRoute `
   -ResourceGroupName $resourceGroup `
   -Name $iotHubName `
   -RouteName $routeName `
   -Source DeviceMessages `
   -EndpointName $endpointName `
   -Condition $condition `
   -Enabled 
```

### <a name="route-to-a-service-bus-queue"></a>Bir Service Bus kuyruğuna yönlendirme

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. Service Bus kuyruğu için bağlantı dizesini almak için tanımlanan doğru haklara sahip bir yetkilendirme kuralı oluşturmanız gerekir. Aşağıdaki betiği bir yetkilendirme kuralı için adlı Service Bus kuyruğu oluşturur `sbauthrule`ve hakları ayarlar `Listen Manage Send`. Bu yetkilendirme kuralı ayarladıktan sonra sıra için bağlantı dizesini almak için kullanabilirsiniz.

```powershell
##### ROUTING FOR SERVICE BUS QUEUE #####

# Create the authorization rule for the Service Bus queue.
New-AzServiceBusAuthorizationRule `
  -ResourceGroupName $resourceGroup `
  -NamespaceName $serviceBusNamespace `
  -Queue $serviceBusQueueName `
  -Name "sbauthrule" `
  -Rights @("Manage","Listen","Send")
```

Artık Service Bus kuyruğu anahtarı almak için yetkilendirme kuralını kullanın. Bu yetkilendirme kuralı, daha sonra betikte bağlantı dizesini almak için kullanılır.

```powershell
$sbqkey = Get-AzServiceBusKey `
    -ResourceGroupName $resourceGroup `
    -NamespaceName $serviceBusNamespace `
    -Queue $servicebusQueueName `
    -Name "sbauthrule"
```

Şimdi yönlendirme uç nokta ve Service Bus kuyruğuna ileti yolunu ayarlayın. Bu değişkenler ayarlanır:

**Uçnoktaadı**: Bu alan, uç nokta tanımlayan addır. 

**endpointType**: Bu alan, uç nokta türüdür. Bu değer ayarlanmalıdır `azurestoragecontainer`, `eventhub`, `servicebusqueue`, veya `servicebustopic`. Amacınıza buraya ayarlayın `servicebusqueue`.

**Routetablename**: Bu alan, ayarladığınız yol adıdır. 

**Koşul**: Bu alan için bu endpoint gönderilen iletileri için filtre uygulamak için kullanılan sorgu gereklidir. Service Bus kuyruğuna yönlendirilen iletileri için sorgu koşulu `level="critical"`.

Azure PowerShell için Service Bus kuyruğuna ileti yönlendirme için aşağıda verilmiştir.

```powershell
$endpointName = "ContosoSBQueueEndpoint"
$endpointType = "servicebusqueue"
$routeName = "ContosoSBQueueRoute"
$condition = 'level="critical"'

# Add the routing endpoint, using the connection string property from the key.
Add-AzIotHubRoutingEndpoint `
  -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EndpointName $endpointName `
  -EndpointType $endpointType `
  -EndpointResourceGroup $resourceGroup `
  -EndpointSubscriptionId $subscriptionId `
  -ConnectionString $sbqkey.PrimaryConnectionString

# Set up the message route for the Service Bus queue endpoint.
Add-AzIotHubRoute `
   -ResourceGroupName $resourceGroup `
   -Name $iotHubName `
   -RouteName $routeName `
   -Source DeviceMessages `
   -EndpointName $endpointName `
   -Condition $condition `
   -Enabled 
```

### <a name="view-message-routing-in-the-portal"></a>Portalda ileti yönlendirme görüntüleyin

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ayarlanan kaynakları ve yapılandırılmış ileti yollarını edindikten sonra IOT hub'ına ileti göndermek ve farklı hedeflere görebileceği hakkında bilgi edinmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [2. Kısım - ileti yönlendirme sonuçlarını görüntüleme](tutorial-routing-view-message-routing-results.md)