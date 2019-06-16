---
title: C için Azure IOT cihaz SDK'sı | Microsoft Docs
description: C için Azure IOT cihaz SDK'sını kullanmaya başlayın ve IOT hub ile iletişim kuran cihaz uygulamaları oluşturmayı öğrenin.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: yizhon
ms.openlocfilehash: d758d761e560642de76e149c83fc6898aa78bafb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65910334"
---
# <a name="azure-iot-device-sdk-for-c"></a>C için Azure IOT cihaz SDK'sı

**Azure IOT cihaz SDK'sını** ileti gönderilmesi ve gelen iletileri alma işlemini basitleştirmek için tasarlanmış bir dizi kitaplıktır **Azure IOT hub'ı** hizmeti. Her belirli bir platform hedefleme SDK'ın farklı Çeşitleme vardır, ancak bu makalede **C için Azure IOT cihaz SDK'sını**.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

C için Azure IOT cihaz SDK'sı, ANSI C taşınabilirliği en üst düzeye (C99) yazılır. Bu özellik kitaplıkları birden çok platform ve cihazlarda, özellikle disk en aza burada çalışmaya uygun hale getirir ve bellek Ayak izi bir önceliktir.

Geniş bir Platform SDK'sı edilmiştir vardır (bkz [IOT cihaz kataloğu için Azure sertifikalı](https://catalog.azureiotsolutions.com/) Ayrıntılar için). Bu makalede Windows platformunda çalışan örnek kodunun izlenecek yollar bulunmasına karşın, bu makalede açıklanan kod aralığını desteklenen platformlar arasında aynıdır.

Aşağıdaki video Azure IOT SDK'sı genel bir bakış için C: sunar.

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-IoT-C-SDK-insights/Player]

Bu makalede c için Azure IOT cihaz SDK'sını mimarisini sunar Bu cihaz kitaplığı başlatılamıyor, IOT Hub'ına veri göndermek ve buradan ileti almak nasıl gösterir. Bu makaledeki bilgiler, SDK'sı ile çalışmaya başlamak için yeterli olmalıdır, ancak ayrıca işaretçileri kitaplıkları hakkında ek bilgi sağlar.

## <a name="sdk-architecture"></a>SDK mimarisi

Bulabilirsiniz [ **C için Azure IOT cihaz SDK'sını** ](https://github.com/Azure/azure-iot-sdk-c) API'de GitHub deposu ve görünüm ayrıntılarını [C API Başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/).

Kitaplıklarının en son sürümünü bulunabilir **ana** deponun dalı:

  ![Ana dalın depo ekran görüntüsü](./media/iot-hub-device-sdk-c-intro/RepoMasterBranch.png)

* Temel uygulama SDK'ın bulunduğu **iothub\_istemci** SDK en düşük API katmanı uygulamasını içeren klasörü: **sı: Iothubclient** kitaplığı. **Sı: Iothubclient** API'leri ham IOT Hub'ına ileti göndermek ve IOT Hub'ından iletiler alan ileti uygulama kitaplığı içerir. Bu kitaplık kullanırken, ileti serileştirme uygulamak için sorumlu olduğunuz, ancak sizin için IOT Hub ile iletişim kuran diğer ayrıntıları işlenir.

* **Seri hale getirici** klasörü yardımcı işlevleri ve istemci kitaplığını kullanarak Azure IOT Hub'ına göndermeden önce verileri seri hale getirme işlemini gösteren örnekleri içerir. Seri hale getiricinin kullanımını zorunlu değildir ve bir kolaylık olarak sağlanır. Kullanılacak **seri hale getirici** kitaplığı, IOT Hub ve ondan almaya beklediğiniz iletileri göndermek için verileri belirten bir model tanımlayın. Modelde tanımlandıktan sonra SDK serileştirme ayrıntılar hakkında endişelenmeden CİHAZDAN buluta ve bulut-cihaz iletilerini ile kolayca çalışmanızı sağlayan bir API yüzeyi sağlar. Kitaplık MQTT ve AMQP gibi protokoller kullanarak aktarım uygulayan diğer açık kaynak kitaplıkları bağlıdır.

* **Sı: Iothubclient** kitaplığı diğer açık kaynak kitaplıkları'nı bağlıdır:

  * [Azure C paylaşılan yardımcı programı](https://github.com/Azure/azure-c-shared-utility) (örneğin, dizeler, listesi işlemelerinin yapıldığı ve g/ç) temel görevler için ortak işlevselliği birden fazla Azure ile ilgili C SDK'ları arasında gerekli sağlayan bir kitaplık.

  * [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) bir istemci-tarafı uygulaması AMQP kısıtlı kaynak cihazlar için iyileştirilmiş olan bir kitaplık.

  * [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) ve MQTT protokolünü uygulayan genel amaçlı bir kitaplık sunulmaktadır ve kısıtlı kaynak cihazlar için iyileştirilmiş, kitaplığı.

Bu kitaplıklar kullanımını örnek koda baktığınızda anlamak daha kolay olur. Aşağıdaki bölümler size SDK'da bulunan örnek uygulamaları birkaç yol. Bu izlenecek yol SDK'ın mimari katmanları ve giriş API'leri çalışmasında çeşitli özelliklerini için iyi bir genel görünüm vermeniz gerekir.

## <a name="before-you-run-the-samples"></a>Bu örneği çalıştırmadan önce

C için Azure IOT cihaz SDK'sı örnekleri çalıştırmadan önce şunları yapmalısınız [IOT Hub hizmeti örneği oluşturma](iot-hub-create-through-portal.md) Azure aboneliğinizdeki. Ardından, aşağıdaki görevleri tamamlayın:

* Geliştirme ortamınızı hazırlama
* Cihaz kimlik bilgilerini edinin.

### <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Paketleri yaygın platformlar (örneğin, NuGet için Windows veya apt_get Debian ve Ubuntu) için sağlanır ve örnekleri kullanılabilir olduğunda bu paketleri kullanın. Bazı durumlarda Cihazınızda veya için SDK derleme gerekir. SDK'yı derlemek ihtiyacınız varsa bkz [geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) GitHub deposunda.

Örnek uygulama kodu almak için SDK'sı bir kopyasını Github'dan indirin. Kaynak kopyasını alma **ana** dalı [GitHub deposu](https://github.com/Azure/azure-iot-sdk-c).


### <a name="obtain-the-device-credentials"></a>Cihaz kimlik bilgilerini alma

Örnek kaynak koda sahip olduğunuza göre bir sonraki şey yapmak için bir cihaz kimlik bilgileri kümesi almaktır. Bir cihaz IOT hub'ı erişebilmesi IOT Hub kimlik kayıt defterinde bir cihaz ilk eklemeniz gerekir. Cihazınızı eklediğinizde, cihazın IOT hub'ına bağlanabilmeniz gereken cihaz kimlik bilgileri kümesi alır. Sonraki bölümde açıklanan örnek uygulamalar, bu kimlik bilgileri biçiminde beklediğiniz bir **cihaz bağlantı dizesini**.

IOT hub'ınıza yönetmenize yardımcı olmak üzere çeşitli açık kaynak araçlar vardır.

* Adlı bir Windows uygulaması [cihaz Gezgini](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

* Platformlar arası Visual Studio Code uzantısı adlı [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

* Platformlar arası Python CLI adlı [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

Bu öğreticide grafik *cihaz Gezgini* aracı. Kullanabileceğiniz *VS Code için Azure IOT Araçları* VS Code'da geliştirme yapıyorsanız. Ayrıca *Azure CLI 2.0 için IOT uzantısı* CLI aracını kullanmayı tercih ederseniz aracı.

Device explorer aracı, IOT Hub cihaz ekleme dahil olmak üzere, çeşitli işlevleri gerçekleştirmek için Azure IOT hizmeti kitaplıklarını kullanır. Bir cihaz eklemek için device explorer aracı kullanırsanız, cihazınız için bir bağlantı dizesini alın. Örnek uygulamaları çalıştırmak için bu bağlantı dizesi gerekir.

Device explorer aracı ile ilgili bilgi sahibi değilseniz, aşağıdaki yordamı, bir cihaz eklemek ve bir cihaz bağlantı dizesini almak için nasıl kullanılacağını açıklar.

1. Device explorer aracı yüklemek için bkz [Device Explorer için IOT Hub cihazlarını nasıl](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

1. Programını çalıştırdığınızda, bu arabirim bakın:

   ![Device Explorer İkizi ekran görüntüsü](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinConfigTab.png)

1. Girin, **IOT Hub bağlantı dizesine** ilk alan tıklayıp **güncelleştirme**. Bu adım, IOT Hub ile iletişim kurabilmesi için Aracı'nı yapılandırır. 

**Bağlantı dizesi** altında bulunan **IOT Hub hizmeti** > **ayarları** > **paylaşılan erişim ilkesi**  >  **iothubowner**.

1. IOT Hub bağlantı dizesine yapılandırıldığında tıklayın **Yönetim** sekmesinde:

   ![Device Explorer İkizi / yönetim ekran görüntüsü](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinManagementTab.png)

Bu sekme, IOT hub'ına kayıtlı cihazları yönettiğiniz kullanılabilir.

1. Tıklayarak bir cihaz oluşturun **Oluştur** düğmesi. Bir iletişim kutusu, bir dizi önceden doldurulmuş anahtarlar (birincil ve ikincil) birlikte görüntüler. Girin bir **cihaz kimliği** ve ardından **Oluştur**.

   ![Cihaz ekran oluşturma](./media/iot-hub-device-sdk-c-intro/CreateDevice.png)

1. Cihaz oluşturulduğunda, cihazları yeni oluşturduğunuz de dahil olmak üzere tüm kayıtlı cihazlarla güncelleştirmeleri listeleyin. Yeni Cihazınızı sağ tıkladığınızda bu menü bakın:

   ![Device Explorer İkizi sağ sonucu](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinManagementTab_RightClick.png)

1. Seçerseniz **seçili cihaz için bağlantı dizesini kopyalayın**, cihaz bağlantı dizesini panoya kopyalandı. Cihaz bağlantı dizesini bir kopyasını tutun. Aşağıdaki bölümlerde açıklanan örnek uygulamaları çalıştırırken gerekir.

Yukarıdaki adımları tamamladıktan sonra biraz kod çalıştırmaya başlamak hazırsınız demektir. Çoğu örnekleri bir sabit bir bağlantı dizesi girin olanak tanıyan ana kaynak dosyasının en üstüne sahip. Örneğin, karşılık gelen satırından **iothub_client\_örnekleri\_iothub_convenience_sample** uygulamayı şu şekilde görünür.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-the-iothubclient-library"></a>Sı: Iothubclient kitaplığını kullanma

İçinde **iothub\_istemci** klasöründe [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) deposu yok bir **örnekleri** adlıuygulamayıiçerenklasörde**iothub\_istemci\_örnek\_mqtt**.

Windows sürümü **iothub_client\_örnekleri\_iothub_convenience_sample** uygulama aşağıdaki Visual Studio çözümünü içerir:

  ![Visual Studio Çözüm Gezgini](./media/iot-hub-device-sdk-c-intro/iothub-client-sample-mqtt.png)

> [!NOTE]
> Visual Studio en son sürüme projeyi yeniden hedefle isterse, istemi kabul edin.

Bu çözüm, tek bir proje içerir. Bu çözümde yüklü dört NuGet paketi vardır:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

Her zaman gerekecektir **Microsoft.Azure.C.SharedUtility** paketini SDK ile çalışırken. Bu örnek ve MQTT protokolünü kullanır, bu nedenle içermelidir **Microsoft.Azure.umqtt** ve **Microsoft.Azure.IoTHub.MqttTransport** paketleri (AMQP ve HTTPS için eşdeğer paketleri yoktur). Örnek kullandığından **sı: Iothubclient** kitaplığı de dahil etmelisiniz **Microsoft.Azure.IoTHub.IoTHubClient** pakette çözümünüze.

Örnek uygulama için uygulama bulabilirsiniz **iothub_client\_örnekleri\_iothub_convenience_sample** kaynak dosyası.

Ne kullanmak için gerekli yoluyla gösterilmesi için bu örnek uygulama aşağıdaki adımları kullanın **sı: Iothubclient** kitaplığı.

### <a name="initialize-the-library"></a>Kitaplığı başlatılamıyor

> [!NOTE]
> Kitaplıkları ile çalışmaya başlamadan önce bazı platforma özgü başlatma gerçekleştirmek gerekebilir. Örneğin, Linux üzerinde AMQP kullanmayı planlıyorsanız OpenSSL kitaplığını başlatmanız gerekir. Örnekler, [GitHub deposu](https://github.com/Azure/azure-iot-sdk-c) yardımcı işlevini çağırın **platform\_init** istemci ne zaman başlar ve çağrı **platform\_deinit**çıkmadan önce işlevi. Bu işlevler platform.h üstbilgi dosyasında bildirilir. Bu işlevler için hedef platformunuzu tanımlarını incelemek [depo](https://github.com/Azure/azure-iot-sdk-c) istemcinizde herhangi bir platforma özgü başlatma kodu eklemek zorunda olup olmadığını belirlemek için.

Kitaplıkları ile çalışmaya başlamak için öncelikle bir IOT Hub istemci işleyicisi atayın:

```c
if ((iotHubClientHandle = 
  IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

Bu işleve device explorer Aracı ' aldığınız cihaz bağlantı dizesini bir kopyasını geçirdiğiniz. Ayrıca, iletişim protokolü kullanmak için de belirleyin. Bu örnek, MQTT kullanır, ancak AMQP ve HTTPS ayrıca seçeneklerdir.

Geçerli bir olduğunda **IOTHUB\_istemci\_İŞLEMEK**, için ve IOT Hub'ından iletiler gönderip almak için API'leri çağırma başlayabilirsiniz.

### <a name="send-messages"></a>İleti gönderme

Örnek uygulama, IOT hub'ınıza ileti göndermek için bir döngü ayarlar. Aşağıdaki kod parçacığı:

- Bir ileti oluşturur.
- Bir özellik ekler.
- Bir ileti gönderir.

İlk olarak bir ileti oluşturun:

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

İleti gönderdiğiniz her gönderinin bir başvuru verileri gönderildiğinde, çağrılan bir geri çağırma işlevini belirtin. Bu örnekte geri çağırma işlevi çağrılır **SendConfirmationCallback**. Aşağıdaki kod parçacığını bu geri çağırma işlevini gösterir:

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

Çağrı unutmayın **IoTHubMessage\_Destroy** iletinin işiniz bittiğinde işlev. Bu işlev, ileti oluştururken ayrılan kaynakları serbest bırakır.

### <a name="receive-messages"></a>İleti alma

Bir mesaj bir zaman uyumsuz bir işlemdir. İlk olarak, cihaz bir ileti aldığında çağrılacak geri arama kaydedin:

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

İstediğiniz void bir işaretçi son parametredir. Örnekte, bir tamsayı işaretçisi olan ancak daha karmaşık bir veri yapısına bir işaretçi olabilir. Bu parametre, paylaşılan durumuyla bu işlev üzerinde çağıranın çalışmaya geri çağırma işlevi sağlar.

Kayıtlı bir geri çağırma işlevi, cihaz bir ileti aldığında çağrılır. Bu geri çağırma işlevini alır:

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

Kullanım **IoTHubMessage\_GetByteArray** işlevi bir dize iletisi alınamıyor.

### <a name="uninitialize-the-library"></a>Kitaplık geri alınıyor

Olayları gönderme ve alma iletileri işiniz bittiğinde, IOT kitaplık geri alınıyor. Bunu yapmak için aşağıdaki işlev çağrısı yürütün:

```c
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Bu çağrı tarafından önceden ayrılan kaynakları serbest bırakan **sı: Iothubclient\_CreateFromConnectionString** işlevi.

Gördüğünüz gibi ile ileti göndermek ve almak kolay **sı: Iothubclient** kitaplığı. Kitaplığı kullanmak için hangi protokolün de dahil olmak üzere IOT Hub ile iletişim kurarken, ayrıntılarını işler (Geliştirici açısından bakıldığında, basit bir yapılandırma seçeneği budur).

**Sı: Iothubclient** kitaplığı, cihazınızın IOT Hub'ına gönderir verileri seri hale getirme nasıl üzerinde kesin denetim de sağlar. Bazı durumlarda bu denetim düzeyi avantajlı değildir, ancak diğerleri ile ilgili olmasını istemediğiniz bir uygulama ayrıntısı olduğunu. Bu durumda, kullanmayı düşünebilirsiniz **seri hale getirici** kitaplığı, sonraki bölümde açıklanmıştır.

## <a name="use-the-serializer-library"></a>Seri hale getirici kitaplığını kullanma

Kavramsal olarak **seri hale getirici** kitaplığı yer alan üst kısmındaki **sı: Iothubclient** SDK kitaplığı. Kullandığı **sı: Iothubclient** IOT Hub, ancak temel alınan iletişimi için kitaplığı, ileti serileştirme uğraşmanızı yük geliştiriciden kaldırmak modelleme özellikleri ekler. Bu kitaplığı çalışır nasıl en iyi bir örnekle gösterilmiştir.

İçinde **seri hale getirici** klasöründe [azure-iot-sdk-c deposu](https://github.com/Azure/azure-iot-sdk-c), olan bir **örnekleri** adlı bir uygulama içeren klasörde **simplesample\_mqtt**. Windows sürümü bu örnek, aşağıdaki Visual Studio çözümünü içerir:

  ![Mqtt örnek için Visual Studio çözümü](./media/iot-hub-device-sdk-c-intro/simplesample_mqtt.png)

> [!NOTE]
> Visual Studio en son sürüme projeyi yeniden hedefle isterse, istemi kabul edin.

İle önceki örnek olarak, bunun çeşitli NuGet paketlerini içerir:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

Önceki örnekte, bu paketlerin en gördüğünüz ancak **Microsoft.Azure.IoTHub.Serializer** yenidir. Bu paket kullandığınızda gereklidir **seri hale getirici** kitaplığı.

Örnek uygulamada uygulamasını bulabilirsiniz **iothub_client\_örnekleri\_iothub_convenience_sample** dosya.

Aşağıdaki bölümlerde, bu örnek anahtar bölümleri arasında yol.

### <a name="initialize-the-library"></a>Kitaplığı başlatılamıyor

İle çalışmaya başlamak için **seri hale getirici** Kitaplığı Başlatma API çağrısı:

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

Çağrı **seri hale getirici\_init** işlevi tek seferlik bir çağrıdır ve temel alınan kitaplığı başlatır. Ardından, arama **sı: Iothubclient\_LL\_CreateFromConnectionString** işlevin giriş olarak aynı API **sı: Iothubclient** örnek. Bu çağrı, cihaz bağlantısı dizeniz ayarlar (Bu çağrı, ayrıca burada protokol'ni seçin, kullanmak istediğiniz). Bu örnek, aktarım olarak MQTT kullanır, ancak AMQP veya HTTPS kullanabilir.

Son olarak, çağrı **Oluştur\_modeli\_örneği** işlevi. **WeatherStation** modelin ad alanıdır ve **ContosoAnemometer** modeli adıdır. Model örneği oluşturulduktan sonra ileti gönderme ve alma başlatmak için kullanabilirsiniz. Ancak, bunu anlaşılması önemlidir hangi bir modeldir.

### <a name="define-the-model"></a>Model tanımlama

Bir modeldeki **seri hale getirici** kitaplığı tanımlar, cihazınızın IOT Hub'ına gönderebilirsiniz iletileri ve denen bir ileti *eylemleri* alabileceği modelleme dilde. Bir model kullanarak bir dizi C makroları olarak tanımlama **iothub_client\_örnekleri\_iothub_convenience_sample** örnek uygulama:

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

**Başlamak\_ad alanı** ve **son\_ad alanı** makroları her iki ad alanı modeli bir bağımsız değişken olarak alın. Bu makrolar arasında herhangi bir şey tanımını modelinizi veya modelleri ve modelleri kullanan veri yapıları olan beklenmektedir.

Bu örnekte, adlı tek bir model yok **ContosoAnemometer**. Bu model, cihazınızın IOT Hub'ına gönderebilirsiniz veri iki parçasını tanımlar: **DeviceID** ve **WindSpeed**. Ayrıca, cihaz alabilir üç eylem (iletiler) tanımlar: **TurnFanOn**, **TurnFanOff**, ve **SetAirResistance**. Her veri öğesini bir tür varsa ve her eylem bir ad (ve isteğe bağlı olarak bir parametre kümesi).

IOT Hub'ına ileti göndermek için kullanın ve cihaza gönderilen iletilere yanıt veren bir API yüzeyi veri ve modelde tanımlı eylemleri tanımlayın. Bu model kullanımını en iyi bir örnek anlaşılır.

### <a name="send-messages"></a>İleti gönderme

IOT Hub'ına gönderebilirsiniz veri modelini tanımlar. Bu örnekte, iki veri öğelerinin anlaşılır biri kullanılarak tanımlanır **WITH_DATA** makrosu. Göndermek için gereken birkaç adım vardır **DeviceID** ve **WindSpeed** bir IOT hub'ına değerleri. Göndermek istediğiniz veri kümesi için ilk şöyledir:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

Daha önce tanımladığınız modeli üyelerinin değerlerini ayarlamak sağlayan bir **yapı**. Ardından, göndermek istediğiniz iletinin seri hale getirme:

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

Bu kod, cihaz-bulut bir arabelleğe serileştirir (tarafından başvurulan **hedef**). Ardından kod çağırır **sendMessage** işlevi, IOT Hub'ına ileti göndermek için:

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

Son parametresi ikinci **sı: Iothubclient\_LL\_SendEventAsync** verileri başarıyla gönderildiğinde çağrılan bir geri çağırma işlevi bir başvurudur. Aşağıdaki örnekte geri çağırma işlevi şöyledir:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

İkinci parametre kullanıcı bağlamı işaretçisidir; aynı işaretçi geçirilen **sı: Iothubclient\_LL\_SendEventAsync**. Bu durumda, basit bir sayaç bağlamıdır, ancak istediğiniz herhangi bir şey olabilir.

Tüm CİHAZDAN buluta iletileri göndermeye yoktur. İleti alma karşılamak için gereken tek şey var.

### <a name="receive-messages"></a>İleti alma

Yol iletileri iş benzer şekilde bir ileti works alma **sı: Iothubclient** kitaplığı. İlk olarak, bir ileti geri çağırma işlevini kaydedin:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, 
  IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable to IoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

Ardından, bir ileti alındığında çağrılan geri çağırma işlevi yazma:

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

Bu kod Demirbaş--herhangi bir çözüm için aynı olacak. Bu işlev iletiyi alır ve uygun işlev çağrısı aracılığıyla yönlendirme üstlenir **yürütme\_komut**. Çağrılan işlev bu noktada eylemleri modelinizdeki tanımına göre değişir.

Modelinizde bir eylem tanımladığınızda, Cihazınızı karşılık gelen ileti aldığında çağrılan bir işlev uygulamak için gereken. Örneğin bu eylem, modelinize tanımlar:

```c
WITH_ACTION(SetAirResistance, int, Position)
```

Bu imzası olan bir fonksiyon tanımlayın:

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

İşlevin adını modelinde eylemin adı nasıl eşleştiğini ve işlev parametrelerinin eylemi için belirtilen parametreler eşleştiğini unutmayın. İlk parametre, her zaman gereklidir ve modelinizi bir örneğine bir işaretçi içerir.

Cihaz, bu imzayla eşleşen bir ileti aldığında, karşılık gelen işlev çağrılır. Bu nedenle, ortak kod eklemek zorunda dışında **IoTHubMessage**, modelinizde tanımlı her eylem için basit bir işlevi tanımlayan adımlarından olduğundan ileti alma.

### <a name="uninitialize-the-library"></a>Kitaplık geri alınıyor

Veri gönderen ve alıcı iletileri işiniz bittiğinde, IOT kitaplık geri alınıyor:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Üç bu işlevlerin her biri daha önce açıklanan üç başlatma işlevleri ile hizalar. Bu API'leri çağırmak için daha önce ayrılan kaynakları serbest sağlar.

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede ele kitaplıklarda kullanmanın temellerini **C için Azure IOT cihaz SDK'sını**. SDK'da nelerin dahil olduğunu anlamak için yeterli bilgi sağlanan mimarisinin yanı sıra, Windows örnekleri ile çalışmaya başlama. Sonraki makalede açıklayarak SDK açıklamasını devam [sı: Iothubclient Kitaplığı hakkında daha fazla](iot-hub-device-sdk-c-iothubclient.md).

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
