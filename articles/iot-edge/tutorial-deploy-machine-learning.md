---
title: Azure Machine Learning Azure IOT kenarıyla dağıtma | Microsoft Docs
description: Sınır cihazı bir modüle olarak Azure Machine Learning dağıtma
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/12/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 3d3a271bcdd5c507125b8b1a5482f833607a5a78
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Azure Machine Learning IOT kenar modül olarak dağıtma - Önizleme

IOT kenar modülleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, bir aygıt, oluşturduğunuz sanal IOT sınır cihazı algılayıcı verilerini göre başarısız olduğunda tahmin bir Azure Machine Learning modülü dağıtımı aracılığıyla açıklanmaktadır [dağıtmak Azure IOT kenarına bir sanal cihaz Windows] [ lnk-tutorial1-win] veya [Linux] [ lnk-tutorial1-lin] öğreticileri. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Bir Azure Machine Learning modülü oluşturun
> * Azure kapsayıcı kayıt defterine modülü kapsayıcı bildirme
> * Bir Azure Machine Learning modülü IOT kenar Cihazınızı dağıtma
> * Oluşturulan görünüm verileri

Bu öğreticide oluşturduğunuz Azure Machine Learning modülü cihazınız tarafından oluşturulan ortam verilerini okur ve iletileri anormal olarak veya etiketler. 

## <a name="prerequisites"></a>Önkoşullar

* Hızlı Başlangıç ya da ilk öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* IOT kenar cihazın bağlandığı IOT hub'ın IOT Hub bağlantı dizesi.
* Bir Azure Machine Learning hesabı. Hesap oluşturmak için'ndaki yönergeleri izleyin [oluşturma Azure Machine Learning hesapları ve Azure Machine Learning çalışma ekranı yükleme](../machine-learning/preview/quickstart-installation.md#create-azure-machine-learning-services-accounts). Bu öğretici için çalışma ekranı uygulama yüklemeniz gerekmez. 
* Azure ML makinenizde modülü yönetimi. Ortamınızı ayarlama ve bir hesap oluşturmak için'ndaki yönergeleri izleyin [Model Yönetimi Kurulumu](https://docs.microsoft.com/azure/machine-learning/preview/deployment-setup-configuration).

Azure Machine Learning modülü ARM işlemcileri desteklemez. 

## <a name="create-the-azure-ml-container"></a>Azure ML kapsayıcı oluşturma
Bu bölümde eğitilen model dosyalarını indirmek ve bunları Azure ML kapsayıcıya dönüştürün.  

Modülü yönetimi için Azure ML çalıştığı makinede karşıdan yükle ve Kaydet [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) ve [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) github'da Azure ML IOT araç setinden. Bu dosyalar IOT kenar Cihazınızı dağıtacağınız modeli learning eğitilen makine tanımlayın. 

Eğitim modeli IOT kenar cihazlara dağıttığınız bir kapsayıcı oluşturmak için kullanın. Aşağıdaki komutu kullanın:

   * Modelinizi kaydedin.
   * Bir bildirim oluşturun.
   * Adlı bir Docker kapsayıcısı görüntü oluşturma *machinelearningmodule*.
   * Görüntü, Azure kapsayıcı hizmeti (AKS) kümeye dağıtın.

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```

### <a name="view-the-container-repository"></a>Kapsayıcı depo görüntüleyin

Kapsayıcı görüntünüzü başarıyla oluşturuldu ve makine öğrenme ortamınız ile ilişkili Azure kapsayıcı deposu içinde depolanan olduğunu denetleyin.

1. Üzerinde [Azure portal](https://portal.azure.com)gidin **tüm hizmetleri** seçip **kapsayıcı kayıt defterleri**.
2. Kayıt defteri seçin. Adı ile başlamalıdır **mlcr** ve kaynak grubu, konum ve modül yönetimini ayarlamak için kullanılan abonelik ait.
3. Seçin **erişim tuşları**
4. Kopya **oturum açma sunucusu**, **kullanıcıadı**, ve **parola**.  Bu sınır cihazlarınızdan kayıt defterine erişim gerekir.
5. Seçin **depoları**
6. Seçin **machinelearningmodule**
7. Artık kapsayıcının tam görüntü yolu vardır. Bu görüntü yolu bir sonraki bölüm için not edin. Aşağıdaki gibi görünmelidir: **< registry_name >.azureacr.io/machinelearningmodule:1**

## <a name="add-registry-credentials-to-your-edge-device"></a>Kayıt defteri kimlik bilgilerini kenar Cihazınızı ekleyin

Sınır cihazı çalıştırdığınız bilgisayarda kenar çalışma zamanı kayıt için kimlik bilgilerini ekleyin. Bu komut, kapsayıcı çıkarmak için çalışma zamanı erişim sağlar.

Linux:
   ```cmd
   sudo iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

Windows:
   ```cmd
   iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. Üzerinde [Azure portal](https://portal.azure.com), IOT hub'ına gidin.
1. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
1. **Modül ayarla**’yı seçin.
1. Daha önce IOT kenar Cihazınızı tempSensor modülü dağıttıktan sonra otomatik olarak doldurma olabilir. Modülleri listenizde değilse ekleyin.
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `tempSensor`.
    3. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. **Kaydet**’i seçin.
1. Makine öğrenimi oluşturduğunuz modül ekleyin.
    1. Seçin **IOT kenar Modül Ekle**.
    1. İçinde **adı** alanında, girin `machinelearningmodule`
    1. İçinde **görüntü** alan, görüntü adresinizi girin; örneğin `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. **Kaydet**’i seçin.
1. **Modül Ekle** adımına dönüp **İleri**’yi seçin.
1. İçinde **belirtin yollar** adım, JSON altındaki metin kutusuna Kopyala. İlk yol, tüm Azure Machine Learning modüllerinin kullanan uç nokta olduğu sıcaklık algılayıcısı iletilerden "amlInput" uç noktası aracılığıyla makine öğrenme modülü taşımaları. İkinci yol makine öğrenme modülü iletilerden IOT Hub'ına taşımaları. Bu rotadaki '' amlOutput'' veri çıkışı için tüm Azure Machine Learning modülleri kullanan uç nokta ve '' upstream$ '' IOT hub'ı gösterir. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. **İleri**’yi seçin. 
1. **Şablonu Gözden Geçirin** adımında **Gönder**’i seçin. 
1. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin.  Yeni görmelisiniz **machinelearningmodule** ile birlikte çalışan **tempSensor** modülü ve IOT kenar çalışma zamanı modülleri.

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

IOT kenar Cihazınızı kullanarak gönderen cihaz bulut iletilerini görüntüleyebilirsiniz [IOT hub'ı explorer](https://github.com/azure/iothub-explorer) veya Visual Studio Code için Azure IOT Araç Seti uzantısı. 

1. Visual Studio kod seçin **IOT Hub cihazları**. 
2. Seçin **...**  seçip **IOT Hub bağlantı dizesine ayarlamak** menüsünde. 

   ![IOT Hub cihazları ek menüsü](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Sayfanın en üstünde açılır metin kutusunda için IOT Hub'ınızı iothubowner bağlantı dizesini girin. IOT kenar Cihazınızı IOT Hub cihazları listesinde görünmesi gerekir.
4. Seçin **...**  yeniden seçip **D2C ileti İzlemeyi Başlat**.
5. Beş saniyede tempSensor gelen iletileri gözlemleyin. Adlı bir özellik ileti gövdesinde bulunsun **anomali** machinelearningmodule true veya false değeri ile sağlar. **AzureMLResponse** model başarıyla çalıştırılmışsa "Tamam" değer özelliği içerir. 

   ![Azure ML yanıt ileti gövdesi olarak](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning tarafından desteklenen bir IOT kenar modülü dağıtıldı. İşletme öngörüleri kenarına içine veri kapatmak Azure IOT kenar yardımcı olabilecek diğer yollarını öğrenmek için diğer öğreticileri birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir Azure işlevi bir modül olarak dağıtma](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
