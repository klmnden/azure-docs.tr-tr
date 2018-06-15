---
title: Azure güvenlik ve uyumluluk şeması FedRAMP Web uygulamaları, Otomasyon - sistem ve iletişim koruma
description: FedRAMP Web uygulamaları Otomasyon - sistem ve iletişim koruması
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: b96cdef1-ce3a-4f73-9a9e-f2cbd056d485
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 6749ad50cd1ea1cd4ec6ca2f86fef43a9f1515d9
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33207132"
---
# <a name="system-and-communications-protection-sc"></a>Sistem ve iletişim Koruması (SC)

> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.

## <a name="nist-800-53-control-sc-1"></a>NIST 800 53 denetim SC-1

#### <a name="system-and-communications-protection-policy-and-procedures"></a>Sistem ve iletişim koruma İlkesi ve yordamları

**SC-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim sistemi ve iletişim koruma ilke Taahhüt, Kurumsal varlıklar ve uyumluluk arasında koordinasyon; ve sistem iletişimi koruma ilkesini ve ilişkili sistem ve iletişim koruma denetimleri uyarlamasını kolaylaştırmak için yordamlar; gözden geçirir ve geçerli sistem ve iletişim koruma İlkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve sistem ve iletişim koruma yordamlar [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde sistem ve iletişim koruma ilkesini ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-2"></a>NIST 800 53 denetim SC-2

#### <a name="application-partitioning"></a>Uygulama bölümlendirme

**SC-2** Information system (kullanıcı arabirimi Hizmetleri dahil) kullanıcı işlevselliği bilgileri sistem yönetimi işlevinden ayırır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması sistem yönetimi işlevselliğini zorlama mantıksal erişim denetimleri ve sistem mimarisi ile kullanıcı işlevselliği ayırır. Müşteri tarafından dağıtılan web uygulama arabirimlerine kullanıcı işlevselliği sınırlıdır. Sistem Yönetimi işlevleri için arabirimleri kullanıcı arabirimlerinden ayrıdır. Üretim kaynakları uygun şekilde erişimi sınırlamak için ağ güvenlik grubu kuralları ile bir yönetim alt ağda bulunan bir güvenli savunma konak (jumpbox) aracılığıyla tüm yönetim bağlantısı durumda. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-3"></a>NIST 800 53 denetim SC-3

#### <a name="security-function-isolation"></a>Güvenlik işlevi yalıtımı

**SC-3** bilgi sistemi güvenlik işlevleri güvenlikle ilgili olmayan işlevlerden yalıtır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Windows, her işlem için bir özel sanal adres alanı atayarak yürütülen her işlem için ayrı yürütme etki alanları korur. Ayrıca, çözüm uygular bir mimari ve erişim denetimleri gerektiğinde güvenlik işlevi yalıtmak için tasarlanmış. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-4"></a>NIST 800 53 denetim SC-4

#### <a name="information-in-shared-resources"></a>Paylaşılan kaynaklar bilgileri

**SC-4** yetkisiz ve istenmeyen bilgi aktarımı paylaşılan sistem kaynaklarının aracılığıyla bilgi sistemi engeller.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Bilgi erişilebilir yalnızca kullanıcılar ve roller uygun izinlere sahip olacak şekilde, işletim sistemi kaynaklarını (örneğin, bellek, depolama) yönetir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-5"></a>NIST 800 53 denetim SC-5

#### <a name="denial-of-service-protection"></a>Hizmet koruma engelleme

**SC-5** bilgi sistemi korur veya hizmet reddi saldırılarını aşağıdaki türlerini etkilerini sınırlar: [atama: kuruluş tanımlı türler hizmet saldırılarını veya kaynakları gibi bilgileri için başvurular reddi] tarafından kullanan [atama: kuruluş tanımlanan güvenlik önlemlerinin].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması Yük Dengeleme yetenekleri ve bir web uygulaması güvenlik duvarı içeren uygulama ağ geçidi dağıtır. Web katmanı, veritabanı katmanı ve Active Directory destekleme dağıtılan sanal makinelerin ölçeklendirilebilir kullanılabilirlik kümesinde dağıtılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-6"></a>NIST 800 53 denetim SC-6

#### <a name="resource-availability"></a>Kaynak kullanılabilirliği

**SC-6** ayırarak bilgileri sistem kaynakları kullanılabilirliği korur [atama: kuruluş tanımlanan kaynaklara] tarafından [seçimi (bir veya daha fazla) öncelik; kota; [Atama: kuruluş tanımlanan güvenlik önlemlerinin]].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Her Windows işlemi bir programı çalıştırmak için gereken kaynakları sağlar. Kaynak önceliği işletim sistemi tarafından yönetilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-7a"></a>NIST 800 53 denetim SC-7.a

#### <a name="boundary-protection"></a>Sınır koruma

**SC-7.a** bilgi sistemi izleyiciler ve iletişimi denetleyen sisteminin dış sınır ve anahtar iç sınırlar içinde sistem.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir uygulama ağ geçidi dağıtır, yük dengeleyici ve commutations dış sınırlarında ve iç alt ağlar arasında denetlemek için ağ güvenlik grubu kuralları yapılandırır. Uygulama ağ geçidi, yük dengeleyici ve ağ güvenlik grubu olayı ve tanılama günlüklerini izleme müşteri izin vermek için günlük analizi tarafından toplanır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-7b"></a>NIST 800 53 denetim SC-7.b

#### <a name="boundary-protection"></a>Sınır koruma

**SC-7.b** bilgi sistemi genel olarak erişilebilir sistem bileşenleri için alt ağlar uygulayan [seçim: fiziksel olarak; mantıksal] iç kuruluş ağlardan ayrılmış.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir mimari ayrı web alt ağ, veritabanı alt ağ, Active Directory alt ve yönetimi alt ağ kaynaklarında dağıtır. Alt ağları için yalnızca bu gerekli sistem ve yönetim işlevselliği için alt ağlar arasında trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kural tarafından mantıksal olarak ayrılır (örn., dış trafiğin veritabanına, yönetim, erişilemiyor veya Active Directory alt ağlar). |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-7c"></a>NIST 800 53 denetim SC-7.c

#### <a name="boundary-protection"></a>Sınır koruma

**SC-7.c** bilgi sistemi dış ağlara ya da bilgi systems yalnızca bir kuruluş güvenliği mimari göre düzenlenmiş sınır koruma cihazları oluşan yönetilen arabirimleri üzerinden bağlanır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması müşteri tarafından dağıtılan web uygulamasının dış bağlantıları yönetmek için uygulama ağ geçidi dağıtır. Dış bağlantıları yönetim erişimi için bir savunma konağa kısıtlanmış / IP adresleri için dış bağlantılar kısıtlamak amacıyla uygulanan ağ güvenlik kuralları ile yönetimi alt ağda dağıtılan jumpbox yetkili. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-3"></a>NIST 800 53 denetim SC-7 (3)

#### <a name="boundary-protection--access-points"></a>Sınır koruma | Erişim noktaları

**SC-7 (3)** kuruluş bilgilerini sisteme dış ağ bağlantılarının sayısını sınırlar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | İki ortak IP adresi bu şeması dağıtır: bir uygulama ağ geçidi ile; ilişkili bir yönetim savunma konakla ilişkili / jumpbox. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-4a"></a>NIST 800 53 denetim SC-7 (4) bir

#### <a name="boundary-protection--external-telecommunications-services"></a>Sınır koruma | Dış telekomünikasyon Hizmetleri

**SC-7 (4) bir** kuruluş her dış telekomünikasyon hizmet için yönetilen bir arabirimi uygular.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | İki ortak IP adresi bu şeması dağıtır: bir uygulama ağ geçidi ile; ilişkili bir yönetim savunma konakla ilişkili / jumpbox. Bu arabirimleri yönetimini yazılım tanımlı ağ üzerinden etkinleştirilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-4b"></a>NIST 800 53 denetim SC-7 (4) .b

#### <a name="boundary-protection--external-telecommunications-services"></a>Sınır koruma | Dış telekomünikasyon Hizmetleri

**SC-7 (4) .b** kuruluş her yönetilen arabirimi için bir trafik akışı ilkesi oluşturur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | İki ortak IP adresi bu şeması dağıtır: bir uygulama ağ geçidi ile; ilişkili bir yönetim savunma konakla ilişkili / jumpbox. Bu arabirimleri yönetimini yazılım tanımlı ağ üzerinden etkinleştirilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-4c"></a>NIST 800 53 denetim SC-7 (4) .c

#### <a name="boundary-protection--external-telecommunications-services"></a>Sınır koruma | Dış telekomünikasyon Hizmetleri

**SC-7 (4) .c** kuruluşun gizlilik ve her bir arabirim aktarılan bilgilerin bütünlüğünü korur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan web uygulama ağ geçidi, bir HTTPS dinleyicisi, olmadığını gizliliği ve bütünlük iletişimleri oturumlarının ile yapılandırılır. Uzak Masaüstü bağlantılarını jumpbox gizliliği ve bütünlük sağlama de şifrelenir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-4d"></a>NIST 800 53 denetim SC-7 (4) .d

#### <a name="boundary-protection--external-telecommunications-services"></a>Sınır koruma | Dış telekomünikasyon Hizmetleri

**SC-7 (4) .d** her özel durum destekleyen görev/iş gereksinimlerini ve gereken süresi ile trafik akışı ilkeye kuruluş belgeleri.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteriler (telekomünikasyon Hizmetleri dahil etmek) datacenter işlemleri için sorumlu değildir. Tüm telekomünikasyon hizmetlerini sağlanan ve Microsoft Azure tarafından yönetilir. Bu denetim Azure kaynağından devralındı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-4e"></a>NIST 800 53 denetim SC-7 (4) .e

#### <a name="boundary-protection--external-telecommunications-services"></a>Sınır koruma | Dış telekomünikasyon Hizmetleri

**SC-7 (4) .e** Trafik akışı ilkesi için özel durumlar kuruluş incelemeleri [atama: kuruluş tarafından tanımlanan sıklığı] ve açık bir görev/iş gereksinimine göre artık desteklenmeyen özel durumları kaldırır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteriler (telekomünikasyon Hizmetleri dahil etmek) datacenter işlemleri için sorumlu değildir. Tüm telekomünikasyon hizmetlerini sağlanan ve Microsoft Azure tarafından yönetilir. Bu denetim Azure kaynağından devralındı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-5"></a>NIST 800 53 denetim SC-7 (5)

#### <a name="boundary-protection--deny-by-default--allow-by-exception"></a>Sınır koruma | Varsayılan olarak reddetme / tarafından özel durumuna izin ver

**SC-7 (5)** yönetilen arabirimleri bilgileri sistem varsayılan olarak ağ iletişimleri trafiği engeller ve bir özel durum nedeniyle ağ iletişimleri trafiğine izin verir (yani, tüm reddet, özel durum nedeniyle izin).

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Ağ güvenlik grubu bu şeması tarafından dağıtılan uygulanan Rulesets varsayılan olarak izin verme düzeni kullanılarak yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-7"></a>NIST 800 53 denetim SC-7 (7)

#### <a name="boundary-protection--prevent-split-tunneling-for-remote-devices"></a>Sınır koruma | Uzak cihazlar için bölünmüş tünel engelle

**SC-7 (7)** bilgi sistemin, uzak bir aygıt ile birlikte aynı anda sistemiyle uzaktan olmayan bağlantı kurma ve dış ağlarda kaynaklara diğer bazı bağlantı üzerinden iletişim kurarken cihazın engeller.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde uzak cihaz yapılandırma İlkesi bölünmüş tünel adresi. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-8"></a>NIST 800 53 denetim SC-7 (8)

#### <a name="boundary-protection--route-traffic-to-authenticated-proxy-servers"></a>Sınır koruma | Doğrulanmış bir Proxy sunucularına trafiği yönlendirme

**SC-7 (8)** bilgileri sistem yolları [atama: kuruluş tarafından tanımlanan iç iletişim trafiği] için [atama: kuruluş tarafından tanımlanan dış ağlara] aracılığıyla yönetilen arabirimleri proxy sunucularda kimlik doğrulaması.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşterinin bir dış ağa yönlendirme müşteri tarafından tanımlanan bilgileri doğrulanmış bir proxy üzerinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-10"></a>NIST 800 53 denetim SC-7 (10)

#### <a name="boundary-protection--prevent-unauthorized-exfiltration"></a>Sınır koruma | Yetkisiz Exfiltration engelle

**SC-7 (10)** kuruluş arasında yönetilen arabirimleri bilgilerin yetkisiz exfiltration engeller.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bilgileri yetkisiz exfiltration yönetilen arabirimleri önlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-12"></a>NIST 800 53 denetim SC-7 (12)

#### <a name="boundary-protection--host-based-protection"></a>Sınır koruma | Ana bilgisayar tabanlı koruma

**SC-7 (12)** kuruluş uygular [atama: ana bilgisayar tabanlı sınır kuruluşunuz tarafından tanımlanan koruma mekanizmalarını] konumundaki [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri bir ana bilgisayar tabanlı güvenlik duvarı etkin olan yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-13"></a>NIST 800 53 denetim SC-7 (13)

#### <a name="boundary-protection--isolation-of-security-tools--mechanisms--support-components"></a>Sınır koruma | Güvenlik araçları yalıtım / mekanizmaları / destek bileşenleri

**SC-7 (13)** kuruluş yalıtır [atama: kuruluşunuz tarafından tanımlanan bilgileri güvenlik araçları, mekanizmaları ve destek bileşenlerini] fiziksel olarak uygulayarak diğer iç bilgileri sistem bileşenlerinden alt ağlar ile ayırın. diğer sistem bileşenlerini yönetilen arabirimleri.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bilgileri güvenlik araçları ve destek bileşenlerini müşteri dağıtımı için ayrı bir yönetim alt ağ ile bir mimari kaynaklarında dağıtır. Alt ağ güvenlik grubu kural tarafından mantıksal olarak ayrılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-18"></a>NIST 800 53 denetim SC-7 (18)

#### <a name="boundary-protection--fail-secure"></a>Sınır koruma | Güvenli başarısız

**SC-7 (18)** bilgi sistemi güvenli bir şekilde bir sınır koruma aygıt işletimsel hatası durumunda başarısız olur.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında fiziksel sınır koruma aygıt yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure, coğrafi olarak ayrı olarak yedekli ağ geçidi sunucularını ve SSL VPN dağıtır. Bir ağ geçidi sistemi başarısız olduğunda, güvenli bir şekilde başarısız olur ve ortama erişimi sınırlıdır. Microsoft Azure ortamına bir bağlantı kurmak için bir kullanıcı Microsoft Azure tarafından yönetilen etkin bir ağ geçidi sunucusu için ayrı bir bağlantı oluşturmanız gerekir. <br /> Microsoft Azure ağ aygıtlarını (sınır yönlendiricileri, erişim yönlendiriciler, yük Dengeleyiciler, toplama anahtarları ve TORS dahil) başarısız olursa, buna ek olarak, etkilenen bağlantı hattı, böylece güvenli bir şekilde başarısız bağlantısının kesilmesi. Microsoft Azure ağ aygıtının bir hata neden veya bilgi cihaz girmek için sisteme dış neden ya da bir hata yetkisiz bilgi yayın izin verebilirsiniz. Microsoft Azure varlıklar kullanılabilirliğini etkilemeden başarısız yerleşik artıklık sağlar. |


 ### <a name="nist-800-53-control-sc-7-20"></a>NIST 800 53 denetim SC-7 (20)

#### <a name="boundary-protection--dynamic-isolation--segregation"></a>Sınır koruma | Dinamik yalıtım / arasında ayrım yapma

**SC-7 (20)** bilgi sistemi dinamik ayırma/kurabilmeleri için yeteneği sağlar [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri] sisteminin diğer bileşenler'den.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, sistem müşteri tarafından dağıtılan kaynakları dinamik olarak ayırmak için özelliğine sahip sağlamak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-7-21"></a>NIST 800 53 denetim SC-7 (21)

#### <a name="boundary-protection--isolation-of-information-system-components"></a>Sınır koruma | Yalıtım bilgileri sistem bileşenleri

**SC-7 (21)** kuruluş sınır koruma mekanizmalarını ayırmak için kullanır [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri] destekleme [atama: kuruluş tarafından tanımlanan görevler ve/veya iş işlevleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir mimari ayrı web alt ağ, veritabanı alt ağ, Active Directory alt ve yönetimi alt ağ kaynaklarında dağıtır. Alt ağları için yalnızca bu gerekli sistem ve yönetim işlevselliği için alt ağlar arasında trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kural tarafından mantıksal olarak ayrılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-8"></a>NIST 800 53 denetim SC-8

#### <a name="transmission-confidentiality-and-integrity"></a>İletim gizliliği ve bütünlük

**SC-8** bilgi sistemi korur [seçimi (bir veya daha fazla): gizlilik; bütünlüğü] aktarılan bilgileri.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | SI-8 (1) uygulama bu denetimin gereksinimini karşılar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-8-1"></a>NIST 800 53 denetim SC-8 (1)

#### <a name="transmission-confidentiality-and-integrity--cryptographic-or-alternate-physical-protection"></a>İletim gizliliği ve bütünlük | Şifreleme veya diğer fiziksel koruma

**SC-8 (1)** bilgileri sistem şifreleme mekanizmasıyla uygulayan [seçimi (bir veya daha fazla): yetkisiz bilgilerin açığa çıkmasının önleme; bilgilere değişikliklerini algılamak] iletim sırasında Aksi halde tarafından korunan sürece [atama: Kuruluşunuz tarafından tanımlanan alternatif fiziksel güvenlik önlemleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması kaynakları yalnızca güvenli protokolleri kullanarak iletişim kuracak şekilde yapılandırır. Uygulama ağ geçidi WAF bileşeninin iletişim ekibi dış kullanır gelen HTTPS/TLS kabul etmek ve arka uç havuzu ile yalnızca HTTPS/TLS iletişim kurmak için yapılandırılır. SQL Server, yalnızca HTTPS/TLS iletişim kuracak şekilde yapılandırıldı. Uzak Masaüstü Hizmetleri, güvenli bağlantı kullanacak şekilde yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-10"></a>NIST 800 53 denetim SC-10

#### <a name="network-disconnect"></a>Ağ bağlantıyı kes

**SC-10** bilgi sistemi iletişimlerdeki oturumunun veya sonra sonunda ilişkili ağ bağlantıyı sonlandırır [atama: kuruluş tanımlı süre] işlem yapılmadan geçen süre.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Uzak Masaüstü oturumları için kimlik doğrulaması Active Directory tarafından yönetilir. Active Directory'de bir kullanıcı için erişim devre dışı bırakıldığında, Uzak Oturumlar hemen sonlandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-12"></a>NIST 800 53 denetim SC-12

#### <a name="cryptographic-key-establishment-and-management"></a>Şifreleme anahtarı oluşturulması ve Yönetimi

**SC-12** kuruluş kurar ve ile uyumlu olarak bilgi sistemi içinde değişiklik gerekli şifreleme için şifreleme anahtarları yönetir [atama: anahtar oluşturma, dağıtım, kuruluşun tanımlı gereksinimleri Depolama, erişimi ve yok etme].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir Azure anahtar kasası dağıtır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure anahtar kasası anahtarlarını kullanarak bir FIPS 140-2 Düzey 2 donanım güvenlik modülü (HSM) anahtar oluşturma yeteneği oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-12-1"></a>NIST 800 53 denetim SC-12 (1)

#### <a name="cryptographic-key-establishment-and-management--availability"></a>Şifreleme anahtarı oluşturulması ve Yönetimi | Kullanılabilirlik

**SC-12 (1)** kuruluş kullanılabilirliği kullanıcılar tarafından şifreleme anahtarlarının bilgi kaybı durumunda tutar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Azure anahtar kasası, şifreleme anahtarları ve bu şeması kullanılan gizli anahtarları depolamak için kullanılır. Anahtar kasası erişmek ve veri şifreleme anahtarları için anahtar yönetimi işlemini kolaylaştırır. Aşağıdaki kimlik doğrulayan anahtar kasasında depolanan: Azure parolasını dağıtma hesabı, sanal makine yönetici parolası, SQL Server hizmet hesabı parolası. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-12-2"></a>NIST 800 53 denetim SC-12 (2)

#### <a name="cryptographic-key-establishment-and-management--symmetric-keys"></a>Şifreleme anahtarı oluşturulması ve Yönetimi | Simetrik anahtarlar

**SC-12 (2)** kuruluş oluşturur, denetler ve simetrik şifreleme anahtarlarını kullanarak dağıtır [seçim: NIST FIPS uyumlu; Anahtar Yönetimi NSA onaylı] teknolojisi ve işlemler.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Azure anahtar kasası oluşturmak, denetlemek ve şifreleme anahtarları dağıtmak için kullanılır. Azure anahtar kasası anahtarlarını kullanarak bir FIPS 140-2 Düzey 2 donanım güvenlik modülü (HSM) anahtar oluşturma yeteneği oluşturabilir. Anahtarları saklanır ve Azure anahtar kasası güvenli bir şekilde şifrelenmiş kapsayıcılara içinde yönetilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-12-3"></a>NIST 800 53 denetim SC-12 (3)

#### <a name="cryptographic-key-establishment-and-management--asymmetric-keys"></a>Şifreleme anahtarı oluşturulması ve Yönetimi | Asimetrik anahtarlar

**SC-12 (3)** kuruluş oluşturur, denetler ve asimetrik şifreleme anahtarlarını kullanarak dağıtır [seçim: NSA onaylı anahtar yönetim teknolojisi ve işlemleri; onaylanmış PKI sınıf 3 sertifikalar veya prepositioned anahtar malzemesini; PKI sınıf 3 veya sınıf 4 sertifikaları ve kullanıcının özel anahtarını korumak donanım güvenlik belirteçleri onaylanmış].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri oluşturan, denetleme ve (müşteri tarafından dağıtılan kaynakları içinde kullanılıyorsa) asimetrik şifreleme anahtarları dağıtma sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-13"></a>NIST 800 53 denetim SC-13

#### <a name="cryptographic-protection"></a>Şifreleme koruma

**SC-13** bilgi sistemi uygulayan [atama: kuruluş tarafından tanımlanan şifreleme kullanır ve her kullanım için gereken şifreleme türünü] geçerli federal yasalar, Executive siparişleri yönergeleri, ilkeleri düzenlemeleri uygun olarak ve standartları.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Windows kimlik doğrulaması, Uzak Masaüstü'nü ve BitLocker tarafından bu şeması görevli olduğu. Bu bileşenler, FIPS 140 doğrulanmış şifreleme modüllerinde yararlanmayı yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-15a"></a>NIST 800 53 denetim SC-15.a

#### <a name="collaborative-computing-devices"></a>Ortak bilgi işlem aygıtları

**SC-15.a** bilgi sistemi uzaktan etkinleştirme aşağıdaki istisnalar dışında İşbirliği Bilgisayar aygıtların yasaklar: [atama: kuruluş tanımlı özel durumları olduğu uzaktan etkinleştirmeye izin].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan işbirliği bilgisayar aygıt yok. Not: Azure üzerinde dağıtılan sistemler kapsamında işbirliği fiziksel hesaplama cihazları vardır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-15b"></a>NIST 800 53 denetim SC-15.b

#### <a name="collaborative-computing-devices"></a>Ortak bilgi işlem aygıtları

**SC-15.b** cihazları fiziksel olarak mevcut kullanıcılara açık göstergesidir kullanım bilgileri sistemi sağlar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan işbirliği bilgisayar aygıt yok. Not: Azure üzerinde dağıtılan sistemler kapsamında işbirliği fiziksel hesaplama cihazları vardır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-17"></a>NIST 800 53 denetim SC-17

#### <a name="public-key-infrastructure-certificates"></a>Ortak anahtar altyapısı sertifikaları

**SC-17** kuruluş altında ortak anahtar sertifikaları veren bir [atama: kuruluş tarafından tanımlanan sertifika ilkesi] veya ortak anahtar sertifikaları bir onaylanan hizmet sağlayıcısı'ndan alır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri kuruluş düzeyi bir ortak anahtar altyapısı için sertifika verme bağlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-18a"></a>NIST 800 53 denetim SC-18.a

#### <a name="mobile-code"></a>Mobil kod

**SC-18.a** kuruluş mobil kod kabul edilebilir ve kabul edilemez ve mobil kod teknolojileri tanımlar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde sistem ve iletişim koruma yordamları kabul edilebilir ve kabul edilemez mobil kod tanımlayabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-18b"></a>NIST 800 53 denetim SC-18.b

#### <a name="mobile-code"></a>Mobil kod

**SC-18.b** kuruluş kullanım kısıtlamaları ve uygulama yönergeleri için kabul edilebilir mobil kodu ve mobil kod teknolojileri oluşturur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde sistem ve iletişim koruma yordamları kullanma kısıtlamaları mobil kod oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-18c"></a>NIST 800 53 denetim SC-18.c

#### <a name="mobile-code"></a>Mobil kod

**SC-18.c** kuruluş yetkilendirir, izler ve bilgi sistemi içinde mobil kod kullanımını denetler.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri yetkilendirme, izlenmesi ve denetimi mobil kod kullanımı için bir kuruluş düzeyi işlemi bağlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-19a"></a>NIST 800 53 denetim SC-19.a

#### <a name="voice-over-internet-protocol"></a>Internet protokolü üzerinden ses

**SC-19.a** kullanım kısıtlamaları ve kötü amaçlı olarak kullanıldığında bilgi sisteme zarar görmesine neden olma olasılığı tabanlı Internet Protokolü (VoIP) teknolojileri üzerinden Uygulama Kılavuzu sesi için kuruluş oluşturur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan Internet Protokolü teknolojileri üzerinden hiçbir voice vardır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-19b"></a>NIST 800 53 denetim SC-19.b

#### <a name="voice-over-internet-protocol"></a>Internet protokolü üzerinden ses

**SC-19.b** kuruluş yetkilendirir, izler ve bilgi sistemi içinde VoIP kullanımını denetler.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan Internet Protokolü teknolojileri üzerinden hiçbir voice vardır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-20a"></a>NIST 800 53 denetim SC-20.a

#### <a name="secure-name--address-resolution-service-authoritative-source"></a>Güvenli adı / adresi çözümleme hizmeti (yetkili kaynağı)

**SC-20.a** bilgi sistemi ek veri kaynağı kimlik doğrulaması ve bütünlük doğrulama yapıları sistem döndürür dış adı/adresi çözümleme sorgulara yanıt yetkili ad çözümlemesi verilerin sağlar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri için güvenli bir ad ve adres çözünürlüğü hizmeti sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-20b"></a>NIST 800 53 denetim SC-20.b

#### <a name="secure-name--address-resolution-service-authoritative-source"></a>Güvenli adı / adresi çözümleme hizmeti (yetkili kaynağı)

**SC-20.b** bilgi sistemi alt bölgeleri ve (alt güvenli Çözümleme Hizmetleri destekliyorsa) güvenlik durumunu göstermek için sağlar parçası olarak çalışırken, bir üst ve alt etki alanları arasında güven zinciri doğrulamayı etkinleştirmek için Dağıtılmış, hiyerarşik ad alanı.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri için güvenli bir ad ve adres çözünürlüğü hizmeti sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-21"></a>NIST 800 53 denetim SC-21

#### <a name="secure-name--address-resolution-service-recursive-or-caching-resolver"></a>Güvenli adı / adresi çözümleme hizmeti (yinelemeli veya önbellek Çözümleyicisi)

**SC-21** bilgi sistemi ister ve sistem alan yetkili kaynaklardan gelen ad/adres çözümlemesi yanıtları veri kaynağı kimlik doğrulaması ve veri bütünlük doğrulaması gerçekleştirir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri isteği ve veri kaynağı kimlik doğrulaması ve veri bütünlük doğrulaması yetkili kaynaklardan alınan ad/adres çözünürlüğü yanıtların gerçekleştirdiğinizden kaynaklarını müşteri tarafından dağıtılan yapılandırma için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-22"></a>NIST 800 53 denetim SC-22

#### <a name="architecture-and-provisioning-for-name--address-resolution-service"></a>Mimari ve sağlama için ad / adres çözümleme hizmeti

**SC-22** topluca bir kuruluş adı/adresi çözümleme hizmeti sağlama bilgi systems hataya dayanıklı ve iç/dış rol ayrımı uygulamak.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri adresi Müşteri tarafından dağıtılan kaynaklar için çözümleme hizmetleri sağlayan sistemleri hataya dayanıklı ve iç/dış rol ayrımı uygulamak sağlamak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-23"></a>NIST 800 53 denetim SC-23

#### <a name="session-authenticity"></a>Oturum Orijinallik Sertifikası

**SC-23** bilgi sistemi iletişim oturumları özgünlüğünü korur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Azure portalı, Uzak Masaüstü Bağlantısı ve web uygulama ağ geçidi dahil olmak üzere bu şeması tarafından dağıtılan kaynaklara uzaktan erişim güvenli TLS kullanarak. TLS oturum düzeyinde iletişimleri için Orijinallik Sertifikası sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-23-1"></a>NIST 800 53 denetim SC-23 (1)

#### <a name="session-authenticity--invalidate-session-identifiers-at-logout"></a>Oturum Orijinallik | Oturum kapatma oturum tanımlayıcıları geçersiz kıl

**SC-23 (1)** bilgi sistemi oturum tanımlayıcıları kullanıcı oturum kapatma veya diğer oturum sonlandırma sırasında geçersiz kılar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Azure portalı, Uzak Masaüstü Bağlantısı ve web uygulama ağ geçidi dahil olmak üzere bu şeması tarafından dağıtılan kaynaklara uzaktan erişim güvenli TLS kullanarak. Uzak Masaüstü oturumları ve Azure portalında oturum tanımlayıcıları oturum kapatma sırasında geçersiz. Web oturumu geçersiz kılma Azure uygulama geçidinden - Web uygulaması Güvenlik Duvarı (WAF) kurallar zorlanır. WAF her oturum tanımlama bilgisi benzeşim uygular ve 30 dakika (Kuruluş belirli kurallar dağıtımına yapılandırılabilir post) istemciden kaldıktan sonra oturum zaman aşımı gerçekleştirir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-24"></a>NIST 800 53 denetim SC-24

#### <a name="fail-in-known-state"></a>Bilinen durumda başarısız

**SC-24** bilgi sistemi başarısız bir [atama: kuruluş tarafından tanımlanan bilinen durumu] için [atama: kuruluş tanımlı türler hatalar] koruma [atama: kuruluş tarafından tanımlanan sistem durumu bilgisi] hata.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakları başarısız bilinen durumda sağlamak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-28"></a>NIST 800 53 denetim SC-28

#### <a name="protection-of-information-at-rest"></a>REST bilgilerin korunması

**SC-28** bilgi sistemi korur [seçimi (bir veya daha fazla): gizlilik; bütünlüğü], [atama: kuruluş tarafından tanımlanan bilgileri REST].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | SC-28 (1) uygulama bu denetimin gereksinimini karşılar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-sc-28-1"></a>NIST 800 53 denetim SC-28 (1)

#### <a name="protection-of-information-at-rest--cryptographic-protection"></a>REST bilgilerin korunması | Şifreleme koruma

**SC-28 (1)** yetkisiz olarak ifşa ve değiştirilmesini önlemek için şifreleme mekanizmasıyla bilgi sistemi uygulayan [atama: kuruluş tarafından tanımlanan bilgileri] üzerinde [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri gizliliği ve bütünlük REST bilgilerinin korumak için disk şifrelemesi uygulayın. Windows için Azure disk şifrelemesi, Windows BitLocker özelliği kullanılarak gerçekleştirilir. SQL Server saydam veri şifreleme (gerçek zamanlı şifreleme ve şifre çözme veri gerçekleştiren TDE), kullanın ve günlük dosyalarını REST bilgileri korumak için yapılandırılır. TDE, depolanan verileri güvence yetkisiz erişim ayarlanmadı sağlar. Müşteri depolanan bilgilerin bütünlüğünü korumak için ek uygulama düzeyinde denetimler uygulamak tercih edebilirsiniz. Gizliliği ve bütünlük bu şeması tarafından dağıtılan tüm depolama BLOB Azure Storage hizmeti şifreleme (SSE) kullanarak korunur. SSE 256 bit AES şifreleme kullanarak Azure depolama hesapları içinde kalan verileri koruma sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-sc-39"></a>NIST 800 53 denetim SC-39

#### <a name="process-isolation"></a>İşlem yalıtımı

**SC-39** yürütülen her işlem için ayrı yürütme etki alanı bilgileri sistem korur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Windows, her işlem için bir özel sanal adres alanı atayarak yürütülen her işlem için ayrı yürütme etki alanları korur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |
