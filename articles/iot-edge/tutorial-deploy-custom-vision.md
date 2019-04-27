---
title: Öğretici - Azure IOT Edge cihaza Custom Vision sınıflandırıcı dağıtma | Microsoft Docs
description: Bu öğreticide, bir görüntü işleme modelinizle özel görüntü işleme ve IOT Edge kullanarak bir kapsayıcı çalışmasını sağlamak öğrenin.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 7a5a92635114be87e59fe8f779c36d4c401a1427
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612844"
---
# <a name="tutorial-perform-image-classification-at-the-edge-with-custom-vision-service"></a>Öğretici: Uç cihazlarında özel görüntü işleme hizmeti ile görüntü sınıflandırma gerçekleştirin

Azure IoT Edge, iş yüklerini buluttan uca taşıyarak IoT çözümünüzü daha verimli hale getirmenizi sağlayabilir. Bu özellik, özel görüntü işleme modelleri gibi çok miktarda veri işleyen hizmetler için uygundur. [Özel Görüntü İşleme Hizmeti](../cognitive-services/custom-vision-service/home.md), özel görüntü sınıflandırıcıları derlemenizi ve bunları cihazlara kapsayıcı olarak dağıtmanızı sağlar. Bu iki hizmet birlikte tüm verileri aktarmak zorunda kalmadan görüntülerden veya video akışlarından içgörü elde etmenizi sağlar. Özel Görüntü İşleme Hizmeti, içgörü oluşturmak için bir görüntüyü eğitilmiş modelle karşılaştıran bir sınıflandırıcı sunar. 

Örneğin bir IoT Edge cihazı üzerinde çalışan Özel Görüntü İşleme Hizmeti bir otoyoldaki trafik yoğunluğunun beklenenden daha yüksek veya düşük olduğunu veya bir otoparkta yeterli yer bulunup bulunmadığını belirleyebilir. Bu içgörüler eylem gerçekleştirmek üzere başka bir hizmetle paylaşılabilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
>
> * Özel Görüntü İşleme Hizmeti ile bir görüntü sınıflandırıcı derleme.
> * Cihazınızdaki Özel Görüntü İşleme Hizmeti web sunucusunu sorgulayan bir IoT Edge modülü dağıtma.
> * Görüntü sınıflandırıcı sonuçlarını IoT Hub'a gönderme.

<center>

![Diyagram - öğretici mimarisi, aşama ve sınıflandırıcı dağıtma](./media/tutorial-deploy-custom-vision/custom-vision-architecture.png)
</center>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* [Linux için hızlı başlangıç](quickstart-linux.md) adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.
* Şu an için Özel Görüntü İşleme Hizmeti Linux kapsayıcısı yalnızca x64 mimarilerinde kullanılabilir. 

Bulut kaynakları:

* Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 
* Kapsayıcı kayıt defteri. Bu öğreticide [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) kullanılır. 
* Kapsayıcı kayıt defterinizin [yönetici hesabının](../container-registry/container-registry-authentication.md#admin-account) kimlik bilgileri.

Geliştirme kaynakları:

* [Python](https://www.python.org/downloads/)
* [Git](https://git-scm.com/downloads)
* [Visual Studio Code](https://code.visualstudio.com/)
* Visual Studio Code için [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) uzantısı
* Visual Studio Code için [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) uzantısı
* [Docker CE](https://docs.docker.com/install/)

## <a name="build-an-image-classifier-with-custom-vision"></a>Özel Görüntü İşleme Hizmeti ile bir görüntü sınıflandırıcı derleme

Görüntü sınıflandırıcı derlemek için bir Özel Görüntü İşleme Hizmeti projesi oluşturmanız ve eğitim amaçlı görüntü sağlamanız gerekir. Bu bölümde gerçekleştireceğiniz adımlar hakkında daha fazla bilgi için bkz. [Özel Görüntü İşleme Hizmeti ile sınıflandırıcı derleme](../cognitive-services/custom-vision-service/getting-started-build-a-classifier.md).

Görüntü sınıflandırıcıyı derleyip eğittikten sonra Docker kapsayıcısı olarak dışarı aktarabilir ve bir IoT Edge cihazına dağıtabilirsiniz. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Web tarayıcınızda [Özel Görüntü İşleme Hizmeti web sayfasına](https://customvision.ai/) gidin.

2. **Sign in** (Oturum aç) öğesini seçin ve Azure kaynaklarına erişmek için kullandığınız hesapla oturum açın. 

3. **New project** (Yeni proje) öğesini seçin.

4. Aşağıdaki değerlerle bir proje oluşturun:

   | Alan | Değer |
   | ----- | ----- |
   | Ad | Projeniz için bir ad belirleyin, örneğin: **EdgeTreeClassifier**. |
   | Açıklama | İsteğe bağlı proje açıklaması. |
   | Kaynak Grubu | Varsayılan **Limited trial** (Sınırlı deneme sürümü) ayarını kabul edin. |
   | Proje Türleri | **Classification** (Sınıflandırma) |
   | Sınıflandırma Türleri | **Multiclass (single tag per image)** (Çok sınıflı (görüntü başına tek bir etiket)) | 
   | Etki Alanları | **General (compact)** (Genel (kompakt)) |

5. **Create project** (Proje oluştur) öğesini seçin.

### <a name="upload-images-and-train-your-classifier"></a>Görüntüleri karşıya yükleme ve sınıflandırıcınızı eğitme

Görüntü sınıflandırıcı oluşturmak için eğitim görüntülerine ve test görüntülerine ihtiyacınız vardır. 

1. [Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows) deposundaki örnek görüntüleri yerel geliştirme makinenize kopyalayın veya indirin. 

   ```cmd/sh
   git clone https://github.com/Microsoft/Cognitive-CustomVision-Windows.git
   ```

2. Özel Görüntü İşleme Hizmeti projenize dönün ve **Add images** (Görüntü ekle) öğesini seçin. 

3. Kopyaladığınız yerel git deposuna gidin ve ilk görüntü klasörünü açın: **Cognitive-CustomVision-Windows / Samples / Images / Hemlock**. Klasördeki 10 görüntüyü ve ardından **Open** (Aç) öğesini seçin. 

4. Bu görüntü grubuna **hemlock** etiketini ekleyin ve **Enter** tuşuna basarak etiketi uygulayın. 

5. **Upload 10 files** (10 dosyayı karşıya yükle) öğesini seçin. 

   ![Köknar etiketlenmiş dosyalar için özel görüntü karşıya yükleme](./media/tutorial-deploy-custom-vision/upload-hemlock.png)

6. Görüntüler başarıyla karşıya yüklendikten sonra **Done** (Bitti) öğesini seçin.

7. Yeniden **Add images** (Görüntü ekle) öğesini seçin.

8. İkinci görüntü klasörüne göz atın: **Cognitive-CustomVision-Windows / Samples / Images / Japanese Cherry**. Klasördeki 10 görüntüyü ve ardından **Open** (Aç) öğesini seçin. 

9. Bu görüntü grubuna **japanese cherry** etiketini ekleyin ve **Enter** tuşuna basarak etiketi uygulayın. 

10. **Upload 10 files** (10 dosyayı karşıya yükle) öğesini seçin. Görüntüler başarıyla karşıya yüklendikten sonra **Done** (Bitti) öğesini seçin. 

11. İki görüntü kümesini de etiketleyip yükledikten sonra sınıflandırıcıyı eğitmek için **Train** (Eğit) öğesini seçin. 

### <a name="export-your-classifier"></a>Sınıflandırıcınızı dışarı aktarma

1. Sınıflandırıcınızı eğittikten sonra sınıflandırıcının Performance (Performans) sayfasında **Export** (Dışarı aktar) öğesini seçin. 

   ![Eğitilen bir görüntü sınıflandırıcınızı dışarı aktarma](./media/tutorial-deploy-custom-vision/export.png)

2. Platform için **DockerFile** girişini seçin. 

3. Sürüm olarak **Linux** seçin.  

4. **Export** (Dışarı aktar) öğesini seçin. 

   ![Linux kapsayıcılarla DockerFile olarak dışarı aktarma](./media/tutorial-deploy-custom-vision/export-2.png)

5. Dışarı aktarma işlemi tamamlandıktan sonra **Download** (İndir) öğesini seçin ve .zip paketini bilgisayarınıza kaydedin. Paketteki tüm dosyaları ayıklayın. Bu dosyaları görüntü sınıflandırma sunucusunu içeren bir IoT Edge modülü oluşturmak için kullanacaksınız. 

Özel Görüntü İşleme Hizmeti projenizi oluşturma ve eğitme adımlarını tamamladınız. Dışarı aktarılan dosyaları bir sonraki bölümde kullanacaksınız ancak Özel Görüntü İşleme Hizmeti web sayfasıyla işiniz bitti. 

## <a name="create-an-iot-edge-solution"></a>IoT Edge çözümü oluşturma

Artık geliştirme makinenizde görüntü sınıflandırıcınızın kapsayıcı sürümü için gerekli dosyalara sahipsiniz. Bu bölümde görüntü sınıflandırıcı kapsayıcısını IoT Edge modülü olarak çalıştıracak şekilde yapılandıracaksınız. Ayrıca görüntü sınıflandırıcı ile birlikte dağıtılacak ikinci bir modül de oluşturacaksınız. İkinci modül istekleri sınıflandırıcıya, sonuçları da ileti olarak IoT Hub'a gönderir. 

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Çözüm, tek bir IoT Edge dağıtımı için birden fazla modül geliştirmek ve düzenlemek için kullanabileceğiniz mantıksal bir yoldur. Bir çözümde bir veya daha fazla modülün kodunun yanı sıra bunların bir IoT Edge cihazında nasıl yapılandırılacağını bildiren dağıtım bildirimi bulunur. 

1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Terminal**'i seçin.

2. Tümleşik terminalde aşağıdaki komutu kullanarak VS Code'da IoT Edge python modülü şablonunu oluşturmak için yararlanacağınız **cookiecutter** paketini yükleyin (veya güncelleştirin):

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```
   >[!Note]
   >Komut isteminden çağrılabilmesi için cookiecutter paketinin yükleneceği dizinin ortamınızın `Path` yolunda olduğundan emin olun.

3. VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 

4. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için **CustomVisionSolution** gibi açıklayıcı bir ad girin veya varsayılan değeri kabul edin. |
   | Modül şablonunu seçin | **Python Modülü**'nü seçin. |
   | Modül adı sağlayın | Modülünüze **classifier** adını verin.<br><br>Modül adının küçük harfli olması önemlidir. IoT Edge, modüllere başvururken büyük/küçük harfe duyarlıdır ve bu çözümde tüm istekleri küçük harf ile biçimlendiren bir kitaplık kullanılmaktadır. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. Dizenin son hali \<registry name\>.azurecr.io/classifier ifadesine benzer olmalıdır. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-deploy-custom-vision/repository.png)

Ardından Visual Studio Code penceresi IoT Edge çözümü çalışma alanınızı yükler.

### <a name="add-your-image-classifier"></a>Görüntü sınıflandırıcınızı ekleme

Visual Studio Code uygulamasındaki Python modülü şablonu, IoT Edge örneğinizi test etmek için çalıştırabileceğiniz örnek kod içerir. Bu senaryoda bu kodu kullanmayacaksınız. Bunun yerine bu bölümdeki adımları kullanarak örnek kodu daha önceden dışarı aktardığınız görüntü sınıflandırıcı kapsayıcısıyla değiştirin. 

1. Dosya gezgininizde indirip ayıkladığınız Özel Görüntü İşleme Hizmeti paketine göz atın. Ayıklanan paketin içeriğin tamamını kopyalayın. **app** ve **azureml** adlı iki klasörün yanı sıra **Dockerfile** ve **README** adlı iki dosyanın bulunması gerekir. 

2. Dosya gezgininde Visual Studio Code'un IoT Edge çözümünüzü oluşturmasını istediğiniz dizine gidin. 

3. Sınıflandırıcı modülü klasörünü açın. Önceki bölümde önerilen adları kullandıysanız klasör yapısı şu şekilde olacaktır: **CustomVisionSolution / modules / classifier**. 

4. Dosyaları **classifier** klasörüne yapıştırın. 

5. Visual Studio Code penceresine dönün. Çözüm çalışma alanınızda görüntü sınıflandırıcı dosyaları modül klasöründe görünmelidir. 

   ![Görüntü sınıflandırıcı dosyalarını gösteren çözüm çalışma alanı](./media/tutorial-deploy-custom-vision/workspace.png)

6. Sınıflandırıcı klasöründeki **module.json** dosyasını açın. 

7. **platforms** parametresini eklediğiniz yeni Dockerfile dosyasını gösterecek şekilde güncelleştirin ve şu an için Özel Görüntü İşleme Hizmeti modülünde desteklenmeyen ARM32 mimarisi ve AMD64.debug seçeneklerini kaldırın. 

   ```json
   "platforms": {
       "amd64": "./Dockerfile"
   }
   ```

8. Yaptığınız değişiklikleri kaydedin. 

### <a name="create-a-simulated-camera-module"></a>Sanal kamera modülü oluşturma

Gerçek bir Özel Görüntü İşleme Hizmeti dağıtımında canlı görüntü veya video akışı sağlayan bir kameranız olacaktır. Bu senaryo için görüntü sınıflandırıcıya test görüntüsü gönderen bir modül oluşturarak kamera simülasyonu yapabilirsiniz. 

#### <a name="add-and-configure-a-new-module"></a>Yeni modül ekleme ve yapılandırma

Bu bölümde aynı CustomVisionSolution öğesine yeni bir modül ekleyecek ve sanal kamerayı oluşturmak için gerekli kodu sağlayacaksınız. 

1. Aynı Visual Studio kod penceresinde komut paletini çalıştırmak için kullanın. **Azure IOT Edge: IOT Edge Modülü Ekle**. Komut paletinde yeni modülünüz için aşağıdaki bilgileri girin: 

   | İstem | Değer | 
   | ------ | ----- |
   | Dağıtım şablonu dosyasını seçin | CustomVisionSolution klasöründeki deployment.template.json dosyasını seçin. |
   | Modül şablonunu seçin | **Python Modülü**'nü seçin |
   | Modül adı sağlayın | Modülünüze **cameraCapture** adını verin |
   | Modül için Docker görüntü deposunu sağlama | **localhost:5000** yerine Azure kapsayıcı kayıt defterinizin oturum açma sunucusu değerini yazın. Dizenin son hali **\<registryname\>.azurecr.io/cameracapture** ifadesine benzer olmalıdır. |

   VS Code penceresi yeni modülünüzü çözüm çalışma alanında yükler ve deployment.template.json dosyasını güncelleştirir. Bu noktada iki modül klasörü görmeniz gerekir: classifier ve cameraCapture. 

2. **modules** / **cameraCapture** klasöründeki **main.py** dosyasını açın. 

3. Dosyanın tamamını aşağıdaki kod ile değiştirin. Bu kod örneği, sınıflandırıcı modülünde çalışan görüntü işleme hizmetine POST istekleri gönderir. Bu modül kapsayıcısı, isteklerde kullanabileceğiniz örnek bir görüntüyle sunulmaktadır. Modül ardından yanıtı bir IoT Hub iletisi olarak paketler ve bir çıkış kuyruğuna gönderir.  

    ```python
    # Copyright (c) Microsoft. All rights reserved.
    # Licensed under the MIT license. See LICENSE file in the project root for
    # full license information.

    import time
    import sys
    import os
    import requests
    import json

    import iothub_client
    # pylint: disable=E0611
    from iothub_client import IoTHubModuleClient, IoTHubClientError, IoTHubTransportProvider
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError
    # pylint: disable=E0401

    # messageTimeout - the maximum time in milliseconds until a message times out.
    # The timeout period starts at IoTHubModuleClient.send_event_async.
    MESSAGE_TIMEOUT = 10000

    # Choose HTTP, AMQP or MQTT as transport protocol.  
    PROTOCOL = IoTHubTransportProvider.MQTT

    # global counters
    SEND_CALLBACKS = 0

    # Send a message to IoT Hub
    # Route output1 to $upstream in deployment.template.json
    def send_to_hub(strMessage):
        message = IoTHubMessage(bytearray(strMessage, 'utf8'))
        hubManager.send_event_to_output("output1", message, 0)

    # Callback received when the message that we send to IoT Hub is processed.
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        SEND_CALLBACKS += 1
        print ( "Confirmation received for message with result = %s" % result )
        print ( "   Total calls confirmed: %d \n" % SEND_CALLBACKS )

    # Send an image to the image classifying server
    # Return the JSON response from the server with the prediction result
    def sendFrameForProcessing(imagePath, imageProcessingEndpoint):
        headers = {'Content-Type': 'application/octet-stream'}

        with open(imagePath, mode="rb") as test_image:
            try:
                response = requests.post(imageProcessingEndpoint, headers = headers, data = test_image)
                print("Response from classification service: (" + str(response.status_code) + ") " + json.dumps(response.json()) + "\n")
            except Exception as e:
                print(e)
                print("Response from classification service: (" + str(response.status_code))

        return json.dumps(response.json())

    class HubManager(object):
        def __init__(self, protocol, message_timeout):
            self.client_protocol = protocol
            self.client = IoTHubModuleClient()
            self.client.create_from_environment(protocol)
            # set the time until a message times out
            self.client.set_option("messageTimeout", message_timeout)
            
        # Sends a message to an output queue, to be routed by IoT Edge hub. 
        def send_event_to_output(self, outputQueueName, event, send_context):
            self.client.send_event_async(
                outputQueueName, event, send_confirmation_callback, send_context)

    def main(imagePath, imageProcessingEndpoint):
        try:
            print ( "Simulated camera module for Azure IoT Edge. Press Ctrl-C to exit." )

            try:
                global hubManager 
                hubManager = HubManager(PROTOCOL, MESSAGE_TIMEOUT)
            except IoTHubError as iothub_error:
                print ( "Unexpected error %s from IoTHub" % iothub_error )
                return

            print ( "The sample is now sending images for processing and will indefinitely.")

            while True:
                classification = sendFrameForProcessing(imagePath, imageProcessingEndpoint)
                send_to_hub(classification)
                time.sleep(10)

        except KeyboardInterrupt:
            print ( "IoT Edge module sample stopped" )

    if __name__ == '__main__':
        try:
            # Retrieve the image location and image classifying server endpoint from container environment
            IMAGE_PATH = os.getenv('IMAGE_PATH', "")
            IMAGE_PROCESSING_ENDPOINT = os.getenv('IMAGE_PROCESSING_ENDPOINT', "")
        except ValueError as error:
            print ( error )
            sys.exit(1)

        if ((IMAGE_PATH and IMAGE_PROCESSING_ENDPOINT) != ""):
            main(IMAGE_PATH, IMAGE_PROCESSING_ENDPOINT)
        else: 
            print ( "Error: Image path or image-processing endpoint missing" )
    ```

4. **main.py** dosyasını kaydedin. 

5. **requrements.txt** dosyasını açın. 

6. Kapsayıcıya dahil etmek için yeni bir kitaplık satırı ekleyin.

   ```Text
   requests
   ```

7. **requirements.txt** dosyasını kaydedin.


#### <a name="add-a-test-image-to-the-container"></a>Kapsayıcıya test görüntüsü ekleme

Bu senaryoda görüntü beslemesi sağlamak için gerçek bir kamera kullanmak yerine tek bir test amaçlı görüntü kullanacağız. Bu öğreticinin önceki bölümlerinde eğitim görüntüleri için indirdiğiniz GitHub deposunda bir test görüntüsü bulunur. 

1. **Cognitive-CustomVision-Windows** / **Samples** / **Images** / **Test** konumundaki test görüntüsüne gidin. 

2. **test_image.jpg** dosyasını kopyalayın 

3. IoT Edge çözümünüzün dizinine gidin ve test görüntüsünü **modules** / **cameraCapture** klasörüne yapıştırın. Görüntü önceki bölümde düzenlediğiniz main.py dosyasıyla aynı klasörde olmalıdır. 

3. Visual Studio Code'da cameraCapture modülünün **Dockerfile.amd64** dosyasını açın. (ARM32 şu an için Özel Görüntü İşleme Hizmeti modülünde desteklenmemektedir). 

4. Çalışma dizinini belirleyen `WORKDIR /app` satırından sonra şu kod satırını ekleyin: 

   ```Dockerfile
   ADD ./test_image.jpg .
   ```

5. Dockerfile dosyasını kaydedin. 

### <a name="prepare-a-deployment-manifest"></a>Dağıtım bildirimi hazırlama

Bu öğreticide bu noktaya kadar ağaç görüntülerini sınıflandırmak için bir Özel Görüntü İşleme Hizmeti modeli eğittiniz ve bu modeli bir IoT Edge modülü olarak paketlediniz. Ardından görüntü sınıflandırma sunucusunu sorgulayıp sonuçlarını IoT Hub'a bildirebilecek ikinci bir modül oluşturdunuz. Artık bir IoT Edge cihazına bu iki modülü birlikte başlatma ve çalıştırma sürecini anlatacak dağıtım bildirimini oluşturmaya hazırsınız. 

Visual Studio Code için IoT Edge uzantısı dağıtım bildirimi oluşturmanıza yardımcı olmak için her IoT Edge çözümünde bir şablon oluşturur. 

1. Çözüm klasöründeki **deployment.template.json** dosyasını açın. 

2. Üç modül içeren **modules** bölümünü bulun: classifier ve cameraCapture olmak üzere sizin oluşturduğunuz iki modüle ek olarak varsayılan olarak dahil edilen tempSensor modülü. 

3. **tempSensor** modülünü ve tüm parametrelerini silin. Bu modül, test senaryoları için örnek veri sağlamak için eklenmiştir ancak bu dağıtımda ihtiyacınız yoktur. 

4. Görüntü sınıflandırma modülüne **classifier** dışında bir ad verdiyseniz adı bu adımda kontrol edin ve tamamının küçük harf olduğundan emin olun. cameraCapture modülü, classifier modülünü tüm istekleri küçük harf olarak biçimlendiren istek kitaplığını kullanarak çağırır ve IoT Edge büyük/küçük harfe duyarlıdır. 

5. cameraCapture modülünün **createOptions** parametresini aşağıdaki JSON koduyla güncelleştirin. Bu bilgiler main.py işleminde alınan modül kapsayıcısında ortam değişkenlerini oluşturur. Bu bilgileri dağıtım bildirimine ekleyerek modül görüntüsünü yeniden derlemek zorunda kalmadan görüntüyü veya uç noktayı değiştirebilirsiniz. 

    ```json
    "createOptions": "{\"Env\":[\"IMAGE_PATH=test_image.jpg\",\"IMAGE_PROCESSING_ENDPOINT=http://classifier/image\"]}"
    ```

    Özel Görüntü İşleme Hizmeti modülünüzün adını *classifier* dışında bir değer olarak belirlediyseniz görüntü işleme uç nokta değerini uygun şekilde güncelleştirin. 

5. Dosyanın en altındaki $edgeHub modülünün **routes** parametresini güncelleştirin. Tahmin sonuçlarının cameraCapture kaynağından IoT Hub'a yönlendirilmesini istersiniz. 

    ```json
        "routes": {
          "CameraCaptureToIoTHub": "FROM /messages/modules/cameraCapture/outputs/* INTO $upstream"
        },
    ```

    İkinci modülün adını *cameraCapture* dışında bir değer olarak belirlediyseniz yönlendirme değerini uygun şekilde güncelleştirin. 

7. **deployment.template.json** dosyasını kaydedin.

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Bu öğreticinin ön koşullarında oluşturduğunuz modüllerin kapsayıcı görüntülerinin depolanması için gerekli olan bir kapsayıcı kayıt defteri listeleniyordu. Kayıt defterinize erişim için gerekli kimlik bilgilerini iki yerde sağlamanız gerekir: görüntülerinizi oluşturup kayıt defterine gönderebilmek için Visual Studio Code uygulamasında ve IoT Edge cihazınızın görüntüleri çekip dağıtabilmesi için dağıtım bildiriminde. 

Azure Container Registry hizmetini kullanıyorsanız [yönetici hesabının](../container-registry/container-registry-authentication.md#admin-account) kullanıcı adı, oturum açma sunucusu ve parola değerlerini bildiğinizden emin olun. 

1. Visual Studio Code'da **Görünüm** > **Terminal**'i seçerek tümleşik terminali açın. 

2. Tümleşik terminale aşağıdaki komutu girin: 

    ```csh/sh
    docker login -u <registry username> <registry login server>
    ```

3. İstendiğinde kayıt defteri parolanızı girin ve **Enter** tuşuna basın.

4. Çözüm klasöründeki **.env** dosyasını açın. Bu dosya git tarafından yoksayılır ve kayıt defteri kimlik bilgilerinizi içerir. Bu sayede bu değerleri dağıtım şablonu dosyanıza eklemeniz gerekmez. 

5. Tırnak işaretlerini eklemeden kapsayıcı kayıt defterinizin kullanıcı adı ve parola değerlerini girin. 

6. **.env** dosyasını kaydedin.

## <a name="build-and-deploy-your-iot-edge-solution"></a>IoT Edge çözümünüzü oluşturma ve dağıtma

İki modülü de oluşturduğunuza ve dağıtım bildirimi şablonunu yapılandırdığınıza göre kapsayıcı görüntülerini oluşturmaya ve kapsayıcı kayıt defterinize göndermeye hazırsınız. 

Görüntüleri kayıt defterinize gönderdikten sonra çözümü bir IoT Edge cihazına dağıtabilirsiniz. IoT Hub üzerinden bir cihazda modül ayarlayabilirsiniz ancak IoT Hub ve cihazlara Visual Studio Code aracılığıyla da erişebilirsiniz. Bu bölümde IoT Hub'ınıza erişim ayarlarını yapacak ve ardından çözümünüzü IoT Edge cihazınıza dağıtmak için VS Code uygulamasını kullanacaksınız.

İlk olarak çözümünüzü oluşturun ve kapsayıcı kayıt defterinize gönderin. 

1. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. Bu işlemin ilerleme durumunu VS Code tümleşik terminalinde izleyebilirsiniz. 
2. Çözümünüze **config** adlı yeni bir klasör eklendiğini göreceksiniz. Bu klasörü genişletin ve içindeki **deployment.json** dosyasını açın.
3. deployment.json dosyasındaki bilgileri gözden geçirin. deployment.json dosyası; yapılandırdığınız dağıtım şablonu dosyasına ek olarak .env dosyası ve module.json dosyaları dahil olmak üzere çözümden alınan bilgilere göre otomatik olarak oluşturulur (veya güncelleştirilir). 

Bir sonraki adımda Visual Studio Code'dan IoT Hub erişimini ayarlayın. 

1. VS Code komut paletinde seçin **Azure IOT Hub: IOT hub'ını seçin**.
2. Azure hesabınızda oturum açmak için yönergeleri izleyin. 
3. Komut paletinde Azure aboneliğinizi ve ardından IoT Hub'ınızı seçin. 

Son olarak cihazınızı seçin ve çözümünüzü dağıtın.

1. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 
2. Dağıtımınızla hedeflemek istediğiniz cihaza sağ tıklayıp **Create deployment for single device** (Tek cihaz için dağıtım oluştur) öğesini seçin. 
3. Dosya gezgininde çözümünüzün içindeki **config** klasörüne gidip **deployment.json** dosyasını seçin. **Select Edge deployment manifest** (Edge dağıtım bildirimini seç) öğesine tıklayın. 

Dağıtım başarılı olursa VS Code çıkışında onay iletisi yazdırılır. VS Code gezgininde bu dağıtım için kullandığınız IoT Edge cihazıyla ilgili ayrıntıları genişletin. Modüller hemen görüntülenmezse imleci **Azure IoT Hub Devices** (Azure IoT Hub Cihazlar) başlığının üzerine getirerek yenileme düğmesini etkinleştirin. Modüllerin başlayıp IoT Hub'a bildirimde bulunması birkaç dakika sürebilir. 

Tüm modüllerin cihazınızda çalışıp çalışmadığını da kontrol edebilirsiniz. IoT Edge cihazında modüllerin durumunu görmek için aşağıdaki komutu çalıştırın. Modüllerin başlaması birkaç dakika sürebilir.

   ```bash
   iotedge list
   ```

## <a name="view-classification-results"></a>Sınıflandırma sonuçlarını görüntüleme

Modüllerinizin sonuçlarını görüntülemek için kullanabileceğiniz iki yöntem vardır. Bunu iletiler gönderilip alındıkça cihazın üzerinden veya iletiler IoT Hub'a ulaştıkça Visual Studio Code'dan gerçekleştirebilirsiniz. 

Cihazdan cameraCapture modülü günlüklerini görüntüleyerek iletilerin gönderildiğini ve IoT Hub tarafından alındığına dair onay verildiğini görebilirsiniz. 

   ```bash
   iotedge logs cameraCapture
   ```

Visual Studio Code'da IoT Edge cihazınızın adına sağ tıklayın ve **Start monitoring D2C message** (D2C iletisini izlemeye başla) öğesini seçin. 

cameraCapture modülünden iletiler olarak gönderilen Özel Görüntü İşleme Hizmeti modülü sonuçları, görüntünün köknar veya kiraz ağacı olma ihtimalini içerir. Görüntü köknar ağacına ait olduğundan olasılık değeri 1.0 olmalıdır. 


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Özel Görüntü İşleme Hizmeti modeli eğittiniz ve bunu modül olarak bir IoT Edge cihazına dağıttınız. Ardından görüntü sınıflandırma hizmetini sorgulayıp sonuçlarını IoT Hub'a bildirebilecek bir modül oluşturdunuz. 

Bu senaryonun canlı kamera akışı kullanan daha ayrıntılı bir sürümünü denemek isterseniz [Custom Vision and Azure IoT Edge on a Raspberry Pi 3](https://github.com/Azure-Samples/Custom-vision-service-iot-edge-raspberry-pi) (Raspberry Pi 3 üzerinde Özel Görüntü İşleme Hizmeti ve Azure IoT Edge) GitHub projesine bakın. 

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Stream Analytics'te kayan pencere kullanarak ortalamaları bulma](tutorial-deploy-stream-analytics.md)
