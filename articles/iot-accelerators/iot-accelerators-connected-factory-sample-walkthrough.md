---
title: Bağlı Fabrika çözümü adım adım kılavuzu - Azure | Microsoft Docs
description: Azure IoT çözüm hızlandırıcısı Bağlı Fabrikası ve mimarisinin açıklaması.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 950d248d2525f053981c8642ee2d39021b9a0494
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61449929"
---
# <a name="connected-factory-solution-accelerator-walkthrough"></a>Bağlı Fabrika çözüm hızlandırıcısı adım adım kılavuzu

Bağlı Fabrika [çözüm hızlandırıcısı][lnk-preconfigured-solutions], aşağıdakileri sağlayan uçtan uca endüstriyel çözümün bir uygulamasıdır:

* Hem sanal fabrika üretim hatlarında OPC UA sunucuları çalıştıran sanal endüstriyel cihazlara hem de gerçek OPC UA sunucu cihazlarına bağlanır. OPC UA hakkında daha fazla bilgi için bkz. [Bağlı Fabrika SSS](iot-accelerators-faq-cf.md).
* Bu cihazların ve üretim hatlarının işlem KPI ve OEE değerlerini gösterir.
* OPC UA sunucu sistemleri ile etkileşimde bulunmak için bulut tabanlı bir uygulamanın nasıl kullanılabileceğini gösterir.
* Kendi OPC UA sunucu cihazlarınıza bağlanmanızı sağlar.
* OPC UA sunucu verilerine göz atıp verileri değiştirmenizi sağlar.
* OPC UA sunucularınızdan verilerin özelleştirilmiş görünümlerini sağlamak üzere Azure Zaman Serisi Görüşleri (TSI) hizmetiyle tümleştirilir.

Çözümü kendi uygulamanız için bir başlangıç noktası olarak kullanabilir ve özel iş gereksinimlerinizi karşılayacak şekilde [özelleştirebilirsiniz][lnk-customize].

Bu makalede Bağlı Fabrika çözümünün nasıl çalıştığını anlamanız için çözümün temel öğelerinden bazıları açıklanmaktadır. Makale aynı zamanda çözümdeki veri akışını açıklar. Bu bilgiler şunları yapmanıza yardımcı olur:

* Çözümdeki sorunları giderme.
* Çözümü kendinize özel gereksinimleri karşılayacak şekilde nasıl özelleştireceğinizi planlama.
* Azure hizmetlerini kullanan kendi IoT çözümünüzü tasarlama.

Daha fazla bilgi için bkz. [Bağlı Fabrika SSS](iot-accelerators-faq-cf.md).

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagram, çözüm hızlandırıcısının mantıksal bileşenlerinin ana hatlarını vermektedir:

![Bağlı Fabrika mantıksal mimarisi][connected-factory-logical]

## <a name="communication-patterns"></a>Kimlik doğrulaması desenleri

Çözüm, OPC UA telemetri verilerini JSON biçiminde IoT Hub’a göndermek için [OPC UA Pub/Sub belirtimini](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) kullanır. Çözüm bu amaçla [OPC Yayımcısı](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge modülünü kullanır.

Çözüm ayrıca şirket içi OPC UA sunucuları ile bağlantı kurabilen bir web uygulaması ile tümleşik OPC UA istemcisine sahiptir. İstemci bir [ters proxy](https://wikipedia.org/wiki/Reverse_proxy) kullanır ve şirket içi güvenlik duvarında bağlantı noktası açmayı gerektirmeksizin bağlantıyı oluşturmak için IoT Hub’dan yardım alır. Bu iletişim deseni hizmet destekli iletişim çağrılır. Çözüm bu amaçla [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge modülünü kullanır.


## <a name="simulation"></a>Benzetim

Sanal istasyonlar ve sanal üretim yürütme sistemleri (MES) bir fabrikanın üretim hattını oluşturur. Sanal cihazlar ve OPC Yayımcı Modülü, OPC Fundation tarafından yayımlanan [OPC UA .NET Standardı][lnk-OPC-UA-NET-Standard]’nı temel alır.

OPC Proxy ve OPC Yayımcısı [Azure IoT Edge][lnk-Azure-IoT-Gateway]'i temel alan modüller olarak uygulanır. Her sanal üretim hattına bağlı bir ağ geçidi var.

Tüm benzetim bileşenleri bir Azure Linux VM’sinde barındırılan Docker kapsayıcıları içinde çalışır. Benzetim, varsayılan olarak sekiz sanal üretim hattını çalıştırmak üzere yapılandırılmıştır.

## <a name="simulated-production-line"></a>Sanal üretim hattı

Üretim hattı, parça üretir. Farklı istasyonlardan oluşur: derleme istasyonu, test istasyonu ve paketleme istasyonu.

Benzetim çalışır ve OPC UA düğümleri aracılığıyla sunulan verileri güncelleştirir. Tüm sanal üretim hattı istasyonları, OPC UA aracılığıyla MES tarafından düzenlenir.

## <a name="simulated-manufacturing-execution-system"></a>Sanal üretim yürütme sistemi

MES, istasyon durumu değişikliklerini algılamak üzere üretim hattındaki her bir istasyonu OPC UA aracılığıyla izler. OPC UA istasyonları denetlemek üzere yöntemlerini çağırır ve tamamlanana kadar ürünü bir istasyondan diğerine geçirir.

## <a name="gateway-opc-publisher-module"></a>Ağ geçidi OPC yayımcı modülü

OPC Yayımcı Modülü, istasyonun OPC UA sunucularına bağlanır ve yayımlanacak OPC düğümlerine abone olur. Modülü:

1. Düğüm verilerini JSON biçimine dönüştürür.
1. JSON şifreler.
1. JSON, OPC UA pub/sub iletileri IOT hub'a gönderir.

OPC Yayımcı modülü, yalnızca bir giden https bağlantı noktası (443) gerektirir ve mevcut kuruluş altyapısı ile birlikte çalışabilir.

## <a name="gateway-opc-proxy-module"></a>Ağ geçidi OPC proxy modülü

Ağ Geçidi OPC UA Proxy Modülü, ikili OPC UA komutu ile denetim iletileri arasında tünel oluşturur ve yalnızca bir giden https bağlantı noktası (443) gerektirir. Web Proxy’leri dahil olmak üzere, mevcut kuruluş altyapısı ile birlikte çalıştırabilir.

Paketlenmiş TCP/IP verilerini uç nokta güveni, veri şifreleme ve SSL/TLS kullanarak bütünlüğünü sağlamak için uygulama katmanında aktarmak için IOT Hub cihaz yöntemlerini kullanır.

Proxy üzerinden geçirilen OPC UA ikili protokolü, UA kimlik doğrulaması ve şifrelemesi kullanır.

## <a name="azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri

Ağ Geçidi OPC Yayımcı Modülü, veri değerlerindeki değişikliği algılamak üzere OPC UA sunucu düğümlerine abone olur. Düğümlerden birinde veri değişikliği algılanırsa, bu modül Azure IoT Hub’a iletiler gönderir.

IoT Hub, Azure TSI’ye bir olay kaynağı gönderir. TSI, iletilere eklenen zaman damgalarına bağlı olarak verileri 30 gün boyunca saklar. Bu veriler şunlardır:

* OPC UA ApplicationUri
* OPC UA NodeId
* Düğümün değeri
* Kaynak zaman damgası
* OPC UA DisplayName

Şu anda TSI, müşterilerin ne kadar süreyle bunlar verileri saklama süresini özelleştirmesine izin vermez.

Düğüm verilerinin zaman tabanlı kullanarak TSI sorguları **SearchSpan** ve göre toplar **OPC UA ApplicationUri** veya **OPC UA nodeId** veya **OPC UA DisplayName**.

Çözüm için verileri OEE ve KPI ölçeklerine ve zaman serisi grafiklerini almak için olayları sayısına göre verilerini toplayan **toplam**, **ortalama**, **Min**, ve  **En fazla**.

Zaman serisi, farklı bir işlem kullanılarak oluşturulur. Çözüm, temel verilerden istasyon OEE ve KPI değerlerini hesaplar ve üretim hatlarının, Fabrikalar ve kuruluş için ayarlanacak değerleri Balonlar.

Buna ek olarak, OEE ve KPI topolojisi için zaman serisi, görüntülenen bir zaman aralığı hazır olduğunda uygulamada hesaplanır. Örneğin, gün görünümü saatte bir kez güncelleştirilir.

Düğüm verilerinin zaman serisi görünümü, zaman aralığı toplaması kullanılarak doğrudan TSI’den gelir.

## <a name="iot-hub"></a>IoT Hub
[IoT hub’ı][lnk-IoT Hub], OPC Yayımcı Modülünden buluta gönderilen verileri alır ve Azure TSI hizmetinde kullanılabilir hale getirir. 

IoT Hub çözümde aynı zamanda şunları yapar:
- Tüm OPC Yayımcı Modülleri ve tüm OPC Proxy Modülleri için kimlikleri depolayan bir kimlik kayıt defteri tutar.
- OPC Proxy Modülünün iki yönlü iletişimi için taşıma kanalı olarak kullanılır.

## <a name="azure-storage"></a>Azure Storage
Çözüm, VM’nin disk deposu ve dağıtım verilerinin depolanması için Azure blob depolama kullanır.

## <a name="web-app"></a>Web uygulaması
Çözüm Hızlandırıcısını bir parçası olarak dağıtılan web uygulaması, bir tümleşik OPC UA istemcisi, uyarı işleme ve telemetri görselleştirmesinden içerir.

## <a name="telemetry-data-flow"></a>Telemetri veri akışı

![Telemetri veri akışı](./media/iot-accelerators-connected-factory-sample-walkthrough/telemetry_dataflow.png)

### <a name="flow-steps"></a>Akış adımları

1. OPC Publisher, yerel sertifika depolama alanından gerekli OPC UA X509 sertifikalarını ve IoT Hub güvenlik kimlik bilgilerini okur.
    - OPC Publisher gerekirse sertifika depolama alanında eksik olan sertifikaları veya kimlik bilgilerini oluşturur ve depolar.

2. OPC Publisher kendini IoT Hub'a kaydeder.
    - Yapılandırılmış protokolü kullanır. IoT Hub istemci SDK'sı destekli tüm protokolleri kullanabilir. Varsayılan MQTT olarak belirlenmiştir.
    - Protokol iletişiminin güvenliği TLS tarafından sağlanır.

3. OPC Publisher yapılandırma dosyasını okur.

4. OPC Publisher, yapılandırılmış olan her bir OPC UA Server için bir OPC Session oluşturur.
    - TCP bağlantısını kullanır.
    - OPC Publisher ve OPC UA Server, X509 sertifikalarını kullanarak birbirlerinin kimliğini doğrular.
    - Sonraki tüm OPC UA trafiği, yapılandırılmış olan OPC UA şifreleme mekanizması tarafından şifrelenir.
    - OPC Publisher, yapılandırılmış olan her yayımlama aralığı için OPC Session içinde bir OPC Subscription oluşturur.
    - OPC Subscription içinde yayımlamak üzere OPC Nodes için OPC Monitored öğeleri oluşturur.

5. İzlenen bir OPC Node değeri değişirse OPC UA Server güncelleştirmeleri OPC Publisher'a gönderir.

6. OPC Publisher yeni değeri dönüştürür.
    - Toplu işleme etkinse birden fazla değişikliği toplu olarak işler.
    - Bir IoT Hub iletisi oluşturur.

7. OPC Publisher, IoT Hub'a bir ileti gönderir.
    - Yapılandırılmış protokolü kullanır.
    - İletişim güvenliği TLS tarafından sağlanır.

8. Zaman Serisi Öngörüleri (TSI), IoT Hub iletilerini okur.
    - TCP/TLS üzerinden AMQP kullanır.
    - Bu adım veri merkezi içinde gerçekleştirilir.

9. TSI içindeki bekleyen veriler.

10. Azure AppService içindeki Bağlı Fabrika WebApp, gerekli verileri TSI kaynağından sorgular.
    - TCP/TLS güvenli iletişimini kullanır.
    - Bu adım veri merkezi içinde gerçekleştirilir.

11. Web tarayıcısı, Bağlı Fabrika WebApp öğesine bağlanır.
    - Bağlı Fabrika panosunu görüntüler.
    - HTTPS üzerinden bağlanır.
    - Bağlı Fabrika uygulaması erişimi için kullanıcının Azure Active Directory kimlik doğrulamasından geçmesi gerekir.
    - Bağlı Fabrika uygulamasına giden WebApi çağrılarının güvenliği, Sahtekarlığı Önleme Belirteçleri ile sağlanır.

12. Veri güncelleştirmelerinde Bağlı Fabrika WebApp, güncelleştirilmiş verileri web tarayıcısına gönderir.
    - SignalR protokolünü kullanır.
    - Güvenliği TCP/TLS ile sağlanır.

## <a name="browsing-data-flow"></a>Göz atma veri akışı

![Göz atma veri akışı](./media/iot-accelerators-connected-factory-sample-walkthrough/browsing_dataflow.png)

### <a name="flow-steps"></a>Akış adımları

1. OPC Proxy (sunucu bileşeni) başlatılır.
    - Paylaşılan erişim anahtarlarını yerel depolama alanından okur.
    - Gerekirse depolama alanındaki eksik erişim anahtarlarını depolar.

2. OPC Proxy (sunucu bileşeni) kendisini IoT Hub'a kaydeder.
    - Bilinen cihazların tümünü IoT Hub'dan okur.
    - Socket veya WebSocket TLS üzerinden MQTT kullanır.

3. Web tarayıcısı bağlı Fabrika WebApp için bağlanır ve bağlı Fabrika panosunu görüntüler.
    - HTTPS kullanır.
    - Kullanıcı bağlanmak istediği OPC UA sunucusunu seçer.

4. Bağlı Fabrika WebApp, seçilen OPC UA sunucusu ile bir OPC UA Oturumu oluşturur.
    - OPC UA yığınını kullanır.

5. OPC Proxy taşıma işlemi, OPC UA yığınından bir istek alarak OPC UA sunucusuyla bir TCP yuva bağlantısı kurar.
    - Yalnızca TCP yükünü alır ve değiştirmeden kullanır.
    - Bu adım, Bağlı Fabrika WebApp içinde gerçekleştirilir.

6. OPC Proxy (istemci bileşeni), IoT Hub cihaz kaydında OPC Proxy (sunucu bileşeni) girişini arar. Ardından IoT Hub içinde OPC Proxy (sunucu bileşeni) cihazının bir cihaz yöntemini çağırır.
    - OPC Proxy araması için TCP/TLS üzerinden HTTPS kullanır.
    - OPC UA sunucusuyla TCP yuvası bağlantısı için TCP/TLS üzerinden HTTPS kullanır.
    - Bu adım veri merkezi içinde gerçekleştirilir.

7. IoT Hub, OPC Proxy (sunucu bileşeni) cihazında bir cihaz yöntemi çağırır.
    - OPC UA sunucusuyla TCP yuvası bağlantısı kurmak için Socket veya Secure Websocket bağlantısı üzerinden TLS üzerinden MQTT kullanır.

8. OPC Proxy (sunucu bileşeni), TCP yükünü atölye ağına gönderir.

9. OPC UA sunucusu yükü işler ve yanıtı geri gönderir.

10. Yanıt OPC Proxy (sunucu bileşeni) yuvası tarafından alınır.
    - OPC Proxy verileri cihaz yönteminin dönüş değeri olarak IoT Hub ve OPC Proxy (istemci bileşeni) öğelerine gönderir.
    - Bu veriler, Bağlı Fabrika uygulamasındaki OPC UA yığınına gönderilir.

11. Bağlı Fabrika WebApp, OPC Browser UX değerini OPC UA sunucusundan aldığı özgün OPC UA verileriyle zenginleştirmiş bir şekilde oluşturulmak üzere Web Tarayıcısına döndürür.
    - Bir kullanıcı OPC adres alanına göz gider ve OPC adres alanındaki düğümlere işlevleri geçerlidir, ancak OPC Browser UX istemci verileri bağlı Fabrika WebApp ' almak için sahteciliğe karşı koruma belirteçleri ile güvenliği sağlanan HTTPS üzerinden AJAX çağrıları kullanır.
    - İstemci gerekirse OPC UA sunucusuyla bilgi alışverişi yapmak için 4-10 arasındaki iletişim adımlarını kullanır.

> [!NOTE]
> OPC Proxy (sunucu bileşeni) ve OPC Proxy (istemci) bileşeni OPC UA iletişimiyle ilgili tüm TCP trafiği için 4-10 arası adımları gerçekleştirir.

> [!NOTE]
> Bağlı Fabrika WebApp içindeki OPC UA sunucusu ve OPC UA yığını için OPC Proxy iletişimi şeffaftır ve tüm OPC UA kimlik doğrulama ve şifreleme güvenlik özellikleri geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri okuyarak IoT çözümünü kullanmaya başlayabilirsiniz:

* [Azureiotsolutions.com sitesindeki izinler][lnk-permissions]
* [Bağlı Fabrika çözüm hızlandırıcısı için Windows’da ağ geçidi dağıtma](iot-accelerators-connected-factory-gateway-deployment.md)
* [OPC Publisher başvuru uygulaması](https://github.com/Azure/iot-edge-opc-publisher/blob/master/README.md).

[connected-factory-logical]:media/iot-accelerators-connected-factory-sample-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]:about-iot-accelerators.md
[lnk-customize]: iot-accelerators-connected-factory-customize.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-accelerators-faq.md
