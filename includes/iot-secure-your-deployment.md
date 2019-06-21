---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: e5acb8e0f8805da7f14bbce58b4bfd2acdc24f23
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67189027"
---
# <a name="secure-your-internet-of-things-iot-deployment"></a>Nesnelerin interneti (IOT) dağıtımınızın güvenliğini sağlama

Bu makalede, Azure IOT tabanlı nesnelerin interneti (IOT) altyapısının güvenliğini sağlamak için sonraki ayrıntı düzeyi sağlar. Yapılandırma ve her bir bileşeni dağıtmak için uygulama düzeyi ayrıntıları bağlar. Koleksiyonlardaki karşılaştırmalar ve tutarlılık arasında çeşitli rakip yöntemler de sağlar.

Azure IOT dağıtımını güvenli hale getirme, aşağıdaki üç güvenlik alanlara ayrılabilir:

* **Cihaz güvenliği**: Gerçek hayattaki dağıtılırken IOT cihazı güvenli hale getirme.

* **Bağlantı güvenliği**: IOT Hub ve IOT cihaz arasında aktarılan tüm verileri sağlayarak, gizli ve değiştirilmeye kanıt.

* **Bulut güvenliği**: Aracılığıyla taşır ve bulutta depolanan verilerin güvenliğini sağlamak için bir yol sağlama.

![Üç güvenlik alanları](./media/iot-secure-your-deployment/overview.png)

## <a name="secure-device-provisioning-and-authentication"></a>Cihaz sağlama ve kimlik doğrulaması güvenliğini sağlama

IOT Çözüm Hızlandırıcıları, aşağıdaki iki yöntemi kullanarak IOT cihazları güvenli:

* Cihazın IOT Hub ile iletişim kurmak için kullandığı her cihaz için benzersiz bir kimlik anahtarı (güvenlik belirteçleri) sağlayarak.

* Bir cihazda kullanarak [X.509 sertifikası](https://www.itu.int/rec/T-REC-X.509-201210-S) ve IOT hub'ına cihaz kimlik doğrulaması için bir yol olarak özel anahtarı. Bu kimlik doğrulama yöntemi, özel anahtarı cihazda cihaz dışında herhangi bir zamanda yüksek seviyede güvenliği sağlama bilinmediğini sağlar.

Güvenlik belirteci yönteminde kimlik doğrulaması yapılan simetrik anahtar ilişkilendirerek IOT Hub'ına cihaz tarafından yapılan her çağrı için sağlar. X.509 tabanlı kimlik doğrulama IOT cihazına TLS bağlantı kurulurken bir parçası olarak fiziksel katmanda olanak sağlar. Belirteç tabanlı güvenlik yöntemi, daha az güvenli bir desendir X.509 kimlik doğrulaması kullanılabilir. İki yöntem arasında seçim yapma öncelikle nasıl güvenli olması için cihaz kimlik doğrulama gereksinimlerini ve (özel anahtarı güvenli bir şekilde depolamak için) cihazın güvenli depolama alanı kullanılabilirliğini belirler.

## <a name="iot-hub-security-tokens"></a>IoT Hub güvenlik belirteçleri

IOT Hub, cihaz ve hizmet anahtarları ağda göndermekten kaçınmanız kimliğini doğrulamak için güvenlik belirteçleri kullanır. Ayrıca, güvenlik belirteçleri geçerlilik tarihi ve kapsam bakımından sınırlıdır. Azure IOT SDK'ları otomatik olarak özel bir yapılandırma gerektirmeden belirteçleri oluşturur. Bazı senaryolar, ancak kullanıcı oluşturmak ve güvenlik belirteçlerini doğrudan gerektirir. Bu senaryolar, MQTT, AMQP veya HTTP yüzey doğrudan kullanımını veya belirteç hizmeti düzeninin uygulaması içerir.

Güvenlik belirteci ve bunun kullanımını yapısı hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir:

* [Güvenlik belirteci yapısı](../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure)

* [Bir cihaz olarak SAS belirteçlerini kullanma](../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app)

Her IOT Hub'ına bir [kimlik kayıt defteri](../articles/iot-hub/iot-hub-devguide-identity-registry.md) kullanılabilecek hizmetindeki uçuşan bulut-cihaz iletilerini içeren bir kuyruk gibi cihaz başına kaynakları oluşturmak ve cihaz'e yönelik uç noktalarına erişime izin vermek için. IOT Hub kimlik kayıt defteri, cihaz kimliklerini ve çözüm için güvenlik anahtarlarınızı güvenli depolama sağlar. Kişi veya grup cihaz kimliklerinin bir izin verilenler listesi veya Engellenenler listesi, cihaz erişiminde tam kontrole etkinleştirme eklenebilir. Aşağıdaki makaleler, desteklenen işlemler ve kimlik kayıt defteri yapısını daha fazla ayrıntı sağlar.

[IOT hub'ın desteklediği MQTT, AMQP ve HTTP gibi protokoller](../articles//iot-hub/iot-hub-devguide-security.md). Bu protokollerin her farklı IOT cihaz IOT hub güvenlik belirteçleri kullanır:

* AMQP: SASL DÜZ ve AMQP talep tabanlı güvenlik (`{policyName}@sas.root.{iothubName}` IOT hub'ı düzeyinde belirteçleriyle; `{deviceId}` kapsamlı cihaz belirteçleri ile).

* MQTT: CONNECT paket kullanan `{deviceId}` olarak `{ClientId}`, `{IoThubhostname}/{deviceId}` içinde **kullanıcıadı** alanı ve SAS belirtecini **parola** alan.

* HTTP: Yetkilendirme istek üst bilgisinde geçerli belirtecidir.

IOT Hub kimlik kayıt defteri, cihaz başına güvenlik kimlik bilgilerini yapılandırmak ve erişim denetimi için kullanılabilir. Ancak, bir IOT çözümünü zaten ciddi bir yatırım varsa bir [özel cihaz kimlik kayıt defteri ve/veya kimlik doğrulama düzeni](../articles/iot-hub/iot-hub-devguide-security.md#custom-device-and-module-authentication), bu IOT Hub ile varolan altyapısıyla belirteç hizmeti oluşturarak tümleştirilebilir.

### <a name="x509-certificate-based-device-authentication"></a>X.509 cihaz sertifika tabanlı kimlik doğrulaması

Kullanımı bir [X.509 sertifikası tabanlı cihaz](../articles/iot-hub/iot-hub-devguide-security.md) ve ilişkili özel ve genel anahtar çiftiyle fiziksel katmanda ek kimlik doğrulama sağlar. Özel anahtar, cihazın güvenli bir şekilde depolanır ve cihaz dışında bulunabilir değildir. X.509 sertifikası gibi cihaz kimliği, cihaz ve diğer kuruluş ayrıntıları hakkında bilgi içerir. İmza sertifikasının özel anahtarı kullanılarak oluşturulur.

Üst düzey cihaz sağlama akış:

* Fiziksel cihaz-cihaz kimliği ve/veya üretim veya commissioning cihaz sırasında cihazla ilişkili X.509 sertifikası için bir tanımlayıcı ile ilişkilendirin.

* IOT hub – cihaz kimliği ve IOT Hub kimlik kayıt defterinde ilişkili cihaz bilgilerini karşılık gelen bir kimlik giriş oluşturun.

* Güvenli bir şekilde X.509 sertifikası parmak izi, IOT Hub kimlik kayıt defterinde saklayın.

### <a name="root-certificate-on-device"></a>Cihaz kök sertifikası

IOT Hub ile güvenli bir TLS bağlantısı kurulurken, IOT cihaz IOT Hub cihaz SDK'sını bir parçası olan bir kök sertifikayı kullanarak doğrular. C istemci SDK'sı sertifika klasörü altında bulunan "\\c\\sertifikaları" deponun kök altında. Bu kök sertifikaları uzun süreli olsa da, bunlar yine de sona erebilir veya iptal edilebilir. Cihazda sertifika güncelleştirme yolu varsa, cihaz IOT hub'ı (veya herhangi bir bulut hizmeti) daha sonra bağlanmak mümkün olmayabilir. IOT cihaz etkili bir şekilde dağıtıldıktan sonra bir kök sertifikayı güncelleştirmek için bir yol olan bu riski azaltır.

## <a name="securing-the-connection"></a>Bağlantı güvenliği

IOT Hub ve IOT cihaz arasında Internet bağlantısı, Aktarım Katmanı Güvenliği (TLS) standardı kullanılarak korunmaktadır. Azure IOT destekler [TLS 1.2](https://tools.ietf.org/html/rfc5246), TLS 1.1 ve TLS 1.0, sırasıyla. TLS 1.0 desteği yalnızca geriye dönük uyumluluk için sağlanır. En yüksek güvenliği sağladığından, mümkün olduğunda, TLS 1.2 kullanın.

## <a name="securing-the-cloud"></a>Bulut güvenliğini sağlama

Azure IOT hub'ı tanımını sağlar [erişim denetim ilkeleri](../articles/iot-hub/iot-hub-devguide-security.md) her güvenlik anahtarı. Her IOT Hub'ın uç noktalar için erişim vermek için aşağıdaki izinler kümesini kullanır. İzinleri işlevselliğine bağlı bir IOT hub'a erişimi sınırlayabilir.

* **RegistryRead**. Okuma kimlik kayıt defterine erişim verir. Daha fazla bilgi için [kimlik kayıt defteri](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

* **RegistryReadWrite**. Okuma ve yazma erişimi için kimlik kayıt defteri. Daha fazla bilgi için [kimlik kayıt defteri](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

* **ServiceConnect**. Bulut hizmeti kullanıma yönelik iletişim ve uç noktalarınızı izleyerek erişimi verir. Örneğin, CİHAZDAN buluta iletileri almak, bulut-cihaz iletilerini göndermek ve karşılık gelen teslim alındı bildirimleri almak için arka uç bulut Hizmetleri için izin verir.

* **DeviceConnect**. Cihaz'e yönelik uç noktalarına erişimi verir. Örneğin, CİHAZDAN buluta iletiler gönderir ve bulut-cihaz iletilerini izni verir. Bu izin, cihazlar tarafından kullanılır.

Alınması için iki yol vardır **DeviceConnect** izinleri ile IOT Hub ile [güvenlik belirteçleri](../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app): cihaz kimlik anahtarı veya paylaşılan erişim anahtarı kullanarak. Ayrıca, tüm işlevselliği cihazlardan erişilebilir tasarım ile ön uç üzerinde kullanıma sunulduğunu unutmayın `/devices/{deviceId}`.

[Hizmet bileşenleri yalnızca güvenlik belirteçleri üretebilir](../articles/iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components) kullanılarak paylaşılan erişim ilkeleri uygun izinleri veriliyor.

Azure IOT Hub ve çözümün bir parçası olabilecek diğer hizmetleri Azure Active Directory kullanarak kullanıcı yönetimi sağlar.

Azure IOT Hub tarafından alınan veriler, çeşitli Azure Stream Analytics ve Azure blob depolama gibi hizmetler tarafından tüketilebilir. Bu hizmet, yönetim erişimi sağlar. Bu hizmetler ve kullanılabilir seçenekler hakkında daha fazlasını okuyun:

* [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/): Meta veri öznitelikleri, yapılandırma ve güvenlik özellikleri gibi sağlamak cihazların yöneten yarı yapılandırılmış veriler için ölçeklenebilir, tam olarak dizini oluşturulan bir veritabanı hizmeti. Azure Cosmos DB, yüksek performanslı ve yüksek performanslı işleme, veri ve zengin bir SQL sorgusu arabirimi şemadan dizin sunar.

* [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/): Gerçek Zamanlı Akış sayesinde hızlı bir şekilde geliştirin ve cihazlar, algılayıcılar, altyapı ve uygulamalardan gerçek zamanlı Öngörüler açığa çıkarmak için düşük maliyetli bir analiz çözümü dağıtmak bulutta işleme. Bu tümüyle yönetilen bu hizmet verileri, herhangi bir birim için yine de yüksek aktarım hızı, düşük gecikme süresi ve dayanıklılık elde edin ölçeklendirebilirsiniz.

* [Azure uygulama hizmetleri](https://azure.microsoft.com/services/app-service/): Güçlü web uygulamaları ve her yerden veri bağlanan mobil uygulamaları oluşturmak için bir bulut platformu; bulutta veya şirket içinde. iOS, Android ve Windows için ilgi çekici mobil uygulamalar oluşturun. Yazılım olarak hizmet (SaaS) ve onlarca bulut tabanlı hizmetler için kullanıma hazır bağlantısı ile Kurumsal uygulamalar ve kurumsal uygulamaları ile tümleştirin. En sevdiğiniz dilde ve IDE'de (.NET, Node.js, PHP, Python veya Java) web uygulamaları ve API'leri her zamankinden daha hızlı derleme için kod.

* [Logic Apps](https://azure.microsoft.com/services/app-service/logic/): Azure App Service'in Logic Apps özelliği, IOT çözümünüzün var olan satır iş kolu sistemlerinize tümleştirin ve iş akışı işlemlerini otomatikleştirmek yardımcı olur. Logic Apps, geliştiricilerin bir tetikleyiciyle başlatılan ve bir dizi adım yürüten iş akışları tasarlamasına olanak tanır; kurallar ve İş süreçlerinizi ile tümleştirmek için güçlü bağlayıcıları kullanma eylemler. Logic Apps geniş bir SaaS, bulut tabanlı ekosistemine kullanıma hazır bağlantısı sunar ve şirket içi uygulamaları.

* [Azure Blob Depolama](https://azure.microsoft.com/services/storage/): Cihazlarınızı buluta gönderdiğiniz verileri için güvenilir, ekonomik bulut depolama.

## <a name="conclusion"></a>Sonuç

Bu makalede düzeyi ayrıntıları tasarlama ve Azure IOT kullanarak bir IOT altyapısı dağıtmak için uygulama genel bakış sağlar. Güvenli olacak şekilde her bir bileşenini yapılandırma genel IOT altyapısı güvenli hale getirmenin anahtardır. Azure IOT içinde kullanılabilir ve tasarım tercihlerinin belirli bir düzeyde esneklik ve seçeneğe; sağlayın Ancak, her seçenek güvenlikle ilgili etkileri olabilir. Bu seçenek her bir risk/maliyeti değerlendirmesi hesaplanacak önerilir.
