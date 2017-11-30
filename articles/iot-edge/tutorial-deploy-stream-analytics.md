---
title: "Azure Stream Analytics Azure IOT Edge ile dağıtma | Microsoft Docs"
description: "Sınır cihazı bir modüle olarak Azure akış analizi dağıtma"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/28/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5a143bbf7abb5304ac51782d517c02ec184a05a2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Azure Stream Analytics IOT kenar modül olarak dağıtma - Önizleme

IOT cihazları çok miktarda veri üretebilir. Bazen bu verileri analiz veya karşıya yüklenen verilerin boyutunu azaltmak için veya bir işlem yapılabilir öngörü gidiş dönüş gecikmesi ortadan kaldırmak için bulut ulaşmadan önce işlenen gerekir.

IOT kenar hızlı dağıtım için önceden oluşturulmuş Azure hizmet IOT kenar modüllerin yararlanır ve [Azure akış analizi] [ azure-stream] (ASA) olan bir modül. Kendi portalından ASA iş oluşturun, sonra bir IOT kenar modülü olarak dağıtmak için IOT hub'ı portalına dönün.  

Azure Stream Analytics cihazlar zengin yapılandırılmış sorgu söz dizimi hem bulutta hem de IOT kenar üzerinde veri analizi için sağlar. IOT kenar ASA hakkında daha fazla bilgi için bkz: [ASA belgelerine](../stream-analytics/stream-analytics-edge.md).

Bu öğreticide bir IOT sınır aygıtında doğrudan cihazda yerel telemetri akışına işlemek ve sürücü Acil eylem cihazda için uyarıları oluşturmak için bir Azure akış analizi işi ve onun dağıtım oluşturulmasını açıklanmaktadır.  Bu öğreticide ilgili iki modülü vardır. Benzetimli sıcaklık algılayıcısı Modülü (tempSensor) sıcaklık veri 120 5 saniyede 1 azaltılır derece, 20 oluşturur. 30 saniyelik ortalama 70 ulaştığında bir akış analizi modülü tempSensor sıfırlar. Bir üretim ortamında, bu işlev devre dışı bir makineyi kapatın veya sıcaklık tehlikeli düzeyleri ulaştığında önlemler almak için kullanılabilir. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Edge verileri işlemek için bir ASA işi oluşturma
> * Yeni ASA işi diğer IOT kenar modüller ile bağlanma
> * ASA iş IOT kenar cihazına dağıtın

## <a name="prerequisites"></a>Ön koşullar

* IOT hub'ı 
* Oluşturduğunuz ve bir sanal cihaz hızlı başlangıç veya Azure IOT kenar dağıtmak om yapılandırılmış aygıt [Windows] [ lnk-tutorial1-win] ve [Linux] [ lnk-tutorial1-lin]. Aygıt bağlantısı anahtarı ve cihaz kimliğini bilmeniz gerekir 
* Docker IOT kenar aygıtta çalışan
    * [Windows Docker yükleyin][lnk-docker-windows]
    * [Docker Linux'ta yüklemek][lnk-docker-linux]
* Python IOT kenar aygıtınızda 2.7.x
    * [Python 2.7 Windows yüklemek][lnk-python].
    * Ubuntu, dahil olmak üzere çoğu Linux dağıtımları Python 2.7 yüklü zaten var.  PIP yüklü olduğundan emin olmak için aşağıdaki komutu kullanın: `sudo apt-get install python-pip`.


## <a name="create-an-asa-job"></a>Bir ASA işi oluşturma

Bu bölümde, verilerin alınacağı IOT hub'ınızı, cihazınızdan gönderilen telemetri verileri sorgulamak ve sonuçları bir Azure depolama kapsayıcısı (BLOB) iletmek için bir Azure akış analizi işi oluşturun. Daha fazla bilgi için bkz: **genel bakış** bölümünü [Stream Analytics belgeleri][azure-stream]. 

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir Azure depolama hesabı ASA işinizde bir çıkış olarak kullanılacak bir uç nokta sağlamak için gereklidir. Aşağıdaki örnek BLOB Depolama türünü kullanır.  Daha fazla bilgi için bkz: **BLOB'lar** bölümünü [Azure Storage belgeleri][azure-storage].

1. Azure portalında gidin **kaynak oluşturma** ve girin `Storage account` arama çubuğunda. Seçin **depolama hesabı - blob, dosya, tablo, kuyruk**.

2. Depolama hesabınız için bir ad girin ve IOT hub'ınızın bulunduğu aynı konumu seçin. **Oluştur**'a tıklayın. Daha sonra için ad unutmayın.

    ![Yeni depolama hesabı][1]

3. Az önce oluşturduğunuz depolama hesabınıza gidin. Tıklatın **Gözat BLOB'lar**. 
4. ASA modülün verileri depolamak yeni bir kapsayıcı oluşturun. Erişim düzeyini ayarlamak **kapsayıcı**. **Tamam** düğmesine tıklayın.

    ![Depolama ayarları][10]

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure portalında gidin **kaynak oluşturma** > **nesnelerin interneti** seçip **akış analizi işi**.

2. Bir ad girin, seçin **kenar** barındırma ortamı olarak ve geri kalan varsayılan değerler kullanın.  **Oluştur**'a tıklayın.

    >[!NOTE]
    >Şu anda, IOT kenar ASA işleri Batı ABD 2 bölgede desteklenmiyor. 

3. Oluşturulan iş gidin. Seçin **girişleri** ardından **Ekle**.

4. Giriş diğer adı için girin `temperature`, kaynak türünü ayarlamak **veri akışı**ve diğer parametreler için varsayılanları kullanın. **Oluştur**'a tıklayın.

   ![ASA giriş](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Seçin **çıkışları** ardından **Ekle**.

6. Çıkış diğer girin `alert`ve diğer parametreler için varsayılanları kullanın. **Oluştur**'a tıklayın.

   ![ASA çıktı](./media/tutorial-deploy-stream-analytics/asa_output.png)


7. Seçin **sorgu**.
8. Varsayılan metin aşağıdaki sorguyla değiştirin:

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
9. **Kaydet** düğmesine tıklayın.

## <a name="deploy-the-job"></a>İş dağıtma

Artık IOT kenar Cihazınızı ASA işinde dağıtmaya hazır olursunuz.

1. Azure portalında IOT hub'ınızı gidin **IOT kenar (Önizleme)** ve IOT kenar cihazınız için Ayrıntılar sayfasını açın.
1. Seçin **ayarlamak modülleri**.
1. Bu aygıtta tempSensor modülü daha önce dağıttıysanız, otomatik olarak doldurma olabilir. Aksi halde, bu modül eklemek için aşağıdaki adımları kullanın:
   1. Tıklatın **IOT kenar Modül Ekle**
   1. Girin `tempSensor` adı olarak ve `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` görüntü URI. 
   1. Diğer ayarları değiştirmeden bırakın ve tıklayın **kaydetmek**.
1. ASA Edge işinizi eklemek için seçin **alma Azure Stream Analytics IOT kenar Modülü**.
1. Aboneliğiniz ve oluşturduğunuz ASA kenar işi seçin. 
1. Aboneliğiniz ve oluşturduğunuz depolama hesabını seçin. **Kaydet** düğmesine tıklayın.

    ![set Modülü][6]

1. ASA modülü için otomatik olarak oluşturulan adı kopyalayın. 

    ![Sıcaklık Modülü][11]

1. Tıklatın **sonraki** yolları yapılandırmak için.
1. Şu şekilde kopyalayın **yollar**.  Değiştir _{moduleName}_ kopyaladığınız modül adı ile:

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

1. **İleri**’ye tıklayın.

1. İçinde **gözden geçirme şablonu** adımını, **gönderme**.

1. Cihaz ayrıntıları sayfasına dönün ve **yenileme**.  İle birlikte çalışan yeni akış analizi modülü görmelisiniz **IOT kenar Aracısı** modülü ve **IOT kenar hub**.

    ![Modül çıkış][7]

## <a name="view-data"></a>Verileri görüntüleme

Artık IOT kenar Cihazınızı ASA modülü ve tempSensor modülünün arasındaki etkileşimi kullanıma gidebilirsiniz.

Tüm modülleri Docker içinde çalıştığından emin olun:

   ```cmd/sh
   docker ps  
   ```

   ![docker çıkış][8]

Tüm sistem günlükleri ve ölçüm verilerini bakın. Stream Analytics modül adı kullanın:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

Makinenin sıcaklığı 30 saniye 70 derece ulaşana kadar kademeli olarak yükselmeye izleyebilirler olması gerekir. Stream Analytics modülü bir sıfırlama sonra tetikler ve makine sıcaklık geri 21 ila bırakır. 

   ![docker günlük][9]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure depolama kapsayıcısı ve IOT kenar cihazınızdan verileri çözümlemek için bir akış analizi işi yapılandırılır.  Ardından BLOB indirmek içine akış aracılığıyla cihazınızdan verileri taşımak için özel bir ASA modülü de yüklenir.  Diğer Azure IOT kenar çözümleri, işiniz için nasıl oluşturabileceğinizi görmek için diğer öğreticileri açın devam edebilirsiniz.

> [!div class="nextstepaction"] 
> [Bir Azure Machine Learning modeli bir modül olarak dağıtma][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/en-us/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/en-us/azure/storage/
[azure-stream]: https://docs.microsoft.com/en-us/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/