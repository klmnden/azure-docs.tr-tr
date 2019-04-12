---
title: Özel Java modülü Öğreticisi - Azure IOT Edge | Microsoft Docs
description: Bu öğreticide Java koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilir.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: f654f33fe03b29a3aa93386d49e8f5a43cffc9c8
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59493051"
---
# <a name="tutorial-develop-a-java-iot-edge-module-and-deploy-to-your-simulated-device"></a>Öğretici: Bir Java IOT Edge modülü geliştirme ve sanal Cihazınızı dağıtma

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bir sanal cihaza dağıtma Azure IOT Edge'de oluşturduğunuz simülasyon IOT Edge cihazı kullanacağınız [Linux](quickstart-linux.md) hızlı başlangıç. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code ile Azure IoT Edge maven şablon paketini ve Azure IoT Java cihaz SDK'sını temel alan bir IoT Edge Java modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* Bir Azure sanal makinesi için hızlı başlangıç adımları izleyerek bir IOT Edge cihazı kullanabilirsiniz [Linux](quickstart-linux.md). 
* IOT Edge için Java modülleri Windows kapsayıcıları desteği yoktur. 

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/). 
* Visual Studio Code için [Java Uzantı Paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).
* [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) Visual Studio Code için. 
* [Java SE Development Kit 10](https://aka.ms/azure-jdks) ve [`JAVA_HOME` ortam değişkenini](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) JDK yüklemenize göre ayarlama.
* [Maven](https://maven.apache.org/)
* [Docker CE](https://docs.docker.com/install/)
   * Windows cihazında geliştiriyorsanız Docker'ın [Linux kapsayıcılarını kullanacak şekilde yapılandırıldığından](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) emin olun. 


## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu öğreticide, bir modül olarak derlemek ve oluşturmak için Visual Studio Code için Azure IOT araçları kullanmak bir **kapsayıcı görüntüsü** dosyalarından. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Herhangi bir Docker ile uyumlu kayıt defteri, kapsayıcı görüntülerinizi tutmak için kullanabilirsiniz. İki popüler Docker kayıt defteri Hizmetleri [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu öğreticide Azure Container Registry kullanılır. 

Kapsayıcı kayıt defteri zaten yoksa, azure'da yeni bir tane oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

2. Kapsayıcı kayıt defterinizi oluşturmak için aşağıdaki değerleri girin:

   | Alan | Değer | 
   | ----- | ----- |
   | Kayıt defteri adı | Benzersiz bir ad girin. |
   | Abonelik | Açılan listeden bir abonelik seçin. |
   | Kaynak grubu | Daha kolay yönetim için tüm IOT Edge hızlı başlangıçlar ve öğreticilerle sırasında oluşturduğunuz test kaynakları için aynı kaynak grubunu kullanın. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. |
   | Yönetici kullanıcı | **Etkinleştir**'i seçin. |
   | SKU | **Temel**'i seçin. | 

5. **Oluştur**’u seçin.

6. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 

7. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Kapsayıcı kayıt defterine erişim sağlamak için öğreticinin ilerleyen bölümlerinde bu değerleri kullanırsınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlar Azure IOT Edge maven şablon paketi ve Azure IOT Java cihaz SDK'sını dayalı bir IOT Edge modülü projesi oluşturur. Proje, Visual Studio Code ve Azure IOT araçları kullanarak oluşturun.

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir Java çözüm şablonu oluşturun. 

1. Visual Studio Code'da VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 

2. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | Seçin **Java Modülü**. |
   | GroupID için değer sağlayın | Bir grup kimliği değeri girin veya varsayılan değerleri kabul **com.edgemodule**. |
   | Modül adı sağlayın | Modülünüzün adını **JavaModule**. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü, son adımda sağladığınız adından doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/javamodule. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-java-module/repository.png)
   
İlk kez bir Java modülü oluşturuyorsanız maven paketlerinin indirilmesi birkaç dakika sürebilir. Ardından VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Çözüm çalışma alanında beş üst düzey bileşen bulunur. **Modülleri** klasörü, Java kodunu modülünüzde yanı sıra, modül olarak bir kapsayıcı görüntüsü oluşturmak için Docker dosyaları içerir. **\.env** dosyasında kapsayıcı kayıt defterinizin kimlik bilgileri yer alır. **deployment.template.json** dosyasında IoT Edge çalışma zamanının modülleri cihazlara dağıtmak için kullandığı bilgiler bulunur. Ve **deployment.debug.template.json** kapsayıcıları dosyanın modülleri hata ayıklama sürümü. Bu öğreticide **\.vscode** klasörünü veya **\.gitignore** dosyasını düzenlemeyeceksiniz. 

Çözümünüzü oluştururken kapsayıcı kayıt defteri belirtmediyseniz ve varsayılan localhost:5000 değerini kabul ettiyseniz \.env dosyanız olmaz. 

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. 

1. VS Code gezgininde .env dosyasını açın. 
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin. 
3. Bu dosyayı kaydedin. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

1. VS Code gezgininde **modules** > **JavaModule** > **src** > **main** > **java** > **com** > **edgemodule** > **App.java** yolunu izleyin.

5. Yeni başvurulan sınıfları içeri aktarmak için aşağıdaki kodu dosyanın en üstüne ekleyin.

    ```java
    import java.io.StringReader;
    import java.util.concurrent.atomic.AtomicLong;
    import java.util.HashMap;
    import java.util.Map;
    
    import javax.json.Json;
    import javax.json.JsonObject;
    import javax.json.JsonReader;
    
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Pair;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Property;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.TwinPropertyCallBack;
    ```

5. Aşağıdaki tanımı **App** sınıfına ekleyin. Bu değişken bir sıcaklık eşik ayarlar. Bu değerin üzerine ölçeklendirilinceye kadar IOT Hub'ına raporlanan ölçülen makine sıcaklık olmaz. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

7. **MessageCallbackMqtt** öğesinin yürütme metodunu aşağıdaki kodla değiştirin. Modül IoT Edge hub'ından bir MQTT iletisi aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler.

    ```java
    protected static class MessageCallbackMqtt implements MessageCallback {
        private int counter = 0;
        @Override
        public IotHubMessageResult execute(Message msg, Object context) {
            this.counter += 1;
 
            String msgString = new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET);
            System.out.println(
                   String.format("Received message %d: %s",
                            this.counter, msgString));
            if (context instanceof ModuleClient) {
                try (JsonReader jsonReader = Json.createReader(new StringReader(msgString))) {
                    final JsonObject msgObject = jsonReader.readObject();
                    double temperature = msgObject.getJsonObject("machine").getJsonNumber("temperature").doubleValue();
                    long threshold = App.tempThreshold.get();
                    if (temperature >= threshold) {
                        ModuleClient client = (ModuleClient) context;
                        System.out.println(
                            String.format("Temperature above threshold %d. Sending message: %s",
                            threshold, msgString));
                        client.sendEventAsync(msg, eventCallback, msg, App.OUTPUT_NAME);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return IotHubMessageResult.COMPLETE;
        }
    }
    ```

8. Aşağıdaki iki statik iç sınıfı **App** sınıfına ekleyin. Modül ikizinin istenen özellik değişiklikleri bu sınıfların tempThreshold değişkeni güncelleştirin. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
 
    protected static class OnProperty implements TwinPropertyCallBack {
        @Override
        public void TwinPropertyCallBack(Property property, Object context) {
            if (!property.getIsReported()) {
                if (property.getKey().equals(App.TEMP_THRESHOLD)) {
                    try {
                        long threshold = Math.round((double) property.getValue());
                        App.tempThreshold.set(threshold);
                    } catch (Exception e) {
                        System.out.println("Faile to set TemperatureThread with exception");
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```

9. Modül ikizi güncelleştirmelerine abone olmak için aşağıdaki satırları **main** metodunun **client.open()** bölümünün sonrasına ekleyin.

    ```java
    client.startTwin(new DeviceTwinStatusCallBack(), null, new OnProperty(), null);
    Map<Property, Pair<TwinPropertyCallBack, Object>> onDesiredPropertyChange = new HashMap<Property, Pair<TwinPropertyCallBack, Object>>() {
        {
            put(new Property(App.TEMP_THRESHOLD, null), new Pair<TwinPropertyCallBack, Object>(new OnProperty(), null));
        }
    };
    client.subscribeToTwinDesiredProperties(onDesiredPropertyChange);
    client.getTwin();
    ```

11. App.java dosyasını kaydedin.

12. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. Bu dosya dağıtmak için hangi modülü IOT Edge Aracısı bildirir ve IOT Edge hub'ı, bunlar arasında iletileri yönlendirmek anlatır. Bu durumda, iki modüllerdir **tempSensor** ve **JavaModule**. Visual Studio Code uzantısı otomatik olarak dağıtım şablonu gerekir, ancak her şeyi çözümünüz için doğru olduğundan emin olun, ilgili bilgilerin çoğunu doldurur: 

   1. Varsayılan platform, IOT Edge kümesine **amd64** , VS Code durum çubuğunda anlamına gelir, **JavaModule** görüntünün amd64 sürüme Linux ayarlanır. Durum çubuğunda varsayılan platform değiştirme **amd64** için **arm32v7** , IOT Edge cihazınızın mimari ise. 

      ![Modül görüntü platform güncelleştirmesi](./media/tutorial-java-module/image-platform.png)

   2. Şablonda doğru modül adının bulunduğunu ve IoT Edge çözümünü oluştururken değiştirdiğiniz varsayılan **SampleModule** adının kullanılmadığından emin olun.

   3. **RegistryCredentials** bölümü depolar, Docker kayıt defteri kimlik bilgilerini böylece modül görüntünüzü IOT Edge Aracısı çekebilirsiniz. Gerçek kullanıcı adı ve parola çiftleri, git tarafından yoksayılan .env dosyasında depolanır. Henüz yapmadıysanız, kimlik bilgilerinizi .env dosyaya ekleyin.  

   4. Dağıtım bildirimleri hakkında daha fazla bilgi edinmek istiyorsanız bkz [modülleri dağıtma ve IOT Edge'de rotaları oluşturmak hakkında bilgi edinin](module-composition.md).

13. Dağıtım bildirimine **JavaModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin: 

   ```json
     "JavaModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-java-module/module-twin.png)

14. Deployment.template.json dosyayı kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Önceki bölümde, IOT Edge çözümü oluşturulur ve koduna eklemiştir **JavaModule** bildirilen makine sıcaklık kabul edilebilir limiti altında olduğu mesajları filtrelemek için. Artık, bir kapsayıcı görüntüsü olarak Çözümü derleyin ve kapsayıcı kayıt defterinize gönderin. 

1. Visual Studio Code'da terminal aşağıdaki komutu girerek Docker'da oturum açın. Ardından, modül görüntünüzü Azure kapsayıcı kayıt defterinize gönderin.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Birinci bölümde Azure kapsayıcı kayıt defterinizden kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın. Bu değerleri Azure portalındaki kayıt defterinizin **Access keys** bölümünden de alabilirsiniz.

2. VS Code gezgininde deployment.template.json dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve **config** adlı yeni bir klasörde deployment.json dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut kodunuzu derler, Java uygulamasıyla kapsayıcı oluşturur ve kodu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini görebilirsiniz. Görüntü adresi module.json dosyasındaki bilgilerden \<depo\>:\<sürüm\>-\<platform\> biçiminde oluşturulur. Bu öğretici için registryname.azurecr.io/javamodule:0.0.1-amd64 şeklinde olmalıdır.

>[!TIP]
>Oluşturun ve gönderin, modül çalışılırken bir hata alırsanız aşağıdaki denetimleri yapın:
>* Docker kapsayıcı kayıt defterinizin kimlik bilgilerini kullanarak Visual Studio code'da oturum? Bu kimlik bilgilerini Azure portalında oturum açmak için kullandığınız yapılandırılanlardan farklı.
>* Kapsayıcı deponuza doğru mu? Açık **modülleri** > **JavaModule** > **module.json** ve bulma **depo** alan. Görüntü deposu gibi görünmelidir  **\<registryname\>.azurecr.io/javamodule**. 
>* Geliştirme makinenizde çalışan kapsayıcılar aynı türde oluşturuyorsunuz? Visual Studio Code için Linux amd64 kapsayıcıları varsayar. Geliştirme makinenizde çalışan Linux arm32v7 kapsayıcılar, kapsayıcı platformunuzun eşleştirmek için VS Code penceresinin alt kısmındaki mavi durum çubuğu platformunda güncelleştirin.
>* IOT Edge için Java modülleri Windows kapsayıcıları desteği yoktur.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını (eski adıyla Azure IOT Toolkit uzantısını) kullanarak modülleri da dağıtabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildirimi dosyasına (**deployment.json**) zaten sahipsiniz. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

1. VS Code komut paletinde komutu çalıştırmak **Azure: Oturum** ve Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

2. VS Code komut paletini içinde çalıştırma **Azure IOT Hub: IOT hub'ını seçin**. 

3. Yapılandırmak istediğiniz IoT Edge cihazını barındıran aboneliği ve IoT hub'ını seçin. 

4. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

5. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin. 

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-java-module/create-deployment.png)

6. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın. 

7. Yenile düğmesine tıklayın. Yeni **JavaModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir.  

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır. 

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin. 

IOT Edge cihazında komutunu kullanarak dağıtım modüllerinizi durumunu görebilirsiniz `iotedge list`. Dört modül görmeniz gerekir: İki IoT Edge çalışma zamanı modülü, tempSensor ve bu öğreticide oluşturduğunuz özel modül. Tüm modüllerin başlatılması birkaç dakika sürebilir. Bu nedenle tümünü görmüyorsanız komutu yeniden çalıştırın. 

Modüller tarafından oluşturulan iletileri görüntülemek için `iotedge logs <module name>` komutunu kullanın. 

Visual Studio Code'u kullanarak IoT hub'ınıza ulaşan iletileri görüntüleyebilirsiniz. 

1. IoT hub'ına gelen verileri izlemek için, üç noktayı (**...**) ve sonra da **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
2. Belirli bir cihazın D2C iletilerini izlemek için listeden cihaza sağ tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
3. Verileri izlemeyi durdurmak için komutu çalıştırmak **Azure IOT Hub: D2C iletisini İzlemeyi Durdur** komut Paleti'nde. 
4. Modül ikizini görüntülemek veya güncelleştirmek için listeden modüle sağ tıklayıp **Edit module twin** (Modül ikizini düzenle) öğesini seçin. Modül ikizini güncelleştirmek için ikiz JSON dosyasını kaydedin, düzenleyici alanına sağ tıklayın ve **Update Module Twin** (Modül İkizini Güncelleştir) öğesini seçin.
5. Docker günlüklerini görüntülemek için VS Code için [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)'ı yükleyin. Çalıştırma modüllerinizi yerel olarak Docker gezgininde bulabilirsiniz. Tümleşik terminalde görüntülemek için bağlam menüsünde **Show Logs** (Günlükleri Göster) öğesini seçin.
 
## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir IoT Edge modülü oluşturdunuz. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabileceği diğer yolları öğrenmek için bir sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [SQL Server veritabanları ile uçta veri Store](tutorial-store-data-sql-server.md)

