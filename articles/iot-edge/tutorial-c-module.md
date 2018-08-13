---
title: Azure IoT Edge C öğreticisi | Microsoft Docs
description: Bu öğreticide C koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilir
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 07/30/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 31560cbd4d8b4572ce930db7ffb8753f3e4a4bc0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425927"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Öğretici: C IoT Edge modülü geliştirme ve simülasyon cihazınıza dağıtma

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak C ile IoT Edge modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kapsayıcı kayıt defterinizde yayımlama 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.

Bulut kaynakları:

* Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 

Geliştirme kaynakları:

* [Visual Studio Code](https://code.visualstudio.com/). 
* Visual Studio Code için [C/C++ uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).
* Visual Studio Code için [Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).
* [Docker CE](https://docs.docker.com/install/). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

Aşağıdaki Azure CLI komutu **IoTEdgeResources** adlı bir kaynak grubunda bir kayıt defteri oluşturur. **{acr_name}** değerini kayıt defteriniz için benzersiz bir adla değiştirin. 

   ```azurecli-interactive
   az acr create --resource-group IoTEdgeResources --name {acr_name} --sku Basic --admin-enabled true
   ```

Kayıt defterinizin kimlik bilgilerini alın. 

   ```azurecli-interactive
   az acr credential show --name {acr_name}
   ```

**Username** değerini ve parolalardan birini kopyalayın. Bu değerleri öğreticide ileride Docker görüntüsünü kayıt defterinizde yayımladığınızda ve kayıt defteri kimlik bilgilerini Edge çalışma zamanına eklediğinizde kullanacaksınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code ve Azure IoT Edge uzantısı kullanılarak .NET Core 2.0'a dayalı bir IoT Edge modülü projesinin nasıl oluşturulduğu gösterilmektedir.
1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Tümleşik Terminal**'i seçin.
2. VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 
3. Komut paletinde **Azure: Sign in** komutunu yazıp çalıştırdıktan sonra yönergeleri izleyerek Azure hesabınızda oturum açın. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.
4. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu yazıp çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 
   1. Çözümü oluşturmak istediğiniz klasörü seçin. 
   2. Çözümünüz için bir ad girin veya varsayılan **EdgeSolution** adını kabul edin.
   3. Modül şablonu olarak **C Module** girişini seçin. 
   4. Modülünüze **CModule** adını verin. 
   5. İlk modülünüz için görüntü deposu olarak önceki bölümde oluşturduğunuz Azure Container Registry bileşenini belirtin. **localhost:5000** yerine kopyaladığınız oturum açma sunucusu değerini yazın. Dizenin son hali **\<kayıt adı\>.azurecr.io/cmodule** ifadesine benzer olmalıdır.
 
4. VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. **modules** klasörü, **.vscode** klasörü, dağıtım bildirimi şablon dosyası ve **.env** dosyası bulunur. Varsayılan modül kodu kanal modülü olarak uygulanır. 

5. JSON biçimindeki iletileri filtrelemek için C için bir JSON kitaplığının içeri aktarılması gerekir. C modülünüzdeki JSON iletilerini ayrıştırmak için istediğiniz JSON kitaplığını seçebilir veya kendi yazdığınız kodu kullanabilirsiniz. Aşağıdaki adımlarda örnek olarak [Parson](https://github.com/kgabis/parson) kullanılmıştır.
   1. [Parson Github deposundan](https://github.com/kgabis/parson) **parson.c** ve **parson.h** belgelerini indirin. Bu dosyaları kopyalayıp **CModule** klasörüne yapıştırın.
   2. **modules** > **CModule** > **CMakeLists.txt** dosyasını açın. Aşağıdaki satırları ekleyerek parson kitaplığını my_parson olarak içeri aktarın.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. **CMakeLists.txt** içindeki `target_link_libraries` bölümüne `my_parson` ekleyin.
   4. **modules** > **CModule** > **main.c** dosyasını açın. include bölümünün en altına JSON desteği için `parson.h` öğesini eklemek üzere aşağıdaki kodu ekleyin:

      ```c
      #include "parson.h"
      ```

6. include bölümünün altına `temperatureThreshold` genel değişkenini ekleyin. Bu değişken, IoT Hub'a veri gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar. 

    ```c
    static double temperatureThreshold = 25;
    ```

7. `CreateMessageInstance` işlevinin tamamını aşağıdaki kodla değiştirin. Bu işlev geri çağırma için bir bağlam ayırır. 

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

8. `InputQueue1Callback` işlevinin tamamını aşağıdaki kodla değiştirin. Bu işlev gerçek bir mesajlaşma filtresi uygular. 

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

9. `moduleTwinCallback` işlevi ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

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

10. `SetupCallbacksForModule` işlevindeki `moduleTwinCallback` için yeni bir geri çağırma ekleyin. `SetupCallbacksForModule` işlevinin tamamını aşağıdaki kodla değiştirin.

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

11. Bu dosyayı kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve CModule modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtreleyecek kodu eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor. 

1. Modül görüntüsünü ACR'ye gönderebilmek için Visual Studio Code tümleşik terminaline aşağıdaki komutu girerek Docker oturumu açın: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Birinci bölümde Azure Container Registry'den kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın. Veya Azure portalındaki kaydınızın **Erişim anahtarları** bölümünden tekrar alın.

2. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. Bu dosya `$edgeAgent` aracısına iki modülü dağıtmasını söyler: **tempSensor** ve **CModule**. `CModule.image`, görüntünün Linux amd64 sürümüne göre ayarlanmıştır. Dağıtım bildirimleri hakkında daha fazla bilgi edinmek için bkz. [IoT Edge modüllerinin kullanılmasını, yapılandırılmasını ve yeniden kullanılmasını anlama](module-composition.md).

3. **deployment.template.json** dosyasında, Docker kayıt defteri kimlik bilgilerinizin yer aldığı **registryCredentials** bölümü bulunur. Gerçek kullanıcı adı ve parola çiftleri, git tarafından yoksayılan .env dosyasında depolanır.

4. Dağıtım bildirimine CModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin: 
    ```json
        "CModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Bu dosyayı kaydedin.
5. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build IoT Edge solution** (IoT Edge çözümü oluştur) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve yeni bir **config** klasöründe bir `deployment.json` dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut kodunuzu derler, `CModule.dll` ile kapsayıcı oluşturur ve bunu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini görebilirsiniz. Görüntü adresi `module.json` dosyasından **\<depo\>:\<sürüm\>-\<platform\>** biçiminde derlenir. Bu öğretici için **registryname.azurecr.io/cmodule:0.0.1-amd64** şeklinde olmalıdır.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

1. Azure IoT Toolkit uzantısını IoT hub'ınızın bağlantı dizesiyle yapılandırın: 
    1. **Görünüm** > **Gezgin**'i seçerek VS Code gezginini açın. 
    2. Gezginde **AZURE IOT HUB CİHAZLARI**'na ve ardından **...** düğmesine tıklayın. **Select IoT Hub** (IoT Hub'ını Seçin) öğesine tıklayın. Yönergeleri izleyerek Azure hesabınızda oturum açın ve IoT hub'ınızı seçin. 
       **Set IoT Hub Connection String** (IoT Hub Bağlantı Dizesini Ayarla) öğesine tıklayarak da kurulumu gerçekleştirebilirsiniz. Açılır pencerede IoT Edge cihazınızın bağlandığı IoT hub'ının bağlantı dizesini girin.

2. Azure IoT Hub Devices (Azure IoT Hub Cihazları) gezgininde IoT Edge cihazınıza ve ardından **Create Deployment for IoT Edge device** (IoT Edge cihazı için dağıtım oluştur) öğesine tıklayın. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın.

3. Yenile düğmesine tıklayın. Yeni **CModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. IoT hub'a gelen verileri izlemek için **...** simgesine tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
2. Belirli bir cihazın D2C iletilerini izlemek için listeden cihaza sağ tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
3. Verileri izlemeyi durdurmak için komut paletinden **Azure IoT Hub: Stop monitoring D2C message** komutunu seçin. 
4. Modül ikizini görüntülemek veya güncelleştirmek için listeden modüle sağ tıklayıp **Edit module twin** (Modül ikizini düzenle) öğesini seçin. Modül ikizini güncelleştirmek için ikiz JSON dosyasını kaydedip düzenleyici alanına sağ tıklayın ve **Update Module Twin** (Modül İkizini Güncelleştir) öğesini seçin.
5. Docker günlüklerini görüntülemek isterseniz VS Code için [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) uygulamasını yükleyebilir ve çalışan modüllerinizi Docker gezgininde yerel olarak bulabilirsiniz. Tümleşik terminalde görüntülemek için bağlam menüsünde **Show Logs** (Günlükleri Göster) öğesine tıklayın.
 
## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçecekseniz oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Azure kaynak gruplarını silme işlemi geri alınamaz. Silindikten sonra kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Yalnızca IoT Hub'ı silmek için hub adını ve kaynak grubu adını kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub delete --name {hub_name} --resource-group IoTEdgeResources
```


Kaynak grubunun tamamını adıyla silmek için:

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda [Visual Studio Code için Azure IoT Edge ile bir C modülü geliştirme](how-to-develop-c-module.md) hakkında daha fazla bilgi edinebilirsiniz. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için bir sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [SQL Server veritabanları ile uç cihazlarda veri depolama](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
