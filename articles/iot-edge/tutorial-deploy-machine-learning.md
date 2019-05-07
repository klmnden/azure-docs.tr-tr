---
title: Azure Machine Learning - bir cihaz için Azure IOT Edge dağıtma | Microsoft Docs
description: Bu öğreticide, bir Azure Machine Learning modeli oluşturmak ve ardından bir modül olarak edge cihazına dağıtma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 6f85b0088fac97f4b9f2dd2835e3052cb598a987
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65142755"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Öğretici: Azure Machine Learning (Önizleme) IOT Edge modülü dağıtma

Azure not defterleri, bir makine öğrenme modülü geliştirme ve Azure IOT Edge çalıştıran bir Linux cihazına dağıtmak için kullanın. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide simülasyon makinesi sıcaklık verilerini temel alarak bir cihazın arızalanacağı zamanı tahmin eden bir Azure Machine Learning modülünü dağıtma adımları açıklanmaktadır. IOT Edge üzerinde Azure Machine Learning hizmeti hakkında daha fazla bilgi için bkz. [Azure Machine Learning belgeleri](../machine-learning/service/how-to-deploy-to-iot.md).

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


## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* Bir Azure sanal makinesi için hızlı başlangıç adımları izleyerek bir IOT Edge cihazı kullanabilirsiniz [Linux](quickstart-linux.md).
* Azure Machine Learning modülü, Windows kapsayıcıları desteklemiyor.
* Azure Machine Learning modülü, ARM işlemcileri desteklemiyor.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* Azure Machine Learning çalışma alanı. Bölümündeki yönergeleri [Azure Machine Learning'i kullanmaya başlamak için Azure portal'ı kullanmanızı](../machine-learning/service/quickstart-get-started.md) oluşturun ve bunu kullanmayı öğrenin.
   * Çalışma alanı adı, kaynak grubu ve abonelik kimliğini not edin Bu değerleri çalışma alanına genel bakış Azure portalında tüm kullanılabilir. Bir Azure not defteri çalışma kaynaklarınıza bağlanmak için öğreticinin ilerleyen bölümlerinde bu değerleri kullanacaksınız. 


## <a name="create-and-deploy-azure-machine-learning-module"></a>Oluşturma ve Azure Machine Learning modülü dağıtma

Bu bölümde, eğitilen makine öğrenme modeli dosyalarını dönüştürmek ve bir Azure Machine Learning kapsayıcı hizmeti. Docker görüntüsü için gerekli tüm bileşenler [Azure IoT Edge için AI Toolkit deposunda](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial) mevcuttur. Microsoft Azure kapsayıcı oluşturma ve Azure Container Registry'ye gönderme için Not Defterleri ile ilgili depoya karşıya yüklemek için aşağıdaki adımları izleyin.


1. Azure not defterleri projelerinize gidin. Azure Machine Learning hizmeti çalışma alanınızın buradan ulaşabilirsiniz [Azure portalında](https://portal.azure.com) veya oturum açarak [Microsoft Azure not defterleri](https://notebooks.azure.com/home/projects) Azure hesabınızla.

2. Seçin **GitHub deposunu karşıya**.

3. Aşağıdaki GitHub deposu adı sağlayın: `Azure/ai-toolkit-iot-edge`. Onay kutusunu temizleyin **genel** projenize özel kalmasını istiyorsanız kutusu. Seçin **alma**. 

4. İçeri aktarma işlemi bittikten sonra yeni gidin **Araç Seti IOT edge yapay zeka** açın ve proje **IOT Edge anomali algılama öğretici** klasör. 

5. Projenizi çalıştığını doğrulayın. Aksi takdirde, seçin **ücretsiz işlem çalıştırma**.

   ![Ücretsiz işlem üzerinde çalıştırın](./media/tutorial-deploy-machine-learning/run-on-free-compute.png)

6. Açık **aml_config/config.json** dosya.

7. Aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanı adınızı Azure abonelik Kimliğinizi değerleri, bir kaynak grubu eklemek için yapılandırma dosyasını düzenleyin. Tüm bu değerleri alabilirsiniz **genel bakış** bölümünde çalışma alanınızı azure'da. 

8. Yapılandırma dosyasını kaydedin.

9. Açık **anomali algılama tutorial.ipynb 00** dosya.

10. Sorulduğunda, **Python 3.6** çekirdek seçip **ayarlamak çekirdek**.

11. İlk hücreye not defterine açıklama yönergelere göre düzenleyin. Aynı kaynak grubunu, abonelik kimliği ve yapılandırma dosyasına eklemiş olduğunuz çalışma alanı adı kullanın.

12. Hücreleri bunları seçerek ve not defteri çalıştırmak **çalıştırma** ya basarak `Shift + Enter`.

    >[!TIP]
    >Bazı kullanıcılar henüz bir IOT Hub gibi olmayabilir veya kaynakları'na oluşturduğundan anomali algılama öğretici not defterinde hücrelerin isteğe bağlı, bazılarıdır. Mevcut kaynak bilgilerinizi ilk hücreye yerleştirirseniz, Azure yinelenen kaynaklar oluşturmaz, çünkü yeni kaynaklar oluşturan hücreleri çalıştırırsanız, hataları alırsınız. Bu bir sakınca yoktur ve hataları yoksayma ya da tamamen isteğe bağlı Bu bölüm atlayın. 

Not Defteri yer alan tüm adımları tamamlayarak bir Docker kapsayıcı görüntüsü yerleşik anomali algılama modeli eğitilir ve bu görüntüyü Azure Container Registry'ye gönderildi. Daha sonra modeli test ve son olarak IOT Edge cihazınıza dağıttınız. 

## <a name="view-container-repository"></a>Bir kapsayıcı deposuna görüntüle

Kapsayıcı görüntünüzü başarıyla oluşturulduğundan ve makine öğrenme ortamınız ile ilişkili Azure container Registry'de depolanan olduğunu kontrol edin. Kapsayıcı görüntüsünü ve IOT Edge cihazınıza kayıt defteri kimlik bilgilerini otomatik olarak kullandığınız önceki bölümde not defteri sağlanan, ancak kendiniz bilgileri bulabilirsiniz, böylece nerede depolandıkları bilmeniz gereken daha sonra. 

1. İçinde [Azure portalında](https://portal.azure.com), Machine Learning hizmeti çalışma alanınıza gidin. 

2. **Genel bakış** bölümü iyi ilişkili kaynakları olarak çalışma alanı ayrıntılarını listeler. Seçin **kayıt defteri** rastgele sayı yazın, çalışma alanı adı olması gereken değer. 

3. Kapsayıcı kayıt defterini seçin **depoları**. Adlı bir depo görmelisiniz **tempanomalydetection** önceki bölümde çalıştırdığınız not defterini tarafından oluşturuldu. 

4. Seçin **tempanomalydetection**. Depo bir etiket olduğunu görmeniz gerekir: **1**. 

   Kayıt defteri adı, depo adı ve etiketi bildiğiniz, kapsayıcının tam görüntü yolu bildirin. Görüntü yollarını göründüğü  **\<registry_name\>.azurecr.io/tempanomalydetection:1**. Görüntü yolu, IOT Edge cihazları bu kapsayıcıyı dağıtmak için kullanabilirsiniz. 

5. Kapsayıcı kayıt defterini seçin **erişim anahtarları**. Erişim kimlik bilgileri de dahil olmak üzere, bir dizi görürsünüz **oturum açma sunucusu** ve **kullanıcıadı**, ve **parola** bir yönetici kullanıcı için.

   Bu kimlik bilgileri, IOT Edge cihaz erişimi için kapsayıcı görüntüleri çekmeniz kayıt defterinden vermek için dağıtım bildirimi içinde eklenebilir. 

Artık Machine Learning kapsayıcı görüntüsünün depolandığı biliyorsunuz. Sonraki bölümde, IOT Edge Cihazınızda bir modül olarak çalışan kapsayıcıyı görmek için adım adım yönergeler sağlanmıştır. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Her bir IoT Edge modülü tarafından oluşturulan ve IoT hub'ınıza gönderilen iletileri görüntüleyebilirsiniz.

### <a name="view-data-on-your-iot-edge-device"></a>IoT Edge cihazınızdaki verileri görüntüleme

IoT Edge cihazınızda her bir modülden gönderilen iletileri görüntüleyebilirsiniz.

Kullanmanız gerekebilir `sudo` çalıştırmak yükseltilmiş izinler için `iotedge` komutları. Oturumunuzu kapatıp cihazınıza otomatik olarak tekrar açarak izinlerinizi güncelleştirir.

1. IoT Edge cihazınızda tüm modülleri görüntüleyin.

   ```cmd/sh
   iotedge list
   ```

2. Belirli bir cihazdan gönderilen iletileri görüntüleyin. Önceki komutun çıktısında yer alan modül adını kullanın.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>IoT hub'ınıza ulaşan verileri görüntüleme

CİHAZDAN buluta iletileri kullanarak IOT hub'ınızın aldığı görüntüleyebileceğiniz [Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eski adıyla Azure IOT Toolkit uzantısını).

Aşağıdaki adımlar, IoT hub'ınıza ulaşan cihazdan buluta iletileri izlemek için yapmanız gereken Visual Studio Code ayarlarını göstermektedir.

1. Visual Studio Code'da **IoT Hub Cihazları**'nı seçin.

2. Menüden **...** öğesini, sonra **IoT Hub Bağlantı Dizesini Ayarla**'yı seçin.

   ![IOT Hub bağlantı dizesine ayarlayın](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Sayfanın üstünde açılan metin kutusuna IoT Hub'ınız için iothubowner bağlantı dizesini girin. IoT Edge cihazınız IoT Hub Cihazları listesinde görünmelidir.

4. Yeniden **...** öğesini, sonra **D2C iletisini izlemeye başla**'yı seçin.

5. tempSensor her beş saniyede bir gelen iletileri gözlemleyin. İleti gövdesi adlı bir özellik içeren **anomali**, doğru veya yanlış değeriyle machinelearningmodule sağlar. Model başarıyla çalıştıysa, **AzureMLResponse** özelliği "OK" değerini içerir.

   ![Azure Machine Learning hizmeti yanıt iletisi gövdesinde](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning ile çalışan bir IoT Edge modülü dağıttınız. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için diğer öğreticilere geçebilirsiniz.

> [!div class="nextstepaction"]
> [Özel Görüntü İşleme hizmetiyle görüntüleri sınıflandırma](tutorial-deploy-custom-vision.md)

