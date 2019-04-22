---
title: Azure CLI kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma | Microsoft Docs
description: Azure CLI kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 6faa585f1ad38eb981e0bbffffef603c4aab0bc8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59360276"
---
# <a name="tutorial-use-the-azure-cli-to-configure-iot-hub-message-routing"></a>Öğretici: IOT Hub ileti yönlendirme yapılandırmak için Azure CLI kullanma

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="download-the-script-optional"></a>(İsteğe bağlı) betiği indirin

Bu öğreticinin ikinci bölümü, indirin ve IOT Hub'ına ileti göndermek için bir Visual Studio uygulamayı çalıştırın. Azure Resource Manager şablonu ve parametre dosyasını yanı sıra, Azure CLI ve PowerShell betikleri içeren indirme bir klasör bulunur.

Tamamlanmış betiği görüntülemek istiyorsanız, indirme [Azure IOT C# örnekleri](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Master.zip dosyanın sıkıştırmasını açın. Azure CLI betiği olan olarak//iot-hub/Tutorials/Routing/SimulatedDevice/resources **iothub_routing_cli.azcli**.

## <a name="use-the-azure-cli-to-create-your-resources"></a>Kaynaklarınızı oluşturmak için Azure CLI'yi kullanma

IOT hub'ı adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adları vardır. Bunu kolaylaştırmak için bu kaynak adları adlı rastgele alfasayısal bir değer eklenir *randomValue*. RandomValue komut dosyasının üst kısmında bir kez oluşturulur ve komut dosyası gerektiğinde kaynak adları için eklenmiş. Rastgele olmasını istemiyorsanız, boş bir dize veya belirli bir değere ayarlayabilirsiniz. 

> [!IMPORTANT]
> İlk komut dosyasında değişkenleri de bu nedenle betiği tümünün aynı Cloud Shell oturumda çalıştırılan yönlendirme komut dosyası tarafından kullanılır. Yönlendirmeyi ayarlama için komut dosyasını çalıştırmak için yeni bir oturum açarsanız, birkaç değişkenlerin değerleri eksik olacaktır.
>

Kopyalama, aşağıdaki komut dosyasını Cloud shell'e yapıştırın ve Enter tuşuna basın. Bu betik bir satır aynı anda çalışır. Betik bu ilk bölümünde, depolama hesabı, IOT hub'ı, Service Bus Namespace ve Service Bus kuyruğu dahil olmak üzere bu öğreticide, temel kaynaklar oluşturacaksınız. Bu öğreticinin geri kalanını giderken, her komut dosyası bloğu kopyalayın ve çalıştırmak için Cloud shell'e yapıştırın.

> [!TIP]
> Hata ayıklama hakkında bir ipucu: Bu betik, devamlılık sembol kullanır (ters eğik çizgi `\`) kod daha okunabilir yapmak için. Betik çalıştırılırken bir sorun varsa, boşluk olmayan herhangi bir ters eğik çizgi sonra emin olun.
> 

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account. 
# This field is used when setting up the routing rules.
subscriptionID=$(az account show --query id)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# This command installs the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that 
#   don't have to be globally unique.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, 
#   so add a random value to the end.
iotHubName=ContosoTestHub$randomValue 
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub for the 'events' endpoint.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, 
#   so add a random value to the end.
storageAccountName=contosostorage$randomValue
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key. 
#    You need this to create the container.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"') 

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# The Service Bus namespace must be globally unique, 
#   so add a random value to the end.
sbNamespace=ContosoSBNamespace$randomValue
echo "Service Bus namespace = " $sbNamespace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNamespace \
    --location $location

# The Service Bus queue name must be globally unique, 
#   so add a random value to the end.
sbQueueName=ContosoSBQueue$randomValue
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNamespace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName
```

Temel kaynaklar ayarlanır, ileti yönlendirme yapılandırabilirsiniz.

## <a name="set-up-message-routing"></a>İleti yönlendirmeyi ayarlama

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

Yönlendirme bir uç nokta oluşturmak için kullanın [az IOT hub'ı üretim uç noktası oluşturma](/cli/azure/iot/hub/routing-endpoint?view=azure-cli-latest#az-iot-hub-routing-endpoint-create). Uç noktası için ileti yolu oluşturmak için kullanın [az IOT hub rotasını oluşturma](/cli/azure/iot/hub/route?view=azure-cli-latest#az-iot-hub-route-create).

### <a name="route-to-a-storage-account"></a>Bir depolama hesabına yönlendirme

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

İlk olarak, depolama hesabı için uç nokta ayarlayın ve ardından rotayı Ayarla. 

Bu değişkenler ayarlanır:

**storageConnectionString**: Bu değer, önceki betikte ayarlanan depolama hesabından alınır. Bu depolama hesabına erişmek için yönlendirme ileti tarafından kullanılır.

  **ResourceGroup**: Kaynak grubunun iki kez vardır; bunları kaynak grubunuzun ayarlayın.

**uç nokta Subscriptionıd**: Bu alan uç noktası için Azure Subscriptionıd ayarlanır. 

**endpointType**: Bu alan, uç nokta türüdür. Bu değer ayarlanmalıdır `azurestoragecontainer`, `eventhub`, `servicebusqueue`, veya `servicebustopic`. Amacınıza buraya ayarlayın `azurestoragecontainer`.

**iotHubName**: Bu alan yönlendirme yapan hub'a adıdır.

**containerName**: Depolama hesabındaki verilerin yazılacağı kapsayıcı adı alandır.

**kodlama**: Bu alan olacaktır `avro` veya `json`. Bu, depolanan verilerin biçimini gösterir.

**Routetablename**: Bu alan, ayarladığınız yol adıdır. 

**Uçnoktaadı**: Bu alan, uç nokta tanımlayan addır. 

**Etkin**: Bu alan için varsayılan olarak `true`, belirten bir ileti yolu oluşturduktan sonra etkinleştirilmelidir.

**Koşul**: Bu alan için bu endpoint gönderilen iletileri için filtre uygulamak için kullanılan sorgu gereklidir. Depolama'ya yönlendirilen iletileri için sorgu koşulu `level="storage"`.

Bu komut dosyasını kopyalayıp, Cloud Shell penceresine yapıştırın ve çalıştırın.

```azurecli
##### ROUTING FOR STORAGE ##### 

endpointName="ContosoStorageEndpoint"
endpointType="azurestoragecontainer"
routeName="ContosoStorageRoute"
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)
```

Sonraki adım, depolama hesabı için yönlendirme uç nokta oluşturmaktır. Ayrıca, sonuçların depolanacağı bir kapsayıcı belirtirsiniz. Depolama hesabı oluşturduğunuzda, kapsayıcı önceden oluşturulmuş.

```azurecli
# Create the routing endpoint for storage.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName \
  --resource-group $resourceGroup \
  --encoding avro
```

Ardından, depolama uç noktası için bir yol oluşturun. Sorgu belirtimi uyan iletileri gönderileceği ileti yolu belirtir. 

```azurecli
# Create the route for the storage endpoint.
az iot hub route create \
  --name $routeName \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName \
  --enabled \
  --condition $condition
```

### <a name="route-to-a-service-bus-queue"></a>Bir Service Bus kuyruğuna yönlendirme

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. Service Bus kuyruğu için bağlantı dizesini almak için tanımlanan doğru haklara sahip bir yetkilendirme kuralı oluşturmanız gerekir. Aşağıdaki betiği bir yetkilendirme kuralı için adlı Service Bus kuyruğu oluşturur `sbauthrule`ve hakları ayarlar `Listen Manage Send`. Bu yetkilendirme kuralı tanımlandıktan sonra sıra için bağlantı dizesini almak için kullanabilirsiniz.

```azurecli
# Create the authorization rule for the Service Bus queue.
az servicebus queue authorization-rule create \
  --name "sbauthrule" \
  --namespace-name $sbNamespace \
  --queue-name $sbQueueName \
  --resource-group $resourceGroup \
  --rights Listen Manage Send \
  --subscription $subscriptionID
```

Artık Service Bus kuyruğuna bağlantı dizesini almak için yetkilendirme kuralını kullanın.

```azurecli
# Get the Service Bus queue connection string.
# The "-o tsv" ensures it is returned without the default double-quotes.
sbqConnectionString=$(az servicebus queue authorization-rule keys list \
  --name "sbauthrule" \
  --namespace-name $sbNamespace \
  --queue-name $sbQueueName \
  --resource-group $resourceGroup \
  --subscription $subscriptionID \
  --query primaryConnectionString -o tsv)

# Show the Service Bus queue connection string.
echo "service bus queue connection string = " $sbqConnectionString
```

Şimdi yönlendirme uç nokta ve Service Bus kuyruğuna ileti yolunu ayarlayın. Bu değişkenler ayarlanır:

**Uçnoktaadı**: Bu alan, uç nokta tanımlayan addır. 

**endpointType**: Bu alan, uç nokta türüdür. Bu değer ayarlanmalıdır `azurestoragecontainer`, `eventhub`, `servicebusqueue`, veya `servicebustopic`. Amacınıza buraya ayarlayın `servicebusqueue`.

**Routetablename**: Bu alan, ayarladığınız yol adıdır. 

**Koşul**: Bu alan için bu endpoint gönderilen iletileri için filtre uygulamak için kullanılan sorgu gereklidir. Service Bus kuyruğuna yönlendirilen iletileri için sorgu koşulu `level="critical"`.

Yönlendirme uç noktası için Azure CLI ve Service Bus kuyruğu için ileti yolu aşağıda verilmiştir.

```azurecli
endpointName="ContosoSBQueueEndpoint"
endpointType="ServiceBusQueue"
routeName="ContosoSBQueueRoute"
condition='level="critical"'

# Set up the routing endpoint for the Service Bus queue.
# This uses the Service Bus queue connection string.
az iot hub routing-endpoint create \
  --connection-string $sbqConnectionString \
  --endpoint-name $endpointName \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --resource-group $resourceGroup 

# Set up the message route for the Service Bus queue endpoint.
az iot hub route create --name $routeName \
  --hub-name $iotHubName \
  --source-type devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName \
  --enabled \
  --condition $condition
  ```

### <a name="view-message-routing-in-the-portal"></a>Portalda ileti yönlendirme görüntüleyin

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ayarlanan kaynakları ve yapılandırılmış ileti yollarını edindikten sonra IOT hub'ına ileti göndermek ve farklı hedeflere görebileceği hakkında bilgi edinmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [2. Kısım - ileti yönlendirme sonuçlarını görüntüleme](tutorial-routing-view-message-routing-results.md)