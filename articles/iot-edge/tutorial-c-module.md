---
title: Öğretici geliştirme C modülü Linux - Azure IOT Edge | Microsoft Docs
description: Bu öğretici, C kodu ile bir IOT Edge modülü oluşturun ve IOT Edge çalıştıran bir Linux cihazına dağıtmak nasıl gösterir
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: f5f6b6473a3cbe078ae2357e1f908788e54af25d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64573437"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-linux-devices"></a>Öğretici: Linux cihazlar için bir C IOT Edge modülü geliştirme

Visual Studio Code, C kodu geliştirmek ve Azure IOT Edge çalıştıran bir Linux cihazına dağıtmak için kullanın. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Visual Studio Code kullanarak C ile IoT Edge modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kapsayıcı kayıt defterinizde yayımlama
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğreticide bir modülde nasıl geliştirilebileceğini gösterir **C** kullanarak **Visual Studio Code**ve dağıtmak nasıl bir **Linux cihaz**. Windows cihazlar için modülleri geliştiriyorsanız, Git [Windows cihazlar için bir C IOT Edge modülü geliştirme](tutorial-c-module-windows.md) yerine.

Geliştirme ve Linux için C modülleri dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın: 

| C | Visual Studio Code | Visual Studio 2017 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Linux AMD64 C modülleri için VS Code kullanma](./media/tutorial-c-module/green-check.png) | ![VS 2017 Linux AMD64 üzerinde C modüller için kullanın](./media/tutorial-c-module/green-check.png) |
| **Linux ARM32** | ![Linux ARM32 üzerinde C modüller için VS Code kullanma](./media/tutorial-c-module/green-check.png) | ![VS 2017 Linux ARM32 üzerinde C modüller için kullanın](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce Linux kapsayıcı geliştirme için geliştirme ortamınızı ayarlamak için önceki öğreticide çalıştınız: [IOT Edge modülleri Linux cihazlar için geliştirme](tutorial-develop-for-linux.md). Bu öğreticiyi izleyerek, aşağıdaki önkoşulların yerinde olmalıdır: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Azure IOT Edge çalıştıran Linux cihaz](quickstart-linux.md)
* Kapsayıcı kayıt defteri gibi [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) ile yapılandırılmış [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak üzere yapılandırılmış.

C dilinde bir IOT Edge modülü geliştirme için geliştirme makinenizde aşağıdaki ek önkoşulları yükleyin: 

* Visual Studio Code için [C/C++ uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).

## <a name="create-a-module-project"></a>Bir modülü projesi oluşturma

Aşağıdaki adımlar, Visual Studio Code ve Azure IOT araçları uzantısını kullanarak, C için IOT Edge modülü projesinde oluşturur. Oluşturulan proje şablonu oluşturduktan sonra böylece modül bildirilen özelliklerine göre iletileri filtreler yeni kod ekleyin. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir C çözüm şablonu oluşturun.

1. VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin.

2. Yazın ve şu komutu çalıştırın komut Paleti'nde **Azure: Oturum** ve Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

3. Yazın ve şu komutu çalıştırın komut Paleti'nde **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | Seçin **C Modülü**. |
   | Modül adı sağlayın | Modülünüze **CModule** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü, son adımda sağladığınız adından doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/cmodule. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-c-module/repository.png)

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde .env dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="select-your-target-architecture"></a>Hedef Mimarinizi seçin

Şu anda, Visual Studio Code, C modülleri Linux AMD64 ve Linux ARM32v7 cihazlar için geliştirebilirsiniz. Kapsayıcı oluşturulur ve her bir mimari türü için farklı çalıştır olduğundan, her bir çözüm ile hedeflediğiniz hangi mimari seçmeniz gerekir. Linux AMD64 varsayılandır. 

1. Komut paletini açın ve arama **Azure IOT Edge: Varsayılan hedef Platform için Edge çözümü ayarlayın**, veya pencerenin alt kısmındaki kenar çubuğu kısayol simgesini seçin. 

2. Komut paletini hedef mimari seçeneklerini listeden seçin. Bu öğreticide, bir Ubuntu sanal makinesi varsayılan tutacak şekilde IOT Edge cihazı kullandığımız **amd64**. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Varsayılan modülü kodu, bir giriş kuyruğundaki iletileri alır ve bunları boyunca bir çıkış kuyruğuna aktarır. Böylece IOT Hub'ına iletmeden önce modülün uçta iletileri işleyen ek biraz kod ekleyelim. Modül güncelleştirin, böylece her ileti sıcaklık verileri analiz eder ve yalnızca sıcaklık belirli bir eşiği aşarsa, IOT Hub'ına ileti gönderir. 

1. Bu senaryoda sensörden alınan veriler JSON biçimindedir. JSON biçimindeki iletileri filtreleme amacıyla C için bir JSON kitaplığını içeri aktarın. Bu öğreticide Parson kullanılmıştır.

   1. İndirme [Parson GitHub deposu](https://github.com/kgabis/parson). **parson.c** ve **parson.h** dosyalarını **CModule** klasörüne kopyalayın.

   2. **modules** > **CModule** > **CMakeLists.txt** dosyasını açın. Dosyanın en üstünde Parson dosyalarını **my_parson** adlı bir kitaplık olarak içeri aktarın.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. **my_parson** kitaplığını CMakeLists.txt dosyasındaki **target_link_libraries** işlevinin kitaplık listesine ekleyin.

   4. **CMakeLists.txt** dosyasını kaydedin.

   5. **modules** > **CModule** > **main.c** dosyasını açın. Listesinin altındaki deyimleri ekleyin, dahil etmek için yeni bir tane `parson.h` JSON desteği:

      ```c
      #include "parson.h"
      ```

1. **main.c** dosyasının include bölümünün sonrasına `temperatureThreshold` adlı bir genel değişken ekleyin. Bu değişken, IoT Hub'a veri gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```c
    static double temperatureThreshold = 25;
    ```

1. Bulma `CreateMessageInstance` main.c işlevi. İç if-else deyimi birkaç satırlık bir işlevsellik ekleyen aşağıdaki kodla değiştirin: 

   ```c
   if ((messageInstance->messageHandle = IoTHubMessage_Clone(message)) == NULL)
   {
       free(messageInstance);
       messageInstance = NULL;
   }
   else
   {
       messageInstance->messageTrackingId = messagesReceivedByInput1Queue;
       MAP_HANDLE propMap = IoTHubMessage_Properties(messageInstance->messageHandle);
       if (Map_AddOrUpdate(propMap, "MessageType", "Alert") != MAP_OK)
       {
          printf("ERROR: Map_AddOrUpdate Failed!\r\n");
       }
   }
   ```

   Else deyimi kod yeni satırlarını yeni bir özellik olarak bir uyarı iletisi etiketler ileti ekleyin. Bu kod tüm iletileri uyarıları olarak etiketler, çünkü bunlar yüksek Sıcaklıkların raporu, yalnızca IOT Hub'ına iletileri gönderir işlevselliği ekleyeceğiz. 

1. `InputQueue1Callback` işlevinin tamamını aşağıdaki kodla değiştirin. Bu işlev gerçek bir mesajlaşma filtresi uygular. Bir ileti alındığında, bildirilen sıcaklık eşiği aşıyor olup olmadığını denetler. Evet ise, daha sonra çıktı sırasındaki üzerinden ileti iletir. Aksi durumda, iletiyi yok sayıyor. 

    ```c
    static IOTHUBMESSAGE_DISPOSITION_RESULT InputQueue1Callback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
        IOTHUBMESSAGE_DISPOSITION_RESULT result;
        IOTHUB_CLIENT_RESULT clientResult;
        IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle = (IOTHUB_MODULE_CLIENT_LL_HANDLE)userContextCallback;

        unsigned const char* messageBody;
        size_t contentSize;

        if (IoTHubMessage_GetByteArray(message, &messageBody, &contentSize) != IOTHUB_MESSAGE_OK)
        {
            messageBody = "<null>";
        }

        printf("Received Message [%zu]\r\n Data: [%s]\r\n",
                messagesReceivedByInput1Queue, messageBody);

        // Check if the message reports temperatures higher than the threshold
        JSON_Value *root_value = json_parse_string(messageBody);
        JSON_Object *root_object = json_value_get_object(root_value);
        double temperature;
        if (json_object_dotget_value(root_object, "machine.temperature") != NULL && (temperature = json_object_dotget_number(root_object, "machine.temperature")) > temperatureThreshold)
        {
            printf("Machine temperature %f exceeds threshold %f\r\n", temperature, temperatureThreshold);
            // This message should be sent to next stop in the pipeline, namely "output1".  What happens at "outpu1" is determined
            // by the configuration of the Edge routing table setup.
            MESSAGE_INSTANCE *messageInstance = CreateMessageInstance(message);
            if (NULL == messageInstance)
            {
                result = IOTHUBMESSAGE_ABANDONED;
            }
            else
            {
                printf("Sending message (%zu) to the next stage in pipeline\n", messagesReceivedByInput1Queue);

                clientResult = IoTHubModuleClient_LL_SendEventToOutputAsync(iotHubModuleClientHandle, messageInstance->messageHandle, "output1", SendConfirmationCallback, (void *)messageInstance);
                if (clientResult != IOTHUB_CLIENT_OK)
                {
                    IoTHubMessage_Destroy(messageInstance->messageHandle);
                    free(messageInstance);
                    printf("IoTHubModuleClient_LL_SendEventToOutputAsync failed on sending msg#=%zu, err=%d\n", messagesReceivedByInput1Queue, clientResult);
                    result = IOTHUBMESSAGE_ABANDONED;
                }
                else
                {
                    result = IOTHUBMESSAGE_ACCEPTED;
                }
            }
        }
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

1. `moduleTwinCallback` işlevi ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

    ```c
    static void moduleTwinCallback(DEVICE_TWIN_UPDATE_STATE update_state, const unsigned char* payLoad, size_t size, void* userContextCallback)
    {
        printf("\r\nTwin callback called with (state=%s, size=%zu):\r\n%s\r\n",
            ENUM_TO_STRING(DEVICE_TWIN_UPDATE_STATE, update_state), size, payLoad);
        JSON_Value *root_value = json_parse_string(payLoad);
        JSON_Object *root_object = json_value_get_object(root_value);
        if (json_object_dotget_value(root_object, "desired.TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_dotget_number(root_object, "desired.TemperatureThreshold");
        }
        if (json_object_get_value(root_object, "TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_get_number(root_object, "TemperatureThreshold");
        }
    }
    ```

1. Bulma `SetupCallbacksForModule` işlevi. İşlev ekleyen aşağıdaki kodla değiştirin. bir **else if** deyimini modül ikizi güncelleştirilip güncelleştirilmediğini kontrol edin.

   ```c
   static int SetupCallbacksForModule(IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle)
   {
       int ret;

       if (IoTHubModuleClient_LL_SetInputMessageCallback(iotHubModuleClientHandle, "input1", InputQueue1Callback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetInputMessageCallback(\"input1\")..........FAILED!\r\n");
           ret = __FAILURE__;
       }
       else if (IoTHubModuleClient_LL_SetModuleTwinCallback(iotHubModuleClientHandle, moduleTwinCallback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetModuleTwinCallback(default)..........FAILED!\r\n");
           ret = __FAILURE__;
       }
       else
       {
           ret = 0;
       }

       return ret;
   }
   ```

1. Main.c dosyayı kaydedin.

1. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. 

1. Dağıtım bildirimine CModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin:

   ```json
   "CModule": {
       "properties.desired":{
           "TemperatureThreshold":25
       }
   }
   ```

   ![Dağıtım şablonuna CModule ikizi ekleme](./media/tutorial-c-module/module-twin.png)

1. **deployment.template.json** dosyasını kaydedin.

## <a name="build-and-push-your-module"></a>Oluşturun ve modülünüzde gönderin

Önceki bölümde, IOT Edge çözümünü oluşturan ve iletileri kabul edilebilir sınırlar içinde bildirilen makine sıcaklık olduğu filtre CModule kod eklenir. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. VS Code terminal seçerek açın **görünümü** > **Terminal**.

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

4. Yenile düğmesine tıklayın. Yeni **CModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code Gezgininde, IOT Edge cihazınızın adına sağ tıklayıp **izleme D2C iletileri Başlat**.

2. IOT hub'da gelen iletileri görüntüleyin. IOT Edge cihazı, yeni dağıtım almak ve tüm modülleri başlatmak olduğundan bunu ulaşması iletileri için biraz zaman alabilir. Daha sonra iletileri göndermeden önce 25 derece makine sıcaklık ulaşana kadar biz CModule koda yapılan değişiklikleri bekleyin. İleti türü de ekler **uyarı** sıcaklık eşiğe ulaşan tüm iletileri. 

   ![IOT hub'da gelen iletileri görüntüleme](./media/tutorial-c-module/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>Modül ikizi Düzenle

Biz CModule modül ikizi dağıtım bildiriminde sıcaklık eşik 25 derecede ayarlamak için kullanılır. Modül ikizi modülü kodunu güncelleştirmek zorunda kalmadan işlevlerini değiştirmek için kullanabilirsiniz.

1. Visual Studio Code'da altında çalışan modülleri görmek için IOT Edge cihaz ayrıntıları genişletin. 

2. Sağ **CModule** seçip **düzenleme modül ikizi**. 

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

