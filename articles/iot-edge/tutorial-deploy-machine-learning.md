---
title: Azure IoT Edge ile Azure Machine Learning dağıtma | Microsoft Belgeleri
description: Azure Machine Learning'i modül olarak bir Edge cihazına dağıtma
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/25/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 62ca816f7bdc183727eb22806ba9e733c8b97c44
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173514"
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Azure Machine Learning'i bir IoT Edge modülü olarak dağıtma - önizleme

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

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide derleyeceğiniz Machine Learning modülünü test etmek için bir IoT Edge cihazına sahip olmanız gerekir. [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) hızlı başlangıcında yapılandırdığınız cihazı kullanabilirsiniz. 

Azure Machine Learning modülü ARM işlemcilerini desteklemez.

Geliştirme makinenizde aşağıdaki önkoşulların karşılandığından emin olun: 
* Bir Azure Machine Learning hesabı. [Azure Machine Learning hesapları oluşturma ve Azure Machine Learning Workbench yükleme](../machine-learning/service/quickstart-installation.md#create-azure-machine-learning-services-accounts) yönergelerini izleyin. Bu öğretici için çalışma ekranı uygulamasını yüklemeniz gerekmez. 
* Makinenizdeki Azure ML için Model Yönetimi. Ortamınızı kurmak ve bir hesap oluşturmak için, [Model yönetimi kurulumu](../machine-learning/desktop-workbench/deployment-setup-configuration.md)'ndaki yönergeleri izleyin. Dağıtım kurulumu sırasında mümkünse küme yerine yerel adımların seçilmesi önerilir.

### <a name="disable-process-identification"></a>İşlem tanımlamasını devre dışı bırakma

>[!NOTE]
>
> Azure Machine Learning, önizleme sürümünde IoT Edge ile varsayılan olarak etkinleştirilen işlem tanımlaması güvenlik özelliğini desteklemez. 
> Aşağıdaki adımları kullanarak bu özelliği devre dışı bırakabilirsiniz. Ancak bu süreç üretimde kullanıma uygun değildir. Bu işlemleri Windows Edge çalışma zamanı kurulum adımlarında zaten gerçekleştireceğinizden bunları yalnızca Linux'ta gerçekleştirmeniz gerekir.

IoT Edge cihazınızda işlem tanımlamasını devre dışı bırakmak için IoT Edge daemon yapılandırmasının **connect** bölümünde yer alan **workload_uri** ve **management_uri** parametreleri için IP adresini ve bağlantı noktasını belirtmeniz gerekir.

Önce IP adresini alın. Komut satırına `ifconfig` yazın ve **docker0** arabiriminin IP adresini kopyalayın.

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

Kapsayıcınızın görüntüsünün başarıyla oluşturulduğundan ve makine öğrenimi ortamınızla ilişkili Azure Kapsayıcısı kayıt defterine depolandığından emin olun.

1. [Azure portalda](https://portal.azure.com), **Tüm Hizmetler**'e gidin ve **Kapsayıcı kayıt defterleri**'ni seçin.
2. Kayıt defterinizi seçin. Ad **mlcr** ile başlamalıdır ve Modül Yönetimini kurmak için kullandığınız kaynak grubuna, konuma ve aboneliğe aittir.
3. **Erişim anahtarları**'nı seçin
4. **Oturum açma sunucusu**'nu, **Kullanıcı adı**'nı ve **Parola**'yı kopyalayın.  Bunlar Edge cihazlarınızdan kayıt defterine erişmek için gerekir.
5. **Depolar**'ı seçin
6. **machinelearningmodule**'u seçin
7. Artık kapsayıcının tam görüntüsünün yoluna sahipsiniz. Bu görüntü yolunu sonraki bölüm için not edin. Biçimi şöyle olmalıdır:  **<kayit_defteri_adi>.azureacr.io/machinelearningmodule:1**

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

IoT Hub'ınızın [IoT Hub gezginini](https://github.com/azure/iothub-explorer) veya [Visual Studio Code için Azure IoT Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) kullanarak aldığı cihazdan buluta iletilerini görüntüleyebilirsiniz.

Aşağıdaki adımlar, IoT hub'ınıza ulaşan cihazdan buluta iletileri izlemek için yapmanız gereken Visual Studio Code ayarlarını göstermektedir. 

1. Visual Studio Code'da **IoT Hub Cihazları**'nı seçin.

2. Menüden **...** öğesini, sonra **IoT Hub Bağlantı Dizesini Ayarla**'yı seçin.

   ![IoT Hub Cihazları diğer menüsü](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Sayfanın üstünde açılan metin kutusuna IoT Hub'ınız için iothubowner bağlantı dizesini girin. IoT Edge cihazınız IoT Hub Cihazları listesinde görünmelidir.

4. Yeniden **...** öğesini, sonra **D2C iletisini izlemeye başla**'yı seçin.

5. tempSensor her beş saniyede bir gelen iletileri gözlemleyin. İleti gövdesinde, machinelearningmodule'un true veya false değeri verdiği **anomaly** adlı bir özellik bulunur. Model başarıyla çalıştıysa, **AzureMLResponse** özelliği "OK" değerini içerir.

   ![İleti gövdesindeki Azure ML yanıtı](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Bir sonraki önerilen makaleye geçecekseniz oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Azure kaynaklarını ve kaynak grubunu silme işlemi geri alınamaz. Silindikten sonra kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Yalnızca IoT Hub'ı silmek için hub adını ve kaynak grubu adını kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Kaynak grubunun tamamını adıyla silmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

2. **Ada göre filtrele...** metin kutusuna IoT Hub'ınızın bulunduğu kaynak grubunun adını girin. 

3. Sonuç listesinde kaynak grubunuzun sağ tarafında **...** ve sonra **Kaynak grubunu sil**'e tıklayın.

<!--
   ![Delete](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)
-->
4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını tekrar yazın ve **Sil**'e tıklayın. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning ile çalışan bir IoT Edge modülü dağıttınız. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için diğer öğreticilere geçebilirsiniz.

> [!div class="nextstepaction"]
> [C# kodunu kullanarak sensör verilerini filtreleme](tutorial-csharp-module.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
