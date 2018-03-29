---
title: Azure IOT Hub cihaz sağlama hizmetinde güvenlik kavramları | Microsoft Docs
description: Güvenlik kavramları aygıt hizmeti sağlama ve IOT Hub ile cihazları için belirli sağlama açıklar
services: iot-dps
keywords: ''
author: nberdy
ms.author: nberdy
ms.date: 03/27/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 5e35a802349bd85b50a13a3d9a7e0c78945937bd
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>IOT Hub cihaz sağlama hizmeti güvenlik kavramları 

IOT Hub cihaz hizmeti sağlama, IOT Hub'de belirtilen bir IOT hub'ına sağlama zero touch Cihazınızı yapılandırmak üzere kullanmak için bir yardımcı hizmetidir. Cihaz Sağlama Hizmeti ile güvenli ve ölçeklenebilir bir şekilde milyonlarca cihaz sağlayabilirsiniz. Bu makalede genel bir fikir veren *güvenlik* kavramları söz konusu aygıt sağlama. Bu makalede, bir cihaz dağıtım için hazır hale katılan tüm kişiler için geçerlidir.

## <a name="attestation-mechanism"></a>Kanıtlama mekanizması

Kanıtlama mekanizması bir cihazın kimliğini onaylamak için kullanılan yöntem budur. Kanıtlama mekanizması, sağlama hizmeti hangi yöntemi ile belirli bir cihazı kullanmak üzere kanıtlama bildiren kayıt listesine de ilgilidir.

> [!NOTE]
> IOT hub'ı "kimlik doğrulama düzeni" benzer bir kavram, hizmet olarak kullanır.

Cihaz sağlama hizmeti kanıtlama iki tür destekler:
* **X.509 sertifikaları** üzerinde standart X.509 sertifikası kimlik doğrulaması akışı tabanlı.
* **Güvenilir Platform Modülü (TPM)** imzalı bir paylaşılan erişim imzası (SAS) belirteci sunmak için anahtarlar TPM standart kullanarak bir nonce challenge üzerinde temel. Bu aygıttaki fiziksel TPM gerektirmez, ancak her onay anahtarını kullanarak şifreli olarak hizmet bekliyor [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülü ya da HSM, cihaz gizli güvenli, donanım tabanlı depolama için kullanılan ve en güvenli gizli depolama biçimidir. X.509 sertifikaları ve SAS belirteci HSM'de depolanabilir. HSM'ler olabilir hem de kanıtlama mekanizmaları ile kullanılan sağlama destekler.

> [!TIP]
> Gizli aygıtlarınızda güvenli bir şekilde depolamak için bir HSM aygıtlar kullanarak öneririz.

Cihaz gizlilikler de yazılım (bellek) depolanan, ancak bu daha az güvenli depolama HSM daha biçimidir.

## <a name="trusted-platform-module"></a>Güvenilir Platform Modülü

TPM platform kimliğini doğrulamak için kullanılan anahtarları güvenli bir şekilde depolamak için bir standart başvurabilir veya standart uygulama modülleri ile etkileşim kurmak için kullanılan g/ç arabirimi başvurabilirsiniz. TPM'ler ayrık donanım, tümleşik donanım, bellenim veya yazılım tabanlı bulunabilir. Daha fazla bilgi edinmek [TPM'ler ve TPM kanıtlama](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Cihaz sağlama hizmeti yalnızca TPM 2.0 destekler.

### <a name="endorsement-key"></a>Onay anahtarı

Onay anahtarı dahili olarak oluşturulan veya zaman üretim sırasında eklenen TPM içinde yer alan bir asimetrik anahtar olduğunu ve her TPM için benzersizdir. Onay anahtarı kaldırılamaz veya değiştirilemez. Onay anahtarı ortak kısmını orijinal TPM tanımak için kullanılırken onay anahtarını özel bölümünü TPM dışında hiçbir zaman yayımlanır. Daha fazla bilgi edinmek [onay anahtarını](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

### <a name="storage-root-key"></a>Depolama kök anahtarı

Depolama kök anahtarı TPM'de depolanan ve böylece bu anahtarlar TPM olmadan kullanılamaz uygulamaları tarafından oluşturulan TPM anahtarlarını korumak için kullanılır. TPM sahipliğini depolama kök anahtarı oluşturulur; Yeni bir kullanıcı sahipliği olabilmesi için TPM temizlediğinizde, yeni bir depolama kök anahtarı oluşturulur. Daha fazla bilgi edinmek [depolama kök anahtarı](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="x509-certificates"></a>X.509 sertifikaları

X.509 sertifikaları kanıtlama mekanizması olarak kullanarak üretim ölçeği ve cihaz sağlamayı kolaylaştırmak için mükemmel bir yoldur. X.509 sertifikaları, genellikle bir Sertifika zincirindeki her sertifika, sonraki yüksek sertifika vb. bir otomatik olarak imzalanan kök sertifikayı sonlandırma, özel anahtarı tarafından imzalanır güven zinciri halinde düzenlenir. Bu, bir temsilci olarak atanan bir güvenilen kök sertifika yetkilisi (CA her ara CA'ın bir aygıtta yüklü son varlık sertifikası aracılığıyla) tarafından oluşturulan kök sertifika güven zinciri oluşturur. Daha fazla bilgi için bkz: [aygıt X.509 CA sertifikaları kullanarak kimlik doğrulaması](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Genellikle sertifika zinciri cihazlarla ilişkilendirilmiş bazı mantıksal veya fiziksel hiyerarşi temsil eder. Bir üretici Örneğin, otomatik olarak imzalanan kök CA sertifikası bu sertifikayı kullanmak her Fabrika için benzersiz bir ara CA sertifikası oluşturmak için her ürün için benzersiz bir ara CA sertifikası oluşturmak için her fabrikasının sertifika kullan Tesis satır ve son satırında üretilen her aygıt için benzersiz cihaz (son varlık) sertifikasını oluşturmak için üretim hattı sertifika kullanın. Daha fazla bilgi için bkz: [IOT endüstri X.509 CA sertifikaları kavramsal anlayış](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-concept). 

### <a name="root-certificate"></a>Kök sertifikası

Bir kök sertifikası, sertifika yetkilisi (CA) temsil eden otomatik olarak imzalanan bir X.509 sertifikasıdır. Terminus ya da sertifika zincirinin güven çıpası olduğundan. Kök sertifikalarını otomatik olarak bir kuruluş tarafından verilen veya bir kök sertifika yetkilisinden satın alınmış. Daha fazla bilgi için bkz: [alma X.509 CA sertifikaları](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). Kök sertifikayı, aynı zamanda bir kök CA sertifikası başvuruda bulunulabilir.

### <a name="intermediate-certificate"></a>Ara Sertifika

Bir ara sertifika kök sertifika (veya başka bir ara sertifika kendi zincirindeki kök sertifikası tarafından) imzalanmış bir X.509 sertifikası yok. Son Ara Sertifika zincirindeki yaprak sertifikasını imzalamak için kullanılır. Ara Sertifika, ayrıca bir ara CA sertifikası olarak başvuruda bulunulabilir.

### <a name="leaf-certificate"></a>Yaprak sertifikası

Yaprak sertifikası veya son varlık sertifikası, sertifika sahibinin tanımlar. Bu kök sertifika, Sertifika zincirindeki yanı sıra sıfır veya daha fazla ara sertifika verir. Yaprak sertifikası diğer sertifikaları imzalamak için kullanılmaz. Benzersiz cihaz sağlama hizmetine tanımlar ve bazen aygıt sertifikası olarak da adlandırılır. Kimlik doğrulaması sırasında aygıt hizmetinden kanıtı elinde sınaması için yanıt bu sertifikayla ilişkili özel anahtarı kullanır. Daha fazla bilgi için bkz: [aygıtları kimlik doğrulaması X.509 CA sertifikaları ile imzalanmış](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>X.509 sertifikaları sağlama hizmetiyle cihaz erişimi denetleme

Sağlama hizmeti X.509 kanıtlama mekanizması kullanan cihazlar için erişimi denetlemek için kullanabileceğiniz kayıt girişi iki tür sunar:  

- [Tek tek kayıt](./concepts-service.md#individual-enrollment) girişleri, belirli bir aygıt ile ilişkili aygıt sertifikası ile yapılandırılır. Bu girişler kayıtları belirli cihazlar için denetler.
- [Kayıt grubu](./concepts-service.md#enrollment-group) girişler belirli ara veya kök CA sertifikası ile ilişkili. Bu girişler Kayıtları Ara veya kök sertifika, Sertifika zincirindeki tüm cihazlar için denetler. 

Bir cihaz sağlama hizmete bağlandığında, hizmet üzerinde daha az belirli kayıt girişlerini daha ayrıntılı kayıt girişlerini öncelik verir. Diğer bir deyişle, cihaz için ayrı bir kayıt varsa, sağlama hizmeti bu girişi geçerlidir. Cihaz için ayrı ayrı hiçbir kayıt ve cihazın Sertifika zincirindeki ilk ara sertifika için bir kayıt grubu var, bu giriş vb., kök zincir oluşturan hizmet geçerlidir. Hizmet bulduğu ilk uygulanabilir girişi geçerlidir şekilde:

- İlk kayıt girdisi bulundu etkinleştirilirse, hizmet cihaz sağlar.
- İlk kayıt girdisi bulundu devre dışıysa, hizmet cihazı hazırlama değil.  
- Herhangi bir cihazın Sertifika zincirindeki tüm sertifikalar için kayıt giriş bulunursa, hizmet cihazı hazırlama değil. 

Bu mekanizma ve sertifika zincirleri hiyerarşik yapısını gruplarına, cihaz için de ayrı ayrı cihazlar için erişimi nasıl kontrol edebilir güçlü esneklik sağlar. Örneğin, aşağıdaki sertifika zincirleri beş aygıtlarla düşünün: 

- *Cihaz 1*: kök sertifikası sertifika A aygıt 1 sertifika -> ->
- *Cihaz 2*: kök sertifikası sertifika A cihaz 2 sertifikası -> ->
- *Cihaz 3*: kök sertifikası sertifika A cihaz 3 sertifikası -> ->
- *Cihaz 4*: kök sertifikası sertifika B -> cihaz 4 sertifikası ->
- *Cihaz 5*: kök sertifikası sertifika B -> cihaz 5 sertifikası ->

Başlangıçta, bir tek etkinleştirilmiş Grup kayıt girişi tüm beş cihazlar için erişimi etkinleştirmek kök sertifikasının oluşturabilirsiniz. Sertifika daha sonra B güvenliği tehlikeye girdiğinde, önlemek B sertifika için bir devre dışı kayıt Grup girişi oluşturabilirsiniz *aygıt 4* ve *aygıt 5* kaydetme gelen. Hala sonraki varsa *aygıt 3* hale tehlikeye, kendi sertifika için bir devre dışı bırakılmış tek tek kayıt girişi oluşturabilirsiniz. Bu erişimi iptal *aygıt 3*, ancak hala verir *aygıt 1* ve *cihaz 2* kaydetmek için.