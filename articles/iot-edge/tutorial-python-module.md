---
title: Azure IOT kenar Python Modülü | Microsoft Docs
description: Python kodu ile bir IOT kenar modül oluşturun ve bir uç cihazın dağıtma
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 3c46df85f95377f5740526542ac1baf5a8fd77c0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Geliştir ve sanal Cihazınızı Python IOT kenar modülünü dağıtmak - Önizleme

IOT kenar modülleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, oluşturma ve dağıtma algılayıcı verileri filtreler bir IOT kenar modülü aracılığıyla açıklanmaktadır. Bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır cihazı kullanacağınız [Windows] [ lnk-tutorial1-win] veya [Linux] [ lnk-tutorial1-lin]öğreticileri. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Bir IOT kenar Python modülü oluşturmak için Visual Studio Code kullanın
> * Docker görüntü oluşturma ve kayıt defterine yayımlama için Visual Studio Code ve Docker kullanın 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturduğunuz IOT kenar modülü cihazınız tarafından oluşturulan sıcaklık verileri filtreler. Sıcaklık belirtilen eşiğin üzerindeyse, yalnızca iletileri upstream gönderir. Bu türde bir kenara çözümlemesini SDK'ya ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

> [!IMPORTANT]
> Şu anda Python modülü yalnızca amd64 Linux kapsayıcılarında çalıştırılabilir; Windows kapsayıcıları veya ARM tabanlı kapsayıcılarını çalıştıramazsınız. 

## <a name="prerequisites"></a>Önkoşullar

* Hızlı Başlangıç ya da ilk öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Visual Studio Code Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker](https://docs.docker.com/engine/installation/) aynı bilgisayarda Visual Studio Code sahiptir. Community Edition (CE), Bu öğretici için yeterlidir. 
* [Python](https://www.python.org/downloads/).
* [PIP](https://pip.pypa.io/en/stable/installing/#installation) (genellikle Python yüklemenizle birlikte dahil) Python paketlerini yüklemek için.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry**'yi seçin.
2. Kayıt defterinize bir ad verin, abonelik seçin, kaynak grubu seçin ve SKU'yu **Temel** olarak ayarlayın. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Öğreticinin sonraki bölümlerinde bu değerleri kullanacaksınız. 

## <a name="create-an-iot-edge-module-project"></a>Bir IOT kenar modülü projesi oluşturma
Aşağıdaki adımlar, Visual Studio Code ve Azure IOT kenar uzantısını kullanarak bir IOT kenar Python modülü oluşturmak nasıl gösterir.
1. Visual Studio kod seçin **Görünüm** > **tümleşik Terminal** VS Code tümleşik Terminali açın.
2. Tümleşik terminale yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin **cookiecutter** (sanal bir ortama ya da kullanıcı yüklemesi aşağıda gösterildiği gibi bunu önerdiğimiz):

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Yeni modül için bir proje oluşturun. Aşağıdaki komut proje klasörünü oluşturur **FilterModule**, kapsayıcı deponuz ile. Parametresi, `image_repository` biçiminde olmalıdır `<your container registry name>.azurecr.io/filtermodule` Azure kapsayıcı kayıt defteri kullanıyorsanız. Geçerli çalışma klasörüne aşağıdaki komutu girin:

    ```cmd/sh
    cookiecutter --no-input https://github.com/Azure/cookiecutter-azure-iot-edge-module module_name=FilterModule image_repository=<your container registry address>/filtermodule
    ```
 
4. Seçin **dosya** > **klasörü açın**.
5. Gözat **FilterModule** klasörü ve tıklatın **klasörü seçin** proje VS Code'da açın.
6. VS Code Explorer'da tıklatın **main.py** açın.
7. Üstündeki **FilterModule** ad alanı, içeri aktarma `json` kitaplığı:

    ```python
    import json
    ```

8. Ekleme `TEMPERATURE_THRESHOLD`, `RECEIVE_CALLBACKS`, ve `TWIN_CALLBACKS` genel sayaçlar altında. Sıcaklık eşiği IOT Hub'ına gönderilecek verileri sırayla ölçülen sıcaklık aşmalıdır değeri ayarlar.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = RECEIVE_CALLBACKS = 0
    ```

9. İşlev güncelleştirme `receive_message_callback` ile içerik aşağıda.

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we will forward this message onto the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print("    Data: <<<{}>>> & Size={:d}".format(message_text, size))
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print("    Properties: {}".format(key_value_pair))
        RECEIVE_CALLBACKS += 1
        print("    Total calls received: {:d}".format(RECEIVE_CALLBACKS))
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature {} exceeds threshold {}".format(data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

10. Yeni bir işlev eklemek `device_twin_callback`. Bu işlev, istenen özelliği güncelleştirildiğinde çağrılır.

    ```python
    # device_twin_callback is invoked when twin's desired properties are updated.
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print("\nTwin callback called with:\nupdateStatus = {}\npayload = {}\ncontext = {}".format(update_state, payload, user_context))
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print("Total calls confirmed: {:d}\n".format(TWIN_CALLBACKS))
    ```

11. Sınıfında `HubManager`, yeni bir satır eklemek `__init__` başlatmak için yöntem `device_twin_callback` eklediğiniz işlevi.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_device_twin_callback(device_twin_callback, self)
    ```


12. Bu dosyayı kaydedin.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker görüntüsü oluşturma ve bunu kayıt defterinize yayımlama

1. VS Code tümleşik terminale aşağıdaki komutu girerek Docker’da oturum açın: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Kullanıcı adı, parola ve oluşturduğunuz sırada, Azure kapsayıcı kayıt defterinden kopyaladığınız oturum açma sunucusu kullanın.

2. VS Code gezgininde **module.json** dosyasına sağ tıklayın ve **IoT Edge modülü Docker görüntüsü derle ve gönder** seçeneğine tıklayın. Örneğin, VS Code pencerenin üstündeki açılır açılır kutusunda kapsayıcı platformunuz seçin **amd64** Linux kapsayıcısı için. VS Code'da containerize `main.py` ve gerekli bağımlılıkları, ardından belirttiğiniz kapsayıcı kayıt defteri, gönderme. Görüntü oluşturmak ilk kez birkaç dakika sürebilir.

3. VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini alabilirsiniz. Derleme ve gönderme tanımı hakkında daha fazla bilgi için `module.json` dosyasına bakabilirsiniz.

## <a name="add-registry-credentials-to-edge-runtime"></a>Kayıt defteri kimlik bilgilerini kenar çalışma zamanına ekleyin
Kayıt defterinizin kimlik bilgilerini, Edge cihazınızı çalıştırdığınız bilgisayarın Edge çalışma zamanına ekleyin. Bu kimlik bilgileri kapsayıcı çıkarmak için çalışma zamanı erişim verin. 

- Windows için şu komutu çalıştırın:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Linux için şu komutu çalıştırın:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. [Azure portalında](https://portal.azure.com) IoT hub'ınıza gidin.
2. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
3. **Modülleri Ayarlama**'yı seçin. 
2. Denetleyin **tempSensor** modülü otomatik olarak doldurulur. Değilse, bunu eklemek için aşağıdaki adımları kullanın:
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `tempSensor` girin.
    3. **Görüntü URI'si** alanına `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` girin.
    4. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'e tıklayın.
9. Ekleme **filterModule** önceki kısımlarında oluşturduğunuz modül. 
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `filterModule` girin.
    3. **Görüntü URI'si** alanına görüntünüzün adresini girin; örneğin `<your container registry address>/filtermodule:0.0.1-amd64`. Tam görüntü adresi, önceki bölümde bulunabilir.
    4. Denetleme **etkinleştirmek** modülü twin düzenleyebilmek kutusu. 
    5. Metin kutusunda JSON modülü çiftinin aşağıdaki JSON ile değiştirin: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. **Kaydet**’e tıklayın.
10. **İleri**’ye tıklayın.
11. **Rota Belirtme** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Modülleri tüm iletileri kenar çalışma zamanına yayımlayın. Çalışma zamanında bildirim temelli kuralları burada iletileri akış tanımlayın. Bu öğreticide, iki yol gerekir. İlk yol sıcaklık algılayıcısı iletilerden filtresi modülü ile yapılandırılmış uç noktası "input1" uç nokta aracılığıyla taşımaları **FilterMessages** işleyicisi. İkinci rota, iletileri filtre modülünden IoT Hub'a taşır. Bu rotada `upstream`, Edge Hub'a iletileri IoT Hub'a göndermesini bildiren özel bir hedeftir. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. **İleri**’ye tıklayın.
5. **Şablonu Gözden Geçirme** adımında **Gönder**’e tıklayın. 
6. IoT Edge cihaz ayrıntıları sayfasına dönün ve **Yenile**’ye tıklayın. Yeni görmelisiniz **filtermodule** ile birlikte çalışan **tempSensor** modülü ve **IOT kenar çalışma zamanı**. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

IoT Edge cihazınızdan IoT hub'ınıza cihazdan buluta gönderilen iletileri izlemek için:
1. Azure IoT Toolkit uzantısını IoT hub'ınızın bağlantı dizesiyle yapılandırın: 
    1. VS Code'da explorer'ı seçerek açın **Görünüm** > **Explorer**. 
    3. Explorer'ın tıklatın **IOT HUB CİHAZLARI** ve ardından **...** . Tıklatın **IOT Hub bağlantı dizesine ayarlamak** ve açılır pencerede IOT kenar Cihazınızı bağlandığı IOT hub'ına yönelik bağlantı dizesini girin. 

        Bağlantı dizesi bulmak için Azure portalında IOT hub'ınız için kutucuğa tıklayın ve ardından **paylaşılan erişim ilkeleri**. İçinde **paylaşılan erişim ilkeleri**, tıklatın **iothubowner** İlkesi ve kopyalama IOT Hub bağlantı dizesi içinde **iothubowner** penceresi.   

1. IOT hub'ına ulaşan verilerin izlemek için seçin **Görünüm** > **komutu palet** arayın ve **IOT: izleme D2C iletisi Başlat** menü komutu. 
2. Veri izlemeyi durdurmak için kullanmak **IOT: D2C ileti izlemeyi durdurmak** menü komutu. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT kenar cihazınız tarafından oluşturulan ham verileri filtrelemek için kodu içeren bir IOT kenar modülünü oluşturuldu. İşletme öngörüleri kenarına içine veri kapatmak Azure IOT kenar yardımcı olabilecek diğer yollarını öğrenmek için aşağıdaki öğreticileri herhangi birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir modül olarak Azure işlevi dağıtmak](tutorial-deploy-function.md)
> [Azure akış analizi bir modül olarak dağıtma](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
