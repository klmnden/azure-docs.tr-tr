---
title: Azure IOT Hub kimlik kayıt defteri anlama | Microsoft Docs
description: Geliştirici Kılavuzu - IOT Hub kimlik kayıt defteri ve cihazlarınızı yönetmek için kullanma açıklaması. Toplu olarak içeri veya dışarı cihaz kimlikleri hakkında bilgi içerir.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 4e23b70c8dc5fdacfd609fb4664a78293b9e2362
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43247654"
---
# <a name="understand-the-identity-registry-in-your-iot-hub"></a>IOT hub'ınızdaki kimlik kayıt defterinde anlama

Her IOT hub, IOT hub'ına bağlanmasına izin verilen modüller ve cihazlar hakkında bilgi depolayan bir kimlik kayıt defteri sahiptir. IOT hub'a bir cihaz veya modül bağlanabilmeleri için cihaz veya IOT hub'ının kimlik kayıt defteri modülü için bir giriş olmalıdır. Bir cihaz veya modül ile de IOT hub'ının kimlik kayıt defterinde depolanan kimlik bilgileri temel kimlik doğrulaması gerekir.

Kimlik kayıt defterinde depolanan cihaz veya modül kimliği büyük/küçük harf duyarlıdır.

Yüksek bir düzeyde kimlik kayıt defteri REST özellikli, cihaz veya modül kimliği kaynakları koleksiyonudur. Kimlik kayıt defterinde girişi eklediğinizde, IOT Hub cihaz başına kaynakları uçuşan bulut-cihaz iletilerini içeren sırası gibi bir dizi oluşturur.

Gerektiğinde, kimlik kayıt defterini kullanın:

* Cihaz sağlama veya IOT hub'ınıza bağlanan modüller.
* Her cihaz/başına-modül, hub'ın cihaz veya modül'e yönelik uç erişimi denetler.

> [!NOTE]
> Kimlik kayıt defteri, uygulamaya özgü meta verileri içermiyor.

## <a name="identity-registry-operations"></a>Kimlik kayıt defteri işlemleri

IOT Hub kimlik kayıt defteri aşağıdaki işlemleri gösterir:

* Cihaz veya modül kimliği oluşturma
* Cihaz veya modül kimliği güncelleştir
* Cihaz veya modül kimliği Kimliğe göre Al
* Cihaz veya modül Kimliği Sil
* 1000'e kadar kimlikleri listesi
> Modül kimliği ve modül ikizi genel Önizleme aşamasındadır. Genel olduğunda, modül kimliği özelliği desteklenecektir kullanılabilir.
* Azure blob depolama alanına cihaz kimliklerini dışarı aktarma
* Cihaz kimliklerini Azure blob depolama alanından içeri aktarma

Tüm bu işlemler belirtilen iyimser eşzamanlılık kullanabilirsiniz [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> Bir IOT hub'ının kimlik kayıt defterinde tüm kimlikleri almak için tek yolu kullanmaktır [dışarı] [ lnk-export] işlevselliği.

Bir IOT Hub kimlik kayıt defteri:

* Uygulama meta verileri içermiyor.
* Bir sözlük gibi kullanarak erişilebilir **DeviceID** veya **Moduleıd** anahtar.
* İfadesel sorguları desteklemez.

Bir IOT çözümünü genellikle uygulamaya özgü meta veriler içeren ayrı bir çözüme özel depo vardır. Örneğin, bir akıllı yapı çözümü çözüme özel depoda bir sıcaklık Sensörüyle dağıtıldığı yer kaydeder.

> [!IMPORTANT]
> Yalnızca kimlik kayıt defteri, cihaz yönetimi ve sağlama işlemleri için kullanın. Yüksek aktarım hızı işlemleri çalışma zamanında kimlik kayıt defterinde işlemleri üzerinde bağlı olmaması gerekir. Örneğin, bir komut göndermeden önce bir cihazın bağlantı durumunu denetleme desteklenen düzeni değil. Mutlaka denetleyin [azaltma hızları] [ lnk-quotas] kimlik kayıt defteri ve [cihaz sinyal] [ lnk-guidance-heartbeat] deseni.

## <a name="disable-devices"></a>Cihazları devre dışı bırak

Güncelleştirerek cihazları devre dışı bırakabilirsiniz **durumu** kimlik kayıt defterinde bir kimlik özelliği. Genellikle, bu özelliğin iki senaryoda kullanır:

* Sağlama bir düzenleme işlemi sırasında. Daha fazla bilgi için [cihaz sağlama][lnk-guidance-provisioning].
* Herhangi bir nedenle düşünüyorsanız, bir cihaz tehlikeye veya yetkisiz haline gelmiştir.

Bu özellik modüller için kullanılamıyor.

## <a name="import-and-export-device-identities"></a>Cihaz kimliklerini içeri ve dışarı aktarma

Zaman uyumsuz işlemler kullanın [IOT hub'ı kaynak sağlayıcısı uç noktası] [ lnk-endpoints] cihaz kimliklerinin toplu bir IOT hub'ının kimlik kayıt defterinden dışarı aktarmak için. Dışarı aktarmalar kimlik kayıt defterinden okuma cihaz kimlik verilerini kaydetmek için bir müşteri tarafından sağlanan blob kapsayıcısını kullanan uzun süren işlerdir.

Zaman uyumsuz işlemler kullanın [IOT hub'ı kaynak sağlayıcısı uç noktası] [ lnk-endpoints] bir IOT hub'ının kimlik kayıt defterine toplu cihaz kimliklerini almak için. İçeri aktarmalar kimlik verilerini cihaz kimlik kayıt defterine yazmak için bir müşteri tarafından sağlanan blob kapsayıcısında veri kullanan uzun süren işlerdir.

İçeri ve dışarı aktarma API'leri hakkında daha fazla bilgi için bkz: [IOT hub'ı kaynak sağlayıcısı REST API'leri][lnk-resource-provider-apis]. İşleri dışarı aktar ve içeri aktarma çalıştırma hakkında daha fazla bilgi edinmek için bkz: [IOT Hub cihaz kimliklerinin toplu Yönetimi][lnk-bulk-identity].

## <a name="device-provisioning"></a>Cihaz sağlama

Belirli bir IOT çözüm depolar cihaz verilerini bu çözüm belirli gereksinimlerine bağlıdır. Ancak, minimum olarak, cihaz kimliklerini ve kimlik doğrulaması anahtarlarını bir çözümün depolamanız gerekir. Azure IOT Hub, her bir cihaz kimlikleri ve kimlik doğrulama anahtarlarını durum kodları gibi değerlerini depolayan bir kimlik kayıt defteri içerir. Bir çözüm, tablo depolama, blob depolama veya Cosmos DB gibi diğer Azure Hizmetleri, herhangi bir ek cihaz verilerini depolamak için kullanabilirsiniz.

*Cihaz sağlama* çözümünüzü depolarında ilk cihaz verilerini ekleme işlemidir. Hub'ınıza bağlanmak yeni bir cihaz etkinleştirmek için IOT Hub kimlik kayıt defterinde bir cihaz kimliği ve anahtarlar eklemelisiniz. Hazırlama işleminin bir parçası, cihaza özgü diğer çözüm depoları verilerde başlatmak gerekebilir. Azure IOT Hub cihazı sağlama hizmeti, sıfır dokunma, yalnızca bir veya daha fazla IOT hub'lara kullanıcı müdahalesine gerek kalmadan sağlama zamanında etkinleştirmek için de kullanabilirsiniz. Daha fazla bilgi için bkz. [sağlama hizmeti belgeleri][lnk-dps].

## <a name="device-heartbeat"></a>Cihaz sistem durumu

IOT Hub kimlik kayıt defteri adlı bir alanı içeren **connectionState**. Yalnızca **connectionState** geliştirme ve hata ayıklama sırasında alan. IOT çözümleri çalışma zamanında sorgu alanı değil. Örneğin, değil sorgu **connectionState** bulut-cihaz iletisi veya SMS göndermeden önce bir aygıt bağlı olup olmadığını denetlemek için alan. Abone öneririz [ **cihaz bağlantısı kesildi** olay](https://docs.microsoft.com/azure/iot-hub/iot-hub-event-grid#event-types) uyarılar alın ve cihaz bağlantı durumunu izlemek için Event Grid hakkında. Bunu kullanın [öğretici](https://docs.microsoft.com/azure/event-grid/publish-iot-hub-events-to-logic-apps) IOT çözümünüzü IOT Hub'ından olayları tümleştirme hakkında bilgi edinmek için.

IOT çözümünüzün bir cihaz bağlıysa, uygulayabilirsiniz bilmeniz gerekiyorsa *sinyal deseni*.
Sinyal desende cihaz her zaman (örneğin, saatte en az bir kez) en az bir kez sabit miktarda CİHAZDAN buluta iletiler gönderir. Bu nedenle, bir cihazda göndermek için herhangi bir veri yok olsa bile, yine de (genellikle bir sinyal tanımlayan bir özelliği) ile boş bir CİHAZDAN buluta ileti gönderir. Hizmet tarafında, çözüm ile her cihaz için alınan son sinyal bir harita tutar. Çözüm CİHAZDAN beklenen süre içinde bir sinyal ileti almazsa, cihaz ile ilgili bir sorun olduğunu varsayar.

Daha karmaşık bir uygulama bilgileri içerebilir [işlem izleme] [ lnk-devguide-opmon] bağlanmak veya iletişim kurmak çalışıyor ancak başarısız olan cihazlar tanımlamak için. Sinyal desenini uyguladığınızda, denetlediğinizden emin olun [IOT Hub kotaları ve kısıtlamaları][lnk-quotas].

> [!NOTE]
> Yalnızca bulut-cihaz iletilerini göndermek belirlemek için bağlantı durumu bir IOT çözümünü kullanır ve iletileri olmayan cihazların büyük kümelerine yayın basit kullanmayı *kısa süre sonu* deseni. Bu düzen daha verimli olmanın yanı sıra sinyal deseni kullanarak cihaz bağlantı durumu kayıt defteri koruma aynı sonucu elde eder. İleti onayları istek, IOT Hub hakkında hangi cihazların ileti alabilen hangilerinin bildirimde bulunabilir.

## <a name="device-and-module-lifecycle-notifications"></a>Cihaz ve modül yaşam döngüsü bildirimleri

Kimlikteki oluşturulduğunda veya yaşam döngüsü bildirimleri göndererek silindi, IOT Hub, IOT çözümünüzün bildirebilir. Bunu yapmak için IOT çözümünüzün yönlendirme oluşturma ve veri kaynağı eşit ayarlamak için gereken *DeviceLifecycleEvents* veya *ModuleLifecycleEvents*. Varsayılan olarak, hiçbir yaşam döngüsü bildirimleri gönderilir, diğer bir deyişle, bu tür bir yol önceden mevcut. Bildirim iletisi, özellikleri ve gövde içerir.

Özellikler: İleti sistemi özellikleri ile ön ekli `'$'` simgesi.

Cihaz için bildirim iletisi:

| Ad | Değer |
| --- | --- |
|$content-türü | uygulama/json |
|$iothub-enqueuedtime |  Bildirim zaman gönderildiği zaman |
|$iothub-message-kaynak | deviceLifecycleEvents |
|$content-kodlama | UTF-8 |
|opType | **createDeviceIdentity** veya **deleteDeviceIdentity** |
|HubName | IOT hub'ı adı |
|deviceId | Cihaz kimliği |
|operationTimestamp | İşlemin ISO8601 zaman damgası |
|ıothub ileti şeması | deviceLifecycleNotification |

Gövde: Bu bölümde, JSON biçimindedir ve ikizi oluşturulmuş cihaz kimliğini temsil eder. Örneğin,

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "etag":"AAAAAAAAAAE=",
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        }
    }
}
```
Bildirim iletisi modülü için:

| Ad | Değer |
| --- | --- |
$content-türü | uygulama/json |
$iothub-enqueuedtime |  Bildirim zaman gönderildiği zaman |
$iothub-message-kaynak | moduleLifecycleEvents |
$content-kodlama | UTF-8 |
opType | **createModuleIdentity** veya **deleteModuleIdentity** |
HubName | IOT hub'ı adı |
Modül kimliği | Modül kimliği |
operationTimestamp | İşlemin ISO8601 zaman damgası |
ıothub ileti şeması | moduleLifecycleNotification |

Gövde: Bu bölümde, JSON biçimindedir ve ikizi oluşturulan modülü kimliğini temsil eder. Örneğin,

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "moduleId":"tempSensor",
    "etag":"AAAAAAAAAAE=",
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        }
    }
}
```

## <a name="device-identity-properties"></a>Cihaz kimlik özellikleri

Cihaz kimliklerini aşağıdaki özelliklerle JSON belgeleri olarak temsil edilir:

| Özellik | Seçenekler | Açıklama |
| --- | --- | --- |
| deviceId |gerekli, salt okunur güncelleştirmeleri |ASCII 7 bit alfasayısal karakterlerin yanı sıra belirli özel karakterler büyük küçük harfe duyarlı bir dize (en çok 128 karakterden uzun): `- . + % _ # * ? ! ( ) , = @ $ '`. |
| Generationıd |gerekli, salt okunur |Bir IOT hub tarafından oluşturulan, büyük küçük harfe duyarlı dize en fazla 128 karakter. Bu değer aynı cihazlar ayrım yapmak için kullanılan **DeviceID**silinmesi ve yeniden oluşturulacak. |
| ETag |gerekli, salt okunur |Bir cihaz kimliği için zayıf bir ETag başına olarak temsil eden bir dize [RFC7232][lnk-rfc7232]. |
| kimlik doğrulama |isteğe bağlı |Kimlik bilgileri ve güvenlik malzemeleri içeren bir bileşik nesne. |
| auth.symkey |isteğe bağlı |Base64 biçiminde depolanan birincil ve ikincil bir anahtar içeren bir bileşik nesne. |
| durum |gerekli |Bir erişim göstergesi. Olabilir **etkin** veya **devre dışı bırakılmış**. Varsa **etkin**, cihaz bağlanmasına izin verilir. Varsa **devre dışı bırakılmış**, bu cihaz herhangi bir cihaz'e yönelik uç noktaya erişilemiyor. |
| statusReason |isteğe bağlı |Cihaz kimlik durumun nedenini depolar 128 karakter uzunluğundaki dize. Tüm UTF-8 karakterlere izin verilir. |
| statusUpdateTime |salt okunur |Son durum güncelleştirme saati ve tarihi gösteren bir zamana bağlı göstergesi. |
| connectionState |salt okunur |Bağlantı durumu gösteren bir alan: ya da **bağlı** veya **bağlantısı kesilmiş**. Bu alan, cihaz bağlantı durumunun IOT Hub görünümünü temsil eder. **Önemli**: Bu alan yalnızca geliştirme/hata ayıklama amacıyla kullanılmalıdır. Bağlantı durumu yalnızca MQTT veya AMQP kullanan cihazlar için güncelleştirilir. Ayrıca, protokol düzeyinde ping (ping MQTT veya AMQP ping) dayalıdır ve yalnızca 5 dakikada en fazla gecikme olabilir. Bu nedenlerden dolayı olabilir hatalı pozitif sonuçları gibi cihazları bağlı olarak bildirilen ancak, kesilir. |
| connectionStateUpdatedTime |salt okunur |Bağlantı durumu son bir saat ve tarihi gösteren bir zamana bağlı göstergesi güncelleştirildi. |
| lastActivityTime |salt okunur |Zamana bağlı bir göstergesi son bir saat ve tarihi cihazı gösteren bağlı, alınan veya gönderilen ileti. |

> [!NOTE]
> Bağlantı durumu yalnızca bağlantının durumunu IOT Hub görünümünü temsil edebilir. Ağ koşulları ve yapılandırmaları bağlı olarak bu durum güncelleştirmeleri gecikebilir.

## <a name="module-identity-properties"></a>Modülü kimlik özellikleri

Modül kimlikleri, aşağıdaki özelliklere sahip JSON belgeleri olarak temsil edilir:

| Özellik | Seçenekler | Açıklama |
| --- | --- | --- |
| deviceId |gerekli, salt okunur güncelleştirmeleri |ASCII 7 bit alfasayısal karakterlerin yanı sıra belirli özel karakterler büyük küçük harfe duyarlı bir dize (en çok 128 karakterden uzun): `- . + % _ # * ? ! ( ) , = @ $ '`. |
| Modül kimliği |gerekli, salt okunur güncelleştirmeleri |ASCII 7 bit alfasayısal karakterlerin yanı sıra belirli özel karakterler büyük küçük harfe duyarlı bir dize (en çok 128 karakterden uzun): `- . + % _ # * ? ! ( ) , = @ $ '`. |
| Generationıd |gerekli, salt okunur |Bir IOT hub tarafından oluşturulan, büyük küçük harfe duyarlı dize en fazla 128 karakter. Bu değer aynı cihazlar ayrım yapmak için kullanılan **DeviceID**silinmesi ve yeniden oluşturulacak. |
| ETag |gerekli, salt okunur |Bir cihaz kimliği için zayıf bir ETag başına olarak temsil eden bir dize [RFC7232][lnk-rfc7232]. |
| kimlik doğrulama |isteğe bağlı |Kimlik bilgileri ve güvenlik malzemeleri içeren bir bileşik nesne. |
| auth.symkey |isteğe bağlı |Base64 biçiminde depolanan birincil ve ikincil bir anahtar içeren bir bileşik nesne. |
| durum |gerekli |Bir erişim göstergesi. Olabilir **etkin** veya **devre dışı bırakılmış**. Varsa **etkin**, cihaz bağlanmasına izin verilir. Varsa **devre dışı bırakılmış**, bu cihaz herhangi bir cihaz'e yönelik uç noktaya erişilemiyor. |
| statusReason |isteğe bağlı |Cihaz kimlik durumun nedenini depolar 128 karakter uzunluğundaki dize. Tüm UTF-8 karakterlere izin verilir. |
| statusUpdateTime |salt okunur |Son durum güncelleştirme saati ve tarihi gösteren bir zamana bağlı göstergesi. |
| connectionState |salt okunur |Bağlantı durumu gösteren bir alan: ya da **bağlı** veya **bağlantısı kesilmiş**. Bu alan, cihaz bağlantı durumunun IOT Hub görünümünü temsil eder. **Önemli**: Bu alan yalnızca geliştirme/hata ayıklama amacıyla kullanılmalıdır. Bağlantı durumu yalnızca MQTT veya AMQP kullanan cihazlar için güncelleştirilir. Ayrıca, protokol düzeyinde ping (ping MQTT veya AMQP ping) dayalıdır ve yalnızca 5 dakikada en fazla gecikme olabilir. Bu nedenlerden dolayı olabilir hatalı pozitif sonuçları gibi cihazları bağlı olarak bildirilen ancak, kesilir. |
| connectionStateUpdatedTime |salt okunur |Bağlantı durumu son bir saat ve tarihi gösteren bir zamana bağlı göstergesi güncelleştirildi. |
| lastActivityTime |salt okunur |Zamana bağlı bir göstergesi son bir saat ve tarihi cihazı gösteren bağlı, alınan veya gönderilen ileti. |

## <a name="additional-reference-material"></a>Ek başvuru malzemesi

IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar.
* [Azaltma ve kotalar] [ lnk-quotas] kotaları açıklar ve IOT Hub hizmetine geçerli davranışlara azaltma.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.
* [IOT Hub sorgu dili] [ lnk-query] IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için kullanabileceğiniz bir sorgu dili açıklar.
* [IOT hub'ı MQTT desteği] [ lnk-devguide-mqtt] ve MQTT protokolünü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub kimlik kayıt defteri kullanmayı öğrendiğinize göre aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilginizi çekebilir:

* [IOT hub'a erişimi denetleme][lnk-devguide-security]
* [Durum ve yapılandırmaları eşitlemek için cihaz ikizlerini kullanma][lnk-devguide-device-twins]
* [Bir cihazda doğrudan yöntem çağırma][lnk-devguide-directmethods]
* [Birden fazla cihazda işleri zamanlama][lnk-devguide-jobs]

Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğreticiye bakın:

* [Azure IOT Hub ile çalışmaya başlama][lnk-getstarted-tutorial]

IOT Hub cihazı sağlama hizmeti kullanarak müdahalesi gerektirmeyen, tam zamanında sağlama etkinleştireceğinizi öğrenmek için keşfetmek için: 

* [Azure IOT Hub cihazı sağlama hizmeti][lnk-dps]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: quickstart-send-telemetry-dotnet.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
