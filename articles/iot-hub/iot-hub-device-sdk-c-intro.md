---
title: "C için Azure IOT cihaz SDK'sı | Microsoft Docs"
description: "C için Azure IOT cihaz SDK'sı ile başlayın ve IOT hub ile iletişim cihaz uygulamaları oluşturmayı öğrenin."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 05a025a02046ff091b4fea75404cb74aad2e07fa
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="azure-iot-device-sdk-for-c"></a>C için Azure IOT cihaz SDK'sı

**Azure IOT cihaz SDK'sı** iletileri gelen iletileri gönderme ve alma işlemini basitleştirmek için tasarlanmış bir dizi kitaplıktır **Azure IOT Hub** hizmet. Her belirli bir platformu hedefleyen SDK, farklı çeşidi vardır, ancak bu makalede **C için Azure IOT cihaz SDK'sı**.

C için Azure IOT cihaz SDK'sı ANSI C taşınabilirliği en üst düzeye çıkarmak için (C99) yazılır. Bu özellik kitaplıkları birden çok platformlarda ve cihazlarda özellikle disk en aza burada çalışması için oldukça uygun hale getirir ve bellek alanını önceliktir.

Çok çeşitli platformlar üzerinde SDK test edilmiştir vardır (bkz [Azure IOT cihaz katalog için onaylanmıştır](https://catalog.azureiotsuite.com/) Ayrıntılar için). Bu makalede Windows platformunda çalışan örnek kodu izlenecek yollar içerse de, bu makaledeki kod desteklenen platformlar aynı aralığıdır.

Aşağıdaki video Azure IOT SDK'sı genel bir bakış için C: gösterir.

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-IoT-C-SDK-insights/Player]

Bu makalede c için Azure IOT cihaz SDK mimarisini sunar Cihaz kitaplığı başlatılamıyor, IOT Hub'ına veri göndermek ve buradan ileti almak nasıl gösterir. Bu makaledeki bilgiler SDK'sını kullanmaya başlamak için yeterli olmalıdır, ancak ayrıca işaretçileri kitaplıkları hakkında ek bilgiler sağlar.

## <a name="sdk-architecture"></a>SDK mimarisi

Bulabileceğiniz [ **C için Azure IOT cihaz SDK'sı** ](https://github.com/Azure/azure-iot-sdk-c) GitHub depo ve görünüm ayrıntılarını API [C API Başvurusu](https://azure.github.io/azure-iot-sdk-c/index.html).

En son sürümünü kitaplıkları bulunabilir **ana** deponun dalı:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* Çekirdek uygulama SDK'ın bulunduğu **ıothub\_istemci** SDK en düşük API katmanı uyarlamasını içeren klasörü: **IoTHubClient** kitaplığı. **IoTHubClient** kitaplığı IOT Hub'ına iletileri göndermek ve IOT Hub'ından iletileri almak için ham Mesajlaşma uygulama API'leri içerir. Bu kitaplık kullanırken, ileti serileştirme uygulamak için sorumlu olan, ancak diğer ayrıntıları IOT Hub ile iletişim için işlenir.
* **Seri hale getirici** klasörü yardımcı işlevleri ve istemci kitaplığı kullanılarak Azure IOT Hub'ına göndermeden önce verilerini seri hale getirmek nasıl Göster örnekleri içerir. Seri hale getirici kullanımını zorunlu değildir ve kolaylık sağlanır. Kullanılacak **seri hale getirici** kitaplığı, IOT Hub ve ondan almayı beklediğinizi iletileri göndermek için veri modeli tanımlayın. Model tanımlandıktan sonra SDK serileştirme ayrıntılar hakkında endişelenmeden cihaz Bulut ve bulut-cihaz iletilerini ile kolayca çalışmanızı sağlayan bir API yüzeyi sağlar. Kitaplık MQTT ve AMQP gibi protokoller kullanarak aktarım uygulayan diğer açık kaynak kitaplıkları bağlıdır.
* **IoTHubClient** kitaplığı diğer açık kaynak kitaplıkları bağlıdır:
  * [Azure C paylaşılan yardımcı programı](https://github.com/Azure/azure-c-shared-utility) kitaplığı çeşitli Azure ile ilgili C SDK gerekli (dizeler, liste işleme ve g/ç gibi) temel görevleri için ortak işlevsellik sağlar.
  * [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) bir istemci-tarafı uygulaması kısıtlı kaynak cihazlar için en iyi duruma getirilmiş AMQP olan kitaplığı.
  * [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) MQTT protokolünü uygulamaya yönelik genel amaçlı bir kitaplığı ve kısıtlı kaynak cihazlar için en iyi duruma getirilmiş kitaplığı.

Bu kitaplıklar kullanımını örnek kodu bakarak anlamak daha kolay olur. Aşağıdaki bölümlerde, SDK'da bulunan örnek uygulamalar çeşitli size yol. Bu kılavuz çeşitli özellikleri SDK'ın mimari katmanları ve API'leri nasıl giriş bilgileri için iyi bir fikir vermesi gerekir.

## <a name="before-you-run-the-samples"></a>Örneği çalıştırmadan önce

Örnekler C için Azure IOT cihaz SDK'sı çalıştırmadan önce şunları yapmalısınız [IOT hub'ı hizmet örneği oluşturma](iot-hub-create-through-portal.md) Azure aboneliğinizde. Ardından aşağıdaki görevleri tamamlayın:

* Geliştirme ortamınızı hazırlama
* Cihaz kimlik bilgileri edinin.

### <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Paketleri (örneğin, Windows için NuGet veya apt_get Debian ve Ubuntu) ortak platformlar için sağlanır ve bu paketleri kullanılabilir olduğunda örnekleri kullanın. Bazı durumlarda, Cihazınızda veya için SDK derleme gerekir. SDK derlemek ihtiyacınız varsa bkz [geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) GitHub deposunda.

Örnek uygulama kodu almak için SDK'ın bir kopyasını Github'dan indirin. Kaynak sunucudan kopyanızı almak **ana** dalı [GitHub deposunu](https://github.com/Azure/azure-iot-sdk-c).


### <a name="obtain-the-device-credentials"></a>Cihaz kimlik bilgilerini alma

Örnek koda sahip olduğunuza göre sonraki bir şey yapmak için aygıt kimlik bilgileri kümesi almaktır. Bir cihaz IOT hub'ı erişebilmeleri, önce cihaz IOT Hub kimlik kayıt defterine eklemeniz gerekir. Cihazınızı eklediğinizde, cihazın IOT hub'ına bağlanabilmesi gereken cihaz kimlik bilgileri kümesi alır. Sonraki bölümde açıklanan örnek uygulamalar bu kimlik bilgileri biçiminde beklediğiniz bir **cihaz bağlantı dizesi**.

IOT hub'ınızı yönetmenize yardımcı olmak için birkaç açık kaynaklı araçları vardır.

* Adlı bir Windows uygulaması [aygıt explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).
* Platformlar arası Python CLI aracı adlı [IOT uzantısı için Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

Bu öğretici grafik kullanır *aygıt explorer* aracı. Aynı zamanda *IOT uzantısı için Azure CLI 2.0* CLI aracını kullanmayı tercih ederseniz, aracı.

Cihaz ekleme de dahil olmak üzere IOT hub'da çeşitli işlevleri gerçekleştirmek için Azure IOT hizmeti kitaplıkları aygıtı explorer aracını kullanır. Bir cihaz eklemek için aygıtı explorer aracını kullanırsanız, cihazınız için bir bağlantı dizesi alın. Örnek uygulamaları çalıştırmak için bu bağlantı dizesi gerekir.

Cihaz explorer aracıyla bilmiyorsanız, aşağıdaki yordamı bir cihaz ekleme ve bir cihaz bağlantı dizesini almak için kullanmak üzere açıklar.

Cihaz explorer aracını yüklemek için bkz: [IOT Hub cihazları için cihaz Gezgini'ni kullanma](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

Programını çalıştırdığınızda, bu arabirim bakın:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Girin, **IOT Hub bağlantı dizesine** tıklatın ve ilk alan **güncelleştirme**. IOT Hub ile iletişim kurabilmesi için bu adımı aracı yapılandırır.

IOT Hub bağlantı dizesine yapılandırıldığında tıklatın **Yönetim** sekmesi:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Bu sekme, IOT hub'ınıza kayıtlı cihazları yöneteceğiniz kullanılabilir.

Tıklayarak bir cihaz oluşturma **oluşturma** düğmesi. Bir iletişim kutusu (birincil ve ikincil) önceden doldurulmuş haldedir anahtarları bir kümesini görüntüler. Girin bir **cihaz kimliği** ve ardından **oluşturma**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Cihaz oluşturulduğunda, cihazları, yeni oluşturduğunuz de dahil olmak üzere tüm kayıtlı cihazlar ile güncelleştirmeleri listeleyin. Yeni aygıtınızı sağ tıklatın, bu menü bakın:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Seçerseniz **kopyalama seçili cihaz için bağlantı dizesi**, cihaz bağlantı dizesini panoya kopyalandı. Cihaz bağlantı dizesi bir kopyasını tutun. Aşağıdaki bölümlerde açıklanan örnek uygulamaları çalıştırırken gerekir.

Yukarıdaki adımları tamamladıktan sonra bazı kodları çalıştıran başlatmaya hazırsınız. Her iki örnek bir sabit bir bağlantı dizesi girin olanak tanıyan ana kaynak dosyasının en üstte vardır. Örneğin, karşılık gelen satırından **ıothub\_istemci\_örnek\_mqtt** uygulama şu şekilde görünür.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-the-iothubclient-library"></a>IoTHubClient kitaplığını kullanma

İçinde **ıothub\_istemci** klasöründe [azure-IOT-sdk-c](https://github.com/azure/azure-iot-sdk-c) deposunu var. bir **örnekleri** adlıbiruygulamaiçerenklasörde**ıothub\_istemci\_örnek\_mqtt**.

Windows sürümü, **ıothub\_istemci\_örnek\_mqtt** uygulama aşağıdaki Visual Studio çözümü içerir:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> Bu projeyi Visual Studio 2017 içinde açarsanız, en son sürüme projenizin hedefini ister kabul edin.

Bu çözüm, tek bir proje içerir. Bu çözümde yüklü olan dört NuGet paketleri vardır:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

Her zaman gereksinim duyduğunuz **Microsoft.Azure.C.SharedUtility** paketini SDK ile çalışırken. Bu örnek MQTT protokolünü kullanır, bu nedenle içermelidir **Microsoft.Azure.umqtt** ve **Microsoft.Azure.IoTHub.MqttTransport** paketleri (AMQP ve HTTPS için eşdeğer paketi vardır). Örnek kullandığından **IoTHubClient** kitaplığı de dahil etmelisiniz **Microsoft.Azure.IoTHub.IoTHubClient** çözümünüzdeki paket.

Örnek uygulama için uygulama Bul **ıothub\_istemci\_örnek\_mqtt.c** kaynak dosyası.

Ne kullanmak için gerekli olan aracılığıyla yürütmek için bu örnek uygulama aşağıdaki adımları kullanın **IoTHubClient** kitaplığı.

### <a name="initialize-the-library"></a>Kitaplığı başlatılamıyor

> [!NOTE]
> Kitaplıkları ile çalışmaya başlamadan önce bazı platforma özgü başlatma gerçekleştirmeniz gerekebilir. Örneğin, AMQP Linux'ta kullanmayı planlıyorsanız, OpenSSL kitaplığı başlatması gerekir. Örnekleri [GitHub deposunu](https://github.com/Azure/azure-iot-sdk-c) yardımcı işlevini çağırın **platform\_init** ne zaman istemci başlar ve arama **platform\_deinit**çıkmadan önce işlevi. Bu işlevler platform.h üstbilgi dosyasında bildirilir. Bu işlevler, hedef platformu için tanımlarını incelemek [depo](https://github.com/Azure/azure-iot-sdk-c) , istemci herhangi bir platforma özgü başlatma kod dahil gerekip gerekmediğini belirlemek için.

Kitaplıkları ile çalışmaya başlamak için öncelikle bir IOT Hub istemci tanıtıcısı atayın:

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

Bu işleve aygıt explorer aracından aldığınız cihaz bağlantı dizesi bir kopyasını geçirin. Ayrıca kullanılacak iletişim protokolü belirlemeniz. Bu örnek MQTT kullanır, ancak AMQP ve HTTPS ayrıca seçeneklerdir.

Geçerli bir olduğunda **IOTHUB\_istemci\_İŞLEMEK**, IOT hub'ı gelen ve giden ileti gönderme ve alma için API'larını çağırma başlatabilirsiniz.

### <a name="send-messages"></a>İleti gönder

Örnek uygulama, IOT hub'ınıza ileti göndermek için bir döngü ayarlar. Aşağıdaki kod parçacığında:

- Bir ileti oluşturur.
- Bir özellik iletiye ekler.
- Bir ileti gönderir.

İlk olarak, bir ileti oluşturun:

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission to IoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

İleti gönderme her zaman, veriler gönderilirken çağrılan bir geri çağırma işlevini referansı belirtin. Bu örnekte geri çağırma işlevi çağrılır **SendConfirmationCallback**. Aşağıdaki kod parçacığında bu geri çağırma işlevini gösterir:

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Çağrı Not **IoTHubMessage\_Destroy** iletiyle bittiğinde işlev. Bu işlev, ileti oluşturduğunuzda ayrılan kaynakları serbest bırakır.

### <a name="receive-messages"></a>İleti al

Bir ileti alma zaman uyumsuz bir işlemdir. İlk olarak, aygıt bir ileti aldığında çağırmak için geri çağırma kaydedin:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

Son parametresi, istediğiniz void bir işaretçidir. Örnekte, bir tamsayıya işaretçidir ancak daha karmaşık veri yapısı için bir işaretçi olabilir. Bu parametre, paylaşılan durumuna bu işlev, çağrıyı ile çalışmak geri çağırma işlevi sağlar.

Aygıt bir ileti aldığında, kayıtlı bir geri çağırma işlevi çağrılır. Bu geri çağırma işlevini alır:

* Bağıntı kimliği iletisi ve ileti kimliği.
* İleti içeriği.
* Tüm özel özellikler iletisi.

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable to retrieve the message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive the work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from the message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Kullanım **IoTHubMessage\_GetByteArray** bir dizedir ileti almak için işlevi.

### <a name="uninitialize-the-library"></a>Kitaplığı kapatması

Gönderen olaylar ve alıcı iletileri bittiğinde, IOT kitaplığı kapatması. Bunu yapmak için aşağıdaki işlev çağrısı yürütün:

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Bu çağrı tarafından önceden ayrılmış kaynakları boşaltır **IoTHubClient\_CreateFromConnectionString** işlevi.

Gördüğünüz gibi ile ileti gönderme ve alma kolay **IoTHubClient** kitaplığı. Kitaplık kullanmak için hangi Protokolü dahil olmak üzere IOT Hub ile iletişim ayrıntılarını işler, (Geliştirici açısından bakıldığında, bir basit yapılandırma seçeneği budur).

**IoTHubClient** kitaplığı da Cihazınızı IOT Hub'ına gönderir verilerini seri hale getirmek nasıl üzerinden kesin bir denetim sağlar. Bazı durumlarda bu düzeyi denetiminin bir avantajı, ancak diğer ile endişelenmeniz istemediğiniz bir uygulama ayrıntısı şöyle. Bu durumda, kullanarak düşünebilirsiniz **seri hale getirici** sonraki bölümde açıklanan kitaplığı.

## <a name="use-the-serializer-library"></a>Seri hale getirici kitaplığını kullanma

Kavramsal olarak **seri hale getirici** kitaplığı bulunur üstünde **IoTHubClient** SDK'sı kitaplığı. Kullandığı **IoTHubClient** kitaplığı IOT Hub, ancak temel alınan iletişimi için ileti serileştirme postalarla yük geliştiriciden kaldırmak modelleme yetenekleri ekler. Nasıl bu kitaplığı works en iyi bir örnek gösterilmiştir.

İçinde **seri hale getirici** klasöründe [azure-IOT-sdk-c deposu](https://github.com/Azure/azure-iot-sdk-c), olan bir **örnekleri** adlı bir uygulama içeren klasörde **simplesample\_mqtt**. Bu örnek Windows sürümü aşağıdaki Visual Studio çözümü içerir:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> Bu projeyi Visual Studio 2017 içinde açarsanız, en son sürüme projenizin hedefini ister kabul edin.

Önceki örnek olarak, bunun birkaç NuGet paketlerini içerir:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

Önceki örnekte, bu paketleri çoğunu gördünüz ancak **Microsoft.Azure.IoTHub.Serializer** yenidir. Bu paket kullandığınızda gereklidir **seri hale getirici** kitaplığı.

Örnek uygulama uyarlamasını bulabilirsiniz **simplesample\_mqtt.c** dosya.

Aşağıdaki bölümlerde bu örnek anahtar bölümleri size yol gösterir.

### <a name="initialize-the-library"></a>Kitaplığı başlatılamıyor

İle çalışmaya başlamak için **seri hale getirici** kitaplığı API'leri başlatma çağırın:

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

Çağrı **seri hale getirici\_init** işlevi bir kerelik çağrısı ve temel kitaplığı başlatır. Ardından, çağrı **IoTHubClient\_ÜM\_CreateFromConnectionString** giriş olarak aynı API işlevi **IoTHubClient** örnek. Bu çağrı, cihaz bağlantı dizesini ayarlar (Bu da Protokolü burada seçtiğiniz çağrıdır, kullanmak istediğiniz). Bu örnek taşıma olarak MQTT kullanır, ancak AMQP veya HTTPS kullanabilir.

Son olarak, arama **oluşturma\_modeli\_örneği** işlevi. **WeatherStation** model ad alanıdır ve **ContosoAnemometer** model adıdır. Model örneği oluşturulduktan sonra ileti gönderme ve alma başlatmak için kullanabilirsiniz. Ancak, anlaşılması önemlidir hangi bir modeldir.

### <a name="define-the-model"></a>Model tanımlayın

Bir model **seri hale getirici** kitaplığı tanımlar Cihazınızı IOT Hub'ına gönderebilirsiniz iletileri ve adlı iletileri *Eylemler* alabileceği modelleme dilde. C makroları olarak bir dizi kullanılarak bir modeli tanımlamak **simplesample\_mqtt** örnek uygulama:

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

**Başlamak\_ad alanı** ve **son\_ad alanı** makroları iki bağımsız değişken olarak model ad alanı alın. Bu makrolar arasında herhangi bir şey tanımını modelinizi veya modelleri ve modelleri kullanın ve veri yapılarını olduğunu beklenir.

Bu örnekte, yok adlı tek bir model **ContosoAnemometer**. Bu model Cihazınızı IOT Hub'ına gönderebilirsiniz veri iki parça tanımlar: **DeviceID** ve **WindSpeed**. Ayrıca Cihazınızı alabilir üç eylem (iletileri) tanımlar: **TurnFanOn**, **TurnFanOff**, ve **SetAirResistance**. Her veri öğesi türüne sahip ve her eylemi bir ad (ve isteğe bağlı bir parametre kümesi) sahiptir.

Model içinde tanımlanan eylemleri ve veri IOT Hub'ına iletileri göndermek için kullandıkları ve cihaza gönderilen iletilere yanıt API yüzeyi tanımlayın. Bu model kullanımı örnek en iyi anlaşılmalıdır.

### <a name="send-messages"></a>İleti gönder

IOT Hub'ına gönderebilirsiniz veri modelini tanımlar. Bu örnekte, iki veri öğelerinin anlaşılır bir tanımlanan kullanarak **WITH_DATA** makrosu. Göndermek için gereken birkaç adım vardır **DeviceID** ve **WindSpeed** bir IOT hub'ına değerleri. Göndermek istediğiniz veri kümesi için ilk şöyledir:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

Daha önce tanımladığınız modeli üyeleri ayarlayarak değerleri ayarlamanıza olanak tanır bir **yapısı**. Ardından, ileti göndermek istediğiniz sıralayın:

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed to serialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

Bu kod cihaz-bulut bir arabelleğe serileştirir (tarafından başvurulan **hedef**). Kod sonra çağırır **sendMessage** işlevi IOT Hub'ına ileti göndermek için:

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable to create a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


Son parametresi, ikinci **IoTHubClient\_ÜM\_SendEventAsync** verileri başarıyla gönderildiğinde çağrılan bir geri çağırma işlevini başvurudur. Aşağıdaki örnekte geri çağırma işlevi şöyledir:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

İkinci parametresi, kullanıcı bağlamı için bir işaretçidir; aynı işaretçi geçirilen **IoTHubClient\_ÜM\_SendEventAsync**. Bu durumda, bağlam basit bir sayaç olmakla birlikte, istediğiniz herhangi bir şey olabilir.

Tüm olan cihaz bulut iletilerini göndermek için yoktur. Karşılamak üzere sol yalnızca ileti alma şeydir.

### <a name="receive-messages"></a>İleti al

Yol iletileri iş ileti works benzer şekilde alma **IoTHubClient** kitaplığı. İlk olarak, bir ileti geri çağırma işlevini kaydedin:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable to IoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

Ardından, bir ileti alındığında çağrılan geri çağırma işlevi yazın:

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Bu kodu Demirbaş.--herhangi bir çözüm için aynı olur. Bu işlev iletiyi alır ve uygun işlev çağrısı aracılığıyla yönlendirme mvc'deki **yürütme\_KOMUTU**. Çağrılan işlev modelinizi Eylemler tanımını bu noktada bağlıdır.

Modelinizi bir eylem tanımladığınızda, Cihazınızı karşılık gelen ileti aldığında çağrılan işlev uygulamak için gereken. Örneğin bu eylemi modelinizi tanımlar:

```c
WITH_ACTION(SetAirResistance, int, Position)
```

Bu imza işleviyle tanımlayın:

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Nasıl işlevi modeldeki eylemin adı adıyla ve işlev parametrelerinin eylem için belirtilen parametreler eşleşen unutmayın. İlk parametre her zaman gereklidir ve model örneği için bir işaretçi içeriyor.

Cihaz Bu imza eşleşen bir ileti aldığında, ilgili işlev çağrılır. Bu nedenle, yanı sıra Demirbaş kod eklemek zorunda **IoTHubMessage**, olan modelinizde tanımlı her eylem için basit bir işlevin tanımlamanın yalnızca sağlasa da iletileri alma.

### <a name="uninitialize-the-library"></a>Kitaplığı kapatması

Verileri gönderilirken ve alıcı iletileri bittiğinde, IOT kitaplığı kapatması:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Bu üç işlevlerin her biri daha önce açıklanan üç başlatma işlevleri ile hizalar. Bu API'larını çağırma önceden ayrılan kaynakları serbest sağlar.

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede kitaplıklarda kullanma temelleri kapsamdaki **C için Azure IOT cihaz SDK'sı**. SDK'ın nelerin dahil olduğunu anlamak için yeterli bilgi sağlanan mimarisi ve Windows örnekleri ile çalışmaya başlamak nasıl. Sonraki makalede açıklayarak SDK açıklaması devam [IoTHubClient Kitaplığı hakkında daha fazla](iot-hub-device-sdk-c-iothubclient.md).

IOT Hub için geliştirme hakkında daha fazla bilgi için bkz: [Azure IOT SDK'ları][lnk-sdks].

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
