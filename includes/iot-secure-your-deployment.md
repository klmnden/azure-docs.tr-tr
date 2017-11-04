# <a name="secure-your-iot-deployment"></a>IoT dağıtımınızın güvenliğini sağlama
Bu makalede, Azure IOT tabanlı nesnelerin interneti (IOT) altyapısı güvenliğini sağlamak için sonraki ayrıntı düzeyi sağlar. Yapılandırma ve dağıtma her bileşen için uygulama düzeyi ayrıntılarını bağlar. Karşılaştırmaları ve çeşitli rakip yöntemleri arasında seçenekleri de sağlar.

Azure IOT dağıtım güvenli hale getirme, aşağıdaki üç güvenlik alanlara ayrılabilir:

* **Cihaz güvenliği**: joker dağıtılırken IOT cihaz güvenli hale getirme.
* **Bağlantı güvenliği**: gizli ve yetkisiz değiştirmeye karşı kanıt IOT hub'ı ve IOT cihaz arasında aktarılan tüm verileri sağlama.
* **Bulut güvenlik**: aracılığıyla taşır ve bulutta depolanan verilerin güvenliğini sağlamak için bir yol sağlar.

![Üç güvenlik alanları][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Güvenli aygıt sağlama ve kimlik doğrulama
Azure IOT paketi, aşağıdaki iki yöntemle IOT cihazları güvenliğini sağlar:

* IOT Hub ile iletişim kurmak için aygıt tarafından kullanılan her aygıt için benzersiz kimlik anahtarı (güvenlik belirteçleri) sağlayarak.
* Bir aygıt kullanarak [X.509 sertifikası] [ lnk-x509] ve cihazın IOT hub'ına kimlik doğrulaması için bir yol olarak özel anahtar. Bu kimlik doğrulama yöntemi, aygıtta özel anahtar cihaz dışında herhangi bir zamanda, daha yüksek düzeyde güvenlik sağlayan bilinmediğini sağlar.

Güvenlik belirteci yöntemi her çağrı için simetrik anahtar ilişkilendirerek IOT Hub'ına cihaz tarafından yapılan her çağrı için kimlik doğrulaması sağlar. X.509 tabanlı kimlik doğrulama IOT cihaz TLS bağlantı kurulurken bir parçası olarak fiziksel katmanında olanak sağlar. Güvenlik belirteci tabanlı yöntemi daha az güvenli bir desen olan X.509 kimlik doğrulaması olmadan kullanılabilir. İki yöntem arasında seçim öncelikle nasıl güvenli olması için cihaz kimlik doğrulama gereksinimlerini ve güvenli depolama (özel anahtarı güvenli bir şekilde depolamak için) cihazın kullanılabilirliğini tarafından dikte edilir.

## <a name="iot-hub-security-tokens"></a>IoT Hub güvenlik belirteçleri
IOT Hub, aygıtları ve ağ üzerinde anahtarları göndermekten kaçınmanız Hizmetleri kimlik doğrulaması için güvenlik belirteçlerini kullanır. Ayrıca, güvenlik belirteçlerini geçerlilik tarihi ve kapsam sınırlıdır. Azure IOT SDK'ları otomatik olarak özel bir yapılandırma gerektirmeden belirteçleri oluşturur. Bazı senaryolar ancak oluşturmak ve güvenlik belirteçlerini doğrudan kullanmak kullanıcının gerektirir. Bunlar, MQTT, AMQP veya HTTP yüzeyleri doğrudan kullanımını ya da belirteci hizmeti düzeni uyarlamasını içerir.

Güvenlik belirteci ve kullanım yapısı hakkında daha ayrıntılı bilgi aşağıdaki makalelerde bulunabilir:

* [Güvenlik belirteci yapısı][lnk-security-tokens]
* [SAS belirteci bir aygıt olarak kullanma][lnk-sas-tokens]

Her IOT hub'ı olan bir [kimlik kayıt defteri] [ lnk-identity-registry] kullanılabilecek yürütülen bulut-cihaz iletilerini içeren bir kuyruk gibi hizmet aygıt başına kaynakları oluşturmak ve erişmesine izin vermek için Aygıt'e yönelik uç noktalar. IOT Hub kimlik kayıt cihaz kimliklerini ve güvenlik anahtarları bir çözüm için güvenli depolama sağlar. Tek tek veya grup cihaz kimlikleri bir izin verilenler listesi veya Engellenenler listesi, cihaz erişimi üzerinde tam denetimi etkinleştirme eklenebilir. Aşağıdaki makaleler yapısını kimlik kayıt defteri ve desteklenen işlemler hakkında daha fazla ayrıntı sağlar.

[IOT hub'ı destekleyen MQTT, AMQP ve HTTP gibi protokoller][lnk-protocols]. Bu protokollerin her kullanın IOT Hub'ına IOT cihaz gelen güvenlik belirteçleri farklı:

* AMQP: SASL DÜZ ve AMQP talep tabanlı güvenlik ({policyName}@sas.root. { iothubName} IOT hub düzeyindeki belirteçleri; söz konusu olduğunda {DeviceID} belirteçleri aygıt kapsamlı durumunda).
* MQTT: {ClientID}, {DeviceID} paket kullanır BAĞLANMAK {IoThubhostname} / {DeviceID} içinde **kullanıcıadı** alanı ve SAS belirteci içinde **parola** alan.
* HTTP: Geçerli, yetkilendirme istek üstbilgisinde belirtecidir.

IOT Hub kimlik kayıt defteri cihaz başına güvenlik kimlik bilgilerini yapılandırmak ve erişim denetimi için kullanılabilir. Ancak, bir IOT Çözüm zaten önemli yatırımınız varsa bir [özel cihaz kimlik kayıt defteri ve/veya kimlik doğrulama düzeni][lnk-custom-auth], IOT Hub ile varolan altyapısıyla tümleştirilebilir bir belirteci hizmeti oluşturarak.

### <a name="x509-certificate-based-device-authentication"></a>X.509 cihaz sertifika tabanlı kimlik doğrulaması
Kullanımını bir [aygıt tabanlı X.509 sertifikası] [ lnk-use-x509] ve ilişkili özel ve ortak anahtar çiftini fiziksel katmanında ek kimlik doğrulaması sağlar. Özel anahtarı aygıtı güvenli bir şekilde depolanır ve cihaz dışında bulunabilirlik değil. X.509 sertifikası cihaz kimliği gibi cihaz ve diğer kuruluş ayrıntıları hakkında bilgi içerir. İmza sertifikasının özel anahtarı kullanılarak oluşturulur.

Üst düzey cihaz sağlama akışı:

* Fiziksel bir aygıtı – cihaz kimliği ve/veya üretim veya commissioning aygıt sırasında cihaza ilişkili X.509 sertifikası için bir tanımlayıcı ilişkilendirin.
* IOT hub – cihaz kimliği ve ilişkili aygıt bilgileri IOT Hub kimlik kayıt defterinde karşılık gelen bir kimlik giriş oluşturun.
* X.509 sertifika parmak izi IOT Hub kimlik kayıt defterinde kaybedilebilir.

### <a name="root-certificate-on-device"></a>Cihaz üzerinde kök sertifikası
IOT Hub ile güvenli TLS bağlantı kurulurken IOT cihaz IOT Hub'ın cihaz SDK'sı parçası olan bir kök sertifikayı kullanarak doğrular. C istemci SDK'sı için sertifika klasörünün altında bulunan "\\c\\sertifikaları" depo kökündeki altında. Bu kök sertifikaları uzun süreli olmakla birlikte, bunlar hala dolabilir veya iptal edilebilir. Aygıtlardaki sertifikanın güncelleştirme hiçbir şekilde varsa, cihaz IOT hub'ı (veya diğer herhangi bir bulut hizmeti) sonradan bağlanabilmek için olmayabilir. IOT cihaz dağıtıldıktan sonra bir kök sertifikayı güncelleştirmek için bir yol olması etkili bir şekilde Bu riskin azaltılmasına.

## <a name="securing-the-connection"></a>Bağlantının güvenliğini sağlama
Internet bağlantısı IOT hub'ı ve IOT cihaz arasında Aktarım Katmanı Güvenliği (TLS) standardını kullanarak güvenlik altına alınır. Azure IOT destekler [TLS 1.2][lnk-tls12], TLS 1.1 ve TLS 1.0, bu sırada. TLS 1.0 desteği yalnızca geriye dönük uyumluluk için sağlanır. En yüksek güvenliği sağlar beri TLS 1.2 kullanılması önerilir.

Azure IOT paketi aşağıdaki şifre paketleri, bu sırada destekler.

| Şifre paketi | uzunluğu |
| --- | --- |
| TLS\_ECDHE\_RSA\_ile\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. 7680 bit RSA) FS |256 |
| TLS\_ECDHE\_RSA\_ile\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. 3072 bit RSA) FS |128 |
| TLS\_ECDHE\_RSA\_ile\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. 7680 bit RSA) FS |256 |
| TLS\_ECDHE\_RSA\_ile\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. 3072 bit RSA) FS |128 |
| TLS\_RSA\_ile\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_ile\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_ile\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_ile\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_ile\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_ile\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_ile\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="securing-the-cloud"></a>Bulut güvenliğini sağlama
Azure IOT Hub verir tanımını [erişim denetimi ilkeleri] [ lnk-protocols] her güvenlik anahtarı için. Her IOT Hub'ın uç noktalar için erişim vermek için aşağıdaki izinler kümesini kullanır. İzinleri işlevselliğine dayalı bir IOT Hub'ına erişimi sınırlayın.

* **RegistryRead**. Okuma kimlik kayıt defterine erişim verir. Daha fazla bilgi için bkz: [kimlik kayıt defteri][lnk-identity-registry].
* **RegistryReadWrite**. Verir okuma ve yazma erişimi kimlik kayıt defterine. Daha fazla bilgi için bkz: [kimlik kayıt defteri][lnk-identity-registry].
* **ServiceConnect**. Hizmet dönük iletişim ve uç nokta izleme buluta verir erişin. Örneğin, cihaz bulut iletilerini, bulut-cihaz iletilerini göndermek ve karşılık gelen teslim alındı bildirimleri almak için arka uç bulut hizmetlerine izin verir.
* **DeviceConnect**. Aygıt'te Uç noktalara erişimi verir. Örneğin, cihaz bulut iletilerini göndermek ve bulut-cihaz iletilerini izni verir. Bu izin, cihazlar tarafından kullanılır.

Almak için iki yolla **DeviceConnect** IOT Hub ile izinlerle [güvenlik belirteçleri][lnk-sas-tokens]: cihaz kimlik anahtar veya bir paylaşılan erişim anahtarı kullanarak. Ayrıca, tüm işlevlere aygıtlardan erişilebilir önekiyle uç noktalarda tasarıma göre sunulur dikkat edilecek önemli `/devices/{deviceId}`.

[Hizmet bileşenleri yalnızca güvenlik belirteçleri oluşturmak] [ lnk-service-tokens] paylaşılan erişim ilkeleri uygun izinleri verme kullanma.

Azure IOT Hub ve çözümün parçası olabilecek diğer hizmetler Azure Active Directory kullanarak kullanıcı yönetimi izin verir.

Azure IOT Hub tarafından alınan verilerin çeşitli Azure akış analizi ve Azure blob depolama gibi hizmetler tarafından kullanılabilecek. Bu hizmetler yönetim erişimi sağlar. Bu hizmetleri ve aşağıdaki kullanılabilir seçenekler hakkında daha fazlasını okuyun:

* [Azure Cosmos DB][lnk-cosmosdb]: cihazlar için meta verileri yönetir yarı yapılandırılmış veriler için ölçeklenebilir, tam olarak dizine veritabanı hizmeti, öznitelikler, yapılandırma ve güvenlik özellikleri gibi sağlamanız. Azure Cosmos DB yüksek performanslı ve yüksek verimlilik işleme, veri ve zengin bir SQL sorgu arabirimi şema belirsiz dizin sunar.
* [Azure Stream Analytics][lnk-asa]: Gerçek Zamanlı Akış hızlı bir şekilde geliştirmek ve cihazlar, algılayıcılar, altyapı, gerçek zamanlı Öngörüler ortaya çıkarmak için düşük maliyetli analiz çözümü dağıtmak sağlayan bulutta işleme ve uygulamaları. Bu tam olarak yönetilen hizmet verilerden herhangi bir birime yüksek performans, düşük gecikme süresi ve dayanıklılık hala elde ederken ölçeklendirebilirsiniz.
* [Azure uygulama hizmetleri][lnk-appservices]: güçlü web ve bağlanmak için herhangi bir yerde veri; bulutta veya şirket içi mobil uygulamaları oluşturmak için bir bulut platformu. iOS, Android ve Windows için ilgi çekici mobil uygulamalar oluşturun. Hizmet (SaaS) ve bulut tabanlı Hizmetleri düzinelerce Giden kutusu bağlantı Kurumsal uygulamaları ve kuruluş uygulamaları olarak, yazılım ile tümleştirin. Sık kullanılan dil ve IDE (.NET, Node.js, PHP, Python veya Java) web uygulamaları ve API'leri her zamankinden daha hızlı oluşturmak için kod.
* [Logic Apps][lnk-logicapps]: Azure App Service Logic Apps özelliğini IOT çözümünüzü mevcut iş kolu satır sistemlerinize tümleştirmek ve iş akışı işlemlerini otomatikleştirmenize yardımcı olur. Logic Apps, geliştiricilerin; bir tetikleyiciyle başlayan ve bir dizi adımı yürütürken iş akışları tasarlamasına sağlar — kurallar ve Eylemler, iş süreçlerini tümleştirmek için güçlü bağlayıcıları kullanın. Logic Apps SaaS, bulut tabanlı büyük ekosistemi Giden kutusu bağlantısını sunar ve şirket içi uygulamalar.
* [Azure blob depolama][lnk-blob]: aygıtlarınızı buluta Gönder verileri için güvenilir ve ekonomik bulut depolama.

## <a name="conclusion"></a>Sonuç
Bu makalede tasarlama ve Azure IOT kullanarak bir IOT altyapı dağıtma için düzeyi Ayrıntılar uygulama genel bakış sağlar. Güvenli olacak şekilde her bileşen yapılandırma genel IOT altyapısını koruma içinde anahtardır. Azure IOT ve tasarım tercihlerinin esneklik ve tercih bazı düzeyi sağlamak; Bununla birlikte, her seçenek güvenlik etkileri olabilir. Bu seçenek her risk/maliyeti değerlendirmesi değerlendirilmesi önerilir.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components
[lnk-cosmosdb]: https://azure.microsoft.com/services/cosmos-db/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
