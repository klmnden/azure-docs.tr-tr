---
title: Öğretici, Azure IOT Edge - özel Python modülü oluşturma | Microsoft Docs
description: Bu öğreticide Python koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilmektedir.
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 11/25/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: a8edf8d67c55cad856eacf883a6449606e594887
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343777"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Öğretici: Geliştirme ve Python IOT Edge modülü sanal Cihazınızı dağıtma

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Hızlı başlangıçlarda oluşturduğunuz sanal Azure IoT Edge cihazını kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak IoT Edge Python modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.
* IoT Edge Python modülleri Windows cihazları desteklemez.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/). 
* Visual Studio Code için [Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).
* Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Python](https://www.python.org/downloads/).
* Python paketlerini (genellikle Python yüklemenize dahildir) yüklemek için [PIP](https://pip.pypa.io/en/stable/installing/#installation).

>[!Note]
>`bin` klasörünüzün platformunuzun yolunda olduğundan emin olun. Bu yol genelde UNIX ve macOS için `~/.local/`, Windows için `%APPDATA%\Python` olacaktır.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu öğreticide, bir modül olarak derlemek ve oluşturmak için Visual Studio Code için Azure IOT Edge uzantısını kullanırsınız bir **kapsayıcı görüntüsü** dosyalarından. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Herhangi bir Docker ile uyumlu kayıt defteri, kapsayıcı görüntülerinizi tutmak için kullanabilirsiniz. İki popüler Docker kayıt defteri Hizmetleri [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu öğreticide Azure Container Registry kullanılır. 

Kapsayıcı kayıt defteri zaten yoksa, azure'da yeni bir tane oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

2. Kapsayıcı kayıt defterinizi oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer | 
   | ----- | ----- |
   | Kayıt defteri adı | Benzersiz bir ad girin. |
   | Abonelik | Açılan listeden bir abonelik seçin. |
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. |
   | Yönetici kullanıcı | **Etkinleştir**'i seçin. |
   | SKU | **Temel**'i seçin. | 

5. **Oluştur**’u seçin.

6. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 

7. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Kapsayıcı kayıt defterine erişim sağlamak için öğreticinin ilerleyen bölümlerinde bu değerleri kullanırsınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code'u ve Azure IoT Python modülünü kullanarak IoT Edge işlevi oluşturulur.

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Üzerine ekleme yapabileceğiniz bir Python çözümü oluşturmak için **cookiecutter** Python paketini kullanın. 

1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Terminal**'i seçin.

2. Tümleşik terminalde aşağıdaki komutu kullanarak VS Code'da IoT Edge çözüm şablonunu oluşturmak için yararlanacağınız **cookiecutter** paketini yükleyin (veya güncelleştirin):

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```
   >[!Note]
   >Dizin olun cookiecutter yüklü olduğu bir komut istemi'nden çağrılacak yapabilmeleri için ortamınızın yolunda olduğundan. Dizin yükleme betiği, çıktısını örneğin parçasıdır `C:\Users\{user}\AppData\Roaming\Python\Python{version}\Scripts`.
   >
   >Visual Studio Code yolu değişiklikleri alması için yeniden başlatın. 

3. VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 

4. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure: Oturum** ve Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

5. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | **Python Modülü**'nü seçin. |
   | Modül adı sağlayın | Modülünüze **PythonModule** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. Dizenin son hali \<kayıt adı\>.azurecr.io/pythonmodule ifadesine benzer olmalıdır. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-python-module/repository.png)

VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Çözüm çalışma alanında beş üst düzey bileşen bulunur. **modules** klasöründe modülünüzün Python kodunun yanı sıra modülünüzden kapsayıcı görüntüsü oluşturmak için kullanılacak Dockerfiles öğeleri bulunur. **\.env** dosyasında kapsayıcı kayıt defterinizin kimlik bilgileri yer alır. **deployment.template.json** dosyasında IoT Edge çalışma zamanının modülleri cihazlara dağıtmak için kullandığı bilgiler bulunur. Ve **deployment.debug.template.json** kapsayıcıları dosyanın modülleri hata ayıklama sürümü. Bu öğreticide **\.vscode** klasörünü veya **\.gitignore** dosyasını düzenlemeyeceksiniz.  

Çözümünüzü oluştururken kapsayıcı kayıt defteri belirtmediyseniz ve varsayılan localhost:5000 değerini kabul ettiyseniz \.env dosyanız olmaz. 

<!--
   ![Python solution workspace](./media/tutorial-python-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı deponuzun kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. 

1. VS Code gezgininde .env dosyasını açın. 
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin. 
3. Bu dosyayı kaydedin. 

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

7. Bu dosyayı kaydedin.

8. VS Code Gezgininde açın **deployment.template.json** dosya. 

   Bu dosya **$edgeAgent** için iki modül dağıtma komutu verir: Cihaz verilerinin simülasyonunu yapan **tempSensor** ve **PythonModule**. Varsayılan platform, IOT Edge kümesine **amd64** , VS Code durum çubuğunda anlamına gelir, **PythonModule** görüntünün amd64 sürüme Linux ayarlanır. Durum çubuğunda varsayılan platform değiştirme **amd64** için **arm32v7** veya **windows-amd64** , IOT Edge cihazınızın mimari ise. Dağıtım bildirimleri hakkında daha fazla bilgi edinmek için bkz. [IoT Edge modüllerinin kullanılmasını, yapılandırılmasını ve yeniden kullanılmasını anlama](module-composition.md).

   Bu dosyada kayıt defteri kimlik bilgileriniz de bulunur. Şablon dosyasında kullanıcı adı ve parola değerlerinin yerine yer tutucular kullanılmıştır. Dağıtım bildirimini oluşturduğunuzda alanlar .env dosyasına eklediğiniz değerlerle güncelleştirilir. 

9. Dağıtım bildirimine **PythonModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin: 

   ```json
       "PythonModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-python-module/module-twin.png)

10. Bu dosyayı kaydedin.

## <a name="build-and-push-your-solution"></a>Oluşturun ve çözümünüzü gönderin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve **PythonModule** modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtreleyen kodu eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor. 

1. Visual Studio Code tümleşik terminaline aşağıdaki komutu girerek Docker’da oturum açın. Ardından, modül görüntünüzü Azure kapsayıcı kayıt defterinize gönderin: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Birinci bölümde Azure kapsayıcı kayıt defterinizden kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın. Bu değerleri Azure portalındaki kayıt defterinizin **Access keys** bölümünden de alabilirsiniz.

2. VS Code gezgininde deployment.template.json dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve **config** adlı yeni bir klasörde deployment.json dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut kodunuzu derler, Python koduyla kapsayıcı oluşturur ve kodu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

VS Code tümleşik terminalinde çalışan `docker build` komutunda tam kapsayıcı görüntüsü adresini ve etiketi görebilirsiniz. Görüntü adresi module.json dosyasındaki bilgilerden \<depo\>:\<sürüm\>-\<platform\> biçiminde oluşturulur. Bu öğretici için registryname.azurecr.io/pythonmodule:0.0.1-amd64 şeklinde olmalıdır.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını (eski adıyla Azure IOT Toolkit uzantısını) kullanarak modülleri da dağıtabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildirimi dosyasına (**deployment.json**) zaten sahipsiniz. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

1. VS Code komut paletini içinde çalıştırma **Azure IOT Hub: IOT hub'ını seçin**. 

2. Yapılandırmak istediğiniz IoT Edge cihazını barındıran aboneliği ve IoT hub'ını seçin. 

3. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

4. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin. 

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-python-module/create-deployment.png)

5. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın. 

6. Yenile düğmesine tıklayın. Yeni **PythonModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır. 

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin. 

IoT Edge cihazında `iotedge list` komutunu kullanarak dağıtım modüllerinin durumunu görebilirsiniz. Dört modül görmeniz gerekir: İki IoT Edge çalışma zamanı modülü, tempSensor ve bu öğreticide oluşturduğunuz özel modül. Tüm modüllerin başlatılması birkaç dakika sürebilir. Bu nedenle tümünü görmüyorsanız komutu yeniden çalıştırın. 

Modüller tarafından oluşturulan iletileri görüntülemek için `iotedge logs <module name>` komutunu kullanın. 

Visual Studio Code'u kullanarak IoT hub'ınıza ulaşan iletileri görüntüleyebilirsiniz. 

1. IoT hub'ına gelen verileri izlemek için, üç noktayı (**...**) ve sonra da **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
2. Belirli bir cihazın D2C iletilerini izlemek için listeden cihaza sağ tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
3. Verileri izlemeyi durdurmak için komutu çalıştırmak **Azure IOT Hub: D2C iletisini İzlemeyi Durdur** komut Paleti'nde. 
4. Modül ikizini görüntülemek veya güncelleştirmek için listeden modüle sağ tıklayıp **Edit module twin** (Modül ikizini düzenle) öğesini seçin. Modül ikizini güncelleştirmek için ikiz JSON dosyasını kaydedin, düzenleyici alanına sağ tıklayın ve **Update Module Twin** (Modül İkizini Güncelleştir) öğesini seçin.
5. Docker günlüklerini görüntülemek için VS Code için [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)'ı yükleyin. Çalıştırma modüllerinizi yerel olarak Docker gezgininde bulabilirsiniz. Tümleşik terminalde görüntülemek için bağlam menüsünde **Show Logs** (Günlükleri Göster) öğesine tıklayın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

### <a name="delete-local-resources"></a>Yerel kaynakları silme

Cihazınızdan IoT Edge çalışma zamanını ve ilgili kaynakları kaldırmak isterseniz aşağıdaki komutları kullanabilirsiniz. 

IoT Edge çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur, ancak cihazınızda yer almaya devam eder. Tüm kapsayıcıları görüntüleyin.

   ```bash
   sudo docker ps -a
   ```

Cihazınızda oluşturulan çalışma zamanı kapsayıcılarını silin.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Kapsayıcı adlarına bakarak `docker ps` çıkışında listelenen ek kapsayıcıları silin. 

Kapsayıcı çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir IoT Edge modülü oluşturdunuz. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabileceği diğer yolları öğrenmek için bir sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [Azure işlevlerini modül olarak dağıtma](tutorial-deploy-function.md)
> [Azure Stream Analytics'i modül olarak dağıtma](tutorial-deploy-stream-analytics.md)
