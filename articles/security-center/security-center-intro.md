---
title: "Azure Güvenlik Merkezi nedir? | Microsoft Docs"
description: "Azure Güvenlik Merkezi, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 50a54b8d2a73807aa9a0217f7ccf971b8c516494
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, karma bulut iş yüklerinde Birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması sağlar. Güvenlik Merkezi ile iş yüklerinizi arasında güvenlik ilkelerini uygulamak, sizin tehditlere maruz sınırlamak ve algılayabilir ve saldırılara karşı yanıt.

Güvenlik Merkezi neden kullanılır?

- **Merkezi ilke yönetimi** – şirket ile uyumluluğu veya yasal güvenlik gereksinimleri, güvenlik ilkelerini merkezi olarak tüm, karma bulut iş yüklerinde yöneterek emin olun.
- **Sürekli güvenlik değerlendirmesi** – makineleri, ağları, depolama ve veri hizmetleri ve uygulamaları olası güvenlik sorunlarını bulmak için güvenliğini izleyin.
- **İşlem yapılabilir önerileri** – öncelikli ve tıklatılabilir güvenlik önerileri ile saldırganlar tarafından yararlanılabilir önce güvenlik açıkları düzeltin.
- **Bulut savunma Gelişmiş** – Vm'leriniz çalışan denetim uygulamalara tehditlere karşı zaman Access'te yalnızca yönetim bağlantı noktalarını ve uygulamaları güvenilir listeye almayı azaltın.
- **Uyarılar ve olaylar öncelik** -en kritik tehditleri odaklanmak ilk ile öncelikli güvenlik uyarıları ve olaylar.
- **Tümleşik güvenlik çözümleri** - toplamak, arama ve kaynakları bağlı iş ortağı çözümleri dahil olmak üzere, çeşitli güvenlik verileri analiz etmek.

**Güvenlik Merkezi - genel bakış** Azure ve Azure dışı iş yükleri bulmak ve güvenlik iş yüklerinizin değerlendirin ve tanımlamak ve riski azaltmak etkinleştirilmesi, güvenlik duruşunu içine hızlı bir görünümünü sağlar. Yerleşik Pano güvenlik uyarıları ve dikkat gerektiren güvenlik açıkları anlık fikir sağlar.

![Genel Bakış][1]

## <a name="centralized-policy-management"></a>Merkezi ilke yönetimi
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Güvenlik Merkezi, ilkeleri tanımlayın ve bunları, iş yükü türünü veya verilerinizin duyarlılığına uyarlama.

Güvenlik Merkezi ilkeleri aşağıdaki bileşenleri içerir:

- **Veri toplama**: sağlama aracısı ve güvenlik belirler [veri toplama](security-center-enable-data-collection.md) ayarlar.
- **Güvenlik İlkesi**: Güvenlik Merkezi izleyiciler denetler ve düzenleyerek önerir belirlenmesi [Güvenlik İlkesi](security-center-policies.md).
- **E-posta bildirimleri**: güvenlik kişiler belirler ve [bildirim e-posta](security-center-provide-security-contact-details.md) ayarlar.
- **Fiyatlandırma katmanı**: tanımlar ücretsiz veya standart [seçimi fiyatlandırma](security-center-pricing.md). Seçtiğiniz katmanı kapsamında kaynaklar için hangi Güvenlik Merkezi özellikleri kullanılabileceğini belirler.

![Güvenlik ilkesi][2]

Bkz: [güvenlik ilkelerine genel bakış](security-center-policies-overview.md) daha fazla bilgi için.

## <a name="continuous-security-assessment"></a>Sürekli güvenlik değerlendirmesi
Güvenlik Merkezi işlem kaynakları, sanal ağlar, depolama ve veri hizmetleri ve uygulamaları güvenlik durumunu çözümler. Sürekli değerlendirme, gösterilen ağ bağlantı noktasını veya güvenlik güncelleştirmeleri eksik olan sistemleri gibi olası güvenlik sorunlarını keşfetmeye yardımcı olur. Bir kutucuk önleme bölümünde kaynakları ve belirlenen tüm güvenlik açıkları listesi dahil olmak üzere daha fazla bilgi görüntülemek için seçin.

![Güvenlik durumunu izleme][3]

Bkz: [güvenlik durumunu izleme](security-center-monitoring.md) daha fazla bilgi için.

## <a name="actionable-recommendations"></a>İşlem yapılabilir önerileri
Güvenlik Merkezi, Azure ve Azure olmayan kaynakları olası güvenlik açıklarını tanımlamak için güvenlik durumunu çözümler. Öncelikli güvenlik önerileri listesini güvenlik sorunlarını çözdükten işleminde size rehberlik eder.

![Öneriler][4]

Bkz: [güvenlik önerilerini yönetme](security-center-recommendations.md) daha fazla bilgi için.

## <a name="just-in-time-vm-access"></a>Anlık VM erişimi
Tam zamanında, Azure vm'lerinde deneme yanılma ve diğer ağ saldırılarına maruz büyük ölçüde azaltır, yönetim noktalarına denetimli erişim ile ağ saldırı yüzeyini azaltın.

![Anlık VM erişimi][5]

Kullanıcıların sanal makineleri nasıl bağlanıp için kuralları belirtin. Gerekli olduğunda, Güvenlik Merkezi'nden veya PowerShell aracılığıyla erişim istenebilir. Talep kuralları ile uyumlu olduğu sürece, erişim için istenen saat otomatik olarak verilir.

Bkz: [tam zamanında kullanarak sanal makine erişimini yönetme](security-center-just-in-time.md) daha fazla bilgi için.

## <a name="adaptive-application-controls"></a>Uyarlamalı uygulama denetimleri
Kötü amaçlı yazılım ve diğer istenmeyebilecek uygulamaları, belirli Azure iş yükleri için uyarlanmış ve makine öğrenme tarafından desteklenen uygulamaları güvenilir listeye almayı önerileri uygulayarak engelleyin.

![Uyarlamalı uygulama denetimleri][6]

Gözden geçirin ve Güvenlik Merkezi tarafından oluşturulan önerilen uygulama uygulamaları güvenilir listeye almayı kurallarını uygulamak için zaten yapılandırılmış kuralları veya düzenleyin.

Bkz: [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md) daha fazla bilgi için.

## <a name="prioritized-alerts-and-incidents"></a>Öncelikli uyarıların ve olaylar
Güvenlik Merkezi, gelen saldırıları algılamak ve etkinlik sonrası ihlal için gelişmiş analizler ve genel tehdit bilgileri kullanır. Uyarıları öncelik ve olaylar gruplandırılır ilk en kritik tehditlerinin odaklanmanıza yardımcı olma. Kendi özel güvenlik uyarıları da oluşturabilirsiniz.

![Öncelikli uyarıların ve olaylar][7]

Hızlı bir şekilde visual, etkileşimli araştırma deneyim saldırının etkisini ve kapsamını değerlendirmek ve önceden tanımlanmış veya geçici sorgular güvenlik verileri daha derin keşif için kullanın.

Bkz: [yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) daha fazla bilgi için.

## <a name="integrate-your-security-solutions"></a>Güvenlik çözümlerini tümleştirmenize
Toplamak, arama ve kaynakları, ağ güvenlik duvarları ve diğer Microsoft Hizmetleri gibi bağlı iş ortağı çözümlerinden Güvenlik Merkezi'nde de dahil olmak üzere çeşitli güvenlik verileri analiz edin.

![Güvenlik çözümlerini tümleştirmenize][8]

Bkz: [güvenlik çözümlerini tümleştirmenize](security-center-partner-integration.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliği gerekir. Bir abonelik yoksa için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/free/).
- Güvenlik Merkezi'nin ücretsiz fiyatlandırma katmanı, Azure aboneliğiniz ile etkinleştirilir. Gelişmiş güvenlik yönetimi ve tehdit algılama özellikleri yararlanmak için standart fiyatlandırma katmanı yükseltmeniz gerekir. Standart katman ilk 60 gün boyunca ücretsizdir. Bkz: [Güvenlik Merkezi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/) daha fazla bilgi için.
- Şimdi, Güvenlik Merkezi standart etkinleştirmek için hazır olduğunuzda [hızlı başlangıç: yerleşik Azure aboneliğinize Güvenlik Merkezi standart](security-center-get-started.md) adımlarda size yol gösterir.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
