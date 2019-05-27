---
title: Güvenlik Merkezi'nin emeklilik özellikleri (Temmuz 2019) | Microsoft Docs
description: Bu makalede Güvenlik Merkezi'nde, 31 Temmuz 2019 üzerinde kullanımdan kaldırılacak özellikleri açıklar.
services: security-center
author: yoavfrancis
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.date: 4/16/2019
ms.author: yoafr
ms.openlocfilehash: 069345f9c2d0fff0b580365153d8be13bb4ba204
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65952146"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Güvenlik Merkezi özelliklerini (Temmuz 2019) devre dışı bırakma

Yaptığımız birkaç [geliştirmeleri](https://azure.microsoft.com/updates/?product=security-center) Azure Güvenlik Merkezi'ne son altı ay içinde.
Bu gelişmiş özelliklerle, biz bazı yedekli özellikler ve ilgili API'leri güvenlik Merkezi'nden 31 Temmuz 2019 üzerinde kaldırırsınız.  

Retiring bu özelliklerin çoğu, Azure Güvenlik Merkezi ya da Azure Log Analytics yeni işlevleriyle değiştirilebilir. Diğer özellikler kullanılarak uygulanabilir [(Önizleme) Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/).

Güvenlik Merkezi özelliklerini yakında kullanımdan kalkacak şunlardır:

- [Olaylar Panosu](#menu_events)
- [Arama menüsü girişi](#menu_search)
- [Görünüm Klasik kimlik ve erişim bağlantısı kimlik ve erişim (Önizleme)](#menu_classicidentity)
- [Güvenlik olayları Eşle düğmesi güvenlik uyarıları harita (Önizleme)](#menu_securityeventsmap)
- [Özel uyarı kuralları (Önizleme)](#menu_customalerts)
- [Tehdit koruması güvenlik uyarıları düğmesini araştırın](#menu_investigate)
- [Güvenlik çözümlerinin bir alt kümesi](#menu_solutions)
- [Güvenlik ilkeleri için güvenlik yapılandırmalarını Düzenle](#menu_securityconfigurations)
- [Güvenlik ve Denetim Panosu (ilk olarak kullanılır. OMS portalında) Log Analytics çalışma alanları için](#menu_securityomsdashboard)

Bu makalede ayrıntılı bilgi ikame özellikler uygulamak için atabileceğiniz her devre dışı bırakılan özellik ve adımları sağlar.

## Olaylar Panosu<a name="menu_events"></a>

Güvenlik Merkezi, makinelerinizden güvenlik-ilgili çeşitli yapılandırmaları ve olayları toplamak için Microsoft İzleme Aracısı kullanır. Bunu, bu olayları, çalışma alanlarında depolar. [Olaylar Panosu](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) Log Analytics'e giriş noktası sağlar ve bu verileri görmenizi sağlar.

Bir çalışma alanı seçtiğinizde görünen olaylar Pano emekli ediyoruz:

![Olaylar panosu][2]

### <a name="events-dashboard---the-new-experience"></a>Olaylar Panosu - yeni deneyim

Üzerinde çalışma alanlarınızı önemli olayları görüntülemek için Azure Log Analytics yerel özelliklerini kullanmak için teşvik ediyoruz.

Güvenlik Merkezi'nde özel önemli olay oluşturduysanız, bunlar erişilebilir olması. Log Analytics'te Git **çalışma alanı seçin** > **kayıtlı aramalar**. Verileriniz kaybolur veya olmaz. Yerel önemli olayları, Log analytics'te aynı ekranından de mevcuttur.

![Çalışma alanı kayıtlı aramalar][3]

## Arama menüsü girişi<a name="menu_search"></a>

Azure Güvenlik Merkezi, Azure İzleyici günlüklerine arama şu anda almak ve güvenlik verilerinizi analiz etmek için kullanır. Bu ekran, Log Analytics arama sayfası için bir pencere görür ve seçilen çalışma alanına arama sorguları çalıştırmak kullanıcıların sağlar. Daha fazla bilgi için [Azure Güvenlik Merkezi arama](https://docs.microsoft.com/azure/security-center/security-center-search). Biz bu arama penceresi emekli:

![Arama sayfası][4]

### <a name="search-menu-entry---the-new-experience"></a>Arama menüsü girişi - yeni deneyim

Çalışma alanlarınızı üzerinde arama sorguları gerçekleştirmek için Azure Log Analytics yerel özellikleri kullanmanızı öneririz. Azure Log Analytics'e gidip seçin **günlükleri**.

![Log Analytics günlükleri sayfasında][5]

## Klasik kimlik ve erişim (Önizleme)<a name="menu_classicidentity"></a>

Güvenlik Merkezi'nde Klasik kimlik ve erişim deneyimi, Log Analytics'te şu anda bir pano, kimlik ve erişim bilgileri gösterir. Bu panoyu görüntülemek için:

1. Seçin **Klasik kimlik ve erişim görüntülemek**.

   ![Kimlik sayfası][6]

1. Görünüm **kimlik ve erişim Panosu**.

    ![Kimlik sayfası - çalışma alanı seçimi][7]

1. Açmak için bir çalışma alanı seçin **kimlik ve erişim** kimlik görüntülemek ve çalışma alanınızda bilgilere erişmek için Log analytics'te Pano.

   ![Kimlik sayfası - Pano][8]

Önceki adımlarda gösterilen tüm üç ekran emekli ediyoruz. Verilerinizi Log Analytics'e güvenlik çözümdeki kullanılabilir kalır ve olmaz değiştirilemiyor veya kaldırılamıyor.

### <a name="classic-identity--access-preview---the-new-experience"></a>Klasik kimlik ve erişim (Önizleme) - yeni deneyim

Log Analytics Panosu, tek bir çalışma alanında ınsights göstermiştir. Ancak, Yerel Güvenlik Merkezi özelliklerini tüm abonelikler ve bunlarla ilişkilendirilmiş tüm çalışma alanlarını görünürlük sağlar. Bir kolayca erişebilirsiniz-olanak sağlayan bir görünüm kullanmak üzere güvenli, puan göre sıralanmış önerilerle birlikte önemli şeylere.

Tüm özelliklerini **kimlik ve erişim** Log analytics'te Pano seçerek ulaşılabilir **kimlik ve erişim (Önizleme)** Güvenlik Merkezi'ni içinde.

![Kimlik sayfası - Klasik deneyimi devre dışı bırakma][9]

## Güvenlik olayları eşleme<a name="menu_securityeventsmap"></a>

Güvenlik Merkezi ile sağlar bir [güvenlik uyarıları harita](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) güvenlik tehditleri belirlemenize yardımcı olması için. **Güvenlik olayları eşlemesine Git** eşlenen düğmesi seçili çalışma alanı ham güvenlik olaylarını görüntüleme olanak tanıyan bir Pano açılır.

Biz kaldırma **güvenlik olayları eşlemesine Git** düğmesi ve çalışma alanı başına Pano.

![Güvenlik uyarılar Haritası - düğme][10]

Seçtiğinizde, **güvenlik olayları eşlemesine Git** düğmesi, tehdit zekası panosunu açın. Tehdit zekası panosunu emekli ediyoruz.  

![Tehdit Zekası panosu][11]

Tehdit zekası panosunu görüntülemek istediğiniz çalışma alanını seçtiğinizde, Log Analytics'te güvenlik uyarıları (Önizleme) map ekranı açın. Biz bu ekranı emekli.

![Log analytics'te güvenlik uyarıları eşleme][12]

Mevcut verilerinizi Log Analytics'e güvenlik çözümdeki kullanılabilir kalır ve olmaz değiştirilmiş veya kaldırılamaz.

### <a name="security-events-map---the-new-experience"></a>Güvenlik olayları eşleme - yeni deneyim

Güvenlik Merkezi ile oluşturulan uyarılar Haritası işlevini kullanmanızı öneririz: **Güvenlik Uyarıları harita (Önizleme)**. Bu işlev, iyileştirilmiş bir deneyim sağlar ve tüm abonelikleri ve ilişkili çalışma alanları arasında çalışır. Ortamınız genelinde üst düzey bir görünüm sağlar ve tek bir çalışma alanına odaklı değildir.

## Özel uyarı kuralları (Önizleme)<a name="menu_customalerts"></a>

Duyuyoruz [özel devre dışı bırakma uyarı deneyimi](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) 30 Haziran 2019 tarihinde olduğundan, temel alınan altyapıyla devre dışı bırakılıyor. O zamana kadar mevcut özel uyarı kuralları düzenleyebilirsiniz ancak yenilerini eklemek mümkün değildir. Etkinleştirmenizi öneririz [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/) otomatik olarak mevcut uyarılarınızı geçirme ve yenilerini oluşturun. Alternatif olarak, Azure İzleyici ile günlük uyarıları uyarılarınızı da oluşturabilirsiniz.

Mevcut uyarılarınızı tutun ve bunları Azure Gözcü için geçirmek için:

1. Azure Gözcü açın ve özel uyarıları depolandığı çalışma alanını seçin.
1. Seçin **Analytics** menüsünde uyarıları otomatik olarak geçirilecek.

![Özel uyarılar][13]

Azure Gözcü için geçiş aşamasında ilgilenmezseniz uyarıları ile Azure izleyici günlüğü uyarıları oluşturma geçirmenizi öneririz. Yönergeler için [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak günlük uyarıları yönetin](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) ve [Azure İzleyici'de günlük uyarıları](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

Özel uyarılar devre dışı bırakma hakkında daha fazla bilgi için bkz. [(Önizleme) Azure Güvenlik Merkezi'nde özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert).

## Güvenlik Uyarıları araştırma<a name="menu_investigate"></a>

[Araştırma özelliği](https://docs.microsoft.com/azure/security-center/security-center-investigation) olası bir güvenlik olayı önceliklendirilecek Güvenlik Merkezi yardımcı olur. Bu özellik bir olay anlamak ve sorunun kök nedenini izlemenize olanak sağlar. Bu gelişmiş bir deneyim ile değiştirilmiş çünkü Biz bu özellik Güvenlik Merkezi'nden kaldıracağınız [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/).

![Güvenlik olayı][14]

Seçtiğinizde, **Araştır** düğmesine bir **güvenlik olayı** ekran, Log Analytics'te araştırma Panosu (Önizleme) açın. Araştırma Panosu emekli ediyoruz.  

Mevcut verilerinizi Log Analytics'e güvenlik çözümdeki kullanılabilir kalır ve olmaz değiştirilmiş veya kaldırılamaz.

![Log analytics'te araştırma Panosu][15]

### <a name="investigation---the-new-experience"></a>Araştırma - yeni deneyim

Geçiş için öneriyoruz [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/) zengin araştırma deneyimi için. Azure Sentinel güvenlik tehditleri için kuruluşunuzun veri kaynaklarında hunt için güçlü arama ve sorgulama araçları sunar.  

## Güvenlik çözümlerinin alt kümesi<a name="menu_solutions"></a>

Güvenlik Merkezi'ni etkinleştirebilirsiniz [tümleşik güvenlik çözümleri azure'da](https://docs.microsoft.com/azure/security-center/security-center-partner-integration). Biz aşağıdaki iş ortağı çözümlerinden Güvenlik Merkezi'nden emekli. Bu çözümler etkinleştirilmiş olan [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/) birkaç ek veri kaynaklarının yanı sıra.

- [Sonraki nesil güvenlik duvarı ve web uygulaması güvenlik duvarı çözümleri](https://docs.microsoft.com/azure/sentinel/connect-data-sources)
- [Common Event Format (CEF) destekleyen güvenlik çözümlerini tümleştirme](https://docs.microsoft.com/azure/sentinel/connect-common-event-format)
- [Microsoft Advanced Threat Analytics](https://docs.microsoft.com/azure/sentinel/connect-azure-atp)
- [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection)

Kullanımdan kaldırma eklemek veya yukarıdaki listede, kullanıcı Arabiriminden ya da API ya da belirtilen çözüm türlerinden herhangi birini değiştirmek mümkün olmayacaktır.

Mevcut bağlantılı çözümler varsa, Azure Gözcü için taşıma geçirmenizi öneririz.

![Güvenlik çözümleri Merkezi][16]

## Güvenlik ilkeleri için güvenlik yapılandırmalarını Düzenle<a name="menu_securityconfigurations"></a>

Azure Güvenlik Merkezi'nin izlediği güvenlik yapılandırmalarını bir dizi uygulayarak [150'den önerilen kurallar](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). işletim sistemini sağlamlaştırma için. Bu kurallar, güvenlik duvarları, Denetim, parola ilkeleri ve daha fazla bilgi için ilgilidir. Bir makinenin yapılandırmasında güvenlik açığı varsa Güvenlik Merkezi bu konuyla ilgili bir güvenlik önerisi oluşturur. [Düzenleme güvenlik yapılandırma ekranında](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) Güvenlik Merkezi'nde varsayılan işletim sistemi güvenlik yapılandırması özelleştirme yapmasını sağlar.

Biz bu önizleme özelliğini devre dışı bırakma. O tarihten sonra güvenlik yapılandırmalarını geri varsayılan değerlerine sıfırlamak istiyorsanız, bunu API veya Powershell kullanarak aracılığıyla yapabilirsiniz [yönergeleri izleyin](https://aka.ms/ascresetsecurityconfigurations)

![Güvenlik yapılandırmalarını düzenle][17]

### <a name="edit-security-configurations---the-new-experience"></a>Güvenlik yapılandırmalarını - yeni deneyime Düzenle

Desteklemek Güvenlik Merkezi'ni etkinleştirmeyi düşündüğünüz [Konuk yapılandırma aracısı](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration). Böyle bir güncelleştirmeyi daha fazla işletim sistemi ve Konuk yapılandırmaları Azure Konuk içi ilkelerini tümleştirme desteği dahil olmak üzere çok daha zengin bir özellik kümesi sağlar. Bu değişiklikler etkinleştirildikten sonra otomatik olarak yeni kaynaklar için geçerlidir ve uygun ölçekte yapılandırmalarını kontrol olanağı da gerekir.

## Log Analytics çalışma alanları için güvenlik ve Denetim Panosu<a name="menu_securityomsdashboard"></a>

Güvenlik ve Denetim Panosu, OMS portalında ilk olarak kullanıldı. Log Analytics'te Panosu önemli güvenlik olayları ve tehditler, tehdit bilgileri Haritası ve çalışma alanına kaydedilir güvenlik olaylarını bir kimlik ve erişim değerlendirmesinin bir çalışma alanı başına genel bakış sağlar. Biz panoyu kaldırırsınız. Biz öneri zaten UI Panoda önerilir gibi Azure Güvenlik Merkezi geçer.

![Log Analytics security Panosu][18]

### <a name="security-and-audit-dashboard---the-new-experience"></a>Güvenlik ve Denetim Panosu - yeni deneyim

Biz, Azure Güvenlik Merkezi'ne geçiş yapmanızı öneriyoruz. Birden fazla aboneliği analiz aynı güvenliğine genel bakış sağlar ve çalışma alanlarını ilişkili yanı sıra daha zengin bir özellik ayarlayın.

Güvenlik ve denetim Panoda dolduran özgün Log Analytics sorguları alabilirsiniz [GitHub deposu](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards) Güvenlik Merkezi.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/).
- Daha fazla bilgi edinin [Azure Gözcü](https://docs.microsoft.com/azure/sentinel).

<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png
