---
title: Azure Güvenlik Merkezi nedir?| Microsoft Docs
description: Azure Güvenlik Merkezi, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: bd0e517845b9cfcbe6090dff8d656edcca782c83
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126301"
---
# <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, hibrit bulut iş yükleri arasında birleşik güvenlik yönetimi ve gelişmiş tehdit koruması sağlar. Güvenlik Merkezi ile, iş yüklerinize güvenlik ilkeleri uygulayabilir, tehditlere maruz kalma riskinizi sınırlayabilir ve saldırıları algılayıp onlara yanıt verebilirsiniz.

Güvenlik Merkezi neden kullanılır?

- **Merkezi ilke yönetimi** – Tüm hibrit bulut iş yüklerinizde güvenlik ilkelerini merkezi şekilde yöneterek şirket veya yasal güvenlik gereksinimleriyle uyumluluk sağlayın.
- **Sürekli güvenlik değerlendirmesi** – Olası güvenlik sorunlarını keşfetmek için makinelerin, ağların, depolama ve veri hizmetlerinin güvenlik durumunu izleyin.
- **Eyleme dönüştürülebilir öneriler** – Önceliği belirlenmiş ve eyleme dönüştürülebilir güvenlik önerileri sayesinde saldırganlar yararlanmadan önce güvenlik açıklarını düzeltin.
- **Önceliği belirlenmiş uyarılar ve olaylar** - Önceliği belirlenmiş güvenlik uyarıları ve olaylar sayesinde öncelikle en kritik tehditlere odaklanın.
- **Gelişmiş bulut savunması** – Sanal makinelerinizde çalıştırılan yönetim bağlantı noktalarına ve uyarlamalı uygulama denetimlerine tam zamanında erişim sayesinde tehditleri azaltın.
- **Tümleşik güvenlik çözümleri** - Bağlantı halindeki iş ortağı çözümleri gibi farklı kaynaklardan güvenlik verileri toplayın, bunlar üzerinde arama ve analiz gerçekleştirin.

**Güvenlik Merkezi - Genel Bakış**, Azure ve Azure olmayan iş yüklerinizin güvenlik durumuna yönelik hızlı bir görünüm sağlayarak iş yüklerinizin güvenliğini keşfedip değerlendirmenize ve riski belirleyip azaltmanıza olanak verir. Yerleşik pano, dikkat gerektiren güvenlik uyarılarına ve güvenlik açıklarına yönelik anında öngörüler sağlar.

![Genel Bakış][1]

## <a name="centralized-policy-management"></a>Merkezi ilke yönetimi
**İlke ve uyumluluk** bölümünde (yukarıda gösterilmiştir), abonelik kapsamınız, ilke uyumluluğunuz ve güvenlik duruşunuzla ilgili hızlı bilgiler sağlar.

### <a name="subscription-coverage"></a>Abonelik kapsamı
Bu bölümde, erişimine (okuma veya yazma) sahip olduğunuz toplam abonelik sayısı ve bir aboneliğin çalıştırıldığı Güvenlik Merkezi kapsamı düzeyi (Standart veya Ücretsiz) görüntülenmektedir:

- **Kapsanan (Standart)** – Kapsanan abonelikler, Güvenlik Merkezi tarafından sunulan maksimum koruma düzeyi kapsamında çalıştırılmaktadır
- **Kapsanan (Ücretsiz)** – Kapsanan abonelikler, Güvenlik Merkezi tarafından sunulan sınırlı, ücretsiz koruma düzeyi kapsamında çalıştırılmaktadır
- **Kapsanmayan** – Bu durumdaki abonelikler, Güvenlik Merkezi tarafından izlenmez

Grafik seçildiğinde **Kapsam** penceresi açılır. Bir sekme (**Kapsanmayan**, **Temel kapsam** veya **Standart kapsam**) seçildiğinde her bir durumda aboneliklerin listesi sağlanır. Sekmelerin birinde bir abonelik seçildiğinde, abonelikle ilgili ek bilgi sağlanır. Bu bilgiler, Güvenlik Merkezi’ni etkinleştirmek veya abonelik kapsamını artırmak için bir abonelik sahibini belirlemenize ve onunla iletişim kurmanıza olanak sağlar.

![Güvenlik Merkezi kapsamı][9]

### <a name="policy-compliance"></a>İlke uyumluluğu
İlke uyumluluğu, atanan tüm ilkelerin uyumluluk faktörleri tarafından belirlenir. Bir yönetim grubu, abonelik veya çalışma alanı için genel uyumluluk puanı, atamaların ağırlıklı ortalamasıdır. Ağırlıklı ortalama, tek bir atamadaki ilke sayısını ve atamanın uygulandığı kaynak sayısını etkiler.

Örneğin aboneliğinizde iki sanal makine ve atanmış beş ilkeli bir girişim varsa aboneliğinizde on değerlendirme olur. Sanal makinelerin biri ilkelerin ikisiyle uyumlu değilse abonelik atamanızın genel uyumluluk puanı %80 olur.

Bu bölümde, genel uyumluluk oranınız ve en az uyumlu abonelikleriniz görüntülenir. **Ortamınızın ilke uyumluluğunu gösterin** seçeneği belirlendiğinde **İlke Yönetimi** penceresi açılır. **İlke Yönetimi** bölümünde, yönetim gruplarının, aboneliklerin ve çalışma alanlarının hiyerarşik yapısı görüntülenir. Burada bir abonelik veya yönetim grubu seçerek güvenlik ilkelerinizi yönetirsiniz.

![İlke yönetimi][10]

Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Güvenlik Merkezi’nde, ilkeleri tanımlar ve Güvenlik Merkezi’nin hangi denetimleri izleyip önerdiğini belirleyerek bunları iş yükünüzün türüne veya verilerinizin duyarlılığına göre uyarlarsınız. Bir yönetim grubuna veya aboneliğe tıklayarak Güvenlik Merkezi’nde güvenlik ilkesini düzenleyebilirsiniz. Yeni tanımlar oluşturmak, ek ilkeler tanımlamak ve yönetim grupları genelinde ilkeler atamak için de Azure İlkesi’ni kullanabilirsiniz.

Abonelik, yönetim grubu, kaynak grubu veya çalışma alanı düzeyinde aşağıdaki Güvenlik Merkezi ayarlarını düzenlemek için **Ayarları düzenle >** seçeneğini belirleyin:

- **Veri toplama**: Aracı sağlama ve güvenlik için [veri toplama](security-center-enable-data-collection.md) ayarlarını belirler.
- **E-posta bildirimleri**: Güvenlik ilgili kişilerini ve [e-posta bildirimi](security-center-provide-security-contact-details.md) ayarlarını belirler.
- **Fiyatlandırma katmanı**: Ücretsiz veya Standart [fiyatlandırma seçimini](security-center-pricing.md) tanımlar. Seçtiğiniz katman, kapsam dahilindeki kaynaklar için hangi Güvenlik Merkezi özelliklerinin kullanılabilir olduğunu belirler.
- **Güvenlik yapılandırmalarını düzenle**: Olası güvenlik açıklarını belirlemek için Güvenlik Merkezi tarafından değerlendirilen işletim sistemi yapılandırmalarını görüntülemenize ve değiştirmenize olanak sağlar.

Daha fazla bilgi için bkz. [Azure İlkesi ile Güvenlik Merkezi güvenlik ilkelerini tümleştirme](security-center-azure-policy.md).

### <a name="manage-and-govern-your-security-posture"></a>Güvenlik duruşunuzu yönetme ve düzenleme
Panonun sağ tarafında **İlke ve uyumluluk** bölümünde size genel güvenlik duruşunuzu iyileştirmek için hemen harekete geçebileceğiniz öngörüler sağlanır. Örnekler şunlardır:

- Güvenlik standartlarına uyumluluğu gözden geçirmek ve izlemek için Güvenlik Merkezi ilkelerini tanımlama ve atama
- Güvenlik Merkezi güvenlik uyarılarını SIEM bağlayıcı için kullanılabilir yapma
- Zaman içindeki ilke uyumluluğu

## <a name="continuous-security-assessment"></a>Sürekli güvenlik değerlendirmesi
**Güvenlik Merkezi - Genel Bakış** bölümündeki Kaynak güvenliği durumu kısmında, kaynaklarınızın güvenlik durumunun hızlı bir görünümü sunularak her bir kaynak türü için güvenlik durumu ve belirlenen sorun sayısı görüntülenir. Sürekli değerlendirme, güvenlik güncelleştirmeleri eksik olan sistemler veya güvenlik açığı olan ağ bağlantı noktaları gibi olası güvenlik sorunlarını keşfetmenize yardımcı olur.

### <a name="secure-score"></a>Güvenlik puanı
Azure Güvenlik Merkezi güvenli puanı, güvenlik önerilerinizi gözden geçirir ve bunları sizin için önceliklendirir; böylece önce hangi önerilerin gerçekleştirileceğini bilirsiniz ve araştırmayı önceliklendirebilmeniz için en ciddi güvenlik açıklarını bulmanız kolaylaşır. Güvenli puan, güvenli bir iş yükü elde etmek için güvenliğinizi güçlendirmenize yardımcı olan bir ölçüm aracıdır. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi’nde güvenli puan](security-center-secure-score.md).

### <a name="health-monitoring"></a>Sistem durumunu izleme
**Kaynak durumu izleme** bölümünden bir kaynak türü seçildiğinde, kaynakların ve belirlenen güvenlik açıklarının bir listesi sağlanır. Kaynak türleri; işlem ve uygulamalar, ağ iletişimi, veri ve depolama, ve kimlik ve erişim şeklindedir.

**İşlem ve uygulamalar**’ı seçtik. **İşlem** bölümünde dört sekme vardır:

- **Genel Bakış**: Güvenlik Merkezi tarafından belirlenen izleme ve öneriler.
- **Sanal makineler ve bilgisayarlar**: Sanal makinelerinizin, bilgisayarlarınızın ve her birinin geçerli güvenlik durumunun listesi.
- **Cloud Services**: Güvenlik Merkezi tarafından izlenen web ve çalışan rollerinizin listesi.
- **Uygulama hizmetleri (Önizleme)**: Web uygulamalarınızın, Uygulama hizmeti ortamlarınızın ve her birinin geçerli güvenlik durumunun listesi.

![Güvenlik durumunu izleme][3]

Daha fazla bilgi için bkz. [Güvenlik durumu izleme](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Eyleme dönüştürülebilir öneriler
Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure ve Azure olmayan kaynaklarınızın güvenlik durumunu analiz eder. **Kaynaklar** bölümünde **Öneriler** seçildiğinde, güvenlik sorunlarına yanıt verme süreci boyunca size yol gösteren, önceliği belirlenmiş güvenlik önerileri listesi sağlanır.

![Öneriler][4]

Daha fazla bilgi için bkz. [Güvenlik önerilerini yönetme](security-center-recommendations.md).

### <a name="most-prevalent-recommendations"></a>En yaygın öneriler
Panonun sağ tarafındaki **Kaynaklar** bölümünde, size en çok sayıda kaynak için mevcut olan en yaygın önerilerin bir listesi sağlanır. En yaygın öneriler, odaklanmanız gereken yerleri vurgular. Sağ ok seçildiğinde, en yüksek etkili öneri sağlanır.

![En yaygın öneriler][11]

Bu, ortamınızda bulunan en etkili tek öneridir. Bu öneri çözümlendiğinde en çok uyumluluğunuz iyileştirilir. Bu örnekte öneri, “disk şifrelemesi uygulama” şeklindedir. **Uyumluluğunuzu artırın** seçeneği belirlendiğinde, önerinin bir açıklaması ve etkilenen kaynakların listesi sağlanır.

![Disk şifrelemesi uygulayın][12]

## <a name="threat-protection"></a>Tehdit koruması
Bu alan, kaynaklarınızda algılanan güvenlik uyarılarına ve bu uyarıların önem düzeyine yönelik görünürlük sağlar.

### <a name="prioritized-alerts-and-incidents"></a>Önceliği belirlenmiş uyarılar ve olaylar
Güvenlik Merkezi, gelen saldırıları ve ihlal sonrası etkinliği algılamak için gelişmiş analiz ve genel tehdit bilgilerini kullanır. Uyarıların öncelikleri belirlenir ve sonra uyarılar, olaylar halinde gruplanır. Böylece en kritik tehditlere en önce odaklanmanız sağlanır. Kendi özel güvenlik uyarılarınızı da oluşturabilirsiniz.

**Önem derecesine göre güvenlik uyarıları** veya **Zaman içindeki güvenlik uyarıları** seçeneği belirlendiğinde, uyarılarla ilgili ayrıntılı bilgi sağlanır.

![Önceliği belirlenmiş uyarılar ve olaylar][7]

Görsel ve etkileşimli bir araştırma deneyiminin kapsamını ve etkisini hızlı şekilde değerlendirebilir ve güvenlik verilerinin daha ayrıntılı keşfi için önceden tanımlanmış veya geçici sorgular kullanabilirsiniz.

Daha fazla bilgi için bkz. [Güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md).

Panonun sağ tarafında, önce hangi uyarıların ele alınacağını önceliklendirmenize ve genel güvenlik açıklarının nerede olduğunu anlamanıza yardımcı olacak bilgiler sağlanır:

- **En fazla saldırıya uğrayan kaynaklar**: En yüksek sayıda uyarı içeren belirli kaynaklar
- **En yaygın uyarılar**: En yüksek sayıda kaynakları etkileyen uyarı türleri

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
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
[9]: ./media/security-center-intro/coverage.png
[10]: ./media/security-center-intro/policy-management.png
[11]: ./media/security-center-intro/highest-impact.png
[12]: ./media/security-center-intro/apply-disk-encryption.png
