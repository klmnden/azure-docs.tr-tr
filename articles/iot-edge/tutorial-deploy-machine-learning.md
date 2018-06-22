---
title: Azure IoT Edge ile Azure Machine Learning dağıtma | Microsoft Belgeleri
description: Azure Machine Learning'i modül olarak bir Edge cihazına dağıtma
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/12/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 248bc97c214c013d10f1839201ce2f572cb854ed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631182"
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Azure Machine Learning'i bir IoT Edge modülü olarak dağıtma - önizleme

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğretici, [Windows'da benzetimli bir cihaza Azure IoT Edge dağıtma][lnk-tutorial1-win] veya [Linux'ta benzetimli bir cihaza Azure IoT Edge dağıtma][lnk-tutorial1-lin] öğreticilerinde oluşturduğunuz benzetimli IoT Edge cihazı üzerindeki algılayıcı verilerine göre bir cihazın hata vereceğini öngören bir Azure Machine Learning modülünü dağıtmayı adım adım gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Machine Learning modülü oluşturma
> * Azure kapsayıcı kayıt defterine bir modülü kapsayıcısı gönderme
> * IoT Edge cihazınıza bir Azure Machine Learning modülü dağıtma
> * Oluşturulan verileri görüntüleme

Bu öğreticide oluşturduğunuz Azure Machine Learning modülü, cihazınızın ürettiği ortam verilerini okur ve iletileri normal veya anormal olarak etiketler.

## <a name="prerequisites"></a>Ön koşullar

* Hızlı başlangıçta veya ilk öğreticide oluşturduğunuz Azure IoT Edge cihazı.
* IoT Edge cihazınızın bağlandığı IoT hub'ının IoT Hub bağlantı dizesi.
* Bir Azure Machine Learning hesabı. Bir hesap oluşturmak için, [Azure Machine Learning hesapları oluşturma ve Azure Machine Learning Workbench yükleme](../machine-learning/service/quickstart-installation.md#create-azure-machine-learning-services-accounts) yönergelerini izleyin. Bu öğretici için çalışma ekranı uygulamasını yüklemeniz gerekmez. 
* Makinenizdeki Azure ML için Modülü Yönetimi. Ortamınızı kurmak ve bir hesap oluşturmak için, [Model yönetimi kurulumu](../machine-learning/desktop-workbench/deployment-setup-configuration.md)'ndaki yönergeleri izleyin.

Azure Machine Learning modülü ARM işlemcilerini desteklemez.

## <a name="create-the-azure-ml-container"></a>Azure ML kapsayıcısını oluşturma
Bu bölümde eğitilen modelin dosyalarını indirecek ve bunları Azure ML kapsayıcısına dönüştüreceksiniz.

Azure ML için Modül Yönetimi'ni çalıştıran makinede, GitHub'daki Azure ML IoT Toolkit'ten [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) ve [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) dosyalarını indirin ve kaydedin. Bu dosyalar, Iot Edge cihazınıza dağıtacağınız eğitilen makine öğrenme modelini tanımlar.

Eğitilen modeli IoT Edge cihazlarına dağıtılabilecek bir kapsayıcı oluşturmak için kullanın. Şunları yapmak için aşağıdaki komutu kullanın:

   * Modelinizi kaydedin.
   * Bir bildirim oluşturun.
   * *machinelearningmodule* adında bir Docker kapsayıcı görüntüsü oluşturun.
   * Görüntüyü Azure Kubernetes Service (AKS) kümesine dağıtın.

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```

### <a name="view-the-container-repository"></a>Kapsayıcı deposunu görüntüleme

Kapsayıcınızın görüntüsünün başarıyla oluşturulduğundan ve makine öğrenimi ortamınızla ilişkili Azure kapsayıcı deposuna depolandığından emin olun.

1. [Azure portalda](https://portal.azure.com), **Tüm Hizmetler**'e gidin ve **Kapsayıcı kayıt defterleri**'ni seçin.
2. Kayıt defterinizi seçin. Ad **mlcr** ile başlamalıdır ve Modül Yönetimini kurmak için kullandığınız kaynak grubuna, konuma ve aboneliğe aittir.
3. **Erişim anahtarları**'nı seçin
4. **Oturum açma sunucusu**'nu, **Kullanıcı adı**'nı ve **Parola**'yı kopyalayın.  Bunlar Edge cihazlarınızdan kayıt defterine erişmek için gerekir.
5. **Depolar**'ı seçin
6. **machinelearningmodule**'u seçin
7. Artık kapsayıcının tam görüntüsünün yoluna sahipsiniz. Bu görüntü yolunu sonraki bölüm için not edin. Biçimi şöyle olmalıdır:  **<kayit_defteri_adi>.azureacr.io/machinelearningmodule:1**

## <a name="add-registry-credentials-to-your-edge-device"></a>Kayıt defteri kimlik bilgilerini Edge cihazınıza ekleme

Kayıt defterinizin kimlik bilgilerini, Edge cihazınızı çalıştırdığınız bilgisayarın Edge çalışma zamanına ekleyin. Bu komut, çalışma zamanına kapsayıcıyı çekmek için erişim verir.

Linux:
   ```cmd
   sudo iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password>
   ```

Windows:
   ```cmd
   iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password>
   ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. [Azure portalda](https://portal.azure.com), IoT hub'ınıza gidin.
1. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
1. **Modül ayarla**’yı seçin.
1. tempSensor modülünü daha önce IoT Edge cihazına dağıttıysanız, otomatik olarak dolabilir. Modül listenizde değilse ekleyin.
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `tempSensor` girin.
    3. **Görüntü URI'si** alanına `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` girin.
    4. **Kaydet**’i seçin.
1. Oluşturduğunuz makine öğrenimi modülünü ekleyin.
    1. **IoT Edge Modülü Ekle**'yi seçin.
    1. **Ad** alanına `machinelearningmodule` girin
    1. **Görüntü** alanına görüntünüzün adresini girin; örneğin `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. **Kaydet**’i seçin.
1. **Modül Ekle** adımına dönüp **İleri**’yi seçin.
1. **Rota Belirtme** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. İlk yol, tüm Azure Machine Learning modüllerinin kullandığı uç nokta olan "amlInput" uç noktası aracılığıyla sıcaklık algılayıcısından makine öğrenimi modülüne ileti taşır. İkinci yol, makine öğrenimi modülünden IoT Hub'ına ileti taşır. Bu yolda, ''amlOutput'' tüm Azure Machine Learning modüllerinin veri çıkışı için kullandığı uç noktadır ve ''$upstream'' IoT Hub'ını gösterir.

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
1. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin.  Yeni **machinelearningmodule** modülünün **tempSensor** modülü ve IoT Edge çalışma zamanı modülleri ile birlikte çalıştığını görmelisiniz.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

IoT Edge cihazınızın [IoT Hub gezginini](https://github.com/azure/iothub-explorer) veya Visual Studio Code için Azure IoT Toolkit uzantısını kullanarak gönderdiği cihazdan buluta iletilerini görüntüleyebilirsiniz.

1. Visual Studio Code'da **IoT Hub Cihazları**'nı seçin.
2. Menüden **...** öğesini, sonra **IoT Hub Bağlantı Dizesini Ayarla**'yı seçin.

   ![IoT Hub Cihazları diğer menüsü](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Sayfanın üstünde açılan metin kutusuna IoT Hub'ınız için iothubowner bağlantı dizesini girin. IoT Edge cihazınız IoT Hub Cihazları listesinde görünmelidir.
4. Yeniden **...** öğesini, sonra **D2C iletisini izlemeye başla**'yı seçin.
5. tempSensor her beş saniyede bir gelen iletileri gözlemleyin. İleti gövdesinde, machinelearningmodule'un true veya false değeri verdiği **anomaly** adlı bir özellik bulunur. Model başarıyla çalıştıysa, **AzureMLResponse** özelliği "OK" değerini içerir.

   ![İleti gövdesindeki Azure ML yanıtı](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning ile çalışan bir IoT Edge modülü dağıttınız. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için diğer öğreticilere geçebilirsiniz.

> [!div class="nextstepaction"]
> [Bir Azure Function'ı modül olarak dağıtma](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
