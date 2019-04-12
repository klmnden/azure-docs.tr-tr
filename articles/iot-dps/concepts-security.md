---
title: Azure IOT Hub cihazı sağlama hizmeti güvenlik kavramları | Microsoft Docs
description: Güvenlik kavramları belirli cihazlara cihaz sağlama hizmeti ve IOT Hub ile sağlama açıklanır
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: e35330874c647eba2cddde694563c8a1d9e83df5
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490281"
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>IOT Hub cihazı sağlama hizmeti güvenlik kavramları 

IOT Hub cihazı sağlama hizmeti, sıfır dokunma cihaz sağlama belirtilen bir IOT hub'ı yapılandırmak için kullandığınız bir IOT Hub için bir yardımcı hizmettir. Cihaz sağlama hizmeti ile yapabilecekleriniz [otomatik sağlama](concepts-auto-provisioning.md) milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde. Bu makalede, genel bir fikir veren *güvenlik* kavramları söz konusu cihaz sağlama. Bu makalede, dağıtım için hazır bir aygıt alma katılan tüm kişiler için uygundur.

## <a name="attestation-mechanism"></a>Kanıtlama mekanizması

Kanıtlama mekanizması bir cihazın kimliğini onaylamak için kullanılan bir yöntemdir. Kanıtlama mekanizması, sağlama hizmeti kanıtlama belirli bir cihaz ile kullanmak için hangi yöntemin belirten bir kayıt listesi için de geçerlidir.

> [!NOTE]
> IOT Hub için benzer bir kavram bu hizmet, "kimlik doğrulama şeması" kullanır.

Cihaz sağlama hizmeti, aşağıdaki biçimlerden kanıtlama destekler:
* **X.509 sertifikaları** temel standart X.509 sertifika kimlik doğrulaması akış.
* **Güvenilir Platform Modülü (TPM)** imzalı bir paylaşılan erişim imzası (SAS) belirteç sunmak için anahtarları için TPM standart kullanarak bir nonce challenge üzerinde temel. Cihazda bir fiziksel TPM'nin bu tür bir kanıtlama gerektirmez, ancak her onay anahtarını kullanarak gerçekleştiğini doğrulamak hizmet bekliyor [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).
* **Simetrik anahtar** paylaşılan erişim imzası (SAS) tabanlı [güvenlik belirteçleri](../iot-hub/iot-hub-devguide-security.md#security-tokens), karma bir imza ve katıştırılmış bir sona erme içerir. Daha fazla bilgi için [simetrik anahtar kanıtı](concepts-symmetric-key-attestation.md).


## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülüne veya HSM, cihaz gizli dizileri güvenli, donanım tabanlı depolama için kullanılır ve en güvenli gizli dizi depolama biçimindedir. X.509 sertifikaları hem de SAS belirteçlerini HSM'de depolanabilir. HSM'ler olabilir hem de kanıtlama mekanizmaları ile kullanılan sağlama destekler.

> [!TIP]
> Güvenli bir şekilde cihazlarınızda gizli dizileri depolamak için cihazlarla bir HSM kullanıldığında öneririz.

Cihaz gizli dizileri, yazılım (bellek) depolanabilir, ancak bu daha az güvenli bir depolama biçiminin bir HSM daha.

## <a name="trusted-platform-module"></a>Güvenilir Platform Modülü

Platform kimliğini doğrulamak için kullanılan anahtarları güvenli bir şekilde depolamak için standart TPM başvurabilir veya standart uygulama modülleri ile etkileşim kurmak için kullanılan g/ç arabirimi başvurabilir. TPM'ler ayrık donanım, tümleşik donanım, üretici yazılımı veya yazılım tabanlı bulunabilir. Daha fazla bilgi edinin [TPM'ler ve TPM kanıtlama](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Cihaz sağlama hizmeti, yalnızca TPM 2.0 destekler.

TPM kanıtlama imzalı bir paylaşılan erişim imzası (SAS) belirteç sunmak için onay ve depolama kök anahtarlarını kullanan bir nonce challenge üzerinde temel alır.

### <a name="endorsement-key"></a>Onay anahtarı

Onay anahtarını dahili olarak oluşturulan veya zaman üretim sırasında eklenen TPM içinde yer alan bir asimetrik anahtardır ve her TPM için benzersizdir. Onay anahtarını değiştirilmesi veya kaldırılması. Orijinal TPM tanımak için onay anahtarını ortak kısmını kullanılırken onay anahtarını özel bölümünü TPM dışında hiçbir zaman serbest bırakılır. Daha fazla bilgi edinin [onay anahtarını](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

### <a name="storage-root-key"></a>Depolama kök anahtarı

Depolama kök anahtarı TPM'de depolanan ve böylece bu anahtarları TPM olmadan kullanılamaz uygulamaları tarafından oluşturulan TPM anahtarlarınızı korumak için kullanılır. TPM sahipliğini depolama kök anahtarı üretilir; böylece yeni bir kullanıcıya sahiplik TPM temizlediğinizde, yeni bir depolama kök anahtarı oluşturulur. Daha fazla bilgi edinin [depolama kök anahtarı](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="x509-certificates"></a>X.509 sertifikaları

Bir kanıtlama mekanizması olarak X.509 sertifikaları kullanarak, üretim ölçeği ve cihaz sağlamayı kolaylaştırmak için mükemmel bir yoldur. X.509 sertifikaları, genellikle güven içinde zincirindeki her sertifika özel anahtarını sonraki daha yüksek sertifika ve benzeri, otomatik olarak imzalanan kök sertifikada sonlandırma tarafından imzalanmış bir sertifika zinciri halinde düzenlenir. Bu düzenleme, bir temsilci kök sertifikadan bir güvenilen kök sertifika yetkilisi (CA) üzerinden her bir ara CA'ya bir cihaza yüklenen son varlık "yaprak" sertifika tarafından oluşturulan güven zinciri oluşturur. Daha fazla bilgi için bkz. [cihaz X.509 CA sertifikalarını kullanarak kimlik doğrulaması](/azure/iot-hub/iot-hub-x509ca-overview). 

Genellikle sertifika zinciri, cihazlar ile ilişkili bazı mantıksal veya fiziksel hiyerarşi temsil eder. Örneğin, bir üreticinin olabilir:
- bir otomatik olarak imzalanan kök CA sertifika
- kök sertifikanın her factory için benzersiz bir ara CA sertifikası oluşturmak için kullanın
- Tesis her bir üretim satırı için benzersiz bir ara CA sertifikasını oluşturmak için her fabrikasının sertifika kullan
- ve son satırında üretilen her cihaz için bir benzersiz cihaz (son varlık) sertifikası oluşturmak için bir üretim sertifika kullanın. 

Daha fazla bilgi için bkz. [X.509 CA sertifikalarını IOT sektördeki kavramsal anlayış](/azure/iot-hub/iot-hub-x509ca-concept). 

### <a name="root-certificate"></a>Kök sertifika

Bir kök sertifika, sertifika yetkilisi (CA) temsil eden otomatik olarak imzalanan bir X.509 sertifikasıdır. Terminus veya sertifika zinciri güven çıpası olduğu. Kök sertifikalar kendi kendine bir kuruluş tarafından verilen veya bir kök sertifika yetkilisinden satın alınmış. Daha fazla bilgi için bkz. [alma X.509 CA sertifikalarını](/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). Kök sertifika da için bir kök CA sertifikası olarak adlandırılır.

### <a name="intermediate-certificate"></a>Ara Sertifika

Bir ara sertifika kök sertifikasını (veya başka bir ara sertifika zincirinde kök sertifikası ile) imzalanmış bir X.509 sertifikasıdır. Son Ara Sertifika zincirindeki yaprak sertifikayı imzalamak için kullanılır. Bir ara sertifika ayrıca için bir ara CA sertifikası olarak adlandırılır.

### <a name="end-entity-leaf-certificate"></a>Son varlık "yaprak" Sertifika

Yaprak sertifikayı ya da son varlık sertifikası, sertifika sahibinin tanımlar. Bunun yanı sıra sıfır veya daha fazla ara sertifikaları, Sertifika zincirindeki kök sertifika vardır. Yaprak sertifikayı başka bir sertifikaları imzalamak için kullanılmaz. Benzersiz cihaz sağlama hizmetine tanımlar ve bazen cihaz sertifikası olarak da adlandırılır. Kimlik doğrulaması sırasında cihaz, hizmetten kavram kanıtı elinde sınaması için yanıt vermek için bu sertifikayla ilişkili özel anahtarı kullanır.

Yaprak sertifikalar ile kullanılan bir [bireysel kayıt](./concepts-service.md#individual-enrollment) girişine sahip bir gereksinim, **konu adı** bireysel kayıt girişi için kayıt kimliği ayarlamanız gerekir. Yaprak sertifikalar ile kullanılan bir [kayıt grubu](./concepts-service.md#enrollment-group) girişi olmalıdır **konu adı** gösterilir istenen cihaz Kimliğine ayarlayın **kayıt kayıtları** için Kimliği doğrulanmış cihaz kayıt grubunda.

Daha fazla bilgi için bkz. [cihazların kimliğini doğrulama imzalanmış olan X.509 CA sertifikalarını](/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>X.509 sertifikalarıyla sağlama hizmetine cihaz erişimini denetleme

X.509 kanıtlama mekanizması kullanan cihazlar için erişimi denetlemek için kullanabileceğiniz bir kayıt girişi iki tür sağlama hizmeti sunar:  

- [Bireysel kayıt](./concepts-service.md#individual-enrollment) girişleri, belirli bir cihazla ilişkili cihaza sertifika ile yapılandırılır. Kayıtları belirli cihazlar için bu girişleri kontrol eder.
- [Kayıt grubu](./concepts-service.md#enrollment-group) girişler belirli ara veya kök CA sertifikası ile ilişkili. Bu girişler ara veya kök sertifika, Sertifika zincirindeki tüm cihazların kaydı sırasında denetler. 

Cihaz sağlama hizmetine bağlandığında, hizmet daha az belirgin kayıt girişlerini daha belirli kayıt girişlerini önceliklendirir. Diğer bir deyişle, cihazı için bireysel bir kayıt varsa, bu giriş sağlama hizmeti geçerlidir. Cihazı için bireysel hiç kayıt yoktur ve cihazın Sertifika zincirindeki ilk ara sertifika için kayıt grubu varsa hizmet, giriş ve kök zinciri'kurmak benzeri geçerlidir. Hizmet bulduğu ilk geçerli girişi geçerli olacağı şekilde:

- Bulunan ilk kayıt girişi etkinse, cihaz hizmet sağlar.
- Bulunan ilk kayıt girişi devre dışıysa, hizmetin cihaz sağlama değil.  
- Kayıt girdisi yok, herhangi bir cihazın Sertifika zincirindeki sertifika bulunursa, hizmetin cihaz sağlama değil. 

Bu mekanizma ve sertifika zincirleri hiyerarşik yapısını, cihaz grupları için de ayrı ayrı cihazlar için erişimi nasıl denetleyebileceğiniz güçlü esneklik sağlar. Örneğin, aşağıdaki sertifika zincirleriyle beş cihazın düşünün: 

- *Cihaz 1*: kök sertifikası sertifika bir sertifika cihaz 1 -> ->
- *Cihaz 2*: kök sertifikası sertifika bir sertifika cihaz 2 -> ->
- *Cihaz 3*: kök sertifikası sertifika bir cihaz 3 sertifika -> ->
- *Cihaz 4*: kök sertifikası sertifika B -> 4 cihaz sertifikası ->
- *Cihaz 5*: kök sertifikası sertifika B -> 5 cihaz sertifikası ->

Başlangıçta, tüm beş cihaza yönelik erişimi etkinleştirmek üzere kök sertifika için bir tek etkin Grup kaydı girişi oluşturun. Daha sonra B sertifika güvenliği tehlikeye girdiğinde, bir devre dışı kayıt grubu girdisini önlemek sertifika B için oluşturabileceğiniz *cihaz 4* ve *cihaz 5* kaydedilmesini. Yine de daha sonra *cihaz 3* olur gizliliği ihlal edilirse, sertifikasını için bir devre dışı bireysel kayıt girişi oluşturabilirsiniz. Bu erişimi iptal eder *cihaz 3*, ancak yine de verir *cihaz 1* ve *cihaz 2* kaydetmek için.