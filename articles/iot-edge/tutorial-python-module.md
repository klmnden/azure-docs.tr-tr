---
title: Azure IoT Edge Python modülü | Microsoft Docs
description: Python kodu ile bir IoT Edge modülü oluşturma ve edge cihazına dağıtma
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 88d772306cb9e67216b380aa885284ebedc77b5f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632117"
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Python IoT Edge modülü geliştirme ve sanal cihazınıza dağıtma - önizleme

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, sensör verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. [Windows][lnk-tutorial1-win] veya [Linux][lnk-tutorial1-lin]’ta sanal makinede Azure IoT Edge’i dağıtma öğreticilerinde oluşturduğunuz sanal IoT Edge cihazını kullanırsınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak IoT Edge Python modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir docker görüntüsü oluşturma ve kayıt defterinize yayımlama 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturduğunuz IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. Yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse ileti yukarı akışını gönderir. Edge’deki bu tür bir analiz, bulutta iletilen ve depolanan veri miktarını azaltmak için yararlıdır. 

> [!IMPORTANT]
> Şu anda Python modülü yalnızca amd64 Linux kapsayıcılarında çalıştırılabilir; Windows kapsayıcılarında veya ARM tabanlı kapsayıcılarda çalıştırılamaz. 

## <a name="prerequisites"></a>Ön koşullar

* Hızlı başlangıçta veya ilk öğreticide oluşturduğunuz Azure IoT Edge cihazı.
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Visual Studio Code için Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* Visual Studio Code içeren aynı bilgisayarda [Docker](https://docs.docker.com/engine/installation/). Community Edition (CE) bu öğretici için yeterlidir. 
* [Python](https://www.python.org/downloads/).
* Python paketlerini (genellikle Python yüklemenize dahildir) yüklemek için [PIP](https://pip.pypa.io/en/stable/installing/#installation).

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry**'yi seçin.
2. Kayıt defterinize bir ad verin, abonelik seçin, kaynak grubu seçin ve SKU'yu **Temel** olarak ayarlayın. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Öğreticinin sonraki bölümlerinde bu değerleri kullanacaksınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code ve Azure IoT Edge uzantısını kullanarak IoT Edge Python modülünün nasıl oluşturulduğu gösterilmektedir.
1. Visual Studio Code’da, VS Code tümleşik terminalini açmak için **Görünüm** > **Tümleşik Terminal**’i seçin.
2. Tümleşik terminalde, **cookiecutter** yüklemek (veya güncelleştirmek) için aşağıdaki komutu girin (bunun sanal bir ortamda veya aşağıda gösterildiği gibi bir kullanıcı yüklemesi olarak yapılmasını öneririz):

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Yeni modül için bir proje oluşturun. Aşağıdaki komut, kapsayıcı deponuzla birlikte **FilterModule** adlı proje klasörünü oluşturur. Azure kapsayıcı kayıt defterini kullanıyorsanız `image_repository` parametresi `<your container registry name>.azurecr.io/filtermodule` biçiminde olmalıdır. Geçerli çalışma klasörüne aşağıdaki komutu girin:

    ```cmd/sh
    cookiecutter --no-input https://github.com/Azure/cookiecutter-azure-iot-edge-module module_name=FilterModule image_repository=<your container registry address>/filtermodule
    ```
 
4. **Dosya** > **Klasörü Aç**’ı seçin.
5. **FilterModule** klasörüne göz atın ve **Klasör Seç**’e tıklayarak VS Code’da projeyi açın.
6. VS Code gezgininde **main.py** dosyasına tıklayıp dosyayı açın.
7. **FilterModule** ad alanının en üst kısmında `json` kitaplığını içeri aktarın:

    ```python
    import json
    ```

8. Genel sayaçlar bölümüne `TEMPERATURE_THRESHOLD`, `RECEIVE_CALLBACKS` ve `TWIN_CALLBACKS` öğesini ekleyin. Sıcaklık eşiği, verilerin IoT Hub’a gönderilmesi için, ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = RECEIVE_CALLBACKS = 0
    ```

9. `receive_message_callback` işlevini aşağıdaki içerikle güncelleştirin.

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

10. Yeni `device_twin_callback` işlevini ekleyin. İstenen özellikler güncelleştirildiğinde bu işlev çağrılır.

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

11. `HubManager` sınıfında, yeni eklediğiniz `device_twin_callback` işlevini başlatmak için `__init__` yöntemine yeni bir satır ekleyin.

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
        
   Oluşturduğunuz zaman Azure kapsayıcı kayıt defterinizden kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın.

2. VS Code gezgininde **module.json** dosyasına sağ tıklayın ve **IoT Edge modülü Docker görüntüsü derle ve gönder** seçeneğine tıklayın. VS Code penceresinin üst kısmındaki açılır kutuda, kapsayıcı platformunuzu (örneğin, Linux kapsayıcı için **amd64**) seçin. VS Code, `main.py` öğesini ve gerekli bağımlılıkları kapsayıcılı hale getirir, daha sonra bunu belirttiğiniz kapsayıcı kayıt defterine gönderir. Görüntüyü ilk kez derlemeniz birkaç dakika sürebilir.

3. VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini alabilirsiniz. Derleme ve gönderme tanımı hakkında daha fazla bilgi için `module.json` dosyasına bakabilirsiniz.

## <a name="add-registry-credentials-to-edge-runtime"></a>Edge çalışma zamanına kayıt defteri kimlik bilgileri ekleme
Kayıt defterinizin kimlik bilgilerini, Edge cihazınızı çalıştırdığınız bilgisayarın Edge çalışma zamanına ekleyin. Bu kimlik bilgileri, kapsayıcıyı çekmek için çalışma zamanı erişimi sağlar. 

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
2. **tempSensor** modülünün otomatik olarak doldurulup doldurulmadığını denetleyin. Doldurulmadıysa, eklemek için aşağıdaki adımları kullanın:
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `tempSensor` girin.
    3. **Görüntü URI'si** alanına `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` girin.
    4. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'e tıklayın.
9. Önceki bölümlerde oluşturduğunuz **filterModule** modülünü ekleyin. 
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `filterModule` girin.
    3. **Görüntü URI'si** alanına görüntünüzün adresini girin; örneğin `<your container registry address>/filtermodule:0.0.1-amd64`. Tam görüntü adresi, önceki bölümde bulunabilir.
    4. Modül ikizini düzenleyebilmeniz için **Etkinleştir** kutusunu işaretleyin. 
    5. Modül ikizinin metin kutusundaki JSON’ı, aşağıdaki JSON ile değiştirin: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. **Kaydet**’e tıklayın.
10. **İleri**’ye tıklayın.
11. **Rota Belirtme** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Modüller tüm iletileri Edge çalışma zamanına yayımlar. Çalışma zamanındaki bildirim kuralları, iletilerin akış yerini tanımlar. Bu öğreticide iki rota gerekir. Birinci rota, sıcaklık sensöründen gelen iletileri, **FilterMessages** işleyicisi ile yapılandırdığınız uç noktası olan "input1" uç noktası aracılığıyla filtre modülüne taşır. İkinci rota, iletileri filtre modülünden IoT Hub'a taşır. Bu rotada `upstream`, Edge Hub'a iletileri IoT Hub'a göndermesini bildiren özel bir hedeftir. 

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
6. IoT Edge cihaz ayrıntıları sayfasına dönün ve **Yenile**’ye tıklayın. **tempSensor** modülü ve **IoT Edge çalışma zamanı** ile birlikte çalışan yeni **filtermodule** modülünü görürsünüz. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

IoT Edge cihazınızdan IoT hub'ınıza cihazdan buluta gönderilen iletileri izlemek için:
1. Azure IoT Toolkit uzantısını IoT hub'ınızın bağlantı dizesiyle yapılandırın: 
    1. **Görünüm** > **Gezgin** seçeneğini belirleyerek VS Code gezginini açın. 
    3. Gezginde **IOT HUB CİHAZLARI**’na ve ardından **...** düğmesine tıklayın. **IoT Hub Bağlantı Dizesi Ayarla** seçeneğine tıklayın ve açılır pencerede IoT Edge cihazınızın bağlandığı IoT hub için bağlantı dizesini girin. 

        Bağlantı dizesini bulmak için, Azure portalında IoT hub’ınız için kutucuğa tıklayın ve **Paylaşılan erişim ilkeleri**’ne tıklayın. **Paylaşılan erişim ilkeleri** penceresinde **iothubowner** ilkesine tıklayın ve **iothubowner** penceresindeki IoT Hub bağlantı dizesini kopyalayın.   

1. IoT hub'da gelen verileri izlemek için, **Görünüm** > **Komut Paleti** seçeneğini belirleyin ve **IoT: D2C iletisini izlemeye başlama** menü komutunu arayın. 
2. Verileri izlemeyi durdurmak için **IoT: D2C iletisini izlemeyi durdur** menü komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Edge cihazınız tarafından oluşturulan ham verileri filtreleme kodunu içeren bir IoT Edge modülü oluşturdunuz. Azure IoT Edge’in verileri edge’de iş içgörüsüne dönüştürmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için aşağıdaki öğreticilerden herhangi biriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Modül olarak Azure İşlevi dağıtma](tutorial-deploy-function.md)
> [Modül olarak Azure Stream Analytics dağıtma](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
