---
title: Öğretici - Azure IOT Hub ileti zenginleştirmelerinin kullanma
description: Azure IOT Hub iletilerini ileti zenginleştirmelerinin kullanmayı gösteren öğretici.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: e4906bf9f2aead69c315ddb7b2e3b10489378d87
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259082"
---
# <a name="tutorial-using-azure-iot-hub-message-enrichments-preview"></a>Öğretici: Azure IOT Hub ileti zenginleştirmelerinin (Önizleme) kullanma

*İleti zenginleştirmelerinin* IOT Hub'ına özelliğidir *damga* iletileri belirlenen uç noktasına gönderilmeden önce ek bilgilerle iletileri. İleti zenginleştirmelerinin kullanmak için bir neden, aşağı akış işleme basitleştirmek için kullanılan veri eklemektir. Örneğin, cihaz ikizi etiketi ile cihaz telemetri iletilerini zenginleştirme, müşterilerin, cihaz ikizi için bu bilgileri API çağrıları yapmak için yük azaltabilir. Daha fazla bilgi için [ileti zenginleştirmelerinin bakış](iot-hub-message-enrichments-overview.md).

Bu öğreticide, kaynakları, ayarlamak için Azure CLI kullanmak için iki farklı depolama kapsayıcılar--gösteren iki uç nokta dahil olmak üzere **zenginleştirilmiş** ve **özgün**. Kullanmanız [Azure portalında](https://portal.azure.com) uç noktası ile gönderilen iletilere uygulanacak ileti zenginleştirmelerinin yapılandırmak için **zenginleştirilmiş** depolama kapsayıcısı. İletileri IOT Hub'ına, her iki depolama kapsayıcılarına yönlendirilen gönderir. Uç nokta için gönderilen iletileri yalnızca **zenginleştirilmiş** depolama kapsayıcısı zenginleştirilmiş.

Bu öğreticiyi tamamlamak için gerçekleştirdiğiniz görevler şunlardır:

**IOT Hub ileti zenginleştirmelerinin kullanma**
> [!div class="checklist"]
> * Azure CLI'yı kullanarak kaynakları--bir IOT hub'ı, iki uç nokta ile bir depolama hesabı ve yönlendirme yapılandırması oluşturun.
> * İleti zenginleştirmelerinin yapılandırmak için Azure portalını kullanın.
> * Bir IOT hub'ına iletileri gönderen bir cihaza benzetim bir uygulamayı çalıştırın.
> * Sonuçları görüntülemek ve ileti zenginleştirmelerinin beklendiği gibi çalıştığını doğrulayın.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* [Visual Studio](https://www.visualstudio.com/)’yu yükleyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="retrieve-the-sample-code"></a>Örnek kodu alın

İndirme [IOT cihaz benzetimi](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) ve sıkıştırmasını açın. Bu depo, IOT hub'ına ileti göndermek için kullanacağınız de dahil olmak üzere çeşitli uygulamaların içinde sahiptir.

Bu yükleme ayrıca ileti zenginleştirmelerinin test etmek için kullanılan kaynakları oluşturmak için komut dosyası içerir. İçinde /azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/iothub_msgenrichment_cli.azcli betiğidir. Şimdilik komut bulun ve kullanın. Ayrıca, komut dosyasını doğrudan makalesinden kopyalayabilirsiniz.

Test başlatmaya hazır olduğunuzda, IOT hub'ınıza ileti göndermek için bu indirmeyi cihaz benzetimi uygulamadan kullanır.

## <a name="set-up-and-configure-resources"></a>Ayarlama ve kaynakları yapılandırma

Azure CLI betiği gerekli kaynakları oluşturmaya ek olarak, ayrı depolama kapsayıcıları uç noktalar için iki yol da yapılandırır. Yönlendirme yapılandırma hakkında daha fazla bilgi için bkz. [yönlendirme öğretici](tutorial-routing.md). Kaynaklar ayarladıktan sonra kullandığınız [Azure portalında](https://portal.azure.com) ileti zenginleştirmelerinin her uç noktası için yapılandırın ve ardından test adımına devam edin.

> [!NOTE]
> Tüm iletiler, hem Uç noktalara yönlendirilir, ancak yalnızca yapılandırılmış ileti zenginleştirmelerinin ile uç noktasına giden iletileri zenginleştirilmiş.
>

Aşağıdaki betiği kullanın ya da indirilen depo /resources klasöründe bulunan komut dosyasını açın. Betik gerçekleştireceğiniz adımlar şunlardır:

* Bir IoT Hub oluşturma.
* Bir depolama hesabı oluşturun.
* --Biri zenginleştirilmiş iletiler için ve biri değil zenginleştirilmiş iletileri için depolama hesabında iki kapsayıcı oluşturun.
* İki farklı depolama hesapları için üretim akışı ayarlayabilirsiniz.
    * Her Depolama hesabı kapsayıcı için bir uç noktası oluşturun.
    * Her Depolama hesabı kapsayıcı uç noktaları için bir yol oluşturun.

IOT hub'ı adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adları vardır. Betik çalıştırmasını sağlamak için bu kaynak adları adlı rastgele alfasayısal bir değer eklenir *randomValue*. RandomValue komut dosyasının üst kısmında bir kez oluşturulur ve komut dosyası gerektiğinde kaynak adları için eklenmiş. Rastgele olmasını istemiyorsanız, boş bir dize veya belirli bir değere ayarlayabilirsiniz.

Zaten yapmadıysanız, açık bir [Bash Cloud Shell penceresini.](https://shell.azure.com). Sıkıştırması açılmış depoda komut dosyasını açın, kullanmak, tümünü seçmek için Ctrl-A ve ardından bunu kopyalamak için Ctrl-C. Alternatif olarak, aşağıdaki CLI betiği kopyalayın veya doğrudan cloud shell'de açın. Komut satırına sağ tıklatıp seçerek betiği Azure cloud shell penceresine yapıştırın **Yapıştır**. Betik, bir deyim teker teker çalıştırılır. Komut çalışmayı bırakan seçin **Enter** son komut çalıştığından emin olmak için. Aşağıdaki kod bloğu, ne yaptığını açıklayan yorumlar ile kullanılan betik gösterir.

Betiği tarafından oluşturulan kaynaklar aşağıda verilmiştir. **Zenginleştirilmiş** kaynak zenginleştirmelerinin ile iletiler için olduğu anlamına gelir. **Özgün** kaynak değil zenginleştirilmiş iletiler için olduğu anlamına gelir.

| Ad | Değer |
|-----|-----|
| Kaynak grubu | ContosoResourcesMsgEn |
| Kapsayıcı adı | Özgün  |
| Kapsayıcı adı | Zenginleştirilmiş  |
| IOT cihaz adı | Contoso-Test-Device |
| IOT hub'ı adı | ContosoTestHubMsgEn |
| Depolama hesabı adı | contosostorage |
| uç nokta adı 1 | ContosoStorageEndpointOriginal |
| uç nokta adı 2 | ContosoStorageEndpointEnriched|
| Rota adı 1 | ContosoStorageRouteOriginal |
| Rota adı 2 | ContosoStorageRouteEnriched |

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing rules.
subscriptionID=$(az account show --query id -o tsv)

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
location=westus2
resourceGroup=ContosoResourcesMsgEn
containerName1=original
containerName2=enriched
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique,
#   so add a random value to the end.
iotHubName=ContosoTestHubMsgEn$randomValue
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# You need a storage account that will have two containers
#   -- one for the original messages and
#   one for the enriched messages.
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
#    You need this to create the containers.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the containers in the storage account.
az storage container create --name $containerName1 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

az storage container create --name $containerName2 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
# If you are using Cloud Shell, you can scroll the window back up to retrieve this value.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

##### ROUTING FOR STORAGE #####

# You're going to have two routes and two endpoints.
# One points to container1 in the storage account
#   and includes all messages.
# The other points to container2 in the same storage account
#   and only includes enriched messages.

endpointType="azurestoragecontainer"
endpointName1="ContosoStorageEndpointOriginal"
endpointName2="ContosoStorageEndpointEnriched"
routeName1="ContosoStorageRouteOriginal"
routeName2="ContosoStorageRouteEnriched"

# for both endpoints, retrieve the messages going to storage
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)

# Create the routing endpoints and routes.
# Set the encoding format to either avro or json.

# This is the endpoint for container 1, for endpoint messages that are not enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName1 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName1 \
  --resource-group $resourceGroup \
  --encoding json

# This is the endpoint for container 2, for endpoint messages that are enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName2 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName2 \
  --resource-group $resourceGroup \
  --encoding json

# This is the route for messages that are not enriched.
# Create the route for the first storage endpoint.
az iot hub route create \
  --name $routeName1 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName1 \
  --enabled \
  --condition $condition

# This is the route for messages that are not enriched.
az iot hub route create \
  --name $routeName2 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName2 \
  --enabled \
  --condition $condition
```

Bu noktada, tüm küme hazır kaynakları ve yönlendirme yapılandırılır. Portalda ileti yönlendirme yapılandırmasını görüntülemek ve ileti zenginleştirmelerinin için giden iletiler için ayarlama **zenginleştirilmiş** depolama kapsayıcısı.

### <a name="view-routing-and-configure-the-message-enrichments"></a>Yönlendirme görüntülemek ve ileti zenginleştirmelerinin yapılandırma

1. Seçerek IOT Hub'ınıza gidin **kaynak grupları**, ardından Bu öğretici için ayarlanan kaynak grubunu seçin (**ContosoResources_MsgEn**). IOT hub'ı listede bulun ve seçin. Seçin *ileti yönlendirme** IOT hub'ının.

   ![İleti yönlendirme seçin](./media/tutorial-message-enrichments/select-iot-hub.png)

   İleti yönlendirme bölmesi olan üç sekme-- **yollar**, **özel uç noktalar**, ve **zenginleştirin iletileri**. Komut dosyası tarafından ayarlanan yapılandırma görmek için ilk iki sekme göz atabilirsiniz. Üçüncü sekme ileti zenginleştirmelerinin eklemek için kullanın. Şimdi adlı depolama kapsayıcısı için uç nokta için giden iletiler zenginleştirin **zenginleştirilmiş**. Ad ve değer girin ve ardından uç noktayı seçin **ContosoStorageEndpointEnriched** aşağı açılan listeden. IOT hub'ı adı ekler bir iyileştirmesini ayarlama bir örnek aşağıda verilmiştir:

   ![İlk zenginleştirme Ekle](./media/tutorial-message-enrichments/add-message-enrichments.png)

2. Bu değerleri ContosoStorageEndpointEnriched uç noktası için listesine ekleyin.

   | Ad | Değer | Uç noktası (açılan listesi) |
   | ---- | ----- | -------------------------|
   | myIotHub | $iothubname | AzureStorageContainers > ContosoStorageEndpointEnriched |
   | Cihaz konumu | $twin.tags.location | AzureStorageContainers > ContosoStorageEndpointEnriched |
   |CustomerID | 6ce345b8-1e4a-411e-9398-d34587459a3a | AzureStorageContainers > ContosoStorageEndpointEnriched |

   > [!NOTE]
   > Cihazınızı bir çifti yoksa buraya koyun değer ileti zenginleştirmelerinin değeri için bir dize olarak damgalı. Cihaz ikizi bilgi, portalda hub'ınıza gidin görmek için ardından **IOT cihazları**Cihazınızı seçin ve ardından **cihaz ikizi** sayfanın üstünde.
   >
   > (Örneğin, konum) etiketler ekleyebilir ve isterseniz, belirli bir değere ayarlamak için ikizi bilgileri düzenleyebilirsiniz. Daha fazla bilgi için bkz. [IoT Hub'daki cihaz ikizlerini kavrama ve kullanma](iot-hub-devguide-device-twins.md)

3. İşlemi tamamladığınızda, bölmenize Bu resme benzer görünmelidir:

   ![Eklenen tüm zenginleştirmelerinin içeren tablo](./media/tutorial-message-enrichments/all-message-enrichments.png)

4. Seçin **Uygula** değişiklikleri kaydedin.

## <a name="send-messages-to-the-iot-hub"></a>IOT Hub'ına ileti gönderme

İleti zenginleştirmelerinin uç nokta için yapılandırılmış olan, IOT Hub'ına ileti göndermek için sanal cihaz uygulaması çalıştırın. Hub'ı aşağıdaki kurallar ile ayarlanan:

* Depolama uç noktasına ContosoStorageEndpointOriginal yönlendirilmiş iletiler değil zenginleştirilmiş ve depolama kapsayıcısında depolanan `original`.

* Depolama uç noktasına ContosoStorageEndpointEnriched yönlendirilmiş iletiler zenginleştirilmiş ve depolama kapsayıcısında depolanan `enriched`.

Sanal cihaz uygulaması sıkıştırması indirmeyle uygulamalardan biridir. Uygulama, her biri farklı bir ileti yönlendirme yöntemleri için iletileri gönderir. [yönlendirme öğretici](tutorial-routing.md); Bu, Azure depolama içerir.

Çözüm dosyası (IoT_SimulatedDevice.sln) kodu Visual Studio'da açmak için çift tıklayın ve ardından program.cs dosyasını açın. Yedek `{your hub name}` ile IOT hub adı. IOT hub ana bilgisayar adı biçimi **{hub adınız} .azure devices.net**. Bu öğreticide, hub ana bilgisayar adı olan **ContosoTestHubMsgEn.azure devices.net**. Ardından, yedek `{device key}` cihaz anahtarı ile kaynakları oluşturmak için komut dosyasını çalıştırırken daha önce kaydedildi.

Cihaz anahtarı sahip değilseniz, portaldan alabilirsiniz. Oturum açtıktan sonra Git **kaynak grupları**, kaynak grubunuzu seçin ve ardından IOT Hub'ınızı seçin. Altına bakın **IOT cihazları** test cihazınız için ve Cihazınızı seçin. Kopyala simgesini seçin **birincil anahtar** panoya kopyalamak için.

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHubMsgEn.azure-devices.net";
        // This is the primary key for the device. This is in the portal.
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key.
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Çalıştırma ve test etme

Konsol uygulamasını çalıştırın. Birkaç dakika bekleyin. Gönderilen iletileri uygulamanın konsol ekranında görüntülenir.

Uygulama, IoT hub'ına her saniye yeni bir cihazdan buluta iletisi gönderir. İleti, cihaz kimliği, sıcaklık, nem düzeyi ve ileti düzeyi (varsayılan `normal` değeriyle) bilgileriyle bir JSON seri hale getirilmiş nesnesi içerir. Rastgele bir düzeyi atar `critical` veya `storage`, varsayılan uç nokta veya depolama hesabına yönlendirilecek ve iletinin işlenmemesine. Gönderilen iletileri **zenginleştirilmiş** depolama hesabında bir kapsayıcıda zenginleştirilmiş.

Birkaç depolama ileti gönderildikten sonra verileri görüntüleyin.

1. Seçin **kaynak grupları**, ardından kaynak grubunuzun (ContosoResourcesMsgEn) bulun ve seçin.

2. Depolama hesabınızı (contosostorage) seçin. Ardından **Depolama Gezgini (Önizleme)** soldaki seçimi bölmesinden.

   ![Depolama Gezgini seçin](./media/tutorial-message-enrichments/select-storage-explorer.png)

   Seçin **BLOB KAPSAYICILARI** kullanılabilecek iki kapsayıcıları görmek için.

   ![Depolama hesabındaki kapsayıcıları bakın](./media/tutorial-message-enrichments/show-blob-containers.png)

Kapsayıcıdaki iletiler olarak adlandırılan **zenginleştirilmiş** iletilerine dahil ileti zenginleştirmelerinin sahip. Kapsayıcıdaki iletiler olarak adlandırılan **özgün** hiçbir zenginleştirmelerinin ham iletilerle sahip olur. Altına alın ve en son ileti dosyası açın ve diğer kapsayıcı kapsayıcıdaki iletilere eklenen hiçbir zenginleştirmelerinin olmadığını doğrulamak için de aynısını yapın kadar kapsayıcıları birine detayına gidin.

Zenginleştirilmiş iletilere göz baktığınızda, "My IOT hub'ı" konumu yanı sıra hub adı ile ve bu gibi müşteri kimliği görmeniz gerekir:

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage","my IoT Hub":"contosotesthubmsgen3276","device location":"$twin.tags.location","customerID":"6ce345b8-1e4a-411e-9398-d34587459a3a"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

Burada da bir unenriched iletisi. "Hiçbir zenginleştirmelerinin Bu uç noktaya sahip olduğundan"My IOT hub'ı ","cihaz konumu"ve"CustomerID"Burada gösterilmez.

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz tüm kaynakları kaldırmak istiyorsanız, kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda, IOT hub'ı, depolama hesabı ve kaynak grubu kaldırılır.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>Kaynakları temizlemek için Azure CLI kullanma

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın. `$resourceGroup` ayarlandı **ContosoResources** Bu öğreticinin geri başında.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yapılandırılmış ve aşağıdaki adımları kullanarak IOT Hub iletilerini ekleme ileti zenginleştirmelerinin test:

**IOT Hub ileti zenginleştirmelerinin kullanma**
> [!div class="checklist"]
> * Azure CLI'yı kullanarak kaynakları--bir IOT hub'ı, bir depolama hesabı ile iki enendpoints ve yönlendirme yapılandırması oluşturun.
> * İleti zenginleştirmelerinin yapılandırmak için Azure portalını kullanın.
> * IOT cihaz gönderen iletisini hub'a taklit eden bir uygulamayı çalıştırın.
> * Sonuçları görüntülemek ve ileti zenginleştirmelerinin beklendiği gibi çalıştığını doğrulayın.

İleti zenginleştirmelerinin hakkında daha fazla bilgi için bkz: [ileti zenginleştirmelerinin bakış](iot-hub-message-enrichments-overview.md).

İleti yönlendirme hakkında daha fazla bilgi için şu makalelere bakın:

* [Farklı uç noktalar için CİHAZDAN buluta iletileri göndermek için IOT Hub ileti yönlendirme kullanın](iot-hub-devguide-messages-d2c.md)

* [Öğretici: IOT Hub'ın Yönlendirme](tutorial-routing.md)