---
title: Güvenlik Merkezi'nin emeklilik özellikleri (Temmuz 2019) | Microsoft Docs
description: Bu makalede Güvenlik Merkezi'nde, 31 Temmuz 2019 üzerinde kullanımdan kaldırılacak özellikleri ayrıntılı olarak açıklanmaktadır.
services: security-center
author: yoavfrancis
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.date: 4/16/2019
ms.author: yoafr
ms.openlocfilehash: 392782310d8bc3b38a3dd1349cb1760ca287acd1
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875580"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Güvenlik Merkezi özelliklerini (Temmuz 2019) devre dışı bırakma

Yaptık birkaç [geliştirmeleri](https://azure.microsoft.com/updates/?product=security-center) son 6 ay içindeki Azure Güvenlik Merkezi'ne.  
Gelişmiş özellikleri ile 31 Temmuz 2019 tarihinde Güvenlik Merkezi'nden yedekli özelliklerin yanı sıra ilgili API'leri, bir dizi kaldırıyoruz.  

Devre dışı bırakılan özelliklerin çoğu, Azure Güvenlik Merkezi veya Log Analytics yeni özelliklerle değiştirilebilir; ve bazı özellikler kullanılarak uygulanabilir [(Önizleme) Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/), duyurulan.

Güvenlik Merkezi'nden kaldırılan özelliklerin listesi içerir:

- [Olaylar Panosu](#menu_events)
- [Arama menüsü girişi](#menu_search)
- [Görünüm Klasik kimlik ve erişim bağlantısı kimlik ve erişim (Önizleme)](#menu_classicidentity)
- [Güvenlik olayları Eşle düğmesi güvenlik uyarıları harita (Önizleme)](#menu_securityeventsmap)
- [Özel uyarı kuralları (Önizleme)](#menu_customalerts)
- [Tehdit koruması güvenlik uyarıları düğmesini araştırın](#menu_investigate)
- [Güvenlik çözümlerinin bir alt kümesi](#menu_solutions)
- [Güvenlik ilkeleri için güvenlik yapılandırmalarını Düzenle](#menu_securityconfigurations)
- [(OMS Portalı'nda ilk olarak kullanılır) güvenlik ve Denetim Panosu Log Analytics çalışma alanları için.](#menu_securityomsdashboard)

Her devre dışı bırakılan özellik ve değiştirme özelliklerini kullanarak gerçekleştirebileceğiniz adımlar aşağıda ayrıntılı bilgileri sağlar.

## Olaylar Panosu<a name="menu_events"></a>
Güvenlik Merkezi kullanan çeşitli güvenlik toplamak için Microsoft Monitoring Agent, makinelerinizden yapılandırmaları ve olayları ilgili ve bu olayları, çalışma Alanlarınızda depolanır. [Olaylar Panosu](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) bu verileri görüntülemeyi sağlar ve temelde Log Analytics'e başka bir giriş noktası sağlar.

Olaylar Panosu ileride kullanımdan kaldırılacak:

![Olaylar çalışma alanı seçimi ekran][1]

Bir çalışma alanında, bir kullanıcı tıklattıktan sonra görüntülenen olaylar panosu da kullanımdan kaldırılacak:

![Olaylar panosu][2]

### <a name="events-dashboard---new-experience"></a>Olaylar Panosu - yeni deneyim

Müşterilerin kendi çalışma alanlarında üzerinde önemli olayları görüntülemek için Log Analytics yerel özellikleri kullanmak için önerilir.
Özel önemli olay Güvenlik Merkezi'nden oluşturduysanız, bunlar günlüğü erişilebilir olacaktır analytics seçin -> çalışma alanı -> kayıtlı aramalar. Verilerinizi kayıp değiştirilmiş veya değil. Yerel önemli olayları da aynı ekranından kullanılabilir.

![Çalışma alanı kayıtlı aramalar][3]

## Arama menüsü girişi<a name="menu_search"></a>
Azure Güvenlik Merkezi, Azure İzleyici günlüklerine arama şu anda almak ve güvenlik verilerinizi analiz etmek için kullanır. Log Analytics için bir cephe gibi bu ekranı temel olarak hizmet veren "[arama](https://docs.microsoft.com/azure/security-center/security-center-search)" sayfası, seçilen çalışma alanına arama sorguları izin vererek –. Bu arama penceresi ileride kullanımdan kaldırılacak:

![Arama sayfası][4]

### <a name="search-menu-entry---new-experience"></a>Arama menüsü girişi - yeni deneyim

Müşteriler kullanmaları **Log Analytics** kendi çalışma alanlarını temel arama sorguları gerçekleştirmek için yerel özellikleri. Bunu yapmak için Azure Log analytics'te gidin ve seçin: "Günlükleri":

![Log Analytics günlükleri sayfasında][5]

## Klasik kimlik ve erişim (Önizleme)<a name="menu_classicidentity"></a>
Güvenlik Merkezi'nde "Klasik" kimlik ve erişim deneyimi, müşterilerin kimliklerini görüntülemek ve log analytics'te ilgili bilgilere erişmek bir yol sağlanan. Bu, aşağıdaki tıklama izleyerek yapıldı:

"View Klasik kimlik ve erişim üzerinde" tıklayın

![Kimlik sayfası][6]

Şu sayfayı açın, ekranla birlikte "kimlik ve erişim Panosu":

![Kimlik sayfası - çalışma alanı seçimi][7]

Çalışma alanını tıklayarak burada müşteri kimlik görüntülemek ve bilgi kendi çalışma alanına erişim "Kimlik ve erişim" log analytics panosunu açar:

![Kimlik sayfası - Pano][8]

Yukarıdaki tüm üç ekran ileride kullanımdan kaldırılacaktır. Verilerinizi log analytics güvenlik çözümdeki kullanılabilir kalır ve değil değiştirilemiyor veya kaldırılamıyor.

### <a name="classic-identity--access-preview---new-experience"></a>Klasik kimlik ve erişim (Önizleme) - yeni deneyim
Log analytics panosunu ınsights üzerinde yalnızca belirli bir çalışma ayarının sağlarken, Yerel Güvenlik Merkezi özelliklerini tüm abonelikler ve bunlarla bir kolayca ilişkili tüm çalışma alanlarını görünürlük sağlar-sağlayan görünümü kullanmak için ne üzerinde odaklanın ait önemli güvenli puanı, kimlik ve erişim öneri göre.
Güvenlik Merkezi içinde "Kimlik ve erişim (Önizleme)"'i seçerek kimlik ve erişim Log analytics panosunu tüm özelliklerini ulaşılabilir:

![Kimlik sayfası - Klasik deneyimi devre dışı bırakma][9]

## Güvenlik olayları eşleme<a name="menu_securityeventsmap"></a>
Güvenlik Merkezi ile sağlar bir [harita](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) yardımcı olan ortama yönelik güvenlik tehditlerini belirleyebilir. Eşlenen 'güvenlik olayları harita için Git' düğmesini seçili çalışma alanı ham güvenlik olaylarını görüntülemek için sağlayan bir Pano neden olur.
Çalışma alanı başına panonun yanı sıra düğmenin sonra kullanımdan kaldırılacak.

![Güvenlik uyarılar Haritası - düğme][10]

Bugün "Güvenlik olaylarını harita Git" tıkladığınızda tehdit zekası panosu açılır. Tehdit zekası panosunu kullanımdan kaldırılacaktır.  

![Tehdit bilgileri panosu][11]

Tehdit zekası panosunu görüntülemek istediğiniz çalışma alanını seçtiğinizde map(Preview) ekran güvenlik uyarıları *Log analytics'te* açılır. Bu ekran kullanımdan kaldırılacaktır.

![Log analytics'te güvenlik uyarıları eşleme][12]

Mevcut verilerinizi log analytics güvenlik çözümdeki kullanılabilir kalır ve değil değiştirilmiş veya kaldırılamaz.

### <a name="security-events-map---new-experience"></a>Güvenlik olayları eşleme - yeni deneyim
Güvenlik Merkezi ile oluşturulan uyarılar Haritası işlevini kullanmak için müşterilerimizin öneririz: "güvenlik uyarıları harita (Önizleme)". Bu en iyi duruma getirilmiş bir deneyim sağlar ve tüm abonelikler ve ilişkili çalışma alanları arasında çalışır bir makro izin vererek ortamınız genelinde görüntülemek ve tek bir çalışma alanında odaklanmak değil.

## Özel uyarı kuralları (Önizleme)<a name="menu_customalerts"></a>
Özel uyarı deneyimi olacaktır [kullanımdan](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) 30 Haziran 2019 altyapının kullanımdan kaldırma nedeniyle, üzerine kurulmuştur. Zaman çerçevesinde kullanımdan kaldırma kadar kullanıcılar var olan özel uyarı kuralları düzenlemek mümkün olacaktır, ancak yenilerini eklemek mümkün olmayacaktır. Kullanıcılar tavsiye etkinleştirmek için [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/) günlük uyarıları ile otomatik olarak mevcut uyarılarını geçirme ve yenilerini oluşturun veya alternatif olarak Azure İzleyici ile uyarılarını yeniden oluşturmak için tek tıklamayla ekleme.

Mevcut uyarılarınızı tutmak ve bunları Azure Gözcü için geçirmek için lütfen Azure Gözcü başlatın. İlk adımı olarak özel uyarıları depolandığı çalışma alanını seçin ve sonra uyarıları otomatik olarak geçirilecek 'Analytics' menü öğesini seçin.

![Özel uyarılar][13]

Müşterilerin Azure Gözcü ekleme İlgilenmiyor uyarılarını Azure İzleyici günlük uyarıları ile yeniden oluşturmanız önerilir. Yönergeler için bkz: [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak günlük uyarıları yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log). Günlük uyarıları oluşturma hakkında yönergeler için bkz: [Azure İzleyici'de günlük uyarıları](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

Özel uyarılar devre dışı bırakma hakkında daha fazla bilgi için lütfen bkz [özel Güvenlik Merkezi uyarıları belgeleri](https://docs.microsoft.com/azure/security-center/security-center-custom-alert).

## Güvenlik Uyarıları araştırma<a name="menu_investigate"></a>
[Araştırma özelliği](https://docs.microsoft.com/azure/security-center/security-center-investigation) Güvenlik Merkezi'nde önceliklendirme, kapsamını anlamanızı ve olası bir güvenlik olayı kök nedenini izlemenize olanak tanır. Bu özellik Gelişmiş bir deneyim ile değiştirilmiştir gibi güvenlik Merkezi'nden kaldırılacak [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/).

![Güvenlik olayı][14]

Yukarıdaki "Araştır" düğmesine tıkladığınızda, "araştırma Panosu (Önizleme)" Log analytics'te açılır. Araştırma Panosu kullanımdan kaldırılacaktır.  
Mevcut verilerinizi Log Analytics'e güvenlik çözümdeki kullanılabilir kalır ve değil değiştirilmiş veya kaldırılamaz.

![Log analytics'te araştırma Panosu][15]

### <a name="investigation---new-experience"></a>Araştırma - yeni deneyim

Yerleşik müşterilerimize öneriyoruz [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/) zengin araştırma deneyimi için desteklenen özellik avcılık göre uyarıları. Azure Sentinel güvenlik tehditleri için kuruluşunuzun veri kaynaklarında hunt için güçlü avcılık arama ve sorgulama araçları sağlar.  

## Güvenlik çözümlerinin alt kümesi<a name="menu_solutions"></a>

Güvenlik Merkezi'nde bulunan sağlama yeteneği [tümleşik güvenlik çözümleri azure'da](https://docs.microsoft.com/azure/security-center/security-center-partner-integration). Aşağıdaki iş ortağı çözümlerini kaldırıldı ve desteklenen [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/), birlikte daha fazla veri kaynakları.

- Yeni nesil güvenlik duvarı ve Web uygulaması güvenlik duvarı çözümleri (Azure Gözcü [belgeleri](https://docs.microsoft.com/azure/sentinel/connect-data-sources))
- Ortak olay biçimi (CEF) destekleyen güvenlik çözümlerini tümleştirme (Azure Gözcü [belgeleri](https://docs.microsoft.com/azure/sentinel/connect-common-event-format))
- Microsoft Advanced Threat Analytics (Azure Sentinel [belgeleri](https://docs.microsoft.com/azure/sentinel/connect-azure-atp))
- Azure AD kimlik koruması (Azure Sentinel [belgeleri](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection))

Kullanımdan kaldırma, kullanıcıların yeni eklemek veya mevcut bağlantılı çözümler hem kullanıcı Arabirimi hem de API, yukarıda belirtilen türlerin değiştirmek mümkün olmayacaktır.
Mevcut bağlantılı çözümler Azure Gözcü için bu dönem sonunda taşımanız önerilir.

![Güvenlik çözümleri Merkezi][16]

## Güvenlik ilkeleri için güvenlik yapılandırmalarını Düzenle<a name="menu_securityconfigurations"></a>
Azure Güvenlik Merkezi'nin izlediği güvenlik yapılandırmalarını bir dizi uygulayarak [150'den önerilen kurallar](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) işletim sistemini sağlamlaştırma için kuralları dahil olmak üzere ilgili güvenlik duvarları, Denetim, parola ilkeleri ve daha fazla bilgi için. Güvenlik açığı bulunan bir yapılandırmaya sahip bir makine bulundu, Güvenlik Merkezi bir güvenlik önerisi oluşturur. [Düzenleme güvenlik yapılandırma ekranında](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) Güvenlik Merkezi'nde varsayılan işletim sistemi güvenlik yapılandırması özelleştirme yapmasını sağlar.

Bu özellik Önizleme aşamasında olan ve kullanımdan kaldırılacaktır.

![Güvenlik yapılandırmalarını düzenle][17]

### <a name="edit-security-configurations---new-experience"></a>Güvenlik yapılandırmalarını - yeni deneyim Düzenle

Güvenlik Merkezi tarafından destekleneceğinden [Konuk Aracısı](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) yakın gelecekte, çok daha zengin bir özellik kümesi - ek işletim sistemleri ve Konuk yapılandırmasıyla Azure ilkeleri (konuk tümleştirme desteği dahil olmak üzere izin verme ilkeleri). Bu, ayrıca ölçekli olarak denetleyen ve yeni kaynaklara otomatik olarak uygulama olanağı sağlayacaktır.

## Güvenlik ve Denetim Panosu (ilk olarak kullanılır. OMS portalında) Log Analytics çalışma alanları için<a name="menu_securityomsdashboard"></a>

Log analytics'te güvenlik Panosu, önemli güvenlik olayları ve tehditler, tehdit bilgileri Haritası ve güvenlik olaylarını çalışma alanına kaydedilir kimlik ve erişim değerlendirmesinin bir çalışma alanı başına genel bakış sağlar. İleride Pano kaldırılacak. UI Panoda zaten önerilir gibi kullanıcılarımızın bundan sonra Azure Güvenlik Merkezi'ni kullanmayı önerilir.

![Log analytics security Panosu][18]

### <a name="security--audit-dashboard---new-experience"></a>Güvenlik ve Denetim Panosu - yeni deneyim
Birden çok aboneliğe ve bunlarla ilişkili daha zengin bir özellik kümesi ile birlikte çalışma alanları arasında aynı güvenliğine genel bakış sağlayan Azure Güvenlik Merkezi'ni kullanmayı müşterilerimizin önerilir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/)
- Daha fazla bilgi edinin [Azure Gözcü](https://docs.microsoft.com/azure/sentinel)

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
