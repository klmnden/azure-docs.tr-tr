---
title: Önemli - güvenli iş istasyonları neden Azure Active Directory
description: Kuruluşlar Azure tarafından yönetilen güvenli iş istasyonları neden oluşturmalısınız öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7e9ca9fa26e9744eb0a9bfafe692a096825b0b5
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357042"
---
# <a name="building-secure-workstations"></a>Yapı güvenli iş istasyonları

Güvenli yalıtılmış iş istasyonları, güvenlik yöneticileri, geliştiriciler ve kritik Hizmetleri işleçler gibi hassas rollerinin öneme sahiptir. Diğer birçok güvenlik denetimleri ve Güvenceleri başarısız veya temel alınan istemci iş istasyonu güvenliği ihlal edilmesi durumunda herhangi bir etkisi yoktur.

Bu belgede, güvenlik denetimleri başlatılıyor ayarlama dahil ayrıntılı adım adım yönergeler içeren bir güvenli istemci iş istasyonu oluşturmak için neler açıklanmaktadır. Bu tür bir iş istasyonları bazen bu başvuruda kullanılan ve üzerine bir ayrıcalıklı erişim iş istasyonu (PAW) adı verilir. Kılavuz ancak teknoloji için bulut tabanlı hizmetini yönetmek için arar ve Windows 10RS5, Microsoft Defender ATP, Azure Active Directory ve Intune başlangıç sunulan güvenlik özellikleri sunar.

## <a name="why-securing-workstation-access-is-important"></a>İş istasyonu erişiminin güvenliğini sağlama neden önemli olduğunu

Bulut Hizmetleri ve her yerden çalışma olanağı hızla benimsenmesini yararlanılması için yeni bir yöntem oluşturdu. Burada, yöneticiler iş ve ayrıcalıklı kaynaklara erişim elde edebilir cihazlarda zayıf güvenlik denetimleri, saldırganlar kötüye kullanan çok.

Açıklandığı gibi [Verizon tehdit raporu](https://enterprise.verizon.com/resources/reports/dbir/), ve [güvenlik zekası raporu](https://aka.ms/sir) ayrıcalıklı kötüye ve tedarik zinciri saldırıları olan kuruluşlar, güvenlik ihlali için kullanılan ilk beş mekanizmaları ve İkinci 2018'de bildirilen olaylarda en yaygın olarak Taktik algılandı.

Çoğu saldırganlar aşağıdaki yolunu izleyin:

* Keşfi, bir şekilde bulmak için bir endüstride genellikle belirli başlayın
* Algılanan düşük değerin iş istasyonunun (İSTİHBARATI) erişim elde etmek için en iyi yolu belirlemek için toplanan bilgileri analiz edin
* Kalıcılığı ve taşımak için yol göz [riskli](https://en.wikipedia.org/wiki/Network_Lateral_Movement)
* Gizli ve hassas veri sızdırabilir

Saldırganlar genellikle düşük risk gibi görünüyor veya takdir edilmediklerini cihazlar için keşif sızmaya. Bu güvenlik açığından etkilenen cihazlar, ardından fırsatı yanal hareket bulun, yönetici kullanıcıları ve cihazları bulmak ve bunlar bu ayrıcalıklı kullanıcı rolleri elde sonra başarıyla sızdırabilir bilgilere yüksek değerli verileri tanımlamak için kullanılır.

![Tipik güvenliğinin aşılmasına deseni](./media/concept-azure-managed-workstation/typical-timeline.png)

Bu belge küçük değerli üretkenlik cihazlardan saldırıları ya da yönetim ve yanal hareket karşı korumaya yardımcı olmak amacıyla hizmetlerin yalıtarak bilgi işlem cihazlarınızı korumaya yardımcı olmak için bir çözüm sağlar. Tasarım, özel olarak başarıyla İSTİHBARATI yönetmek veya önemli bulut kaynaklarına erişmek için kullanılan cihazın önce zinciri bölerek ihlal yürütme yeteneği azaltmaya yardımcı olur. Açıklanan çözümünü dahil olmak üzere Microsoft 365 Kurumsal yığınının bir parçası olan yerel Azure hizmetlerine yararlanacaktır:

* Intune cihaz yönetimi, uygulama ve URL beyaz listeye ekleme dahil olmak üzere
* AutoPilot cihaz Kurulum ve dağıtım ve yenileme 
* Kullanıcı yönetimi, koşullu erişim ve çok faktörlü kimlik doğrulaması için Azure AD
* Cihaz sistem durumu kanıtlama ve kullanıcı deneyimi için Windows 10 (geçerli sürüm)
* Microsoft Defender Gelişmiş tehdit Koruması (ATP) için endpoint protection, algılama ve yanıt bulut Yönetimi
* Azure AD PIM yalnızca zamanında (JIT) dahil olmak üzere yetkilendirme için ayrıcalıklı kaynaklara erişimi

## <a name="who-benefit-from-using-a-secure-workstation"></a>Kimin güvenli bir iş istasyonu kullanarak avantaj elde

Tüm kullanıcılar ve işleçlerini kullanarak güvenli bir iş istasyonu avantaj elde. Ödün bir bilgisayar veya cihaz bir saldırgan, tüm önbelleğe alınan hesaplar ve kimlik bilgilerini kullan ve bunlar oturumunuz açıkken bu aygıtta kullanılan belirteçleri dahil birçok şey kimliğine bürün. Bu riski ayrıcalıklı bir hesap kullanıldığı cihazlar yanal hareket ve ayrıcalık yükseltme saldırılarını hedefler, böylece önemli yönetim hakları dahil tüm ayrıcalıklı rol için kullanılan cihazların güvenliğini sağlama işlemini yapar. Bu hesaplar gibi çeşitli varlıklar için kullanılabilir:

* Şirket içi ve bulut tabanlı sistemlerinin yöneticileri
* Kritik sistemleri için geliştirici iş istasyonları
* Sosyal medya hesaplarını yüksek Etkilenme yöneticisiyle
* SWIFT ödeme terminaller gibi son derece hassas iş istasyonları
* Ticari sırlar işleme iş istasyonları

Microsoft, bu hesapları riskini azaltmak için kullanıldığı ayrıcalıklı iş istasyonları için yükseltilmiş güvenlik denetimlerini uygulama önerir. Ek yönergeler bulunabilir [Azure Active Directory özelliği Dağıtım Kılavuzu](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2), [Office 365 yol haritası](https://aka.ms/o365secroadmap), ve [ayrıcalıklı erişim güvenliğini sağlama yol haritası](https://aka.ms/sparoadmap)).

## <a name="why-dedicated-workstations"></a>Neden adanmış iş istasyonları

Güvenlik için mevcut bir cihazın eklenmesi mümkün olsa da, güvenli bir temel ile başlatmak daha iyidir. İyi bilinen bir cihaz ve güvenlik denetimleri puts kümesi ile kuruluşunuz, korumak için en iyi konumu başlayarak, güvenlik düzeyi artar. Sıradan bir e-posta ve Web'e Gözatmayı tarafından izin verilen saldırı vektörlerinin sürekli büyüyen sayısı ile bir cihaz güvenilirliğinden emin olmak giderek daha fazla sabit. Bu kılavuzu çalışır ayrı bir iş istasyonu varsayımı tarama, standart üretkenliğinden ayrılır ve e-posta görevleri tamamlandı. Üretkenlik, Web'e göz atma ve e-posta bir CİHAZDAN kaldırılmasını üretkenliğine negatif bir etkiye sahip olabilir, ancak bu koruma genellikle iş görevleri açıkça zorunlu olmayan ve bir güvenlik olayı riskini yüksek olduğu yerde senaryoları için kullanılabilir.

> [!NOTE]
> Burada gözatmayı belirgin bir web tarayıcısı kullanarak yönetim iyi bilinen Web siteleri gibi Azure, Office 365, diğer bulut sağlayıcıları ve SaaS Hizmetleri için az sayıda erişmek için farklı bir yüksek risk rastgele Web siteleri'ne genel erişim başvurur. uygulamalar.

Kapsama stratejileri denetimleri saldırganın hassas varlıklara erişmek için üstesinden gelmek için sahip türü ve numarası artırarak daha fazla güvenlik Güvenceleri sağlar. Burada geliştirilen model yönetim ayrıcalıkları katmanlı ayrıcalık modeli kullanarak belirli cihazlara yayılmasını önleyen kendini kanıtlamış sağlar.

## <a name="supply-chain-management"></a>Tedarik zinciri yönetimi

Tedarik zinciri çözüm kullandığınız iş istasyonu olduğu güvenilir, güvenli bir iş istasyonunda gereklidir 'root' güven. Bu çözüm, güven kullanmanın kök ele alınacaktır [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot) teknoloji. Güvenli bir iş istasyonu için iyi bilinen bir duruma üreticisinden sağlayan Microsoft OEM için iyileştirilmiş, Windows 10 cihazları yararlanma olanağına Microsoft Autopilot sağlar. Yeniden görüntü güvenilmemiş olabilir bir cihaz yerine Microsoft Autopilot Windows cihaz bir "çalışma ortamına hazır" duruma uygulama ayarları ve ilkeleri, uygulamaları yükleme dönüştürebilirsiniz ve bile Windows 10 sürümü değiştirme (örneğin, den kullanılıyor Windows 10 Pro gelişmiş özelliklerini desteklemek üzere Windows 10 Enterprise için).

![Güvenli bir iş istasyonu düzeyleri](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Cihaz roller ve Profiller

Kılavuz kullanıcılar, geliştiriciler ve BT operasyon personeli için daha güvenli bir çözüm elde etmek için birden çok güvenlik profilleri ve rolleri getirilecektir. Bu profiller bir Gelişmiş ya da güvenli iş istasyonundan, kullanılabilirlik ve risk Dengeleme çalışırken yararlı olabilir kuruluşlarda ortak kullanıcılarını destekleyecek şekilde hizalı. Kılavuz kabul endüstri standartlarına göre ayarlarının yapılandırılmasını sağlar. Bu kılavuz, Windows 10 sağlamlaştırma ve güvenlik özellikleri ve riskleri yönetmenize yardımcı olmak için ilke ve teknoloji kullanarak cihaz veya kullanıcı güvenlik aşılması ile ilişkili risklerini azaltarak bir yöntem göstermek için kullanılır.
![Güvenli bir iş istasyonu düzeyleri](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Düşük güvenlik** – yönetilen standart iş istasyonu çoğu ev ve küçük Kurumsal kullanım için iyi bir başlangıç noktası sağlar. Bu cihazlar Azure AD'ye kayıtlı olan ve Intune yönetilen. Profil, kullanıcıların tüm uygulamalarını çalıştırın ve herhangi bir Web sitesine Gözat izin verir. Kötü amaçlı yazılımdan koruma çözümünü ister [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) etkinleştirilmelidir.
* **Gelişmiş Güvenlik** – Ev kullanıcıları, küçük işletme kullanıcılarının, ek olarak geliştiricilere genel bir giriş düzeyi Korumalı çözüm iyi olduğu.
   * Gelişmiş iş istasyonu, düşük güvenlik profilinin güvenliğini artırmak bir ilke tabanlı araçları sağlar. Bu profili müşteri verileri ile çalışma ve e-posta ve Web'e göz atma denetimi gibi üretkenlik araçları kullanabilmek için güvenli bir yol sağlar. Gelişmiş bir iş istasyonu, denetim ilkeleri etkinleştirme ve günlük kaydı için Intune tarafından bir iş istasyonunda profili kullanımını ve kullanıcı davranışını denetlemek için kullanılabilir. Bu profilde iş istasyonu güvenlik denetimleri etkinleştirir ve ilkeleri içeriğinde açıklandığı ve Gelişmiş iş istasyonunda - Windows 10 (1809) betik dağıtılan. Dağıtım ayrıca Gelişmiş kötü amaçlı yazılım koruması kullanmanın yararlanır [Gelişmiş tehdit Koruması (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp)
* **Yüksek güvenlik** – bir iş istasyonu saldırı yüzeyini azaltmak için en etkili yöntem, iş istasyonu self-administer olanağı kaldırmaktır. Yerel yönetici hakları kaldırılıyor güvenliğini geliştiren bir adımdır ve üretkenlik yanlış uygulanırsa etkileyebilir. Yüksek güvenlik profilini Gelişmiş Güvenlik profili ile bir önemli ölçüde değişiklik, yerel yönetici kaldırılmasını geliştirir Bu profili, belki de yüksek profili bir kullanıcı bir yönetici gibi veya olabilecek kullanıcıların başvurun Bordro veya hizmet ve işlem onayını gibi hassas veriler ile kullanıcılara yardımcı olmak amacıyla tasarlanmıştır.
   * Yüksek güvenlik kullanıcı profili, yine de posta ve web tarama deneyimi kullanmak için basit bir korurken gibi üretkenliği kullandıklarının gerçekleştirebilir olmanın yanı sıra daha yüksek denetimli bir ortamda ihtiyaç duyar. Kullanıcılar tanımlama bilgilerini ve Sık Kullanılanlar çalışmak kullanılabilen diğer kısayolları gibi özellikleri bekler. Ancak bu kullanıcılar değiştirmek ya da cihazını hata ayıklama özelliği gerekli değil ve sürücüleri yüklemeniz gerekmez. Yüksek güvenlik profilini, yüksek güvenlik - Windows 10 (1809) betiği kullanılarak dağıtılır.
* **Özelleştirilmiş** – geliştiriciler ve BT yöneticileri saldırganların cazip bir hedef olarak bu roller, saldırganların ilgisini sistemleri değiştirebilirsiniz. Yüksek güvenlik iş istasyonu ve yerel uygulamaları yönetme, Internet web siteleri sınırlama ve kısıtlama yüksek üretkenlik özellikleri, güvenlik riski ActiveX gibi başka emphases dağıtılan çaba alan özel iş istasyonu Java, tarayıcı eklentisi'nın ve bir Windows cihazda çeşitli diğer bilinen yüksek riskli denetimler. Bu profilde iş istasyonu güvenlik denetimleri etkinleştirir ve ilkeleri içeriğinde açıklandığı ve DeviceConfiguration_NCSC - Windows 10 (1803) SecurityBaseline betik dağıtılmış.
* **Güvenli** – bir yönetici hesabı tehlikeye atabilir bir saldırgan genellikle önemli iş zarar veri hırsızlığına, veri değişikliği veya hizmet kesintisi neden olabilir. Sağlamlaştırılmış bu durumdaki iş istasyonu tüm güvenlik denetimleri ve kısıtlama doğrudan denetimi yerel uygulama yönetimi ilkelerini etkinleştirmek ve üretkenlik araçlarına kaldırılır. Sonuç olarak, cihaz ödün posta daha zor yapılır ve sosyal medya, kimlik avı saldırıları başarılı olabilmesi için en yaygın yolu yansıtacak engellenir.  İş istasyonu güvenli - Windows 10 (1809) SecurityBaseline betik güvenli iş istasyonu dağıtılabilir.

   ![Güvenli bir iş istasyonu](./media/concept-azure-managed-workstation/secure-workstation.png)

   Güvenli bir iş istasyonunda, yönetici NET uygulama denetimi ve application guard sağlamlaştırılmış iş istasyonu sağlar. İş istasyonu, kötü amaçlı bir davranış ana bilgisayarı korumak için kimlik bilgisi, cihaz ve exploit guard'ı kullanır. Ayrıca tüm yerel diskler Bitlocker şifrelemesi ile şifrelenir.

* **Yalıtılmış** – bu özel bir çevrimdışı senaryoda (Bu durumda yükleme betik sağlanır) spektrumun aşırı sonunu temsil eder. Kuruluşlar, eski işletim sistemlerini desteklenmeyen/düzeltme yüklenmemiş gerektiren bir yalıtılmış iş kritik işlevi yüksek değerli üretim satırı veya yaşam sistemleri gibi yönetmek gerekebilir. Güvenlik kritik öneme sahiptir ve bulut hizmetleri kullanılamıyor çünkü kuruluşların el ile bu bilgisayarları yönetme/güncelleştirme veya bunları yönetmek için yalıtılmış bir Active Directory orman mimari (gibi gelişmiş güvenlik yönetim ortamı (ESAE)) kullanın. Bu durumlarda, Intune ve temel ATP dışındaki tüm erişimi kaldırılıyor sistem durumu denetimi düşünülmelidir.
   * [Intune ağ iletişimi gereksinimleri](https://docs.microsoft.com/intune/network-bandwidth-use)
   * [ATP ağ iletişimi gereksinimleri](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Sonraki adımlar

[Azure tarafından yönetilen güvenli iş istasyonunu dağıtma](howto-azure-managed-workstation.md)
