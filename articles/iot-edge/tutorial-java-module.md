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
ms.openlocfilehash: a105e59007543ffaf31b586707390954643e8bee
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239642"
---
# <a name="tutorial-develop-a-java-iot-edge-module-for-linux-devices"></a>Öğretici: Linux cihazlar için bir Java IOT Edge modülü geliştirme

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bir sanal cihaza dağıtma Azure IOT Edge'de oluşturduğunuz simülasyon IOT Edge cihazı kullanacağınız [Linux](quickstart-linux.md) hızlı başlangıç. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code ile Azure IoT Edge maven şablon paketini ve Azure IoT Java cihaz SDK'sını temel alan bir IoT Edge Java modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğreticide bir modülde nasıl geliştirilebileceğini gösterir **Java** kullanarak **Visual Studio Code**ve dağıtmak nasıl bir **Linux cihaz**. IOT Edge, Windows cihazları için Java modülleri desteklemez. 

Geliştirme ve Java modülleri dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın: 

| Java | Visual Studio Code | Visual Studio 2017 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Linux AMD64 üzerinde Java modüller için VS Code kullanma](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![Linux ARM32 üzerinde Java modüller için VS Code kullanma](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce Linux kapsayıcı geliştirme için geliştirme ortamınızı ayarlamak için önceki öğreticide çalıştınız: [IOT Edge modülleri Linux cihazlar için geliştirme](tutorial-develop-for-linux.md). Ya da bu öğreticileri izleyerek aşağıdaki önkoşulların yerinde olmalıdır: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Azure IOT Edge çalıştıran Linux cihaz](quickstart-linux.md)
* Kapsayıcı kayıt defteri gibi [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) ile yapılandırılmış [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak üzere yapılandırılmış.

Java'da bir IOT Edge modülü geliştirme için geliştirme makinenizde aşağıdaki ek önkoşulları yükleyin: 

* Visual Studio Code için [Java Uzantı Paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).
* [Java SE Development Kit 10](https://aka.ms/azure-jdks) ve [`JAVA_HOME` ortam değişkenini](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) JDK yüklemenize göre ayarlama.
* [Maven](https://maven.apache.org/)

## <a name="create-a-module-project"></a>Bir modülü projesi oluşturma
Aşağıdaki adımlar Azure IOT Edge maven şablon paketi ve Azure IOT Java cihaz SDK'sını dayalı bir IOT Edge modülü projesi oluşturur. Proje, Visual Studio Code ve Azure IOT araçları kullanarak oluşturun.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturun

Kendi yazacağınız kodla özelleştirebileceğiniz bir Java çözüm şablonu oluşturun. 

1. Visual Studio Code'da VS Code komut paletini açmak için **View (Görünüm)**  > **Command Palette (Komut Paleti)** öğesini seçin. 

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

### <a name="select-your-target-architecture"></a>Hedef Mimarinizi seçin

Şu anda, Visual Studio Code Linux AMD64 ve Linux ARM32v7 cihazlar için Java modülleri geliştirebilirsiniz. Kapsayıcı oluşturulur ve her bir mimari türü için farklı çalıştır olduğundan, her bir çözüm ile hedeflediğiniz hangi mimari seçmeniz gerekir. Linux AMD64 varsayılandır. 

1. Komut paletini açın ve arama **Azure IOT Edge: Varsayılan hedef Platform için Edge çözümü ayarlayın**, veya pencerenin alt kısmındaki kenar çubuğu kısayol simgesini seçin. 

2. Komut paletini hedef mimari seçeneklerini listeden seçin. Bu öğreticide, bir Ubuntu sanal makinesi varsayılan tutacak şekilde IOT Edge cihazı kullandığımız **amd64**. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

1. VS Code gezgininde **modules** > **JavaModule** > **src** > **main** > **java** > **com** > **edgemodule** > **App.java** yolunu izleyin.

2. Yeni başvurulan sınıfları içeri aktarmak için aşağıdaki kodu dosyanın en üstüne ekleyin.

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

3. Aşağıdaki tanımı **App** sınıfına ekleyin. Bu değişken bir sıcaklık eşik ayarlar. Bu değerin üzerine ölçeklendirilinceye kadar IOT Hub'ına raporlanan ölçülen makine sıcaklık olmaz. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

4. **MessageCallbackMqtt** öğesinin yürütme metodunu aşağıdaki kodla değiştirin. Modül IoT Edge hub'ından bir MQTT iletisi aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler.

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

5. Aşağıdaki iki statik iç sınıfı **App** sınıfına ekleyin. Modül ikizinin istenen özellik değişiklikleri bu sınıfların tempThreshold değişkeni güncelleştirin. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

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

6. Modül ikizi güncelleştirmelerine abone olmak için aşağıdaki satırları **main** metodunun **client.open()** bölümünün sonrasına ekleyin.

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

7. App.java dosyasını kaydedin.

8. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın.

9. Dağıtım bildirimine **JavaModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin: 

   ```json
     "JavaModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-java-module/module-twin.png)

10. Deployment.template.json dosyayı kaydedin.

## <a name="build-and-push-your-module"></a>Oluşturun ve modülünüzde gönderin

Önceki bölümde, IOT Edge çözümü oluşturulur ve koduna eklemiştir **JavaModule** bildirilen makine sıcaklık kabul edilebilir limiti altında olduğu mesajları filtrelemek için. Artık, bir kapsayıcı görüntüsü olarak Çözümü derleyin ve kapsayıcı kayıt defterinize gönderin. 

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

4. Yenile düğmesine tıklayın. Yeni **JavaModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir.  

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code Gezgininde, IOT Edge cihazınızın adına sağ tıklayıp **Başlat yerleşik olay uç nokta izleme**.

2. IOT hub'da gelen iletileri görüntüleyin. IOT Edge cihazı, yeni dağıtım almak ve tüm modülleri başlatmak olduğundan bunu ulaşması iletileri için biraz zaman alabilir. Daha sonra iletileri göndermeden önce 25 derece makine sıcaklık ulaşana kadar biz JavaModule koda yapılan değişiklikleri bekleyin. İleti türü de ekler **uyarı** sıcaklık eşiğe ulaşan tüm iletileri. 

## <a name="edit-the-module-twin"></a>Modül ikizi Düzenle

Biz JavaModule modül ikizi dağıtım bildiriminde sıcaklık eşik 25 derecede ayarlamak için kullanılır. Modül ikizi modülü kodunu güncelleştirmek zorunda kalmadan işlevlerini değiştirmek için kullanabilirsiniz.

1. Visual Studio Code'da altında çalışan modülleri görmek için IOT Edge cihaz ayrıntıları genişletin. 

2. Sağ **JavaModule** seçip **düzenleme modül ikizi**. 

3. Bulma **TemperatureThreshold** istenen özellikleri. Değeri 5 10 derece son bildirilen sıcaklık daha yüksek derece için yeni bir sıcaklık değiştirin. 

4. Modül ikizi dosyayı kaydedin.

5. Modül ikizi düzenleme bölmesinde ve seçme içinde herhangi bir yeri sağ **güncelleştirme modül ikizi**. 

6. CİHAZDAN buluta gelen iletileri izleyin. Yeni sıcaklık eşiği ulaşılana kadar Durdur iletileri görmeniz gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi derlemek için hazır olduğunuzda, daha fazla bilgi edinebilirsiniz [kendi IOT Edge modülleri geliştirmek](module-development.md) veya nasıl [modülleri Visual Studio Code ile geliştirme](how-to-vs-code-develop-module.md). Azure IOT Edge, uçta verilerini işlemek ve çözümlemek için Azure bulut Hizmetleri dağıtmayı nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticiler açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [İşlevleri](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [makine öğrenimi](tutorial-deploy-machine-learning.md)
> [özel görüntü işleme hizmeti](tutorial-deploy-custom-vision.md)
