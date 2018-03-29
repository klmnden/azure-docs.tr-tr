---
title: IOT Hub cihaz sağlama hizmeti - otomatik sağlama kavramları
description: Bu makale Aygıt otomatik IOT cihaz hizmeti sağlama, IOT Hub ve istemci SDK'ları kullanarak sağlama, aşamaları kavramsal genel bakış sağlar.
services: iot-dps
keywords: ''
author: BryanLa
ms.author: bryanla
ms.date: 03/27/2018
ms.topic: conceptual
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: cd458b1f6d26fbd5f5821a04cd01be5c3a4e4514
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="auto-provisioning-concepts"></a>Otomatik sağlama kavramları

Bölümünde açıklandığı gibi [genel bakış](about-iot-dps.md), aygıt hizmeti sağlama, insan etkileşimi olmadan bir IOT hub'ına aygıtların tam zamanı sağlama sağlayan bir yardımcı hizmetidir. Başarılı sağladıktan sonra cihazları belirlenen IOT Hub ile doğrudan kendi bağlayın. Bu işlem otomatik sağlama olarak adlandırılır ve cihazlar için bir giden kutusu kaydı ve ilk yapılandırma deneyimi sunar.

## <a name="overview"></a>Genel Bakış

Azure IOT otomatik sağlama üç aşamaya ayrılabilir:

1. **Hizmet yapılandırması** -bunları oluşturma ve bunları arasında bağlantı oluşturma Azure IOT hub'ı ve IOT Hub cihaz sağlama hizmet örneği, tek seferlik yapılandırma.

   > [!NOTE]
   > IOT çözümünüzü boyutundan bağımsız olarak milyonlarca cihaza, desteklemeyi planladığınız olsa bile bu olan bir **tek seferlik yapılandırma**.

2. **Cihaz kaydı** -cihaz sağlama hizmet örneği gelecekte kaydolmayı deneyecek cihazların uyumlu hale getirme işlemidir. Kayıt aygıtı kimlik bilgileri sağlama hizmetinde bir "tek tek kayıt" tek bir aygıt için veya bir "grubu"için kayıt birden çok aygıt olarak yapılandırılarak sağlanır. Kimlik temel [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) cihazı kullanmak için kayıt sırasında cihazın Orijinallik şifreli olarak sağlama hizmeti sağlayan tasarlanmıştır:

   - **TPM**: bir "tek tek kaydı" yapılandırılmış, cihaz kimliği TPM kayıt kimliği ve ortak onay anahtarını dayanır. TPM olması koşuluyla, bir [belirtimi]((https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)), hizmet yalnızca TPM uygulamadan (donanım veya yazılım) bağımsız olarak belirtimi başına şifreli olarak bekler. Bkz: [cihaz sağlama: TPM ile kimlik kanıtlama](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/) TPM tabanlı kanıtlama hakkında ayrıntılı bilgi için. 

   - **X509**: bir "tek tek kayıt" veya "grubu kayıt" olarak yapılandırılan, cihaz kimliği kayıt .pem veya .cer dosyası olarak yüklenen bir X.509 dijital sertifika temel alır.

   > [!IMPORTANT]  
   > Cihaz sağlama hizmetleri kullanmak için bir önkoşul değil de, bu cihaz kullanımınız anahtarları ve X.509 sertifikaları gibi hassas aygıt kimlik bilgilerini depolamak için donanım güvenlik modülü (HSM) öneririz.

3. **Cihaz kaydı ve yapılandırması** - cihazı sağlama hizmeti istemci SDK'sı için cihaz ve kanıtlama mekanizması uygun kullanılarak oluşturulan kayıt yazılımı tarafından başlatılan önyükleme bağlı. Yazılım sağlama hizmeti kimlik doğrulama için bir bağlantı aygıtı ve sonraki kayıt IOT hub'ı kurar. Başarılı kaydı sırasında aygıt ilk yapılandırmasına çekmek ve telemetri işlemine başlamak için kendi IOT hub'ı benzersiz cihaz kimliği ve bağlantı bilgilerini ile sağlanır. Üretim ortamlarında, bu aşama, hafta veya ay önceki iki aşamaya sonra ortaya çıkabilir.

## <a name="roles-and-operations"></a>Roller ve işlemler

Önceki bölümde tartışılan aşamaları, hafta veya ay, sevkiyat, vb. Gümrük işlem üretim realities nedeniyle üretim süresi gibi yayılabilir. Buna ek olarak, bunlar arasında birden çok rol ilgili çeşitli varlıkları verilen etkinlikleri yayılabilir. Bu bölümde çeşitli rolleri ve her aşamada ilgili işlemleri daha derin göz alır, ardından bir sıralı diyagramda akışı gösterilmektedir. 

Otomatik sağlama da gereksinimleri kanıtlama mekanizması etkinleştirme için belirli cihaz üreticisi yerleştirir. İşlemleri üretim da özellikle otomatik sağlama zaten oluşturulduktan sonra yeni aygıtları burada temin edilir durumlarda otomatik sağlama aşamaları zamanlama bağımsız ortaya çıkabilir.

Bir dizi Quickstarts, uygulamalı deneyim otomatik sağlama açıklamaya yardımcı olması için sola içindekiler tablosu sağlanır. Yazılım öğrenme işlemini kolaylaştırmak/basitleştirmek için bir fiziksel cihaz kaydı ve kayıt için benzetimini yapmak için kullanılır. Bazı Quickstarts Quickstarts benzetimli yapısı nedeniyle, mevcut olmayan roller için işlemleri dahil olmak üzere birden çok rol için işlemler gerçekleştirmek gerektirir.

| Rol | İşlem | Açıklama | 
|------| --------- | ------------| 
| Üretici | Kimlik ve kayıt URL kodlama | Kullanılan kanıtlama mekanizması bağlı olarak, üretici aygıt kimlik bilgisi ve cihaz sağlama hizmeti kayıt URL'si kodlama için sorumludur.<br><br>**QuickStarts**: cihaz benzetimli olduğundan, herhangi bir üretici rol yoktur. Ayrıntılar için geliştirici rol örnek kayıt uygulama kodlama kullanılan bu bilgileri nasıl almak bakın. |  
| | Cihaz kimliği girin | Gönderenin aygıt kimlik bilgisi üreticisi işleci (veya belirlenen aracı) iletişimde veya doğrudan aygıt hizmeti sağlama API'leri aracılığıyla kaydetme sorumludur.<br><br>**QuickStarts**: cihaz benzetimli olduğundan, herhangi bir üretici rol yoktur. Ayrıntılar için operatörü rolü aygıtı sağlama hizmeti örneğinizi sanal bir cihaz kaydetmek için kullanılan cihaz kimliği nasıl alır bakın. | 
| İşleç | Otomatik sağlamayı Yapılandır | Bu işlem ilk aşaması otomatik sağlama ile karşılık gelir.<br><br>**QuickStarts**: cihaz sağlama hizmeti yapılandırma operatörü rolü gerçekleştirmek ve IOT hub'ı, Azure aboneliğinizde örnekleri. | Kurulum otomatik sağlama |
|  | Cihaz kimliği kaydetme | Bu işlem, ikinci aşaması otomatik sağlama ile karşılık gelir.<br><br>**QuickStarts**: cihaz sağlama hizmeti örneğinizi sanal Cihazınızı kaydetme işleci rolü gerçekleştirilemiyor. Cihaz kimliği (TPM veya X.509) başlangıcın benzetimli kanıtlama yöntemi tarafından belirlenir. Geliştirici rolü kanıtlama ayrıntıları için bkz. | 
| Cihaz sağlama hizmeti<br>IoT Hub’ı | \<tüm işlemleri\> | Hem bir üretim uygulaması fiziksel aygıtlardan ve sanal cihazlar ile Quickstarts için bu rolleri Azure aboneliğinizde yapılandırma IOT Hizmetleri aracılığıyla karşılandıktan. IOT Hizmetleri fiziksel ve sanal cihazlar sağlama için ilgisiz gibi rolleri/operations tam olarak aynı işlev. | 
| Geliştirici | Derleme/kayıt yazılım dağıtma | Bu işlem otomatik sağlama üçüncü aşaması ile karşılık gelir. Geliştirici uygun SDK'sını kullanarak oluşturma ve aygıta, kayıt yazılım dağıtma sorumludur.<br><br>**QuickStarts**: oluşturacağınız örnek kayıt uygulama platformu/dili (yerine bir fiziksel cihaza dağıtmaya) istasyonunuzda çalışır tercih için gerçek bir cihaza benzetim. Kayıt uygulama tek bir fiziksel cihaza dağıtılan olarak aynı işlemleri gerçekleştirir. Kayıt URL'si ve aygıtı sağlama hizmeti örneğinizi "kimliği kapsamı" kanıtlama yöntemi (TPM veya X.509 sertifikası) belirtin. Cihaz kimliği, belirttiğiniz yöntemine dayalı zamanında SDK kanıtlama mantığı tarafından belirlenir: <ul><li>**TPM kanıtlama** -geliştirme iş istasyonunuzu çalıştıran bir [TPM simulator uygulama](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator). Çalışan sonra ayrı bir uygulama TPM'nin "Onay anahtarını" ve "Kayıt kimliği" kullanmak için aygıt kimlik kaydetme ayıklamak için kullanılır. SDK kanıtlama mantığı kayıt sırasında simulator imzalı bir SAS belirteci için kimlik doğrulama ve kayıt doğrulama sunmak için de kullanır.</li><li>**X509 kanıtlama** -bir araç kullanmanız [bir sertifika oluşturma](how-to-use-sdk-tools.md#x509-certificate-generator). Oluşturduktan sonra kayıt kullanmak için gerekli olan sertifika dosyası oluşturun. SDK kanıtlama mantığı kayıt sırasında sertifika kimlik doğrulaması ve kayıt doğrulama için sunmak için de kullanır.</li></ul> | 
| Cihaz | Önyükleme ve kaydetme | Bu işlem otomatik sağlama üçüncü aşaması ile karşılık gelen geliştirici tarafından oluşturulan cihaz kayıt yazılım yerine. Geliştirici rolü Ayrıntılar için bkz. İlk önyükleme sırasında: <ol><li>Uygulama genel URL ve hizmeti "Kimliği geliştirme sırasında belirtilen Scope" başına cihaz sağlama hizmet örneği ile bağlanır.</li><li>Bağlantı kurulduktan sonra cihaz kaydı sırasında belirtilen kimliğinin ve kanıtlama yöntemi karşı doğrulanır.</li><li>Kimliği doğrulanmış sonra cihaz sağlama hizmet örneği tarafından belirtilen IOT Hub örneği ile kaydedilir.</li><li>Başarılı kaydı sırasında bir benzersiz cihaz kimliği ve IOT Hub uç noktası IOT Hub ile iletişim kurmak için kayıt uygulamaya döndürülür.</li><li> Buradan, cihaz ilk çekebilir [cihaz çifti](~/articles/iot-hub/iot-hub-devguide-device-twins.md) durum için yapılandırma ve telemetri verileri raporlama işlemi başlar.</li></ol>**QuickStarts**: cihaz benzetimli olduğundan, kayıt yazılım geliştirme iş istasyonunda çalışır.| 

Aşağıdaki diyagramda rolleri ve işlemleri sıralama aygıt otomatik sağlama sırasında özetlenmektedir:
<br><br>
![Bir cihaz için otomatik sağlama dizisi](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png) 

> [!NOTE]
> İsteğe bağlı olarak, üretici cihaz sağlama hizmet API'leri kullanılarak "cihaz kimliği kaydetme" işlemi de gerçekleştirebilirsiniz (yerine işleci yoluyla). Bu sıralama ve daha ayrıntılı bilgi için bkz: [sıfır dokunma cihaz kaydı ile Azure IOT video](https://myignite.microsoft.com/sessions/55087) (işaret 41:00 başlayarak)

## <a name="next-steps"></a>Sonraki adımlar

Karşılık gelen otomatik sağlama Quickstarts tartışılmadığından çalışırken bu makalede bir başvuru noktası olarak yer işareti yararlı olabilir. 

"Hizmet yapılandırması" aşamada anlatılmaktadır, yönetim aracı tercihlerinizi uygun bir "yukarı otomatik olarak sağlama ayarlama" bir hızlı başlangıç tamamlayarak başlayın:

- [Azure CLI kullanarak otomatik sağlama ayarlama](quick-setup-auto-provision-cli.md)
- [Azure Portalı'nı kullanarak otomatik sağlama ayarlama](quick-setup-auto-provision.md)
- [Resource Manager şablonu kullanarak otomatik sağlama ayarlama](quick-setup-auto-provision-rm.md)

Ardından aygıt kanıtlama mekanizması ve aygıt hizmeti sağlama SDK/dil tercihi uygun bir "Otomatik sağlama sanal bir cihaz" hızlı başlangıcı ile devam edin. Bu Hızlı Başlangıç "Cihaz kaydı" ve "cihaz kaydı ve yapılandırması" aşamaları yol: 

|  | Sanal cihaz kanıtlama mekanizması | Hızlı Başlangıç SDK/dili |  |
|--|--|--|--|
|  | Güvenilir Platform Modülü (TPM) | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |  |
|  | X.509 sertifikası | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |  |




