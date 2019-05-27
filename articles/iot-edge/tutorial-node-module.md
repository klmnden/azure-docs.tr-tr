---
title: Öğretici geliştirme Node.js modülünü Linux - Azure IOT Edge | Microsoft Docs
description: Bu öğreticide Node.js koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilmektedir
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 5147a0d164f1fac67a1c70429fc2e9b1caa00685
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244831"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-for-linux-devices"></a>Öğretici: Linux cihazlar için bir Node.js IOT Edge modülü geliştirme ve dağıtma

Visual Studio Code Node.js kodu geliştirmek ve Azure IOT Edge çalıştıran bir Linux cihazına dağıtmak için kullanın. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Hızlı başlangıçlarda oluşturduğunuz sanal Azure IoT Edge cihazını kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak IoT Edge Node.js modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğreticide bir modülde nasıl geliştirilebileceğini gösterir **Node.js** kullanarak **Visual Studio Code**ve dağıtmak nasıl bir **Linux cihaz**. IOT Edge, Windows cihazları için Node.js modüllerini desteklemez.

Geliştirme ve dağıtma Node.js modüllerini seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın: 

| Node.js | Visual Studio Code | Visual Studio 2017 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![VS Code için Linux AMD64 üzerinde Node.js modüllerini kullanma](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![VS Code için Linux ARM32 üzerinde Node.js modüllerini kullanma](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce Linux kapsayıcı geliştirme için geliştirme ortamınızı ayarlamak için önceki öğreticide çalıştınız: [IOT Edge modülleri Linux cihazlar için geliştirme](tutorial-develop-for-linux.md). Ya da bu öğreticileri izleyerek aşağıdaki önkoşulların yerinde olmalıdır: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Azure IOT Edge çalıştıran Linux cihaz](quickstart-linux.md)
* Kapsayıcı kayıt defteri gibi [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) ile yapılandırılmış [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak üzere yapılandırılmış.

Node.js'de bir IOT Edge modülü geliştirme için geliştirme makinenizde aşağıdaki ek önkoşulları yükleyin: 

* [Node.js ve npm](https://nodejs.org). npm paketi, Node.js ile birlikte dağıtılır. Başka bir deyişle Node.js'yi indirdiğinizde npm de bilgisayarınıza otomatik olarak yüklenir.

## <a name="create-a-module-project"></a>Bir modülü projesi oluşturma
Aşağıdaki adımlar Visual Studio Code ve Azure IOT araçları kullanarak bir IOT Edge Node.js modülünü nasıl oluşturulacağını gösterir.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturun

Temel olarak kullanabileceğiniz bir Node.js çözüm şablonu oluşturmak için **npm** kullanın. 

1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Tümleşik Terminal**'i seçin.

2. Tümleşik terminalde **yeoman** ve Node.js Azure IoT Edge modülü oluşturucuyu yüklemek için aşağıdaki komutu girin: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. VS Code komut paletini açmak için **View (Görünüm)**  > **Command Palette (Komut Paleti)** öğesini seçin. 

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


### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı deponuzun kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. 

1. VS Code gezgininde **.env** dosyasını açın. 
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin. 
3. Bu dosyayı kaydedin. 

### <a name="select-your-target-architecture"></a>Hedef Mimarinizi seçin

Şu anda, Visual Studio Code Node.js modüllerini Linux AMD64 ve Linux ARM32v7 cihazlar için geliştirebilirsiniz. Kapsayıcı oluşturulur ve her bir mimari türü için farklı çalıştır olduğundan, her bir çözüm ile hedeflediğiniz hangi mimari seçmeniz gerekir. Linux AMD64 varsayılandır. 

1. Komut paletini açın ve arama **Azure IOT Edge: Varsayılan hedef Platform için Edge çözümü ayarlayın**, veya pencerenin alt kısmındaki kenar çubuğu kısayol simgesini seçin. 

2. Komut paletini hedef mimari seçeneklerini listeden seçin. Bu öğreticide, bir Ubuntu sanal makinesi varsayılan tutacak şekilde IOT Edge cihazı kullandığımız **amd64**.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Her şablonda, **tempSensor** modülündeki sensör simülasyon verilerini alıp IoT Hub'a yönlendiren örnek kod bulunur. Bu bölümde NodeModule modülünün verileri göndermeden önce analiz etmesi için gerekli kodu ekleyin. 

1. VS Code gezgininde **modules** > **NodeModule** > **app.js** girişini açın.

2. Gerekli düğüm modülleri altına bir sıcaklık eşik değeri girin. Sıcaklık eşiği, verilerin IoT Hub’a gönderilmesi için, ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```javascript
    var temperatureThreshold = 25;
    ```

3. `PipeMessage` işlevinin tamamını `FilterMessage` işleviyle değiştirin.
    
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

4. `pipeMessage` işlev adını `client.on()` işlevindeki `filterMessage` ile değiştirin.

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

5. Aşağıdaki kod parçacığını `client.open()` geri çağırma işlevinin içine, `else` deyiminin içindeki `client.on()` sonrasına kopyalayın. İstenen özellikler güncelleştirildiğinde bu işlev çağrılır.

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

6. App.js dosyayı kaydedin.

7. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın.

8. Dağıtım bildirimine NodeModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin: 

   ```json
     "NodeModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-node-module/module-twin.png)

9. Deployment.template.json dosyayı kaydedin.


## <a name="build-and-push-your-module"></a>Oluşturun ve modülünüzde gönderin

Önceki bölümde, IOT Edge çözümünü oluşturan ve iletileri kabul edilebilir sınırlar içinde bildirilen makine sıcaklık olduğu filtre NodeModule kod eklenir. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

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

4. Yenile düğmesine tıklayın. Yeni **NodeModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code Gezgininde, IOT Edge cihazınızın adına sağ tıklayıp **Başlat yerleşik olay uç nokta izleme**.

2. IOT hub'da gelen iletileri görüntüleyin. IOT Edge cihazı, yeni dağıtım almak ve tüm modülleri başlatmak olduğundan bunu ulaşması iletileri için biraz zaman alabilir. Daha sonra iletileri göndermeden önce 25 derece makine sıcaklık ulaşana kadar biz NodeModule koda yapılan değişiklikleri bekleyin. İleti türü de ekler **uyarı** sıcaklık eşiğe ulaşan tüm iletileri. 

## <a name="edit-the-module-twin"></a>Modül ikizi Düzenle

Biz NodeModule modül ikizi dağıtım bildiriminde sıcaklık eşik 25 derecede ayarlamak için kullanılır. Modül ikizi modülü kodunu güncelleştirmek zorunda kalmadan işlevlerini değiştirmek için kullanabilirsiniz.

1. Visual Studio Code'da altında çalışan modülleri görmek için IOT Edge cihaz ayrıntıları genişletin. 

2. Sağ **NodeModule** seçip **düzenleme modül ikizi**. 

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
