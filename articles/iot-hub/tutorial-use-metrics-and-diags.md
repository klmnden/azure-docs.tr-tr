---
title: Ayarlama ve ölçümleri ve tanılama günlükleri ile bir Azure IOT hub'ı kullanma | Microsoft Docs
description: Ayarlama ve ölçümleri ve tanılama günlükleri ile bir Azure IOT hub'ı kullanma
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 3/13/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 40e54daa60efedd84b32c72f29d1e2a8858c27da
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66162543"
---
# <a name="tutorial-set-up-and-use-metrics-and-diagnostic-logs-with-an-iot-hub"></a>Öğretici: Ayarlama ve ölçümleri ve tanılama günlükleri ile IOT hub'ı kullanma

Üretim ortamında çalışan bir IOT hub'ı çözümü varsa, bazı Ölçümler ve tanılama günlüklerini etkinleştirme istersiniz. Bir sorun oluşursa, sorunu tanılamak ve daha hızlı bir şekilde giderin yardımcı olacak bakmak için verileri sahip. Bu makalede, tanılama günlüklerini etkinleştirme ve bunları hataları denetlemek nasıl görürsünüz. Bazı ölçümler izleyin ve ölçümleri belirli bir sınır ulaştığınızda tetiklenen uyarılar ayarlarsınız. Örneğin, size gönderilen telemetri iletilerini sayısı belirli bir sınırı aşan yaklaştığında veya günde IOT Hub için izin verilen ileti kotası yakın kullanılan ileti sayısını alır. gönderilen e-posta olabilir. 

Bir örnek kullanmak benzin istasyonunu pompalara Gönder IOT cihazları bir IOT hub ile iletişim kurmak olduğu bir durumdur. Kredi kartı doğrulanır ve son işlem, bir veri deposuna yazılır. IOT cihazları hub'a bağlanan durdurur ve ileti gönderme, çok daha fazla olduğu ne hiçbir görünürlük varsa, gidermek daha zor olan bitenlerin.

Bu öğreticide Azure örnekten [IOT hub'ı yönlendirme](tutorial-routing.md) IOT hub'ına ileti göndermek için.

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Azure CLI'yı kullanarak bir IOT hub'ı, bir sanal cihaz ve bir depolama hesabı oluşturun.  
> * Tanılama günlüklerini etkinleştirin.
> * Ölçümleri sağlar.
> * Bu ölçümler için uyarılar ayarlayın. 
> * İndirin ve hub'ına gönderme IOT cihazına taklit eden bir uygulamayı çalıştırın. 
> * Uyarıların tetikleneceği başlayana kadar uygulamayı çalıştırın. 
> * Ölçümleri sonuçları görüntüleyebilir ve tanılama günlüklerini kontrol edin. 

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- [Visual Studio](https://www.visualstudio.com/)’yu yükleyin. 

- Bir e-posta hesabı posta alabildiğini.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="set-up-resources"></a>Kaynakları ayarlama

Bu öğreticide, IOT hub'ı, bir depolama hesabı ve sanal bir IOT cihazı gerekir. Bu kaynaklar Azure CLI veya Azure PowerShell kullanılarak oluşturulabilir. Tüm kaynaklar için aynı kaynak grubunu ve konumunu kullanın. Sonunda kaynak grubunu silerek her şeyi tek adımda kaldırabilirsiniz.

Gerekli adımlar şunlardır.

1. Bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. 

2. IOT hub oluşturun.

3. Standard_LRS çoğaltmasıyla standart bir V1 depolama hesabı oluşturun.

4. Hub'ınıza iletiler gönderen simülasyon cihazı için cihaz kimliği oluşturun. Test aşaması için anahtarı kaydedin.

### <a name="set-up-resources-using-azure-cli"></a>Azure CLI kullanarak kaynaklarını ayarlama

Bu betiği kopyalayıp Cloud Shell'e yapıştırın. Zaten oturum açmış olduğunuzu varsayarak, betiği bir kerede bir satır olmak üzere çalıştırır. Yeni kaynaklar ContosoResources kaynak grubunda oluşturulur.

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
iotDeviceName=Contoso-Test-Device 

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$RANDOM
echo "IoT hub name = " $iotHubName

# Create the IoT hub in the Free tier.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku F1 --location $location

# The storage account name must be globally unique, so add a random number to the end.
storageAccountName=contosostoragemon$RANDOM
echo "Storage account name = " $storageAccountName

# Create the storage account.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName 

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

```

>[!NOTE]
>Cihaz kimliği oluşturma, şu hatayla karşılaşabilirsiniz: *Anahtar için IOT hub'ı ContosoTestHub, ilke iothubowner bulunan*. Bu hatayı düzeltmek için Azure CLI IOT uzantısı güncelleştirin ve son iki komutlar komut dosyasını yeniden çalıştırın. 
>
>Uzantıyı güncelleştirmek için komutu aşağıda verilmiştir. Bu, Cloud Shell'i Örneğinizde çalıştırın.
>
>```cli
>az extension update --name azure-cli-iot-ext
>```

## <a name="enable-the-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme 

[Tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md) yeni bir IOT hub'ı oluşturduğunuzda, varsayılan olarak devre dışıdır. Bu bölümde, hub'ınız için tanılama günlüklerini etkinleştirin.

1. İlk olarak, siz değil zaten şirket Portalı'nda hub'ınıza gibiyseniz, tıklayın **kaynak grupları** Contoso kaynaklar kaynak grubuna tıklayın. Hub görüntülenen kaynak listesinden seçin. 

2. Aranacak **izleme** IOT Hub dikey bölümde. **Tanılama ayarları**'na tıklayın. 

   ![IOT Hub dikey tanılama ayarları bölümünü gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/01-diagnostic-settings.png)


3. Abonelik ve kaynak grubunun doğru olduğundan emin olun. Altında **kaynak türü**, onay kutusunu temizleyin **Tümünü Seç**, ardından aramak ve denetleme **IOT hub'ı**. (Yanında onay işareti koyar *Tümünü Seç* yeniden yalnızca onu yok sayabilirsiniz.) Altında **kaynak**, hub adını seçin. Ekranın bu resimdeki gibi görünmelidir: 

   ![IOT Hub dikey tanılama ayarları bölümünü gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/02-diagnostic-settings-start.png)

4. Şimdi tıklayarak **tanılamayı Aç**. Tanılama ayarları bölmesi görüntülenir. Tanılama günlükleri ayarlarınızı adını "tanıyı-hub" belirtin.

5. Denetleme **bir depolama hesabında arşivle**. 

   ![Bir depolama hesabında arşivlemek için tanılama ayarını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/03-diagnostic-settings-storage.png)

    Tıklayın **yapılandırma** görmek için **bir depolama hesabı seçin** ekranında, doğru olanı seçin (*contosostoragemon*), tıklatıp **Tamam** için Tanılama ayarları bölmesine döndürür. 

   ![Bir depolama hesabında arşivlemek için tanılama günlüklerini ayarını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/04-diagnostic-settings-after-storage.png)

6. Altında **günlük**, kontrol **bağlantıları** ve **cihaz Telemetrisi**ve **bekletme (gün)** her 7 gün için. Tanılama ayarları ekranınız şimdi resimdeki gibi görünmelidir:

   ![Son tanılama günlüğü ayarları gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/05-diagnostic-settings-done.png)

7. Ayarları kaydetmek için **Kaydet**’e tıklayın. Tanılama ayarları bölmesini kapatın.

Daha sonra tanılama günlüklerine bakın, Bağlan bakın ve cihaz için günlüğü bağlantısını kesmek mümkün olacaktır. 

## <a name="set-up-metrics"></a>Ölçümleri ' ayarlayın 

Şimdi bazı ölçümler, hub'a gönderilen iletileri izlemek üzere ayarlayın. 

1. IOT hub'ının ayarlar bölmesinde, tıklayarak **ölçümleri** seçeneğini **izleme** bölümü.

2. Ekranın üst kısmında tıklayın **son 24 saat (otomatik)**. Görünen açılır menüden seçin **son 4 saat** için **zaman aralığı**, ayarlayıp **zaman ayrıntı düzeyi** için **1 dakika**, yerel saat. Tıklayın **Uygula** bu ayarları kaydedin. 

   ![Saat ayarlarını ölçümleri gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/06-metrics-set-time-range.png)

3. Varsayılan olarak tek bir ölçüm giriş yok. Kaynak grubunu varsayılan ve ölçüm ad alanı olarak bırakın. İçinde **ölçüm** açılan listesinden **gönderilen Telemetri iletilerini**. Ayarlama **toplama** için **toplam**.

   ![Bir ölçüm için gönderilen telemetri iletilerini ekleme gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/07-metrics-telemetry-messages-sent.png)


4. Şimdi tıklayarak **ölçüm Ekle** başka bir ölçüm grafiği ekleme. Kaynak grubunuzu seçin (**ContosoTestHub**). Altında **ölçüm**seçin **kullanılan iletilerin toplam sayısını**. İçin **toplama**seçin **ortalama**. 

   Artık ekranınızı simge durumuna küçültülmüş ölçüsünü gösterir *gönderilen Telemetri iletilerini*, yeni ölçüm için ayrıca *kullanılan iletilerin toplam sayısını*.

   ![Bir ölçüm için gönderilen telemetri iletilerini ekleme gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/07-metrics-num-messages-used.png)

   Tıklayın **panoya Sabitle**. Yeniden erişebilmesi için bu, Azure portal'ın panoya sabitleyin. Panoya Sabitle yoksa, ayarlarınızı korunmaz.

## <a name="set-up-alerts"></a>Uyarılar ayarlayın

Portalda hub'ına gidin. Tıklayın **kaynak grupları**seçin *ContosoResources*, IOT hub'ı seçip *ContosoTestHub*. 

IOT hub'ı değil geçirildiğini [Azure İzleyicisi'nde ölçümler](/azure/azure-monitor/platform/data-collection#metrics) kullanmak zorunda henüz; [Klasik uyarılar](/azure/azure-monitor/platform/alerts-classic.overview).

1. Altında **izleme**, tıklayın **uyarılar** bu ana uyarı ekranı gösterilir. 

   ![Klasik uyarılar gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/08-find-classic-alerts.png)

2. Buradan Klasik uyarıları almak için tıklayın **Klasik uyarıları görüntüleyip**. 

    ![Klasik uyarılar ekranını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/09-view-classic-alerts.png)

    Şu alanları doldurun: 

    **Abonelik**: Bu alan geçerli aboneliğinizi bırakın.

    **Kaynak**: Bu alan kümesine *ölçümleri*.

    **Kaynak grubu**: Bu alan, geçerli kaynak grubunuza kümesi *ContosoResources*. 

    **Kaynak türü**: Bu alan, IOT Hub'ına ayarlayın. 

    **Kaynak**: IOT hub'ınızı seçin *ContosoTestHub*.

3. Tıklayın **ölçüm uyarısı Ekle (Klasik)** yeni bir uyarı ayarlama.

    Şu alanları doldurun:

    **Ad**: Uyarı, kural için bir ad sağlayın *telemetri iletilerini*.

    **Açıklama**: Uyarınız bir açıklamasını sağlayın *gönderilen 1000 telemetri iletilerini olduğunda uyarı*. 

    **Kaynak**: Bu ayar *ölçümleri*.

    **Abonelik**, **kaynak grubu**, ve **kaynak** , seçtiğiniz değerlere ayarlanması gerekir **Klasik uyarıları görüntüleyip** ekran. 

    Ayarlama **ölçüm** için *gönderilen Telemetri iletilerini*.

    ![Gönderilen telemetri iletilerini klasik bir uyarı ayarlama gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/10-alerts-add-rule-telemetry-top.png)

4. Grafik sonra aşağıdaki alanları ayarlayın:

   **Koşul**: Kümesine *büyüktür*.

   **Eşik**: 1000 olarak ayarlarsınız.

   **Dönem**: Kümesine *son 5 dakika üzerinden*.

   **Bildirim e-posta alıcılarını**: E-posta adresinizi buraya koyun. 

   ![Uyarılar ekranın alt gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/11-alerts-add-rule-bottom.png)

   Tıklayın **Tamam** uyarısını kaydedin. 

5. Başka bir uyarı için artık ayarlama *kullanılan iletilerin toplam sayısını*. Bu ölçüm, kullanılan ileti sayısı kotayı IOT hub'ının--yaklaşırken bir uyarı göndermek istiyorsanız hub bildirmek için yakında iletileri reddetme başlayacak yararlıdır.

   Üzerinde **Klasik uyarıları görüntüleyip** ekranında **ölçüm uyarısı Ekle (Klasik)**, bu alanları ardından doldurun **Kuralı Ekle** bölmesi.

   **Ad**: Uyarı, kural için bir ad sağlayın *sayı--iletileri-genişliğinde*.

   **Açıklama**: Uyarınız bir açıklamasını sağlayın *kotanız dolmak alma olduğunda uyarı*.

   **Kaynak**: Bu alan kümesine *ölçümleri*.

    **Abonelik**, **kaynak grubu**, ve **kaynak** , seçtiğiniz değerlere ayarlanması gerekir **Klasik uyarıları görüntüleyip** ekran. 

    Ayarlama **ölçüm** için *kullanılan iletilerin toplam sayısını*.

6. Grafiğin altında aşağıdaki alanları doldurun:

   **Koşul**: Kümesine *büyüktür*.

   **Eşik**: 1000 olarak ayarlarsınız.

   **Dönem**: Bu alan kümesine *son 5 dakika üzerinden*. 

   **Bildirim e-posta alıcılarını**: E-posta adresinizi buraya koyun. 

   Tıklayın **Tamam** kuralını kaydetmek için. 

5. Artık Klasik uyarılar bölmesinde iki uyarı da görmeniz gerekir: 

   ![Ekran gösteren yeni uyarı kuralları ile klasik uyarıları ekranı.](./media/tutorial-use-metrics-and-diags/12-alerts-done.png)

6. Uyarılar bölmesini kapatın. 
    
    İleti sayısı 400 ve kullanılan iletilerin toplam sayısını sayı aştığında büyükse gönderildiğinde, bu ayarlarla bir uyarı alırsınız.

## <a name="run-simulated-device-app"></a>Simülasyon Cihazı uygulamasını çalıştırma

Betik ayarlama bölümünün başlarında, IoT cihazı kullanarak simülasyonu yapılacak bir cihaz ayarlamıştınız. Bu bölümde, IoT Hub'a cihazdan buluta iletiler gönderen bir cihazın simülasyonunu yapan bir .NET konsol uygulaması indireceksiniz.  

[IoT Cihaz Simülasyonu](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) çözümünü indirin. Bu bağlantı, çeşitli uygulamalarda depoyla indirir; Aradığınız IOT-hub/öğreticiler/yönlendirme çözümüdür /.

Kodu Visual Studio'da açmak için çözüm dosyasına (SimulatedDevice.sln) çift tıklayın, sonra da Program.cs'yi açın. `{iot hub hostname}` değerini IoT hub'ı konak adıyla değiştirin. IoT hub'ı konak adı **{iot-hub-adı}.azure-devices.net** biçimindedid. bu öğreticide, hub konak adı olarak **ContosoTestHub.azure-devices.net** kullanılır. Ardından, `{device key}` değerini daha önce simülasyon cihazını ayarlarken kaydettiğiniz cihaz anahtarıyla değiştirin. 

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Çalıştırma ve test etme 

Program.cs içinde değiştirmek `Task.Delay` 10 1000'den 1 saniye.01 saniyeye gönderme arasındaki süre miktarını azaltır. Bu gecikme kısaltmayı gönderilen ileti sayısını artırır.

```csharp
await Task.Delay(10);
```

Konsol uygulamasını çalıştırın. (10-15 arasında) birkaç dakika bekleyin. Sanal CİHAZDAN konsol ekranında uygulamasının hub'a gönderilen iletileri görebilirsiniz.

### <a name="see-the-metrics-in-the-portal"></a>Portalda ölçümlerini bakın

Ölçümlerinizin panoyu açın. Saat değerleri için değiştirme *son 30 dakika* zaman ayrıntı düzeyini ile *1 dakika*. Bu, gönderilen telemetri iletilerini ve grafikteki en son numaralarını grafiğin altına ile kullanılan iletilerin toplam sayısını gösterir.

   ![Ölçümleri gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/13-metrics-populated.png)

### <a name="see-the-alerts"></a>Uyarıları görmek

Uyarılar için geri dönün. Tıklayın **kaynak grupları**seçin *ContosoResources*, hub'ı seçip *ContosoTestHub*. Hub için görüntülenen Özellikler sayfasında seçin **uyarılar**, ardından **Klasik uyarıları görüntüleyip**. 

Gönderilen ileti sayısını sınırını aşarsa, e-posta uyarıları almak başlatın. Tüm etkin uyarılar olup olmadığını görmek için seçin ve hub gidin **uyarılar**. Bu, etkin uyarıları gösterir ve tüm uyarılar olduğunda. 

   ![Uyarıları gösteren ekran görüntüsü başlatıldı.](./media/tutorial-use-metrics-and-diags/14-alerts-firing.png)

Telemetri iletilerini için uyarıya tıklayın. Bu ölçüm sonucu ve sonuçları içeren bir grafik gösterir. Ayrıca, tetikleme uyarı sizi uyarabilmek için gönderilen e-posta, bu resimdeki gibi görünür:

   ![E-posta uyarıları başlatıldı gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/15-alert-email.png)

### <a name="see-the-diagnostic-logs"></a>Tanılama günlüklerine bakın

Tanılama günlüklerinize BLOB depolamaya aktarılması ayarlayın. Depolama hesabınızı seçin ve kaynak grubunuzun Git *contosostoragemon*. BLOB'ları seçin ve ardından açın kapsayıcı *ınsights günlükleri bağlantıları*. Geçerli tarihe alın ve en son dosya seçmek kadar detayına gidin. 

   ![Tanılama günlükleri görmek için detaya depolama kapsayıcısına ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/16-diagnostics-logs-list.png)

Tıklayın **indirme** indirip açın. Bağlanma ve hub'a iletiler gönderir gibi kesme cihaz günlüklerini görürsünüz. Aşağıda bir örnek:

``` json
{ 
  "time": "2018-12-17T18:11:25Z", 
  "resourceId": 
    "/SUBSCRIPTIONS/your-subscription-id/RESOURCEGROUPS/CONTOSORESOURCES/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/CONTOSOTESTHUB", 
  "operationName": "deviceConnect", 
  "category": "Connections", 
  "level": "Information", 
  "properties": 
      {"deviceId":"Contoso-Test-Device",
       "protocol":"Mqtt",
       "authType":null,
       "maskedIpAddress":"73.162.215.XXX",
       "statusCode":null
       }, 
  "location": "westus"
}
{ 
   "time": "2018-12-17T18:19:25Z", 
   "resourceId": 
     "/SUBSCRIPTIONS/your-subscription-id/RESOURCEGROUPS/CONTOSORESOURCES/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/CONTOSOTESTHUB", 
    "operationName": "deviceDisconnect", 
    "category": "Connections", 
    "level": "Error", 
    "resultType": "404104", 
    "resultDescription": "DeviceConnectionClosedRemotely", 
    "properties": 
        {"deviceId":"Contoso-Test-Device",
         "protocol":"Mqtt",
         "authType":null,
         "maskedIpAddress":"73.162.215.XXX",
         "statusCode":"404"
         }, 
    "location": "westus"
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bu öğreticide oluşturduğunuz tüm kaynakları kaldırmak için kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda, IOT hub'ı, depolama hesabı ve kaynak grubu kaldırılır. Ölçümleri panoya sabitlenmiş, her sağ üst köşedeki üç noktaya tıklayarak ve seçerek tarafından el ile kaldırmanız gerekir **Kaldır**.

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri gerçekleştirerek ölçümleri ve tanılama günlüklerini kullanma hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Azure CLI'yı kullanarak bir IOT hub'ı, bir sanal cihaz ve bir depolama hesabı oluşturun.  
> * Tanılama günlüklerini etkinleştirin. 
> * Ölçümleri sağlar.
> * Bu ölçümler için uyarılar ayarlayın. 
> * İndirin ve hub'ına gönderme IOT cihazına taklit eden bir uygulamayı çalıştırın. 
> * Uyarıların tetikleneceği başlayana kadar uygulamayı çalıştırın. 
> * Ölçümleri sonuçları görüntüleyebilir ve tanılama günlüklerini kontrol edin. 

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
> [Cihazlarınızı bir arka uç hizmetinden yapılandırma](tutorial-device-twins.md)