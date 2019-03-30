---
title: Azure CLI ve Azure portalını kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma | Microsoft Docs
description: Azure CLI ve Azure portalını kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/12/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 2f382c31c6bfb6ab71afd495c4c3f702715633c0
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661897"
---
# <a name="tutorial-use-the-azure-cli-and-azure-portal-to-configure-iot-hub-message-routing"></a>Öğretici: IOT Hub ileti yönlendirme yapılandırmak için Azure CLI ve Azure Portalı'nı kullanın

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="use-the-azure-cli-to-create-the-base-resources"></a>Temel kaynakları oluşturmak için Azure CLI kullanma

Bu öğretici, temel kaynakları oluşturmak için Azure CLI'yı kullanır ve ardından kullanan [Azure portalında](https://portal.azure.com) ileti yönlendirmeyi yapılandırma ve test için sanal cihaz ayarlama işlemini göstermek için.

IOT hub'ı adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adları vardır. Bunu kolaylaştırmak için bu kaynak adları adlı rastgele alfasayısal bir değer eklenir *randomValue*. RandomValue komut dosyasının üst kısmında bir kez oluşturulur ve komut dosyası gerektiğinde kaynak adları için eklenmiş. Rastgele olmasını istemiyorsanız, boş bir dize veya belirli bir değere ayarlayabilirsiniz.

Kopyalama, aşağıdaki komut dosyasını Cloud shell'e yapıştırın ve Enter tuşuna basın. Bu betik bir satır aynı anda çalışır. Bu depolama hesabı, IOT hub'ı, Service Bus Namespace ve Service Bus kuyruğu dahil olmak üzere bu öğreticide, temel kaynakları oluşturur.

Hata ayıklama hakkında bir Not: Bu betik, devamlılık sembol kullanır (ters eğik çizgi `\`) kod daha okunabilir yapmak için. Betik çalıştırılırken bir sorun varsa, boşluk olmayan herhangi bir ters eğik çizgi sonra emin olun.

```azurecli-interactive
# This retrieves the subscription id of the account 
#   in which you're logged in.
# This field is used to set up the routing rules.
subscriptionID=$(az account show --query id)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# Set the values for the resource names that 
#   don't have to be globally unique.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults

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

```

Temel kaynaklar ayarlanır, içinde ileti yönlendirme yapılandırabilirsiniz [Azure portalında](https://portal.azure.com).

## <a name="set-up-message-routing"></a>İleti yönlendirmeyi ayarlama

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

### <a name="route-to-a-storage-account"></a>Bir depolama hesabına yönlendirme

Şimdi depolama hesabı için yönlendirmeyi ayarlayın. İleti Yönlendirme bölmesine gider ve bir yol eklersiniz. Yolu eklerken, yol için yeni bir uç nokta tanımlayın. Bu yönlendirme ayarlandıktan sonra nereye iletileri **düzeyi** özelliği **depolama** bir depolama hesabına otomatik olarak yazılır. 

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

1. İçinde [Azure portalında](https://portal.azure.com)seçin **kaynak grupları**, ardından kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

2. Kaynak listesi altında IOT hub'ı seçin. Bu öğreticide **ContosoTestHub** kullanılır.

3. Seçin **ileti yönlendirme**. İçinde **ileti yönlendirme** bölmesinde +**Ekle**. Üzerinde **bir yol eklemek** bölmesinde +**Ekle** desteklenen uç noktalar aşağıdaki resimde gösterildiği göstermek için uç nokta alanın yanındaki:

   ![Bir rota için bir uç nokta ekleme işlemini Başlat](./media/tutorial-routing/message-routing-add-a-route-w-storage-ep.png)

4. **Blob depolama**'yı seçin. Gördüğünüz **depolama uç noktası ekleme** bölmesi.

   ![Bir uç nokta ekleme](./media/tutorial-routing/message-routing-add-storage-ep.png)

5. Uç nokta için bir ad girin. Bu öğreticide **ContosoStorageEndpoint**.

6. Seçin **bir kapsayıcı seçin**. Bu sizi depolama hesaplarınızın listesine götürür. Hazırlık adımlarında ayarladığınız hesabı seçin. Bu öğreticide **contosostorage** kullanılır. Söz konusu depolama hesabının içindeki kapsayıcıların listesi gösterilir. **Seçin** kapsayıcıyı hazırlama adımları ayarladığınız. Bu öğreticide **contosoresults** kullanılır. Geri **depolama uç noktası ekleme** bölmesi ve yaptığınız seçimleri bakın.

7. AVRO veya JSON kodlama ayarlayın. Bu öğretici için kalan alanlarda varsayılan değerleri kullanın. Seçilen bölge JSON kodlama desteklemiyorsa bu alan gri görünür.,

   > [!NOTE]
   > Blob adının biçimini **Blob dosya adı biçimi** ile ayarlayabilirsiniz. Varsayılan değer: `{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}`. Biçim {iothub}, {partition}, {YYYY}, {MM}, {DD}, {HH} ve {mm} öğelerini içermelidir, sıralama farklı olabilir.
   >
   > Örneğin varsayılan blob dosya adı biçimini kullandığınızda hub adı ContosoTestHub ve tarih/saat 30 Ekim 2018 10:56 ise blob adı şu şekilde olacaktır: `ContosoTestHub/0/2018/10/30/10/56`.
   > 
   > Bloblar Avro biçiminde yazılır.
   >

8. Seçin **Oluştur** depolama uç noktası oluşturma ve yol ekleyin. **Yol ekle** bölmesine dönersiniz.

9. Şimdi yönlendirme sorgusu bilgilerinin kalan kısmını tamamlayın. Bu sorgu, az önce uç nokta olarak eklediğiniz depolama kapsayıcısına ileti gönderme ölçütlerini belirtir. Ekrandaki alanları doldurun.

   **Ad**: Yönlendirme sorgunuz için bir ad girin. Bu öğreticide **ContosoStorageRoute**.

   **Uç nokta**: Bu yeni ayarladığınız uç nokta gösterir.

   **Veri kaynağı**: Seçin **cihaz Telemetri iletilerini** aşağı açılan listeden.

   **Rota etkinleştirme**: Bu alanı mutlaka `enabled`.
   
   **Yönlendirme sorgusu**: Girin `level="storage"` sorgu dizesi olarak.

   ![Depolama hesabı için yönlendirme sorgu oluşturma](./media/tutorial-routing/message-routing-finish-route-storage-ep.png)  

   **Kaydet**’i seçin. İşlem bittiğinde İleti Yönlendirme bölmesine döner ve burada depolama için yeni yönlendirme sorgunuzu görebilirsiniz. Yollar bölmesini kapatın; Kaynak grubu sayfasına dönersiniz.

### <a name="route-to-a-service-bus-queue"></a>Bir Service Bus kuyruğuna yönlendirme

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. İleti Yönlendirme bölmesine gider ve bir yol eklersiniz. Yolu eklerken, yol için yeni bir uç nokta tanımlayın. Bu rota ayarlandıktan sonra nereye iletileri **düzeyi** özelliği **kritik** tetikler sonra bilgileri içeren bir e-posta gönderen bir mantıksal uygulama, Service Bus kuyruğuna yazılır.

1. Kaynak grubunuzun sayfasında, IOT hub'ınızı seçin ve ardından **ileti yönlendirme**.

2. İçinde **ileti yönlendirme** bölmesinde +**Ekle**.

3. Üzerinde **bir yol eklemek** bölmesinde seçin +**Ekle** uç nokta alanın yanındaki. **Service Bus Kuyruğu**'nu seçin. **Service Bus Uç Noktası Ekle** bölmesini görürsünüz.

   ![Bir service bus uç noktası ekleme](./media/tutorial-routing/message-routing-add-sbqueue-ep.png)

4. Şu alanları doldurun:

   **Uç nokta adı**: Uç nokta için bir ad girin. Bu öğreticide **ContosoSBQueueEndpoint**.
   
   **Service Bus Namespace**: Hazırlama adımları sizin ayarladığınız service bus ad alanı seçmek için açılan listeyi kullanın. Bu öğreticide **ContosoSBNamespace** kullanılır.

   **Service Bus kuyruğu**: Service Bus kuyruğuna seçmek için açılan listeyi kullanın. Bu öğreticide **contososbqueue** kullanılır.

5. Seçin **Oluştur** Service Bus kuyruk uç noktası eklemek için. **Yol ekle** bölmesine dönersiniz.

6. Şimdi yönlendirme sorgusu bilgilerinin kalan kısmını tamamlarsınız. Bu sorgu, az önce uç nokta olarak eklediğiniz Service Bus kuyruğuna ileti gönderme ölçütlerini belirtir. Ekrandaki alanları doldurun. 

   **Ad**: Yönlendirme sorgunuz için bir ad girin. Bu öğreticide **ContosoSBQueueRoute**. 

   **Uç nokta**: Bu yeni ayarladığınız uç nokta gösterir.

   **Veri kaynağı**: Seçin **cihaz Telemetri iletilerini** aşağı açılan listeden.

   **Yönlendirme sorgusu**: Girin `level="critical"` sorgu dizesi olarak. 

   ![Service Bus kuyruğu için yönlendirme sorgu oluşturma](./media/tutorial-routing/message-routing-finish-route-sbq-ep.png)

7. **Kaydet**’i seçin. Yollar bölmesine dönüldüğünde, burada gösterildiği gibi yeni yönlendirme yollarınızın ikisini de görürsünüz.

   ![Yeni ayarladığınız yolları](./media/tutorial-routing/message-routing-show-both-routes.png)

8. Ayarladığınız seçerek özel uç noktalar görebilirsiniz **özel uç noktalar** sekmesi.

   ![Yeni ayarladığınız özel uç nokta](./media/tutorial-routing/message-routing-show-custom-endpoints.png)

9. İleti yönlendirme bölmesini kapatın; Kaynak grubu bölmesine dönersiniz.

## <a name="create-a-simulated-device"></a>Sanal cihaz oluşturma

[!INCLUDE [iot-hub-include-create- imulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ayarlanan kaynakları ve yapılandırılmış ileti yollarını edindikten sonra IOT hub'ına ileti göndermek ve farklı hedeflere görebileceği hakkında bilgi edinmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [2. Kısım - ileti yönlendirme sonuçlarını görüntüleme](tutorial-routing-view-message-routing-results.md)
