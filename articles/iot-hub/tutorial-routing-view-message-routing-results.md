---
title: Görünüm Azure IOT Hub ileti yönlendirme sonuçları (.NET) | Microsoft Docs
description: Azure IOT Hub ileti yönlendirme sonuçlarını görüntüleme
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 1417ecdaf6a85f491e1accfb9564e27d15e13445
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045842"
---
# <a name="tutorial-part-2---view-the-routed-messages"></a>Öğretici: 2. Kısım - yönlendirilmiş iletileri görüntüleyin

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="rules-for-routing-the-messages"></a>İleti yönlendirme kuralları

Bu ileti yönlendirme için kurallar şunlardır; Bunlar bu öğreticinin 1 ayarlanmış ve bunları bu ikinci bölümünde görürsünüz.

|değer |Sonuç|
|------|------|
|level="storage" |Azure Depolama'ya yazın.|
|level="critical" |Service Bus kuyruğuna yazın. Mantıksal Uygulama iletiyi kuyruktan alır ve Office 365 kullanarak iletiyi e-postayla gönderir.|
|default |Power BI'ı kullanarak bu verileri görüntüleyin.|

Şimdi iletileri hub'ına ileti göndermek için bir uygulamayı çalıştırmayı yönlendirilir ve yönlendirme eylemini bkz kaynakları oluşturun.

## <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma  

Service Bus kuyruğu kritik olarak belirlenmiş iletileri almak için kullanılacaktır. Service Bus kuyruğunu izlemek ve kuyruğa ileti eklendiğinde bir e-posta göndermek için bir Mantıksal uygulama ayarlayın.

1. İçinde [Azure portalında](https://portal.azure.com)seçin **+ kaynak Oluştur**. Arama kutusuna **mantıksal uygulama** yazın ve Enter tuşuna basın. Görüntülenen Arama sonuçlarından mantıksal uygulama seçin ve ardından **Oluştur** devam etmek için **mantıksal uygulama oluştur** bölmesi. Alanları doldurun.

   **Ad**: Bu alan, mantıksal uygulama adıdır. Bu öğreticide **ContosoLogicApp** kullanılır.

   **Abonelik**: Azure aboneliğinizi seçin.

   **Kaynak grubu**: Seçin **var olanı kullan** ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

   **Konum**: Konumunuz olarak kullanın. Bu öğreticide **West US** kullanılır.

   **Log Analytics**: Bu iki durumlu kapatılmalıdır.

   ![Mantıksal uygulama oluşturma ekranı](./media/tutorial-routing-view-message-routing-results/create-logic-app.png)

   **Oluştur**’u seçin.

2. Şimdi Mantıksal Uygulama'ya gidin. Mantıksal uygulamaya almak için en kolay yolu seçmektir **kaynak grupları**, kaynak grubunuzu seçin (Bu öğreticide **ContosoResources**), ardından kaynak listesinden mantıksal uygulama seçin. Logic Apps Tasarımcısı sayfası gösterilir (sayfanın tamamını görmek için ekranı sağa doğru kaydırmanız gerekebilir). İfadesini içeren bir kutucuk görene kadar aşağı kaydırın Logic Apps Tasarımcısı'nda sayfasında **boş mantıksal uygulama +** ve bu seçeneği belirleyin. Varsayılan sekme "İçin" kullanılır. Bu bölme boşsa seçin **tüm** tüm bağlayıcılar ve tetikleyiciler yok görmek için.

3. Seçin **Service Bus** bağlayıcılar listesinden.

   ![Bağlayıcıların listesi](./media/tutorial-routing-view-message-routing-results/logic-app-connectors.png)

4. Tetikleyici listesi görüntülenir. Seçin **ne zaman bir ileti alındığında (Otomatik Tamamlama) kuyrukta / Service Bus**.

   ![Service Bus tetikleyicileri listesi](./media/tutorial-routing-view-message-routing-results/logic-app-triggers.png)

5. Sonraki ekranda, Bağlantı Adı alanını doldurun. Bu öğreticide **ContosoConnection** kullanılır.

   ![Service Bus kuyruğu için bağlantı kurma](./media/tutorial-routing-view-message-routing-results/logic-app-define-connection.png)

   Service Bus ad alanını seçin. Bu öğreticide **ContosoSBNamespace** kullanılır. Ad alanını seçtiğinizde, portal anahtarları almak için Service Bus ad alanını sorgular. Seçin **RootManageSharedAccessKey** seçip **Oluştur**.

   ![Bağlantı kurma bitiriliyor](./media/tutorial-routing-view-message-routing-results/logic-app-finish-connection.png)

6. Sonraki ekranda, açılan listeden kuyruğun adını seçin (bu öğreticide **contososbqueue** kullanılır). Kalan alanlar için varsayılan değerleri kullanabilirsiniz.

   ![Kuyruk seçenekleri](./media/tutorial-routing-view-message-routing-results/logic-app-queue-options.png)

7. Şimdi kuyrukta bir ileti alındığında e-posta gönderme eylemini ayarlayın. Logic Apps Tasarımcısı'nda seçin **+ yeni adım** bir adım eklemek, ardından **tüm** tüm kullanılabilir seçenekleri görmek için. İçinde **eylem seçin** bölmesinde bulun ve seçin **Office 365 Outlook**. Tetikleyiciler ekranında seçin **e-posta Gönder / Office 365 Outlook**.  

   ![Office365 seçenekleri](./media/tutorial-routing-view-message-routing-results/logic-app-select-outlook.png)

8. Bağlantı kurmak için Office 365 hesabınızda oturum açın. Yalnızca bu zaman aşımına uğrarsa, yeniden deneyin. E-postaların alıcıları için e-posta adreslerini belirtin. Ayrıca konuyu da belirtin ve alıcının gövdede görmesini istediğiniz iletiyi yazın. Test amacıyla, alıcı olarak kendi e-posta adresinizi girin.

   Seçin **dinamik içerik Ekle** ekleyebileceğiniz iletinin içeriğini göstermek için. **İçerik** öğesini seçin; e-postadaki iletiyi içerecektir.

   ![Mantıksal uygulama için e-posta Seçenekleri](./media/tutorial-routing-view-message-routing-results/logic-app-send-email.png)

9. **Kaydet**’i seçin. Sonra Logic App Tasarımcısı'nı kapatın.

## <a name="set-up-azure-stream-analytics"></a>Azure Stream Analytics'i ayarlama

Verileri Power BI görselleştirmesinde görmek için, önce bir Stream Analytics işi ayarlayarak verileri alın. Yalnızca **düzeyi** **normal** olan iletilerin varsayılan uç noktaya gönderildiğini ve Power BI görselleştirmesi için Stream Analytics işi tarafından alınacağını unutmayın.

### <a name="create-the-stream-analytics-job"></a>Stream Analytics işi oluşturma

1. İçinde [Azure portalında](https://portal.azure.com)seçin **kaynak Oluştur** > **nesnelerin interneti** > **Stream Analytics işi**.

2. İş için aşağıdaki bilgileri girin.

   **İş adı**: İş adı. Adın genel olarak benzersiz olması gerekir. Bu öğreticide **contosoJob** kullanılır.

   **Abonelik**: Öğretici için kullandığınız Azure aboneliği.

   **Kaynak grubu**: IOT hub tarafından kullanılan aynı kaynak grubunu kullanın. Bu öğreticide **ContosoResources** kullanılır.

   **Konum**: Kurulum komut dosyasında kullandığınız konumun aynısını kullanın. Bu öğreticide **West US** kullanılır.

   ![Stream analytics işi oluşturma](./media/tutorial-routing-view-message-routing-results/stream-analytics-create-job.png)

3. Seçin **Oluştur** işi oluşturmak için. İşe dönmek için seçin **kaynak grupları**. Bu öğreticide **ContosoResources** kullanılır. Kaynak grubunu seçin, sonra Stream Analytics işi kaynak listesinden seçin.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

4. Altında **iş topolojisi**seçin **girişleri**.

5. İçinde **girişleri** bölmesinde **akış Girişi Ekle** ve IOT hub'ı seçin. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Giriş diğer adı**: Bu öğreticide **contosoinputs** kullanılır.

   **IOT hub'ı seçin, abonelikten**: Bu radyo düğmesi seçeneği belirleyin.

   **Abonelik**: Bu öğretici için kullandığınız Azure aboneliğini seçin.

   **IOT hub'ı**: IOT hub'ı seçin. Bu öğreticide **ContosoTestHub** kullanılır.

   **Uç nokta**: Seçin **Mesajlaşma**. (İşlem İzleme'yi seçerseniz, gönderdiğiniz veriler yerine IoT hub'ı hakkındaki telemetri verilerini alırsınız.) 

   **Paylaşılan erişim ilkesi adı**: Seçin **iothubowner**. Paylaşılan Erişim İlkesi Anahtarı'nı portal sizin için doldurur.

   **Tüketici grubu**: Bu öğreticinin 1 adımda ayarladığınız tüketici grubu seçin. Bu öğreticide **contosoconsumers** kullanılır.
   
   Kalan alanlar için varsayılan değerleri kabul edin. 

   ![Stream analytics işine ilişkin girişleri ayarlama](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-inputs.png)

6. **Kaydet**’i seçin.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. Altında **iş topolojisi**seçin **çıkışları**.

2. İçinde **çıkışları** bölmesinde **Ekle**ve ardından **Power BI**. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Çıkış diğer adı**: Çıkış için benzersiz diğer adı. Bu öğreticide **contosooutputs** kullanılır. 

   **Veri kümesi adı**: Power BI'da kullanılacak veri kümesinin adı. Bu öğreticide **contosodataset** kullanılır. 

   **Tablo adı**: Power BI'da kullanılacak tablonun adı. Bu öğreticide **contosotable** kullanılır.

   Kalan alanlarda varsayılan değerleri kabul edin.

3. Seçin **Authorize**ve Power BI hesabınızda oturum açın. (Bu, birden fazla deneme sürebilir).

   ![Akış analizi işi için çıkış ayarlama](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-outputs.png)

4. **Kaydet**’i seçin.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'yu seçin.

2. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin. Bu öğreticide **contosoinputs** kullanılır.

3. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin. Bu öğreticide **contosooutputs** kullanılır.

   ![Stream analytics işi sorgusu ayarlama](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-query.png)

4. **Kaydet**’i seçin.

5. Sorgu bölmesini kapatın. Kaynak grubundaki kaynakların görünümünü dönün. Stream Analytics işi seçin. Bu öğreticide **contosoJob** olarak adlandırılmıştır.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde seçin **Başlat** > **artık** > **Başlat**. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

Power BI raporunu ayarlamak için verilere ihtiyacınız vardır; bu nedenle, cihazı oluşturup cihaz simülasyon uygulamasını çalıştırdıktan sonra Power BI'ı ayarlarsınız.

## <a name="run-simulated-device-app"></a>Sanal cihaz uygulaması çalıştırma

Öğreticinin bu 1, IOT cihazına kullanarak benzetimini yapmak için bir cihazı ayarlama. Bu bölümde, uygulama ve kaynakları bu öğreticinin 1 bölümünde zaten indirilmedi varsayılarak bir IOT hub'ına CİHAZDAN buluta ileti gönderme, bir cihaza benzetim .NET konsol uygulaması indirin.

Bu uygulamayı her biri farklı bir ileti yönlendirme yöntemleri için iletileri gönderir. Azure CLI ve PowerShell betikleri yanı sıra tam Azure Resource Manager şablonu ve parametre dosyasını içeren indirme ayrıca bir klasör var.

Bu öğreticinin 1. adımda deposundan dosyalar indirilmedi, devam edin ve bunları şimdi indirin [IOT cihaz benzetimi](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Bu bağlantı seçildiğinde, çeşitli uygulamalarda deposuyla indirir; Aradığınız iot-hub/Tutorials/Routing/IoT_SimulatedDevice.sln çözümüdür. 

Çözüm dosyası (IoT_SimulatedDevice.sln) kodu Visual Studio'da açmak için çift tıklayın ve ardından program.cs dosyasını açın. `{iot hub hostname}` değerini IoT hub'ı konak adıyla değiştirin. IoT hub'ı konak adı **{iot-hub-adı}.azure-devices.net** biçimindedid. bu öğreticide, hub konak adı olarak **ContosoTestHub.azure-devices.net** kullanılır. Ardından, `{device key}` değerini daha önce simülasyon cihazını ayarlarken kaydettiğiniz cihaz anahtarıyla değiştirin. 

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

   ![Sonuçta elde edilen e-postaları](./media/tutorial-routing-view-message-routing-results/results-in-email.png)

   Bu sonuç, aşağıdaki deyimleri doğru olduğu anlamına gelir. 

   * Service Bus kuyruğunda yönlendirme düzgün çalışıyor.
   * İletiyi Service Bus kuyruğundan alan Mantıksal Uygulama düzgün çalışıyor.
   * Mantıksal Uygulama'nın Outlook bağlayıcısı düzgün çalışıyor. 

2. İçinde [Azure portalında](https://portal.azure.com)seçin **kaynak grupları** ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır. Depolama hesabı seçin, **Blobları**, ardından kapsayıcıyı seçin. Bu öğreticide **contosoresults** kullanılır. Bir klasör görüyor olmalısınız ve bir veya birden çok dosya görünceye kadar dizinlerde ayrıntıya gidebilirsiniz. Bu dosyalardan birini açın; depolama hesabına yönlendirilen girdileri içerir. 

   ![Sonuç dosyaları depolama](./media/tutorial-routing-view-message-routing-results/results-in-storage.png)

Bu sonuç, aşağıdaki ifade true anlamına gelir.

   * Depolama hesabına yapılan yönlendirme düzgün çalışıyor.

Şimdi uygulamanız hala çalışırken, varsayılan yönlendirme aracılığıyla gelen iletileri görmek için Power BI görselleştirmesini ayarlayın.

## <a name="set-up-the-power-bi-visualizations"></a>Power BI görselleştirmelerini ayarlama

1. [Power BI](https://powerbi.microsoft.com/) hesabınızda oturum açın.

2. **Çalışma Alanları**'na gidin ve Stream Analytics işi için çıkış oluştururken ayarladığını çalışma alanını seçin. Bu öğreticide **My Workspace** kullanılır. 

3. Seçin **veri kümeleri**. Hiç veri kümeniz yoksa, birkaç dakika bekleyin ve tekrar kontrol edin.

   Stream Analytics işi için çıkış oluştururken belirttiğiniz veri kümesinin listelendiğini görüyor olmalısınız. Bu öğreticide **contosodataset** kullanılır. (Veri kümesinin ilk kez görüntülenmesi 5-10 dakika kadar sürebilir.)

4. Altında **eylemleri**, bir rapor oluşturmak için ilk simgesini seçin.

   ![Power BI çalışma alanı ile vurgulanmış Eylemler ve rapor simgesi](./media/tutorial-routing-view-message-routing-results/power-bi-actions.png)

5. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.

   * Rapor oluşturma sayfasındaki, çizgi grafik simgesini seçerek bir çizgi grafiğe ekleyin.

     ![Görsel öğeler ve alanlar](./media/tutorial-routing-view-message-routing-results/power-bi-visualizations-and-fields.png)

   * **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin. Bu öğreticide **contosotable** kullanılır.

   * **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.

   * **temperature** öğesini **Değerler** üzerine sürükleyin.

   Bir çizgi grafik oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

6. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. İkinci grafiği ayarlamak için yukarıdaki adımları aynı şekilde izleyin ve x eksenine **EventEnqueuedUtcTime**, y eksenine de **humidity** öğelerini yerleştirin.

   ![İki grafik ile Power BI raporun son hali](./media/tutorial-routing-view-message-routing-results/power-bi-report.png)

7. Seçin **Kaydet** raporu kaydetmek için.

Her iki grafikteki verileri de görüyor olmalısınız. Bu sonuç, aşağıdaki deyimleri true anlamına gelir:

   * Varsayılan uç noktaya yapılan yönlendirme düzgün çalışıyor.
   * Azure Stream Analytics işi doğru akış yapıyor.
   * Power BI Görselleştirmesi doğru ayarlanmış.

Power BI penceresinin üst kısmındaki Yenile düğmesini seçerek en son verileri görmek için grafikler yenileyebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bu öğreticide iki bölümünü oluşturduğunuz tüm kaynakları kaldırmak istiyorsanız, kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda IoT hub'ını, Service Bus ad alanıyla kuyruğunu, Mantıksal Uygulamayı, depolama hesabını ve kaynak grubunun kendisini kaldırır. 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Power BI görselleştirmesinde kaynakları temizleme

[Power BI](https://powerbi.microsoft.com/) hesabınızda oturum açın. Çalışma alanınıza gidin. Bu öğreticide **My Workspace** kullanılır. Power BI görselleştirmesini kaldırmak için veri kümelerine gidin ve çöp kutusu simgesine veri kümesini silmek için kullanabilirsiniz. Bu öğreticide **contosodataset** kullanılır. Veri kümesini kaldırdığınızda, rapor da kaldırılır.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>Kaynakları temizlemek için Azure CLI kullanma

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın. `$resourceGroup` ayarlandı **ContosoResources** Bu öğreticinin geri başında.

```azurecli-interactive
az group delete --name $resourceGroup
```

### <a name="use-powershell-to-clean-up-resources"></a>Kaynakları temizlemek için PowerShell kullanma

Kaynak grubunu kaldırmak için [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) komutu. `$resourceGroup` ayarlandı **ContosoResources** Bu öğreticinin geri başında.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

2-bir parçası olan Bu öğreticide, IOT Hub iletilerini aşağıdaki görevleri gerçekleştirerek farklı hedeflere yönlendirmek için ileti yönlendirme kullanan öğrendiniz.  

**Bölümü I: İleti yönlendirmeyi'kurmak, kaynakları oluşturma**
> [!div class="checklist"]
> * Kaynakları--bir IOT hub'ı, bir depolama hesabı, Service Bus kuyruğuna ve bir simülasyon cihazı oluşturun.
> * Uç noktaları ve ileti yollarını IOT Hub'ında depolama hesabı ve Service Bus kuyruğu için yapılandırın.

**Bölüm II: Hub'ına ileti göndermek, yönlendirilmiş sonuçlarını görüntüleme**
> [!div class="checklist"]
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için bir Power BI görselleştirmesi oluşturun.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleyin.

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
> [Ayarlama ve ölçümleri ve Tanılama ile IOT hub'ı kullanma](tutorial-use-metrics-and-diags.md)
