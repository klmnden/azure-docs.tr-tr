---
title: IOT Hub cihazı sağlama hizmeti - otomatik sağlama kavramları
description: Bu makalede, cihaz otomatik IOT cihaz sağlama hizmeti ve IOT Hub istemci SDK'ları kullanarak sağlama, aşamaları kavramsal bir genel bakış sağlar.
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 0df4eb664accd828c47d834fb0014d0d60f57458
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60746021"
---
# <a name="auto-provisioning-concepts"></a>Otomatik sağlama kavramları

Bölümünde anlatıldığı gibi [genel bakış](about-iot-dps.md), cihaz sağlama hizmeti, kullanıcı müdahalesine gerek kalmadan tam zamanında bir IOT hub cihaz sağlama sağlayan bir yardımcı hizmettir. Başarılı sağlama işleminden sonra atanan IOT Hub ile doğrudan kendi cihazları bağlayın. Bu işlem otomatik sağlama olarak adlandırılır ve cihazlar için bir giden kutusunda kayıt ve ilk yapılandırma deneyimi sunar.

## <a name="overview"></a>Genel Bakış

Azure IOT otomatik sağlama üç aşamaya bölünebilir:

1. **Hizmet yapılandırması** -bunları oluşturma ve bunlar arasında bağlantı oluşturma Azure IOT Hub ve IOT Hub cihazı sağlama hizmeti örneklerinin, tek seferlik yapılandırma.

   > [!NOTE]
   > IOT çözümünüzü boyutundan bağımsız olarak milyonlarca cihaza, desteklemeyi planladığınız bile bu geçerlidir bir **tek seferlik yapılandırma**.

2. **Cihaz kaydı** -cihaz sağlama hizmeti örneğini gelecekte kaydolmayı deneyecek cihazların uyumlu hale getirme işlemidir. [Kayıt](concepts-service.md#enrollment) tek bir cihaz "bireysel kayıt" ya da bir "Grup Kaydı" birden çok cihaza yönelik olarak sağlama hizmetine cihaz kimlik bilgilerini yapılandırılarak gerçekleştirilir. Kimlik bilgisi dayanır [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) cihazı kullanmak için cihazın kimlik doğrulaması için kayıt sırasında gerçekleştiğini doğrulamak sağlama hizmeti sağlayan tasarlanmıştır:

   - **TPM**: "bireysel kayıt" yapılandırılan, cihaz kimliğini TPM kayıt kimliği ve ortak onay anahtarını dayanır. TPM olması koşuluyla, bir [belirtimi](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/), hizmet yalnızca TPM uygulaması (donanım veya yazılım) bağımsız olarak belirtimi başına gerçekleştiğini doğrulamak bekliyor. Bkz: [cihaz sağlama: Kimlik kanıtlama ile TPM](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/) TPM tabanlı kanıtlama hakkında ayrıntılı bilgi için. 

   - **X509**: "bireysel kayıt" veya "Grup Kaydı" yapılandırılan, cihaz kimliği kayıt .pem veya .cer dosyası olarak yüklenen bir X.509 dijital sertifika temel alır.

   > [!IMPORTANT]  
   > Cihaz sağlama hizmetlerini kullanmak için bir önkoşul olmamasına rağmen, cihazın bir donanım güvenlik modülü (anahtarlar ve X.509 sertifikaları gibi hassas cihaz kimlik bilgilerini depolamak için HSM) kullanmanız önerilir.

3. **Cihaz kaydı ve yapılandırması** - bir cihaz sağlama hizmeti istemci SDK'sı cihaz ve kanıtlama mekanizmasına uygun kullanarak yerleşik kayıt yazılımı tarafından başlatılan önyükleme bağlı. Yazılım, IOT hub'ı sonraki kayıt ve cihaz sağlama hizmeti için kimlik doğrulaması için bir bağlantı kurar. Başarılı kayıt sırasında cihaz ilk yapılandırmasına ve telemetri işlemine başlamak veren, IOT hub'ı benzersiz cihaz kimliği ve bağlantı bilgilerini kullanarak, sağlanır. Üretim ortamlarında, bu aşama, haftalar veya aylar önceki iki aşamadan sonra ortaya çıkabilir.

## <a name="roles-and-operations"></a>Roller ve işlemler

Önceki bölümde açıklanan aşamaları haftalar veya aylar, kargo, Gümrük işlem vb. üretim realities nedeniyle üretim süresinin gibi yayılabilir. Ayrıca, bunlar arasında birden çok rol dahil çeşitli varlıklar verilen etkinlikleri yayılabilir. Bu bölümde, çeşitli roller ve her aşaması için ilgili işlemleri daha derin bakar ve ardından akış sıralı diyagramda gösterilmektedir. 

Otomatik sağlama da gereksinimleri cihaz üreticisi, kanıtlama mekanizması etkinleştirilmesi için belirli yerleştirir. İşlemler üretim da özellikle olduğu otomatik sağlama zaten oluşturulduktan sonra yeni cihazları temin edilir durumlarda otomatik sağlama aşamaları zamanlaması bağımsızdır meydana gelebilir.

Bir dizi hızlı Başlangıçlar, uygulamalı deneyim otomatik sağlama açıklamanıza yardımcı olmak için sol taraftaki İçindekiler sağlanır. Yazılım learning işlemini kolaylaştırmak/basitleştirmek için fiziksel bir cihaz kaydı ve kayıt için benzetimini yapmak için kullanılır. Bazı hızlı Başlangıçlar, hızlı başlangıçları sanal niteliği nedeniyle, mevcut olmayan roller için işlemleri dahil olmak üzere birden çok rol için işlemler gerçekleştirmek gerektirir.

| Rol | İşlem | Açıklama |
|------| --------- | ------------|
| Üretici | Kimlik ve kayıt URL kodlaması | Kullanılan kanıtlama mekanizmasına dayalı, üretici cihaz kimlik bilgileri ve cihaz sağlama hizmeti kayıt URL'si kodlama için sorumludur.<br><br>**Hızlı Başlangıçlar**: cihaz benzetimi olmadığından, üretici rol yok. Geliştirici rolünü Ayrıntılar için örnek kayıt uygulaması kodlama kullanılan bu bilgileri nasıl alma bakın. |
| | Cihaz kimliği girin | Gönderene cihaz kimlik bilgileri, üreticiye işleci (veya belirtilen bir aracı) iletişim veya doğrudan API'ler aracılığıyla cihaz sağlama Hizmeti'ne kaydetme sorumludur.<br><br>**Hızlı Başlangıçlar**: cihaz benzetimi olmadığından, üretici rol yok. Sanal cihazı Cihaz sağlama hizmeti örneğinizi olarak kaydetmek için kullanılan cihaz kimliğini nasıl alır operatörü rolü Ayrıntılar için bkz. |
| İşleç | Otomatik sağlamayı Yapılandır | Bu işlem otomatik sağlama ilk aşaması ile karşılık gelir.<br><br>**Hızlı Başlangıçlar**: Operatörü rolü, cihaz sağlama hizmeti ve IOT hub'ı örnekleri, Azure aboneliğinizde yapılandırma gerçekleştirin. |
|  | Cihaz kimliği kaydetme | Bu işlem otomatik sağlama ikinci aşaması ile karşılık gelir.<br><br>**Hızlı Başlangıçlar**: Operatörü rolü, cihaz sağlama hizmeti örneğinizi içinde sanal Cihazınızı kaydetme gerçekleştirin. Cihaz kimliği (TPM veya X.509) hızlı başlangıçta sanal kanıtlama yöntemi tarafından belirlenir. Geliştirici rol kanıtlama ayrıntıları için bkz. |
| Cihaz sağlama hizmeti<br>IoT Hub | \<tüm işlemler\> | Fiziksel cihazlar ile hem bir üretim uygulamanız ve benzetilmiş aygıtlardan hızlı Başlangıçlar, bu rolleri aracılığıyla Azure aboneliğinize yapılandırma IOT Hizmetleri karşılandıktan. IOT Hizmetleri, fiziksel ve sanal cihaz sağlama için farklı olarak rol/operations tam olarak aynı şekilde işlev. |
| Geliştirici | Kayıt yazılımı derleme/dağıtma | Bu işlem otomatik sağlama üçüncü aşaması ile karşılık gelir. Geliştirici uygun SDK'sını kullanarak oluşturma ve aygıt kayıt yazılımı dağıtma sorumludur.<br><br>**Hızlı Başlangıçlar**: Örnek kayıt uygulaması oluşturma (yerine fiziksel bir cihaz için dağıttıktan) istasyonunuzda çalışan seçtiğiniz platform/dil gerçek bir cihaz benzetimini yapar. Kayıt uygulaması, tek bir fiziksel cihaza dağıtılan olarak aynı işlemleri gerçekleştirir. "Kimlik kapsamı" cihaz sağlama hizmeti örneğinizi ve kayıt URL'si (sertifika), TPM veya X.509 kanıtlama yöntemi belirtin. Cihaz kimliği, çalışma zamanında, belirttiğiniz yöntemi temel SDK'sı kanıtlama mantığı tarafından belirlenir: <ul><li>**TPM kanıtlama** -geliştirme iş istasyonunuzu çalışır bir [TPM simülatörü uygulama](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator). Çalışmaya sonra ayrı bir uygulama cihaz kimliğini kaydetme TPM'nin "Onay anahtarı" ve "Kayıt kimliği" kullanmak için ayıklamak için kullanılır. SDK kanıtlama mantıksal kayıt sırasında simülatör imzalı bir SAS belirteci kimlik doğrulaması ve kayıt doğrulama sunmak için de kullanır.</li><li>**X509 kanıtlama** -için bir araç kullanmanız [bir sertifika oluşturmasını](how-to-use-sdk-tools.md#x509-certificate-generator). Oluşturduktan sonra kayıt kullanmak için gerekli sertifika dosyası oluşturun. SDK kanıtlama mantıksal kayıt sırasında sertifika kimlik doğrulaması ve kayıt doğrulama için sunmak için de kullanır.</li></ul> |
| Cihaz | Önyükleme ve kaydetme | Bu işlem ile üçüncü aşamada otomatik sağlama, karşılık gelen geliştirici tarafından oluşturulan cihaz kayıt yazılımı tarafından yerine. Geliştirici rolünü Ayrıntılar için bkz. İlk önyükleme sırasında: <ol><li>Uygulama başına genel URL'yi ve hizmeti geliştirme sırasında belirtilen "kimlik kapsamı" cihaz sağlama hizmeti örneğine bağlanır.</li><li>Bağlantı kurulduktan sonra cihaz kanıtlama yöntemi ve kayıt sırasında belirtilen kimlik karşı kimlik doğrulaması yapılır.</li><li>Kimlik doğrulaması yapıldıktan sonra cihaz sağlama hizmeti örneği tarafından belirtilen IOT hub'ı örneği ile kaydedilir.</li><li>Başarılı kayıt sırasında bir benzersiz cihaz kimliği ve IOT Hub uç noktası IOT Hub ile iletişim kurmak için kayıt uygulamaya döndürülür.</li><li> Burada, cihaz ilk çekebilirsiniz [cihaz ikizi](~/articles/iot-hub/iot-hub-devguide-device-twins.md) durum yapılandırmasını ve telemetri verileri raporlama işlemine başlar.</li></ol>**Hızlı Başlangıçlar**: cihaz benzetimi olduğundan, kayıt yazılım geliştirme iş istasyonunuzda çalışır.|

Aşağıdaki diyagramda, roller ve işlemleri sıralama sırasında cihaz otomatik sağlama özetlenmektedir:
<br><br>
[![Bir cihaz için otomatik sağlama dizisi](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png)](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png#lightbox) 

> [!NOTE]
> İsteğe bağlı olarak, üretici "cihaz kimlik kayıt" işlemi cihaz sağlama hizmeti API'lerini kullanarak da gerçekleştirebilirsiniz (yerine işleci aracılığıyla). Bu sıralama ve daha ayrıntılı bir açıklaması için [sıfır dokunma cihaz kaydı ile Azure IOT video](https://youtu.be/cSbDRNg72cU?t=2460) (işaret 41:00 başlayarak)

## <a name="roles-and-azure-accounts"></a>Rolleri ve Azure hesapları

Her rol için bir Azure hesabı nasıl eşlendiğini senaryoya bağlıdır ve söz konusu olabilecek çok sayıda videonuz senaryo vardır. Ortak desenler aşağıdaki rolleri genel olarak eşlenmiş bir Azure hesabına nasıl ile ilgili genel bir yaklaşım sağlamak.

#### <a name="chip-manufacturer-provides-security-services"></a>Yonga üretici güvenlik hizmetleri sağlar.

Bu senaryoda, üretici birinci düzey müşteriler için güvenliği yönetir. Ayrıntılı güvenlik yönetmek zorunda değilsiniz gibi bu senaryo Bu düzey bir müşteriler tarafından tercih edilen. 

Üretici, donanım güvenlik modülleri (HSM'ler) içinde güvenlik sunar. Bu güvenlik anahtarları, sertifikaları, vb. DPS örnekleri ve kayıt grupları Kurulum zaten sahip olası müşterilerden alma üretici içerebilir. Üretici müşterileri için bu güvenlik bilgileri de oluşturabilir.

Bu senaryoda, olabilir iki Azure hesapları dahil:

- **Hesap #1**: Büyük olasılıkla bir dereceye işleci ve geliştirici rolleri arasında paylaşılan. Bu taraf HSM yongaları üreticisinden satın alabilirsiniz. Bu yongaları hesabı 1 ile ilişkili DPS örneğine işaret ettiği. DPS kayıtlar ile cihazları birden çok düzeyi iki müşterilere DPS cihaz kayıt ayarlarını yapılandırarak bu taraf kiralayabilir. Bu taraf için son kullanıcı arka uç sistemleri ile cihaz telemetrisi vb. erişmek için arabirim oluşturmak ayrılan IOT hub'ları da olabilir. Bu ikinci durumda, ikinci bir hesabı gerekli değildir.

- **Hesap #2**: Son kullanıcılar, ikinci düzey müşterilerin kendi IOT hub'larına sahip olabilir. Bu hesap doğru hub'ına cihaz hesabı 1 yalnızca noktalarıyla ilişkilendirilmiş taraf kiralanmış. Bu yapılandırma, Azure Resource Manager şablonları ile yapılabilir Azure hesapları arasında dağıtım noktaları ve IOT hub'ları bağlama gerektirir.

#### <a name="all-in-one-oem"></a>Hepsi bir arada OEM

Üretici, burada yalnızca bir tek üretici hesaba gereken bir "hepsi bir arada OEM" olabilir. Üretici, güvenlik ve uçtan uca sağlama işlemini gerçekleştirir.

Üretici cihazlar satın almış olan müşteriler için bulut tabanlı bir uygulama sağlayabilir. Bu uygulama üretici tarafından ayrılan IOT Hub ile arabirim.

Bu senaryo için örnekleri, satış makineler veya otomatik kahve makineler temsil eder.




## <a name="next-steps"></a>Sonraki adımlar

Kendi tarzınızda karşılık gelen otomatik sağlama hızlı başlangıçları ile çalışırken bir başvuru noktası olarak bu makalede yer işareti koymak yararlı olabilir. 

"Hizmet yapılandırması" aşaması size, yönetim aracı tercihiniz uygun bir "otomatik sağlamayı ayarlama Ayarla" bir hızlı başlangıcı tamamladığınızda başlayın:

- [Azure CLI kullanarak otomatik sağlamayı ayarlama](quick-setup-auto-provision-cli.md)
- [Azure portalını kullanarak otomatik sağlamayı ayarlama](quick-setup-auto-provision.md)
- [Resource Manager şablonu kullanarak otomatik sağlamayı ayarlama](quick-setup-auto-provision-rm.md)

Ardından, cihaz sağlama hizmeti SDK'sı / dil tercihleri ve cihaz kanıtlama mekanizması uyan bir "Otomatik sağlama bir simülasyon cihazı" hızlı başlangıcı ile devam edin. Bu hızlı başlangıçta, "Cihaz kaydı" ve "cihaz kaydı ve yapılandırmasıyla" aşamalarda size yol: 

|  | Sanal cihaz kanıtlama mekanizması | Hızlı Başlangıç SDK/dili |  |
|--|--|--|--|
|  | Güvenilir Platform Modülü (TPM) | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |  |
|  | X.509 sertifikası | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |  |




