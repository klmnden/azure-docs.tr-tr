---
title: Birleşik Krallık resmi üç katmanlı Web uygulamaları Otomasyon - genel bakış
description: Birleşik Krallık resmi üç katmanlı Web uygulamaları Otomasyon - genel bakış
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 49652fd9-b8c6-4a88-bc5e-0b58a0260ed6
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 787f504dea6e98911d6ba4716e88aff322c70c5b
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
ms.locfileid: "29151387"
---
# <a name="national-cyber-security-centre-cloud-security-principles-overview"></a>Ulusal siber Güvenlik Merkezi bulut Güvenlik İlkeleri'ne genel bakış


> [!NOTE]
> Bu güvenlik ilkelerine UK Ulusal siber Güvenlik Merkezi (NCSC tarafından) tanımlanır. Başvurmak [NCSC belgelerine](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) yordamları ve yönergeler için her güvenlik ilkesini test etme hakkında bilgi için.



## <a name="ncsc-cloud-security-principle-1"></a>NCSC bulut güvenlik ilkesini 1
### <a name="data-in-transit-protection"></a>Veri aktarım koruma
Kullanıcı verilerini ağları transiting yeterli izinsiz ve gizli dinleme karşı korunmalıdır.

Bu, bir birleşimi elde:

- Ağ koruması - verilere müdahale yeteneği, saldırganın engelleme
- verileri okuma özelliği, saldırganın reddetme şifreleme-


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Bu şeması kaynakları yalnızca güvenli protokolleri kullanarak iletişim kuracak şekilde yapılandırır. Uygulama ağ geçidi WAF bileşeninin iletişim ekibi dış kullanır gelen HTTPS/TLS kabul etmek ve arka uç havuzu ile yalnızca HTTPS/TLS iletişim kurmak için yapılandırılır. Uzak Masaüstü Hizmetleri, güvenli bağlantı kullanacak şekilde yapılandırılır. VPN AppGateway ve Azure arasında web trafiğinin güvenliğini sağlamak için kullanılır. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure endüstri standardı Aktarım Katmanı Güvenliği (TLS) 1.2 protokolünü kullanır 2048 bit RSA/SHA256 şifreleme anahtarları ile CESG/NCSC tarafından önerildiği hem müşteri ve bulut arası ve dahili olarak Azure sistemleri arasındaki iletişimi şifrelemek için ve veri merkezleri. Örneğin, Yöneticiler, kuruluşlarının hizmetini yönetmek için Microsoft Azure Portalı'nı kullandığınızda, portal ve yöneticinin aygıt arasında aktarılan veriler şifreli TLS kanal gönderilir. Standart web tarayıcısı kullanarak Outlook.com bir e-posta kullanıcı bağlandığında, HTTPS bağlantısı alırken ve e-posta göndermek için güvenli bir kanal sağlar.<br /> <br /> Azure, müşterilerinin kendi veri ve trafiğinin güvenliğini sağlamaya yönelik seçenekler sunmaktadır. Azure'da yerleşik olarak bulunan sertifika yönetimi özelliklerinden Yöneticiler sertifikaları ve şifreleme anahtarları yönetim sistemleri, tek tek Hizmetleri, güvenli Kabuk (SSH) oturumları, sanal özel ağ (VPN) bağlantıları için yapılandırmaya yönelik esneklik, Uzak Masaüstü (RDP) bağlantıları ve diğer işlevleri. <br /><br /> Geliştiriciler dijital doğrulama gibi görevleri işlemek için güvenli karma algoritması (SHA-2) işlevselliği yanı sıra Gelişmiş Şifreleme Standardı (AES) algoritmaları erişmek için Microsoft .NET Framework'e yerleşik şifreleme hizmeti sağlayıcıları (CSP'ler) kullanabilirsiniz imzalar. Azure anahtar kasası, müşterilerin donanım güvenlik modülleri (HSM'ler) depolayarak şifreleme anahtarları ve gizli anahtarları korumaya yardımcı olur. |


 ## <a name="ncsc-cloud-security-principle-2"></a>NCSC bulut güvenlik ilkesini 2
### <a name="asset-protection-and-resilience"></a>Varlık koruma ve esnekliği
Kullanıcı verileri ve depolanması ya da, işleme varlıklar fiziksel kurcalanmaya karşı kaybı, hasar ya da alma korunmalıdır.

Dikkate alınması gereken yönleri şunlardır:

1. Fiziksel konumu ve yasal dairesi
2. Veri Merkezi Güvenliği
3. Rest koruma verileri
4. Veri temizleme
5. Donanımı elden
6. Fiziksel esnekliği ve kullanılabilirliği


 ## <a name="ncsc-cloud-security-principle-21"></a>NCSC bulut güvenlik ilkesini 2.1
### <a name="physical-location-and-legal-jurisdiction"></a>Fiziksel konumu ve yasal dairesi
Yasal koşullar altında verilerinizi sizin izniniz olmadan erişilebilir olduğunu anlamak için onu depolandığı, işlenen yönetilen ve konumları tanımlamanız gerekir.
Ayrıca hizmet içinde nasıl veri işleme denetimleri anlamanıza gerek zorlandığında, UK Yasama göreli olur. Kullanıcı verilerinin uygunsuz koruma hukuk ve Mevzuat sanction veya reputational zarar görmesine neden olabilir.


**Sorumlulukları:**`Customer`

> [!NOTE]
> Azure Hizmetleri bölgesel dağıtılır ve müşteriler yalnızca tek bir bölgede müşteri verilerini depolamak için belirli Azure Hizmetleri yapılandırabilirsiniz. Microsoft Azure kullanılabilirliği ve güvenilirliği genel bir ölçekte sağlamak için genel olarak kullanılabilir veri merkezlerinin listesini sağlar. Tüm Azure veri merkezleri, ISO/IEC 27001: 2013 karşı onaylanmış. Birleşik Krallık coğrafi 2 bölgeden oluşur: Birleşik Krallık Güney ve Birleşik Krallık Batı.

|||
|---|---|
| **Müşteri** | Bu şeması Azure kaynaklarına dağıtmak hangi bölgede Yöneticisi ister. Birleşik Krallık Güney veya Birleşik Krallık Batı dağıtımı için önerilen bölgeler markalarıdır. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Uygulanamaz |


 ## <a name="ncsc-cloud-security-principle-22"></a>NCSC bulut güvenlik ilkesini 2.2
### <a name="datacenter-security"></a>Veri Merkezi Güvenliği
Bulut hizmetleri sağlamak için kullanılan konumlardan fiziksel korunma yetkisiz erişim, oynama, hırsızlığı veya sistemleri yeniden yapılandırılması gerekir. Yetersiz korumaları açığa çıkması, değişikliğinin veya veri kaybına neden olabilir.


**Sorumlulukları:**`Microsoft Azure`


|||
|---|---|
| **Müşteri** | Müşteriler, Azure veri merkezlerinde tüm sistem kaynakları için fiziksel erişimi yoktur; Veri Merkezi güvenlik koruma önlemleri uygulanan ve Microsoft Azure tarafından yönetilir. Bu ilke, Microsoft Azure kaynağından devralındı. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure bu ilkeye müşteriler adına uygular. Microsoft Azure coğrafi olarak dağıtılmış Microsoft tesislerde yer alan ve yardımcı programlar diğer Microsoft online services ile paylaşımı çalışır. Her tesis 24 x 7 x 365 çalışmak üzere tasarlanmıştır ve işlemleri güç kesintisi, fiziksel yetkisiz erişim ve ağ kesintilerine karşı korumaya yardımcı olmak için çeşitli endüstri standardı önlemleri. Bu veri merkezlerinde fiziksel güvenlik ve kullanılabilirlik için endüstri standartları (örneğin, ISO 27001) uyumlu. Bunlar yönetilen, izlenen ve Microsoft operations personeli tarafından yönetilir. <br /> <br /> Azure müşterilerine fiziksel güvenlik denetimleri hiç Azure veri merkezleri ISO/IEC 27001: 2013 standart tüm veri merkezleri, sertifikaları tutan Azure nedeniyle yerinde olduğundan emin olabilir. Birleşik Krallık coğrafi iki bölgeden oluşur: Birleşik Krallık Güney ve Birleşik Krallık Batı. |


 ## <a name="ncsc-cloud-security-principle-23"></a>NCSC bulut güvenlik ilkesini 2.3
### <a name="data-at-rest-protection"></a>Rest koruma verileri
Verileri yetkisiz taraflara altyapı fiziksel erişimi olan kullanılamadığından emin olmak için hizmet içinde tutulan kullanıcı verileri tutulan depolama medyası bağımsız olarak korunmalıdır. Yerinde uygun önlemleri veri yanlışlıkla atılan, kaybolan veya çalınan medyada açık olabilir.


**Sorumlulukları:**`Shared`

> [!NOTE]
> Microsoft Azure müşterilerine kolayca sağlayan tüm şifreleme çözümleri, şifreleme anahtarlarını kolay yönetim için sağlayan Azure anahtar kasası ile tümleştirin.

|||
|---|---|
| **Müşteri** | Gizliliği ve bütünlük bu şeması çözümü tarafından dağıtılmış tüm blob storage'nın Azure depolama hizmeti şifreleme (SSE) kullanarak korunur. SSE 256 bit AES şifreleme kullanarak Azure depolama hesapları içinde kalan verileri koruma sağlar. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure çok çeşitli müşteriler kendi gereksinimlerine en uygun çözümü seçme esnekliğine vermiş şifreleme yetenekleri sunar. Azure anahtar kasası müşterileri kolayca yardımcı olur ve maliyet etkili bir şekilde verilerini şifrelemek için bulut uygulamalar ve hizmetler tarafından kullanılan anahtarların denetimi korumak. Sanal makineler şifrelenecek müşterilerin Azure Disk şifrelemesi sağlar. Azure depolama hizmeti şifrelemesi, bir müşterinin depolama hesabı yerleştirilen tüm verileri şifrelemek mümkün kılar. |


 ## <a name="ncsc-cloud-security-principle-24"></a>NCSC bulut güvenlik ilkesini 2.4
### <a name="data-sanitization"></a>Veri temizleme
Hazırlama, geçirme ve XML'deki sağlama kaynakları sürecini yetkisiz kullanıcı veri için neden değil.

Veri yetersiz temizleme neden olabilir:

- Hizmet sağlayıcısı tarafından süresiz olarak saklanmakta kullanıcı verileri
- Kaynakları yeniden gibi diğer hizmet kullanıcıları için erişilebilir olan kullanıcı verileri
- Kayıp veya atılan, kaybolan veya çalınan medyada duyurulmuş kullanıcı verileri.


**Sorumlulukları:**`Microsoft Azure`


|||
|---|---|
| **Müşteri** | Microsoft Azure kalıcı veri silme Azure ile ilgili sözleşme güvence sağlar. Bu nedenle, bu ilkeyi Microsoft Azure kaynağından devralındı. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure NIST SP800-88r1 elden işlemi FIPS 199 Orta hizalı veri sınıflandırması ile izler. NIST destekleyen sürücüler için güvenli silme yaklaşım (aracılığıyla sabit sürücü bellenimini) sağlar. Microsoft silinemez sabit sürücüler için bunları yok eder ve kurtarma bilgilerinin imkansız oluşturur (örn., disintegrate parçalara ayır, pulverize veya incinerate). Elden uygun çeşit varlık türüne göre belirlenir. Yok etme kayıtları korunur. Tüm Microsoft Azure hizmetlerini onaylanan medya depolama ve elden Yönetim Hizmetleri kullanan. <br />  <br /> Sona erme ya da hizmet aboneliği sonlandırılması müşteri Microsoft ile iletişime geçin ve söyleyin kullanılıp kullanılmayacağını: <br /><br /> (1) müşterinin hesabına devre dışı bırakın ve sonra müşteri verilerini silin; veya <br /> (2) çevrimiçi olarak depolanan verileri tutmak en az 90 gün sonra sona erme ya da müşterinin aboneliğini ("Bekletme dönemi") sonlandırılması için sınırlı işlevi hesabındaki bir hizmet için o müşteri verileri ayıklamak. Saklama dönemi sona erme Microsoft Müşteri'nin hesabı devre dışı bırakın ve tüm verileri silin. Önbelleğe alınacak veya yedek kopyaları saklama süresi son 30 gün içinde temizlenecek. |


 ## <a name="ncsc-cloud-security-principle-25"></a>NCSC bulut güvenlik ilkesini 2,5
### <a name="equipment-disposal"></a>Donanımı elden
Bir hizmet sunmak için kullanılan donanım kullanım ömrünün sonuna ulaştığında, bu, hizmet veya hizmet içinde depolanan kullanıcı verilerini güvenliğini tehlikeye olmayan bir şekilde atılmalıdır.


**Sorumlulukları:**`Microsoft Azure`


|||
|---|---|
| **Müşteri** | Müşteriler, Azure veri merkezlerinde tüm sistem kaynakları için fiziksel erişimi yoktur; donanımı elden yordamları uygulanan ve Microsoft Azure tarafından yönetilir. Bu ilke, Microsoft Azure kaynağından devralındı. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure bu ilkeye müşteriler adına uygular. Sistemin son yaşam, yordamlar ve müşteri verilerini içerebilir hiçbir donanım olmalarını sağlamak için donanımı elden işlemleri işleme ayrıntılı veri işletimsel personel izleyin Microsoft Güvenilmeyen taraflar için kullanılabilir hale getirilir. Microsoft Azure NIST SP800-88r1 elden işlemi FIPS 199 Orta hizalı veri sınıflandırması ile izler. NIST destekleyen sürücüler için güvenli silme yaklaşım (aracılığıyla sabit sürücü bellenimini) sağlar. Microsoft silinemez sabit sürücüler için bunları yok eder ve kurtarma bilgilerinin imkansız oluşturur (örn., disintegrate parçalara ayır, pulverize veya incinerate). Elden uygun çeşit varlık türüne göre belirlenir. Yok etme kayıtları korunur. Tüm Microsoft Azure hizmetlerini onaylanan medya depolama ve elden Yönetim Hizmetleri kullanan. |


 ## <a name="ncsc-cloud-security-principle-26"></a>NCSC bulut güvenlik ilkesini 2.6
### <a name="physical-resilience-and-availability"></a>Fiziksel esnekliği ve kullanılabilirliği
Hizmetleri hataları, olayları veya saldırıları durumunda normal şekilde çalışmaya yeteneklerini etkiler esnekliği değişen düzeylerde sahiptir. Bir hizmeti kullanılabilirlik olmadan bağımsız olarak işinizi üzerindeki etkiyi uzun süren süreyle potansiyel olarak, kullanılamayabilir.


**Sorumlulukları:**`Shared`

> [!NOTE]
> Müşteri Azure Site Recovery ve coğrafi grafik bulunduğu başka bir veri merkezinde verilerin alternatif depolama etkinleştirerek Microsoft Azure uygun şekilde yapılandırır. varsa, Microsoft Azure müşteri tarafından dağıtılan kaynakların sürekli işlemi destekler.

|||
|---|---|
| **Müşteri** | Müşteri, bir alternatif depolama site kurmak için sorumludur. Müşteri denetim uygulama deyimi bir olay durumunda çalışması için Müşteri'nin özelliği giderilmelidir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure farklı coğrafi konumlarda (Birleşik Krallık Güney ve Birleşik Krallık Batı) esnekliği ve kullanılabilirlik sağlamak amacıyla UK Ulusal siber Güvenlik Merkezi (onaylanmış NCSC) veri merkezleri vardır. Yedek kapasite Azure Site Recovery hizmetini kullanarak başka bir bölgede için Müşteri'nin sorumluluğu olacaktır. Azure Site Recovery yapılandırdıktan sonra Azure başlatın ve sorunsuz bir geçiş aşamasında alternatif işleme site müşterinin hizmetlerini durdurun. |


 ## <a name="ncsc-cloud-security-principle-3"></a>NCSC bulut güvenlik ilkesini 3
### <a name="separation-between-users"></a>Kullanıcılar arasında ayrım
Hizmetinin kötü amaçlı ya da güvenliği aşılmış bir kullanıcı hizmet veya başka bir veri etkileyen olmamalıdır.

Kullanıcı ayrımı etkileyen faktörler şunlardır:

- Burada ayrımı denetimleri uygulanan - bu hizmet modeli (örneğin Iaas, PaaS, SaaS) yoğun bir şekilde etkilenir
- kimin hizmetiyle paylaşımı - bu dağıtım modeli (örn. genel, özel veya topluluk Bulutu) tarafından dikte edilir
- ayırma denetimleri uygulamasında kullanılabilir güvence düzeyini

> [!NOTE]
> Bir Iaas hizmetini işlem, depolama ve ağ bileşenleri tarafından sağlanan ayrımını göz önünde bulundurmalısınız. Ayrıca, Iaas kurulmuş SaaS ve PaaS hizmetler Iaas altyapının ayrımı özelliklerini devralır.

**Sorumlulukları:**`Microsoft Azure`


|||
|---|---|
| **Müşteri** | Microsoft Azure hizmet veya başka bir veri etkileyen bir kötü amaçlı ya da güvenliği aşılmış kullanıcının önlemek her bir kullanıcı için yalıtım sağlar. Bu nedenle, bu ilkeyi Microsoft Azure kaynağından devralındı. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Müşteri bulut sunucuları sanal olması nedeniyle, artık fiziksel ayrımı kip geçerlidir. Microsoft Azure tanımlamak ve çok kiracılı bir ortamda devralınan risklerin sayaç yardımcı olmak için tasarlanmıştır. Veri depolama ve işleme Azure Active Directory kullanarak kullanıcı arasında yinelenmeli mantıksal olarak ve paylaşılan Azure veri merkezlerinde depolanan kullanıcı verilerinin emin olmak için amaçlar özellikle çok kiracılı hizmetler için geliştirilen işlevselliği bir başkası tarafından erişilebilir durumda değil Kuruluş. <br /> <br /> Herhangi bir paylaşılan bulut mimarisi kötü amaçlı ya da güvenliği aşılmış bir kullanıcı hizmet veya başka bir veri etkilemesini önlemek her bir kullanıcı için sağlanan yalıtım esastır. <br /> <br /> Microsoft Kiracı ayrımı hakkında daha fazla bilgi için tam açıklamaya bakın [Azure güvenliği ve uyumluluk şeması - NCSC bulut güvenlik ilkeleri - müşteri sorumlulukları matris](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-4"></a>NCSC bulut güvenlik ilkesi 4
### <a name="governance-framework"></a>İdare Framework
Hizmet sağlayıcısı düzenler ve kendi yönetim hizmeti ve içerdiği bilgileri yönlendiren bir güvenlik idare framework olması gerekir. Bu çerçeve dışında dağıtılan herhangi bir teknik denetimler temelde hatalı.
Bu yordamı etkili idare framework sahip olun personel, fiziksel ve teknik denetimlerle hizmet yaşam süresi ile çalışmaya devam. Ayrıca, hizmetin, teknolojik gelişmeler ve yeni tehditleri görünümünü değişikliklere kullanmalıdır.


**Sorumlulukları:**`Microsoft Azure`


|||
|---|---|
| **Müşteri** | Microsoft Azure, Azure Hizmetleri için belgelenen güvenlik idare framework tutar. Bu nedenle, bu ilkeyi Microsoft Azure kaynağından devralındı. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Hangi hedefleri verilen takım ya da varlık geçerli belirlemenize ve uygulandıkları gibi etki alanı denetimi hedeflerinizi, yeterli ayrıntılarla nasıl ele alınan yakalama uyumluluk etki alanları tanımlamak için standart bir Metodoloji Microsoft Uyumluluk framework içeren bir endüstri standartları, düzenlemeler veya iş gereksinimleri verilen kümesi. Framework, tasarım ve böylece uyumluluk düzenlemeleri aralığı bugün hızlandırma denetimleri, ortak bir dizi kullanarak hizmetleri oluşturmak Microsoft sağlayan birden çok yasal standartlarına denetimleri eşler ve gelecekte geliştikçe. <br /> <br /> Microsoft Uyumluluk işlemleri aynı zamanda müşterilerin uyumluluk birden fazla hizmet elde etmek ve verimli bir şekilde değiştirmeyi gereksinimlerini karşılamak kolaylaştırır. Birlikte, güvenliği artırmaya teknoloji ve etkili uyumluluk işlemleri korumak ve zengin bir üçüncü taraf sertifikaları genişletmek Microsoft etkinleştirin. Bu sertifikalar, müşteriler, denetçiler ve düzenleyiciler uyumluluk hazırlık göstermek müşterilere yardımcı olun. <br /> <br />  Azure, ISO 27001, FedRAMP, SOC 1 ve SOC 2 gibi bölgesel ve sektöre özel uyumluluk standartları yanı sıra uluslararası geniş kümesi ile uyumludur. Bu standartlar bulunan katı güvenlik denetimleri ile uyumluluk Azure hizmetleri çalışmak ve dünya çapındaki endüstri standartları, sertifikaları, attestations ve yetkilerini karşılayan gösteren ayrıntılı üçüncü taraf denetimleri tarafından doğrulanır. <br /> <br /> Azure müşteri adresi iş hedeflerini yanı sıra endüstri standartları ve düzenlemelere yardımcı olan bir uyumluluk stratejisi tasarlanmıştır. Güvenlik uyumluluk framework test ve denetim aşamaları, güvenlik analizi, risk yönetimi en iyi yöntemler ve sertifikaları ve attestations elde etmek için güvenlik Kıyaslama analizi içerir. <br /> <br /> Microsoft'un bağlılığı uyumluluk çerçeveleri ile ilgili daha fazla bilgi için tam açıklamaya bakın [Azure güvenliği ve uyumluluk şeması - NCSC bulut güvenlik ilkeleri - müşteri sorumlulukları matris](https://aka.ms/blueprintuk-gcrm).|


 ## <a name="ncsc-cloud-security-principle-5"></a>NCSC bulut güvenlik ilkesi 5
### <a name="operational-security"></a>İşlem güvenliği
Hizmet işletilen ve dönüşü, algılamak veya saldırılarını önlemek için güvenli bir şekilde yönetilen gerekir. İyi bir işletimsel güvenlik karmaşık, bureaucratic, zaman alabilir veya pahalı işlemleri gerektirmemelidir.

Dikkate alınması gereken dört öğe vardır:

- Yapılandırma ve değişiklik yönetimi - sistem değişiklikleri düzgün yetkili ve sınanmış emin olmanız gerekir. Değişiklikleri beklenmedik biçimde güvenlik özellikleri değiştirip değiştirmeyeceğini değil
- Güvenlik Açığı Yönetimi - tanımlamak ve bağlı bileşenlerinde güvenlik sorunları hafifletmek gerekir
- Koruyucu izleme - saldırıları ve hizmet üzerinde yetkisiz bir etkinliği algılamak yerinde ölçüleri yerleştirileceği
- Olay yönetimi - olaylarına yanıt ve güvenli ve kullanılabilir bir hizmet kurtarmak emin olun




 ## <a name="ncsc-cloud-security-principle-51"></a>NCSC bulut güvenlik ilkesini 5.1
### <a name="configuration-and-change-management"></a>Yapılandırma ve değişiklik Yönetimi
Doğru bir resmini kendi yapılandırmaları ve bağımlılıkları ile birlikte service oluşturan ve varlıkları olması gerekir.
Hizmet güvenliğini etkileyebilecek değişiklikler tanımlanır ve yönetilen. Yetkisiz değişiklikler algılandı.
Güvenlik açıkları, farkında olmadan değişiklik etkili bir şekilde yönetilmediği durumlarda, bir hizmete sunulan. Ve güvenlik açığı tanıma bile söz konusu olduğunda, onu değil tam olarak azaltılması gereken.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Bu şeması oluşturan eşlik eden kaynakları ve Azure Resource Manager şablonları "yapılandırma kod olarak" temel dağıtılmış mimarisi için temsil eder. Çözüm, ancak yapılandırma denetimi için kullanılabilen GitHub sağlanır. <br /> <br /> Azure Active Directory ayrıcalıkları katı sağlama rollere kullanıcıları atayarak rol tabanlı erişim denetimini kullanarak uygulanan hesap denetim hangi kullanıcıların görüntüleyebileceği ve denetim kaynakları dağıtıldı. Active Directory hesap ayrıcalığı kullanıcıları güvenlik gruplarına atayarak rol tabanlı erişim denetimini kullanarak uygulanır. Bu güvenlik grupları kullanıcıların işletim sistemi yapılandırmasına göre gerçekleştirebileceğiniz eylemler denetler. Bu rol tabanlı düzenleri iş gereksinimlerini karşılamak üzere müşteri tarafından genişletilebilir. <br /> <br /> Bu ilke uyumlu olması için daha fazla yapılandırma üretimde kullanım için müşteri tarafından gerekli değildir. Bu nedenle, bu yapılandırmalar, Müşteri'nin değişiklik Yönetimi sürecinin bir parçası olması gerekir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure gözden geçirir ve yapılandırma ayarlarını ve donanım, yazılım ve ağ aygıtlarının temel yapılandırmalar yıllık olarak güncelleştirir. Değişiklikleri geliştirilmiş, test ve geliştirme ve/veya test ortamından üretim ortamına girme önce onaylanan. <br /> <br />Microsoft Azure Microsoft Azure yazılım bileşenleri (örneğin işletim sistemi, doku, RDFE, XStore, vb.) ve önyükleme yapılandırma işlemi girme donanım ve ağ aygıtı bileşenleri için değişiklik ve yayın işlemini kullanarak temel yapılandırmalar uygular Aşağıda özetlendiği gibi Microsoft Azure üretim ortamı. <br /> <br /> Azure tabanlı hizmetler için gerekli temel yapılandırmalar Azure güvenliği ve uyumluluğu ekibi tarafından ve bunların üretim hizmet dağıtımdan önce test bir parçası olarak hizmet takımlara incelenen. |


 ## <a name="ncsc-cloud-security-principle-52"></a>NCSC bulut güvenlik ilkesini 5.2
### <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi
Hizmet sağlayıcıları yönetim işlemleri tanımlamak, Önceliklendirme ve güvenlik açıkları azaltmak için yerinde olması gerekir. Yok, hızlı bir şekilde genel olarak bilinen yöntemlerini ve araçlarını kullanarak saldırılara karşı savunmasız hale gelecek Hizmetleri.


**Sorumlulukları:**`Customer`


|||
|---|---|
| **Müşteri** | Müşteri, güvenlik açığı (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için) müşteri tarafından dağıtılan kaynakları tarama sorumludur. Müşteri uygulama bildirimi güvenlik açığı taramaları gerçekleştirmek için kullanılan araçlar giderilmelidir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Güvenlik güncelleştirmesi yönetim sistemleri bilinen açıklarından korunmasına yardımcı olur. Dağıtım ve yükleme için Microsoft yazılımları için güvenlik güncelleştirmelerini yönetmek için tümleşik dağıtım sistemleri azure kullanır. Azure de, kaynaklar, Microsoft Güvenlik Yanıt Merkezi (tanımlayan izler, yanıt verir ve güvenlik olaylarını çözümler MSRC), çizme ve güvenlik açıkları saati geçici yılın her günü bulut mümkün olur. |


 ## <a name="ncsc-cloud-security-principle-53"></a>NCSC bulut güvenlik ilkesini 5.3
### <a name="protective-monitoring"></a>Koruyucu izleme
Etkin izleme saldırı, kötüye kullanımı ve hatası için bir hizmeti (başarılı ve başarısız) saldırıları algılamak olası olacaktır. Sonuç olarak, ortamları ve verilerinizin olası güvenlik ihlalleri için hızlı yanıt kuramayacaktır.


**Sorumlulukları:**`Customer`


|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için) izlemekten sorumludur. Müşteri denetim uygulama bildirimi izlemek, algılamak ve saldırıları, kötüye kullanımı ve arızası yanıt mekanizmalarını giderilmelidir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure güvenlik etkin izleme gereksinimleri tanımlanır. Hizmet ekipleri bu gereksinimlere uygun etkin izleme araçları yapılandırın. Etkin izleme araçları Acil eylem gerektiren durumlarda Microsoft Azure güvenliği personeli için sağlamak zaman uyarılar için yapılandırılan İzleme Aracısı (MA) ve System Center Operations Manager (SCOM) içerir. |


 ## <a name="ncsc-cloud-security-principle-54"></a>NCSC bulut güvenlik ilkesini 5.4
### <a name="incident-management"></a>Olay yönetimi
Dikkatle önceden planlanan Olay yönetimi işlemleri yerinde olmadığı sürece, zayıf kararları olaylar potansiyel olarak genel kullanıcılar üzerindeki etkiyi exacerbating ortaya çıktığında yapılması olasılığı yüksektir.
Bu işlemler gerekmez veya karmaşık açıklama büyük miktarlarda gerektirir, ancak iyi Olay yönetimi, güvenlik, güvenilirlik ve bir hizmet ile ortam sorunlarını kullanıcılara etkisini en aza indirmiş olursunuz.


**Sorumlulukları:**`Shared`

> [!NOTE]
> Müşteri güvenlik olaylarına Microsoft Azure güvenlik durumunu burada etkileyebilecek durumlarda, Microsoft Azure bilgilendirmek için müşteri sorumludur.

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil olmak üzere) için bir olay Yönetimi işlemini kurmak için sorumludur. Müşteri uygulama bildirimini raporlama olayları ve zamanında olay yanıtları destekleme ve PGA ve uygun şekilde diğer HMG kuruluşlar için bilgileri iletme uyarıları giderilmelidir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft bir gerçekleşeceğini olaylar için eş güdümlü yanıtlar kolaylaştırmak için bir güvenlik olay yönetimi süreci uyguladı. <br /> <br /> Microsoft, donanım veya kendi tesis ya da böyle bir donanım veya kayıp, açığa çıkması veya Müşteri verilerinin değişikliğinin kaynaklanan tesis yetkisiz erişim depolanan tüm müşteri verilerinin yetkisiz erişim farkında olursa, Microsoft, belirtilen içinde: <br /> <br />   -Derhal müşteri ve güvenlik olay bildirim; <br /> -Derhal güvenlik olayı araştırın ve güvenlik olay hakkındaki ayrıntılı bilgilerle birlikte müşteri sağlar; ve <br /> -Etkilerini azaltmak ve güvenlik olay kaynaklanan bir zarar en aza indirmek için makul ve komut istemi adımları uygulayın.  <br />  <br /> Bir olay yönetim çerçevesi tanımlanmış roller ve Sorumluluklar ayrılan ile kuruldu. Windows Azure güvenlik olay Yönetimi (WASIM) ekibin güvenlik olayları yönetme, yükseltme de dahil olmak üzere ve uzmanı takımlar gerektiğinde katılımı sağlama sorumludur. Azure operasyon yöneticileri, araştırma ve diğer işlevleri desteğiyle güvenlik ve gizlilik olayların çözümlenmesi gözlemledikten sorumludur. <br /> <br /> Microsoft'un olay yanıtı işlemleri ile ilgili daha fazla bilgi için tam açıklamaya bakın [Azure güvenliği ve uyumluluk şeması - NCSC bulut güvenlik ilkeleri - müşteri sorumlulukları matris](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-6"></a>NCSC bulut güvenlik ilkesi 6
### <a name="personnel-security"></a>Personel Güvenliği
Servis Sağlayıcı personeli sistemleri ve veri erişimi nerede yüksek ölçüde kendi güvenilirliği güvenmesi gerekir. Kapsamlı filtreleme desteklenen yeterli eğitim tarafından hizmet sağlayıcısı personeli tarafından yanlışlıkla veya kötü amaçlı güvenliğinin aşılması olasılığını azaltır.
Hizmet sağlayıcısı personel güvenlik filtreleme ve normal güvenlik eğitimi konu. Bu rollerde personel, sorumluluklarını anlamanız gerekir. Sağlayıcıları nasıl ekran ve ayrıcalıklı rolleri içinde personel yönetmek Temizle olmanız gerekir.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Müşteri, kişiler filtreleme ve normal güvenlik eğitimi kişiler için müşteri tarafından dağıtılan kaynaklara erişimi sağlama sorumludur. Müşteri uygulaması deyimi rolleri ve güvenlik eğitimi sıklığını filtreleme ölçütlerini giderilmelidir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure Hizmetleri çalışır ve müşteri desteği (veya platform işlemleri, sorun giderme ve teknik destek ile yardımcı olan Microsoft Yükleniciler) sağlayan Microsoft Azure personeli uygulanabilecek bir Microsoft Standart arka plan (veya eşdeğeri) denetleyin çalışan eğitim, çalışma ve cezai geçmişi değerlendirin. Arka plan denetimleri kapsamlı UK kamu'nın BPSS/BS7858 gereksinimlerine uygun olarak verilmiştir. Bunlar özellikle resmi kimlik denetimi dahil etmeyin.  <br /> <br /> Microsoft gizlilik hükümleri, çalışan ve alt yüklenici sözleşmeleri içerir. Tüm uygun Microsoft çalışanlarına ve alt yüklenicilerimiz bölümü, sorumluluklarını için bilgi güvenliği personeli bildiren bir Microsoft Azure sponsorlu güvenlik eğitim programı alın. <br /> <br /> Microsoft Azure Hizmetleri personeli ve güvenlik ihlallerini yapılıyor ve/veya bilgi güvenlik ilkesini ihlal, şüpheli alt yüklenicilerimiz araştırma işlemi ve uygun Disiplin eylemi kadar ve da dahil olmak üzere sonlandırma tabi olan. Koşullar bunu garanti, Microsoft konuyu yasaları zorlama Teşkilatı tarafından için Takipte başvurabilir. <br /> <br /> Bu sistem arka plan denetim ve güvenlik eğitimi tamamlamak için Microsoft yetkisiz Geliştirici ve/veya yönetim etkinliği, aşağıdakiler de dahil olmak üzere karşı korunmasına yardımcı olmak için önleyici, savunma ve geriye dönük denetimleri birleşimlerini dağıtır. düzenekleri: <br />  <br /> -Sıkı erişim denetimleri hassas işlemlerini gerçekleştirmek iki öğeli akıllı kart tabanlı kimlik doğrulama gereksinimini gibi hassas verileri. <br /> -Kötü amaçlı etkinliğin bağımsız algılamasını iyileştirmek denetimleri birleşimlerini. <br /> -İzleme, günlüğe kaydetme ve raporlama birden çok düzeyi. |


 ## <a name="ncsc-cloud-security-principle-7"></a>NCSC bulut güvenlik ilkesi 7
### <a name="secure-development"></a>Güvenli geliştirme
Hizmetleri tasarlanmış ve tanımlamak ve bunların güvenlik tehditleri azaltmak için geliştirilmiştir. Hangi olmayan olanlar verilerinizi tehlikeye, hizmet kaybına neden veya diğer kötü amaçlı etkinliği etkinleştir güvenlik sorunları için açık halde olabilir.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Windows, gerçek zamanlı dosya bütünlüğünü doğrulama, koruma ve kurtarma, Windows veya gerçek zamanlı sağlayan Windows Kaynak Koruması (WRP) özelliği, yetkili Windows Sistem güncelleştirmelerde parçası olarak yüklenen çekirdek sistem dosyalarının sağlar tutarlılığı denetleniyor. <br /> <br /> Windows kod yürütmeyi kısıtlı bellek konumlarda engellemek için yerinde korumaları sahiptir: yürütme yok (NX), adres alanı düzeni rastgele seçimini (ASLR) ve Veri Yürütme Engellemesi (DEP). <br /> <br /> Bu şeması tüm dağıtılan Windows sanal Microsoft Windows Defender'ı kullanılarak uygulanan makineler için kötü amaçlı yazılımdan koruma ana bilgisayar tabanlı korumalar dağıtır. Windows Defender, kötü amaçlı yazılımdan koruma altyapısı ve koruma imzaları kullanılabilir hale yayın otomatik olarak güncelleştirmek için yapılandırılır. <br /> <br /> Bu ilke uyumlu olması için daha fazla yapılandırma üretimde kullanım için müşteri tarafından gerekli değildir. Bu nedenle, bu yapılandırmalar, Müşteri'nin güvenli geliştirme sürecinin bir parçası olması gerekir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Security Development Lifecycle (SDL) Tehditler ve yazılım ve hizmetlerinin güvenlik açıklarını tanımlamak için etkili bir tehdit modelleme işlemi sunar. Tehdit modelleme operations manager, program/proje yöneticileri, geliştiriciler ve sınayıcılar, bir takım, bir uygulamadır ve bir anahtar güvenlik analiz görevin gerçekleştirilmesi için çözüm tasarımı temsil eder. Takım üyeleri, tüm hizmet ve projeler, oluşturulduklarında hem yeni özellikler ve işlevsellik ile güncelleştirildiğinde modellemek için SDL tehdit modelleme Aracı'nı kullanır. Tehdit modelleri saldırı yüzeyini ve tarafından yazılan veya bir üçüncü taraf lisanslı tüm kod kullanıma sunulan tüm kod kapsar ve tüm güven sınırları göz önünde bulundurun. STRIDE sistem (sahtekarlık, kurcalama, ret, bilgi açıklama, hizmet reddi ve ayrıcalıkların) tanımlamak ve müşteriler etkileyebilir önce güvenlik tehditlerini erken tasarım işleminde çözmeye yardımcı olmak için kullanılır. |


 ## <a name="ncsc-cloud-security-principle-8"></a>NCSC bulut güvenlik ilkesi 8
### <a name="supply-chain-security"></a>Tedarik zinciri güvenlik
Hizmet sağlayıcısı tedarik zinciri başarılı şekilde tüm uygulamak için hizmet talepleri güvenlik ilkelerinin desteklediğini emin olmalısınız.
Bulut Hizmetleri genellikle üçüncü taraf ürünleri ve Hizmetleri kullanır. Sonuç olarak, bu ilkeye uygulanmaz, tedarik zinciri güvenliğinin aşılmasına hizmet güvenlik olumsuz ve diğer güvenlik ilkelerinin uygulanması etkiler.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Müşteri, herhangi bir üçüncü taraf yazılım ve işletim sistemleri Azure aboneliğini kullanılan alım için güvenli tedarik zinciri belgelerine sağlamaktan sorumludur. Müşteri uygulaması deyimi bu tedarik zinciri belge tarafından tanımlanan işlemleri izlemek için özel durumla. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft bulut tedarik zinciri (MCSC) grubu, her Azure tedarik zinciri için tehditlere karşı korumaya katkıda bulunan altı benzersiz ekiplerin oluşur.  <br />  <br /> -Satın alma <br /> -Müşteri işlemleri <br /> -Dağıtım kalitesi <br /> -Tedarikçi İlişkileri Yönetimi <br /> -Yedek <br />  <br /> Microsoft'un MCSC grubu ile ilgili daha fazla bilgi için tam açıklamaya bakın [Azure güvenliği ve uyumluluk şeması - NCSC bulut güvenlik ilkeleri - müşteri sorumlulukları matris](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-9"></a>NCSC bulut güvenlik ilkesi 9
### <a name="secure-user-management"></a>Kullanıcı Yönetimi güvenli
Sağlayıcınız araçları, kendi hizmet kullanımınız güvenli bir şekilde yönetmek kullanılabilir olmanız gerekir. Yönetim arabirimleri ve yordamları yetkisiz erişime ve kaynakları, uygulamaları ve verileri değişikliğinin engelleyen güvenlik engeli önemli bir parçasıdır.

Dikkate alınması gereken yönleri şunlardır:

- Kullanıcılara yönetim arabirimleri ve Destek kanallarını kimlik doğrulaması
- Yönetim arabirimleri içinde ayırma ve erişim denetimi



 ## <a name="ncsc-cloud-security-principle-91"></a>NCSC bulut güvenlik ilkesini 9.1
### <a name="authentication-of-users-to-management-interfaces-and-within-support-channels"></a>Kullanıcıların yönetim arabirimleri ve Destek kanallarını içinde kimlik doğrulaması
Kullanıcılar Yönetim etkinlikleri gerçekleştirmesine izin verilmeden önce düzgün bir şekilde doğrulanmasını gerek güvenli bir hizmeti sürdürmek için rapor arıza ya da istek hizmete değiştirir.
Bu etkinlikler bir hizmet Yönetim web portalı üzerinden veya telefonla veya e-posta gibi diğer Kanallar üzerinden gerçekleştirilmesi. Bunlar, yeni hizmet öğeleri sağlama, kullanıcı hesaplarını yönetme ve kullanıcı verilerini yönetme gibi işlevler olasılığı yüksektir.
Hizmet sağlayıcıları güvenlik etkisi olan tüm yönetim isteği güvenli ve kimliği doğrulanmış kanalları gerçekleştirildiğinden emin olmak gerekir. Kullanıcıları güçlü kimlik doğrulaması, ardından bir imzalandığını hizmet ya da veri güvenlik zayıflatabilir başarıyla ayrıcalıklı eylemler gerçekleştirmek mümkün olabilir.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Yöneticiler kuruluşlarında Azure kaynaklarınızı yönetmek için Microsoft Azure portalına eriştiğinde, 2048 bit RSA kullanarak bir şifrelenmiş Aktarım Katmanı Güvenliği (TLS) kanalı üzerinden gönderilen portalı ve Yönetici'nin aygıt arasında aktarılan veri / CESG/NCSC tarafından önerildiği gibi SHA256 şifreleme anahtarları.  <br /> <br /> Bu şeması Windows kimlik doğrulaması, Uzak Masaüstü'nü ve BitLocker'ı kullanır. Bu bileşenler, FIPS 140 doğrulanmış şifreleme modüllerinde yararlanmayı yapılandırılabilir. <br /> <br /> Roller, kullanıcılar güvenlik gruplarına atayarak Active Directory Kullanıcıları atayarak Azure Active Directory tarafından zorlanan rol tabanlı erişim denetimini kullanarak mantıksal erişimi yetkilerini bu şeması zorlar ve Windows işletim sistemi düzeyinde denetler. Azure Active Directory rolleri kullanıcılara atanan veya grupları mantıksal kaynak, Grup veya abonelik düzeyinde Azure içindeki kaynaklara erişimi denetler. Active Directory güvenlik grupları, işletim sistemi düzeyinde kaynakları ve işlevleri mantıksal erişimi denetler. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Müşteriler, tüm sanal makineler, veritabanları, bulut Hizmetleri için erişim sağlayan Azure Portalı aracılığıyla Azure kaynaklarını ve Müşteri'nin hesabı için yapılandırılmış diğer kaynakları yönetme. Azure portalına Web erişimi, endüstri standardı Aktarım Katmanı Güvenliği (TLS) 1.2 bağlantıları CESG/NCSC tarafından önerilen olarak 2048 bit RSA/SHA256 şifreleme anahtarları kullanılarak güvenli hale getirilir. Rol tabanlı erişim denetimlerini, müşterilerin belirli kullanıcılar ve gruplar için Azure yönetim kaynaklara sınırlı erişim sağlamak için etkinleştirmek için sağlanmıştır. |


 ## <a name="ncsc-cloud-security-principle-92"></a>NCSC bulut güvenlik ilkesini 9.2
### <a name="separation-and-access-control-within-management-interfaces"></a>Ayırma ve erişim denetimi yönetim arabirimleri içinde
Birçok bulut Hizmetleri, web uygulamaları veya API'ler yönetilir. Bu arabirimleri hizmetin güvenlik önemli bir parçasıdır. Kullanıcıların yeterli yönetim arabirimleri içinde ayrılmış değil ise, bir kullanıcı hizmetini etkileyen veya başka bir veri değiştirmek mümkün olabilir.
Ayrıcalıklı yönetici hesaplarınızı muhtemelen büyük miktarda veriyi erişiminiz vardır. Bu kesinlikle gerekli bireysel kullanıcılara izinler sınırlama kötü niyetli kullanıcılar tarafından kimlik bilgilerinin gizliliğinin tehlikeye veya güvenliği aşılmış cihazlara neden hasarı sınırlayabilirsiniz yardımcı olabilir.
Rol tabanlı erişim denetimi, bunu başarmak için bir mekanizma sağlar ve daha büyük dağıtımlarda yöneten kullanıcılar için özellikle önemlidir bir özellik olması olasıdır.
Bunlar ilk olarak bu ağlardan birine erişmek ihtiyacınız olacak yönetim arabirimleri (örneğin topluluk yerine ortak ağlara) az erişilebilir ağlara gösterme, saldırganlar erişmek ve onları, saldırı daha zorlaştırır. Farklı türlerde ağ arabirimlerine gösterme riskleri değerlendirme kılavuzu İlkesi 11 altında sağlanır.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir uygulama ağ geçidi dağıtır, yük dengeleyici ve commutations dış sınırlarında ve iç alt ağlar arasında denetlemek için ağ güvenlik grubu kuralları yapılandırır. Kullanıcı işlevselliği, sistem yönetimi işlevselliğini zorlama mantıksal erişim denetimleri ve sistem mimarisi ile gelen ayrılır. Sistem Yönetimi işlevleri için arabirimleri kullanıcı arabirimlerinden ayrıdır. Üretim kaynakları uygun şekilde erişimi sınırlamak için ağ güvenlik grubu kuralları ile bir yönetim alt ağda bulunan bir güvenli savunma konak (jumpbox) aracılığıyla tüm yönetim bağlantısı durumda. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Belirtilen kullanıcının ayrımı arasında ayrım özünde Azure'da oluşturulmuştur. Azure Active Directory (Azure AD veya AAD) Azure portalında bunlar görmeyi ve yönetmeyi yetkilendirildiği kaynaklara erişimi kimlik doğrulamasını her kullanıcı sağlamak için kullanılabilir. Sonuç olarak, farklı müşteri hesapları kesinlikle birbirinden ortak Azure portalı üzerinden yönetildiğinde yinelenmeli. |


 ## <a name="ncsc-cloud-security-principle-10"></a>NCSC bulut güvenlik ilkesi 10
### <a name="identity-and-authentication"></a>Kimlik ve kimlik doğrulama
Hizmet Arabirimleri tüm erişimi kimliği doğrulanmış ve yetkilendirilmiş kişilerle sınırlı.
Bu arabirimleri için zayıf bir kimlik doğrulama hırsızlığı veya hizmetinize yapılan değişiklikler, verilerinizin değişiklik ya da bir hizmet reddi kaynaklanan sistemlerinizi yetkisiz erişim sağlayabilir.
Önemlisi, kimlik doğrulaması güvenli kanallar üzerinde gerçekleşmesi gereken. E-posta, HTTP veya telefon kişiler tarafından ele ve sosyal mühendislik saldırılarına karşı savunmasız.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Bu şeması Active Directory hesap yönetimi için kullanır. Bu şeması tarafından dağıtılan kaynaklarına erişimi yeniden yürütme saldırılara karşı yerleşik Kerberos işlevselliğini Azure Active Directory, Active Directory ve Windows işletim sistemi tarafından korunur. Kerberos kimlik doğrulaması, istemci tarafından gönderilen Doğrulayıcı şifrelenmiş IP listesi, istemci zaman damgaları ve bilet yaşam süresi gibi ek veriler içerir. Bir paket tekrarlanır, zaman damgası denetlenir. Zaman damgası daha önceki ise veya önceki bir Doğrulayıcı, paket ile aynı reddedilir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure kimlik yanı sıra zaten kullanımda olabilir kimlik depolarına tümleştirileceğini izlenmesine yardımcı olmak için hizmetleri sağlar. Azure AD şirket içi veri erişimi güvenli hale getirmek ve bulut uygulamalarında yardımcı olan bulut için kapsamlı bir kimlik ve erişim yönetimi hizmeti ' dir. Azure AD Kimlik Yönetimi, güvenlik ve uygulamaya erişim yönetimi Gelişmiş çekirdek Dizin Hizmetleri birleştirerek kullanıcılar ve gruplar yönetimini de basitleştirir. |


 ## <a name="ncsc-cloud-security-principle-11"></a>NCSC bulut güvenlik ilkesi 11
### <a name="external-interface-protection"></a>Dış arabirimi koruma
Tüm dış ya da daha az güvenilir arabirimleri hizmetinin tanımlanır ve uygun şekilde defended.
Açık arabirimler bazıları (Yönetim arabirimleri gibi) özel değilse güvenliğinin aşılmasına etkisini daha önemli olabilir.
Kuruluş sistemlerinizi değişen düzeylerde risk kullanıma bulut hizmetlerine bağlanmak için farklı modelleri kullanabilirsiniz.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir mimari ayrı web alt ağ, veritabanı alt ağ, Active Directory alt ve yönetimi alt ağ kaynaklarında dağıtır. Alt ağları için yalnızca bu gerekli sistem ve yönetim işlevselliği için alt ağlar arasında trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kural tarafından mantıksal olarak ayrılır (örn., dış trafiğin veritabanına, yönetim, erişilemiyor veya Active Directory alt ağlar).  <br /> <br /> Bir uygulama ağ geçidi müşteri tarafından dağıtılan web uygulamasının dış bağlantıları yönetmek için dağıtılır. Dış bağlantıları yönetim erişimi için yetkilendirilmiş IP adreslerine dış bağlantılar kısıtlamak amacıyla uygulanan ağ güvenlik kuralları ile yönetimi alt ağda dağıtılan bir jumpbox (savunma ana bilgisayarı) kısıtlanır. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure güvenlik denetimleri ve normal sızma testi yoluyla süreçlerini geliştirmek için "Red gruplama" çağıran bir yöntem kullanır. Kırmızı takım bir tam zamanlı personel içinden, hedeflenen gerçekleştirme odaklanır Microsoft ve Microsoft altyapı, platformlar ve uygulamalar, ancak son-müşterilerin uygulamaları veya veri kalıcı saldırıları grubudur. <br /> <br /> Microsoft'un kırmızı ekibi oluşturma ile ilgili daha fazla bilgi yanı sıra mavi gruplama çaba açıklamasını, tam açıklamaya bakın [Azure güvenliği ve uyumluluk şeması - NCSC bulut güvenlik ilkeleri - müşteri sorumlulukları Matrisi ](https://aka.ms/blueprintuk-gcrm).  |


 ## <a name="ncsc-cloud-security-principle-12"></a>NCSC bulut güvenlik ilkesi 12
### <a name="secure-service-administration"></a>Güvenli Hizmet Yönetimi
Bir bulut Hizmeti Yönetimi için kullanılan sistemler yüksek oranda hizmet ayrıcalıklı erişimi olan. Kendi güvenliğinin aşılmasına güvenlik denetimlerini atlama ve çalma veya büyük miktarda veriyi işlemek için araçlar da dahil olmak üzere, önemli bir etkisi yoktur.
Tasarım ve Uygulama Yönetimi yönetim sistemlerinin Kurumsal iyi bir uygulama, yüksek değerlerine saldırganlar algılamayı adımında izlemelisiniz.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Müşteri, kendi Azure aboneliği ve müşteri tarafından dağıtılan kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için), Yönetim için güvenli bir iş istasyonu sağlamak için sorumludur. Müşteri uygulaması deyimi müşteri tarafından dağıtılan kaynakları yararlanılması riskini azaltmak için kullanılan mekanizmaları giderilmelidir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure işlemler personel, güvenli yönetim iş istasyonları kullanmak için gereklidir (kurlar; Ayrıca, ayrıcalıklı erişimli iş istasyonlarının veya Patiler olarak bilinir). TESTERE yaklaşım, yönetim personeli için ayrı yönetici ve kullanıcı hesaplarını kullanmak için tanınmış önerilen uygulama uzantısıdır. Bu yöntem, kullanıcının standart kullanıcı hesabından tamamen ayrı ayrı olarak atanmış bir yönetici hesabı kullanır. Derlemeleri hassas bu hesaplar için güvenilir bir iş istasyonu sağlayarak bu hesabı ayırma uygulama gördünüz. |


 ## <a name="ncsc-cloud-security-principle-13"></a>NCSC bulut güvenlik ilkesi 13
### <a name="audit-information-for-users"></a>Kullanıcılar için denetim bilgileri
Denetim ile hizmetinizi ve veri erişimi izlemek için gereken kayıtlarını içinde tutulan sağlanan olmalıdır. Denetim bilgileri türü bir doğrudan algılamasını ve makul zaman ölçekleri içinde uygun veya kötü amaçlı etkinlik yanıtlamasını yeteneğinizi üzerinde etkisi olacaktır.


**Sorumlulukları:**`Shared`


|||
|---|---|
| **Müşteri** | Bu şeması tarafından denetlenen olayları Azure etkinlik günlükleri dağıtılan kaynakların, işletim sistemi düzeyinde günlükleri ve Active Directory günlükleri tarafından denetlenmesini içerir. Bu olay günlüklerindeki olaylar meydana geldiğinde belirlemek yeterli bilgi, olay kaynağı, olay ve güvenlik olaylarını incelenmesi destekleyen diğer ayrıntılı bilgilerden sonucunu içerir. Müşteriler, iş gereksinimlerini karşılamak için denetlenecek ek olaylar seçebilirsiniz. Tüm Azure kaynaklarına denetim günlüklerini Azure portalında kullanılabilir olması. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure günlük analizi herkes bunlarla değiştirmesine önce ortaya hemen bir kuruluşun sistemleri ve ağları içinde gerçekleşen olayların kayıtları toplar ve analiz farklı türlerde birden fazla bilgisayara veri ilişkilendirilmesi yoluyla sağlar. Azure güvenlik olay oluşturma ile toplama kendi Aboneliklerde merkezi depolama için Azure Iaas ve PaaS rol gerçekleştirmek müşterilerin sağlar. Bu toplanan olaylar, şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemleri devam eden izleme için verilebilir. Veri depolama alanına aktarıldıktan sonra tanılama verilerini görüntülemek için birçok seçeneğiniz vardır. <br /> <br /> Azure yerleşik tanılama hata ayıklamaya yardımcı olabilir. Azure üzerinde dağıtılan uygulamalar için işletim sistemi güvenlik olaylarını bir dizi varsayılan olarak etkinleştirilir. Müşteriler ekleyin, kaldırın veya işletim sistemi denetim ilkesi özelleştirerek denetlenecek olayları değiştirin. <br /> <br /> Yüksek bir düzeyde oldukça kolaydır ve Azure Iaas kullanarak Windows tabanlı VM'ler dağıtıldığında basit günlükleri toplamaya başlamak Windows Olay iletme (WEF) veya daha fazlasını kullanarak Azure tanılama Gelişmiş. Ayrıca, Azure tanılama günlüklerini ve olayları PaaS rol örneklerinizden toplamak üzere yapılandırılabilir. Iaas tabanlı VM'ler kullanırken, bir müşteri yalnızca yapılandırır ve Windows Server'ın denetimleri kendi şirket içi veri merkezine günlüğü aktive ettiğiniz şekilde istenen güvenlik olayları sağlar. Web uygulamaları için de birincil uygulama ve dağıtım azure'da ise, IIS günlüğü etkinleştirmek mümkündür. Müşteriler depolama hesaplarında veri egemenliği gereksinimlerini karşılamak için kendi seçtikleri desteklenen coğrafi konumda bulunan güvenlik verileri her zaman depolayabilirsiniz. |


 ## <a name="ncsc-cloud-security-principle-14"></a>NCSC bulut güvenlik ilkesi 14
### <a name="secure-use-of-the-service"></a>Güvenli ve hizmetin kullanımını
Hizmet kötü kullanırsanız, bulut Hizmetleri ve bunların içinde tutulan verileri güvenliğini hatalı. Sonuç olarak, hizmet verileriniz için sırayla yeterli korunacak kullanırken bazı sorumlulukları vardır.
Sizin sorumluluğunuzdadır kapsamını bulut hizmeti ve hizmetin kullanmayı düşündüğünüz senaryo dağıtım modellerini bağlı olarak değişir. Tek tek Hizmetleri belirli özelliklerini şifrelemeyle da sahip olabilirsiniz. Örneğin, bir içerik teslim ağına özel anahtarınızı nasıl korur veya nasıl bir bulut ödeme sağlayıcısı sahte işlemleri algılar, önemli güvenlik konuları bulut güvenlik ilkeleri tarafından kapsanan genel konular üzerine markalarıdır.
Iaas ve PaaS teklifleri ile verileri ve iş yükleri güvenliğini önemli yönleri sorumludur. Örneğin, tedarik etmek, bir Iaas işlem örneği, normalde modern bir işletim sistemi yükleme, işletim sistemi güvenli bir şekilde yapılandırmayı, güvenli bir şekilde tüm uygulamaları dağıtma ve ayrıca bu örnek uygulama aracılığıyla koruma sorumlu olacaktır düzeltme ekleri veya gerekli bakım gerçekleştirme.


**Sorumlulukları:**`Customer`

> [!NOTE]
> Müşteri, Azure Güvenlik Merkezi engellemenize, algılamanıza ve Artırılmış görünürlük ve Azure kaynaklarınızın güvenliğini denetlemenize tehditlerine karşı yanıt yardımcı olmak için kullanabilirsiniz. Azure Güvenlik Merkezi, Azure abonelikleri arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur.

|||
|---|---|
| **Müşteri** | Bu şeması oluşturan eşlik eden kaynakları ve Azure Resource Manager şablonları defence derinlemesine yaklaşım güvenlik izleyin. Bu ilke uyumlu olması için üretim (örn., veritabanı yönetim yazılımı, web uygulama dağıtımı) kullanmak için müşteri tarafından başka bir yapılandırma gereklidir. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Uygulanamaz |

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.
 - Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
 - Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.