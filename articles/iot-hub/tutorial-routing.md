---
title: Azure IoT Hub'ı (.NET) ile ileti yönlendirmeyi yapılandırma | Microsoft Docs
description: Azure IoT Hub'ı ile ileti yönlendirmeyi yapılandırma
services: iot-hub
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 0674ed033f77d7d2eca319d0b1e82171dfa4256d
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-configure-message-routing-with-iot-hub"></a>Öğretici: IoT Hub'ı ile ileti yönlendirmeyi yapılandırma

İleti Yönlendirme IoT cihazlarınızdan yerleşik Olay Hub'ı ile uyumlu uç noktalara veya blob depolama, Service Bus Kuyruğu, Service Bus Konusu ve Olay Hub'ları gibi özel uç noktalara telemetri verileri gönderilmesine olanak tanır. İleti Yönlendirme'yi yapılandırırken belirli bir kurala uyan yolu özelleştirmek için yönlendirme kuralları oluşturabilirsiniz. Ayarlandıktan sonra, gelen veriler IoT Hub'ı tarafından otomatik olarak uç noktalara yönlendirilir. 

Bu öğreticide, IoT Hub'ı ile yönlendirme kurallarını ayarlamayı ve kullanmayı öğreneceksiniz. İletileri IoT cihazından blob depolama ve Service Bus kuyruğu gibi çeşitli hizmetlerden birine yönlendireceksiniz. Service Bus kuyruğuna yönelik iletiler bir Mantıksal Uygulama tarafından seçilecek ve e-postayla gönderilecek. Özel olarak yönlendirmenin ayarlanmadığı iletiler varsayılan uç noktaya gönderilir ve PowerBI görselleştirmesinde gösterilir.

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Azure CLI veya PowerShell kullanarak temel kaynakları (IoT hub'ı, depolama hesabı, Service Bus kuyruğu ve simülasyon cihazı) ayarlama.
> * IoT hub'ında depolama hesabı ve Service Bus kuyruğu için uç noktaları ve yolları yapılandırma.
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için PowerBI görselleştirmesi oluşturma.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleme.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- [Windows için Visual Studio](https://www.visualstudio.com/) yükleyin. 

- Varsayılan uç noktanın akış analizini yapmak için PowerBI hesabı. ([PowerBI'ı ücretsiz deneyin](https://app.powerbi.com/signupredirect?pbi_source=web))

- Bildirim e-postalarını göndermek için Office 365 hesabı. 

Bu öğreticideki kurulum adımlarını uygulamak için Azure CLI'ye veya Azure PowerShell'e ihtiyacınız vardır. 

Azure CLI kullanmak için, Azure CLI'yi yerel olarak yükleyebilirsiniz ama biz Azure Cloud Shell kullanmanızı öneririz. Azure Cloud Shell, Azure CLI betiklerini çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır, dolayısıyla bunları yerel olarak yüklemeniz gerekmez. 

PowerShell kullanmak için, aşağıdaki yönergeleri kullanın ve bunu yerel olarak yükleyin. 

### <a name="azure-cloud-shell"></a>Azure Cloud Shell

Cloud Shell’i açmanın birkaç yolu vardır:

|  |   |
|-----------------------------------------------|---|
| Kod bloğunun sağ üst köşesindeki **Deneyin**’i seçin. | ![Bu makaledeki Cloud Shell](./media/tutorial-routing/cli-try-it.png) |
| Cloud Shell’i tarayıcınızda açın. | [![https://shell.azure.com/bash](./media/tutorial-routing/launchcloudshell.png)](https://shell.azure.com) |
| [Azure portalının](https://portal.azure.com) sağ üst köşesindeki menüde yer alan **Cloud Shell** düğmesini seçin. |    ![Portalda Cloud Shell](./media/tutorial-routing/cloud-shell-menu.png) |
|  |  |

### <a name="using-azure-cli-locally"></a>Azure CLI’yi yerel olarak kullanma

Cloud Shell kullanmak yerine CLI'yi yerel olarak kullanmayı tercih ederseniz, Azure CLI modülünün 2.0.30.0 veya daha sonraki bir sürümü gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

### <a name="using-powershell-locally"></a>PowerShell’i yerel olarak kullanma

Bu öğretici için Azure PowerShell modülünün 5.7 veya daha sonraki bir sürümü gerekir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="set-up-resources"></a>Kaynakları ayarlama

Bu öğreticide bir IoT hub'ınız, depolama hesabınız ve Service Bus kuyruğunuz olmalıdır. Bu kaynakların tümü Azure CLI veya Azure PowerShell kullanılarak oluşturulabilir. Tüm kaynaklar için aynı kaynak grubunu ve konumunu kullanın. Sonunda kaynak grubunu silerek her şeyi tek adımda kaldırabilirsiniz.

Aşağıdaki bölümlerde bu gerekli adımların nasıl uygulanacağı açıklanır. CLI *veya* PowerShell yönergelerini izleyin.

1. Bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. 

    <!-- When they add the Basic tier, change this to use Basic instead of Standard. -->

2. S1 katmanında IoT hub'ı oluşturun. IOT hub'ınızı bir tüketici grubu ekleyin. Tüketici grubu Azure Stream Analytics tarafından veriler alınırken kullanılır.

3. Standard_LRS çoğaltmasıyla standart bir V1 depolama hesabı oluşturun.

4. Service Bus ad alanı ve kuyruğu oluşturun. 

5. Hub'ınıza iletiler gönderen simülasyon cihazı için cihaz kimliği oluşturun. Test aşaması için anahtarı kaydedin.

### <a name="azure-cli-instructions"></a>Azure CLI yönergeleri

Bu betiği kullanmanın en kolay yolu, betiği kopyalayıp Cloud Shell'e yapıştırmaktır. Zaten oturum açmış olduğunuzu varsayarak, betiği bir kerede bir satır olmak üzere çalıştırır. 

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device 

# These resource names must be globally unique.
# You might need to change these if they are already in use by someone else.
iotHubName=ContosoTestHub 
storageAccountName=contosoresultsstorage 
sbNameSpace=ContosoSBNamespace 
sbQueueName=ContosoSBQueue

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

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

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNameSpace \
    --location $location
    
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

### <a name="powershell-instructions"></a>PowerShell yönergeleri

Bu betiği kullanmanın en kolay yolu [PowerShell ISE](/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise.md)'yi açmak, betiği panoya kopyalamak ve ardından betiğini tamamını betik penceresine yapıştırmaktır. Ardından, kaynak adı değerlerini (isterseniz) değiştirebilir ve betiğin tamamını çalıştırabilirsiniz. 

```azurepowershell-interactive
# Log into Azure account.
Login-AzureRMAccount

# Set the values for the resource names.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"
$iotDeviceName = "Contoso-Test-Device"

# These resource names must be globally unique.
# You might need to change these if they are already in use by someone else.
$iotHubName = "ContosoTestHub"
$storageAccountName = "contosoresultsstorage"
$serviceBusNamespace = "ContosoSBNamespace"
$serviceBusQueueName  = "ContosoSBQueue"

# Create the resource group to be used  
#   for all resources for this tutorial.
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

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

# Create the Service Bus namespace.
New-AzureRmServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

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
|default |Bu verileri PowerBI kullanarak görüntüleyin.|

### <a name="routing-to-a-storage-account"></a>Depolama hesabına yönlendirme 

Şimdi depolama hesabı için yönlendirmeyi ayarlayın. Uç nokta tanımlayın ve ardından bu uç nokta için bir yol ayarlayın. **level** özelliği **storage** olarak ayarlanmış olan iletiler otomatik olarak depolama hesabına yazılır.

1. [Azure portalında](https://portal.azure.com) **Kaynak Grupları**'na tıklayın ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır. Kaynak listesinin altında IoT hub'ına tıklayın. Bu öğreticide **ContosoTestHub** kullanılır. **Uç Noktalar**’a tıklayın. **Uç Noktalar** bölmesinde **+Ekle**'ye tıklayın. Aşağıdaki bilgileri girin:

   **Ad**: Uç nokta için bir ad girin. Bu öğreticide **StorageContainer** kullanılır.
   
   **Uç Nokta Türü**: Açılan listeden **Azure Depolama Kapsayıcısı**'nı seçin.

   Depolama hesaplarının listesini görmek için **Bir kapsayıcı seçin** öğesine tıklayın. Depolama hesabınızı seçin. Bu öğreticide **contosoresultsstorage** kullanılır. Ardından, kapsayıcıyı seçin. Bu öğreticide **contosoresults** kullanılır. **Seç**'e tıklayın; Uç nokta ekle bölmesine dönersiniz. 
   
   ![Uç nokta eklemeyi gösteren ekran görüntüsü.](./media/tutorial-routing/add-endpoint-storage-account.png)
   
   Uç nokta ekleme işlemini bitirmek için **Tamam**'a tıklayın.
   
2. IoT hub'ınızda **Yollar**'a tıklayın. İletileri az önce uç nokta olarak eklediğiniz depolama kapsayıcısına yönlendiren bir yönlendirme kuralı oluşturacaksınız. Yollar bölmesinin üst kısmındaki **+Ekle**'ye tıklayın. Ekrandaki alanları doldurun. 

   **Ad**: Yönlendirme kuralınız için bir ad girin. Bu öğreticide **StorageRule** kullanılır.

   **Veri kaynağı**: Açılan listeden **Cihaz İletileri**'ni seçin.

   **Uç nokta**: Az önce ayarladığınız uç noktayı seçin. Bu öğreticide **StorageContainer** kullanılır. 
   
   **Sorgu dizesi**: Sorgu dizesi olarak `level="storage"` girin. 

   ![Depolama hesabı için yönlendirme kuralı oluşturmayı gösteren ekran görüntüsü.](./media/tutorial-routing/create-a-new-routing-rule-storage.png)
   
   **Kaydet**’e tıklayın. İşlem bittiğinde Yollar bölmesine döner ve burada depolama için yeni yönlendirme kuralınızı görebilirsiniz. Yollar bölmesini kapatın; Kaynak grubu sayfasına dönersiniz.

### <a name="routing-to-a-service-bus-queue"></a>Service Bus kuyruğuna yönlendirme 

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. Uç nokta tanımlayın ve ardından bu uç nokta için bir yol ayarlayın. **level** özelliği **critical** olarak ayarlanmış iletiler Service Bus kuyruğuna yazılır; Mantıksal Uygulama tetiklenir ve bu da bilgilerin bulunduğu bir e-posta gönderir. 

1. Kaynak grubu sayfasında IoT hub'ınıza tıklayın ve sonra da **Uç Noktalar**'a tıklayın. **Uç Noktalar** bölmesinde **+Ekle**'ye tıklayın. Aşağıdaki bilgileri girin.

   **Ad**: Uç nokta için bir ad girin. Bu öğreticide **CriticalQueue** kullanılır. 

   **Uç Nokta Türü**: Açılan listeden **Service Bus kuyruğu**'nu seçin.

   **Service Bus ad alanı**: Açılan listeden bu öğretici için Service Bus ad alanını seçin. Bu öğreticide **ContosoSBNamespace** kullanılır.

   **Service Bus kuyruğu**: Açılan listeden Service Bus kuyruğunu seçin. Bu öğreticide **contososbqueue** kullanılır.

   ![Service Bus kuyruğu için uç nokta eklemeyi gösteren ekran görüntüsü.](./media/tutorial-routing/add-endpoint-sb-queue.png)

   Uç noktayı kaydetmek için **Tamam**’a tıklayın. İşlem tamamlandıktan sonra Uç Noktalar bölmesini kapatın. 
    
2. IoT hub'ınızda **Yollar**'a tıklayın. İletileri az önce uç nokta olarak eklediğiniz Service Bus kuyruğuna yönlendiren bir yönlendirme kuralı oluşturacaksınız. Yollar bölmesinin üst kısmındaki **+Ekle**'ye tıklayın. Ekrandaki alanları doldurun. 

   **Ad**: Yönlendirme kuralınız için bir ad girin. Bu öğreticide **SBQueueRule** kullanılır. 

   **Veri kaynağı**: Açılan listeden **Cihaz İletileri**'ni seçin.

   **Uç nokta**: Az önce ayarladığınız **CriticalQueue** uç noktasını seçin.

   **Sorgu dizesi**: Sorgu dizesi olarak `level="critical"` girin. 

   ![Service Bus kuyruğu için yönlendirme kuralı oluşturmayı gösteren ekran görüntüsü.](./media/tutorial-routing/create-a-new-routing-rule-sbqueue.png)
   
   **Kaydet**’e tıklayın. Yollar bölmesine dönüldüğünde, burada gösterildiği gibi yeni yönlendirme kurallarınızın ikisini de görürsünüz.

   ![Az önce ayarladığınız yolları gösteren ekran görüntüsü.](./media/tutorial-routing/show-routing-rules-for-hub.png)

   Yollar bölmesini kapatın; Kaynak grubu sayfasına dönersiniz.

## <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma  

Service Bus kuyruğu kritik olarak belirlenmiş iletileri almak için kullanılacaktır. Service Bus kuyruğunu izlemek ve kuyruğa ileti eklendiğinde bir e-posta göndermek için bir Mantıksal uygulama ayarlayın. 

1. [Azure portalında](https://portal.azure.com) **+ Kaynak oluştur**'a tıklayın. Arama kutusuna **mantıksal uygulama** yazın ve Enter tuşuna basın. Görüntülenen arama sonuçlarında Mantıksal Uygulama'yı seçin ve **Mantıksal uygulama oluştur** bölmesinden devam etmek için **Oluştur**'a tıklayın. Alanları doldurun. 

   **Ad**: bu alan, mantıksal uygulamanın adıdır. Bu öğreticide **ContosoLogicApp** kullanılır. 

   **Abonelik**: Azure aboneliğinizi seçin.

   **Kaynak grubu**: **Var olanı kullan**'a tıklayın ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır. 

   **Konum**: Konumunuzu kullanın. Bu öğreticide **West US** kullanılır. 

   **Log Analytics**: Bu iki durumlu düğme kapalı konuma getirilmelidir. 

   ![Mantıksal Uygulama Oluştur ekranını gösteren ekran görüntüsü.](./media/tutorial-routing/create-logic-app.png)

   **Oluştur**’a tıklayın.

4. Şimdi Mantıksal Uygulama'ya gidin. Mantıksal Uygulama'ya ulaşmanın en kolay yolu **Kaynak grupları**'na tıklamak, kaynak grubunuzu seçmek (bu öğreticide **ContosoResources** kullanılır), sonra da kaynak listesinden Mantıksal Uygulama'yı seçmektir. Logic Apps Tasarımcısı sayfası gösterilir (sayfanın tamamını görmek için ekranı sağa doğru kaydırmanız gerekebilir). Logic Apps Tasarımcısı sayfasında, **Boş Mantıksal Uygulama +** adlı kutucuğu görene kadar ekranı aşağı kaydırın ve bu kutucuğa tıklayın. 

5. Bağlayıcı listesi görüntülenir. **Service Bus**'ı seçin. 

   ![Bağlayıcı listesini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-connectors.png)

6. Tetikleyici listesi görüntülenir. **Service Bus - Kuyrukta bir ileti alındığında (otomatik tamamla)** öğesini seçin. 

   ![Service Bus için tetikleyici listesini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-triggers.png)

6. Sonraki ekranda, Bağlantı Adı alanını doldurun. Bu öğreticide **ContosoConnection** kullanılır. 

   ![Service Bus kuyruğu için bağlantının ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-define-connection.png)

   Service Bus ad alanına tıklayın. Bu öğreticide **ContosoSBNamespace** kullanılır. Ad alanını seçtiğinizde, portal anahtarları almak için Service Bus ad alanını sorgular. **RootManageSharedAccessKey** öğesini seçin ve **Oluştur**'a tıklayın. 
   
   ![Bağlantı ayarlama işleminin bitirilmesini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-finish-connection.png)

7. Sonraki ekranda, açılan listeden kuyruğun adını seçin (bu öğreticide **contososbqueue** kullanılır). Kalan alanlar için varsayılan değerleri kullanabilirsiniz. 

   ![Kuyruk seçeneklerini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-queue-options.png)

7. Şimdi kuyrukta bir ileti alındığında e-posta gönderme eylemini ayarlayın. Logic Apps Tasarımcısı'nda **+ Yeni adım**'a tıklayarak bir adım ekleyin ve ardından **Eylem ekle**'ye tıklayın. **Eylem seçin** bölmesinde **Office 365 Outlook**'u bulun ve tıklayın. Tetikleyiciler ekranında **Office 365 Outlook - E-posta gönder**'i seçin.  

   ![Office 365 seçeneklerini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-select-outlook.png)

8. Ardından, bağlantıyı ayarlamak için Office 365 hesabınızda oturum açın. E-postaların alıcıları için e-posta adreslerini belirtin. Ayrıca konuyu da belirtin ve alıcının gövdede görmesini istediğiniz iletiyi yazın. Test amacıyla, alıcı olarak kendi e-posta adresinizi girin.

   Eklediğiniz iletinin içeriğini göstermek için **Dinamik içerik ekle**'ye tıklayın. **İçerik** öğesini seçin; e-postadaki iletiyi içerecektir. 

   ![Mantıksal uygulama için e-posta seçeneklerini gösteren ekran görüntüsü.](./media/tutorial-routing/logic-app-send-email.png)

9. **Kaydet**’e tıklayın. Sonra Logic App Tasarımcısı'nı kapatın.

## <a name="set-up-azure-stream-analytics"></a>Azure Stream Analytics'i ayarlama

Verileri PowerBI görselleştirmesinde görmek için, önce verileri almak için bir Stream Analytics işi ayarlayın. Yalnızca **level** değeri **normal** olan iletilerin varsayılan uç noktaya gönderildiğini ve PowerBI görselleştirmesi için Stream Analytics işi tarafından alınacağını unutmayın.

### <a name="create-the-stream-analytics-job"></a>Stream Analytics işi oluşturma

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işi**'ne tıklayın.

2. İş için aşağıdaki bilgileri girin.

   **İş adı**: İşin adı. Adın genel olarak benzersiz olması gerekir. Bu öğreticide **contosoJob** kullanılır.

   **Kaynak grubu**: IoT hub'ınız tarafından kullanılan kaynak grubunun aynısını kullanın. Bu öğreticide **ContosoResources** kullanılır. 

   **Konum**: Kurulum betiğinde kullanılan konumun aynısını kullanın. Bu öğreticide **West US** kullanılır. 

   ![Stream Analytics işi oluşturma işleminin gösterildiği ekran görüntüsü.](./media/tutorial-routing/stream-analytics-create-job.png)

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. **İş Topolojisi**'nin altında **Girişler**'e tıklayın.

2. **Girişler** bölmesinde **Akış girişi ekle**'ye tıklayın ve IoT Hub'ını seçin. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Giriş diğer adı**: Bu öğreticide **contosoinputs** kullanılır.

   **Abonelik**: Aboneliğinizi seçin.

   **IOT Hub'ı**: IOT Hub'ını seçin. Bu öğreticide **ContosoTestHub** kullanılır.

   **Uç nokta**: **Mesajlaşma**'yı seçin. (İşlem İzleme'yi seçerseniz, gönderdiğiniz veriler yerine IoT hub'ı hakkındaki telemetri verilerini alırsınız.) 

   **Paylaşılan erişim ilkesi adı**: **iothubowner** öğesini seçin. Paylaşılan Erişim İlkesi Anahtarı'nı portal sizin için doldurur.

   **Tüketici grubu**: Daha önce oluşturduğunuz tüketici grubunu seçin. Bu öğreticide **contosoconsumers** kullanılır.
   
   Kalan alanlar için varsayılan değerleri kabul edin. 

   ![Stream Analytics işi için girişlerin ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/stream-analytics-job-inputs.png)

5. **Kaydet**’e tıklayın.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**'nin altında **Çıkışlar**'a tıklayın.

2. **Çıkışlar** bölmesinde **Ekle**'ye tıklayın ve **PowerBI** öğesini seçin. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Çıkış diğer adı**: Çıkışın benzersiz diğer adı. Bu öğreticide **contosooutputs** kullanılır. 

   **Veri kümesi adı**: PowerBI'da kullanılacak olan veri kümesinin adı. Bu öğreticide **contosodataset** kullanılır. 

   **Tablo adı**: PowerBI'da kullanılacak olan tablonun adı. Bu öğreticide **contosotable** kullanılır.

   Kalan alanlarda varsayılan değerleri kabul edin.

3. **Yetkilendir**'e tıklayın ve PowerBI hesabınızda oturum açın.

   ![Stream Analytics işi için çıkışların ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/stream-analytics-job-outputs.png)

4. **Kaydet**’e tıklayın.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'ya tıklayın.

2. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin. Bu öğreticide **contosoinputs** kullanılır.

3. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin. Bu öğreticide **contosooutputs** kullanılır.

   ![Stream Analytics işi için sorgunun ayarlanmasını gösteren ekran görüntüsü.](./media/tutorial-routing/stream-analytics-job-query.png)

4. **Kaydet**’e tıklayın.

5. Sorgu bölmesini kapatın.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde **Başlat** > **Şimdi** > **Başlat**'a tıklayın. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

PowerBI raporunu ayarlamak için verilere ihtiyacınız vardır. Bu nedenle, cihazı oluşturduktan ve cihaz simülasyon uygulamasını çalıştırdıktan sonra PowerBI'ı ayarlayacaksınız.

## <a name="run-simulated-device-app"></a>Simülasyon Cihazı uygulamasını çalıştırma

Betik ayarlama bölümünün başlarında, IoT cihazı kullanarak simülasyonu yapılacak bir cihaz ayarlamıştınız. Bu bölümde, IoT Hub'a cihazdan buluta iletiler gönderen bir cihazın simülasyonunu yapan bir .NET konsol uygulaması indireceksiniz. Bu uygulama, farklı yönlendirme yöntemlerinin her biri için iletiler gönderir. 

[IoT Cihaz Simülasyonu](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) çözümünü indirin. Bu işlem, içinde birkaç uygulama bulunan bir depo indirir; aradığınız çözüm Tutorials/Routing/SimulatedDevice/ yolunda yer alır.

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

Şimdi uygulama hala çalışır durumdayken, varsayılan yönlendirme üzerinden gelen iletileri görmek için PowerBI görselleştirmesini ayarlayın. 

## <a name="set-up-the-powerbi-visualizations"></a>PowerBI Görselleştirmelerini ayarlama

1. [PowerBI](https://powerbi.microsoft.com/) hesabınızda oturum açın.

2. **Çalışma Alanları**'na gidin ve Stream Analytics işi için çıkış oluştururken ayarladığını çalışma alanını seçin. Bu öğreticide **My Workspace** kullanılır. 

3. **Veri kümeleri**'ne tıklayın.

   Stream Analytics işi için çıkış oluştururken belirttiğiniz veri kümesinin listelendiğini görüyor olmalısınız. Bu öğreticide **contosodataset** kullanılır. (Veri kümesinin ilk kez görüntülenmesi 5-10 dakika kadar sürebilir.)

4. **EYLEMLER**'in altında, ilk simgeye tıklayarak bir rapor oluşturun.

   ![Eylemler ve rapor simgesinin vurgulandığı PowerBI çalışma alanını gösteren ekran görüntüsü.](./media/tutorial-routing/power-bi-actions.png)

5. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.

   a. Rapor oluşturma sayfasında, çizgi grafik simgesine tıklayarak çizgi grafiği oluşturun.

   ![Görselleştirmeleri ve alanları gösteren ekran görüntüsü.](./media/tutorial-routing/power-bi-visualizations-and-fields.png)

   b. **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin. Bu öğreticide **contosotable** kullanılır.

   c. **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.

   d. **temperature** öğesini **Değerler** üzerine sürükleyin.

   Bir çizgi grafik oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

7. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. İkinci grafiği ayarlamak için yukarıdaki adımları aynı şekilde izleyin ve x eksenine **EventEnqueuedUtcTime**, y eksenine de **humidity** öğelerini yerleştirin.

   ![İki grafiğin bulunduğu son PowerBI raporunu gösteren ekran görüntüsü.](./media/tutorial-routing/power-bi-report.png)

8. Raporu kaydetmek için **Kaydet**’e tıklayın.

Her iki grafikteki verileri de görüyor olmalısınız. Bunun anlamı:

   * Varsayılan uç noktaya yapılan yönlendirme düzgün çalışıyor.
   * Azure Stream Analytics işi doğru akış yapıyor.
   * PowerBI Görselleştirmesi doğru ayarlanmış.

PowerBI penceresinin üst kısmındaki Yenile düğmesine tıklayarak grafikleri yenileyip en son verileri görebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Oluşturduğunuz tüm kaynakları kaldırmak istiyorsanız, kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda IoT hub'ını, Service Bus ad alanıyla kuyruğunu, Mantıksal Uygulamayı, depolama hesabını ve kaynak grubunun kendisini kaldırır. 

### <a name="clean-up-resources-in-the-powerbi-visualization"></a>PowerBI görselleştirmesindeki kaynakları temizleme

[PowerBI](https://powerbi.microsoft.com/) hesabınızda oturum açın. Çalışma alanınıza gidin. Bu öğreticide **My Workspace** kullanılır. PowerBI görselleştirmesini kaldırmak için DataSets'e gidin ve çöp kutusu simgesine tıklayarak veri kümesini silin. Bu öğreticide **contosodataset** kullanılır. Veri kümesini kaldırdığınızda, rapor da kaldırılır.

### <a name="clean-up-resources-using-azure-cli"></a>Azure CLI kullanarak kaynakları temizleme

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın.

```azurecli-interactive
az group delete --name $resourceGroup
```
### <a name="clean-up-resources-using-powershell"></a>PowerShell kullanarak kaynakları temizleme

Kaynak grubunu kaldırmak için [Remove-AzureRmResourceGroup](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanın. $resourceGroup, yeniden bu öğreticinin başındaki **ContosoIoTRG1** değerine ayarlanır.

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
> * Varsayılan uç noktaya gönderilen veriler için PowerBI görselleştirmesi oluşturma.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleme.

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
[Azure IoT Hub'ı cihaz ikizlerini kullanmaya başlama](iot-hub-node-node-twin-getstarted.md)

 <!--  [Manage the state of a device](./tutorial-manage-state.md) -->