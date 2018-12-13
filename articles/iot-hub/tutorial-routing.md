---
title: Azure IoT Hub'ı (.NET) ile ileti yönlendirmeyi yapılandırma | Microsoft Docs
description: Azure IoT Hub'ı ile ileti yönlendirmeyi yapılandırma
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 09/11/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 6f1cd08e3c786a1d163a22b5da5150fde5f45b95
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135347"
---
# <a name="tutorial-configure-message-routing-with-iot-hub"></a>Öğretici: IOT Hub ile ileti yönlendirmeyi yapılandırma

[İleti yönlendirme](iot-hub-devguide-messages-d2c.md) IoT cihazlarınızdan yerleşik Olay Hub'ı ile uyumlu uç noktalara veya blob depolama, Service Bus Kuyruğu, Service Bus Konusu ve Olay Hub'ları gibi özel uç noktalara telemetri verileri gönderilmesine olanak tanır. İleti yönlendirmeyi yapılandırırken belirli bir koşula uyan yolu özelleştirmek için [yönlendirme sorguları](iot-hub-devguide-routing-query-syntax.md) oluşturabilirsiniz. Ayarlandıktan sonra, gelen veriler IoT Hub'ı tarafından otomatik olarak uç noktalara yönlendirilir. 

Bu öğreticide, IoT Hub'ı ile yönlendirme sorgularını ayarlamayı ve kullanmayı öğreneceksiniz. İletileri IoT cihazından blob depolama ve Service Bus kuyruğu gibi çeşitli hizmetlerden birine yönlendireceksiniz. Service Bus kuyruğuna yönelik iletiler bir Mantıksal Uygulama tarafından seçilecek ve e-postayla gönderilecek. Özel olarak yönlendirmenin ayarlanmadığı iletiler varsayılan uç noktaya gönderilir ve Power BI görselleştirmesinde görüntülenir.

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Azure CLI veya PowerShell kullanarak temel kaynakları (IoT hub'ı, depolama hesabı, Service Bus kuyruğu ve simülasyon cihazı) ayarlama.
> * IoT hub'ında depolama hesabı ve Service Bus kuyruğu için uç noktaları ve yolları yapılandırma.
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için bir Power BI görselleştirmesi oluşturun.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- [Visual Studio](https://www.visualstudio.com/)’yu yükleyin. 

- Varsayılan uç noktanın akış analizini yapmak için Power BI hesabı. ([Power BI'ı ücretsiz deneyin](https://app.powerbi.com/signupredirect?pbi_source=web).)

- Bildirim e-postalarını göndermek için Office 365 hesabı. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="set-up-resources"></a>Kaynakları ayarlama

Bu öğreticide bir IoT hub'ınız, depolama hesabınız ve Service Bus kuyruğunuz olmalıdır. Bu kaynaklar Azure CLI veya Azure PowerShell kullanılarak oluşturulabilir. Tüm kaynaklar için aynı kaynak grubunu ve konumunu kullanın. Sonunda kaynak grubunu silerek her şeyi tek adımda kaldırabilirsiniz.

Aşağıdaki bölümlerde bu gerekli adımların nasıl uygulanacağı açıklanır. CLI *veya* PowerShell yönergelerini izleyin.

1. Bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. 

2. S1 katmanında IoT hub'ı oluşturun. IOT hub'ınızı bir tüketici grubu ekleyin. Tüketici grubu Azure Stream Analytics tarafından veriler alınırken kullanılır.

   > [!NOTE]
   > Bu öğreticiyi tamamlamak için ücretli bir katmanda bir IOT hub'ı kullanmanız gerekir. Ücretsiz katman yalnızca bir uç noktasını ayarlamanıza olanak tanır ve Bu öğretici için birden fazla uç noktası gereklidir.
   > 

3. Standard_LRS çoğaltmasıyla standart bir V1 depolama hesabı oluşturun.

4. Service Bus ad alanı ve kuyruğu oluşturun. 

5. Hub'ınıza iletiler gönderen simülasyon cihazı için cihaz kimliği oluşturun. Test aşaması için anahtarı kaydedin.

### <a name="set-up-your-resources-using-azure-cli"></a>Azure CLI kullanarak kaynaklarınızı ayarlama

Bu betiği kopyalayıp Cloud Shell'e yapıştırın. Zaten oturum açmış olduğunuzu varsayarak, betiği bir kerede bir satır olmak üzere çalıştırır. 

Genel olarak benzersiz olması gereken değişkenlere `$RANDOM` eklenmiştir. Betik çalıştırıldığında ve değişkenler ayarlandığında, rastgele bir sayısal dize oluşturulur ve sabit dizenin sonuna eklenerek benzersiz hale getirilmesi sağlanır.

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device 

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$RANDOM
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, so add a random number to the end.
storageAccountName=contosostorage$RANDOM
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
echo "$storageAccountKey"

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off 

# The Service Bus namespace must be globally unique, so add a random number to the end.
sbNameSpace=ContosoSBNamespace$RANDOM
echo "Service Bus namespace = " $sbNameSpace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNameSpace \
    --location $location
    
# The Service Bus queue name must be globally unique, so add a random number to the end.
sbQueueName=ContosoSBQueue$RANDOM
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNameSpace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

```

### <a name="set-up-your-resources-using-azure-powershell"></a>Azure PowerShell kullanarak kaynaklarınızı ayarlama

Bu betiği kopyalayıp Cloud Shell'e yapıştırın. Zaten oturum açmış olduğunuzu varsayarak, betiği bir kerede bir satır olmak üzere çalıştırır.

Genel olarak benzersiz olması gereken değişkenlere `$(Get-Random)` eklenmiştir. Betik çalıştırıldığında ve değişkenler ayarlandığında, rastgele bir sayısal dize oluşturulur ve sabit dizenin sonuna eklenerek benzersiz hale getirilmesi sağlanır.

```azurepowershell-interactive
# Log into Azure account.
Login-AzureRMAccount

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"
$iotDeviceName = "Contoso-Test-Device"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, so add a random number to the end.
$iotHubName = "ContosoTestHub$(Get-Random)"
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzureRmIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzureRmIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random number to the end.
$storageAccountName = "contosostorage$(Get-Random)"
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
$storageContext = $storageAccount.Context 

# Create the container in the storage account.
New-AzureStorageContainer -Name $containerName `
    -Context $storageContext

# The Service Bus namespace must be globally unique,
#   so add a random number to the end.
$serviceBusNamespace = "ContosoSBNamespace$(Get-Random)"
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzureRmServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random number to the end.
$serviceBusQueueName  = "ContosoSBQueue$(Get-Random)"
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzureRmServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName 

```

Sonra bir cihaz kimliği oluşturun ve anahtarını daha sonra kullanmak üzere kaydedin. Bu cihaz kimliği simülasyon uygulaması tarafından IoT hub'ına iletileri göndermek için kullanılır. Bu özellik PowerShell'de sağlanmaz ama cihazı [Azure portalında](https://portal.azure.com) oluşturabilirsiniz.

1. [Azure portalını](https://portal.azure.com) açın ve Azure hesabınızla oturum açın.

2. **Kaynak grupları**'na tıklayın ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

3. Kaynak listesinde IoT hub'ınıza tıklayın. Bu öğreticide **ContosoTestHub** kullanılır. Hub bölmesinden **IoT Cihazları**'nı seçin.

4. **+ Ekle**'ye tıklayın. Cihaz Ekle bölmesinde cihaz kimliğini girin. Bu öğreticide **Contoso-Test-Device** kullanılır. Anahtarları boş bırakın ve **Anahtarları Otomatik Olarak Oluştur**'a tıklayın. **Cihazı IoT Hub'ına bağla** seçeneğinin etkinleştirildiğinden emin olun. **Kaydet**’e tıklayın.

   ![Cihaz ekle ekranı gösteren ekran görüntüsü.](./media/tutorial-routing/add-device.png)

5. Artık oluşturulduğuna göre, üretilen anahtarları görmek için cihaza tıklayın. Birincil anahtarda Kopyala simgesine tıklayın ve bu anahtarı bu öğreticinin test aşaması için Not Defteri gibi bir yere kaydedin.

   ![Anahtarlarla birlikte cihaz ayrıntılarını gösteren ekran görüntüsü.](./media/tutorial-routing/device-details.png)

## <a name="set-up-message-routing"></a>İleti yönlendirmeyi ayarlama

Simülasyon cihazı tarafından iletiye eklenen özellikler temelinde iletileri farklı kaynaklara yönlendireceksiniz. Özel yönlendirmesi olmayan iletiler varsayılan uç noktaya gönderilir (iletiler/olaylar). 

|değer |Sonuç|
|------|------|
|level="storage" |Azure Depolama'ya yazın.|
|level="critical" |Service Bus kuyruğuna yazın. Mantıksal Uygulama iletiyi kuyruktan alır ve Office 365 kullanarak iletiyi e-postayla gönderir.|
|default |Power BI'ı kullanarak bu verileri görüntüleyin.|

### <a name="routing-to-a-storage-account"></a>Depolama hesabına yönlendirme 

Şimdi depolama hesabı için yönlendirmeyi ayarlayın. İleti Yönlendirme bölmesine gider ve bir yol eklersiniz. Yolu eklerken, yol için yeni bir uç nokta tanımlayın. Bu ayarlandıktan sonra, **level** özelliği **storage** olarak ayarlanmış olan iletiler otomatik olarak depolama hesabına yazılır. 

Veriler, blob depolama alanına Avro biçiminde yazılır.

1. [Azure portalında](https://portal.azure.com) **Kaynak Grupları**'na tıklayın ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır. 

2. Kaynak listesinin altında IoT hub'ına tıklayın. Bu öğreticide **ContosoTestHub** kullanılır. 

3. **İleti Yönlendirme**'ye tıklayın. **İleti Yönlendirme** bölmesinde +**Ekle**'ye tıklayın. **Yol Ekle** bölmesinde, aşağıdaki resimde gösterildiği gibi Uç Nokta alanının yanındaki +**Ekle**'ye tıklayın:

   ![Yola uç nokta eklemeye başlamayı gösteren ekran görüntüsü.](./media/tutorial-routing/message-routing-add-a-route-w-storage-ep.png)

4. **Blob depolama**'yı seçin. **Depolama Uç Noktası Ekle** bölmesini görürsünüz. 

   ![Uç nokta eklemeyi gösteren ekran görüntüsü.](./media/tutorial-routing/message-routing-add-storage-ep.png)

5. Uç nokta için bir ad girin. Bu öğreticide **StorageContainer** kullanılır.

6. **Bir kapsayıcı seçin** öğesine tıklayın. Bu sizi depolama hesaplarınızın listesine götürür. Hazırlık adımlarında ayarladığınız hesabı seçin. Bu öğreticide **contosostorage** kullanılır. Söz konusu depolama hesabının içindeki kapsayıcıların listesi gösterilir. Hazırlık adımlarında ayarladığınız kapsayıcıyı seçin. Bu öğreticide **contosoresults** kullanılır. **Seç**'e tıklayın. **Uç nokta ekle** bölmesine dönersiniz. 

7. Bu öğretici için kalan alanlarda varsayılan değerleri kullanın. 

   > [!NOTE]
   > Blob adının biçimini **Blob dosya adı biçimi** ile ayarlayabilirsiniz. Varsayılan değer: `{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}`. Biçim {iothub}, {partition}, {YYYY}, {MM}, {DD}, {HH} ve {mm} öğelerini içermelidir, sıralama farklı olabilir. 
   > 
   > Örneğin varsayılan blob dosya adı biçimini kullandığınızda hub adı ContosoTestHub ve tarih/saat 30 Ekim 2018 10:56 ise blob adı şu şekilde olacaktır: `ContosoTestHub/0/2018/10/30/10/56`.
   > 
   > Bloblar Avro biçiminde yazılır.
   >

8. Depolama uç noktasını oluşturmak ve yola eklemek için **Oluştur**'a tıklayın. **Yol ekle** bölmesine dönersiniz.

9. Şimdi yönlendirme sorgusu bilgilerinin kalan kısmını tamamlayın. Bu sorgu, az önce uç nokta olarak eklediğiniz depolama kapsayıcısına ileti gönderme ölçütlerini belirtir. Ekrandaki alanları doldurun. 

   **Ad**: Yönlendirme sorgunuz için bir ad girin. Bu öğreticide **StorageRoute** kullanılır.

   **Uç nokta**: Bu yeni ayarladığınız uç nokta gösterir. 
   
   **Veri kaynağı**: Seçin **cihaz Telemetri iletilerini** aşağı açılan listeden.

   **Rota etkinleştirme**: Bu etkin olduğundan emin olun.
   
   **Yönlendirme sorgusu**: Girin `level="storage"` sorgu dizesi olarak. 

   ![Depolama hesabı için yönlendirme sorgusu oluşturmayı gösteren ekran görüntüsü.](./media/tutorial-routing/message-routing-finish-route-storage-ep.png)  
   
   **Kaydet**’e tıklayın. İşlem bittiğinde İleti Yönlendirme bölmesine döner ve burada depolama için yeni yönlendirme sorgunuzu görebilirsiniz. Yollar bölmesini kapatın; Kaynak grubu sayfasına dönersiniz.

### <a name="routing-to-a-service-bus-queue"></a>Service Bus kuyruğuna yönlendirme 

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. İleti Yönlendirme bölmesine gider ve bir yol eklersiniz. Yolu eklerken, yol için yeni bir uç nokta tanımlayın. Bu ayarlandıktan sonra, **level** özelliği **critical** olarak ayarlanmış iletiler Service Bus kuyruğuna yazılır; Mantıksal Uygulama tetiklenir ve bu da bilgilerin bulunduğu bir e-posta gönderir. 

1. Kaynak grubu sayfasında IoT hub'ınıza tıklayın ve sonra da **İleti Yönlendirme**'ye tıklayın. 

2. **İleti Yönlendirme** bölmesinde +**Ekle**'ye tıklayın. 

3. **Yol Ekle** bölmesinde, Uç Noktası alanının yanındaki +**Ekle**'ye tıklayın. **Service Bus Kuyruğu**'nu seçin. **Service Bus Uç Noktası Ekle** bölmesini görürsünüz. 

   ![Service Bus uç noktası ekleme işleminin ekran görüntüsü](./media/tutorial-routing/message-routing-add-sbqueue-ep.png)

4. Şu alanları doldurun:

   **Uç nokta adı**: Uç nokta için bir ad girin. Bu öğreticide **CriticalQueue** kullanılır.
   
   **Service Bus Namespace**: Açılan listede gösterilmesi için bu alan; tıklayın. Hazırlama adımları sizin ayarladığınız service bus ad alanı seçin. Bu öğreticide **ContosoSBNamespace** kullanılır.

   **Service Bus kuyruğu**: Açılan listede gösterilmesi için bu alan; tıklayın. Service Bus kuyruğu, açılır listeden seçin. Bu öğreticide **contososbqueue** kullanılır.

5. Service Bus kuyruğu uç noktasını eklemek için **Oluştur**'a tıklayın. **Yol ekle** bölmesine dönersiniz. 

6.  Şimdi yönlendirme sorgusu bilgilerinin kalan kısmını tamamlarsınız. Bu sorgu, az önce uç nokta olarak eklediğiniz Service Bus kuyruğuna ileti gönderme ölçütlerini belirtir. Ekrandaki alanları doldurun. 

   **Ad**: Yönlendirme sorgunuz için bir ad girin. Bu öğreticide **SBQueueRoute** kullanılır. 

   **Uç nokta**: Bu yeni ayarladığınız uç nokta gösterir.

   **Veri kaynağı**: Seçin **cihaz Telemetri iletilerini** aşağı açılan listeden.

   **Yönlendirme sorgusu**: Girin `level="critical"` sorgu dizesi olarak. 

   ![Service Bus kuyruğu için yönlendirme sorgusu oluşturmayı gösteren ekran görüntüsü.](./media/tutorial-routing/message-routing-finish-route-sbq-ep.png)

7. **Kaydet**’e tıklayın. Yollar bölmesine dönüldüğünde, burada gösterildiği gibi yeni yönlendirme yollarınızın ikisini de görürsünüz.

   ![Az önce ayarladığınız yolları gösteren ekran görüntüsü.](./media/tutorial-routing/message-routing-show-both-routes.png)

8. Ayarladığınız özel uç noktaları **Özel Uç Noktalar** sekmesine tıklayarak görebilirsiniz.

   ![Az önce ayarladığınız özel uç noktaları gösteren ekran görüntüsü.](./media/tutorial-routing/message-routing-show-custom-endpoints.png)

9. İleti yönlendirme bölmesini kapatın; Kaynak grubu bölmesine dönersiniz.

## <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma  

Service Bus kuyruğu kritik olarak belirlenmiş iletileri almak için kullanılacaktır. Service Bus kuyruğunu izlemek ve kuyruğa ileti eklendiğinde bir e-posta göndermek için bir Mantıksal uygulama ayarlayın. 

1. [Azure portalında](https://portal.azure.com) **+ Kaynak oluştur**'a tıklayın. Arama kutusuna **mantıksal uygulama** yazın ve Enter tuşuna basın. Görüntülenen arama sonuçlarında Mantıksal Uygulama'yı seçin ve **Mantıksal uygulama oluştur** bölmesinden devam etmek için **Oluştur**'a tıklayın. Alanları doldurun. 

   **Ad**: Bu alan, mantıksal uygulama adıdır. Bu öğreticide **ContosoLogicApp** kullanılır. 

   **Abonelik**: Azure aboneliğinizi seçin.

   **Kaynak grubu**: Tıklayın **var olanı kullan** ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır. 

   **Konum**: Konumunuz olarak kullanın. Bu öğreticide **West US** kullanılır. 

   **Log Analytics**: Bu iki durumlu kapatılmalıdır. 

   ![Mantıksal Uygulama Oluştur ekranını gösteren ekran görüntüsü.](./media/tutorial-routing/create-logic-app.png)

   **Oluştur**’a tıklayın.

2. Şimdi Mantıksal Uygulama'ya gidin. Mantıksal Uygulama'ya ulaşmanın en kolay yolu **Kaynak grupları**'na tıklamak, kaynak grubunuzu seçmek (bu öğreticide **ContosoResources** kullanılır), sonra da kaynak listesinden Mantıksal Uygulama'yı seçmektir. Logic Apps Tasarımcısı sayfası gösterilir (sayfanın tamamını görmek için ekranı sağa doğru kaydırmanız gerekebilir). Logic Apps Tasarımcısı sayfasında, **Boş Mantıksal Uygulama +** adlı kutucuğu görene kadar ekranı aşağı kaydırın ve bu kutucuğa tıklayın. 

3. Bağlayıcı listesi görüntülenir. **Service Bus**'ı seçin. 

   ![Bağlayıcı listesini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-connectors.png)

4. Tetikleyici listesi görüntülenir. **Service Bus - Kuyrukta bir ileti alındığında (otomatik tamamla)** öğesini seçin. 

   ![Service Bus için tetikleyici listesini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-triggers.png)

5. Sonraki ekranda, Bağlantı Adı alanını doldurun. Bu öğreticide **ContosoConnection** kullanılır. 

   ![Service Bus kuyruğu için bağlantının ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-define-connection.png)

   Service Bus ad alanına tıklayın. Bu öğreticide **ContosoSBNamespace** kullanılır. Ad alanını seçtiğinizde, portal anahtarları almak için Service Bus ad alanını sorgular. **RootManageSharedAccessKey** öğesini seçin ve **Oluştur**'a tıklayın. 
   
   ![Bağlantı ayarlama işleminin bitirilmesini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-finish-connection.png)

6. Sonraki ekranda, açılan listeden kuyruğun adını seçin (bu öğreticide **contososbqueue** kullanılır). Kalan alanlar için varsayılan değerleri kullanabilirsiniz. 

   ![Kuyruk seçeneklerini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-queue-options.png)

7. Şimdi kuyrukta bir ileti alındığında e-posta gönderme eylemini ayarlayın. Logic Apps Tasarımcısı'nda **+ Yeni adım**'a tıklayarak bir adım ekleyin ve ardından **Eylem ekle**'ye tıklayın. **Eylem seçin** bölmesinde **Office 365 Outlook**'u bulun ve tıklayın. Tetikleyiciler ekranında **Office 365 Outlook - E-posta gönder**'i seçin.  

   ![Office 365 seçeneklerini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-select-outlook.png)

8. Ardından, bağlantıyı ayarlamak için Office 365 hesabınızda oturum açın. E-postaların alıcıları için e-posta adreslerini belirtin. Ayrıca konuyu da belirtin ve alıcının gövdede görmesini istediğiniz iletiyi yazın. Test amacıyla, alıcı olarak kendi e-posta adresinizi girin.

   Eklediğiniz iletinin içeriğini göstermek için **Dinamik içerik ekle**'ye tıklayın. **İçerik** öğesini seçin; e-postadaki iletiyi içerecektir. 

   ![Mantıksal uygulama için e-posta seçeneklerini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-send-email.png)

9. **Kaydet**’e tıklayın. Sonra Logic App Tasarımcısı'nı kapatın.

## <a name="set-up-azure-stream-analytics"></a>Azure Stream Analytics'i ayarlama

Verileri Power BI görselleştirmesinde görmek için, önce bir Stream Analytics işi ayarlayarak verileri alın. Yalnızca **düzeyi** **normal** olan iletilerin varsayılan uç noktaya gönderildiğini ve Power BI görselleştirmesi için Stream Analytics işi tarafından alınacağını unutmayın.

### <a name="create-the-stream-analytics-job"></a>Stream Analytics işi oluşturma

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işi**'ne tıklayın.

2. İş için aşağıdaki bilgileri girin.

   **İş adı**: İş adı. Adın genel olarak benzersiz olması gerekir. Bu öğreticide **contosoJob** kullanılır.

   **Kaynak grubu**: IOT hub tarafından kullanılan aynı kaynak grubunu kullanın. Bu öğreticide **ContosoResources** kullanılır. 

   **Konum**: Kurulum komut dosyasında kullandığınız konumun aynısını kullanın. Bu öğreticide **West US** kullanılır. 

   ![Stream Analytics işi oluşturma işleminin gösterildiği ekran görüntüsü.](./media/tutorial-routing/stream-analytics-create-job.png)

3. İşi oluşturma için **Oluştur**'a tıklayın. İşe dönmek için **Kaynak grupları**'na tıklayın. Bu öğreticide **ContosoResources** kullanılır. Kaynak grubunu seçin ve ardından kaynak listesinde Stream Analytics işine tıklayın. 

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

4. **İş Topolojisi**'nin altında **Girişler**'e tıklayın.

5. **Girişler** bölmesinde **Akış girişi ekle**'ye tıklayın ve IoT Hub'ını seçin. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Giriş diğer adı**: Bu öğreticide **contosoinputs** kullanılır.

   **Abonelik**: Aboneliğinizi seçin.

   **IOT hub'ı**: IOT hub'ı seçin. Bu öğreticide **ContosoTestHub** kullanılır.

   **Uç nokta**: Seçin **Mesajlaşma**. (İşlem İzleme'yi seçerseniz, gönderdiğiniz veriler yerine IoT hub'ı hakkındaki telemetri verilerini alırsınız.) 

   **Paylaşılan erişim ilkesi adı**: Seçin **iothubowner**. Paylaşılan Erişim İlkesi Anahtarı'nı portal sizin için doldurur.

   **Tüketici grubu**: Daha önce oluşturduğunuz tüketici grubu seçin. Bu öğreticide **contosoconsumers** kullanılır.
   
   Kalan alanlar için varsayılan değerleri kabul edin. 

   ![Stream Analytics işi için girişlerin ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/stream-analytics-job-inputs.png)

6. **Kaydet**’e tıklayın.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**'nin altında **Çıkışlar**'a tıklayın.

2. **Çıkışlar** bölmesinde **Ekle**'ye tıklayın ve **Power BI**'ı seçin. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Çıkış diğer adı**: Çıkış için benzersiz diğer adı. Bu öğreticide **contosooutputs** kullanılır. 

   **Veri kümesi adı**: Power BI'da kullanılacak veri kümesinin adı. Bu öğreticide **contosodataset** kullanılır. 

   **Tablo adı**: Power BI'da kullanılacak tablonun adı. Bu öğreticide **contosotable** kullanılır.

   Kalan alanlarda varsayılan değerleri kabul edin.

3. **Yetkilendir**'e tıklayın ve Power BI hesabınızda oturum açın.

   ![Stream Analytics işi için çıkışların ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/stream-analytics-job-outputs.png)

4. **Kaydet**’e tıklayın.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'ya tıklayın.

2. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin. Bu öğreticide **contosoinputs** kullanılır.

3. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin. Bu öğreticide **contosooutputs** kullanılır.

   ![Stream Analytics işi için sorgunun ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/stream-analytics-job-query.png)

4. **Kaydet**’e tıklayın.

5. Sorgu bölmesini kapatın. Bu sizi Kaynak Grubu'ndaki kaynakların görünümüne döndürür. Stream Analytics işine tıklayın. Bu öğreticide **contosoJob** olarak adlandırılmıştır.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde **Başlat** > **Şimdi** > **Başlat**'a tıklayın. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

Power BI raporunu ayarlamak için verilere ihtiyacınız vardır; bu nedenle, cihazı oluşturup cihaz simülasyon uygulamasını çalıştırdıktan sonra Power BI'ı ayarlarsınız.

## <a name="run-simulated-device-app"></a>Simülasyon Cihazı uygulamasını çalıştırma

Betik ayarlama bölümünün başlarında, IoT cihazı kullanarak simülasyonu yapılacak bir cihaz ayarlamıştınız. Bu bölümde, IoT Hub'a cihazdan buluta iletiler gönderen bir cihazın simülasyonunu yapan bir .NET konsol uygulaması indireceksiniz. Bu uygulama, farklı yönlendirme yöntemlerinin her biri için iletiler gönderir. 

[IoT Cihaz Simülasyonu](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) çözümünü indirin. İçinde çeşitli uygulamalar bulunan bir depo indirilir; aradığınız çözüm iot-hub/Tutorials/Routing/SimulatedDevice/ klasöründedir.

Kodu Visual Studio'da açmak için çözüm dosyasına (SimulatedDevice.sln) çift tıklayın, sonra da Program.cs'yi açın. `{iot hub hostname}` değerini IoT hub'ı konak adıyla değiştirin. IoT hub'ı konak adı **{iot-hub-adı}.azure-devices.net** biçimindedid. bu öğreticide, hub konak adı olarak **ContosoTestHub.azure-devices.net** kullanılır. Ardından, `{device key}` değerini daha önce simülasyon cihazını ayarlarken kaydettiğiniz cihaz anahtarıyla değiştirin. 

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Çalıştırma ve test etme 

Konsol uygulamasını çalıştırın. Birkaç dakika bekleyin. Uygulamanın konsol ekranında iletilerin gönderildiğini görebilirsiniz.

Uygulama, IoT hub'ına her saniye yeni bir cihazdan buluta iletisi gönderir. İleti, cihaz kimliği, sıcaklık, nem düzeyi ve ileti düzeyi (varsayılan `normal` değeriyle) bilgileriyle bir JSON seri hale getirilmiş nesnesi içerir. Buna rastgele olarak `critical` veya `storage` düzeyi atanır; bu, iletinin depolama hesabına veya Service Bus kuyruğuna (Mantıksal Uygulamanızı e-posta göndermesi için tetikler) yönlendirilmesine neden olur. Varsayılan (`normal`) okumalar, bundan sonra ayarlayacağınız BI raporunda görüntülenir.

Her şey düzgün ayarlandıysa, bu noktada aşağıdaki sonuçları görüyor olmalısınız:

1. Kritik iletilerle ilgili e-postalar almaya başlarsınız. 

   ![Sonuçta elde edilen e-postaların gösterildiği ekran görüntüsü.](./media/tutorial-routing/results-in-email.png)

   Bunun anlamı:

   * Service Bus kuyruğunda yönlendirme düzgün çalışıyor.
   * İletiyi Service Bus kuyruğundan alan Mantıksal Uygulama düzgün çalışıyor.
   * Mantıksal Uygulama'nın Outlook bağlayıcısı düzgün çalışıyor. 

2. [Azure portalında](https://portal.azure.com) **Kaynak grupları**'na tıklayın ve Kaynak Grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır. Depolama hesabını seçin, **Blob'lar** öğesine tıklayın ve Kapsayıcı seçin. Bu öğreticide **contosoresults** kullanılır. Bir klasör görüyor olmalısınız ve bir veya birden çok dosya görünceye kadar dizinlerde ayrıntıya gidebilirsiniz. Bu dosyalardan birini açın; depolama hesabına yönlendirilen girdileri içerir. 

   ![Depolamada sonuç dosyalarını gösteren ekran görüntüsü.](./media/tutorial-routing/results-in-storage.png)

Bunun anlamı:

   * Depolama hesabına yapılan yönlendirme düzgün çalışıyor.

Şimdi uygulamanız hala çalışırken, varsayılan yönlendirme aracılığıyla gelen iletileri görmek için Power BI görselleştirmesini ayarlayın. 

## <a name="set-up-the-power-bi-visualizations"></a>Power BI Görselleştirmelerini ayarlama

1. [Power BI](https://powerbi.microsoft.com/) hesabınızda oturum açın.

2. **Çalışma Alanları**'na gidin ve Stream Analytics işi için çıkış oluştururken ayarladığını çalışma alanını seçin. Bu öğreticide **My Workspace** kullanılır. 

3. **Veri kümeleri**'ne tıklayın.

   Stream Analytics işi için çıkış oluştururken belirttiğiniz veri kümesinin listelendiğini görüyor olmalısınız. Bu öğreticide **contosodataset** kullanılır. (Veri kümesinin ilk kez görüntülenmesi 5-10 dakika kadar sürebilir.)

4. **EYLEMLER**'in altında, ilk simgeye tıklayarak bir rapor oluşturun.

   ![Eylemler ve rapor simgesinin vurgulandığı Power BI çalışma alanını gösteren ekran görüntüsü.](./media/tutorial-routing/power-bi-actions.png)

5. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.

   * Rapor oluşturma sayfasında, çizgi grafik simgesine tıklayarak çizgi grafiği oluşturun.

   ![Görselleştirmeleri ve alanları gösteren ekran görüntüsü.](./media/tutorial-routing/power-bi-visualizations-and-fields.png)

   * **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin. Bu öğreticide **contosotable** kullanılır.

   * **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.

   * **temperature** öğesini **Değerler** üzerine sürükleyin.

   Bir çizgi grafik oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

6. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. İkinci grafiği ayarlamak için yukarıdaki adımları aynı şekilde izleyin ve x eksenine **EventEnqueuedUtcTime**, y eksenine de **humidity** öğelerini yerleştirin.

   ![İki grafiği olan Power BI raporunun son halini gösteren ekran görüntüsü.](./media/tutorial-routing/power-bi-report.png)

7. Raporu kaydetmek için **Kaydet**’e tıklayın.

Her iki grafikteki verileri de görüyor olmalısınız. Bunun anlamı:

   * Varsayılan uç noktaya yapılan yönlendirme düzgün çalışıyor.
   * Azure Stream Analytics işi doğru akış yapıyor.
   * Power BI Görselleştirmesi doğru ayarlanmış.

Power BI penceresinin en üstündeki Yenile düğmesine tıklayıp grafikleri yenileyerek en son verileri görebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Oluşturduğunuz tüm kaynakları kaldırmak istiyorsanız, kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda IoT hub'ını, Service Bus ad alanıyla kuyruğunu, Mantıksal Uygulamayı, depolama hesabını ve kaynak grubunun kendisini kaldırır. 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Power BI görselleştirmesinde kaynakları temizleme

[Power BI](https://powerbi.microsoft.com/) hesabınızda oturum açın. Çalışma alanınıza gidin. Bu öğreticide **My Workspace** kullanılır. Power BI görselleştirmesini kaldırmak için, DataSets'e gidin ve çöp kutusu simgesine tıklayarak veri kümesini silin. Bu öğreticide **contosodataset** kullanılır. Veri kümesini kaldırdığınızda, rapor da kaldırılır.

### <a name="clean-up-resources-using-azure-cli"></a>Azure CLI kullanarak kaynakları temizleme

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın.

```azurecli-interactive
az group delete --name $resourceGroup
```
### <a name="clean-up-resources-using-powershell"></a>PowerShell kullanarak kaynakları temizleme

Kaynak grubunu kaldırmak için [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanın. $resourceGroup, yeniden bu öğreticinin başındaki **ContosoIoTRG1** değerine ayarlanır.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri gerçekleştirerek IoT Hub'ı iletilerini farklı hedeflere yönlendirmek için ileti yönlendirme özelliğini kullanmayı öğrendiniz.  

> [!div class="checklist"]
> * Azure CLI veya PowerShell kullanarak temel kaynakları (IoT hub'ı, depolama hesabı, Service Bus kuyruğu ve simülasyon cihazı) ayarlama.
> * IoT hub'ında depolama hesabı ve Service Bus kuyruğu için uç noktaları ve yolları yapılandırma.
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için bir Power BI görselleştirmesi oluşturun.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleyin.

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
[Cihazlarınızı bir arka uç hizmetinden yapılandırma](tutorial-device-twins.md)
