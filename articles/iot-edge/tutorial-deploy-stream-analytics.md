---
title: 'Öğretici: ASA işlerini Azure IoT Edge cihazlarına dağıtma | Microsoft Docs'
description: Bu öğreticide Azure Stream Analytics’i bir IoT Edge cihazına modül olarak dağıtacaksınız.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/25/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: afbdf2171c1fc1eef95514526a509d171e262d4a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435691"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module-preview"></a>Öğretici: Azure Stream Analytics’i bir IoT Edge modülüne dağıtma (önizleme)

Çoğu IoT çözümünde, IoT cihazlarından buluta ulaşan veriler hakkında içgörü edinmek için analiz hizmetleri kullanılır. Azure IoT Edge ile [Azure Stream Analytics][azure-stream] mantığını alıp cihazın kendisine aktarabilirsiniz. Telemetri akışlarını uç cihazlarda işleyerek yüklenen veri miktarını ve eyleme dönüştürülebilir içgörülere tepki verme süresini azaltabilirsiniz.

Azure IoT Edge ve Azure Stream Analytics tümleşik olduğundan Azure portalında bir Azure Stream Analytics işi oluşturup ek kod yazmadan IoT Edge modülü olarak dağıtabilirsiniz.  

Azure Stream Analytics hem bulutta hem de IoT Edge cihazlarında veri analizi için zengin bir yapılandırılmış sorgu söz dizimi sunar. IoT Edge üzerinde Azure Stream Analytics hakkında daha fazla bilgi için bkz. [Azure Stream Analytics belgeleri](../stream-analytics/stream-analytics-edge.md).

Bu öğreticideki Stream Analytics modülü, tekrar eden 30 saniyelik dönemler içindeki ortalama sıcaklığı hesaplar. Bu ortalama değer 70'e ulaştığında modül, eyleme geçmek için cihazla ilgili bir uyarı gönderir. Bu durumda eylem, sıcaklık sensörü simülasyonunu sıfırlamaktır. Bir üretim ortamında bu işlevi kullanarak sıcaklık tehlikeli düzeylere ulaştığında bir makineyi kapatabilir veya önleyici işlemler gerçekleştirebilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Verileri uç cihazlarda işlemek için bir Azure Stream Analytics işi oluşturma.
> * Yeni Azure Stream Analytics işini diğer IoT Edge modüllerine bağlama.
> * Azure Stream Analytics işini Azure portalından bir IoT Edge cihazına dağıtma.

>[!NOTE]
>Azure Stream Analytics IoT Edge modülleri [genel önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) sürümündedir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.
* Azure Machine Learning modülü ARM işlemcilerini desteklemez.

Bulut kaynakları:

* Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 


## <a name="create-an-azure-stream-analytics-job"></a>Azure Stream Analytics işi oluşturma

Bu bölümde IoT hub'ınızdaki verileri almak, cihazınızdan gönderilen telemetri verilerini sorgulamak ve ardından sonuçları bir Azure Blob depolama kapsayıcısına iletmek için bir Azure Stream Analytics işi oluşturacaksınız. Daha fazla bilgi için [Stream Analytics belgelerinin][azure-stream] "Genel Bakış" bölümüne bakın. 

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Azure Stream Analytics işleri için iş çıktısı uç noktası olarak kullanılacak bir Azure Depolama hesabı gereklidir. Bu bölümdeki örnekte Blob depolama türü kullanılmıştır. Daha fazla bilgi için [Azure Storage belgelerinin][azure-storage] "Bloblar" bölümüne bakın.

1. Azure portalında **Kaynak oluştur** bölümüne gidin, arama kutusuna **Depolama hesabı** yazın ve ardından **Depolama hesabı - blob, dosya, tablo, kuyruk** girişini seçin.

1. **Depolama hesabı oluştur** bölmesinde depolama hesabınız için bir ad girin, IoT hub'ınızın depolandığı konumu seçin, IoT hub'ınızın bulunduğu kaynak grubunu seçin ve **Oluştur**'u seçin. Adı daha sonra kullanmak için not edin.

    ![Depolama hesabı oluşturma][1]


### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure portalında **Kaynak oluştur** > **Nesnelerin İnterneti**'ne gidin ve **Stream Analytics İşi**'ni seçin.

1. **Yeni Stream Analytics İşi** bölmesinde aşağıdaki adımları uygulayın:

   1. **İş adı** kutusuna bir iş adı yazın.
   
   1. IoT hub'ınızla aynı **Kaynak grubu** ve **Konum** bilgilerini kullanın. 

      > [!NOTE]
      > Şu an için IoT Edge üzerindeki Azure Stream Analytics işleri Batı ABD Batı 2 bölgesinde desteklenmemektedir. 

   1. **Barındırma ortamı**'nda **Edge**'i seçin.
    
1. **Oluştur**’u seçin.

1. Oluşturulan işin **İş Topolojisi** bölümünde **Girişler**'i açın.

   ![Azure Stream Analytics girişi](./media/tutorial-deploy-stream-analytics/asa_input.png)

1. **Akış girişi ekle**'yi ve ardından **Edge Hub'ı** öğesini seçin.

1. **Yeni giriş** bölmesinde giriş diğer adı olarak **temperature** yazın. 

1. **Kaydet**’i seçin.

1. **İş Topolojisi**'nin altında **Çıkışlar**'ı açın.

   ![Azure Stream Analytics çıkışı](./media/tutorial-deploy-stream-analytics/asa_output.png)

1. **Ekle**'yi ve ardından **Edge Hub'ı** öğesini seçin.

1. **Yeni çıkış** bölmesinde çıkış diğer adı olarak **alert** yazın. 

1. **Kaydet**’i seçin.

1. **İş Topolojisi** bölümünde **Sorgu**'yu seçip varsayılan metnin yerine ortalama makine sıcaklığının 30 saniyelik bir zaman penceresinde 70 dereceye ulaşması halinde bir uyarı oluşturan aşağıdaki sorguyu girin:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```

1. **Kaydet**’i seçin.

1. **Yapılandır**'ın altında **IoT Edge ayarları**'nı seçin.

1. Açılan menüden **Depolama hesabınızı** seçin.

1. **Kapsayıcı** alanında **Yeni oluştur**'u seçip depolama kapsayıcısı için bir ad girin. 

1. **Kaydet**’i seçin. 


## <a name="deploy-the-job"></a>İşi dağıtma

Artık Azure Stream Analytics işinizi IoT Edge cihazınıza dağıtmaya hazırsınız.

1. Azure portalında, IoT hub'ınızda **IoT Edge** bölümüne gidip IoT Edge cihazınızın ayrıntılar sayfasını açın.

1. **Modül ayarla**’yı seçin.  

   tempSensor modülünü bu cihaza önceden dağıttıysanız değer otomatik olarak doldurulabilir. Aksi takdirde aşağıdaki adımlarla modülü ekleyin:

   1. **Ekle**'ye tıklayıp **IoT Edge Modülü**'nü seçin.
   1. Ad alanına **tempSensor** yazın.
   1. Görüntü URI'si alanına **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** yazın. 
   1. Diğer ayarları değiştirmeden bırakın.
   1. **Kaydet**’i seçin.

1. Aşağıdaki adımları uygulayarak Azure Stream Analytics Edge işinizi ekleyin:

   1. **Ekle**'ye tıklayıp **Azure Stream Analytics Modülü**'nü seçin.
   1. Aboneliğinizi ve oluşturduğunuz Azure Stream Analytics Edge işini seçin. 
   1. **Kaydet**’i seçin.

1. **İleri**’yi seçin.

1. **Rotalar** bölümündeki varsayılan değeri aşağıdaki kodla değiştirin. _{moduleName}_ alanını Azure Stream Analytics modülünüzün adıyla güncelleştirin. Modülün oluşturulduğu işle aynı ada sahip olması gerekir. 

    ```json
    {
        "routes": {
            "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream",
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")",
            "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")"
        }
    }
    ```

1. **İleri**’yi seçin.

1. **Dağıtımı Gözden Geçirin** adımında **Gönder**'i seçin.

1. Cihaz ayrıntıları sayfasına dönüp **Yenile**'yi seçin.  

    Yeni Stream Analytics modülü ve IoT Edge aracı modülü ile birlikte çalışan yeni IoT Edge hub'ını görürsünüz.

    ![Modül çıkışı][7]

## <a name="view-data"></a>Verileri görüntüleme

Artık IoT Edge cihazınıza giderek Azure Stream Analytics modülüyle tempSensor modülü arasındaki etkileşimi kontrol edebilirsiniz.

1. Docker'daki tüm modüllerin çalıştığından emin olun:

   ```cmd/sh
   iotedge list  
   ```
<!--
   ![Docker output][8]
-->
1. Tüm sistem günlüklerini ve ölçüm verilerini görüntüleyin. Stream Analytics modülünün adını kullanın:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Makine sıcaklığını kademeli olarak 30 saniye boyunca 70 dereceye ulaşana kadar izleyebiliyor olmanız gerekir. Bu noktada Stream Analytics modülü bir sıfırlama işlemini tetikler ve makine sıcaklığı 21'e düşer. 

   ![Docker günlüğü][9]

## <a name="clean-up-resources"></a>Kaynakları temizleme 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Bir sonraki önerilen makaleye geçecekseniz oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Azure kaynaklarını ve kaynak grubunu silme işlemi geri alınamaz. Silindikten sonra kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Yalnızca IoT Hub'ı silmek için hub adını ve kaynak grubu adını kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub delete --name {hub_name} --resource-group IoTEdgeResources
```


Kaynak grubunun tamamını adıyla silmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

1. **Ada göre filtrele...** metin kutusuna IoT Hub'ınızın bulunduğu kaynak grubunun adını girin. 

1. Sonuç listesinde kaynak grubunuzun sağ tarafında **...** ve sonra **Kaynak grubunu sil**'e tıklayın.

<!--
   ![Delete](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)
-->
1. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını tekrar yazın ve **Sil**'e tıklayın. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınızdan gelen verileri analiz etmek için bir Azure Streaming Analytics işi yapılandırdınız. Ardından bu Azure Stream Analytics modülünü IoT Edge cihazınıza yükleyerek yerel ortamdaki sıcaklık artışını izleme ve buna tepki göstermeye ek olarak toplanan veri akışının buluta gönderilmesini sağladınız. Azure IoT Edge sisteminin işletmeniz için oluşturabileceği ek çözümleri görmek için diğer öğreticilere geçin.

> [!div class="nextstepaction"] 
> [Bir Azure Machine Learning modelini modül olarak dağıtma][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output2.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

