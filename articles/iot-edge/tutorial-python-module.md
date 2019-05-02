---
title: Azure IOT Edge özel Python modülü - oluştur | Microsoft Docs
description: Bu öğreticide Python koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilmektedir.
services: iot-edge
author: shizn
manager: philmea
ms.reviewer: kgremban
ms.author: xshi
ms.date: 03/24/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 82da44409c4500ff097805efec33cec8cf6bbedd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64575627"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-for-linux-devices"></a>Öğretici: Linux cihazlar için bir Python IOT Edge modülü geliştirme ve dağıtma

Visual Studio Code, C kodu geliştirmek ve Azure IOT Edge çalıştıran bir Linux cihazına dağıtmak için kullanın. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, oluşturma ve hızlı başlangıç bölümünde ayarladığınız IOT Edge cihazı üzerinde algılayıcı verilerini filtreleyen bir IOT Edge modülü dağıtımı açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak IoT Edge Python modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğreticide bir modülde nasıl geliştirilebileceğini gösterir **Python** kullanarak **Visual Studio Code**ve dağıtmak nasıl bir **Linux cihaz**. IOT Edge, Windows cihazları için Python modüllerini desteklemez. 

Geliştirme ve Python modüllerini Linux'a dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın: 

| Python | Visual Studio Code | Visual Studio 2017 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Python modüllerde Linux AMD64 için VS Code kullanma](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![Linux ARM32 Python modüllerde için VS Code kullanma](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce Linux kapsayıcı geliştirme için geliştirme ortamınızı ayarlamak için önceki öğreticide çalıştınız: [IOT Edge modülleri Linux cihazlar için geliştirme](tutorial-develop-for-linux.md). Ya da bu öğreticileri izleyerek aşağıdaki önkoşulların yerinde olmalıdır: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Azure IOT Edge çalıştıran Linux cihaz](quickstart-linux.md)
* Kapsayıcı kayıt defteri gibi [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) ile yapılandırılmış [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak üzere yapılandırılmış.

Python'da bir IOT Edge modülü geliştirme için geliştirme makinenizde aşağıdaki ek önkoşulları yükleyin: 

* Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Python](https://www.python.org/downloads/).
* Python paketlerini (genellikle Python yüklemenize dahildir) yüklemek için [PIP](https://pip.pypa.io/en/stable/installing/#installation).

>[!Note]
>`bin` klasörünüzün platformunuzun yolunda olduğundan emin olun. Bu yol genelde UNIX ve macOS için `~/.local/`, Windows için `%APPDATA%\Python` olacaktır.

## <a name="create-a-module-project"></a>Bir modülü projesi oluşturma
Aşağıdaki adımlar Visual Studio Code ve Azure IOT Araçları'nı kullanarak bir IOT Edge Python modülü oluşturur.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Üzerine ekleme yapabileceğiniz bir Python çözümü oluşturmak için **cookiecutter** Python paketini kullanın. 

1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Terminal**'i seçin.

2. Terminalde yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin **cookiecutter**, IOT Edge çözüm şablonu oluşturmak için kullanın:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```
   >[!Note]
   >Dizin olun cookiecutter yüklü olduğu bir komut istemi'nden çağrılacak yapabilmeleri için ortamınızın yolunda olduğundan. Dizin yükleme betiği, çıktısını örneğin parçasıdır `C:\Users\{user}\AppData\Roaming\Python\Python{version}\Scripts`.
   >
   >Visual Studio Code yolu değişiklikleri alması için yeniden başlatın. 

3. VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 

4. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure: Oturum** ve Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

5. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Komut istemlerini izleyin ve çözümünüzü oluşturmak için aşağıdaki bilgileri sağlayın:

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | **Python Modülü**'nü seçin. |
   | Modül adı sağlayın | Modülünüze **PythonModule** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü, son adımda sağladığınız adından doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/pythonmodule. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-python-module/repository.png)


### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı deponuzun kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. 

1. VS Code gezgininde **.env** dosyasını açın. 
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin. 
3. .env dosyasını kaydedin. 

### <a name="select-your-target-architecture"></a>Hedef Mimarinizi seçin

Şu anda, Visual Studio Code, C modülleri Linux AMD64 ve Linux ARM32v7 cihazlar için geliştirebilirsiniz. Kapsayıcı oluşturulur ve her bir mimari türü için farklı çalıştır olduğundan, her bir çözüm ile hedeflediğiniz hangi mimari seçmeniz gerekir. Linux AMD64 varsayılandır. 

1. Komut paletini açın ve arama **Azure IOT Edge: Varsayılan hedef Platform için Edge çözümü ayarlayın**, veya pencerenin alt kısmındaki kenar çubuğu kısayol simgesini seçin. 

2. Komut paletini hedef mimari seçeneklerini listeden seçin. Bu öğreticide, bir Ubuntu sanal makinesi varsayılan tutacak şekilde IOT Edge cihazı kullandığımız **amd64**. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Her şablonda, **tempSensor** modülündeki sensör simülasyon verilerini alıp IoT Hub'a yönlendiren örnek kod bulunur. Bu bölümde **PythonModule** modülünün iletileri göndermeden analiz etmesini sağlayan kodu ekleyeceksiniz. 

1. VS Code gezgininde **modules** > **PythonModule** > **main.py** girişini açın.

2. **main.py** dosyasının en üst kısmından **json** kitaplığını içeri aktarın:

    ```python
    import json
    ```

3. **TEMPERATURE_THRESHOLD** ve **TWIN_CALLBACKS** değişkenlerini global sayaçlarının altına ekleyin. Sıcaklık eşiği, verilerin IoT Hub’a gönderilmesi için, ölçülen makine sıcaklığının aşması gereken değeri ayarlar.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. **receive_message_callback** işlevini aşağıdaki kodla değiştirin:

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we forward this message to the "output1" queue.
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

5. **module_twin_callback** adlı yeni bir işlev ekleyin. İstenen özellikler güncelleştirildiğinde bu işlev çağrılır.

    ```python
    # module_twin_callback is invoked when the module twin's desired properties are updated.
    def module_twin_callback(update_state, payload, user_context):
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

6. **HubManager** sınıfında **__init__** metoduna yeni bir satır ekleyerek az önce eklediğiniz **module_twin_callback** işlevini başlatın:

    ```python
    # Sets the callback when a module twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Main.py dosyayı kaydedin.

8. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. 

9. Dağıtım bildirimine **PythonModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin: 

   ```json
       "PythonModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-python-module/module-twin.png)

10. Deployment.template.json dosyayı kaydedin.

## <a name="build-and-push-your-module"></a>Oluşturun ve modülünüzde gönderin

Önceki bölümde, IOT Edge çözümünü oluşturan ve iletileri kabul edilebilir sınırlar içinde bildirilen makine sıcaklık olduğu filtre PythonModule kod eklenir. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. **Görünüm** > **Terminal**'i seçerek VS Code tümleşik terminalini açın.

1. Terminalde aşağıdaki komutu girerek Docker'da oturum açın. Kullanıcı adı, parola ve oturum açma sunucusu, Azure container registry'den oturum açın. Bu değerleri alabilirsiniz **erişim anahtarları** Azure portalında kayıt defterinizin bölümü.
     
   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Kullanılmasını öneren bir güvenlik uyarısı alabilirsiniz `--password-stdin`. Bu en iyi uygulama, üretim senaryoları için önerilir, bu öğreticinin kapsamı dışında olan. Daha fazla bilgi için [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvuru.

2. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin.

   Derleme ve gönderme komut üç işlemi başlatır. İlk olarak, adlı bir çözüm içinde yeni bir klasör oluşturur **config** , tam bir dağıtım bildirimi, yerleşik dağıtım şablonu bilgilerinin kullanıma ve diğer çözüm dosyalarını içerir. İkinci olarak, çalıştığında `docker build` uygun dockerfile, hedef mimari için temel kapsayıcı görüntüsünü oluşturmak için. Ardından, çalışan `docker push` kapsayıcı kayıt defterinize görüntü deposuna gönderin.


## <a name="deploy-modules-to-device"></a>Modüller cihazına dağıtma

IOT Edge cihazınıza modülü projeyi dağıtmak için Visual Studio kod Gezgini ve Azure IOT araçları uzantısını kullanın. Senaryonuz için hazırlanmış bir dağıtım bildirimi zaten **deployment.json** config klasöründeki dosya. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IOT Edge Cihazınızı ve çalışıyor olduğundan emin olun.

1. Visual Studio Code Gezgini'nde **Azure IOT Hub cihazları** IOT cihazlarınızın listesini görmek için bölümü.

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin.

3. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın.

4. Yenile düğmesine tıklayın. Yeni **PythonModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code Gezgininde, IOT Edge cihazınızın adına sağ tıklayıp **izleme D2C iletileri Başlat**.

2. IOT hub'da gelen iletileri görüntüleyin. IOT Edge cihazı, yeni dağıtım almak ve tüm modülleri başlatmak olduğundan bunu ulaşması iletileri için biraz zaman alabilir. Daha sonra iletileri göndermeden önce 25 derece makine sıcaklık ulaşana kadar biz PythonModule koda yapılan değişiklikleri bekleyin. İleti türü de ekler **uyarı** sıcaklık eşiğe ulaşan tüm iletileri. 

## <a name="edit-the-module-twin"></a>Modül ikizi Düzenle

Biz PythonModule modül ikizi dağıtım bildiriminde sıcaklık eşik 25 derecede ayarlamak için kullanılır. Modül ikizi modülü kodunu güncelleştirmek zorunda kalmadan işlevlerini değiştirmek için kullanabilirsiniz.

1. Visual Studio Code'da altında çalışan modülleri görmek için IOT Edge cihaz ayrıntıları genişletin. 

2. Sağ **PythonModule** seçip **düzenleme modül ikizi**. 

3. Bulma **TemperatureThreshold** istenen özellikleri. Değeri 5 10 derece son bildirilen sıcaklık daha yüksek derece için yeni bir sıcaklık değiştirin. 

4. Modül ikizi dosyayı kaydedin.

5. Modül ikizi düzenleme bölmesinde ve seçme içinde herhangi bir yeri sağ **güncelleştirme modül ikizi**. 

5. CİHAZDAN buluta gelen iletileri izleyin. Yeni sıcaklık eşiği ulaşılana kadar Durdur iletileri görmeniz gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Aksi takdirde, yerel yapılandırmaları ve ücretleri önlemek için bu makalede kullanılan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi derlemek için hazır olduğunuzda, daha fazla bilgi edinebilirsiniz [kendi IOT Edge modülleri geliştirmek](module-development.md) veya nasıl [modülleri Visual Studio Code ile geliştirme](how-to-vs-code-develop-module.md). Azure IOT Edge, uçta verilerini işlemek ve çözümlemek için Azure bulut Hizmetleri dağıtmayı nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticiler açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [İşlevleri](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [makine öğrenimi](tutorial-deploy-machine-learning.md)
> [özel görüntü işleme hizmeti](tutorial-deploy-custom-vision.md)