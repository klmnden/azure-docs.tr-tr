---
title: "Azure IOT cihaz SDK'sı c - IoTHubClient | Microsoft Docs"
description: "Azure IOT cihaz SDK'sı c IoTHubClient kitaplıkta bir IOT hub ile iletişim cihaz uygulamaları oluşturmak için nasıl kullanılacağını."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: 828cf2bf-999d-4b8a-8a28-c7c901629600
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: obloch
ms.openlocfilehash: 6e015d391067271cf71eb865af1b469135c8fcaa
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>C – IoTHubClient hakkında daha fazla bilgi için Azure IOT cihaz SDK'sı
[İlk makale](iot-hub-device-sdk-c-intro.md) sunulan bu serideki **C için Azure IOT cihaz SDK'sı**. Bu makale, SDK içinde iki Mimari katman vardır açıklanmıştır. Temeli **IoTHubClient** doğrudan IOT Hub ile iletişim yöneten kitaplığı. Ayrıca **seri hale getirici** serileştirme hizmetleri sağlamak için açık üst o derlemeler kitaplığı. Bu makalede ek ayrıntı sağlarız **IoTHubClient** kitaplığı.

Önceki makalede açıklanan nasıl kullanılacağını **IoTHubClient** iletileri IOT Hub'ına olayları göndermek ve almak için kitaplık. Bu makalede bu tartışma nasıl daha kesin olarak yönetilir açıklayarak genişletir *zaman* için giriş veri gönderip **düşük düzeyli API'leri**. Olayları Özellikler ekleme (ve gelen iletileri almak) nasıl ayrıca açıklayacağız özelliklerinde işleme özelliğini kullanarak **IoTHubClient** kitaplığı. Son olarak, IOT Hub'ından alınan iletileri işlemek için farklı yollar ek açıklama sağlarız.

Makaleyi birkaç aygıt kimlik bilgisi ve davranışını değiştirme hakkında daha fazla dahil çeşitli konuları kapsayan sonucuna **IoTHubClient** yapılandırma seçenekleri üzerinden.

Kullanacağız **IoTHubClient** SDK örnekleri bu konular açıklanır. İzlemek istiyorsanız, bkz: **ıothub\_istemci\_örnek\_http** ve **ıothub\_istemci\_örnek\_amqp**C. aşağıdaki bölümlerde açıklanan her şey bu örnekleri gösterilmiştir için Azure IOT cihaz SDK'sı bulunan uygulamalar.

Bulabileceğiniz [ **C için Azure IOT cihaz SDK'sı** ](https://github.com/Azure/azure-iot-sdk-c) GitHub depo ve görünüm ayrıntılarını API [C API Başvurusu](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="the-lower-level-apis"></a>Alt düzey API'ları
Temel işlemi önceki makalede açıklanan **IotHubClient** bağlamında **ıothub\_istemci\_örnek\_amqp** uygulama. Örneğin, bu kodu kullanarak kitaplığı başlatılamadı nasıl açıklanmıştır.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Ayrıca, bu işlev çağrısı kullanarak olayları göndermek nasıl açıklanmaktadır.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Makale, bir geri çağırma işlevini kaydederek ileti alma de açıklanmaktadır.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Makale ayrıca aşağıdaki gibi kod kullanarak kaynakları serbest nasıl oluşturulacağını gösterir.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Ancak bu API'leri her yardımcı işlevleri vardır:

* IoTHubClient\_ÜM\_CreateFromConnectionString
* IoTHubClient\_ÜM\_SendEventAsync
* IoTHubClient\_ÜM\_SetMessageCallback
* IoTHubClient\_ÜM\_yok

Tüm bu işlevler "Tümü" API adını içerir. İlgili dışında bu işlevlerin her biri parametrelerinin ÜM olmayan dekiler aynıdır. Ancak, bu işlevler davranışını önemli bir şekilde farklıdır.

Çağırdığınızda **IoTHubClient\_CreateFromConnectionString**, temel alınan kitaplıkları arka planda çalışan yeni bir iş parçacığı oluşturabilir. Bu iş parçacığı olayları gönderir ve IOT Hub'ından, iletileri alır. Bu tür bir iş parçacığı "ÜM ile" API'leri çalışırken oluşturulur. Arka plan iş parçacığı oluşturmayı geliştiriciye kolaylık sağlamak amacıyla ' dir. Açıkça olayları ileti gönderme ve IOT Hub'ından--arka planda otomatik olarak gerçekleşir alma hakkında endişelenmeniz gerekmez. Buna karşılık, ihtiyacınız olursa "Tümü" API'leri, IOT Hub ile iletişim üzerinde açık denetim sağlar.

Bu daha iyi anlamak için bir örneğe bakalım:

Çağırdığınızda **IoTHubClient\_SendEventAsync**, gerçekte ne gerçekleştirmekte olduğunuz olay arabellekte koyuyor. Çağırdığınızda oluşturulan arka plan iş parçacığı **IoTHubClient\_CreateFromConnectionString** sürekli olarak bu arabellek izler ve içerdiği herhangi bir veri IOT Hub'ına gönderir. Bu, ana iş parçacığının başka işler gerçekleştiriyor aynı anda arka planda gerçekleşir.

Benzer şekilde, bir geri çağırma işlevini kullanarak iletileri için kaydettiğinizde **IoTHubClient\_SetMessageCallback**, SDK'sını bir ileti olduğunda geri çağırma işlevi çağırma arka plan iş parçacığı sahip sunuculardaki gerektiğini belirten alınan, ana iş parçacığı bağımsızdır.

"Tümü" API'leri arka plan iş parçacığı oluşturmayın. Bunun yerine, yeni bir API açıkça göndermek ve IOT Hub'ından veri almak için çağrılmalıdır. Bu aşağıdaki örnekte gösterilmiştir.

**Iothub\_istemci\_örnek\_http** SDK'da bulunan uygulama alt düzey API'ları gösterir. Bu örnekte, biz olayları IOT Hub'ına aşağıdaki gibi kod ile gönder:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

İlk üç satırını iletisi oluşturun ve son satırında olay gönderir. Ancak, daha önce belirtildiği gibi "Olay verileri yalnızca bir arabellek yerleştirilir anlamına gelir gönderme". Biz çağırdığınızda hiçbir şey ağ üzerinde aktarılan **IoTHubClient\_ÜM\_SendEventAsync**. Gerçekte giriş verileri IOT Hub'ına için sırayla çağırmalısınız **IoTHubClient\_ÜM\_DoWork**, bu örnekteki gibi:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Bu kod (gelen **ıothub\_istemci\_örnek\_http** uygulama) art arda çağırır **IoTHubClient\_ÜM\_DoWork**. Her zaman **IoTHubClient\_ÜM\_DoWork** olan çağrılır, bazı olaylar arabelleğinden IOT Hub'ına gönderir ve cihaza gönderilen sıraya alınmış bir iletiyi alır. İkinci durumda, biz iletiler için bir geri çağırma işlevini kaydettiyseniz, daha sonra geri çağırma (herhangi bir ileti kuyruğa varsayılarak) çağrılması anlamına gelir. Biz bu tür bir geri çağırma işlevi aşağıdaki gibi kod ile kayıtlı:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Bunun nedeni, **IoTHubClient\_ÜM\_DoWork** çoğunlukla adı verilir bir döngü değil, her kez çağırıldığında, gönderir *bazı* IOT Hub ve alır olaylarınıarabelleğe*sonraki* iletisi sıraya cihaz için. Her çağrı tüm arabelleğe alınmış olayları göndermek için garantisi yoktur veya tüm almak için sıraya alınan iletileri. Arabellekte tüm olayları göndermek ve ardından başka bir işlem ile çalışmaya devam etmek istiyorsanız, aşağıdaki gibi kod ile bu döngü değiştirebilirsiniz:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Bu kod çağırır **IoTHubClient\_ÜM\_DoWork** arabelleği tüm olayları IOT hub'a gönderilen kadar. Bu ayrıca tüm sıraya alınan iletileri alınmış olan anlamına gelmez unutmayın. Bunun nedeni parçası "tümü" iletileri denetleme gibi belirleyici bir eylem olmadığından emin olur. "Tüm" iletileri almak, ancak başka bir cihaza gönderilir ne olur hemen sonra? Programlı bir zaman aşımı ile ile mücadele etmek için daha iyi bir yoludur. Örneğin, ileti geri çağırma işlevi çağrılması her zaman bir süreölçeri sıfırlanamadı. Örneğin, son alınan ileti değilse, işleme devam etmek için mantığı sonra yazabilirsiniz *X* saniye.

Ne zaman tamamlanmış ingressing olayları olduğunuz ve iletilerini alma kaynakları temizlemek için karşılık gelen işlevi çağırdığınızdan emin olun.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Temel olarak yalnızca bir dizi arka plan iş parçacığı ve başka bir dizi arka plan iş parçacığı olmadan aynı şeyi yapar API ile veri göndermek ve almak için API yoktur. Geliştiricilerin çok olmayan - ÜM API'leri tercih edebilirsiniz, ancak geliştirici ağ aktarımları üzerinde açık denetim istediğinde düşük düzeyli API'leri yararlıdır. Örneğin, bazı aygıtlar veri zaman ve yalnızca giriş olayları belirtilen aralıklarla (örneğin, saatte bir kez veya günde bir kez) toplar. IOT Hub'ından veri gönderip alırken düşük düzeyli API'leri, açıkça denetimine sağlıyor. Başkalarının yalnızca alt düzey API'ler sağlar basitliğini tercih eder. Bazı iş oluşmasını arka planda yerine ana iş parçacığı her şeyi olur.

Seçtiğiniz hangi model kullandığınız hangi API'leri tutarlı olacak şekilde emin olun. Çağırarak başlatırsanız **IoTHubClient\_ÜM\_CreateFromConnectionString**, yalnızca kullandığınız karşılık gelen alt düzey API'ları için herhangi bir izleme iş emin olun:

* IoTHubClient\_ÜM\_SendEventAsync
* IoTHubClient\_ÜM\_SetMessageCallback
* IoTHubClient\_ÜM\_yok
* IoTHubClient\_ÜM\_DoWork

Bunun tersi de geçerlidir. İle başlatırsanız, **IoTHubClient\_CreateFromConnectionString**, ardından herhangi bir ek işlem dışı - ÜM API'leri kullanın.

Azure IOT cihazı SDK C için bkz: **ıothub\_istemci\_örnek\_http** düşük düzeyli API'leri tam bir örnek uygulama. **Iothub\_istemci\_örnek\_amqp** uygulama tam bir örnek için harici - ÜM API'leri başvuruda bulunabilir.

## <a name="property-handling"></a>Özellik işleme
Verileri gönderilirken açıklanan zaman kadarki biz ileti gövdesini başvuran. Örneğin, bu kod göz önünde bulundurun:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Bu örnek ileti IOT Hub'ına metinle "Hello World." gönderir Ancak, IOT hub'ı her iletisine iliştirilecek özellikleri de sağlar. İletiye eklenmiş ad/değer çiftleri özelliklerdir. Örneğin, biz iletiye bir özellik eklemek için önceki kod değiştirebilirsiniz:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Arayarak Başlat **IoTHubMessage\_özellikleri** ve bizim ileti işleyicisini geçirme. Ne biz geri alırsınız bir **harita\_İŞLEMEK** Özellikler ekleme başlatmak için bize etkinleştirir başvuru. İkinci çağırarak gerçekleştirilir **harita\_örnek**, bir harita başvuru alır\_TANITICISI, özellik adı ve özellik değeri. Bu API ile biz istediğiniz sayıda özellik ekleyebiliriz.

Ne zaman olay okunur gelen **olay hub'ları**, alıcı özellikler numaralandırılmaya ve bunlara karşılık gelen değerler alır. Örneğin, .NET içinde bu erişerek gerçekleştirilmesi [EventData nesne üzerindeki özellikleri koleksiyonu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

Önceki örnekte, biz özellikleri size IOT Hub'ına göndereceğiz olaya ekleniyor. Özellikler, IOT Hub'ından alınan iletileri da eklenebilir. Biz iletiden özelliklerini almak istiyorsanız, aşağıdaki gibi kod bizim ileti geri çağırma işlevini kullanabilirsiniz:

```
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

Çağrı **IoTHubMessage\_özellikleri** döndürür **harita\_İŞLEMEK** başvuru. Biz sonra bu başvuruyu iletebilir **harita\_GetInternals** ad/değer çiftleri (yanı sıra özellikleri sayısı) dizisi başvuru elde edilir. Bu noktada istiyoruz değerlerini almak için özellikleri numaralandırma, atmaktan değil.

Uygulamanızda özelliklerini kullanmak zorunda değilsiniz. Ancak, olaylarına ayarlamak veya gelen iletileri almak gerekiyorsa **IoTHubClient** kitaplığı kolaylaştırır.

## <a name="message-handling"></a>İleti işleme
Daha önce belirtildiği gibi iletiler geldiğinde IOT Hub'ından **IoTHubClient** kitaplığı yanıt kayıtlı geri çağırma işlevini çağırarak. Bazı ek açıklama hak bu işlevin dönüş parametresi yok. Geri çağırma işlevi bir alıntı işte **ıothub\_istemci\_örnek\_http** örnek uygulama:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Dönüş türü olduğuna dikkat edin **IOTHUBMESSAGE\_değerlendirme\_sonuç** ve bu belirli durumda döndürürüz **IOTHUBMESSAGE\_kabul edilen**. Değiştirme Biz bu işlevden dönebilirsiniz diğer değerler nasıl **IoTHubClient** kitaplığı tepki verdiğini için ileti geri çağırma. Seçenekler aşağıda belirtilmiştir.

* **IOTHUBMESSAGE\_kabul edilen** – iletiyi başarıyla işledi. **IoTHubClient** kitaplığı değil aynı ileti yeniden ile geri çağırma işlevi çağırır.
* **IOTHUBMESSAGE\_reddedildi** – ileti işlenmedi ve gelecekte bunun için hiçbir desire yoktur. **IoTHubClient** kitaplığı çağrılamadı aynı ileti yeniden ile geri çağırma işlevi.
* **IOTHUBMESSAGE\_ABANDONED** – ileti başarıyla işlenmedi ancak **IoTHubClient** kitaplığı aynı ileti yeniden ile geri çağırma işlevi çağırma.

İlk iki dönüş kodları, için **IoTHubClient** kitaplığı ileti gönderir IOT Hub'ına ileti aygıt sıradan silinir ve gerekir yeniden teslim edilmedi olduğunu gösteren. Net etkisi (ileti aygıt sıradan silinir) aynıdır, ancak ileti kabul edilen veya reddedilen hala kaydedilir.  Bu ayrım kaydı geri bildirim için dinleyen ve bir cihaz kabul ettiğini veya belirli bir ileti reddedildi varsa öğrenmek Gönderenler iletinin için yararlıdır.

Son durumda bir ileti da IOT Hub'ına gönderilir, ancak ileti yeniden teslim olduğunu gösterir. Genellikle bazı hatayla karşılaşırsanız, ancak iletiyi yeniden işlemek denemek istiyorsanız bir ileti abandon. Buna karşılık, bir ileti reddetme kurtarılamaz bir hatayla karşılaştığında (veya yalnızca iletiyi işlemek istemediğinize karar verirseniz) uygundur.

Böylece, istediğiniz davranışı tasarlanmalıdır herhangi bir durumda, farklı dönüş kodları unutmayın **IoTHubClient** kitaplığı.

## <a name="alternate-device-credentials"></a>Alternatif aygıt kimlik bilgisi
İle çalışırken yapılacak ilk şey daha önce açıklandığı gibi **IoTHubClient** kitaplığıdır elde etmek için bir **IOTHUB\_istemci\_İŞLEMEK** aşağıdaki gibi bir çağrıyla:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Bağımsız değişkenleri **IoTHubClient\_CreateFromConnectionString** cihaz bağlantı dizesini ve IOT Hub ile iletişim kurmak için kullandığımız protokolü belirten bir parametre. Cihaz bağlantı dizesi şu şekilde görünen bir biçime sahiptir:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Bu dize bilgilerinde dört adet vardır: IOT hub'ı adı, IOT hub'ı soneki, cihaz kimliği ve paylaşılan erişim anahtarı. Azure portalında, IOT hub örneği oluşturduğunuzda IOT hub'ı tam etki alanı adı (FQDN) elde — Bu, IOT hub adını (FQDN ilk parçası) ve IOT hub soneki (FQDN rest) sunar. IOT Hub ile Cihazınızı kaydederken cihaz kimliği ve paylaşılan erişim anahtarı almanız (açıklandığı gibi [önceki makalede](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** kitaplığı başlatmak için bir yol sağlar. İsterseniz, yeni bir oluşturabileceğiniz **IOTHUB\_istemci\_İŞLEMEK** cihaz bağlantı dizesi yerine bu tek tek parametreleri kullanarak. Bu, aşağıdaki kod ile sağlanır:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Bu aynı gerçekleştirir **IoTHubClient\_CreateFromConnectionString**.

Kullanmak istediğiniz belirgin görünebilir **IoTHubClient\_CreateFromConnectionString** başlatma daha ayrıntılı bu yöntem yerine. IOT Hub ' bir aygıtı kaydederken bir cihaz kimliği ve aygıt anahtarı (olmayan bir bağlantı dizesi) aldığınızdır olduğunu, ancak unutmayın. *Aygıt explorer* SDK Aracı kullanıma sunulan [önceki makalede](iot-hub-device-sdk-c-intro.md) kitaplıklarında kullanan **Azure IOT hizmeti SDK'sını** aygıt Kimliğinden cihaz bağlantı dizesi oluşturmak için , aygıt anahtarı ve IOT Hub ana bilgisayar adı. Bu nedenle çağırma **IoTHubClient\_ÜM\_oluşturma** bir bağlantı dizesi oluşturma adım ortadan kaldırdığı için tercih olabilir. Hangi yöntemi uygundur kullanın.

## <a name="configuration-options"></a>Yapılandırma seçenekleri
Şimdiye kadar her şeyi şekilde anlatılan **IoTHubClient** kitaplığı works varsayılan davranışını yansıtır. Ancak, kitaplık çalışma şeklini değiştirmek için ayarlayabileceğiniz birkaç seçenek vardır. Bu yararlanarak gerçekleştirilir **IoTHubClient\_ÜM\_SetOption** API. Bu örneği göz önünde bulundurun:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Birkaç yaygın olarak kullanılan seçenekler şunlardır:

* **SetBatching** (Boole) – varsa **doğru**, sonra da IOT Hub'ına gönderilen verileri toplu olarak gönderilir. Varsa **yanlış**, ayrı ayrı gönderilen iletileri sonra. Varsayılan değer **false**. Unutmayın **SetBatching** seçeneği yalnızca MQTT veya AMQP protokollerini değil de, HTTPS protokolü için geçerlidir.
* **Zaman aşımı** (imzasız int) – bu değer milisaniye cinsinden gösterilir. Bir HTTPS isteği ya da bir yanıt alırken bu süreden daha uzun sürer gönderme, ardından bağlantı zaman aşımına uğradı.

Toplu işleme seçeneği önemlidir. Varsayılan olarak, kitaplık ingresses olayları ayrı ayrı (tek bir olay için ne olursa olsun, geçirdiğiniz olan **IoTHubClient\_ÜM\_SendEventAsync**). Toplu işleme seçeneği ise **doğru**, kitaplık (kadar IOT hub'ı kabul edeceği maksimum ileti boyutu) arabellekteki mümkün olduğunca çok olaylarını toplar.  Olay toplu IOT Hub'ına (olayları tek tek bir JSON dizisine paketlenmiştir) tek bir HTTPS çağrı gönderilir. Ağ gidiş dönüş azaltma bu yana genellikle toplu etkinleştirme büyük performans artışları sonuçlanır. Tek bir olay toplu ile HTTPS üstbilgi kümesi yerine tek tek her olay için üstbilgi kümesi gönderme beri bant genişliği de önemli ölçüde azaltır. Genellikle, aksi takdirde yapmak için özel bir nedeniniz yoksa, toplu etkinleştirme istersiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede davranışını ayrıntılı olarak **IoTHubClient** kitaplığı bulunan **C için Azure IOT cihaz SDK'sı**. Bu bilgiyle özelliklerini iyi anlamış olmanız **IoTHubClient** kitaplığı. [Sonraki makalede](iot-hub-device-sdk-c-serializer.md) benzer ayrıntı sağlar **seri hale getirici** kitaplığı.

IOT Hub için geliştirme hakkında daha fazla bilgi için bkz: [Azure IOT SDK'ları][lnk-sdks].

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [Bir aygıt ile Azure IOT kenar benzetimini yapma][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
