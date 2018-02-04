---
title: "Azure IOT Hub kimlik kayıt defteri anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - açıklaması IOT Hub kimlik kayıt defteri ve cihazlarınızı yönetmek için kullanma. Toplu olarak içeri ve dışarı aktarma cihaz kimlikleri hakkında bilgi içerir."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0706eccd-e84c-4ae7-bbd4-2b1a22241147
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 50020f007096b45b843515ff765e40c550fcf4e3
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="understand-the-identity-registry-in-your-iot-hub"></a>IOT hub'ınızdaki kimlik kayıt defterinde anlama

Her IOT hub'ı IOT hub'ına bağlanmasına izin verilen cihazlar hakkındaki bilgileri depolayan bir kimlik kayıt defteri sahiptir. Bir cihaz IOT hub'a bağlanmadan önce bir cihazın IOT hub'ın kimlik kayıt defterinde girişi olmalıdır. Ayrıca, bir cihaz kimlik kayıt defterinde depolanan kimlik bilgileri dayalı IOT hub ile doğrulaması gerekir.

Kimlik kayıt defterinde depolanan cihaz kimliği büyük/küçük harf duyarlıdır.

Yüksek düzeyde, kimlik kayıt defteri REST özellikli cihaz kimlik kaynakları koleksiyonudur. Bir giriş kimlik kayıt defterinde eklediğinizde, IOT Hub cihaz başına kaynakları yürütülen bulut-cihaz iletilerini içeren sıranın gibi bir dizi oluşturur.

Gerektiğinde kimlik kayıt defteri kullanın:

* IOT hub'ınıza bağlanın cihazları sağlamak.
* Hub'ın aygıt'e yönelik uç noktalar aygıt başına erişimi denetler.

> [!NOTE]
> Kimlik kayıt defteri herhangi bir uygulamaya özgü meta veri içermiyor.

## <a name="identity-registry-operations"></a>Kimlik kayıt defteri işlemleri

IOT Hub kimlik kayıt defteri aşağıdaki işlemleri sunar:

* Cihaz kimliği oluşturma
* Güncelleştirme cihaz kimliği
* Cihaz kimliği Kimliğine göre alma
* Cihaz Kimliği Sil
* En fazla 1000 kimlikleri listesi
* Tüm kimlikler Azure blob depolama alanına dışarı aktarma
* Azure blob depolama alanından kimlikleri alma

Bu işlemler belirtildiği gibi iyimser eşzamanlılık kullanabilirsiniz [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> Bir IOT hub'ın kimlik kayıt defterinde tüm kimlikleri almanın tek yolu kullanmaktır [verme] [ lnk-export] işlevselliği.

Bir IOT Hub kimlik kayıt defteri:

* Herhangi bir uygulama meta veri içermiyor.
* Kullanarak bir sözlük gibi erişilebilir **DeviceID** anahtar olarak.
* Açıklayıcı sorguları desteklemez.

Bir IOT çözüm genellikle uygulamaya özgü meta veriler içeren ayrı bir çözüme özel deposu vardır. Örneğin, bir akıllı yapı çözümü çözüme özel deposunda sıcaklık algılayıcısı dağıtıldığı yer kaydedebilir.

> [!IMPORTANT]
> Yalnızca kimlik kayıt defteri aygıt yönetimi ve işlemleri sağlama için kullanın. Çalışma zamanında yüksek verimlilik işlemleri kimlik kayıt defterinde işlemlerini gerçekleştirme üzerinde bağlı olmaması gerekir. Örneğin, bir cihaz bağlantı durumunun bir komut göndermeden önce desteklenen desen olmadığı denetleniyor. Denetlediğinizden emin olun [oranları azaltma] [ lnk-quotas] kimlik kayıt defteri ve [aygıt sinyal] [ lnk-guidance-heartbeat] düzeni.

## <a name="disable-devices"></a>Cihazları devre dışı bırak

Güncelleştirerek aygıtlarını devre dışı bırakabilir **durum** kimlik kayıt defterinde bir kimlik özelliği. Genellikle, iki senaryoda bu özelliği kullanın:

* Sağlama orchestration sürecinde. Daha fazla bilgi için bkz: [cihaz sağlamayı][lnk-guidance-provisioning].
* Herhangi bir nedenle düşünüyorsanız, bir cihaz tehlikeye veya yetkisiz haline gelmiştir.

## <a name="import-and-export-device-identities"></a>İçeri ve dışarı aktarma aygıt kimlikleri

Zaman uyumsuz işlemleri kullanılacağı [IOT hub'ı kaynak sağlayıcı uç] [ lnk-endpoints] bir IOT hub'ın kimlik kayıt defterinden toplu cihaz kimliklerini vermek için. Olan kimlik kayıt defterinden okunacak aygıt kimlik verilerini kaydetmek için müşteri tarafından sağlanan blob kapsayıcısı kullanmak uzun süre çalışan işleri dışarı aktarır.

Zaman uyumsuz işlemleri kullanılacağı [IOT hub'ı kaynak sağlayıcı uç] [ lnk-endpoints] bir IOT hub'ın kimlik kayıt defterine toplu cihaz kimliklerini almak için. İçeri aktarmalar olan kimlik kayıt defterine aygıt kimlik verilerini yazmak için bir müşteri tarafından sağlanan blob kapsayıcısında verileri kullanmak uzun süre çalışan işleri.

İçeri ve dışarı aktarma API'ler hakkında daha fazla bilgi için bkz: [IOT hub'ı kaynak sağlayıcısı REST API'leri][lnk-resource-provider-apis]. İçeri aktarma çalıştırma hakkında daha fazla bilgi edinin ve işleri dışarı aktarmak için bkz: [toplu yönetim IOT Hub cihaz kimlikleri][lnk-bulk-identity].

## <a name="device-provisioning"></a>Cihaz sağlama

Belirli bir IOT çözüm depolar cihaz verileri bu çözüm belirli gereksinimlerine bağlıdır. Ancak, en az bir çözüm cihaz kimliklerini ve kimlik doğrulama anahtarları depolamanız gerekir. Azure IOT Hub kimlikleri, kimlik doğrulama anahtarları ve durum kodları gibi her bir cihaz için değerler saklayabilirsiniz bir kimlik kayıt defterini içerir. Bir çözüm, herhangi bir ek aygıt veri depolamak için tablo, blob depolama veya Cosmos DB gibi diğer Azure hizmetleriyle kullanabilirsiniz.

*Cihaz sağlama* ilk cihaz veri depolarına çözümünüzdeki ekleme işlemidir. Hub'ınıza bağlanmak yeni bir cihaz etkinleştirmek için bir cihaz kimliği ve anahtarları IOT Hub kimlik kayıt defterine eklemeniz gerekir. Hazırlama işleminin bir parçası olarak, diğer çözüm depolarında aygıta özgü verileri başlatma gerekebilir. Azure IOT Hub cihaz sağlama hizmeti, sıfır-touch, yalnızca insan etkileşimi olmadan bir veya daha fazla IOT hub'ları için sağlama zaman etkinleştirmek için de kullanabilirsiniz. Daha fazla bilgi için bkz: [hizmet belgeleri sağlama][lnk-dps].

## <a name="device-heartbeat"></a>Cihaz sinyal

IOT Hub kimlik kayıt adlı bir alanı içeren **connectionState**. Yalnızca **connectionState** geliştirme ve hata ayıklama sırasında alan. IOT çözümleri alanın çalışma zamanında sorgu değil. Örneğin, değil sorgu **connectionState** alan bir bulut cihaz ileti veya SMS göndermeden önce bir aygıt bağlı olup olmadığını denetleyin.

IOT çözümünüzün bir aygıt bağlıysa, uygulamanız gerekir bilmeniz gerekiyorsa *sinyal düzeni*.

Sinyal desende aygıt en az bir kez her sabit süreyi (örneğin, en az bir kez her saat) cihaz bulut iletilerini gönderir. Bu nedenle, bir cihaz göndermek için herhangi bir veri sahip değilse, hala bir boş cihaz bulut iletisiyle (genellikle bir sinyal tanımlayan bir özellik) gönderir. Hizmet tarafında çözümü her cihaz için alınan son sinyal ile bir eşleme tutar. Çözüm aygıttan beklenen süre içinde bir sinyal iletisi almazsa, cihaz ile ilgili bir sorun olduğunu varsayar.

Daha karmaşık bir uygulama bilgileri içerebilir [izleme işlemleri] [ lnk-devguide-opmon] bağlanmak veya iletişim kurmak çalışıyor, ancak başarısız olan cihazları tanımlayabilirsiniz. Sinyal düzeni uyguladığınızda olduğundan emin olun [IOT Hub kotaları ve kısıtlamaları][lnk-quotas].

> [!NOTE]
> Yalnızca bulut cihaz iletileri gönderilip gönderilmeyeceğini belirlemek için bağlantı durumu IOT çözümünü kullanır ve iletileri değil aygıtlarını büyük kümeleri için yayını basit kullanmayı *kısa süre sonu zamanı* düzeni. Bu desen daha verimli devam ederken sinyal desenini kullanarak bir aygıtı bağlantı durumu kayıt Bakımı aynı sonucu elde eder. İleti onayları isterse, IOT Hub, hangi aygıtların hakkında iletiler alabilir ve hangi bildirebilir.

## <a name="device-lifecycle-notifications"></a>Cihaz yaşam döngüsü bildirimleri

Bir cihaz kimliği oluşturulduğunda veya cihaz yaşam döngüsü bildirimleri göndererek silinmiş IOT hub'ı IOT çözümünüzü bildirebilir. Bunu yapmak için IOT çözümünüzün bir rota oluşturmak ve veri kaynağı eşit ayarlamak için gereken *DeviceLifecycleEvents*. Varsayılan olarak, hiçbir yaşam döngüsü bildirimleri gönderilir, diğer bir deyişle, bu tür bir yollar önceden mevcut. Bildirim iletisi özelliklerini ve gövde içerir.

Özellikler: İleti sistemi özellikleri ile önek `'$'` simgesi.

| Ad | Değer |
| --- | --- |
$content-türü | uygulama/json |
$iothub-enqueuedtime |  Zaman zaman bildirim gönderildi |
$iothub-message-source | deviceLifecycleEvents |
$content-encoding | utf-8 |
opType | **createDeviceIdentity** veya **deleteDeviceIdentity** |
hubName | IOT hub'ının adı |
deviceId | Cihaz kimliği |
operationTimestamp | İşlemin ISO8601 zaman damgası |
iothub-message-schema | deviceLifecycleNotification |

Gövde: Bu bölümde, JSON biçiminde ve twin oluşturulan cihaz kimliğini temsil eder. Örneğin,

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

## <a name="device-identity-properties"></a>Cihaz kimlik özellikleri

Bir cihaz kimlikleri aşağıdaki özelliklerle JSON belgeleri olarak temsil edilir:

| Özellik | Seçenekler | Açıklama |
| --- | --- | --- |
| deviceId |gerekli, güncelleştirmelerinin salt okunur |Büyük küçük harf duyarlı dize (en fazla 128 karakter uzunluğunda) ASCII 7 bit alfasayısal karakterler ve belirli özel karakterler: `- . + % _ # * ? ! ( ) , = @ $ '`. |
| generationId |gerekli, salt okunur |Bir IOT hub tarafından üretilen, büyük küçük harf duyarlı dize en çok 128 karakter uzunluğunda. Bu değer ile aynı cihazları ayırt etmek için kullanılır **DeviceID**, silinmiş ve yeniden oluşturulacak. |
| ETag |gerekli, salt okunur |Göre zayıf bir ETag cihaz kimliğini temsil eden bir dize [RFC7232][lnk-rfc7232]. |
| kimlik doğrulama |isteğe bağlı |Kimlik doğrulama bilgileri ve güvenlik malzemeleri içeren bileşik bir nesne. |
| auth.symkey |isteğe bağlı |Base64 biçiminde depolanan birincil ve ikincil bir anahtar içeren bileşik bir nesne. |
| durum |Gerekli |Bir erişim göstergesidir. Olabilir **etkin** veya **devre dışı**. Varsa **etkin**, cihaz bağlanmasına izin verilir. Varsa **devre dışı**, bu cihazı herhangi bir aygıt'e yönelik uç nokta erişemiyor. |
| statusReason |isteğe bağlı |Cihaz kimliği durum nedeni depolayan bir 128 karakter uzunluğundaki dize. Tüm UTF-8 karakterlere izin verilir. |
| statusUpdateTime |Salt okunur |Son durum güncelleştirmesi saat ve tarihi gösteren bir zamana bağlı göstergesi. |
| connectionState |Salt okunur |Bağlantı durumunu gösteren bir alan: ya da **bağlı** veya **bağlantı kesildi**. Bu alan cihaz bağlantı durumunun IOT Hub görünümünü temsil eder. **Önemli**: Bu alan yalnızca geliştirme/hata ayıklama amacıyla kullanılmalıdır. Bağlantı durumu yalnızca MQTT veya AMQP kullanarak aygıtlar için güncelleştirilmiştir. Ayrıca, protokol düzeyi ping (ping MQTT veya AMQP ping) dayalıdır ve yalnızca 5 dakika cinsinden bir maksimum gecikme olabilir. Bu nedenlerle, olabilir hatalı pozitif sonuç gibi cihazlar bağlı olarak bildirilen ancak, kesilir. |
| connectionStateUpdatedTime |Salt okunur |Son saat ve tarihi bağlantı durumunu gösteren bir zamana bağlı göstergesi güncelleştirildi. |
| lastActivityTime |Salt okunur |Zamana bağlı bir gösterge son saat ve tarihi cihazı gösteren, bağlı, alınan veya gönderilen bir ileti. |

> [!NOTE]
> Bağlantı durumu yalnızca bağlantı durumunun IOT Hub görünümünü temsil edebilir. Ağ koşulları ve yapılandırmaları bağlı olarak bu durum güncelleştirmeleri gecikebilir.

## <a name="additional-reference-material"></a>Ek başvuru bilgileri

IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı çalışma zamanı ve yönetim işlemleri için kullanıma sunan çeşitli uç noktaları açıklar.
* [Azaltma ve kotaları] [ lnk-quotas] kotaları açıklar ve IOT hub'ı hizmete uygulamak davranışları azaltma.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT Hub ile etkileşim hem cihaz hem de hizmet uygulamaları geliştirirken kullanabilir SDK'ları listeler.
* [IOT hub'ı sorgu dili] [ lnk-query] IOT Hub'ından, cihaz çiftlerini ve işleri hakkında bilgi almak için kullanabileceğiniz sorgu dili açıklar.
* [IOT Hub MQTT Destek] [ lnk-devguide-mqtt] IOT hub'ı desteği hakkında daha fazla bilgi için MQTT Protokolü sağlar.

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub kimlik kayıt defteri kullanmayı öğrendiniz, aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilgilenen olabilir:

* [IOT Hub'ına erişimi denetleme][lnk-devguide-security]
* [Cihaz çiftlerini durumu ve yapılandırmaları eşitlemek için kullanın][lnk-devguide-device-twins]
* [Bir cihazda doğrudan bir yöntem çağırma][lnk-devguide-directmethods]
* [Birden çok aygıta işleri zamanla][lnk-devguide-jobs]

Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğretici bakın:

* [Azure IOT Hub ile çalışmaya başlama][lnk-getstarted-tutorial]

IOT Hub cihaz sağlama hizmeti kullanarak zero touch, yalnızca zaman sağlama etkinleştirmek için bkz: keşfetmek için: 

* [Azure IOT Hub cihaz hizmet sağlama][lnk-dps]


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

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
