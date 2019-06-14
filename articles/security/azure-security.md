---
title: Azure güvenliğine giriş | Microsoft Docs
description: Azure güvenlik, hizmetlerinin ve nasıl çalıştığı hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: ed57d72d32ba82a37036c9af77590bd4e93db8d9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60610494"
---
# <a name="introduction-to-azure-security"></a>Azure güvenliğine giriş
## <a name="overview"></a>Genel Bakış
Güvenlik bulutta iş ve nasıl önemli Azure güvenliği hakkında doğru ve güncel bilgi bulmak olduğunu olduğunu biliyoruz. Azure uygulamaları ve Hizmetleri için kullanılacak en iyi nedenlerden biri dolayısıyla kendi güvenlik araçları ve özellikleri çeşit yararlanmak sağlamaktır. Güvenli Azure platformunda güvenli çözümleri kolayca oluşturmasına olanak sağlar, bu araçları ve özellikleri yardımcı olur. Microsoft Azure, saydam sorumluluk etkinleştirirken, gizlilik, bütünlük ve kullanılabilirlik Müşteri verilerinin sağlar.

Güvenlik koleksiyonunu daha iyi anlamanıza yardımcı olması için hem müşterinin hem de Microsoft Operasyon perspektiften, bu teknik incelemede, "Giriş için Azure güvenlik", Microsoft Azure içinde uygulanan denetimleri yazılır kapsamlı bir bakış sağlar Güvenlik Microsoft Azure ile kullanılabilir.

### <a name="azure-platform"></a>Azure platformu
Azure çok sayıda işletim sistemi, dilleri, çerçeveler, Araçlar, veritabanları ve cihazlar programlama destekleyen bir genel bulut hizmeti platformudur. Linux kapsayıcılarını Docker tümleştirmesiyle çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js kullanarak uygulamalar oluşturun; arka iOS, Android ve Windows cihazları uçlar oluşturun.

Azure genel bulut Hizmetleri geliştiricilerin milyonlarca teknolojileri destekler ve BT profesyonellerinin zaten kullandığı ve güvendiği. Oluşturmak ya da BT varlıklarınızı geçirme uygulamalar ve hizmetler ve denetimleri ile verileri korumak için bir kuruluşun yeteneklerine bağlı bir genel bulut hizmet sağlayıcısı, bulut tabanlı varlıklarınızı gizliliğini yönetmenizi sağlarlar.

Azure altyapısı tesisten uygulamalara milyonlarca müşteriye aynı anda barındırmak için tasarlanmıştır ve işletmelerin güvenlik gereksinimlerine bağlı karşılayabilecek güvenilir bir temel sunar.

Ayrıca, Azure çeşit yapılandırılabilir güvenlik seçenekleri ve kuruluşunuzun dağıtımlarının benzersiz gereksinimleri karşılamak için güvenlik özelleştirebilmeniz için bunları denetleme olanağı ile sağlar. Bu belge Azure güvenlik anlamanıza yardımcı olur. özellikler, bu gereksinimleri karşılayan yardımcı olabilir.

> [!Note]
> Bu belgenin birincil odağı, özelleştirme ve uygulama ve hizmetlerinizin güvenliğini artırmak için kullanabileceğiniz müşterilere yönelik denetimler açıktır.
>
> Biz bazı genel bakış bilgileri sağlar, ancak sağlanan bilgileri nasıl Microsoft Azure platformu korur hakkında ayrıntılı bilgi için bkz. [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

### <a name="abstract"></a>Özet
Başlangıçta, genel bulut geçişleri maliyet tasarrufu ve çevikliğinden tarafından yenilik odaklı. Güvenlik süre ve hatta bir show stopper, genel buluta geçiş için önemli bir sorun olarak kabul edildi. Ancak, genel bulut güvenlik buluta geçiş sürücülerini birine önemli önemli çözümlemeye geçmiş. Bu gerekçelerini, uygulamaları korumak için üstün özelliği büyük genel bulut hizmet sağlayıcılarının ve veri varlıklarının bulut tabanlı olduğundan.

Azure altyapısı tesisten uygulamalara kadar milyonlarca müşteriye aynı anda hizmet verecek şekilde tasarlanmıştır ve işletmelerin güvenlik ihtiyaçlarını karşılayabilecek güvenilir bir temel sunar. Ayrıca, Azure bir çeşit yapılandırılabilir güvenlik seçenekleri sunar ve BT karşılamak için dağıtımlarınıza özel gereksinimleri karşılamak için güvenlik özelleştirebilmeniz için bunları denetleme olanağı denetimi ilkeleri ve uyması için dış düzenlemeler.

Bu yazıda, Microsoft Azure bulut platformu içerisindeki güvenlik Microsoft'un yaklaşımlar:
* Azure altyapı, müşteri verileri ve uygulamaları güvenli hale getirmek için Microsoft tarafından uygulanan güvenlik özellikleri.
* Azure Hizmetleri ve güvenlik özellikleri, hizmetleri ve Azure aboneliklerinizi içinde verilerinizin güvenliğini yönetmek için kullanılabilir.

## <a name="summary-azure-security-capabilities"></a>Özet Azure güvenlik özellikleri
Aşağıdaki tablo, Azure altyapısı, müşteri verilerini ve güvenli uygulamaları güvenli hale getirmek için Microsoft tarafından gerçekleştirilen güvenlik özelliklerinin kısa bir açıklama sağlayın.
### <a name="security-features-implemented-to-secure-the-azure-platform"></a>Azure platformu güvenliğini sağlamak için uygulanan güvenlik özellikleri:
Aşağıdaki listelenen özellikleri olan özelliklerini Azure platformu güvenli bir şekilde yönetilir güvencesi sağlamak için gözden geçirebilirsiniz. Bağlantılar için daha fazla Microsoft dört alanlarda müşteri güveni sorularının nasıl ele detaya verildi: Platform, gizlilik ve denetimleri, uyumluluk ve saydamlık güvenli hale getirin.


| [Güvenli Platform](https://www.microsoft.com/en-us/trustcenter/Security/default.aspx)  | [Gizlilik ve denetimleri](https://www.microsoft.com/en-us/trustcenter/Privacy/default.aspx)  |[Uyumluluk](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)   | [Saydamlık](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Güvenlik geliştirme döngüsü](https://www.microsoft.com/en-us/sdl/)iç denetimleri | [Her zaman verilerinizi yönetin](https://www.microsoft.com/en-us/trustcenter/Privacy/You-own-your-data) | [Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/default.aspx) |[Microsoft Azure hizmetlerindeki müşteri verilerini nasıl korur](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| [Zorunlu güvenlik Eğitimi, arka plan denetimleri](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx) |  [Veri konumu üzerinde denetimi](https://www.microsoft.com/en-us/trustcenter/Privacy/Where-your-data-is-located) |  [Ortak Denetimler Hub](https://www.microsoft.com/en-us/trustcenter/Common-Controls-Hub) |[Microsoft, Azure hizmetlerindeki veri konumu yönetme](https://azuredatacentermap.azurewebsites.net/)|
| [Sızma testi](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx), [izinsiz giriş algılama, DDoS](https://www.microsoft.com/en-us/trustcenter/Security/ThreatManagement), [denetimleri ve günlüğe kaydetme](https://www.microsoft.com/en-us/trustcenter/Security/AuditingAndLogging) | [Kendi koşullarınıza göre veri erişimi sağlar](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [Bulut Hizmetleri son dikkatli olmanızı denetim listesi](https://www.microsoft.com/en-us/trustcenter/Compliance/Due-Diligence-Checklist) |[Microsoft, verilerinizi hangi koşullarda erişebilecek kişileri](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [Teknoloji veri merkezi](https://www.microsoft.com/en-us/cloud-platform/global-datacenters), fiziksel güvenlik [ağ güvenliğini sağlama](https://docs.microsoft.com/azure/security/security-network-overview) | [Kanuni yaptırım yetkililerine yanıt](https://www.microsoft.com/en-us/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Hizmet, konum ve sektöre göre uyumluluk](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx) |[Microsoft Azure hizmetlerindeki müşteri verilerini nasıl korur](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx)|
|  [Güvenlik olay yanıtı](https://aka.ms/SecurityResponsepaper), [paylaşılan sorumluluk](https://aka.ms/sharedresponsibility) |[Katı gizlilik standartları](https://www.microsoft.com/en-us/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Azure Hizmetleri, saydamlık hub için sertifika gözden geçirin](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)|



### <a name="security-features-offered-by-azure-to-secure-data-and-application"></a>Veri ve uygulama güvenliğini sağlamak için Azure tarafından sunulan güvenlik özellikleri
Bulut hizmeti modeli bağlı olarak, kimin uygulamaya veya hizmete güvenliğini yönetmek için sorumlu olduğu için değişken sorumluluğu yoktur. Azure aboneliğinin dağıtılabilir iş ortağı çözümlerini ve yerleşik özellikleri aracılığıyla bu sorumlulukları Karşılama konusunda yardımcı olmak için Azure platformundaki kullanılabilen özellikleri vardır.

Yerleşik özellikleri altı (6) işlevsel alanları düzenlenmiştir: İşlemleri, uygulamaları, depolama, ağ, hesaplama ve kimlik. Özellikler ve yetenekler Azure platformunda bu altı (6) alanlarda ek ayrıntı Özet bilgiler sağlanır.

## <a name="operations"></a>İşlemler
Bu bölüm, temel özellikler güvenlik işlemleri ile ilgili ek bilgiler ve bu özellikler hakkındaki özet bilgileri sağlar.

### <a name="security-and-audit-dashboard"></a>Güvenlik ve Denetim Panosu
[Güvenlik ve denetim çözümü](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) kuruluşunuzun kapsamlı bir görünüm sağlar IT güvenlik duruşuna [yerleşik arama sorguları](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) ilgilenmenizi gerektiren önemli sorunlar için. [Güvenlik ve Denetim](https://technet.microsoft.com/library/mt484091.aspx) panodur giriş ekranına her şeyi Azure İzleyici günlüklerine güvenlik ilgili. Bu, bilgisayarların güvenlik durumunu yüksek düzeyde öngörü sağlar. Ayrıca son 24 saat, 7 gün veya herhangi bir özel zaman dilimine ait tüm olayları görüntüleme becerisine sahiptir.

Ayrıca, güvenlik ve uyumluluk için yapılandırabilirsiniz [otomatik olarak belirli eylemleri gerçekleştirme](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) zaman belirli bir olay algılandı.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) kaynaklarla çözümünüzdeki bir grup olarak çalışmanıza olanak tanır. Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Kullandığınız bir [Azure Resource Manager şablonu](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) için dağıtım ve bu şablon test, hazırlama ve üretim gibi farklı ortamlarda da çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar.

Azure Resource Manager şablon tabanlı dağıtımlar yardımcı standart güvenlik ayarlarını denetleme ve Azure'da dağıtılan çözümleri güvenliğini artırmak standartlaştırılmış şablon tabanlı dağıtımlar tümleştirilebilir. Bu, dağıtımlar sırasında yer alabilen güvenlik yapılandırması hataları riskini azaltır.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/) web geliştiricileri için genişletilebilir bir uygulama performans yönetimi (APM) hizmetidir. Application Insights ile canlı web uygulamalarınızı izleyebilir ve performans anormalliklerini otomatik olarak algıla. Bu sorunları tanılamanıza yardımcı olur ve kullanıcıların gerçekten uygulamalarınızla neler anlamak için güçlü analiz araçları içerir. Uygulamanız, test sırasında hem yayımlanan veya dağıttıktan sonra çalıştığı her zaman izler.

Application Insights grafikleri ve, örneğin gösteren tablolar, günün hangi saatlerinde size çoğu kullanıcının, uygulamanın nasıl yanıt veriyor ve ne kadar iyi bağımlı olan dış hizmetler tarafından sunulur oluşturur.

Kilitlenmeler, hata veya performans sorunları varsa, nedenini tanılamak için ayrıntılı telemetri verilerini aracılığıyla arayabilirsiniz. Ve hizmet kullanılabilirliği ve uygulamanızın performansını herhangi bir değişiklik varsa e-posta gönderir. Gizlilik, bütünlük ve kullanılabilirlik güvenlik Üçlü kullanılabilirlikle yardımcı olacağından application Insight böylece değerli güvenlik aracı haline gelir.

### <a name="azure-monitor"></a>Azure İzleyici
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) görselleştirme, sorgu, yönlendirme, uyarı, Otomatik ölçek ve Otomasyon verileri iki Azure altyapısından sunar ([etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)) ve tek tek her Azure kaynağı ([tanılama Günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)). Azure İzleyici, Azure günlükleri üretilir, güvenlikle ilgili olaylar, sizi uyarmak için kullanabilirsiniz.

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri
[Azure İzleyici günlüklerine](https://azure.microsoft.com/documentation/services/log-analytics/) – hem şirket içi hem de üçüncü taraf bulut tabanlı altyapı (AWS gibi) ek Azure kaynakları için bir BT yönetimi çözümü sağlar. Ölçüm ve günlükleri görebilmeniz tek bir yerden, tüm ortamınız için Azure İzleyici verileri doğrudan Azure İzleyici günlüklerine yönlendirilebilir.

Azure İzleyici günlüklerine araç aracılığıyla bir esnek sorgu yaklaşım girişler güvenlikle ilgili büyük miktarlarda kolayca aramanızı sağlar adli ve diğer güvenlik analizi, kullanışlı bir aracı olabilir. Ayrıca, şirket içi [güvenlik duvarınızdan ve Ara günlükleri dışarı Azure'a ve Azure İzleyici günlüklerine kullanarak analiz için kullanılabilir.](https://docs.microsoft.com/azure/log-analytics/log-analytics-proxy-firewall)

### <a name="azure-advisor"></a>Azure Advisor
[Azure Danışmanı](https://docs.microsoft.com/azure/advisor/) Azure dağıtımlarınızın iyileştirilmesine yardımcı olacak bir kişiselleştirilmiş bir bulut danışmanıdır. Kaynak yapılandırmanızı ve kullanım telemetrinizi çözümler. Ardından geliştirmenize yardımcı olacak çözümler önerir [performans](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), [güvenlik](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations), ve [yüksek kullanılabilirlik](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations) Fırsatlararanıyorçalışırken,kaynaklarınızın[azaltmaya genel Azure harcamalarınızı](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations). Azure Advisor, genel güvenlik duruşunuzu çözümleri Azure'da dağıtmak için önemli ölçüde iyileştirebilen güvenlik önerileri sağlar. Bu öneri tarafından gerçekleştirilen güvenlik analizi çekildiği [Azure Güvenlik Merkezi.](https://docs.microsoft.com/azure/security-center/security-center-intro)

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim yoluyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Ayrıca, Azure Güvenlik Merkezi ile güvenlik işlemlerini tek bir Pano sağlayarak bu yüzeyleri uyarılar ve üzerinde hemen işlem yapılmasının öneriler yardımcı olur. Genellikle, Azure Güvenlik Merkezi konsolunda tek bir tıklamayla sorunları düzeltebilir.
## <a name="applications"></a>Uygulamalar
Bölüm, bu özellikleri hakkında daha fazla uygulama güvenlik ve Özet bilgilerini temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="web-application-vulnerability-scanning"></a>Web uygulaması güvenlik açığı taraması
Güvenlik açıklarına karşı testini kullanmaya başlamanın en kolay yollarından biri, [App Service uygulaması](https://docs.microsoft.com/azure/app-service/overview) kullanmaktır [Tinfoil Security ile tümleştirme](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) tek tıklamayla güvenlik açığı uygulamanız üzerinde tarama gerçekleştirmek için. Anlaşılması kolay bir raporda test sonuçlarını görüntülemek ve adım adım yönergeler içeren her bir güvenlik açığı düzeltmeyi öğrenin.

### <a name="penetration-testing"></a>Sızma Testi
Kendi sızma testleri gerçekleştirmeniz veya başka bir tarayıcı suite veya sağlayıcısı kullanmak isterseniz, izlemeniz gereken [onay işlemi Azure sızma](https://docs.microsoft.com/azure/security/azure-security-pen-testing ) ve istenen sızma testleri gerçekleştirmeniz için önceki onay alın.

### <a name="web-application-firewall"></a>Web uygulaması güvenlik duvarı
Web uygulaması Güvenlik Duvarı (WAF) [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) SQL ekleme gibi yaygın web tabanlı saldırıları, siteler arası komut dosyası saldırıları ve oturum ele geçirme web uygulamaları korumaya yardımcı olur. Bunu tarafından belirlenen tehditlerden koruma ile önceden yapılandırılmış olarak gelir [açık Web uygulaması güvenlik Project (OWASP) ilk 10 yaygın güvenlik açıklarına olarak](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Azure Uygulama Hizmeti’nde kimlik doğrulaması ve yetkilendirme
[App Service kimlik doğrulaması / yetkilendirme](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization) uygulamanız, böylece uygulama arka ucu kodunu değiştirmeniz gerekmez kullanıcılarının oturumunu açmak için bir yol sağlayan bir özelliktir. Uygulamanızı korumak ve kullanıcı başına verilerle çalışmak için kolay bir yol sunar.

### <a name="layered-security-architecture"></a>Katmanlı güvenlik mimarisi
Bu yana [App Service ortamları](https://docs.microsoft.com/azure/app-service/environment/app-service-app-service-environment-intro) içinde dağıtılan bir yalıtılmış bir çalışma zamanı ortamı sağlayan bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), geliştiriciler, farklı düzeylerde sağlama katmanlı güvenlik mimarisi oluşturabilirsiniz her uygulama katmanı için ağ erişimi. Ortak gizliliğinizi korumayı taahhüt eder API arka ucu genel Internet erişiminden gizleme ve yalnızca API'leri Yukarı Akış web uygulamalar tarafından çağrılmasına izin vermektir. [Ağ güvenlik grupları (Nsg'ler)](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) App Service ortamları içeren Azure sanal ağ alt ağlarda, API uygulamaları için ortak erişimi kısıtlamak için kullanılabilir.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Web sunucusu tanılama ve uygulama tanılama
App Service web apps için web sunucusunu hem web uygulamasının içinden bilgileri günlüğe kaydetme tanılama işlevi sağlar. Bu mantıksal olarak ayrılır [web sunucu tanılamalarını](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs) ve [uygulama tanılama](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Tanılama ve sorun giderme siteleri ve uygulamaları, Web sunucusu iki önemli geliştirmeleri içerir.

İlk yeni uygulama havuzları, alt işlemlerin, siteler, uygulama etki alanları ve çalışan istekleri gerçek zamanlı durum bilgilerini özelliğidir. İkinci yeni avantajları tam istek ve yanıt işlemi boyunca bir isteğin izlenmesi ayrıntılı izleme olaylardır.

Bu izleme olaylarını toplamayı etkinleştirmek için IIS 7 otomatik olarak tam izleme günlükleri, geçen süre veya hata yanıt kodları göre herhangi belirli bir istek için XML biçiminde yakalamak için yapılandırılabilir.

#### <a name="web-server-diagnostics"></a>Web sunucusu tanılama
Etkinleştirmek veya günlükleri aşağıdaki türde devre dışı bırakabilirsiniz:

-   Ayrıntılı hata günlüğü - ayrıntılı hata bilgileri belirten bir hata (durum kodu 400 veya üzeri) HTTP durum kodları için. Bu sunucunun döndürülen hata kodu neden belirlemek yardımcı olabilecek bilgiler içerebilir.

-   Başarısız istek izlemeyi - ayrıntılı bir izleme isteği ve her bir bileşende geçen süre işlemek için kullanılan IIS bileşenlerini de dahil olmak üzere, başarısız isteklerle bilgileri. Bu site performansını artırmak veya döndürülecek belirli bir HTTP hatası neden olan yalıtmak çalışıyorsanız yararlı olabilir.

-   Web sunucusu günlüğü - HTTP işlemlerini W3C Genişletilmiş günlük dosyası biçimini kullanarak hakkında bilgi Bu, işlenen isteklerin veya özel bir IP adresinden kaç isteklerdir sayısı gibi genel site ölçümleri belirlerken kullanışlıdır.

#### <a name="application-diagnostics"></a>Uygulama tanılama
[Uygulama tanılama](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs) bir web uygulaması tarafından üretilen bilgileri yakalamanıza olanak sağlar. ASP.NET uygulamalarında kullanabileceğiniz [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) uygulama tanılama günlüğüne bilgileri günlüğe kaydetmek için sınıf. Uygulama Tanılama'da olayları, uygulama performansı ile ilgili ve uygulama arızaları ve hataları ile ilgili başlıca iki türde vardır. Arızalar ve hatalar ayrılabilir daha ayrıntılı bağlantı, güvenlik ve arıza sorunları. Arıza sorunları, genellikle uygulama kodundaki bir sorunla ilgilidir.

Uygulama Tanılama'da bu şekilde gruplandırılmış olayları görebilirsiniz:

-   Tümü (tüm olayları görüntüler)
-   Uygulama hataları (özel durumları görüntüler)
-   Performans (performans olaylarını görüntüler)

## <a name="storage"></a>Depolama
Bu bölümde Azure depolama güvenlik ve Özet bilgilerini bu özellikleri hakkında temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlayabilirsiniz. Erişimi kısıtlama temel alarak [bilmeniz gereken](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunlu. Bu erişim hakları, gruplara ve uygulamalara belirli bir kapsama uygun RBAC rolü atanarak verilir. Kullanabileceğiniz [yerleşik RBAC rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles), ayrıcalıkları kullanıcılara atamak için depolama hesabı katılımcısı gibi. Depolama erişim tuşlarını için bir depolama hesabıyla [Azure Resource Manager](https://docs.microsoft.com/azure/storage/storage-security-guide) modeli, rol tabanlı erişim denetimi (RBAC) denetlenebilir.

### <a name="shared-access-signature"></a>Paylaşılan erişim imzası
[Paylaşılan erişim imzası (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. SAS, belirli bir süre için ve belirli bir izin kümesi ile bir istemci, depolama hesabınızdaki nesnelere sınırlı verebilirsiniz anlamına gelir. Hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan bu sınırlı izinler verebilirsiniz.

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Aktarım sırasında şifreleme ağlar üzerinden iletilirken, veri koruma, bir mekanizmadır. Azure Storage ile verileri kullanarak güvenli hale getirebilirsiniz:
-   [Aktarım düzeyinde şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), Azure depolama içine veya dışına veri aktarımı HTTPS gibi.

-   [Şifreleme wire](https://docs.microsoft.com/azure/storage/storage-security-guide#using-encryption-during-transit-with-azure-file-shares), gibi [SMB 3.0 şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide) için [Azure dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

-   İstemci tarafı şifreleme, verileri şifrelemenizi depolama alanına aktarılır ve depolama alanınız aktarıldıktan sonra verilerin şifresini çözmek için.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Çoğu kuruluş için bekleyen veri şifreleme veri gizlilik, uyumluluk ve veri egemenliği doğrultusunda zorunlu bir adımdır. "Beklemede" olan verilerin şifrelenmesini sağlayan üç Azure depolama güvenlik özellikleri vardır:

-   [Depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption) depolama hizmeti otomatik olarak verileri Azure depolama alanına yazarken şifrelemeniz istemenizi sağlar.

-   [İstemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) bekleyen şifreleme özelliği de sağlar.

-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi diskleri ve veri diskleri bir Iaas sanal makine tarafından kullanılan şifreleme sağlar.

### <a name="storage-analytics"></a>Depolama Analizi
[Azure depolama analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydetme işlemlerini gerçekleştiren ve bir depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. Depolama analizi, başarılı ve başarısız istekler hakkında ayrıntılı bilgi için bir depolama hizmetine kaydeder. Bu bilgiler, tek tek istekleri izlemek için ve bir depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. Bir en iyi çaba ilkesine göre istekleri günlüğe kaydedilir. Aşağıdaki türde kimliği doğrulanmış istekler kaydedilir:
-   Başarılı istekler.

-   İstek zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere, başarısız oldu.

-   Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istek sayısı.

-   Analiz verilerini istekleri.

### <a name="enabling-browser-based-clients-using-cors"></a>CORS kullanarak tarayıcı tabanlı istemcileri etkinleştirme
[Çıkış noktaları arası kaynak paylaşımı (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) birbirine birbirlerinin kaynaklarına erişim izni vermek etki alanları izin veren bir mekanizmadır. Kullanıcı Aracısı, belirli bir etki alanından yüklenen JavaScript kodunu başka bir etki alanında bulunan kaynaklara erişmek için izin verildiğinden emin olmak için fazladan üst bilgiler gönderir. İkinci etki alanı, ardından izin verme veya reddetme kaynaklarını özgün etki alanı erişimi fazladan üst bilgiler ile yanıt verir.

Azure depolama hizmetleri artık CORS desteği, böylece hizmet için CORS kurallarını ayarladığınızda farklı bir etki alanından hizmet karşı yapılan düzgün şekilde kimliği doğrulanmış bir istek, belirttiğiniz kurallara göre izin verilip verilmeyeceğini belirlemek için değerlendirilir.
## <a name="networking"></a>Ağ
Bölüm Azure ağ güvenliği ve Özet bilgilerini bu özellikleri hakkında temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="network-layer-controls"></a>Ağ katmanı denetimleri
Ağ erişim denetimi, belirli cihazları veya alt ağlara gelen ve giden bağlantı sınırlama, işlemidir ve ağ güvenliği setinin temsil eder. Ağ erişim denetimi sanal makineler ve hizmetler yalnızca kullanıcılara ve cihazlara bunları erişilebilir istediğiniz erişilebilir olduğundan emin olmaktır.

#### <a name="network-security-groups"></a>Ağ Güvenlik Grupları
A [ağ güvenlik grubu (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) güvenlik duvarı ve filtreleme temel durum bilgisi olan bir paket dayalı olarak erişimi denetlemenize olanak sağlayan, bir [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). Nsg'ler, uygulama katmanı İnceleme sağlamaz veya kimliği doğrulanmış erişim denetimleri. Bir Azure sanal ağ içindeki alt ağlar ve Azure sanal ağı ve Internet arasında trafiği arasında taşıma trafiği denetlemek için kullanılabilir.

#### <a name="route-control-and-forced-tunneling"></a>Rota denetimi ve zorlamalı tünel
Azure sanal ağlarınızda yönlendirme davranışını denetlemek için bir kritik ağ güvenlik ve erişim denetimi özelliği yeteneğidir. Örneğin, Azure sanal ağınıza gelen ve giden tüm trafiği, güvenlik sanal gereçten aşması emin olmak istiyorsanız, yönlendirme davranışını özelleştirmek ve denetlemek gerekir. Azure'da kullanıcı tanımlı rotaları yapılandırarak bunu yapabilirsiniz.

[Kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) taşınmasını trafiği gelen ve giden yollar özelleştirmenize olanak sağlar ve tek tek sanal makineleri veya en çok sağlamak için alt ağ dışına rota mümkün olan en güvenli hale getirin. [Zorlamalı tünel](https://www.petri.com/azure-forced-tunneling) hizmetlerinizi Internet'teki cihazlar için bir bağlantı başlatmak için izin verilmiyor emin olmak için kullanabileceğiniz bir mekanizmadır.

Bu, gelen bağlantıları kabul edebilir ve bunlara yanıt farklıdır. Ön uç web sunucusuna Internet konaklarından isteklerine yanıt gerekir ve bu nedenle Internet kaynaklı trafik bu web sunucularına gelen ve web sunucularını yanıt izin verilir.

Zorlamalı tünel şirket içi güvenlik proxy ve güvenlik duvarları gitmek için İnternet'e giden trafiği zorlamak için yaygın olarak kullanılır.

#### <a name="virtual-network-security-appliances"></a>Sanal ağ güvenlik Gereçleri
Ağ güvenlik grupları, kullanıcı tanımlı rotalar ve zorlamalı tünel ağ ve Aktarım katmanı güvenlik düzeyini sunarken [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), güvenlik daha yüksek düzeylerde etkinleştirmek istediğinizde zamanlar olabilir yığın. Bir Azure iş ortağı ağ güvenlik Gereci çözümü kullanarak gelişmiş ağ güvenliği özelliklere erişebilirsiniz. Güvenlik çözümleri ziyaret ederek en güncel Azure iş ortağı ağı bulabilirsiniz [Azure Marketi](https://azure.microsoft.com/marketplace/) ve "güvenlik" ve "ağ güvenliği" için arama

### <a name="azure-virtual-network"></a>Azure Sanal Ağ

Azure Virtual Network (VNet) buluttaki kendi ağınızın bir gösterimidir. Azure ağ doku aboneliğinize adanmış mantıksal bir ayırma olduğu. Bu ağ içindeki IP adres bloklarını, DNS ayarlarını, güvenlik ilkelerini ve yol tablolarını tam olarak denetleyebilirsiniz. Sanal ağınızı alt ağlara ayırabilir ve Azure Iaas sanal makineleri (VM'ler) koyun ve/veya [Cloud services (PaaS rolü örnekleri)](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) Azure Sanal Ağları üzerindeki.

Bunun yanı sıra, Azure'ın sunduğu [bağlantı seçeneklerinden](https://docs.microsoft.com/azure/vpn-gateway/) birini kullanarak sanal ağı şirket içi ağınıza bağlayabilirsiniz. Özetle, IP adres blokları üzerinde tam bir kontrol sahibi olarak ve Azure'ın sunduğu kurumsal ölçek avantajıyla, ağınızı Azure'a genişletebilirsiniz.

Azure ağ güvenli uzaktan erişim çeşitli senaryoları destekler. Bunlardan bazıları şunlardır:

-   [Ayrı iş istasyonları bir Azure sanal ağına bağlama](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

-   [Bir Azure sanal ağı bir VPN ile şirket içi ağa bağlanma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

-   [Şirket içi ağı ayrılmış bir WAN bağlantısı ile bir Azure sanal ağ bağlama](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)

-   [Azure sanal ağları birbirine bağlama](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)

### <a name="vpn-gateway"></a>VPN Gateway
Azure sanal ağınız ile şirket içi siteniz arasında ağ trafiği göndermek için Azure sanal ağınız için bir VPN ağ geçidi oluşturmanız gerekir. A [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) bir ortak bağlantı üzerinde şifrelenmiş trafik gönderen sanal ağ geçidi türüdür. Azure ağ yapısı üzerinden Azure sanal ağları arasında trafik göndermek için VPN ağ geçitlerini de kullanabilirsiniz.

### <a name="express-route"></a>Express Route
Microsoft Azure [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden, şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlayan özel bir WAN bağlantısı.

![Express Route](./media/azure-security/azure-security-fig1.png)

ExpressRoute ile Microsoft Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantı kurabilirsiniz. Ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden herhangi bir ağdan herhangi bir ağa (IP VP), noktadan noktaya Ethernet ağı veya sanal çapraz bağlantısından bağlantı olabilir.

ExpressRoute bağlantıları ortak Internet üzerinden geçmemektedir ve bu nedenle VPN tabanlı çözümler daha fazla güvenli kabul edilebilir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) sağlayan bir [uygulama teslim denetleyicisi (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) çeşitli 7. Katman Yük Dengeleme uygulamanız için özellikleri sunan hizmet olarak.

![Application Gateway](./media/azure-security/azure-security-fig2.png)

CPU yoğunluklu SSL sonlandırma (diğer adıyla "SSL yük boşaltma" veya "SSL köprüleme") Application gateway'e boşaltarak web grubu üretkenliğini iyileştirme olanağı tanır. Ayrıca, hepsini bir kez deneme dağıtımını gelen trafiği, tanımlama bilgilerine dayalı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir Application Gateway arkasında birden fazla Web sitesi barındırma olanağı dahil olmak üzere diğer 7. Katman yönlendirme özelliklerini sağlar. Azure Application Gateway, bir katman 7 yük dengeleyicidir.

Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar.

Uygulamanın sağladığı HTTP dahil birçok uygulama teslim denetleyicisi (ADC) özelliği Yük Dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi, [Güvenli Yuva Katmanı (SSL)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-powershell) yük boşaltma, özel sistem durumu araştırmaları, çoklu site için destek ve diğer birçok.

### <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı
Web uygulaması güvenlik duvarı özelliğidir [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) standart Application Delivery Control (ADC) işlevleri için uygulama ağ geçidi kullanan web uygulamalarına koruma sağlar. Web uygulaması güvenlik duvarı bunu, uygulamaları OWASP tarafından sunulan en yaygın 10 web güvenlik açığının çoğuna karşı koruyarak gerçekleştirir.

![Web Uygulaması Güvenlik Duvarı](./media/azure-security/azure-security-fig1.png)

-   SQL ekleme koruması

-   Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

-   HTTP protokolü ihlallerine karşı koruma

-   Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

-   Robotlar, gezginler ve tarayıcıları önleme

-   Yaygın yanlış uygulama yapılandırmalarını (diğer bir deyişle, Apache, IIS, vb.) algılama


Web saldırılarına karşı korunacak merkezi bir web uygulaması, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim tehditlerine karşı uygulamayı daha güvende tutar. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.
### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) , farklı veri merkezlerindeki hizmet uç noktaları için kullanıcı trafiğinin dağıtımını denetlemenize olanak tanır. Hizmet uç noktaları traffic Manager tarafından desteklenen Azure Vm'lerinde, Web uygulamaları ve bulut hizmetleri içerir. Traffic Manager’ı harici, Azure dışı uç noktalar için de kullanabilirsiniz. Traffic Manager, istemci isteklerine göre en uygun uç noktaya doğrudan etki alanı adı sistemi (DNS) kullanan bir [trafik yönlendirme yöntemine](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) ve sistem durumu uç nokta.

Traffic Manager trafik yönlendirme yöntemlerini uç nokta sistem durumu olan farklı uygulama gereksinimlerinize göre bir dizi sunar [izleme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)ve otomatik yük devretme. Traffic Manager, bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.
### <a name="azure-load-balancer"></a>Azure Load Balancer
[Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview), uygulamalarınıza yüksek düzeyde kullanılabilirlik ve ağ performansı sağlar. Bu gelen trafiği sağlıklı yük dengeli bir küme içinde tanımlı Hizmetleri örnekleri arasında dağıtan bir katman 4 (TCP, UDP) yük dengeleyicidir. Azure Load Balancer için yapılandırılabilir:

-   Sanal makinelere gelen Internet trafiğini Dengeleme yükleyin. Bu yapılandırma olarak bilinir [Internet'e yönelik Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Bir sanal ağdaki sanal makineler arasında bulut Hizmetleri sanal makineler arasında veya şirket içi bilgisayarlar ile şirketler arası sanal ağdaki sanal makineler arasında Yük Dengeleme trafiği. Bu yapılandırma olarak bilinir [iç Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview). 

- Dış trafiği belirli bir sanal makineye ilet

### <a name="internal-dns"></a>İç DNS
Yönetim Portalı'nda veya ağ yapılandırma dosyasında bir sanal ağda kullanılan DNS sunucularının listesini yönetebilirsiniz. Müşteri, her sanal ağ için en fazla 12 DNS sunucuları ekleyebilirsiniz. DNS sunucuları belirlerken, müşterinin ortam için doğru sırada müşterinin DNS sunucuları listesi doğrulamak önemlidir. DNS sunucu listelerini, hepsini bir kez deneme çalışmaz. Bunlar, bunlar belirttiğiniz sırayla kullanılır. Listedeki ilk DNS sunucusunu erişilmesi mümkün ise, istemci DNS sunucusu yok ya da düzgün çalıştığından bağımsız olarak, DNS sunucusunu kullanır. Müşterinin sanal ağın DNS sunucusu sırasını değiştirme, DNS sunucuları listeden kaldırın ve bunları sırayla, müşteri eklemek istiyor. DNS "CIA" güvenlik Üçlü kullanılabilirlik yönüyle destekler.

### <a name="azure-dns"></a>Azure DNS
[Etki alanı adı sistemi](https://technet.microsoft.com/library/bb629410.aspx), veya DNS çevirmek için sorumlu (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) DNS etki alanları, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamak için bir barındırma hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. DNS "CIA" güvenlik Üçlü kullanılabilirlik yönüyle destekler.
### <a name="azure-monitor-logs-nsgs"></a>Azure İzleyici Nsg'ler günlüğe kaydeder.
Nsg'ler için aşağıdaki tanılama günlük kategorileri etkinleştirebilirsiniz:
-   olay: Vm'lere ve örnek rollerinizin MAC adresini temel alarak için hangi NSG kuralları uygulanır girişler içeriyor. Bu kurallar durumu, 60 saniyede toplanır.

-   Kuralları sayacı: Girişleri için kaç kez trafiğine izin vermek veya reddetmek için uygulanan her bir NSG kuralı içerir.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Güvenlik Merkezi, tehditleri önleyin, algılayın ve yardımcı olur ve görünürlük artırdık sağlar ve Azure kaynaklarınızın güvenliğini, üzerinde denetim. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan geçebilir tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur. Ağ önerileri Merkezi güvenlik duvarları, ağ güvenlik grupları, gelen trafik kuralları yapılandırma ve daha fazla.

Kullanılabilir ağ önerileri aşağıdaki gibidir:

-   [Yeni nesil güvenlik duvarı ekleme](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) bir Microsoft iş ortağından, güvenlik korumaları artırmak için bir sonraki Generation Firewall (NGFW) eklediğiniz önerir

-   [Trafiği yalnızca NGFW aracılığıyla yönlendirme](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall#route-traffic-through-ngfw-only) ağ güvenlik grubu (NSG) kuralları, NGFW aracılığıyla sanal makinenize gelen trafiği zorlamak yapılandırdığınız önerir.

-   [Alt ağlar veya sanal makinelerde ağ güvenlik gruplarını etkinleştirme](https://docs.microsoft.com/azure/security-center/security-center-enable-network-security-groups) Nsg'leri alt ağlara veya sanal makineleri üzerinde etkinleştirmenizi önerir.

-   [Internet'e yönelik uç noktalarla erişimi sınırlama](https://docs.microsoft.com/azure/security-center/security-center-restrict-access-through-internet-facing-endpoints) Nsg'ler için gelen trafik kuralları yapılandırma önerir.


## <a name="compute"></a>İşlem

Bölüm, bu alan ve Özet bilgilerini bu özellikleri hakkında temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="antimalware--antivirus"></a>Kötü amaçlı yazılımdan koruma & virüsten koruma
Azure Iaas ile sanal makinelerinizi kötü amaçlı dosyalardan, reklam yazılımlarından ve diğer tehditlerden korumak amacıyla Microsoft, Symantec, Trend Micro, McAfee ve Kaspersky gibi güvenlik hizmeti satıcısı kötü amaçlı yazılımdan koruma yazılımından da kullanabilirsiniz. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) Azure bulut Hizmetleri ve sanal makineler için belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan bir koruma özelliğidir. Microsoft Antimalware bilinen kötü amaçlı veya istenmeyen yazılım kendini yüklemeye veya Azure sistemlerinize çalışmayı denediğinde, yapılandırılabilir uyarılar sağlar. Microsoft Antimalware ayrıca Azure Güvenlik Merkezi kullanılarak dağıtılabilir

### <a name="hardware-security-module"></a>Donanım güvenlik modülü
Anahtarlar korunan sürece şifreleme ve kimlik doğrulaması güvenliğini artırmak değildir. Bunları depolayarak, yönetim ve güvenlik kritik gizli dizileri ve anahtarları basitleştirebilirsiniz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault, anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) depolama seçeneği sunar. SQL Server şifreleme anahtarları için yedekleme veya [saydam veri şifrelemesi](https://msdn.microsoft.com/library/bb934049.aspx) tüm anahtar Kasası'nda tüm anahtarlar veya parolalar uygulamalarınızdan depolanabilir. İçin bu korumalı öğelere izni veya erişimi aracılığıyla yönetilir [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Sanal makine yedekleme
[Azure yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) uygulama verilerinizi sıfır sermaye yatırımı ve en az bir çözüm işletim maliyetlerini. Uygulama hataları verilerinizi bozabilir ve insan hatalarına uygulamalarınıza güvenlik sorunlarına yol açabilir hatalar ortaya çıkarabilir. Windows ve Linux çalıştıran sanal makinelerinizi Azure Backup ile korunmuş olur.

### <a name="azure-site-recovery"></a>Azure Site Recovery
Kuruluşunuzun önemli bir parçası [iş sürekliliği/olağanüstü durum kurtarma (BCDR)](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) stratejisi kurumsal iş yüklerini ve uygulamaları çalıştırmaya planlı korumak nasıl anlamak ve plansız kesintiler. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) yardımcı olur, çoğaltma, yük devretme ve kurtarma, iş yükleri ve uygulamalar, birincil çökmesi durumunda ikincil konum kullanılabilir, böylece düzenleyin.

### <a name="sql-vm-tde"></a>SQL VM TDE
[Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault) ve sütun düzeyinde şifrelemeyi (CLE) SQL server şifreleme özellikleri. Bu şifreleme biçimi şifreleme için kullandığınız şifreleme anahtarlarını depolamak ve yönetmek müşterilerin gerektirir.

Azure Key Vault (AKV) hizmeti, güvenlik ve bu anahtarları güvenli ve yüksek oranda kullanılabilir bir konumda yönetimini iyileştirmek için tasarlanmıştır. SQL Server Bağlayıcısı, SQL Server'ın bu anahtarları Azure Key vault'tan kullanmayı sağlar.

SQL Server ile şirket içi makineleri çalıştırıyorsanız, şirket içi SQL Server makinenizden Azure Key Vault'a erişmek için izlemeniz gereken adımlar vardır. Ancak, Azure sanal makinelerinde SQL Server için Azure anahtar kasası tümleştirmeyi özelliğini kullanarak zaman kazanabilirsiniz. Bu özelliği etkinleştirmek için birkaç Azure PowerShell cmdlet'leriyle, bir SQL VM anahtar kasanıza erişmek gerekli yapılandırmayı otomatik hale getirebilirsiniz.

### <a name="vm-disk-encryption"></a>VM Disk şifrelemesi
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) olan yardımcı olan yeni bir özellik, Windows ve Linux Iaas sanal makine disklerinizi şifreleyin. Bu sektör standart BitLocker özelliğini Windows ve Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için DM-Crypt özelliğini uygular. Denetim ve disk şifreleme anahtarlarını ve gizli anahtarları Key Vault aboneliğinizdeki yönetmenize yardımcı olması için Azure Key Vault ile tümleşik bir çözüm. Çözüm ayrıca sanal makine disklerindeki tüm veriler Azure depolama alanınızda bekleyen şifrelenmesini sağlar.

### <a name="virtual-networking"></a>Sanal ağ
Sanal makinelerin ağ bağlantısı gerekir. Bu gereksinimi desteklemek için Azure sanal makineler bir Azure sanal ağına bağlanması gerekir. Bir Azure sanal ağ üzerinde fiziksel Azure ağ dokusu oluşturulmuş mantıksal bir yapıdır. Her mantıksal [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) diğer tüm Azure sanal ağlardan yalıtılır. Bu yalıtım, dağıtımlarınızı ağ trafiğini diğer Microsoft Azure müşterileri için erişilebilir değil Sigortası yardımcı olur.

### <a name="patch-updates"></a>Düzeltme eki güncelleştirmeleri
Düzeltme eki güncelleştirmeleri, bulma ve olası sorunları düzeltmek için temeli sağlar ve yazılım güncelleştirme yönetimi işlemi, kuruluşunuza dağıtmadan yazılım güncelleştirme sayısını azaltarak ve uyumluluğunu izleme olanağı artırarak basitleştirin.

### <a name="security-policy-management-and-reporting"></a>Güvenlik İlkesi Yönetimi ve Raporlama
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) önlemek, algılamak ve yanıt Tehditler ve artan görünürlük sağlar ve denetim üzerinde Azure kaynaklarınızın güvenliğini yardımcı olur. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan geçebilir tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

Sistemleri, uygulamalar ve veri güvenliği, kimlik tabanlı erişim denetimleriyle başlar. Microsoft iş ürünlerini ve hizmetlerini yerleşik kimlik ve erişim yönetimi özellikleri, kullanıcıların kullanımına yasal zaman veya mekandan bağımsız olmasını sağlarken Kurumsal ve kişisel bilgilerinizi yetkisiz erişimden korunmasına yardımcı Yöneticiler gerekir.

### <a name="secure-identity"></a>Güvenli kimlik
Microsoft birden çok güvenlik uygulamaları ve teknolojileri, ürün ve hizmetleri arasında kimlik ve erişim yönetmek için kullanır.
-   [Çok faktörlü kimlik doğrulaması](https://azure.microsoft.com/services/multi-factor-authentication/) kullanıcıların erişim, şirket içi ve buluttaki birden çok yöntem kullanmasını gerektirir. Bu, kullanıcıların basit bir oturum açma işlemi ile destekleme çalışırken güçlü kimlik doğrulaması ile çeşitli kolay doğrulama seçenekleri sağlar.

-   [Microsoft Authenticator](https://aka.ms/authenticator) , Microsoft Azure Active Directory ve Microsoft hesapları ile çalışır ve giyilebilir cihazlar ve parmak izi tabanlı bir onayları için destek içerir kullanıcı dostu bir çok faktörlü kimlik doğrulaması deneyimi sağlar.

-   [Parola ilkesi zorlama](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) düzenli rotasyonu ve hesap kilitleme başarısız kimlik doğrulama girişimlerinin sonra zorlanmış uzunluğu ve karmaşıklık gereksinimleri koyarak geleneksel parolaların güvenliğini artırır.

-   [Belirteç tabanlı kimlik doğrulaması](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) Azure Active Directory aracılığıyla kimlik doğrulamasını etkinleştirir.

-   [Rol tabanlı erişim denetimi (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) kullanıcılara yalnızca kendi iş görevlerini gerçekleştirmek için ihtiyaç duydukları erişim miktarını verebilirsiniz kolaylaştırma kullanıcının atanan role göre dayalı olarak erişimi vermenizi sağlar. RBAC, kuruluşunuzun iş modeli ve risk toleransınıza özelleştirebilirsiniz.

-   [Tümleşik kimlik yönetimine (karma kimlik)](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) iç veri merkezlerinden ve bulut platformunda kimlik doğrulaması ve yetkilendirme için tüm kaynakları tek bir kullanıcı kimliği oluşturma, kullanıcıların erişimini denetiminizde tutmanıza olanak sağlar.

### <a name="secure-apps-and-data"></a>Güvenli uygulamalar ve veriler
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/), bir kapsamlı kimlik ve erişim yönetimi bulut çözümü, sitede uygulamalarda ve bulut veri erişimin korunmasına yardımcı olur ve kullanıcıların ve grupların yönetimini basitleştirir. Temel dizin hizmetlerini, Gelişmiş kimlik yönetimini, güvenlik ve uygulama erişim yönetimi, bir araya getirir ve geliştiricilerin uygulamalarında kimlik ilke tabanlı yönetime kolaylaştırır. Azure Active Directory'nizi geliştirmek için Azure Active Directory Temel, Premium P1 ve Premium P2 sürümleriyle ücretli özellikler ekleyebilirsiniz.

| Ücretsiz / ortak özellikleri     | Temel özellikleri    |Premium P1 özellikleri |Premium P2 özellikleri | Azure Active Directory Join – yalnızca Windows 10 ilgili özellikleri|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Dizin nesneleri](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [kullanıcı/Grup Yönetimi (ekleme/güncelleştirme/silme) / kullanıcı tabanlı sağlama, cihaz kaydı](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [çoklu oturum açma (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Self Servis parola Bulut kullanıcıları için değiştirme](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Connect (şirket içi dizinleri Azure Active Directory'ye genişleten eşitleme motoru)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [güvenlik / kullanım raporları](https://docs.microsoft.com/azure/active-directory/active-directory-editions)       |     [Grup tabanlı erişim yönetimi / sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [bulut kullanıcıları için Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [şirket markalama (oturum açma sayfaları/erişim paneli özelleştirme)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [ Uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [SLA'sı % 99,9 oranında](https://docs.microsoft.com/azure/active-directory/active-directory-editions) |  [Self Servis grup ve uygulama yönetimi/Self Servis uygulama eklemeleri/dinamik gruplar](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Self Servis parola sıfırlama/değiştirme/kilidini açma ile şirket içi geri yazma](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [multi-Factor Authentication Kimlik doğrulaması (Bulut ve şirket içi (MFA sunucusu))](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [MIM CAL + MIM sunucusu](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Connect Health](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Grup hesapları için otomatik parola geçişi](https://docs.microsoft.com/azure/active-directory/active-directory-editions)|  [Kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)|  [Azure AD, Masaüstü SSO, Microsoft Passport Azure AD için yönetici BitLocker kurtarmasına cihaz ekleme](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [MDM otomatik kayıt, Self Servis BitLocker kurtarması, Azure AD aracılığıyla Windows 10 cihazlar için ek yerel Yöneticiler Katılın](https://docs.microsoft.com/azure/active-directory/active-directory-editions)|


- [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-cloudappdiscovery-whatis) kuruluşunuzdaki çalışanlar tarafından kullanılan bulut uygulamaları tanımlamanızı sağlayan Azure Active Directory premium özelliğidir.

- [Azure Active Directory kimlik koruması](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) anomali algılama özellikleri Azure Active Directory risk olayları ve etkileyebilecek olası güvenlik açıklarına ilişkin birleştirilmiş görünüm sağlamak için kullandığı bir güvenlik hizmetidir, kuruluşun kimlik.

- [Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/) bir etki alanına etki alanı denetleyicileri dağıtmak zorunda kalmadan Azure Vm'leri ekleme sağlar. Kullanıcılar, bu Vm'lere Kurumsal Active Directory kimlik bilgilerini kullanarak oturum açın ve kaynaklara sorunsuz şekilde erişin.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) bir yüz milyonlarca kimliğe kadar ölçeklendirme ve mobil tümleştirme tüketiciye yönelik uygulamalar için yüksek kullanılabilirliğe sahip, global Kimlik Yönetimi hizmetidir ve web platformlar. Müşterilerinizin özelleştirilebilir bir deneyimle mevcut sosyal medya hesaplarını kullanan tüm uygulamaları oturum açabilir veya yeni tek başına kimlik oluşturabilirsiniz.

- [Azure Active Directory B2B işbirliği](https://aka.ms/aad-b2b-collaboration) , iş ortaklarının Kurumsal uygulama ve verilere kendi kendi kendine yönetilen kullanarak erişmesini olanaklı kılarak şirketler arası ilişkilerinizi destekler güvenli iş ortağı tümleştirme çözümü kimlik.

- [Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) Merkezi Yönetimi için Windows 10 cihazlarına bulut özellikleri genişletmenizi sağlar. Bu, kullanıcıların şirket veya kuruluş bulut Azure Active Directory üzerinden bağlanmasına olanak sağlar ve uygulama ve kaynaklara erişimi basitleştirir.

- [Azure Active Directory Uygulama proxy'si](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) şirket içinde barındırılan web uygulamalarını SSO ve güvenli uzaktan erişim sağlar.

## <a name="next-steps"></a>Sonraki Adımlar
- [Microsoft Azure'daki güvenlik özellikleriyle çalışmaya başlama](https://docs.microsoft.com/azure/security/azure-security-getting-started)

Azure’daki hizmet ve verilerinizin güvenliğini sağlamanıza yardımcı olması için kullanabileceğiniz Azure hizmetleri ve özellikleri

- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)

Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim sayesinde tehditleri önleyin, algılayın ve bu tehditlere yanıt verin

- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](https://docs.microsoft.com/azure/security-center/security-center-monitoring)

İlkelerle uyumluluğu izlemek için izleme özellikleri Azure Güvenlik Merkezi'nde.
