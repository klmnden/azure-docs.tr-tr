---
title: "Azure IOT cihaz SDK'sını c - sı: Iothubclient | Microsoft Docs"
description: "Bir IOT hub ile iletişim kuran cihaz uygulamaları oluşturmak için Azure IOT cihaz SDK'sını c sı: Iothubclient kitaplıkta kullanma"
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: yizhon
ms.openlocfilehash: ff766375dd9ad7cb3bbdf1ef686abb77d1206099
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797862"
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>C – sı: Iothubclient hakkında daha fazla bilgi için Azure IOT cihaz SDK'sı

[C için Azure IOT cihaz SDK'sı](iot-hub-device-sdk-c-intro.md) bu giriş serisi içinde ilk makale **C için Azure IOT cihaz SDK'sını**. Bu makalede, SDK'da iki mimari katmanları olan açıklanmıştır. Temeli **sı: Iothubclient** doğrudan IOT Hub ile iletişimi yönetir kitaplığı. Ayrıca **seri hale getirici** üstüne serileştirme hizmetleri sağlamak için derleme kitaplığı. Bu makalede, ek ayrıntılı üzerinde sağlarız **sı: Iothubclient** kitaplığı.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Önceki makalede açıklanan nasıl kullanılacağını **sı: Iothubclient** ileti IOT Hub'ına olayları göndermek ve almak için kitaplık. Bu makalede bu tartışma daha kesin bir şekilde yönetmek nasıl açıklayarak genişletir *olduğunda* için giriş verileri gönderip **alt düzey API'ler**. Olayları Özellikler ekleme (ve bunları iletileri almak) nasıl de açıklayacağız özellikleri işleme özelliğini kullanarak **sı: Iothubclient** kitaplığı. Son olarak, farklı yollarla IOT Hub'ından alınan iletileri işlemek için ek açıklama sağlarız.

Makaleyi birkaç cihaz kimlik bilgilerini ve davranışını değiştirme hakkında daha fazla dahil çeşitli konuları kapsayan sonucuna **sı: Iothubclient** yapılandırma seçenekleri üzerinden.

Kullanacağız **sı: Iothubclient** SDK örnekleri bu konularda açıklanır. Örneği takip etmek istiyorsanız, bkz. **iothub\_istemci\_örnek\_http** ve **iothub\_istemci\_örnek\_amqp**c aşağıdaki bölümlerde açıklanan her şey bu örneklerde gösterilen için Azure IOT cihaz SDK'sını dahil edilen uygulamalar.

Bulabilirsiniz [ **C için Azure IOT cihaz SDK'sını** ](https://github.com/Azure/azure-iot-sdk-c) API'de GitHub deposu ve görünüm ayrıntılarını [C API Başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/).

## <a name="the-lower-level-apis"></a>Alt düzey API'ler

Önceki makalede açıklanan temek işleyişini **sı: Iothubclient** bağlamında **iothub\_istemci\_örnek\_amqp** uygulama. Örneğin, bu kodu kullanmadan Kitaplığı'nı başlatmak nasıl açıklanmıştır.

```C
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Ayrıca, bu işlev çağrısı kullanarak olayları göndermek nasıl açıklanmaktadır.

```C
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Makale, bir geri çağırma işlevini kaydederek ileti alma de açıklanmaktadır.

```C
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Makalede ayrıca, aşağıdaki gibi bir kod kullanarak kaynakları serbest nasıl oluşturulacağını gösterir.

```C
IoTHubClient_Destroy(iotHubClientHandle);
```

Bu API'ların her biri için yardımcı işlevleri vardır:

* Sı: Iothubclient\_LL\_CreateFromConnectionString
* Sı: Iothubclient\_LL\_SendEventAsync
* Sı: Iothubclient\_LL\_SetMessageCallback
* Sı: Iothubclient\_LL\_yok

Bu işlevlerin her **LL** API adı. Diğer **LL** bölümü adı, bu işlevlerin her biri parametreleri olmayan LL karşılıkları aynıdır. Ancak, bu işlevler davranışını önemli bir biçimde farklıdır.

Çağırdığınızda **sı: Iothubclient\_CreateFromConnectionString**, arka planda çalışan yeni bir dizi temel kitaplıklar oluşturun. Bu iş parçacığı için olaylar gönderir ve IOT Hub'ından, iletileri alır. Böyle bir iş parçacığı ile çalışırken oluşturulan **LL** API'leri. Arka plan iş parçacığı oluşturma geliştiriciye kolaylık olması açısından ' dir. Açıkça olayları ileti gönderme ve IOT Hub'ından--arka planda otomatik olarak gerçekleşir alma hakkında endişelenmeniz gerekmez. Buna karşılık, **LL** API'leri size IOT Hub ile iletişim kesin denetime ihtiyacınız varsa.

Bu kavramı daha iyi anlamak için bir örneğe göz atalım:

Çağırdığınızda **sı: Iothubclient\_SendEventAsync**, gerçekte ne yaptığını olay arabellekte koyuyor. Çağırdığınızda oluşturulan arka plan iş parçacığı **sı: Iothubclient\_CreateFromConnectionString** sürekli olarak bu arabellek izler ve içerdiği herhangi bir veri IOT Hub'ına gönderir. Bu, ana iş parçacığı diğer iş gerçekleştiriyor aynı zamanda arka planda gerçekleşir.

Benzer şekilde, bir geri çağırma işlevini kullanarak ileti için kaydettiğinizde **sı: Iothubclient\_SetMessageCallback**, arka plan iş parçacığı bir ileti olduğunda geri çağırma işlevi çağırmak için SDK'sı yönergelerini içeren alınan, ana iş parçacığı bağımsızdır.

**LL** API'leri, bir arka plan iş parçacığı oluşturmayın. Bunun yerine, yeni bir API açıkça göndermek ve IOT Hub'ından veri almak için çağrılmalıdır. Bu, aşağıdaki örnekte gösterilmiştir.

**İothub\_istemci\_örnek\_http** SDK'da bulunan uygulama alt düzey API'leri gösterir. Bu örnekte, olayları IOT Hub'ına ile aşağıdaki gibi bir kod göndereceğiz:

```C
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

İlk üç satırını iletisi oluşturun ve son satırı olay gönderir. Ancak, daha önce belirtildiği gibi Olay verileri yalnızca bir arabellek yerleştirilir anlamına gelir. gönderme. Dediğimiz şey ağda iletilir **sı: Iothubclient\_LL\_SendEventAsync**. Sırada aslında giriş IOT Hub'ına verileri çağırmalısınız **sı: Iothubclient\_LL\_DoWork**, bu örnekte olduğu gibi:

```C
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(100);
}
```

Bu kod (gelen **iothub\_istemci\_örnek\_http** uygulama) tekrar tekrar çağırır **sı: Iothubclient\_LL\_DoWork**. Her zaman **sı: Iothubclient\_LL\_DoWork** olan çağrılır, bunu bazı olaylar arabellekteki IOT Hub'ına gönderir ve cihaza gönderilen bir kuyruğa alınmış ileti alır. İkinci durumda, iletiler için bir geri çağırma işlevini kaydettiğimiz, daha sonra geri çağırma (tüm iletileri kuyruğa varsayılarak) çağrılması anlamına gelir. Bu tür bir geri çağırma işlevi aşağıdaki gibi kod ile kaydettiğimiz:

```C
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Nedeni, **sı: Iothubclient\_LL\_DoWork** genellikle çağrılma yeri çağırıldığında her zaman bir döngü nedir, gönderen *bazı* IOT Hub ve alır olaylarınıarabelleğe*sonraki* ileti kuyruktaki aygıt için. Her çağrı, arabelleğe alınmış tüm olayları göndermek için garanti yoktur veya tüm almak için kuyruğa alınan iletiler. Arabellekteki tüm olayları gönderir ve ardından başka bir işlem devam istiyorsanız bu döngü kod aşağıdaki gibi değiştirebilirsiniz:

```C
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(100);
}
```

Bu kod **sı: Iothubclient\_LL\_DoWork** kadar tüm olayları arabellekteki IOT Hub'ına gönderildi. Bu da tüm kuyruğa alınmış iletiler aldınız emin göstermez unutmayın. Bunun nedeni bir parçası, "tüm" iletileri denetleme olarak belirlenimci bir eylem olmadığından emin olur. "Tüm" iletileri almak, ancak başka bir cihaza gönderilir ne hemen sonra? Programlanmış bir zaman aşımı ile birlikte dağıtılacak daha iyi bir yoludur. Örneğin, her çağrıldığında ileti geri çağırma işlevi bir zamanlayıcı sıfırlanamadı. Örneğin, son alınan ileti, işleme devam etmek için mantıksal ardından yazabileceğiniz *X* saniye.

Ne zaman olduğunuz tamamlanmış ingressing olayları ve iletileri alma kaynakları temizlemek için karşılık gelen bir işlevi çağırmak emin olun.

```C
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Temel olarak tek bir arka plan iş parçacığı ve başka bir dizi arka plan iş parçacığı olmadan aynı şeyi yapar API'si ile veri göndermek ve almak için API kümesi yok. Birçok geliştiriciyi olmayan - LL API'leri tercih edebilirsiniz, ancak alt düzey API'ler Geliştirici ağ aktarımları üzerinde kesin denetim istediğinde faydalıdır. Örneğin, bazı cihazlar veri zaman ve yalnızca giriş olayları üzerinden belirli aralıklarla (örneğin, saatte bir kez veya günde bir kez) toplar. IOT Hub'ından veri gönderip, alt düzey API'ler şunları yapabilmenize açıkça denetimi sağlar. Diğerleri yalnızca alt düzey API'ler sağlayan basitliğini tercih eder. Her şeyi, ana iş parçacığı yerine bazı iş gerçekleşmesini arka planda gerçekleşir.

Seçtiğiniz hangi modeli kullandığınız hangi API'leri tutarlı olmasını emin olun. Çağırarak başlatırsanız **sı: Iothubclient\_LL\_CreateFromConnectionString**, yalnızca kullandığınız ilgili alt düzey API'ler için herhangi bir izleme iş emin olun:

* Sı: Iothubclient\_LL\_SendEventAsync
* Sı: Iothubclient\_LL\_SetMessageCallback
* Sı: Iothubclient\_LL\_yok
* Sı: Iothubclient\_LL\_DoWork

Bunun tersi de geçerlidir. İle başlatırsanız **sı: Iothubclient\_CreateFromConnectionString**, ardından herhangi bir ek işlem olmayan - LL API'leri kullanın.

Azure IOT cihaz SDK'sını C için bkz: **iothub\_istemci\_örnek\_http** alt düzey API'ler için tam bir örnek uygulama. **İothub\_istemci\_örnek\_amqp** uygulama tam bir örnek için olmayan - LL API'leri başvurulabilir.

## <a name="property-handling"></a>Özellik işleme

Veri gönderimi açıkladığımız, şimdiye biz iletisinin gövdesine başvuran. Örneğin, bu kodu göz önünde bulundurun:

```C
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Bu örnek ileti IOT Hub'ına metniyle "Hello World." gönderir Ancak, IOT hub'ı her iletiye iliştirilecek özellikleri de sağlar. İletiye eklenmiş ad/değer çiftleri özelliklerdir. Örneğin, şu iletiye bir özellik eklemek için önceki kodu değiştirebilirsiniz:

```C
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Çağırarak Başlat **IoTHubMessage\_özellikleri** ve bizim ileti tanıtıcısını geçirme. Yeniden elde ederiz olduğu bir **harita\_İŞLEMEK** özellikleri eklemeye başlamak sağlıyor başvuru. İkinci çağrılarak gerçekleştirilir **harita\_AddOrUpdate**, bir harita başvuru alır\_tanıtıcı, özellik adı ve özellik değeri. Bu API ile istiyoruz sayıda özelliği ekleyebiliriz.

Ne zaman olay okuma gelen **Event Hubs**, alıcı özellikleri listeleme ve bunlara karşılık gelen değerler Al. Örneğin,. NET'te bu erişerek ulaşılacağına [EventData nesnesinde özellikler koleksiyonu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

Önceki örnekte, IOT Hub'ına göndereceğiz olaya biz özellikleri iliştiriliyor. Özellikleri IOT Hub'ından alınan iletileri de iliştirilebilir. İleti özelliklerini almak istiyoruz, aşağıdaki gibi kod bizim ileti geri çağırma işlevini kullanabilirsiniz:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Çağrı **IoTHubMessage\_özellikleri** döndürür **harita\_İŞLEMEK** başvuru. Biz, ardından bu başvurusuna geçirin **harita\_GetInternals** ad/değer çiftleri (özellikleri sayısı için ek olarak) bir diziye bir başvuru elde edilir. Bu noktada istiyoruz değerlerini almak için özellikleri numaralandırma ibarettir olduğu.

Özellikler, uygulamanızda kullanmak zorunda değilsiniz. Ancak, bunları olayları ayarlayın veya bunları, kuyruktan almak gerekiyorsa **sı: Iothubclient** kitaplığı kolaylaştırır.

## <a name="message-handling"></a>İleti işleme

Daha önce belirtildiği gibi ileti geldiğinde IOT Hub'ından **sı: Iothubclient** kitaplığı yanıt veren bir kayıtlı bir geri çağırma işlevini çağırarak. Ek açıklama hak bu işlevin dönüş parametresinin yoktur. Geri arama işlevinin bir alıntı işte **iothub\_istemci\_örnek\_http** örnek uygulama:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Dönüş türü olduğuna dikkat edin **IOTHUBMESSAGE\_değerlendirme\_sonucu** ve bu belirli durumda döndürürüz **IOTHUBMESSAGE\_kabul edilen**. Değiştirme Biz bu işlevden döndürebilir diğer değerleri vardır nasıl **sı: Iothubclient** kitaplığı için ileti geri çağırma tepki verir. Seçenekleri aşağıda verilmiştir.

* **IOTHUBMESSAGE\_kabul edilen** – ileti başarıyla işlendi. **Sı: Iothubclient** kitaplık değil aynı ileti yeniden ile geri çağırma işlevi çağırır.

* **IOTHUBMESSAGE\_reddedildi** : ileti işleme ve gelecekte bunun için hiçbir arzusu yoktur. **Sı: Iothubclient** kitaplığı ile aynı ileti yeniden geri çağırma işlevi değil çağırır.

* **IOTHUBMESSAGE\_ABANDONED** – ileti başarıyla işlenmedi ancak **sı: Iothubclient** kitaplığı ile aynı ileti yeniden geri çağırma işlevi çağırma.

Dönüş kodları, ilk iki için **sı: Iothubclient** kitaplığı ileti gönderir IOT Hub'ına ileti cihaz kuyruktan silinmesi ve yeniden teslim edilemedi olduğunu gösteren. Net etkisiyle (ileti, cihaz kuyruktan silinir) aynıdır, ancak iletiyi kabul veya reddedilen hala kaydedilir.  Bu fark kaydı kimin dinlemek için geri bildirim ve bir cihaz kabul ettiğini veya belirli bir iletiyi reddetti, öğrenmek Gönderenler ileti için kullanışlıdır.

Son durumda bir ileti da IOT Hub'ına gönderilir, ancak ileti yeniden teslim olduğunu gösterir. Genellikle bazı hatayla karşılaşırsanız, ancak yeniden iletiyi işlemek denemek istiyorsanız bir iletiyi bırak. Buna karşılık, bir ileti reddetme kurtarılamaz bir hata karşılaştığınızda (veya yalnızca iletiyi işlemek istemediğiniz karar verirseniz) uygundur.

İstediğiniz davranışı çözüleceği, böylece herhangi bir durumda, farklı dönüş kodlarına dikkat **sı: Iothubclient** kitaplığı.

## <a name="alternate-device-credentials"></a>Diğer cihaz kimlik bilgileri

İle çalışırken yapılacak ilk şey daha önce açıklandığı gibi **sı: Iothubclient** kitaplığı olduğundan edinmek için bir **IOTHUB\_istemci\_İŞLEMEK** aşağıdaki gibi bir çağrı ile:

```C
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Bağımsız değişkenleri **sı: Iothubclient\_CreateFromConnectionString** cihaz bağlantı dizesini ve IOT Hub ile iletişim kurmak için kullandığımız protokolü belirten bir parametre. Cihaz bağlantı dizesi şu şekilde görünen bir biçime sahiptir:

```C
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Bu dize bilgilerinde dört adet vardır: IOT Hub adına, IOT hub'ı soneki, cihaz kimliği ve paylaşılan erişim anahtarı. Azure portalında, IOT hub'ı örneği oluşturduğunuzda IOT hub'ı tam etki alanı adı (FQDN) elde — Bu, IOT hub adının (FQDN'nin ilk parçası) ile IOT hub'ı soneki (FQDN rest) sağlar. Cihazınızı IOT hub'ı ile kaydettiğinizde cihaz Kimliğine ve paylaşılan erişim anahtarı alma (açıklandığı [önceki makalede](iot-hub-device-sdk-c-intro.md)).

**Sı: Iothubclient\_CreateFromConnectionString** Kitaplığı'nı başlatmak için bir yol sağlar. İsterseniz, yeni bir oluşturabilirsiniz **IOTHUB\_istemci\_İŞLEMEK** cihaz bağlantı dizesi yerine bu tek tek parametreleri kullanarak. Bu, aşağıdaki kod ile elde edilir:

```C
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Bu aynı şeyi gerçekleştirir **sı: Iothubclient\_CreateFromConnectionString**.

Kullanmak istediğiniz belirgin görünebilir **sı: Iothubclient\_CreateFromConnectionString** başlatma daha ayrıntılı bu yöntem yerine. IOT Hub'ında bir cihaz kaydettiğinizde neler bir cihaz kimliği ve cihaz anahtarı (bağlantı dizesi değil) olduğunu, ancak unutmayın. *Cihaz Gezgini* SDK aracı sunulan [önceki makalede](iot-hub-device-sdk-c-intro.md) kitaplıkları kullanan **Azure IOT hizmeti SDK'sını** cihaz kimlik, cihaz bağlantı dizesi oluşturmak için , cihaz anahtarı ve IOT Hub ana bilgisayar adı. Yöntemini çağırır; dolayısıyla **sı: Iothubclient\_LL\_Oluştur** çünkü bir bağlantı dizesi oluşturma adım kaydeder tercih edilebilir. Hangi kullanışlı bir yöntemdir kullanın.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Şimdiye kadar şekli hakkında her şeyi anlatılan **sı: Iothubclient** kitaplığı works varsayılan davranışını yansıtır. Ancak, kitaplık çalışma şeklini değiştirmek için ayarlayabileceğiniz birkaç seçenek vardır. Bu yararlanarak gerçekleştirilir **sı: Iothubclient\_LL\_SetOption** API. Bu örneği göz önünde bulundurun:

```C
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Birkaç yaygın olarak kullanılan bir seçenek vardır:

* **SetBatching** (Boole) – varsa **true**, ardından IOT Hub'ına gönderilen veriler, toplu olarak gönderilir. Varsa **false**, sonra iletileri ayrı olarak gönderilir. Varsayılan değer **false**. AMQP üzerinden toplu işleme / AMQP WS yanı sıra Sistem özellikleri D2C iletilerde ekleme desteklenir.

* **Zaman aşımı** (işaretsiz int) – bu değeri, milisaniye cinsinden gösterilir. Bir HTTPS isteği veya bir yanıt alma bu süreden daha uzun sürer, ardından bağlantı zaman aşımına gönderiliyorsa.

Toplu işlem seçeneği büyük/küçük harf önemlidir. Varsayılan olarak, kitaplığı ingresses olaylarını tek başına (tek bir olay için ne olursa olsun, geçirdiğiniz olan **sı: Iothubclient\_LL\_SendEventAsync**). Toplu işlem seçeneği ise **true**, kitaplığı (kadar IOT hub'ı kabul edeceği maksimum ileti boyutu) arabellekteki mümkün olduğunca çok olaylarını toplar.  Olay batch (olayları tek tek bir JSON dizisi paketlenir) tek bir HTTPS çağrı IOT Hub'ına gönderilir. Ağ gidiş dönüş azaltma olduğundan, genellikle toplu etkinleştirme büyük performans artışları sonuçlanır. Bir olay şablonunu içeren HTTPS üstbilgileri kümesi yerine tek tek her olay için üstbilgileri kümesi gönderme olduğundan bant genişliği Ayrıca önemli ölçüde azaltır. Genellikle yapmak için özel bir nedeniniz yoksa, toplu işleme etkinleştirmek isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede davranışını ayrıntılı olarak **sı: Iothubclient** kitaplığı içinde bulunan **C için Azure IOT cihaz SDK'sını**. Bu bilgilerle yeteneklerini iyi anlamış olmanız gerekir **sı: Iothubclient** kitaplığı. Bu serinin ikinci makalesinde [C - seri hale getirici için Azure IOT cihaz SDK'sını](iot-hub-device-sdk-c-serializer.md), üzerinde benzer ayrıntı sağlayan **seri hale getirici** kitaplığı.

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

IOT hub'ı yeteneklerini daha iyi keşfedilebilmesi için bkz: [Azure IOT Edge dağıtımı AI uç cihazlara](../iot-edge/tutorial-simulate-device-linux.md).
