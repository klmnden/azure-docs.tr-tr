---
title: Öğretici, Windows - Azure IOT Edge C modülü geliştirme | Microsoft Docs
description: Bu öğretici, C kodu ile bir IOT Edge modülü oluşturun ve IOT Edge çalıştıran bir Windows cihazına dağıtmak nasıl gösterir
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 05/28/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 49f853341edab7c7dc92f72472b81f7fb22c0ad8
ms.sourcegitcommit: f9448a4d87226362a02b14d88290ad6b1aea9d82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66808763"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-windows-devices"></a>Öğretici: Windows cihazlar için bir C IOT Edge modülü geliştirme

C kodu geliştirmek ve Azure IOT Edge çalıştıran bir Windows cihazına dağıtmak için Visual Studio'yu kullanın. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * C SDK'sını alan bir IOT Edge modülünüzü oluşturmak için Visual Studio'yu kullanın.
> * Bir Docker görüntüsü oluşturma ve bunu kayıt defterinize yayımlama için Visual Studio ve Docker'ı kullanın.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğreticide bir modülde nasıl geliştirilebileceğini gösterir **C** kullanarak **Visual Studio 2019**ve dağıtmak nasıl bir **Windows cihaz**. Modüller için Linux cihazlarını geliştiriyorsanız, Git [Linux cihazlar için bir C IOT Edge modülü geliştirme](tutorial-c-module.md) yerine. 

Geliştirme ve C modülleri Windows cihazlarına dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın: 

| C | Visual Studio Code | Visual Studio 2017/2019 | 
| -- | ------------------ | ------------------ |
| **Windows AMD64** |  | ![Visual Studio'da WinAMD64 için C modülleri geliştirme](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce Windows kapsayıcı geliştirme için geliştirme ortamınızı ayarlamak için önceki öğreticide çalıştınız: [Windows cihazları için IOT Edge modülleri geliştirme](tutorial-develop-for-windows.md). Bu öğreticiyi tamamladıktan sonra aşağıdaki önkoşulların yerinde olmalıdır: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Azure IOT Edge çalıştıran Windows cihazı](quickstart.md).
* Kapsayıcı kayıt defteri gibi [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio 2019](https://docs.microsoft.com/visualstudio/install/install-visual-studio) ile yapılandırılmış [Azure IOT Edge araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) uzantısı.
* [Docker CE](https://docs.docker.com/install/) Windows kapsayıcılarını çalıştırmaya yönelik yapılandırılmış.
* C için Azure IOT SDK'sı 

> [!TIP]
> Visual Studio 2017 (sürüm 15.7 veya üzeri) kullanıyorsanız, lütfen yükleyip [Azure IOT Edge Araçları (Önizleme)](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) VS 2017 Visual Studio Market

## <a name="create-a-module-project"></a>Bir modülü projesi oluşturma

Aşağıdaki adımlar üzerinde C SDK'sı Visual Studio ve Azure IOT Edge araçları uzantısı kullanarak temel alan bir IOT Edge modülü projesi oluşturur. Oluşturulan proje şablonu oluşturduktan sonra böylece modül bildirilen özelliklerine göre iletileri filtreler yeni kod ekleyin. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir C çözüm şablonu oluşturun.

1. Visual Studio 2019 başlatın ve seçin **yeni proje oluştur**.

2. Yeni Proje penceresinde, arama **IOT Edge** projesini ve ardından **Azure IOT Edge (Windows amd64)** proje. **İleri**’ye tıklayın. 

   ![Yeni Azure IOT Edge projesi oluşturma](./media/tutorial-c-module-windows/new-project.png)

3. Yapılandırma, yeni proje penceresini benzer bir şey açıklayıcı projeyi ve çözümü yeniden adlandır **CTutorialApp**. Tıklayın **Oluştur** projeyi oluşturmak için. 

   ![Yeni bir Azure IOT Edge proje yapılandırma](./media/tutorial-c-module-windows/configure-project.png)

4. IOT Edge uygulama ve modül penceresinde, projenize aşağıdaki değerleri yapılandırın: 

   | Alan | Değer |
   | ----- | ----- |
   | Bir şablon seçin | Seçin **C Modülü**. | 
   | Modül proje adı | Modülünüze **CModule** adını verin. | 
   | Docker görüntü deposu | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü modülü proje adı değerini doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/cmodule. |

   ![Hedef cihaz, modül türü ve kapsayıcı kayıt defteri için projenizi yapılandırın](./media/tutorial-c-module-windows/add-application-and-module.png)

5. Seçin **Tamam** yaptığınız değişiklikleri uygulamak için. 

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Dağıtım bildirimi IOT Edge çalışma zamanı ile kapsayıcı kayıt defterinizin kimlik bilgilerini paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. Kimlik bilgileri kullanmak **erişim anahtarları** Azure kapsayıcı kayıt defterinizin bölümü. 

1. Visual Studio Çözüm Gezgini'nde açın **deployment.template.json** dosya. 

2. Bulma **registryCredentials** $edgeAgent özelliğinde istenen özellikleri. 

3. Özelliği bu biçim izleyen, kimlik bilgileriyle güncelleştirin: 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }
   ```

4. Deployment.template.json dosyayı kaydedin. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Varsayılan modülü kodu, bir giriş kuyruğundaki iletileri alır ve bunları boyunca bir çıkış kuyruğuna aktarır. Böylece IOT Hub'ına iletmeden önce modülün uçta iletileri işleyen ek biraz kod ekleyelim. Modül güncelleştirin, böylece her ileti sıcaklık verileri analiz eder ve yalnızca sıcaklık belirli bir eşiği aşarsa, IOT Hub'ına ileti gönderir. 


1. Bu senaryoda sensörden alınan veriler JSON biçimindedir. JSON biçimindeki iletileri filtreleme amacıyla C için bir JSON kitaplığını içeri aktarın. Bu öğreticide Parson kullanılmıştır.

   1. İndirme [Parson GitHub deposu](https://github.com/kgabis/parson). Kopyalama **parson.c** ve **parson.h** dosyalarınızı **CModule** proje.

   2. Visual Studio'da açın **CMakeLists.txt** CModule proje klasöründeki dosya. Dosyanın en üstünde Parson dosyalarını **my_parson** adlı bir kitaplık olarak içeri aktarın.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. Ekleme **my_parson** kitaplıklarında listesine **target_link_libraries** CMakeLists.txt dosyasıyla bölümü.

   4. **CMakeLists.txt** dosyasını kaydedin.

   5. Açık **CModule** > **main.c**. Listesinin altındaki deyimleri ekleyin, dahil etmek için yeni bir tane `parson.h` JSON desteği:

      ```c
      #include "parson.h"
      ```

2.  İçinde **main.c** adlı bir genel değişken ekleyin `temperatureThreshold` messagesReceivedByInput1Queue değişkeni yanında. Bu değişken, IoT Hub'a veri gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```c
    static double temperatureThreshold = 25;
    ```

3. Bulma `CreateMessageInstance` main.c işlevi. İç if-else deyimi birkaç satırlık bir işlevsellik ekleyen aşağıdaki kodla değiştirin: 

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

4. Bulma `InputQueue1Callback` işlev ve tüm işlevini aşağıdaki kodla değiştirin. Bu işlev gerçek bir mesajlaşma filtresi uygular. Bir ileti alındığında, bildirilen sıcaklık eşiği aşıyor olup olmadığını denetler. Evet ise, daha sonra çıktı sırasındaki üzerinden ileti iletir. Aksi durumda, iletiyi yok sayıyor. 

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

        // If temperature exceeds threshold, send to output1
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
        // If message does not exceed threshold, do not forward
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

5. `moduleTwinCallback` işlevi ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

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

6. Bulma `SetupCallbacksForModule` işlevi. İşlev ekleyen aşağıdaki kodla değiştirin. bir **else if** deyimini modül ikizi güncelleştirilip güncelleştirilmediğini kontrol edin. 

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

7. Main.c dosyayı kaydedin.

8. Açık **deployment.template.json** dosya. 

9. Dağıtım bildirimine CModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin:

   ```json
   "CModule": {
       "properties.desired":{
           "TemperatureThreshold":25
       }
   }
   ```

   ![Dağıtım şablonuna CModule ikizi ekleme](./media/tutorial-c-module-windows/module-twin.png)

1. **deployment.template.json** dosyasını kaydedin.

## <a name="build-and-push-your-module"></a>Oluşturun ve modülünüzde gönderin

Önceki bölümde, IOT Edge çözümü oluşturulur ve koduna eklemiştir **CModule** bildirilen makine sıcaklık kabul edilebilir eşiğin altında olduğu mesajları filtrelemek için. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor. 

1. Docker için geliştirme makinenizde oturum açmak için aşağıdaki komutu kullanın. Kullanıcı adı, parola ve oturum açma sunucusu, Azure container registry'den oturum açın. Bu değerleri alabilirsiniz **erişim anahtarları** Azure portalında kayıt defterinizin bölümü.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Kullanılmasını öneren bir güvenlik uyarısı alabilirsiniz `--password-stdin`. Bu en iyi uygulama, üretim senaryoları için önerilir, bu öğreticinin kapsamı dışında olan. Daha fazla bilgi için [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvuru.

2. Visual Studio Çözüm Gezgini'nde, derlemek istediğiniz proje adına sağ tıklayın. Varsayılan ad **AzureIotEdgeApp1** ve Windows modülü oluştururken bu yana uzantısı olmalıdır **Windows.Amd64**. 

3. Seçin **oluşturun ve gönderin, IOT Edge modülleri**. 

   Derleme ve gönderme komut üç işlemi başlatır. İlk olarak, adlı bir çözüm içinde yeni bir klasör oluşturur **config** , tam bir dağıtım bildirimi, yerleşik dağıtım şablonu bilgilerinin kullanıma ve diğer çözüm dosyalarını içerir. İkinci olarak, çalıştığında `docker build` uygun dockerfile, hedef mimari için temel kapsayıcı görüntüsünü oluşturmak için. Ardından, çalışan `docker push` kapsayıcı kayıt defterinize görüntü deposuna gönderin. 

## <a name="deploy-modules-to-device"></a>Modüller cihazına dağıtma

IOT Edge cihazınıza modülü projeyi dağıtmak için Visual Studio cloud explorer ve Azure IOT Edge araçları uzantısını kullanın. Senaryonuz için hazırlanmış bir dağıtım bildirimi zaten **deployment.json** config klasöründeki dosya. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IOT Edge Cihazınızı ve çalışıyor olduğundan emin olun. 

1. Visual Studio cloud Explorer'da kaynakları IOT cihazlarınızın listesini görmek için genişletin. 

2. Dağıtım almak istediğiniz IOT Edge cihaz adına sağ tıklayın. 

3. Seçin **dağıtım oluşturma**.

4. Dosya Gezgini'nde seçin **deployment.windows amd64** çözümünüzün config klasöründeki dosya. 

5. Cihazınızı altında listelenen dağıtılan modüller görmek için cloud explorer'ı yenileyin. 


## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır. 

IOT Edge araçları uzantısı IOT Hub'ınıza geldikçe iletilerini görüntülemek için kullanabilirsiniz. 

1. Visual Studio cloud explorer'ın IOT Edge Cihazınızı adını seçin. 

2. İçinde **eylemleri** listesinden **Başlat yerleşik olay uç nokta izleme**. 

3. IOT hub'da gelen iletileri görüntüleyin. IOT Edge cihazı, yeni dağıtım almak ve tüm modülleri başlatmak olduğundan bunu ulaşması iletileri için biraz zaman alabilir. Daha sonra iletileri göndermeden önce 25 derece makine sıcaklık ulaşana kadar biz CModule koda yapılan değişiklikleri bekleyin. İleti türü de ekler **uyarı** sıcaklık eşiğe ulaşan tüm iletileri. 

   ![IOT hub'da gelen iletileri görüntüleme](./media/tutorial-c-module-windows/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>Modül ikizi Düzenle

25 derecede sıcaklık eşiği ayarlamak için CModule modül ikizi kullandık. Modül ikizi modülü kodunu güncelleştirmek zorunda kalmadan işlevlerini değiştirmek için kullanabilirsiniz.

1. Visual Studio'da açın **deployment.windows amd64.json** dosya. (Deployment.template dosyası değil. Dağıtım yapılandırma dosyası Çözüm Gezgini'nde bildirimi, seçin görmüyorsanız **tüm dosyaları göster** Gezgini araç çubuğundaki simgesi.)

2. CModule ikizi bulun ve değiştirin **temperatureThreshold** parametresi için yeni bir sıcaklık 5 derece 10 derece son bildirilen sıcaklık daha yüksek. 

3. Kaydet **deployment.windows amd64.json** dosya.

4. Güncelleştirilmiş bir dağıtım bildirimi için Cihazınızı yeniden uygulamak için dağıtım adımları izleyin. 

5. CİHAZDAN buluta gelen iletileri izleyin. Yeni sıcaklık eşiği ulaşılana kadar Durdur iletileri görmeniz gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Aksi takdirde, yerel yapılandırmaları ve ücretleri önlemek için bu makalede kullanılan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi derlemek için hazır olduğunuzda, daha fazla bilgi edinebilirsiniz [kendi IOT Edge modülleri geliştirmek](module-development.md) veya nasıl [modülleri Visual Studio ile geliştirin](how-to-visual-studio-develop-module.md). Azure IOT Edge, uçta verilerini işlemek ve çözümlemek için Azure bulut Hizmetleri dağıtmayı nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticiler açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [İşlevleri](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [makine öğrenimi](tutorial-deploy-machine-learning.md)
> [özel görüntü işleme hizmeti](tutorial-deploy-custom-vision.md)
