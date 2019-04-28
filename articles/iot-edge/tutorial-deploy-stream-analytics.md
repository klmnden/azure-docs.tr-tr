---
title: Öğretici - Azure IOT Edge cihaza Azure Stream Analytics işi dağıtma | Microsoft Docs
description: Bu öğreticide Azure Stream Analytics’i bir IoT Edge cihazına modül olarak dağıtacaksınız.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 31330c3d2b10e6245db775da4039fd1948539df4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62096292"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module"></a>Öğretici: Azure Stream Analytics IOT Edge modülü dağıtma

Çoğu IoT çözümünde, IoT cihazlarından buluta ulaşan veriler hakkında içgörü edinmek için analiz hizmetleri kullanılır. Azure IoT Edge ile [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) mantığını alıp cihazın kendisine aktarabilirsiniz. Telemetri akışlarını uç cihazlarda işleyerek yüklenen veri miktarını ve eyleme dönüştürülebilir içgörülere tepki verme süresini azaltabilirsiniz.

Azure IoT Edge ve Azure Stream Analytics tümleşik olduğundan Azure portalında bir Azure Stream Analytics işi oluşturup ek kod yazmadan IoT Edge modülü olarak dağıtabilirsiniz.  

Azure Stream Analytics hem bulutta hem de IoT Edge cihazlarında veri analizi için zengin bir yapılandırılmış sorgu söz dizimi sunar. IoT Edge üzerinde Azure Stream Analytics hakkında daha fazla bilgi için bkz. [Azure Stream Analytics belgeleri](../stream-analytics/stream-analytics-edge.md).

Bu öğreticideki Stream Analytics modülü, tekrar eden 30 saniyelik dönemler içindeki ortalama sıcaklığı hesaplar. Bu ortalama değer 70'e ulaştığında modül, eyleme geçmek için cihazla ilgili bir uyarı gönderir. Bu durumda eylem, sıcaklık sensörü simülasyonunu sıfırlamaktır. Bir üretim ortamında bu işlevi kullanarak sıcaklık tehlikeli düzeylere ulaştığında bir makineyi kapatabilir veya önleyici işlemler gerçekleştirebilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Verileri uç cihazlarda işlemek için bir Azure Stream Analytics işi oluşturma.
> * Yeni Azure Stream Analytics işini diğer IoT Edge modüllerine bağlama.
> * Azure Stream Analytics işini Azure portalından bir IoT Edge cihazına dağıtma.

<center>

![Diyagram - öğretici mimarisi, aşama ve ASA işi dağıtma](./media/tutorial-deploy-stream-analytics/asa-architecture.png)
</center>


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 


## <a name="create-an-azure-stream-analytics-job"></a>Azure Stream Analytics işi oluşturma

Bu bölümde IoT hub'ınızdaki verileri almak, cihazınızdan gönderilen telemetri verilerini sorgulamak ve ardından sonuçları bir Azure Blob depolama kapsayıcısına iletmek için bir Azure Stream Analytics işi oluşturacaksınız. 

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

IoT Edge cihazında çalıştırmak üzere oluşturduğunuz Azure Stream Analytics işinin cihazdan çağrılabilecek bir şekilde depolanması gerekir. Mevcut bir Azure depolama hesabını kullanabilir veya yeni bir hesap oluşturabilirsiniz. 

1. Azure portalda **Kaynak oluştur** > **Depolama** > **Depolama hesabı - blob, dosya, tablo, sorgu** yolunu izleyin. 

1. Depolama hesabınızı oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer |
   | ----- | ----- |
   | Ad | Depolama hesabınıza benzersiz bir ad verin. | 
   | Konum | Size yakın bir konum seçin. |
   | Abonelik | IoT hub'ınızla aynı aboneliği seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |

1. Diğer alanlar için varsayılan değerleri kullanın ve **Oluştur**'u seçin. 

### <a name="create-a-new-job"></a>Yeni bir iş oluşturma

1. Azure portalda **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics İşi**'ne gidin.

1. İşinizi oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer |
   | ----- | ----- |
   | İş adı | İşinize bir ad verin. Örneğin, **IoTEdgeJob** | 
   | Abonelik | IoT hub'ınızla aynı aboneliği seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. | 
   | Barındırma ortamı | **Kenar**'ı seçin. |
 
1. **Oluştur**’u seçin.

### <a name="configure-your-job"></a>İşinizi yapılandırma

Azure portalda Stream Analytics işiniz oluşturulduktan sonra iletilen verilerle çalışması için giriş, çıkış ve sorgu ile yapılandırabilirsiniz. 

Bu bölümde giriş, çıkış ve sorgu öğelerini kullanarak IoT Edge cihazından sıcaklık verilerini alan bir iş oluşturabilirsiniz. Bu iş 30 saniyelik aralıklarla verileri analiz eder. Herhangi bir aralıktaki ortalama sıcaklık değeri 70 derecenin üzerine çıkarsa IoT Edge cihazına bir uyarı gönderilir. Bir sonraki bölümde işi dağıtırken verilerin geldiği ve gittiği yerleri belirteceksiniz.  

1. Azure portalda Stream Analytics işinize gidin. 

1. **İş Topolojisi** bölümünde **Girişler**'i ve **Akış girişi ekle**'yi seçin.

   ![Azure Stream Analytics Girişi Ekle](./media/tutorial-deploy-stream-analytics/asa_input.png)

1. Açılan listeden **Edge Hub**'ını seçin.

1. **Yeni giriş** bölmesinde giriş diğer adı olarak **temperature** yazın. 

1. Diğer alanlar için varsayılan değerleri kullanın ve **Kaydet**'i seçin.

1. **İş Topolojisi**'nin altında **Çıkışlar**'ı açın ve **Ekle**'yi seçin.

   ![Azure Stream Analytics çıkış Ekle](./media/tutorial-deploy-stream-analytics/asa_output.png)

1. Açılan listeden **Edge Hub**'ını seçin.

1. **Yeni çıkış** bölmesinde çıkış diğer adı olarak **alert** yazın. 

1. Diğer alanlar için varsayılan değerleri kullanın ve **Kaydet**'i seçin.

1. **İş Topolojisi**'nin altında **Sorgu**'yu seçin.

1. Varsayılan metni aşağıdaki sorguyla değiştirin. 30 saniyelik süre içindeki ortalama makine sıcaklığı 70 dereceye ulaşırsa SQL kodu uyarı çıkışına bir sıfırlama komutu gönderir. Sıfırlama komutu, gerçekleştirilebilecek bir eylem olarak önceden sensöre programlanmıştır. 

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

### <a name="configure-iot-edge-settings"></a>IoT Edge ayarlarını yapılandırma

IoT Edge cihazınıza dağıtılacak Stream Analytics işinizi hazırlamak için işi depolama hesabındaki bir kapsayıcıyla ilişkilendirmeniz gerekir. İşinizi dağıttığınızda iş tanımı depolama kapsayıcısına aktarılır. 

1. **Yapılandır** bölümünde **Depolama hesabı ayarları**'nı seçin.

1. **Depolama hesabı ekle**'yi seçin. 

1. Açılan menüden **Depolama hesabınızı** seçin.

1. **Kapsayıcı** alanında **Yeni oluştur**'u seçip depolama kapsayıcısı için bir ad girin. 

1. **Kaydet**’i seçin. 

## <a name="deploy-the-job"></a>İşi dağıtma

Artık Azure Stream Analytics işinizi IoT Edge cihazınıza dağıtmaya hazırsınız. 

Bu bölümde Azure portaldaki **Modülleri Ayarlama** sihirbazını kullanarak bir *dağıtım bildirimi* oluşturacaksınız. Dağıtım bildirimi bir cihaza dağıtılacak tüm modülleri, modül görüntülerinin depolandığı kapsayıcı kayıt defterlerini, modüllerin yönetilme şeklini ve modüllerin birbirleriyle iletişim kurma şeklini belirten bir JSON dosyasıdır. IoT Edge cihazınız, dağıtım bildirimini IoT Hub'dan aldıktan sonra içindeki bilgileri kullanarak tüm atanmış modülleri dağıtır ve yapılandırır. 

Bu öğreticide iki modül dağıtacaksınız. Birinci, sıcaklık ve nem sensörü benzetimi yapan **tempSensor** modülüdür. İkincisi ise Stream Analytics işinizdir. Sensör modülü, iş sorgunuzun analiz edeceği veri akışını sağlar. 

1. Azure portalında, IoT hub'ınızda **IoT Edge** bölümüne gidip IoT Edge cihazınızın ayrıntılar sayfasını açın.

1. **Modül ayarla**’yı seçin.  

1. tempSensor modülünü bu cihaza önceden dağıttıysanız değer otomatik olarak doldurulabilir. Aksi takdirde aşağıdaki adımlarla modülü ekleyin:

   1. **Ekle**'ye tıklayıp **IoT Edge Modülü**'nü seçin.
   1. Ad alanına **tempSensor** yazın.
   1. Görüntü URI'si alanına **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** yazın. 
   1. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'i seçin.

1. Aşağıdaki adımları uygulayarak Azure Stream Analytics Edge işinizi ekleyin:

   1. **Ekle**'ye tıklayıp **Azure Stream Analytics Modülü**'nü seçin.
   1. Aboneliğinizi ve oluşturduğunuz Azure Stream Analytics Edge işini seçin. 
   1. **Kaydet**’i seçin.

1. Stream Analytics işiniz oluşturduğunuz depolama kapsayıcısında yayımlandıktan sonra modül adına tıklayarak Stream Analytics modülünün yapısını görebilirsiniz. 

   Görüntü URI'si, standart bir Azure Stream Analytics görüntüsünü işaret eder. Bu, IoT Edge cihazına dağıtılan her iş için kullanılan görüntüyle aynıdır. 

   Modül ikizi, **ASAJobInfo** adlı istenen özellikle yapılandırılır. Bu özelliğin değeri, depolama kapsayıcınızdaki iş tanımını gösterir. Bu özellik, Stream Analytics görüntüsünün belirli iş bilgilerinizle yapılandırılma şeklidir. 

1. Modül sayfasını kapatın.

1. Bir sonraki adımda kullanacağınız için Stream Analytics modülünüzün adını not edin ve devam etmek için **İleri**'yi seçin.

1. **Rotalar** bölümündeki varsayılan değeri aşağıdaki kodla değiştirin. _{moduleName}_ alanının üç örneğini Azure Stream Analytics modülünüzün adıyla güncelleştirin. 

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

   Burada belirttiğiniz rotalar, IoT Edge cihazından veri akışını belirler. tempSensor kaynağından gelen telemetri verileri, IoT Hub'a ve Stream Analytics işinde yapılandırılan **sıcaklık** girişine gönderilir. **Uyarı** çıkış iletileri, IoT Hub'a ve sıfırlama komutunu tetiklemek için tempSensor modülüne gönderilir. 

1. **İleri**’yi seçin.

1. **Dağıtımı Gözden Geçirin** adımında **Gönder**'i seçin.

1. Cihaz ayrıntıları sayfasına dönüp **Yenile**'yi seçin.  

    Yeni Stream Analytics modülü ve IoT Edge aracı modülü ile birlikte çalışan yeni IoT Edge hub'ını görürsünüz.

    ![tempSensor ve cihaz tarafından bildirilen ASA Modülü](./media/tutorial-deploy-stream-analytics/module_output2.png)

## <a name="view-data"></a>Verileri görüntüleme

Artık IoT Edge cihazınıza giderek Azure Stream Analytics modülüyle tempSensor modülü arasındaki etkileşimi kontrol edebilirsiniz.

1. Docker'daki tüm modüllerin çalıştığından emin olun:

   ```cmd/sh
   iotedge list  
   ```
   <!--
   ![Docker output](./media/tutorial-deploy-stream-analytics/docker_output.png)
   -->
1. Tüm sistem günlüklerini ve ölçüm verilerini görüntüleyin. Stream Analytics modülünün adını kullanın:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Makine sıcaklığını kademeli olarak 30 saniye boyunca 70 dereceye ulaşana kadar izleyebiliyor olmanız gerekir. Bu noktada Stream Analytics modülü bir sıfırlama işlemini tetikler ve makine sıcaklığı 21'e düşer. 

   ![Komut çıktısı modülü oturum açtığı Sıfırla](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 
 
[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınızdan gelen verileri analiz etmek için bir Azure Streaming Analytics işi yapılandırdınız. Ardından bu Azure Stream Analytics modülünü IoT Edge cihazınıza yükleyerek yerel ortamdaki sıcaklık artışını izleme ve buna tepki göstermeye ek olarak toplanan veri akışının buluta gönderilmesini sağladınız. Azure IoT Edge sisteminin işletmeniz için oluşturabileceği ek çözümleri görmek için diğer öğreticilere geçin.

> [!div class="nextstepaction"] 
> [Bir Azure Machine Learning modelini modül olarak dağıtma](tutorial-deploy-machine-learning.md)
