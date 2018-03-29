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
ms.openlocfilehash: d5bad277e6a54b23f0e3ef7321e82d212ae885d3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Geliştir ve sanal Cihazınızı Python IOT kenar modülünü dağıtmak - Önizleme

IOT kenar modülleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, oluşturma ve dağıtma algılayıcı verileri filtreler bir IOT kenar modülü aracılığıyla açıklanmaktadır. Bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır cihazı kullanacağınız [Windows] [ lnk-tutorial1-win] veya [Linux] [ lnk-tutorial1-lin]öğreticileri. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Bir IOT kenar Python modülü oluşturmak için Visual Studio Code kullanın
> * Docker görüntü oluşturma ve kayıt defterine yayımlama için Visual Studio Code ve Docker kullanın 
> * Modül IOT kenar Cihazınızı dağıtma
> * Oluşturulan görünüm verileri


Bu öğreticide oluşturduğunuz IOT kenar modülü cihazınız tarafından oluşturulan sıcaklık verileri filtreler. Sıcaklık belirtilen eşiğin üzerindeyse, yalnızca iletileri upstream gönderir. Bu türde bir kenara çözümlemesini SDK'ya ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

> [!IMPORTANT]
> Şu anda Python modülü yalnızca amd64 Linux kapsayıcılarında çalışıyor olabilir. Çalışan Windows kapsayıcıları veya ARM tabanlı kapsayıcıları yapılamıyor. 

## <a name="prerequisites"></a>Önkoşullar

* Hızlı Başlangıç ya da ilk öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Visual Studio Code Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker](https://docs.docker.com/engine/installation/) aynı bilgisayarda Visual Studio Code sahiptir. Community Edition (CE), Bu öğretici için yeterlidir. 
* [Python](https://www.python.org/downloads/).
* [PIP](https://pip.pypa.io/en/stable/installing/#installation) Python paketlerini yüklemek için.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide, bir modülü oluşturmak ve oluşturmak için Azure IOT kenar uzantısı VS Code için kullanmak bir **kapsayıcı görüntü** dosyalarından. Bu görüntü için anında sonra bir **kayıt defteri** depolar ve resimlerinizi yönetir. Son olarak, IOT kenar aygıtınızda çalıştırmak için kayıt defterinden görüntünüzü dağıtın.  

Bu öğretici için Docker uyumlu kayıt kullanabilirsiniz. İki popüler Docker kayıt defteri hizmetlerinin kullanılabilir bulutta [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) ve [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu öğretici, Azure kapsayıcı kayıt defteri kullanır. 

1. İçinde [Azure portal](https://portal.azure.com)seçin **kaynak oluşturma** > **kapsayıcıları** > **Azure kapsayıcı kayıt defteri** .
2. Kayıt bir ad verin, bir abonelik seçin, bir kaynak grubu seçin ve SKU kümesine **temel**. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kaydınız oluşturulduktan sonra kendisine gidin ve seçin **erişim anahtarları**. 
5. İki durumlu **yönetici kullanıcı** için **etkinleştirmek**.
6. Değerleri kopyalamak **oturum açma sunucusu**, **kullanıcıadı**, ve **parola**. Öğreticide daha sonra bu değerleri kullanacaksınız. 

## <a name="create-an-iot-edge-module-project"></a>Bir IOT kenar modülü projesi oluşturma
Aşağıdaki adımlar, Visual Studio Code ve Azure IOT kenar uzantısını kullanarak bir IOT kenar Python modülü oluşturmak nasıl gösterir.
1. Visual Studio kod seçin **Görünüm** > **tümleşik Terminal** VS Code tümleşik Terminali açın.
2. Tümleşik terminale yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin **cookiecutter**:

    ```cmd/sh
    pip install -U cookiecutter
    ```

3. Yeni modül için bir proje oluşturun. Aşağıdaki komut proje klasörünü oluşturur **FilterModule**, kapsayıcı deponuz ile. Parametresi, `image_repository` biçiminde olmalıdır `<your container registry name>.azurecr.io/filtermodule` Azure kapsayıcı kayıt defteri kullanıyorsanız. Geçerli çalışma klasöründe aşağıdaki komutu girin:

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

8. Ekleme `TEMPERATURE_THRESHOLD` ve `TWIN_CALLBACKS` genel sayaçlar altında. Sıcaklık eşiği IOT Hub'ına gönderilecek verileri sırayla ölçülen sıcaklık aşmalıdır değeri ayarlar.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
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
        print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

10. Yeni bir işlev eklemek `device_twin_callback`. Bu işlev, istenen özelliği güncelleştirildiğinde çağrılır.

    ```python
    # device_twin_callback is invoked when twin's desired properties are updated.
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print ( "\nTwin callback called with:\nupdateStatus = %s\npayload = %s\ncontext = %s" % (update_state, payload, user_context) )
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
    ```

11. Sınıfında `HubManager`, yeni bir satır eklemek `__init__` başlatmak için yöntem `device_twin_callback` eklediğiniz işlevi.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_device_twin_callback(device_twin_callback, self)
    ```


12. Bu dosyayı kaydedin.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker görüntü oluşturma ve kayıt defterine yayımlama

1. VS Code tümleşik terminale aşağıdaki komutu girerek Docker için oturum açın: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Kullanıcı adı, parola ve oluşturduğunuz sırada, Azure kapsayıcı kayıt defterinden kopyaladığınız oturum açma sunucusu kullanın.

2. VS Code Explorer'da sağ **module.json** dosya ve tıklayın **oluşturma ve gönderme IOT kenar modülü Docker görüntü**. Örneğin, VS Code pencerenin üstündeki açılır açılır kutusunda kapsayıcı platformunuz seçin **amd64** Linux kapsayıcısı için. VS Code'da containerize `main.py` ve gerekli bağımlılıkları, ardından belirttiğiniz kapsayıcı kayıt defteri, gönderme. Görüntü oluşturmak ilk kez birkaç dakika sürebilir.

3. Tam kapsayıcı görüntü adresi etiketi ile tümleşik VS Code'da terminal alabilirsiniz. Derleme ve anında iletme tanımı hakkında daha fazla bilgi için başvurabilirsiniz `module.json` dosya.

## <a name="add-registry-credentials-to-edge-runtime"></a>Kayıt defteri kimlik bilgilerini kenar çalışma zamanına ekleyin
Sınır cihazı çalıştırdığınız bilgisayarda kenar çalışma zamanı kayıt için kimlik bilgilerini ekleyin. Bu kimlik bilgileri kapsayıcı çıkarmak için çalışma zamanı erişim verin. 

- Windows için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Linux için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. İçinde [Azure portal](https://portal.azure.com), IOT hub'ına gidin.
2. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
3. Seçin **ayarlamak modülleri**. 
2. Denetleyin **tempSensor** modülü otomatik olarak doldurulur. Değilse, bunu eklemek için aşağıdaki adımları kullanın:
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `tempSensor`.
    3. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Diğer ayarları değiştirmeden bırakın ve tıklayın **kaydetmek**.
9. Ekleme **filterModule** önceki kısımlarında oluşturduğunuz modül. 
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `filterModule`.
    3. İçinde **görüntü URI** alan, görüntü adresinizi girin; örneğin `<your container registry address>/filtermodule:0.0.1-amd64`. Tam görüntüyü adresi önceki bölümden bulunabilir.
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
11. İçinde **belirtin yollar** adım, JSON altındaki metin kutusuna Kopyala. Modülleri tüm iletileri kenar çalışma zamanına yayımlayın. Çalışma zamanında bildirim temelli kuralları burada iletileri akış tanımlayın. Bu öğreticide, iki yol gerekir. İlk yol sıcaklık algılayıcısı iletilerden filtresi modülü ile yapılandırılmış uç noktası "input1" uç nokta aracılığıyla taşımaları **FilterMessages** işleyicisi. İkinci yol filtresi modülü gelen iletileri IOT Hub'ına taşımaları. Bu rotadaki `upstream` kenar Hub'ın IOT Hub'ına iletileri göndermek için söyler özel bir hedef. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. **İleri**’ye tıklayın.
5. İçinde **gözden geçirme şablonu** adımını, **gönderme**. 
6. IOT Edge cihaz ayrıntıları sayfasına dönün ve **yenileme**. Yeni görmelisiniz **filtermodule** ile birlikte çalışan **tempSensor** modülü ve **IOT kenar çalışma zamanı**. 

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Cihaz bulut iletilerini IOT kenar cihazınızın IOT hub'ına gönderilen izlemek için:
1. IOT hub'ınız için bağlantı dizesi ile Azure IOT Araç Seti uzantısı yapılandırın: 
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
