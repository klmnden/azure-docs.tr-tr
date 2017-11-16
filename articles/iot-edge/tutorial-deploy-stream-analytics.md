---
title: "Azure Stream Analytics Azure IOT Edge ile dağıtma | Microsoft Docs"
description: "Sınır cihazı bir modüle olarak Azure akış analizi dağıtma"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: ebda79442b8feb9f052c3ae455fa43aafb7b5a6a
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Azure Stream Analytics IOT kenar modül olarak dağıtma - Önizleme

IOT cihazları çok miktarda veri üretebilir. Bazen bu verileri analiz veya karşıya yüklenen verilerin boyutunu azaltmak için veya bir işlem yapılabilir öngörü gidiş dönüş gecikmesi ortadan kaldırmak için bulut ulaşmadan önce işlenen gerekir.

[Azure Stream Analytics] [ azure-stream] (ASA) bir zengin yapılandırılmış sorgu söz dizimi cihazları hem bulutta hem de IOT kenar üzerinde veri analizi için sağlar. IOT kenar ASA hakkında daha fazla bilgi için bkz: [ASA belgelerine](../stream-analytics/stream-analytics-edge.md).

Bu öğreticide bir IOT sınır aygıtında doğrudan cihazda yerel telemetri akışına işlemek ve sürücü Acil eylem cihazda için uyarıları oluşturmak için bir Azure akış analizi işi ve onun dağıtım oluşturulmasını açıklanmaktadır.  Bu öğreticide ilgili iki modülü vardır. 20 120 5 saniyede 1 azaltılır dereceye sıcaklık veri üreten bir benzetimli sıcaklık algılayıcısı Modülü (tempSensor) ve etme 100 dereceden büyüktür filtreleyen bir ASA modülü. 30 saniyelik ortalama 100 ulaştığında ASA modülü de tempSensor sıfırlar.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Edge verileri işlemek için bir ASA işi oluşturma
> * Yeni ASA işi diğer IOT kenar modüller ile bağlanma
> * ASA iş IOT kenar cihazına dağıtın

## <a name="prerequisites"></a>Ön koşullar

* IOT hub'ı 
* Oluşturduğunuz ve bir sanal cihaz hızlı başlangıç veya Azure IOT kenar dağıtmak om yapılandırılmış aygıt [Windows] [ lnk-tutorial1-win] ve [Linux] [ lnk-tutorial1-lin].
* Docker IOT kenar aygıtınızda
    * [Docker Windows yükleme] [ lnk-docker-windows] ve emin olun çalıştığından.
    * [Docker Linux'ta yüklemek] [ lnk-docker-linux] ve emin olun çalıştığından.
* Python IOT kenar aygıtınızda 2.7.x
    * [Python 2.7 Windows yüklemek][lnk-python].
    * Ubuntu, dahil olmak üzere çoğu Linux dağıtımları Python 2.7 yüklü zaten var.  PIP yüklü olduğundan emin olmak için aşağıdaki komutu kullanın: `sudo apt-get install python-pip`.

> [!NOTE]
> Cihaz bağlantı dizesini ve IOT kenar cihaz kimliği Bu öğretici için gerekli olacak unutmayın.

IOT kenar hızlı dağıtım için önceden oluşturulmuş Azure hizmet IOT kenar modülleri yararlanır ve Azure akış analizi (ASA) bir tür modülüdür. Kendi portalından ASA iş oluşturun, sonra bir IOT kenar modülü olarak dağıtmak için IOT hub'ı portalına dönün.  

Azure Stream Analytics hakkında daha fazla bilgi için bkz: **genel bakış** bölümünü [Stream Analytics belgeleri][azure-stream].

## <a name="create-an-asa-job"></a>Bir ASA işi oluşturma

Bu bölümde, verilerin alınacağı IOT hub'ınızı, cihazınızdan gönderilen telemetri verileri sorgulamak ve sonuçları bir Azure depolama kapsayıcısı (BLOB) iletmek için bir Azure akış analizi işi oluşturun. Daha fazla bilgi için bkz: **genel bakış** bölümünü [Stream Analytics belgeleri][azure-stream]. 

> [!NOTE]
> Bir Azure depolama hesabı ASA işinizde bir çıkış olarak kullanılacak bir uç nokta sağlamak için gereklidir. Aşağıdaki örnek BLOB Depolama türünü kullanır.  Daha fazla bilgi için bkz: **BLOB'lar** bölümünü [Azure Storage belgeleri][azure-storage].

1. Azure portalında gidin **kaynak Oluştur -> depolama**, tıklatın **tümünü görmek**, tıklatıp **depolama hesabı - blob, dosya, tablo, kuyruk**.

2. Depolama hesabınız için bir ad girin ve IOT Hub'ınızı depolandığı aynı konumu seçin. **Oluştur**'a tıklayın. Daha sonra için adını not edin.

    ![Yeni depolama hesabı][1]

3. Azure portalında az önce oluşturduğunuz depolama hesabınıza gidin. Tıklatın **Gözat BLOB'lar** altında **Blob hizmeti**. 
1. ASA modülün verileri depolamak yeni bir kapsayıcı oluşturun. Erişim düzeyini ayarlamak _kapsayıcı_. **Tamam** düğmesine tıklayın.

    ![Depolama ayarları][10]

1. Azure portalında gidin **kaynak oluşturma** > **nesnelerin interneti** seçip **akış analizi işi**.

1. Bir ad girin **barındırma ortamı olarak "kenar" seçin** ve geri kalan varsayılan değerler kullanın.  **Oluştur**'a tıklayın.

    ![ASA oluşturma][5]

2. Oluşturulan iş altında gidin **iş topoloji**seçin **girişleri**, tıklatın **Ekle**.

3. Bir ad girin `temperature`, seçin **veri akışı** kaynak türü ve diğer parametreler için varsayılanları kullan. **Oluştur**'a tıklayın.

    ![ASA giriş][2]

    > [!NOTE]
    > Ek girişleri IOT kenar belirli uç noktalar ekleyebilirsiniz.

4. Altında **iş topoloji**seçin **çıkışları**, tıklatın **Ekle**.

5. Bir ad girin `alert` ve varsayılanları kullanın. **Oluştur**'a tıklayın.

    ![ASA çıktı][3]

6. Altında **iş topoloji**seçin **sorgu**ve aşağıdakileri girin:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 100
    ```

## <a name="deploy-the-job"></a>İş dağıtma

Artık IOT kenar Cihazınızı ASA işinde dağıtmaya hazır olursunuz.

1. Azure portalında IOT Hub'ınızı gidin **IOT kenar (Önizleme)** ve açın, *{DeviceID}*'s dikey.

1. Seçin **ayarlamak modülleri**seçeneğini belirleyip **alma Azure hizmeti IOT kenar Modülü**.

1. Abonelik ve oluşturduğunuz ASA kenar işi seçin. Ardından depolama hesabınızı seçin. **Kaydet** düğmesine tıklayın.

    ![set Modülü][6]

1. Tıklatın **IOT kenar Modül Ekle** sıcaklık algılayıcısı modül eklemek için. Girin _tempSensor_ ad `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` için resim URL'si. Diğer ayarları değiştirmeden bırakın ve tıklayın **kaydetmek**.

    ![Sıcaklık Modülü][11]

1. ASA modülünün adını kopyalayın. Tıklatın **sonraki** yolları yapılandırmak için.

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

1. Cihaz ayrıntıları sayfasına dönün ve **yenileme**.  Yeni görmelisiniz _{moduleName}_ modülü ile birlikte çalışan **IOT kenar Aracısı** modülü ve **IOT kenar hub**.

    ![Modül çıkış][7]

## <a name="view-data"></a>Verileri görüntüleme

Artık IOT kenar Cihazınızı ASA modülü ve tempSensor modülünün arasındaki etkileşimi kullanıma gidebilirsiniz.

1. Bir komut isteminde IOT kenar cihaz bağlantı dizenizi ile çalışma zamanı yapılandırın:

    ```cmd/sh
    iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords  
    ```

1. Çalışma zamanı başlatmak için komutu çalıştırın:

    ```cmd/sh
    iotedgectl start  
    ```

1. Çalışan modüllerin görmek için komutu çalıştırın:

    ```cmd/sh
    docker ps  
    ```

    ![docker çıkış][8]

1. Tüm sistem günlüklerini ve ölçüm verilerini görmek için komutu çalıştırın. Modül adı üstten kullanın:

    ```cmd/sh
    docker logs -f {moduleName}  
    ```

    ![docker günlük][9]

1. Azure portalında depolama hesabınızın altında **Blob hizmeti**, tıklatın **Gözat BLOB'lar**, kapsayıcıyı seçin ve yeni oluşturulan JSON dosyasını seçin.

1. Tıklatın **karşıdan** ve sonuçları görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure depolama kapsayıcısı ve IOT kenar cihazınızdan verileri çözümlemek için bir akış analizi işi yapılandırılır.  Ardından BLOB indirmek içine akış aracılığıyla cihazınızdan verileri taşımak için özel bir ASA modülü de yüklenir.  Diğer Azure IOT kenar çözümleri, işiniz için nasıl oluşturabileceğinizi görmek için diğer öğreticileri açın devam edebilirsiniz.

> [!div class="nextstepaction"] 
> [Bir Azure Machine Learning modeli bir modül olarak dağıtma][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[2]: ./media/tutorial-deploy-stream-analytics/asa_input.png
[3]: ./media/tutorial-deploy-stream-analytics/asa_output.png
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