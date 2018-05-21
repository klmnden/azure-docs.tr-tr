---
title: Azure Stream Analytics Azure IOT Edge ile dağıtma | Microsoft Docs
description: Sınır cihazı bir modüle olarak Azure akış analizi dağıtma
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5d4c2b3bc55b94b08287a06125e15ac61013834a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Azure Stream Analytics IOT kenar modül olarak dağıtma - Önizleme

IOT cihazları çok miktarda veri üretebilir. Karşıya yüklenen veri miktarını azaltmak için bir eyleme dönüştürülebilir öngörü gidiş dönüş gecikmesi ortadan kaldırmak için verileri gereken bazen analiz veya Bulut erişmeden önce işlenen.

Azure IOT kenar modülleri hızlı dağıtım için önceden oluşturulmuş Azure hizmeti IOT kenar yararlanır. [Azure Stream Analytics] [ azure-stream] böyle bir modüldür. Kendi portalından bir Azure Stream Analytics işi oluşturmak ve IOT kenar modülü olarak dağıtmak için Azure IOT hub'ı portal gidin. 

Azure Stream Analytics cihazlar zengin yapılandırılmış sorgu söz dizimi hem bulutta hem de IOT kenar üzerinde veri analizi için sağlar. IOT kenar Azure Stream Analytics hakkında daha fazla bilgi için bkz: [Azure akış analizi belgeleri](../stream-analytics/stream-analytics-edge.md).

Bu öğreticide, bir Azure akış analizi işi oluşturma ve bir IOT sınır cihazı dağıtma aracılığıyla açıklanmaktadır. Bunun yapılması, doğrudan cihazda yerel telemetri akışına işlemek ve cihazda Acil eylem sürücü uyarı oluşturmasını sağlar. 

Öğretici iki modülleri gösterir: 
* 20 120 5 saniyede 1 azaltılır dereceye sıcaklık veri üreten benzetimli sıcaklık algılayıcısı modül (tempSensor). 
* 30 saniyelik ortalama 70 ulaştığında tempSensor sıfırlar Stream Analytics modül. Bir üretim ortamında, bir makineyi kapatın veya sıcaklık tehlikeli düzeyleri ulaştığında önlemler almak için bu işlevi kullanabilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Verileri işlemek için bir Azure akış analizi işi kenarına oluşturun.
> * Yeni Azure akış analizi işi diğer IOT kenar modüller ile bağlanır.
> * Azure Stream Analytics işi IOT kenar cihazına dağıtın.

## <a name="prerequisites"></a>Önkoşullar

* IOT hub'ı. 
* Oluşturulan ve hızlı veya sanal bir cihaz üzerinde Azure IOT kenar dağıtma hakkında makaleleri yapılandırılmış aygıt [Windows] [ lnk-tutorial1-win] veya [Linux] [ lnk-tutorial1-lin]. Aygıt bağlantısı anahtarı ve cihaz kimliğini bilmeniz gerekir 
* IOT kenar cihazda çalışan docker.
    * [Docker Windows yükleme][lnk-docker-windows].
    * [Docker Linux'ta yüklemek][lnk-docker-linux].
* Python 2.7.x IOT kenar Cihazınızda.
    * [Python 2.7 Windows yüklemek][lnk-python].
    * Ubuntu, dahil olmak üzere çoğu Linux dağıtımları Python 2.7 yüklü zaten var. PIP yüklendiğinden emin olmak için aşağıdaki komutu kullanın: `sudo apt-get install python-pip`.

## <a name="create-an-azure-stream-analytics-job"></a>Bir Azure akış analizi işi oluşturma

Bu bölümde, verilerin alınacağı IOT hub'ınızı, cihazınızdan gönderilen telemetri verileri sorgulamak ve sonuçları bir Azure Blob Depolama kapsayıcısını iletmek için bir Azure akış analizi işi oluşturun. Daha fazla bilgi için "Genel bakış" bölümüne bakın [Stream Analytics belgeleri][azure-stream]. 

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir Azure Storage hesabı, bir Azure Stream Analytics işi çıkış olarak kullanılacak bir uç nokta sağlamak için gereklidir. Bu bölümdeki örnek Blob Depolama türünü kullanır. Daha fazla bilgi için "BLOB" bölümüne bakın [Azure Storage belgeleri][azure-storage].

1. Azure portalında Git **kaynak oluşturma**, girin **depolama hesabı** arama kutusuna ve ardından **depolama hesabı - blob, dosya, tablo, kuyruk**.

2. İçinde **depolama hesabı oluşturma** bölmesinde, depolama hesabınız için bir ad girin, IOT hub'ınızı depolandığı aynı konumu seçin, IOT hub'ınızı aynı kaynak grubunu seçin ve ardından **oluşturma**. Daha sonra kullanmak için adını not edin.

    ![Depolama hesabı oluşturma][1]

3. Az önce oluşturduğunuz depolama hesabına gidin ve ardından **Gözat BLOB'lar**. 

4. Veri depolamak erişim ayarlamak Azure Stream Analytics modülü için yeni bir kapsayıcı düzeyini oluşturma **kapsayıcı**ve ardından **Tamam**.

    ![Depolama ayarları][10]

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure portalında Git **kaynak oluşturma** > **nesnelerin interneti**ve ardından **akış analizi işi**.

2. İçinde **yeni akış analizi işi** bölmesinde, aşağıdakileri yapın:

    a. İçinde **iş adı** bir iş adı yazın.
    
    b. Altında **barındırma ortamı**seçin **kenar**.
    
    c. Kalan alanlara varsayılan değerleri kullanın.

    > [!NOTE]
    > Şu anda, IOT kenar Azure akış analizi işleri Batı ABD 2 bölgede desteklenmiyor. 

3. **Oluştur**’u seçin.

4. Oluşturulan işteki altında **iş topoloji**, açık **girişleri**.

   ![Azure Stream Analytics giriş](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Seçin **akış giriş Ekle**seçeneğini belirleyip **kenar Hub**.

5. İçinde **yeni giriş** bölmesinde girin **sıcaklık** giriş diğer adı. 

6. **Kaydet**’i seçin.

7. Altında **iş topoloji**, açık **çıkışları**.

   ![Azure Stream Analytics çıktı](./media/tutorial-deploy-stream-analytics/asa_output.png)

8. Seçin **Ekle**seçeneğini belirleyip **kenar Hub**.

8. İçinde **yeni çıktı** bölmesinde girin **uyarı** çıktı diğer adı. 

9. **Oluştur**’u seçin.

9. Altında **iş topoloji**seçin **sorgu**ve ardından varsayılan metni aşağıdaki sorguyla değiştirin:

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

10. **Kaydet**’i seçin.

## <a name="deploy-the-job"></a>İş dağıtma

Artık IOT kenar Cihazınızı Azure Stream Analytics işinde dağıtmaya hazır olursunuz.

1. Azure portalında IOT hub'ınızı Git **IOT kenar (Önizleme)** ve IOT kenar cihazınız için Ayrıntılar sayfasını açın.

2. **Modül ayarla**’yı seçin.  
    Bu aygıtta tempSensor modülü daha önce dağıttıysanız, otomatik olarak doldurma olabilir. Yoksa, aşağıdakileri yaparak modüle ekleyin:

   a. **IoT Edge Modülü Ekle**'yi seçin.

   b. Bir ad yazın **tempSensor**.
    
   c. URI görüntü için girin **microsoft/azureiotedge-simulated-sıcaklık-algılayıcı: 1.0-Önizleme**. 

   d. Diğer ayarları değiştirmeden bırakın.
   
   e. **Kaydet**’i seçin.

3. Azure Stream Analytics Edge işinizi eklemek için seçin **alma Azure Stream Analytics IOT kenar Modülü**.

4. Aboneliğiniz ve oluşturduğunuz Azure Stream Analytics kenar işi seçin. 

5. Aboneliğiniz ve oluşturulan ve ardından depolama hesabını seçin **kaydetmek**.

    ![set Modülü][6]

6. Azure Stream Analytics modülünüzün adını kopyalayın. 

    ![Sıcaklık Modülü][11]

7. Yolları yapılandırmak için seçin **sonraki**.

8. Aşağıdaki kodu kopyalayın **yollar**. Değiştir _{moduleName}_ kopyaladığınız modül adı ile:

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

9. **İleri**’yi seçin.

10. **Şablonu Gözden Geçirin** adımında **Gönder**’i seçin.

11. Cihaz ayrıntıları sayfasına dönün ve ardından **yenileme**.  
    IOT kenar aracı modülü ve IOT kenar hub'ı ile birlikte çalışan yeni akış analizi modülü görmeniz gerekir.

    ![Modül çıkış][7]

## <a name="view-data"></a>Verileri görüntüleme

Artık IOT kenar Cihazınızı Azure akış analizi modülü ve tempSensor modülünün arasındaki etkileşimi kullanıma gidebilirsiniz.

1. Tüm modülleri Docker içinde çalıştığından emin olun:

   ```cmd/sh
   docker ps  
   ```

   ![docker çıkış][8]

2. Tüm sistem günlükleri ve ölçüm verilerini görüntüleme. Stream Analytics modül adı kullanın:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

Makinenin sıcaklığı 30 saniye 70 derece ulaşana kadar kademeli olarak yükselmeye izleyebilirler olması gerekir. Stream Analytics modülü bir sıfırlama sonra tetikler ve makine sıcaklık geri 21 ila bırakır. 

   ![docker günlük][9]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure depolama kapsayıcısı ve IOT kenar cihazınızdan verileri çözümlemek için bir akış analizi işi yapılandırılır. Ardından blob indirmek içine akış aracılığıyla cihazınızdan verileri taşımak için özel bir Azure akış analizi modülü de yüklenir. Azure IOT kenar işletmeniz için diğer çözümleri nasıl oluşturabileceğinizi görmek için diğer öğreticileri açın devam edin.

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
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/
