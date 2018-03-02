---
title: "Azure Güvenlik Merkezi nedir?| Microsoft Docs"
description: "Azure Güvenlik Merkezi, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: terrylan
ms.openlocfilehash: 08102ce4caead003925aa600f4f7f005b1c336e0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, hibrit bulut iş yükleri arasında birleşik güvenlik yönetimi ve gelişmiş tehdit koruması sağlar. Güvenlik Merkezi ile, iş yüklerinize güvenlik ilkeleri uygulayabilir, tehditlere maruz kalma riskinizi sınırlayabilir ve saldırıları algılayıp onlara yanıt verebilirsiniz.

Güvenlik Merkezi neden kullanılır?

- **Merkezi ilke yönetimi** – Tüm hibrit bulut iş yüklerinizde güvenlik ilkelerini merkezi şekilde yöneterek şirket veya yasal güvenlik gereksinimleriyle uyumluluk sağlayın.
- **Sürekli güvenlik değerlendirmesi** – Olası güvenlik sorunlarını keşfetmek için makinelerin, ağların, depolama ve veri hizmetlerinin güvenliğini izleyin.
- **Eyleme dönüştürülebilir öneriler** – Önceliği belirlenmiş ve eyleme dönüştürülebilir güvenlik önerileri sayesinde saldırganlar yararlanmadan önce güvenlik açıklarını düzeltin.
- **Gelişmiş bulut savunması** – Sanal makinelerinizde çalışan uygulamaları denetlemek için beyaz listeye alma ve yönetim bağlantı noktalarına tam zamanında erişim sayesinde tehditleri azaltın.
- **Önceliği belirlenmiş uyarılar ve olaylar** - Önceliği belirlenmiş güvenlik uyarıları ve olaylar sayesinde öncelikle en kritik tehditlere odaklanın.
- **Tümleşik güvenlik çözümleri** - Bağlantı halindeki iş ortağı çözümleri gibi farklı kaynaklardan güvenlik verileri toplayın, bunlar üzerinde arama ve analiz gerçekleştirin.

**Güvenlik Merkezi - Genel Bakış**, Azure ve Azure olmayan iş yüklerinizin güvenlik durumuna yönelik hızlı bir görünüm sağlayarak iş yüklerinizin güvenliğini keşfedip değerlendirmenize ve riski belirleyip azaltmanıza olanak verir. Yerleşik pano, dikkat gerektiren güvenlik uyarılarına ve güvenlik açıklarına yönelik anında öngörüler sağlar.

![Genel Bakış][1]

## <a name="centralized-policy-management"></a>Merkezi ilke yönetimi
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Güvenlik Merkezi’nde, ilkeleri tanımlar ve bunları iş yükünüzün türüne veya verilerinizin duyarlılığına göre uyarlarsınız.

Güvenlik Merkezi ilkeleri aşağıdaki bileşenleri içerir:

- **Veri toplama**: Aracı sağlama ve güvenlik için [veri toplama](security-center-enable-data-collection.md) ayarlarını belirler.
- **Güvenlik ilkesi**: [Güvenlik ilkesini](security-center-policies.md) düzenleyerek Güvenlik Merkezi’nin hangi denetimleri izlediğini ve önerdiğini belirleyin.
- **E-posta bildirimleri**: Güvenlik ilgili kişilerini ve [e-posta bildirimi](security-center-provide-security-contact-details.md) ayarlarını belirler.
- **Fiyatlandırma katmanı**: Ücretsiz veya Standart [fiyatlandırma seçimini](security-center-pricing.md) tanımlar. Seçtiğiniz katman, kapsam dahilindeki kaynaklar için hangi Güvenlik Merkezi özelliklerinin kullanılabilir olduğunu belirler.

![Güvenlik ilkesi][2]

Daha fazla bilgi için bkz. [Güvenlik ilkelerine genel bakış](security-center-policies-overview.md).

## <a name="continuous-security-assessment"></a>Sürekli güvenlik değerlendirmesi
Güvenlik Merkezi, işlem kaynaklarınızın, sanal ağlarınızın, depolama ve veri hizmetlerinizin ve uygulamalarınızın güvenlik durumunu analiz eder. Sürekli değerlendirme, güvenlik güncelleştirmeleri eksik olan sistemler veya güvenlik açığı olan ağ bağlantı noktaları gibi olası güvenlik sorunlarını keşfetmenize yardımcı olur. Kaynakların listesi ve belirlenen güvenlik açıkları gibi daha fazla bilgi görüntülemek için Koruma bölümünden bir kutucuk seçin.

![Güvenlik durumunu izleme][3]

Daha fazla bilgi için bkz. [Güvenlik durumunu izleme](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Eyleme dönüştürülebilir öneriler
Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure ve Azure olmayan kaynaklarınızın güvenlik durumunu analiz eder. Önceliği belirlenmiş güvenlik önerileri listesi, güvenlik sorunlarına yanıt verme süreci boyunca size yol gösterir.

![Öneriler][4]

Daha fazla bilgi için bkz. [Güvenlik önerilerini yönetme](security-center-recommendations.md).

## <a name="just-in-time-vm-access"></a>Tam zamanında VM erişimi
Azure VM’lerdeki yönetim bağlantı noktalarına tam zamanında, denetimli erişim sayesinde deneme yanılma ve diğer ağ saldırılarına maruz kalma oranını büyük ölçüde düşürerek ağ saldırısına maruz kalan yüzey alanını azaltın.

![Tam zamanında VM erişimi][5]

Kullanıcıların sanal makinelere nasıl bağlanabileceğine ilişkin kuralları belirtin. Gerektiğinde, Güvenlik Merkezi'nden veya PowerShell aracılığıyla erişim istenebilir. İstek kurallara uygun olduğu sürece, istenen süre boyunca otomatik olarak erişim izni verilir.

Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](security-center-just-in-time.md).

## <a name="adaptive-application-controls"></a>Uyarlamalı uygulama denetimleri
Kendi Azure iş yüklerinize göre uyarlanmış ve makine öğrenmesi tarafından desteklenen beyaz listeye alma önerilerini uygulayarak kötü amaçlı yazılımları ve diğer istenmeyen uygulamaları engelleyin.

![Uyarlamalı uygulama denetimleri][6]

Güvenlik Merkezi tarafından oluşturulan önerilen uygulama beyaz listeye alma kurallarını uygulamak veya önceden yapılandırılmış kuralları düzenlemek için gözden geçirin ve tıklayın.

Daha fazla bilgi için bkz. [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md).

## <a name="prioritized-alerts-and-incidents"></a>Önceliği belirlenmiş uyarılar ve olaylar
Güvenlik Merkezi, gelen saldırıları ve ihlal sonrası etkinliği algılamak için gelişmiş analiz ve genel tehdit bilgilerini kullanır. Uyarıların öncelikleri belirlenir ve sonra uyarılar, olaylar halinde gruplanır. Böylece en kritik tehditlere en önce odaklanmanız sağlanır. Kendi özel güvenlik uyarılarınızı da oluşturabilirsiniz.

![Önceliği belirlenmiş uyarılar ve olaylar][7]

Görsel ve etkileşimli bir araştırma deneyiminin kapsamını ve etkisini hızlı şekilde değerlendirebilir ve güvenlik verilerinin daha ayrıntılı keşfi için önceden tanımlanmış veya geçici sorgular kullanabilirsiniz.

Daha fazla bilgi için bkz. [Güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md).

## <a name="integrate-your-security-solutions"></a>Güvenlik çözümlerinizi tümleştirmenize
Güvenlik Merkezi’nde, ağ güvenlik duvarları ve diğer Microsoft hizmetleri gibi bağlantı halindeki iş ortağı çözümlerini kapsayan çeşitli kaynaklardan güvenlik verileri toplayabilir, bunlar üzerinde arama ve analiz gerçekleştirebilirsiniz.

![Güvenlik çözümlerini tümleştirme][8]

Daha fazla bilgi için bkz. [Güvenlik çözümlerini tümleştirme](security-center-partner-integration.md).

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik Merkezi ile çalışmaya başlamak için bir Microsoft Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Güvenlik Merkezi’nin Ücretsiz fiyatlandırma katmanı, Azure aboneliğiniz ile etkinleştirilir. Gelişmiş güvenlik yönetimi ve tehdit algılama yeteneklerinden yararlanmak için Standart fiyatlandırma katmanına yükseltmeniz gerekir. Standart katman ilk 60 gün boyunca ücretsizdir. Daha fazla bilgi için [Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/) bakın.
- Şimdi Güvenlik Merkezi Standart katmanını etkinleştirmeye hazırsanız, [Hızlı Başlangıç: Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md) başlıklı konuda işlem adım adım açıklanmıştır.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
