---
title: Öğretici, Azure IOT Edge - özel C modülü oluşturma | Microsoft Docs
description: Bu öğreticide C koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilir
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 04/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: eeaff4769dba5b6e6951665d09cd12d13f22af07
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273728"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Öğretici: Bir C IOT Edge modülü geliştirme ve sanal Cihazınızı dağıtma

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Visual Studio Code kullanarak C ile IoT Edge modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kapsayıcı kayıt defterinizde yayımlama
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* Bir Azure sanal makinesi için hızlı başlangıç adımları izleyerek bir IOT Edge cihazı kullanabilirsiniz [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md). 

   >[!TIP]
   >Bu öğreticide, Linux kapsayıcıları kullanarak bir C modülü geliştirme için Visual Studio Code kullanır. C için Windows kapsayıcıları geliştirmek istiyorsanız, Visual Studio 2017'kullanmanız gerekir. Daha fazla bilgi için [kullanım Visual Studio 2017 geliştirme ve modülleri, Azure IOT Edge için hata ayıklama için](how-to-visual-studio-develop-module.md).

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/).
* Visual Studio Code için [C/C++ uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).
* [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Visual Studio Code için.
* [Docker CE](https://docs.docker.com/install/).

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
Aşağıdaki adımlar, bir IOT Edge modülü projesi oluşturmak için .NET tabanlı nasıl 2.0 kullanarak Visual Studio Code ve Azure IOT araçları çekirdek gösterir.

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

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

VS Code penceresinin beş düzey bileşenleri ile IOT Edge çözüm çalışma yükler. **Modülleri** modülünüzde C kodunu ve dockerfile'ları, modül olarak bir kapsayıcı görüntüsü oluşturmak için klasör içerir. **\.env** dosyasında kapsayıcı kayıt defterinizin kimlik bilgileri yer alır. **deployment.template.json** dosyasında IoT Edge çalışma zamanının modülleri cihazlara dağıtmak için kullandığı bilgiler bulunur. Ve **deployment.debug.template.json** kapsayıcıları dosyanın modülleri hata ayıklama sürümü. Bu öğreticide **\.vscode** klasörünü veya **\.gitignore** dosyasını düzenlemeyeceksiniz.

Çözümünüzü oluştururken kapsayıcı kayıt defteri belirtmediyseniz ve varsayılan localhost:5000 değerini kabul ettiyseniz \.env dosyanız olmaz.

<!--
   ![C solution workspace](./media/tutorial-c-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde .env dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Kodu C modülünüzde bildirilen makine sıcaklık güvenli bir eşiği aştı olup olmadığını denetlemek izin ekleyin. Sıcaklık çok yüksekse, modül IOT Hub'ına verileri göndermeden önce bir uyarı parametresi iletiye ekler. 

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

1. `CreateMessageInstance` işlevinin tamamını aşağıdaki kodla değiştirin. Bu işlev geri çağırma için bir bağlam ayırır.

    ```c
    static MESSAGE_INSTANCE* CreateMessageInstance(IOTHUB_MESSAGE_HANDLE message)
    {
        MESSAGE_INSTANCE* messageInstance = (MESSAGE_INSTANCE*)malloc(sizeof(MESSAGE_INSTANCE));
        if (NULL == messageInstance)
        {
            printf("Failed allocating 'MESSAGE_INSTANCE' for pipelined message\r\n");
        }
        else
        {
            memset(messageInstance, 0, sizeof(*messageInstance));

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
        }

        return messageInstance;
    }
    ```

1. `InputQueue1Callback` işlevinin tamamını aşağıdaki kodla değiştirin. Bu işlev gerçek bir mesajlaşma filtresi uygular.

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

1. `SetupCallbacksForModule` işlevini aşağıdaki kodla değiştirin.

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

1. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. Bu dosya, bu durumda dağıtmak için hangi modülü IOT Edge Aracısı söyler **tempSensor** ve **CModule**ve bunlar arasında iletileri yönlendirme hakkında IOT Edge hub'ı söyler. Visual Studio Code uzantısı otomatik olarak dağıtım şablonu gerekir, ancak her şeyi çözümünüz için doğru olduğundan emin olun, ilgili bilgilerin çoğunu doldurur: 

   1. Varsayılan platform, IOT Edge kümesine **amd64** , VS Code durum çubuğunda anlamına gelir, **CModule** görüntünün Linux amd64 sürümünü ayarlayın. Durum çubuğunda varsayılan platform değiştirme **amd64** için **arm32v7** , IOT Edge cihazınızın mimari ise. 

      ![Modül görüntü platform güncelleştirmesi](./media/tutorial-c-module/image-platform.png)

   2. Şablonda doğru modül adının bulunduğunu ve IoT Edge çözümünü oluştururken değiştirdiğiniz varsayılan **SampleModule** adının kullanılmadığından emin olun.

   3. **RegistryCredentials** bölümü depolar, Docker kayıt defteri kimlik bilgilerini böylece modül görüntünüzü IOT Edge Aracısı çekebilirsiniz. Gerçek kullanıcı adı ve parola çiftleri, git tarafından yoksayılan .env dosyasında depolanır. Henüz yapmadıysanız, kimlik bilgilerinizi .env dosyaya ekleyin.  

   4. Dağıtım bildirimleri hakkında daha fazla bilgi edinmek istiyorsanız bkz [modülleri dağıtma ve IOT Edge'de rotaları oluşturmak hakkında bilgi edinin](module-composition.md).

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

## <a name="build-and-push-your-solution"></a>Oluşturun ve çözümünüzü gönderin

Önceki bölümde, IOT Edge çözümünü oluşturan ve iletileri kabul edilebilir sınırlar içinde bildirilen makine sıcaklık olduğu filtre CModule kod eklenir. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. **Görünüm** > **Terminal**'i seçerek VS Code tümleşik terminalini açın.

1. Visual Studio Code tümleşik terminaline aşağıdaki komutu girerek Docker’da oturum açın. Modül görüntünüzü kayıt defterine gönderebilmek için Azure Container Registry kimlik bilgilerinizle oturum açmanız gerekir.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Birinci bölümde Azure Container Registry'den kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın. Veya Azure portalındaki kaydınızın **Erişim anahtarları** bölümünden tekrar alın.

2. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin.

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde yeni bir **config** klasöründe bir `deployment.json` dosyası oluşturur. deployment.json dosyası için gerekli bilgiler güncelleştirdiğiniz şablon dosyasından, kapsayıcı kayıt defterinizi depolamak için kullandığınız .env dosyasından ve CModule klasöründeki module.json dosyasından alınır.

Ardından Visual Studio Code, tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut kodunuzu derler, `CModule.dll` ile kapsayıcı oluşturur ve bunu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir.

VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini görebilirsiniz. Görüntü adresi `module.json` dosyasından **\<depo\>:\<sürüm\>-\<platform\>** biçiminde derlenir. Bu öğretici için **myregistry.azurecr.io/cmodule:0.0.1-amd64** şeklinde olmalıdır.

>[!TIP]
>Oluşturun ve gönderin, modül çalışılırken bir hata alırsanız aşağıdaki denetimleri yapın:
>* Docker kapsayıcı kayıt defterinizin kimlik bilgilerini kullanarak Visual Studio code'da oturum? Bu kimlik bilgilerini Azure portalında oturum açmak için kullandığınız yapılandırılanlardan farklı.
>* Kapsayıcı deponuza doğru mu? Açık **modülleri** > **cmodule** > **module.json** ve bulma **depo** alan. Görüntü deposu gibi görünmelidir  **\<registryname\>.azurecr.io/cmodule**. 
>* Geliştirme makinenizde çalışan kapsayıcılar aynı türde oluşturuyorsunuz? Visual Studio Code için Linux amd64 kapsayıcıları varsayar. Geliştirme makinenizde çalışan Linux arm32v7 kapsayıcılar, kapsayıcı platformunuzun eşleştirmek için VS Code penceresinin alt kısmındaki mavi durum çubuğu platformunda güncelleştirin. C modülleri Windows kapsayıcıları olarak oluşturulamıyor. 

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını (eski adıyla Azure IOT Toolkit uzantısını) kullanarak modülleri da dağıtabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildirimi dosyasına (**deployment.json**) zaten sahipsiniz. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

1. VS Code komut paletini içinde çalıştırma **Azure IOT Hub: IOT hub'ını seçin**.

2. Yapılandırmak istediğiniz IoT Edge cihazını barındıran aboneliği ve IoT hub'ını seçin.

3. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin.

4. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin.

   ![Tek bir cihaz için dağıtım oluşturma](./media/tutorial-c-module/create-deployment.png)

5. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın.

6. Yenile düğmesine tıklayın. Yeni **CModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

IOT Edge cihazında kendisi, komutunu kullanarak dağıtım modüllerinizi durumunu görebilirsiniz `iotedge list`. Dört modül görmeniz gerekir: İki IoT Edge çalışma zamanı modülü, tempSensor ve bu öğreticide oluşturduğunuz özel modül. Tüm modüllerin başlatılması birkaç dakika sürebilir. Bu nedenle tümünü görmüyorsanız komutu yeniden çalıştırın.

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

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda [Visual Studio Code için Azure IoT Edge ile bir C modülü geliştirme](how-to-develop-c-module.md) hakkında daha fazla bilgi edinebilirsiniz. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için bir sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [SQL Server veritabanları ile uçta veri Store](tutorial-store-data-sql-server.md)

