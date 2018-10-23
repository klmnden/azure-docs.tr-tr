---
title: Azure IoT Edge ile Azure Machine Learning dağıtma | Microsoft Belgeleri
description: Bu öğreticide Azure Machine Learning'i modül olarak bir Edge cihazına dağıtacaksınız.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0af63c7beef0f5268ba078d517556fc64ac7bf87
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116362"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Öğretici: Azure Machine Learning'i bir IoT Edge modülü olarak dağıtma (önizleme)

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide simülasyon makinesi sıcaklık verilerini temel alarak bir cihazın arızalanacağı zamanı tahmin eden bir Azure Machine Learning modülünü dağıtma adımları açıklanmaktadır. IoT Edge üzerinde Azure ML hakkında daha fazla bilgi için bkz. [Azure Machine Learning belgeleri](../machine-learning/desktop-workbench/use-azure-iot-edge-ai-toolkit.md).

Bu öğreticide oluşturduğunuz Azure Machine Learning modülü, cihazınızın ürettiği ortam verilerini okur ve iletileri normal veya anormal olarak etiketler.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Machine Learning modülü oluşturma
> * Azure kapsayıcı kayıt defterine bir modülü kapsayıcısı gönderme
> * IoT Edge cihazınıza bir Azure Machine Learning modülü dağıtma
> * Oluşturulan verileri görüntüleme

>[!NOTE]
>Azure IoT Edge üzerindeki Azure Machine Learning modülleri genel önizleme sürümündedir. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.
* Azure Machine Learning modülü ARM işlemcilerini desteklemez.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 
* Azure Machine Learning çalışma alanı. Yeni bir tane oluşturmak için [IoT Edge'de model dağıtmaya hazırlanma](../machine-learning/service/how-to-deploy-to-iot.md) bölümündeki yönergeleri izleyin.


### <a name="disable-process-identification"></a>İşlem tanımlamasını devre dışı bırakma

>[!NOTE]
>
> Azure Machine Learning, önizleme sürümünde IoT Edge ile varsayılan olarak etkinleştirilen işlem tanımlaması güvenlik özelliğini desteklemez. 
> Aşağıdaki adımları kullanarak bu özelliği devre dışı bırakabilirsiniz. Ancak bu süreç üretimde kullanıma uygun değildir. Bu işlemleri Windows Edge çalışma zamanı yüklemesinde zaten gerçekleştireceğinizden bunları yalnızca Linux'ta gerçekleştirmeniz gerekir.

IoT Edge cihazınızda işlem tanımlamasını devre dışı bırakmak için IoT Edge daemon yapılandırmasının **connect** bölümünde yer alan **workload_uri** ve **management_uri** parametreleri için IP adresini ve bağlantı noktasını belirtmeniz gerekir.

Önce IP adresini alın. Komut satırına `ipconfig` yazın ve **docker0** arabiriminin IP adresini kopyalayın.

IoT Edge daemon yapılandırma dosyasını düzenleyin:

```cmd/sh
sudo nano /etc/iotedge/config.yaml
```

Yapılandırmanın **connect** bölümünü IP adresinizle güncelleştirin. Örnek:
```yaml
connect:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Aynı adresleri yapılandırmanın **listen** bölümüne girin. Örnek:

```yaml
listen:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

IOTEDGE_HOST ortam değişkenini management_uri adresiyle oluşturun (Kalıcı olarak ayarlamak için `/etc/environment` yoluna ekleyin). Örneğin:

```cmd/sh
export IOTEDGE_HOST="http://172.17.0.1:15580"
```


## <a name="create-the-azure-ml-container"></a>Azure ML kapsayıcısını oluşturma
Bu bölümde eğitilen modelin dosyalarını indirecek ve bunları Azure ML kapsayıcısına dönüştüreceksiniz.

Makine öğrenmesi modelinizle bir Docker kapsayıcısı oluşturmak için [IoT Edge'de model dağıtmaya hazırlanma](../machine-learning/service/how-to-deploy-to-iot.md) belgesindeki yönergeleri izleyin.  Docker görüntüsü için gerekli tüm bileşenler [Azure IoT Edge için AI Toolkit deposunda](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial) mevcuttur.

### <a name="view-the-container-repository"></a>Kapsayıcı deposunu görüntüleme

Kapsayıcınızın görüntüsünün başarıyla oluşturulduğundan ve makine öğrenimi ortamınızla ilişkili Azure Kapsayıcısı kayıt defterine depolandığından emin olun.

1. [Azure portalda](https://portal.azure.com), **Tüm Hizmetler**'e gidin ve **Kapsayıcı kayıt defterleri**'ni seçin.
2. Kayıt defterinizi seçin. Ad **mlcr** ile başlamalıdır ve Modül Yönetimini kurmak için kullandığınız kaynak grubuna, konuma ve aboneliğe aittir.
3. **Erişim anahtarları**'nı seçin
4. **Oturum açma sunucusu**'nu, **Kullanıcı adı**'nı ve **Parola**'yı kopyalayın.  Bunlar Edge cihazlarınızdan kayıt defterine erişmek için gerekir.
5. **Depolar**'ı seçin
6. **machinelearningmodule**'u seçin
7. Artık kapsayıcının tam görüntüsünün yoluna sahipsiniz. Bu görüntü yolunu sonraki bölüm için not edin. Biçimi şöyle olmalıdır: **<registry_name>.azurecr.io/machinelearningmodule:1**

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

1. [Azure portalda](https://portal.azure.com), IoT hub'ınıza gidin.

1. **IoT Edge** sayfasına gidip IoT Edge cihazınızı seçin.

1. **Modül ayarla**’yı seçin.

1. **Kayıt Defteri Ayarları** bölümünde Azure kapsayıcı kayıt defterinden kopyaladığınız kimlik bilgilerini ekleyin. 

   ![Kayıt defteri kimlik bilgilerini ekleme](./media/tutorial-deploy-machine-learning/registry-settings.png)

1. tempSensor modülünü daha önce IoT Edge cihazına dağıttıysanız, otomatik olarak dolabilir. Modül listenizde değilse ekleyin.

    1. **Ekle**'ye tıklayıp **IoT Edge Modülü**'nü seçin.
    2. **Ad** alanına `tempSensor` girin.
    3. **Görüntü URI'si** alanına `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0` girin.
    4. **Kaydet**’i seçin.

1. Oluşturduğunuz makine öğrenimi modülünü ekleyin.

    1. **Ekle**'ye tıklayıp **IoT Edge Modülü**'nü seçin.
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

1. **Dağıtımı Gözden Geçirin** adımında **Gönder**'i seçin.

1. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin.  Yeni **machinelearningmodule** modülünün **tempSensor** modülü ve IoT Edge çalışma zamanı modülleri ile birlikte çalıştığını görmelisiniz.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Her bir IoT Edge modülü tarafından oluşturulan ve IoT hub'ınıza gönderilen iletileri görüntüleyebilirsiniz.

### <a name="view-data-on-your-iot-edge-device"></a>IoT Edge cihazınızdaki verileri görüntüleme

IoT Edge cihazınızda her bir modülden gönderilen iletileri görüntüleyebilirsiniz. 

Bu komutları bir Linux cihazında gerçekleştirirseniz yükseltilmiş izinler için `sudo` kullanmanız gerekebilir.

1. IoT Edge cihazınızda tüm modülleri görüntüleyin.

   ```cmd/sh
   iotedge list
   ```

2. Belirli bir cihazdan gönderilen iletileri görüntüleyin. Önceki komutun çıktısında yer alan modül adını kullanın.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>IoT hub'ınıza ulaşan verileri görüntüleme

IoT Hub'ınızın [Visual Studio Code için Azure IoT Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) kullanarak aldığı cihazdan buluta iletilerini görüntüleyebilirsiniz.

Aşağıdaki adımlar, IoT hub'ınıza ulaşan cihazdan buluta iletileri izlemek için yapmanız gereken Visual Studio Code ayarlarını göstermektedir. 

1. Visual Studio Code'da **IoT Hub Cihazları**'nı seçin.

2. Menüden **...** öğesini, sonra **IoT Hub Bağlantı Dizesini Ayarla**'yı seçin.

   ![IoT Hub Cihazları diğer menüsü](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Sayfanın üstünde açılan metin kutusuna IoT Hub'ınız için iothubowner bağlantı dizesini girin. IoT Edge cihazınız IoT Hub Cihazları listesinde görünmelidir.

4. Yeniden **...** öğesini, sonra **D2C iletisini izlemeye başla**'yı seçin.

5. tempSensor her beş saniyede bir gelen iletileri gözlemleyin. İleti gövdesinde, machinelearningmodule'un true veya false değeri verdiği **anomaly** adlı bir özellik bulunur. Model başarıyla çalıştıysa, **AzureMLResponse** özelliği "OK" değerini içerir.

   ![İleti gövdesindeki Azure ML yanıtı](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning ile çalışan bir IoT Edge modülü dağıttınız. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için diğer öğreticilere geçebilirsiniz.

> [!div class="nextstepaction"]
> [C# kodunu kullanarak sensör verilerini filtreleme](tutorial-csharp-module.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
