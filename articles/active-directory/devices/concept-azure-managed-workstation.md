---
title: Güvenli, Azure tarafından yönetilen iş istasyonları - Azure Active Directory anlama
description: Güvenli, Azure tarafından yönetilen iş istasyonları hakkında bilgi edinin ve neden önemli anlayın.
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
ms.openlocfilehash: 02a6ddef294c4872f2d7e50e8940ecbb4b4b7bc4
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491586"
---
# <a name="understand-secure-azure-managed-workstations"></a>Güvenli, Azure tarafından yönetilen iş istasyonları anlama

Güvenli, yalıtılmış iş istasyonları, güvenlik yöneticileri, geliştiriciler ve kritik hizmet işleçler gibi hassas rollerinin öneme sahiptir. İstemci iş istasyonu güvenliği tehlikedeyse, çok sayıda güvenlik denetimleri ve Güvenceleri başarısız veya etkisiz.

Bu belgede, genellikle bir ayrıcalıklı erişim iş istasyonu (PAW) olarak bilinen güvenli bir iş istasyonu oluşturmak için gerekenler açıklanmaktadır. Makale ayrıca ilk güvenlik denetimlerini'kurmak için ayrıntılı yönergeler içerir. Bu kılavuz nasıl bulut tabanlı teknolojisini açıklar hizmeti yönetebilir. Windows 10RS5, Microsoft Defender Gelişmiş tehdit Koruması (ATP), Azure Active Directory ve Intune sunulan güvenlik özelliklerine güvenir.

> [!NOTE]
> Bu makalede, güvenli bir iş istasyonu ve önem derecesini kavramını açıklar. Zaten kavramına aşina olan ve dağıtım için atlamak istiyorsanız, ziyaret [güvenli bir iş istasyonu dağıtma](https://docs.microsoft.com/azure/active-directory/devices/howto-azure-managed-workstation).

## <a name="why-secure-workstation-access-is-important"></a>Güvenli bir iş istasyonu erişim neden önemlidir

Bulut Hizmetleri ve her yerden çalışma olanağı hızla benimsenmesini istismarıyla yöntem oluşturdu. Saldırganlar zayıf güvenlik denetimleri nerede Yöneticiler iş cihazlarda yararlanarak ayrıcalıklı kaynaklarına erişim elde edebilirsiniz.

Ayrıcalıklı kötüye ve tedarik zinciri saldırıları saldırganlar, kuruluşlar güvenlik ihlali için kullandığınız en çok beş arasında yöntemlerdir. Ayrıca ikinci oldukları en yaygın olarak algılanan taktikleri 2018'e göre bildirilen olaylarda [Verizon tehdit raporu](https://enterprise.verizon.com/resources/reports/dbir/)ve [güvenlik zekası raporu](https://aka.ms/sir).

Çoğu saldırganlar, şu adımları izleyin:

1. Keşif yöntemi, genellikle sektör için belirli bulunamadı.
1. Bilgi toplamak ve düşük değer olarak algılanan bir iş istasyonu sızmaya en iyi yolu belirlemek için analiz.
1. Kalıcılık taşımak için bir yol için aranacak [riskli](https://en.wikipedia.org/wiki/Network_Lateral_Movement).
1. Gizli ve hassas veri Sızdırma.

Keşfi sırasında saldırganlar genellikle düşük risk gibi görünüyor veya takdir edilmediklerini cihazlar sızmaya. Bunlar, yanal hareket fırsatı bulmak ve yönetici kullanıcıları ve cihazları bulmak için bu savunmasız cihazlar kullanır. Bunlar ayrıcalıklı kullanıcı rolleri için erişim elde ettikten sonra saldırganlar bu verileri yüksek değerli veri ve başarıyla sızdırabilir belirleyin.

![Tipik güvenliğinin aşılmasına deseni](./media/concept-azure-managed-workstation/typical-timeline.png)

Bu belgede, bilgi işlem cihazlarınızın bu tür yanal saldırılara karşı korunmasına yardımcı olabilecek bir çözüm açıklanmaktadır. Çözüm, yönetim ve hassas bulut kaynaklarına erişimi cihaz infiltrated önce zinciri bozucu küçük değerli üretkenlik cihazlardan Hizmetleri yalıtır. Çözüm Microsoft 365 Kurumsal yığınının parçası yerel Azure hizmetlerini kullanır:

* Intune cihaz yönetimi ve güvenli uygulamalar ve URL'ler listesi için
* AutoPilot cihaz kurulumu, dağıtım ve yenileme
* Kullanıcı yönetimi, koşullu erişim ve çok faktörlü kimlik doğrulaması için Azure AD
* Cihaz sistem durumu kanıtlama ve kullanıcı deneyimi için Windows 10 (geçerli sürüm)
* Defender ATP bulutta yönetilen bir uç nokta koruması, algılama ve yanıtlama
* Yetkilendirme ve tam zamanında (JIT) yönetmek için Azure AD PIM ayrıcalıklı kaynaklara erişimi

## <a name="who-benefits-from-a-secure-workstation"></a>Kimin bir güvenli iş istasyonundan fayda sağlar?

Tüm kullanıcılar ve işleçler güvenli bir iş istasyonu kullanırken yararlanır. Bir bilgisayarı veya cihazı etkilediğinde bir saldırgan, önbellekte saklanan tüm hesapları bürünebilir. Cihaza oturum açtıktan sonra bunlar ayrıca kimlik bilgileri ve belirteçleri kullanabilirsiniz. Bu riski önemli yönetim hakları dahil olmak üzere ayrıcalıklı roller için kullanılan güvenli cihazlara kolaylaştırır. Ayrıcalıklı hesaplar cihazlarla yanal hareket ve ayrıcalık yükseltme saldırılarını hedeflerdir. Bu hesaplar gibi çeşitli varlıklar için kullanılabilir:

* Şirket içi veya bulut tabanlı sistemleri Yöneticisi
* Kritik sistemleri için geliştirici iş istasyonu
* Sosyal medya hesap yöneticisine yüksek Etkilenme
* Bir terminal SWIFT ödeme gibi son derece hassas iş istasyonu
* İş istasyonu işleme ticari sırlar

Riski azaltmak için bu hesapları olun ayrıcalıklı iş istasyonları kullanmak için yükseltilmiş güvenlik denetimleri uygulamalıdır. Daha fazla bilgi için [Azure Active Directory özelliği Dağıtım Kılavuzu](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2), [Office 365 yol haritası](https://aka.ms/o365secroadmap), ve [ayrıcalıklı erişim güvenliğini sağlama yol haritası](https://aka.ms/sparoadmap)).

## <a name="why-use-dedicated-workstations"></a>Özel iş istasyonları neden kullanmalısınız?

Güvenlik için mevcut bir cihazın eklenmesi mümkün olsa da, güvenli bir temel ile başlatmak daha iyidir. Kuruluşunuz bir yüksek güvenlik düzeyi sağlamak için en iyi konumu koymak için güvenli olduğunu bildiğiniz bir cihazı ile başlatın ve bir dizi güvenlik denetimi uygulayın.

Giderek artan sayıda saldırı vektörlerinin e-posta ve Web'e göz atma üzerinden giderek bir cihaz güvenilirliğinden emin olmak zorlaştırır. Bu kılavuz, ayrı bir iş istasyonu standart üretkenlik, göz atma ve e-posta yalıtılmış olduğu varsayılır. Üretkenlik, Web'e göz atma ve e-posta bir CİHAZDAN kaldırılmasını üretkenliğine negatif bir etkiye sahip olabilir. Ancak, bu koruma genellikle iş görevleri açıkça zorunlu olmayan ve bir güvenlik olayı riskini yüksek olduğu yerde senaryoları için kabul edilebilir.

> [!NOTE]
> Burada gözatmayı yüksek riskli etkinlik olabilecek isteğe bağlı Web siteleri'ne genel erişim ifade eder. Bu tarama, iyi bilinen Yönetim Web sitelerinden Hizmetleri gibi Azure, Office 365, diğer bulut sağlayıcıları ve SaaS uygulamaları için az sayıda erişmek için bir web tarayıcısı kullanarak sonuçlanmaz farklıdır.

Kapsama stratejileri sayısını ve türünü bir saldırganın hassas varlıklara erişmesini önüne geçilmesine denetimlerin artırarak güvenliğini sıkıştırabilir. Bu makalede açıklanan modeli, bir katmanlı ayrıcalık tasarım kullanır ve yönetici ayrıcalıkları belirli cihazlara kısıtlar.

## <a name="supply-chain-management"></a>Tedarik zinciri yönetimi

Tedarik zinciri çözüm, 'root' güven adlı güvenilir bir iş istasyonu kullandığınız için güvenli bir iş istasyonu gereklidir. Bu çözüm için güven köküne kullanan [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot) teknoloji. Bir iş istasyonu güvenliğini sağlamak için Autopilot Microsoft OEM için iyileştirilmiş, Windows 10 cihazları yararlanmanıza olanak tanır. Bu cihazlar, üretici tarafından bilinen iyi bir durumda gelir. Autopilot, güvensiz olabilecek bir cihazı yeniden görüntü yerine bir "çalışma ortamına hazır" duruma Windows cihaz dönüştürebilirsiniz. Ayarları ve ilkeleri uygular, uygulamaları yükler ve bile Windows 10 sürümü değiştirir. Gelişmiş özellikleri kullanabilmesi için örneğin, Autopilot bir cihazın Windows yükleme Windows 10 Pro ' Windows 10 Enterprise için değişebilir.

![Güvenli bir iş istasyonu düzeyleri](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Cihaz roller ve Profiller

Bu kılavuz çeşitli güvenlik profilleri atıfta bulunan ve yardımcı olabilecek rolleri kullanıcılar, geliştiriciler ve BT personeli için daha güvenli çözümler oluşturun. Bu profiller, kullanılabilirlik ve riskleri bir güvenli veya Gelişmiş iş istasyonundan yararlanabilir genel kullanıcılar için dengeleyin. Burada sağlanan yapılandırmaları kabul endüstri standartlarına göre temel ayarları. Bu kılavuz, Windows 10 sağlamlaştırmak ve cihaz veya kullanıcı güvenlik aşılması ile ilişkili riskleri azaltmak gösterilmektedir. Bunu güvenlik özellikleri ve riskleri yönetilmesine yardımcı olmak için ilke ve teknoloji kullanarak yapar.
![Güvenli bir iş istasyonu düzeyleri](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Düşük güvenlik** – yönetilen, standart bir iş istasyonu çoğu ev ve küçük Kurumsal kullanım için iyi bir başlangıç noktası sağlar. Bu cihazları Azure AD'de kayıtlı ve Intune ile yönetilen. Bu profil, kullanıcıların tüm uygulamalarını çalıştırın ve herhangi bir Web sitesine Gözat izin verir. Kötü amaçlı yazılımdan koruma çözümünü ister [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) etkinleştirilmelidir.

* **Gelişmiş Güvenlik** – bu giriş düzeyi korumalı Ev kullanıcıları, küçük işletme kullanıcılarının ve genel geliştiriciler için iyi bir çözümdür.

   Gelişmiş iş istasyonu düşük güvenlik profilini güvenliğini artırmak için bir ilke tabanlı yoludur. Bu e-posta ve Web'e göz atma gibi üretkenlik araçları kullanırken de müşteri verilerle çalışmak için güvenli bir yol sağlar. Denetim ilkeleri ve Intune kullanıcı davranışı ve profil kullanımı için Gelişmiş bir iş istasyonu izlemek için kullanabilirsiniz. Windows 10 (1809) betiği ile Gelişmiş iş istasyonu profili dağıttığınızda ve Gelişmiş kötü amaçlı yazılım koruması kullanmanın avantajı sürer [Gelişmiş tehdit Koruması (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp).

* **Yüksek güvenlik** – bir iş istasyonu saldırı yüzeyini azaltmak için en etkili yöntem, iş istasyonu self-administer olanağı kaldırmaktır. Yerel yönetici hakları kaldırılıyor, güvenliği artıran bir adım, ancak yanlış uygulanırsa, üretkenliğini etkileyebilir. Yüksek güvenlik profilini Gelişmiş Güvenlik profili ile önemli ölçüde değişiklik geliştirir: yerel yönetici kaldırma Bu profili yüksek profili kullanıcılar için tasarlanmıştır: Yöneticiler, bordro ve kullanıcıların hassas verileri, hizmetleri ve işlemleri için onaylayan.

   Yüksek güvenlik kullanıcı daha denetimli bir ortamda hala e-posta ve Web'e Gözatmayı bir basit kullanımı deneyimi gibi etkinlikler gerçekleştirebilirsiniz olmanın yanı sıra ihtiyaç duyar. Kullanıcılar tanımlama bilgilerini, Sık Kullanılanlar ve diğer kısayolları çalışmaya gibi özellikleri bekler. Ancak, bu kullanıcıların cihazlarını hata ayıklama veya değiştirme olanağı gereksinim duymayabilirsiniz. Bunlar ayrıca sürücüleri yüklemeniz gerekmez. Yüksek güvenlik profilini, yüksek güvenlik - Windows 10 (1809) betiği kullanılarak dağıtılır.

* **Özelleştirilmiş** – saldırganlar, geliştiriciler ve BT yöneticileri, saldırganların ilgisini sistemleri değiştirebilirsiniz çünkü hedefler. Özel iş istasyonu, yerel uygulamaları yönetme ve Web siteleri sınırlama ilkeleri yüksek güvenlik iş istasyonunun genişletir. Ayrıca, ActiveX, Java, tarayıcı eklentileri ve diğer Windows denetimlerini gibi yüksek riskli üretkenlik özellikleri kısıtlar. Bu profille DeviceConfiguration_NCSC - Windows 10 (1803) SecurityBaseline betik dağıtın.

* **Güvenli** – bir yönetici hesabı etkilediğinde bir saldırgan önemli iş zarar veri hırsızlığına, veri değişikliği veya hizmet kesintisi. Sağlamlaştırılmış bu durumda, tüm güvenlik denetimleri ve yerel uygulama yönetiminin doğrudan denetim kısıtlama ilkeleri iş istasyonu sağlar. Üretkenlik araçları olmadan güvenli bir iş istasyonu serileştirilmesini cihaz tehlikeye daha zor. Kimlik avı saldırıları için en yaygın saldırı engeller: e-posta ve sosyal medya.  İş istasyonu güvenli - Windows 10 (1809) SecurityBaseline betik güvenli iş istasyonu dağıtılabilir.

   ![Güvenli bir iş istasyonu](./media/concept-azure-managed-workstation/secure-workstation.png)

   Güvenli bir iş istasyonunda, yönetici NET uygulama denetimi ve application guard sağlamlaştırılmış iş istasyonu ile sağlar. İş istasyonu kötü amaçlı bir davranış ana bilgisayarı korumak için credential guard, device guard ve exploit guard'ı kullanır. Tüm yerel diskler de BitLocker ile şifrelenir.

* **Yalıtılmış** – bu özel, çevrimdışı senaryoda spektrumun aşırı sonunu temsil eder. Yükleme komut dosyası, bu durum için sağlanır. Desteklenmeyen veya yüklenmemiş eski işletim sistemi gerektiren bir iş açısından kritik işlevi yönetmeniz gerekebilir. Örneğin, bir yüksek değerli üretim hattı veya yaşam – destek sistemi. Güvenlik kritik öneme sahiptir ve bulut hizmetleri kullanılamıyor çünkü yönetin ve bu bilgisayarları el ile veya yalıtılmış bir Active Directory orman mimarisi gibi gelişmiş güvenlik yönetim ortamı (ESAE) ile güncelleştirin. Bu durumlarda, temel Intune ve ATP sistem durumu denetimleri dışındaki tüm erişimi kaldırmayı düşünün.

  * [Intune ağ iletişimi gereksinimleri](https://docs.microsoft.com/intune/network-bandwidth-use)
  * [ATP ağ iletişimi gereksinimleri](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Sonraki adımlar

[Azure tarafından yönetilen güvenli iş istasyonunu dağıtma](howto-azure-managed-workstation.md).
