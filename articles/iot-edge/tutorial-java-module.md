---
title: Azure IoT Edge Java öğreticisi | Microsoft Docs
description: Bu öğreticide Java koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilir.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: d5201f3b2a0a3548b1f9eaf4a3f6c6b0fe160d18
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567715"
---
# <a name="tutorial-develop-a-java-iot-edge-module-and-deploy-to-your-simulated-device"></a>Öğretici: Java IoT Edge modülü geliştirme ve simülasyon cihazınıza dağıtma

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. [Windows](quickstart.md)'da veya [Linux](quickstart-linux.md)'ta sanal bir cihaza Azure IoT Edge dağıtma hızlı başlangıçlarında oluşturduğunuz sanal IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code ile Azure IoT Edge maven şablon paketini ve Azure IoT Java cihaz SDK'sını temel alan bir IoT Edge Java modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.
* Java IoT Edge modülleri Windows cihazlarını desteklemez.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/). 
* Visual Studio Code için [Java Uzantı Paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).
* Visual Studio Code için [Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Java SE Development Kit 10](https://aka.ms/azure-jdks) ve [`JAVA_HOME` ortam değişkenini](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) JDK yüklemenize göre ayarlama.
* [Maven](https://maven.apache.org/)
* [Docker CE](https://docs.docker.com/install/)
   * Windows cihazında geliştiriyorsanız Docker'ın [Linux kapsayıcılarını kullanacak şekilde yapılandırıldığından](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) emin olun. 


## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry**'yi seçin.
2. Kayıt defterinize bir ad verin, abonelik seçin, kaynak grubu seçin ve SKU'yu **Temel** olarak ayarlayın. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Bu değerleri öğreticide ileride Docker görüntüsünü kayıt defterinizde yayımlamak ve kayıt defteri kimlik bilgilerini Azure IoT Edge çalışma zamanına eklemek için kullanırsınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code ve Azure IoT Edge uzantısı kullanılarak Azure IoT Edge maven şablon paketi ve Azure IoT Java cihazına dayalı bir IoT Edge modülü projesi oluşturulur.

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir Java çözüm şablonu oluşturun. 

1. Visual Studio Code'da VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 

2. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 

   1. Çözümü oluşturmak istediğiniz klasörü seçin. 
   2. Çözümünüz için bir ad girin veya varsayılan **EdgeSolution** adını kabul edin.
   3. Modül şablonu olarak **Java Module** girişini seçin. 
   4. groupId için bir değer girin veya varsayılan **com.edgemodule** değerini kabul edin.
   5. Varsayılan modül adını **JavaModule** olarak değiştirin. 
   6. İlk modülünüz için görüntü deposu olarak önceki bölümde oluşturduğunuz Azure Container Registry bileşenini belirtin. **localhost:5000** yerine kopyaladığınız oturum açma sunucusu değerini yazın. Dizenin son hali \<kayıt adı\>.azurecr.io/javamodule ifadesine benzer olmalıdır.


İlk kez bir Java modülü oluşturuyorsanız maven paketlerinin indirilmesi birkaç dakika sürebilir. Ardından VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Çözüm çalışma alanında beş üst düzey bileşen bulunur. Bu öğreticide **\.vscode** klasörünü veya **\.gitignore** dosyasını düzenlemeyeceksiniz. **modules** klasöründe modülünüzün Java kodunun yanı sıra modülünüzden kapsayıcı görüntüsü oluşturmak için kullanılacak Dockerfiles öğeleri bulunur. **\.env** dosyasında kapsayıcı kayıt defterinizin kimlik bilgileri yer alır. **deployment.template.json** dosyasında IoT Edge çalışma zamanının modülleri cihazlara dağıtmak için kullandığı bilgiler bulunur. 

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

5. Aşağıdaki tanımı **App** sınıfına ekleyin. Bu değişken, IoT Hub'a verilerin gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

7. **MessageCallbackMqtt** öğesinin yürütme metodunu aşağıdaki kodla değiştirin. Modül IoT Edge hub'ından bir MQTT iletisi aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler.

    ```java
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
    ```

8. Aşağıdaki iki statik iç sınıfı **App** sınıfına ekleyin. Bu sınıflar modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **tempThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

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

11. Bu dosyayı kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve **JavaModule** modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtreleyen kodu eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor. 

1. Visual Studio Code tümleşik terminaline aşağıdaki komutu girerek Docker’da oturum açın. Ardından, modül görüntünüzü Azure kapsayıcı kayıt defterinize gönderin.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Birinci bölümde Azure kapsayıcı kayıt defterinizden kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın. Bu değerleri Azure portalındaki kayıt defterinizin **Access keys** bölümünden de alabilirsiniz.

2. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki deployment.template.json dosyasını açın. Bu dosya **$edgeAgent** aracısına iki modülü dağıtmasını söyler: **tempSensor** ve **JavaModule**. **JavaModule.image** değeri, görüntünün Linux amd64 sürümüne göre ayarlanmıştır. IoT Edge cihazınızın mimarisine göre görüntü sürümünü **arm32v7** olarak değiştirebilirsiniz. 

   Şablonda doğru modül adının bulunduğunu ve IoT Edge çözümünü oluştururken değiştirdiğiniz varsayılan **SampleModule** adının kullanılmadığından emin olun.

   Dağıtım bildirimleri hakkında daha fazla bilgi edinmek için bkz. [IoT Edge modüllerinin kullanılmasını, yapılandırılmasını ve yeniden kullanılmasını anlama](module-composition.md).

3. Deployment.template.json dosyasının **registryCredentials** bölümünde Docker kayıt defteri kimlik bilgileriniz depolanır. Gerçek kullanıcı adı ve parola çiftleri, git tarafından yoksayılan .env dosyasında depolanır.  

4. Dağıtım bildirimine **JavaModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin: 
    ```json
        "JavaModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Bu dosyayı kaydedin.

5. VS Code gezgininde deployment.template.json dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve **config** adlı yeni bir klasörde deployment.json dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut kodunuzu derler, Java uygulamasıyla kapsayıcı oluşturur ve kodu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini görebilirsiniz. Görüntü adresi module.json dosyasındaki bilgilerden \<depo\>:\<sürüm\>-\<platform\> biçiminde oluşturulur. Bu öğretici için registryname.azurecr.io/javamodule:0.0.1-amd64 şeklinde olmalıdır.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Modülleri dağıtmak için Visual Studio Code Azure IoT Araç Seti eklentisini de kullanabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildirimi dosyasına (**deployment.json**) zaten sahipsiniz. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

1. VS Code komut paletinde **Azure: Sign in** komutunu çalıştırdıktan sonra yönergeleri izleyerek Azure hesabınızda oturum açın. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

2. VS Code komut paletinde **Azure IoT Hub: Select IoT Hub** komutunu çalıştırın. 

3. Yapılandırmak istediğiniz IoT Edge cihazını barındıran aboneliği ve IoT hub'ını seçin. 

4. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

5. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin. 

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-java-module/create-deployment.png)

6. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın. 

7. Yenile düğmesine tıklayın. Yeni **JavaModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir.  

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır. 

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin. 

IoT Edge cihazında `iotedge list` komutunu kullanarak dağıtım modüllerinin durumunu görebilirsiniz. Dört modül görmeniz gerekir: İki IoT Edge çalışma zamanı modülü, tempSensor ve bu öğreticide oluşturduğunuz özel modül. Tüm modüllerin başlatılması birkaç dakika sürebilir. Bu nedenle tümünü görmüyorsanız komutu yeniden çalıştırın. 

Modüller tarafından oluşturulan iletileri görüntülemek için `iotedge logs <module name>` komutunu kullanın. 

Visual Studio Code'u kullanarak IoT hub'ınıza ulaşan iletileri görüntüleyebilirsiniz. 

1. IoT hub'ına gelen verileri izlemek için, üç noktayı (**...**) ve sonra da **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
2. Belirli bir cihazın D2C iletilerini izlemek için listeden cihaza sağ tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
3. Verileri izlemeyi durdurmak için komut paletinden **Azure IoT Hub: Stop monitoring D2C message** komutunu seçin. 
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
> [SQL Server veritabanları ile uç cihazlarda veri depolama](tutorial-store-data-sql-server.md)

