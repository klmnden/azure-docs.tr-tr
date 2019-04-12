---
title: Öğretici, Azure IOT Edge özel Node.js modülünü - oluşturma | Microsoft Docs
description: Bu öğreticide Node.js koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilmektedir
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 3b79c75b9846a4f8966a113c6e06fabc25bcf011
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59491175"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-to-your-simulated-device"></a>Öğretici: Geliştirme ve sanal cihazınız için bir Node.js IOT Edge modülü dağıtma

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Hızlı başlangıçlarda oluşturduğunuz sanal Azure IoT Edge cihazını kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak IoT Edge Node.js modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.
* IOT Edge için node.js modüllerini Windows kapsayıcıları desteği yoktur. 

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) Visual Studio Code için. 
* [Docker CE](https://docs.docker.com/engine/installation/). 
   * Windows cihazında geliştiriyorsanız Docker'ın [Linux kapsayıcılarını kullanacak şekilde yapılandırıldığından](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) emin olun. 
* [Node.js ve npm](https://nodejs.org). npm paketi, Node.js ile birlikte dağıtılır. Başka bir deyişle Node.js'yi indirdiğinizde npm de bilgisayarınıza otomatik olarak yüklenir.

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
   | Kaynak grubu | IoT Edge hızlı başlangıçlarında ve öğreticilerinde oluşturduğunuz tüm test kaynakları için aynı kaynak grubunu kullanmanızı öneririz. Örneğin, **IoTEdgeResources**. |
   | Konum | Size yakın bir konum seçin. |
   | Yönetici kullanıcı | **Etkinleştir**'i seçin. |
   | SKU | **Temel**'i seçin. |

5. **Oluştur**’u seçin.

6. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 

7. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Kapsayıcı kayıt defterine erişim sağlamak için öğreticinin ilerleyen bölümlerinde bu değerleri kullanırsınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlar Visual Studio Code ve Azure IOT araçları kullanarak bir IOT Edge Node.js modülünü nasıl oluşturulacağını gösterir.

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Temel olarak kullanabileceğiniz bir Node.js çözüm şablonu oluşturmak için **npm** kullanın. 

1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Tümleşik Terminal**'i seçin.

2. Tümleşik terminalde **yeoman** ve Node.js Azure IoT Edge modülü oluşturucuyu yüklemek için aşağıdaki komutu girin: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 

3. Yazın ve şu komutu çalıştırın komut Paleti'nde **Azure: Oturum** ve Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

4. Yazın ve şu komutu çalıştırın komut Paleti'nde **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | Seçin **Node.js modülünü**. |
   | Modül adı sağlayın | Modülünüze **NodeModule** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü, son adımda sağladığınız adından doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/nodemodule. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-node-module/repository.png)

VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Çözüm çalışma alanında beş üst düzey bileşen bulunur. **modules** klasöründe modülünüzün Node.js kodunun yanı sıra modülünüzden kapsayıcı görüntüsü oluşturmak için kullanılacak Dockerfiles öğeleri bulunur. **\.env** dosyasında kapsayıcı kayıt defterinizin kimlik bilgileri yer alır. **Deployment.template.json** dosya IOT Edge çalışma zamanı bir cihazda modüllerini dağıtmak için kullandığı bilgileri içerir ve **deployment.debug.template.json** dosyası hata ayıklama sürümünü içerir modüller. Bu öğreticide **\.vscode** klasörünü veya **\.gitignore** dosyasını düzenlemeyeceksiniz. 

Çözümünüzü oluştururken kapsayıcı kayıt defteri belirtmediyseniz ve varsayılan localhost:5000 değerini kabul ettiyseniz \.env dosyanız olmaz. 

<!--
   ![Node.js solution workspace](./media/tutorial-node-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı deponuzun kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. 

1. VS Code gezgininde **.env** dosyasını açın. 
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin. 
3. Bu dosyayı kaydedin. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Her şablonda, **tempSensor** modülündeki sensör simülasyon verilerini alıp IoT Hub'a yönlendiren örnek kod bulunur. Bu bölümde NodeModule modülünün verileri göndermeden önce analiz etmesi için gerekli kodu ekleyin. 

1. VS Code gezgininde **modules** > **NodeModule** > **app.js** girişini açın.

5. Gerekli düğüm modülleri altına bir sıcaklık eşik değeri girin. Sıcaklık eşiği, verilerin IoT Hub’a gönderilmesi için, ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```javascript
    var temperatureThreshold = 25;
    ```

6. `PipeMessage` işlevinin tamamını `FilterMessage` işleviyle değiştirin.
    
    ```javascript
    // This function filters out messages that report temperatures below the temperature threshold.
    // It also adds the MessageType property to the message with the value set to Alert.
    function filterMessage(client, inputName, msg) {
        client.complete(msg, printResultFor('Receiving message'));
        if (inputName === 'input1') {
            var message = msg.getBytes().toString('utf8');
            var messageBody = JSON.parse(message);
            if (messageBody && messageBody.machine && messageBody.machine.temperature && messageBody.machine.temperature > temperatureThreshold) {
                console.log(`Machine temperature ${messageBody.machine.temperature} exceeds threshold ${temperatureThreshold}`);
                var outputMsg = new Message(message);
                outputMsg.properties.add('MessageType', 'Alert');
                client.sendOutputEvent('output1', outputMsg, printResultFor('Sending received message'));
            }
        }
    }

    ```

7. `pipeMessage` işlev adını `client.on()` işlevindeki `filterMessage` ile değiştirin.

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

8. Aşağıdaki kod parçacığını `client.open()` geri çağırma işlevinin içine, `else` deyiminin içindeki `client.on()` sonrasına kopyalayın. İstenen özellikler güncelleştirildiğinde bu işlev çağrılır.

    ```javascript
    client.getTwin(function (err, twin) {
        if (err) {
            console.error('Error getting twin: ' + err.message);
        } else {
            twin.on('properties.desired', function(delta) {
                if (delta.TemperatureThreshold) {
                    temperatureThreshold = delta.TemperatureThreshold;
                }
            });
        }
    });
    ```

9. App.js dosyayı kaydedin.

10. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. Bu dosya, bu durumda dağıtmak için hangi modülü IOT Edge Aracısı söyler **tempSensor** ve **NodeModule**ve bunlar arasında iletileri yönlendirme hakkında IOT Edge hub'ı söyler. Visual Studio Code uzantısı otomatik olarak dağıtım şablonu gerekir, ancak her şeyi çözümünüz için doğru olduğundan emin olun, ilgili bilgilerin çoğunu doldurur: 

   1. Varsayılan platform, IOT Edge kümesine **amd64** , VS Code durum çubuğunda anlamına gelir, **NodeModule** görüntünün amd64 sürüme Linux ayarlanır. Durum çubuğunda varsayılan platform değiştirme **amd64** için **arm32v7** , IOT Edge cihazınızın mimari ise. 

      ![Modül görüntü platform güncelleştirmesi](./media/tutorial-node-module/image-platform.png)

   2. Şablonda doğru modül adının bulunduğunu ve IoT Edge çözümünü oluştururken değiştirdiğiniz varsayılan **SampleModule** adının kullanılmadığından emin olun.

   3. **RegistryCredentials** bölümü depolar, Docker kayıt defteri kimlik bilgilerini böylece modül görüntünüzü IOT Edge Aracısı çekebilirsiniz. Gerçek kullanıcı adı ve parola çiftleri, git tarafından yoksayılan .env dosyasında depolanır. Henüz yapmadıysanız, kimlik bilgilerinizi .env dosyaya ekleyin.  

   4. Dağıtım bildirimleri hakkında daha fazla bilgi edinmek istiyorsanız bkz [modülleri dağıtma ve IOT Edge'de rotaları oluşturmak hakkında bilgi edinin](module-composition.md).

11. Dağıtım bildirimine NodeModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin: 

   ```json
     "NodeModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-node-module/module-twin.png)

12. Deployment.template.json dosyayı kaydedin.


## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve NodeModule modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtreleyecek kodu eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor. 

1. Modül görüntüsünü ACR'ye gönderebilmek için Visual Studio Code tümleşik terminaline aşağıdaki komutu girerek Docker oturumu açın: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Birinci bölümde Azure Container Registry'den kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın. Veya Azure portalındaki kaydınızın **Erişim anahtarları** bölümünden tekrar alın.

2. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve yeni bir **config** klasöründe bir `deployment.json` dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut, kodunuzu derlemek, Node.js kodunuzu kapsayıcılı hale getirme ve ardından çözümü hazırlarken belirttiğiniz kapsayıcı kayıt defterine gönderin. 

VS Code tümleşik terminalinde çalışan `docker build` komutunda tam kapsayıcı görüntüsü adresini ve etiketi görebilirsiniz. Görüntü adresi `module.json` dosyasından **\<depo\>:\<sürüm\>-\<platform\>** biçiminde derlenir. Bu öğretici için **registryname.azurecr.io/nodemodule:0.0.1-amd64** şeklinde olmalıdır.

>[!TIP]
>Oluşturun ve gönderin, modül çalışılırken bir hata alırsanız aşağıdaki denetimleri yapın:
>* Docker kapsayıcı kayıt defterinizin kimlik bilgilerini kullanarak Visual Studio code'da oturum? Bu kimlik bilgilerini Azure portalında oturum açmak için kullandığınız yapılandırılanlardan farklı.
>* Kapsayıcı deponuza doğru mu? Açık **modülleri** > **nodemodule** > **module.json** ve bulma **depo** alan. Görüntü deposu gibi görünmelidir  **\<registryname\>.azurecr.io/nodemodule**. 
>* Geliştirme makinenizde çalışan kapsayıcılar aynı türde oluşturuyorsunuz? Visual Studio Code için Linux amd64 kapsayıcıları varsayar. Geliştirme makinenizde çalışan Linux arm32v7 kapsayıcılar, kapsayıcı platformunuzun eşleştirmek için VS Code penceresinin alt kısmındaki mavi durum çubuğu platformunda güncelleştirin.
>* IOT Edge için node.js modüllerini Windows kapsayıcıları desteği yoktur.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını (eski adıyla Azure IOT Toolkit uzantısını) kullanarak modülleri da dağıtabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildirimi dosyasına (**deployment.json**) zaten sahipsiniz. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

1. VS Code komut paletini içinde çalıştırma **Azure IOT Hub: IOT hub'ını seçin**. 

2. Yapılandırmak istediğiniz IoT Edge cihazını barındıran aboneliği ve IoT hub'ını seçin. 

3. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

4. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin. 

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-node-module/create-deployment.png)

5. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın. 

6. Yenile düğmesine tıklayın. Yeni **NodeModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir. 


## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır. 

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin. 

IoT Edge cihazında `iotedge list` komutunu kullanarak dağıtım modüllerinin durumunu görebilirsiniz. Dört modül görmeniz gerekir: İki IoT Edge çalışma zamanı modülü, tempSensor ve bu öğreticide oluşturduğunuz özel modül. Tüm modüllerin başlatılması birkaç dakika sürebilir. Bu nedenle tümünü görmüyorsanız komutu yeniden çalıştırın. 

Modüller tarafından oluşturulan iletileri görüntülemek için `iotedge logs <module name>` komutunu kullanın. 

Visual Studio Code'u kullanarak IoT hub'ınıza ulaşan iletileri görüntüleyebilirsiniz. 

1. IoT hub'a gelen verileri izlemek için **...** simgesine tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
2. Belirli bir cihazın D2C iletilerini izlemek için listeden cihaza sağ tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
3. Verileri izlemeyi durdurmak için komutu çalıştırmak **Azure IOT Hub: D2C iletisini İzlemeyi Durdur** komut paleti içinde. 
4. Modül ikizini görüntülemek veya güncelleştirmek için listeden modüle sağ tıklayıp **Edit module twin** (Modül ikizini düzenle) öğesini seçin. Modül ikizini güncelleştirmek için ikiz JSON dosyasını kaydedip düzenleyici alanına sağ tıklayın ve **Update Module Twin** (Modül İkizini Güncelleştir) öğesini seçin.
5. Docker günlüklerini görüntülemek isterseniz VS Code için [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) uygulamasını yükleyebilir ve çalışan modüllerinizi Docker gezgininde yerel olarak bulabilirsiniz. Tümleşik terminalde görüntülemek için bağlam menüsünde **Show Logs** (Günlükleri Göster) öğesine tıklayın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Azure IoT Edge'in verileri iş içgörüsüne dönüştürmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için aşağıdaki öğreticilerden birine devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Function'ı modül olarak dağıtma](tutorial-deploy-function.md)
> [Azure Stream Analytics'i modül olarak dağıtma](tutorial-deploy-stream-analytics.md)

