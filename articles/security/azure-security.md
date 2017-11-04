---
title: "Azure güvenlik giriş | Microsoft Docs"
description: "Azure güvenlik, hizmetlerinin ve nasıl çalıştığı hakkında bilgi edinin."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: TomSh
ms.openlocfilehash: 54bbd7dd1d0ecad79f86e0ab16be3a48854093ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-security"></a>Azure güvenlik giriş
## <a name="overview"></a>Genel Bakış
Güvenlik bulutta iş ve nasıl önemli Azure güvenliği hakkında doğru ve güncel bilgi bulma olduğunu olduğunu biliyoruz. Çeşitli güvenlik araçları ve yetenekleri yararlanmak için uygulamalar ve hizmetler için Azure kullanmak için en iyi nedenlerinden biridir. Bu araçları ve yetenekleri güvenli Azure platformunda güvenli çözümler oluşturmak mümkün kılar yardımcı olur. Microsoft Azure gizlilik, bütünlük ve müşteri verilerini, kullanılabilirliğini de saydam sorumluluk etkinleştirirken sağlar.

Microsoft Azure içinde hem de uygulanan güvenlik denetimleri koleksiyonunu daha iyi anlamanıza yardımcı olması için Microsoft operations ve müşterinin Perspektifler, "Giriş için Azure güvenlik", bu teknik incelemede yazılır Microsoft Azure ile kullanılabilir güvenlik kapsamlı bir bakış sağlar.

### <a name="azure-platform"></a>Azure platformu
Azure işletim sistemleri, programlama dilleri, çerçeveleri, Araçlar, veritabanları ve aygıtları geniş çapta destekleyen bir genel bulut hizmeti platformudur. Docker Tümleştirmesi ile Linux kapsayıcıları çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturma; Yapı geri-iOS, Android ve Windows cihazları sona erer.

Azure genel bulut Hizmetleri aynı teknolojileri geliştiriciler milyonlarca destekler ve BT uzmanları zaten kullanır ve güven. Oluşturmanıza veya BT varlıklarına geçirmek, uygulamalar ve hizmetler ve denetimleri ile verileri korumak için bir kuruluşun yeteneklerini güvenmek bir genel bulut hizmeti sağlayıcısı bulut tabanlı varlıklarınızı güvenliği yönetmek için sağlarlar.

Azure'nın altyapı tesis aynı anda milyonlarca müşteri barındırmak için uygulamalar için tasarlanmıştır ve güvenlik gereksinimlerine bağlı işletmeler karşılayabilecek güvenilir bir temel sağlar.

Ayrıca, Azure ile geniş bir yelpazesini yapılandırılabilir güvenlik seçenekleri ve kuruluşunuzun dağıtımları benzersiz gereksinimlerini karşılamak için güvenlik özelleştirebilirsiniz böylece bunları denetleme olanağı sağlar. Bu belge, nasıl Azure güvenliği anlamanıza yardımcı olur. özellikler, bu gereksinimleri karşılamak yardımcı.

> [!Note]
> Bu belgenin birincil odağı özelleştirebilir ve uygulamalar ve hizmetler için güvenliği artırmak için kullanabileceğiniz müşteri dönük denetimleri açıktır.
>
> Biz bazı genel bir bakış sağlar, ancak sağlanan bilgileri nasıl Microsoft Azure platformu güvenliğini sağlama ile ilgili ayrıntılı bilgi için bkz: [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

### <a name="abstract"></a>Özet
Başlangıçta, genel bulut geçişler maliyet tasarrufu ve çeviklik tarafından yenilik için güdümlü. Güvenlik süre ve hatta bir Göster stopper, genel bulut geçiş için başlıca sorunlardan olarak kabul edildi. Ancak, genel bulutun güvenlik buluta geçiş için sürücülerin birine başlıca sorunlardan çözümlemeye geçmiş. Yanıtın bu arkasındaki mantığı uygulamaların koruma üstün yeteneğini büyük genel bulut hizmeti sağlayıcıları ve bulut tabanlı varlıklar veri ' dir.

Azure altyapısı tesisten uygulamalara kadar milyonlarca müşteriye aynı anda hizmet verecek şekilde tasarlanmıştır ve işletmelerin güvenlik ihtiyaçlarını karşılayabilecek güvenilir bir temel sunar. Ayrıca, Azure çok çeşitli yapılandırılabilir güvenlik seçenekleri sağlar ve böylece BT karşılamak üzere dağıtımlarınızı benzersiz gereksinimlerini karşılamak için güvenlik özelleştirebilirsiniz bunları denetleme olanağı ilkeleri denetlemek ve dış düzenlemelere uygun.

Bu raporda Microsoft Azure bulut platformu güvenliğe Microsoft'un yaklaşımı özetlenmektedir:
* Azure altyapı, müşteri verilerini ve uygulamalarını güvenli hale getirmek için Microsoft tarafından uygulanan güvenlik özellikleri.
* Azure Hizmetleri ve güvenlik özellikleri hizmetlerinin ve verilerinizi Azure aboneliklerinin içindeki güvenliği yönetmek için de kullanılabilir.

## <a name="summary-azure-security-capabilities"></a>Özet Azure güvenlik özellikleri
Tablo aşağıdaki Azure altyapısı, müşteri verileri ve güvenli uygulamalar güvenli hale getirmek için Microsoft tarafından uygulanan güvenlik özelliklerinin kısa bir açıklama sağlayın.
### <a name="security-features-implemented-to-secure-the-azure-platform"></a>Azure platformu güvenli hale getirmek için uygulanan güvenlik özellikleri:
Aşağıdaki listelenen özellikleri olan özelliklerini Azure platformu güvenli bir şekilde yönetilir güvence altına almaya gözden geçirebilirsiniz. Bağlantılar için daha fazla Microsoft Müşteri güven soruları dört alanlarda nasıl ele üzerinde ayrıntıya sağlandı: Güvenli Platform, gizlilik ve denetimler, uyumluluk ve saydamlığı.


| [Güvenli Platform](https://www.microsoft.com/en-us/trustcenter/Security/default.aspx)  | [Gizlilik ve denetimleri](https://www.microsoft.com/en-us/trustcenter/Privacy/default.aspx)  |[Uyumluluk](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)   | [Saydamlık](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Güvenlik geliştirme döngüsü](https://www.microsoft.com/en-us/sdl/), dahili denetimler | [Verilerinizin her zaman Yönetimi](https://www.microsoft.com/en-us/trustcenter/Privacy/You-own-your-data) | [Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/default.aspx) |[Microsoft Azure hizmetlerinde müşteri verilerini nasıl korur](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| [Zorunlu güvenlik Eğitimi, arka plan denetimleri](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc) |  [Veri konumu denetleme](https://www.microsoft.com/en-us/trustcenter/Privacy/Where-your-data-is-located) |  [Ortak Denetimler Hub](https://www.microsoft.com/en-us/trustcenter/Common-Controls-Hub) |[Microsoft Azure Hizmetleri veri konumda yönetmek hakkında](http://azuredatacentermap.azurewebsites.net/)|
| [Sızma testi](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc), [izinsiz giriş algılama DDoS](https://www.microsoft.com/en-us/trustcenter/Security/ThreatManagement), [denetimleri & günlüğe kaydetme](https://www.microsoft.com/en-us/trustcenter/Security/AuditingAndLogging) | [Sizin koşullarınızda veri erişimi sağlar](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [Tespitlerini denetim listesi son bulut Hizmetleri](https://www.microsoft.com/en-us/trustcenter/Compliance/Due-Diligence-Checklist) |[Microsoft, verilerinizi hangi koşullarınızda erişebilecek mi](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [Resim datacentre durumu](https://www.microsoft.com/en-us/cloud-platform/global-datacenters), fiziksel güvenlik [güvenli ağ](https://docs.microsoft.com/en-us/azure/security/security-network-overview) | [Yasa uygulama yanıt](https://www.microsoft.com/en-us/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Hizmet, konum ve endüstri göre uyumluluk](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx) |[Microsoft Azure hizmetlerinde müşteri verilerini nasıl korur](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx)|
|  [Güvenlik olay yanıtı](http://aka.ms/SecurityResponsepaper), [paylaşılan sorumluluk](http://aka.ms/sharedresponsibility) |[Sıkı gizlilik standartları](https://www.microsoft.com/en-us/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Sertifika saydamlık hub'ı, Azure Hizmetleri için inceleyin](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)|



### <a name="security-features-offered-by-azure-to-secure-data-and-application"></a>Veri ve uygulama güvenliğini sağlamak için Azure tarafından sunulan güvenlik özellikleri
Bulut hizmeti modeline bağlı olarak, uygulama veya hizmet güvenliğini yönetmekten sorumlu kim değişken sorumluluğunu yoktur. Bu Sorumluluklar Azure aboneliğinize dağıtılabilir iş ortağı çözümleri ve yerleşik özellikleri aracılığıyla toplantı yardımcı olmak üzere Azure platformu bulunan özellikleri vardır.

Yerleşik özellikleri altı (6) işlevsel düzenlenir: işlem, uygulamalar, depolama, ağ, hesaplama ve kimlik. Özellik ve yetenekler Azure platformunda bu altı (6) alanlarda ek ayrıntılı Özet bilgiler sağlanır.

## <a name="operations"></a>İşlemler
Bu bölüm, güvenlik işlemlerinde temel özellikleri ile ilgili ek bilgiler ve bu özellikler hakkındaki özet bilgileri sağlar.

### <a name="operations-management-suite-security-and-audit-dashboard"></a>Operations Management Suite güvenlik ve Denetim Panosu
[OMS güvenlik ve denetim çözümü](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) kuruluşunuzun kapsamlı bir görünüme sağlar BT güvenlik tutumunu ile [yerleşik arama sorguları](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) dikkat etmeniz gereken önemli sorunlar için. [Güvenlik ve Denetim](https://technet.microsoft.com/library/mt484091.aspx) panosu, OMS'de yer alan güvenlikle ilgili tüm öğelere ilişkin giriş ekranıdır. Bilgisayarlarınızı güvenlik durumunu üst düzey bir anlayış sağlar. Ayrıca son 24 saat, 7 gün veya herhangi bir özel zaman dilimine ait tüm olayları görüntüleme becerisine sahiptir.

Ayrıca, OMS güvenlik ve uyumluluk için yapılandırabilirsiniz [otomatik olarak belirli eylemleri gerçekleştirme](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) zaman belirli bir olay algılandı.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager ](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) bir grup olarak çözümünüzdeki kaynaklar ile çalışmanıza olanak tanır. Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Kullandığınız bir [Azure Resource Manager şablonu](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) için dağıtım ve bu şablon test, hazırlama ve üretim gibi farklı ortamları için çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar.

Azure Resource Manager şablonu tabanlı dağıtımlar standart güvenlik ayarlarını denetleme ve Azure üzerinde dağıtılan çözümleri güvenliğini artırılmasına yardımcı tümleşik standartlaştırılmış şablon tabanlı dağıtımlar. Bu dağıtımlar sırasında gerçekleşmesi güvenlik yapılandırma hataları riskini azaltır.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/) web geliştiricileri için genişletilebilir bir uygulama performansı Yönetimi (APM) hizmetidir. Application Insights ile canlı web uygulamalarınızı izleyin ve performans anormalliklerini otomatik olarak algıla. Sorunları tanılamanıza yardımcı ve kullanıcıların gerçekte uygulamalarınızı ile neler anlamak için güçlü analytics araçlar içerir. Uygulamanız, test sırasında hem yayımlanan veya dağıtılmış sonra çalıştığı her zaman izler.

Application Insights grafikler ve, örneğin Göster tabloları, kullanıcıların çoğunun Al ne zaman günün, nasıl yanıt vereceğini uygulama olur ve ne kadar iyi bağımlı herhangi bir dış hizmetler tarafından sunulur oluşturur.

Kilitlenmeler, hatalar veya performans sorunları varsa, nedenini tanılamak için ayrıntılı telemetri verilerde arama yapabilirsiniz. Ve hizmet kullanılabilirlik ve performans, uygulamanızın herhangi bir değişiklik varsa e-posta gönderir. Gizlilik, bütünlük ve kullanılabilirlik güvenlik Üçlü kullanılabilirlik ile yardımcı olması uygulama Insight böylece değerli güvenlik aracı haline gelir.

### <a name="azure-monitor"></a>Azure İzleyici
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) görselleştirme, sorgu, yönlendirme, uyarı, otomatik ölçeklendirme ve Otomasyon verileri hem de Azure altyapısından sunar ([etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)) ve tek tek her Azure kaynak ([tanılama günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)). Azure günlükleri tarafından oluşturulan güvenlikle ilgili olaylar hakkında sizi uyarmak için Azure İzleyicisi'ni kullanabilirsiniz.

### <a name="log-analytics"></a>Log Analytics
[Günlük analizi](https://azure.microsoft.com/documentation/services/log-analytics/) parçası [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – hem şirket içi hem de üçüncü taraf bulut tabanlı altyapı (AWS gibi) Azure kaynaklarını yanı sıra bir BT yönetim çözümü sunar. Tek bir yerde, tüm ortamınız için ölçümleri ve günlükleri görebilmeniz için Azure İzleyici verileri doğrudan günlük analizi için yönlendirilebilir.

Günlük analizi aracı ile ilgili girdileri esnek sorgu yaklaşımı büyük miktarlarda aracılığıyla hızlı bir şekilde aramanıza olanak tanır adli ve diğer güvenlik analizi yararlı bir aracı olabilir. Ayrıca, şirket içi [güvenlik duvarı ve proxy günlüklerini Azure'a dışarı ve günlük analizi kullanarak analiz için kullanılabilir.](https://docs.microsoft.com/azure/log-analytics/log-analytics-proxy-firewall)

### <a name="azure-advisor"></a>Azure Advisor
[Azure Danışmanı](https://docs.microsoft.com/azure/advisor/) Azure dağıtımlarınızı iyileştirmek için yardımcı olan kişiselleştirilmiş bulut Danışman değil. Kaynak yapılandırmanızı ve kullanım telemetrinizi çözümler. Ardından geliştirmeye yardımcı olmak için çözüm önerir [performans](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), [güvenlik](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations), ve [yüksek kullanılabilirlik](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations) fırsatları arayan çalışırken, kaynaklarınızın [genel Azure azaltmak harcamanız](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations). Azure Danışmanı güvenlik önerileri, geliştirme, genel güvenlik duruşunu Azure'da dağıtmak çözümleri için önemli hangi yollarını sağlar. Bu öneriler tarafından gerçekleştirilen güvenlik analizi gelen çizilir [Azure Güvenlik Merkezi.](https://docs.microsoft.com/azure/security-center/security-center-intro)

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim yoluyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Ayrıca, Azure Güvenlik Merkezi güvenlik işlemleri ile tek bir panoya sağlayarak bu yüzeyleri uyarıları ve sonra hemen işlem yapılması öneriler yardımcı olur. Genellikle, Azure Güvenlik Merkezi Konsolu içinden tek bir tıklatmayla sorunları düzeltebilir.
## <a name="applications"></a>Uygulamalar
Bölüm uygulama güvenlik ve Özet bilgilerini bu özellikleri hakkında temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="web-application-vulnerability-scanning"></a>Web uygulaması güvenlik açığı taraması
Güvenlik açıkları için testi ile çalışmaya başlamak için en kolay yollarından biri, [App Service uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-overview) kullanmaktır [Tinfoil Security ile tümleştirme](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) tek tıklatmayla güvenlik açığı uygulamanıza tarama gerçekleştirmek için. Kolay anlaşılır bir raporda test sonuçlarını görüntülemek ve adım adım yönergeler içeren her bir güvenlik açığı düzeltme öğrenin.

### <a name="penetration-testing"></a>Sızma Testi
Kendi sızma sınamalar gerçekleştirmek veya başka bir tarayıcı paketi ya da sağlayıcı kullanmayı tercih ederseniz, uymanız gereken [onay işlemini test Azure sızma](https://security-forms.azure.com/penetration-testing/terms) ve istenen sızma testleri gerçekleştirmek için önce onay almak.

### <a name="web-application-firewall"></a>Web uygulaması güvenlik duvarı
Web uygulaması Güvenlik Duvarı (WAF) içinde [Azure uygulama ağ geçidi](https://azure.microsoft.com/services/application-gateway/) SQL ekleme gibi ortak web tabanlı saldırıları, siteler arası komut dosyası saldırıları ve oturumu ele geçirme web uygulamalarını korumaya yardımcı olur. Bunu tarafından tanımlanan tehditlere karşı koruma ile önceden yapılandırılmış olarak gelir [açık Web uygulaması güvenlik proje (OWASP) ilk 10 ortak güvenlik açıkları olarak](https://msdn.microsoft.com/library/).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Azure Uygulama Hizmeti’nde kimlik doğrulaması ve yetkilendirme
[App Service kimlik doğrulama / yetkilendirme](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) uygulama arka uç kodunu değiştirmek zorunda değilsiniz böylece kullanıcılar oturum uygulamanız için bir yol sağlayan bir özelliktir. Uygulamanızı korumak ve kullanıcı başına verilerle çalışmak için kolay bir yol sağlar.

### <a name="layered-security-architecture"></a>Katmanlı güvenlik mimarisi
Bu yana [App Service ortamları](https://docs.microsoft.com/azure/app-service/environment/app-service-app-service-environment-intro) içine dağıtılan bir yalıtılmış çalışma zamanı ortamı sağlamak bir [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), geliştiriciler, her uygulama katmanı için ağ erişim düzeyleri farklı sağlayan bir katmanlı güvenlik mimarisi oluşturabilirsiniz. API arka uçları genel Internet erişimine karşı gizlemek ve yalnızca Yukarı Akış web uygulamaları tarafından çağrılmasına izin vermek için ortak bir desire var. [Ağ güvenlik grubu (Nsg'ler)](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) App Service ortamları içeren Azure sanal ağ alt ağlardaki API uygulamaları için ortak erişimi kısıtlamak için kullanılabilir.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Web sunucusu tanılama ve uygulama tanılama
App Service web uygulamalarının web sunucusu ve web uygulamasının içinden bilgileri günlüğe kaydetme için tanılama işlevsellik sağlar. Bu mantıksal olarak ayrılmış [web sunucusu tanılama](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log) ve [uygulama tanılama](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Web sunucusu iki önemli gelişmeler tanılama ve sorun giderme siteleri ve uygulamaları içerir.

İlk yeni uygulama havuzları, çalışan işlem, site, uygulama etki alanları ve çalışan istekleri gerçek zamanlı durum bilgilerini özelliğidir. İkinci yeni avantajları tam istek ve yanıt işlemi boyunca bir isteğin izlenmesi ayrıntılı izleme olaylardır.

Bu izleme olaylarını toplanmasını etkinleştirmek için IIS 7 geçen süre veya hata yanıt kodları göre herhangi belirli bir istek için XML biçiminde tam izleme günlükleri otomatik olarak yakalamak için yapılandırılabilir.

#### <a name="web-server-diagnostics"></a>Web sunucu tanıları
Etkinleştirmek veya günlükleri şu tür devre dışı bırakabilirsiniz:

-   Hata günlüğü - ayrıntılı ayrıntılı hata bilgileri (durum kodu 400 veya daha büyük) hatası olduğunu gösteren HTTP durum kodları için. Bu neden sunucu döndürülen hata kodu belirlemek yardımcı olabilecek bilgiler içerebilir.

-   Başarısız istek izleme - ayrıntılı izleme istek ve her bileşenin geçen süre işlemek için kullanılan IIS bileşenlerini de dahil olmak üzere, başarısız istekler hakkında bilgi. Bu site performansını artırabilir ya da döndürülecek belirli bir HTTP hata neden olan yalıtmak çalıştığınız durumlarda yararlı olabilir.

-   Web sunucu günlüğü - HTTP işlemlerini W3C Genişletilmiş günlük dosyası biçimi kullanma hakkında bilgi. Bu, belirli bir IP adresinden işlenen isteklerin ya da kaç istek sayısı gibi genel site ölçümleri belirlerken yararlıdır.

#### <a name="application-diagnostics"></a>Uygulama tanılama
[Uygulama tanılama](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log) bir web uygulaması tarafından üretilen bilgileri yakalamanıza olanak sağlar. ASP.NET uygulamaları kullanabileceğiniz [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) bilgi uygulama tanılama günlüğüne sınıfı. Uygulama tanılamada, olayları, uygulama performansı ile ilgili olanlar ve uygulama arızaları ve hataları ile ilgili olanlar başlıca iki türde vardır. Arızalar ve hatalar ayrılabilir bağlantı, güvenlik ve arıza sorunları daha. Arıza sorunları genellikle uygulama kodundaki bir sorunla ilgilidir.

Uygulama tanılamada, bu şekilde gruplandırılmış olayları görebilirsiniz:

-   Tümü (tüm olayları görüntüler)
-   Uygulama hataları (özel durumları görüntüler)
-   Performans (performans olaylarını görüntüler)

## <a name="storage"></a>Depolama
Bölüm Azure depolama güvenlik ve Özet bilgilerini bu özellikleri hakkında temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlayabilirsiniz. Erişimi kısıtlamak temel alarak [bilmeniz](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri kesinlik temelli veri erişimi için güvenlik ilkelerini zorlamak istiyorsanız kuruluşlar için. Bu erişim haklarını grupları ve belirli bir kapsamda uygulamalara uygun RBAC rolü atayarak verilir. Kullanabileceğiniz [yerleşik RBAC rolleri](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles), ayrıcalıkları kullanıcılara atamak için depolama hesabı katılımcı gibi. Depolama erişim tuşlarını için bir depolama hesabıyla [Azure Resource Manager](https://docs.microsoft.com/azure/storage/storage-security-guide) modeli rol tabanlı erişim denetimi (RBAC) denetlenebilir.

### <a name="shared-access-signature"></a>Paylaşılan erişim imzası
[Paylaşılan erişim imzası (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. SAS anlamına gelir, bir istemci belirli bir süre için ve belirtilen bir izin kümesi ile sınırlı depolama hesabındaki nesnelere izinleri verebilirsiniz. Hesap erişim tuşlarınızı paylaşmak gerek kalmadan bu sınırlı izinleri verebilirsiniz.

### <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
Şifreleme Aktarımdaki ağlar üzerinden iletilirken veri koruma, bir mekanizmadır. Azure Storage ile verileri kullanarak güvenliğini sağlayabilirsiniz:
-   [Aktarım düzeyinde şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), içine veya dışına Azure Storage veri aktarırken HTTPS gibi.

-   [Şifreleme wire](https://docs.microsoft.com/azure/storage/storage-security-guide#using-encryption-during-transit-with-azure-file-shares), gibi [SMB 3.0 şifrelemesi](https://docs.microsoft.com/azure/storage/storage-security-guide) için [Azure dosya paylaşımları](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

-   İstemci tarafı şifreleme, depolama alanına transfer önce verileri şifrelemek ve depolama alanı biterse aktarıldıktan sonra verilerin şifresini çözmek için.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Çoğu kuruluş için bekleyen verileri şifreleme veri gizliliği, uyumluluk ve veri egemenliği doğru zorunlu bir adımdır. "Bekleyen" olan verilerin şifrelenmesini sağlayan üç Azure depolama güvenlik özellikleri şunlardır:

-   [Depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption) depolama birimi hizmeti otomatik olarak verileri Azure depolama birimine yazılırken şifrelemeniz istemenizi sağlar.

-   [İstemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) bekleyen şifreleme özelliği de sağlar.

-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve bir Iaas sanal makine tarafından kullanılan veri disklerle şifrelemenizi sağlar.

### <a name="storage-analytics"></a>Depolama Analizi
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler veriler için bir depolama hesabı sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. Storage Analytics bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgi günlüğe kaydeder. Bu bilgiler, istekleri ayrı ayrı izlemek ve depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. İstekleri en iyi çaba ilkesine göre günlüğe kaydedilir. Kimliği doğrulanmış istekler aşağıdaki türlerini kaydedilir:
-   Başarılı istek sayısı.

-   İstek zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere, başarısız oldu.

-   Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istek sayısı.

-   Analiz verileri için istek sayısı.

### <a name="enabling-browser-based-clients-using-cors"></a>Tarayıcı tabanlı istemcileri CORS kullanarak etkinleştirme
[Çıkış noktaları arası kaynak paylaşımı (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) birbirine birbirlerinin kaynaklarına erişim izni vermek etki alanları sağlayan bir mekanizmadır. Kullanıcı Aracısı başka bir etki alanında bulunan kaynaklara erişmek için belirli bir etki alanından yüklenen JavaScript kodu izin verildiğinden emin olmak için fazladan üstbilgiler gönderir. İkinci etki alanı, ardından izin verme veya reddetme kaynaklarını özgün etki alanı erişimi fazladan üstbilgiler ile yanıt verir.

Hizmet için CORS kuralları ayarladıktan sonra hizmete yönelik farklı bir etki alanından yapılan düzgün bir şekilde kimliği doğrulanmış bir isteği, belirlediğiniz kurallara göre izin verilip verilmediğini belirlemek için değerlendirilir böylece azure storage Hizmetleri artık CORS desteği.
## <a name="networking"></a>Ağ
Bölüm Azure ağ güvenliği ve Özet bilgilerini bu özellikleri hakkında temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="network-layer-controls"></a>Ağ katmanı denetimleri
Ağ erişim denetimi ve belirli aygıtları veya alt ağlar sınırlama, işlemidir ve ağ güvenliği çekirdek temsil eder. Ağ erişim denetimi amacı, sanal makineleri ve Hizmetleri yalnızca kullanıcılar ve aygıtlar için bunları erişilebilir istediğiniz erişilebilir olduğundan emin olmaktır.

#### <a name="network-security-groups"></a>Ağ Güvenlik Grupları
A [ağ güvenlik grubu (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) temel bir durum bilgisi olan paket güvenlik duvarı ve filtreleme temelinde erişimi denetlemenize olanak sağlayan olan bir [5-tanımlama grubu](https://www.techopedia.com/definition/28190/5-tuple). Nsg'ler erişim denetimleri kimlik doğrulamalı veya uygulama katmanı denetleme sağlamaz. Bir Azure sanal ağ içindeki alt ağlara ve bir Azure sanal ağı ve Internet arasında trafiği arasında taşıma trafiği denetlemek için kullanılabilir.

#### <a name="route-control-and-forced-tunneling"></a>Rota denetimi ve zorlamalı tünel
Azure sanal ağlarınızdaki yönlendirme davranışını denetlemek için bir kritik ağ güvenliği ve erişim denetimi yetenektir. Örneğin, Azure sanal ağınızda gelen ve giden tüm trafiği, sanal güvenlik gereç yoluyla gider emin olmak istiyorsanız, denetlemek ve yönlendirme davranışını özelleştirmek olması gerekir. Azure'da kullanıcı tanımlı rotaları yapılandırarak bunu yapabilirsiniz.

[Kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) taşınmasını trafiği gelen ve giden yollar özelleştirmenize olanak sağlar ve tek tek sanal makineleri veya en güvence altına almaya alt dışında rota olası güvenliğini sağlayın. [Zorlanan tünel](https://www.petri.com/azure-forced-tunneling) hizmetlerinizi Internet'teki cihazlar için bir bağlantı başlatmak için izin verilmiyor emin olmak için kullanabileceğiniz bir mekanizmadır.

Bu gelen bağlantıları kabul edilemiyor ve bunlara yanıt farklıdır. Internet ana bilgisayarlarını isteklerini yanıtlamak ön uç web sunucuları gerekir ve bu nedenle Internet kaynaklanan trafiği bu web sunucularına gelen ve web sunucularını yanıtlayabilir izin verilir.

Zorlamalı tünel şirket içi güvenlik proxy ve güvenlik duvarları gitmek için Internet giden trafiği zorlamak için yaygın olarak kullanılır.

#### <a name="virtual-network-security-appliances"></a>Sanal ağ güvenlik uygulamaları
Ağ güvenlik grupları, kullanıcı tanımlı yollar ve zorlamalı tünel ağ ve Aktarım katmanı güvenlik düzeyini sunarken [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), yığınının yüksek düzeyde güvenlik etkinleştirmek istediğiniz zaman zamanlar olabilir. Bu gelişmiş ağ güvenlik özellikleri Azure iş ortağı ağ güvenlik Gereci çözümünü kullanarak erişebilirsiniz. En güncel Azure iş ortağı ağı güvenlik çözümleri ziyaret ederek bulabileceğiniz [Azure Marketi](https://azure.microsoft.com/marketplace/) ve "güvenlik" ve "ağ güvenliği" için arama

### <a name="azure-virtual-network"></a>Azure Sanal Ağ

Azure Virtual Network (VNet) buluttaki kendi ağınızın bir gösterimidir. Azure ağı doku aboneliğinize adanmış mantıksal bir yalıtım olur. Bu ağ içindeki IP adres bloklarını, DNS ayarlarını, güvenlik ilkelerini ve yol tablolarını tam olarak denetleyebilirsiniz. Sanal ağınızı alt ağlara ayırabilir ve Azure Iaas sanal makineleri (VM'ler) koyun ve/veya [bulut hizmetlerini (PaaS rolü örnekleri)](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) Azure sanal ağlar üzerindeki.

Bunun yanı sıra, Azure'ın sunduğu [bağlantı seçeneklerinden](https://docs.microsoft.com/azure/vpn-gateway/) birini kullanarak sanal ağı şirket içi ağınıza bağlayabilirsiniz. Özetle, IP adres blokları üzerinde tam bir kontrol sahibi olarak ve Azure'ın sunduğu kurumsal ölçek avantajıyla, ağınızı Azure'a genişletebilirsiniz.

Azure ağı çeşitli güvenli uzaktan erişim senaryoları destekler. Bunlardan bazıları şunlardır:

-   [Ayrı iş istasyonları bir Azure sanal ağa bağlan](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

-   [Bir Azure sanal ağ VPN ile şirket içi ağ bağlanmak](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

-   [Şirket içi ağ ayrılmış bir WAN bağlantısı ile bir Azure sanal ağınıza bağlanmak](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)

-   [Azure sanal ağları birbirine bağlayan](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)

### <a name="vpn-gateway"></a>VPN Gateway
Azure sanal ağınızda ve şirket içi sitenizi arasındaki ağ trafiğini göndermek için Azure sanal ağınız için bir VPN ağ geçidi oluşturmanız gerekir. A [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) şifrelenmiş trafik ortak bir bağlantı üzerinden gönderir sanal ağ geçidi türüdür. VPN ağ geçitleri, Azure ağ yapısı üzerinden Azure sanal ağlar arasında trafiği göndermek için de kullanabilirsiniz.

### <a name="express-route"></a>Express Route
Microsoft Azure [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar adanmış bir WAN bağlantısı.

![Express Route](./media/azure-security/azure-security-fig1.png)

ExpressRoute ile Microsoft Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantı kurabilirsiniz. Ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden herhangi bir ağdan herhangi bir ağa (IP VP), noktadan noktaya Ethernet ağı veya sanal çapraz bağlantısından bağlantı olabilir. 

ExpressRoute bağlantıları ortak Internet üzerinden geçmemektedir ve bu nedenle VPN tabanlı çözümler daha güvenli kabul edilebilir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) sağlayan bir [uygulama teslim denetleyici (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) çeşitli katman 7 Yük Dengeleme, uygulamanız için sunumu bir hizmet olarak.

![Application Gateway](./media/azure-security/azure-security-fig2.png)

Uygulama ağ geçidi (olarak da bilinen "SSL boşaltma" veya "SSL köprülemesi") için CPU yoğunluklu SSL sonlandırma boşaltarak web grubu verimliliği en iyi duruma getirme imkan tanır. Ayrıca, hepsini dağıtım gelen trafiği, tanımlama bilgisi tabanlı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir uygulama ağ geçidi arkasında birden çok Web sitesini barındırmak için özelliği de dahil olmak üzere diğer katman 7 Yönlendirme yetenekleri sağlar. Azure Application Gateway, bir katman 7 yük dengeleyicidir.

Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar.

Uygulamanın sağladığı HTTP dahil olmak üzere pek çok uygulama teslim denetleyici (ADC) özelliklerini Yük Dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi [Güvenli Yuva Katmanı (SSL)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-powershell) boşaltma, özel sistem durumu araştırmalarının, çoklu site desteği ve diğer birçok.

### <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı
Web uygulaması güvenlik duvarı özelliğidir [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) standart uygulama teslim denetimi (ADC) işlevleri için uygulama ağ geçidini kullanan web uygulamaları için koruma sağlar. Web uygulaması güvenlik duvarı bunu, uygulamaları OWASP tarafından sunulan en yaygın 10 web güvenlik açığının çoğuna karşı koruyarak gerçekleştirir.

![Web Uygulaması Güvenlik Duvarı](./media/azure-security/azure-security-fig1.png)

-   SQL ekleme koruması

-   Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

-   HTTP protokolü ihlallerine karşı koruma

-   Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

-   Robotlar, gezginler ve tarayıcıları önleme

-   Ortak uygulama yapılandırma hataları (diğer bir deyişle, Apache, IIS, vb.) algılanması


Web saldırılarına karşı korunacak merkezi bir web uygulaması, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim tehditlerine karşı uygulamayı daha güvende tutar. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.
### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) , farklı veri merkezlerinde hizmet uç noktaları için kullanıcı trafiğinin dağıtımını denetlemenize olanak tanır. Hizmet uç noktaları traffic Manager tarafından desteklenen Azure VM'ler, Web uygulamaları ve bulut hizmetleri içerir. Traffic Manager’ı harici, Azure dışı uç noktalar için de kullanabilirsiniz. Trafik yöneticisi etki alanı adı sistemi (DNS) istemci isteklerini göre en uygun uç noktasına yönlendirmek için kullandığı bir [trafik yönlendirme yöntemini](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) ve bitiş noktalarının sistem durumu.

Trafik Yöneticisi uç noktası sistem durumu, farklı uygulama gereksinimlerinde uyacak şekilde trafik yönlendirme yöntemleri bir dizi sağlar [izleme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)ve otomatik yük devretme. Trafik Yöneticisi dahil tüm bir Azure bölgesi başarısızlığını hatalarına karşı esnektir.
### <a name="azure-load-balancer"></a>Azure Load Balancer
[Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview), uygulamalarınıza yüksek düzeyde kullanılabilirlik ve ağ performansı sağlar. Gelen trafiği yükü dengelenmiş bir kümede tanımlanmış Hizmetleri sağlıklı örnekleri arasında dağıtır bir katman 4 (TCP, UDP) yük dengeleyici olur. Azure yük dengeleyici için yapılandırılabilir:

-   Gelen Internet trafiği dengelemek için sanal makineler yükleyin. Bu yapılandırma olarak bilinir [Internet'e yönelik Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Yük Dengeleme trafiği sanal ağ içindeki sanal makineler arasında bulut Hizmetleri içindeki sanal makineler arasında veya şirket içi bilgisayarlar ve bir şirket içi sanal ağdaki sanal makineler arasında. Bu yapılandırma olarak bilinir [iç Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview). 

- Belirli bir sanal makine için dış trafiğin ilet

### <a name="internal-dns"></a>İç DNS
Bir sanal ağ Yönetim Portalı'nda ya da ağ yapılandırma dosyasında kullanılan DNS sunucularının listesini yönetebilirsiniz. Müşteri, her sanal ağ için en fazla 12 DNS sunucuları ekleyebilirsiniz. DNS sunucuları belirtirken, müşterinin ortamınız için doğru sırada Müşteri'nin DNS sunucularını listelemek doğrulamak önemlidir. DNS sunucu listelerini hepsini çalışmaz. Bunlar belirtildikleri sırada kullanılır. Listenin ilk DNS sunucusunda erişilemediğini ise, istemci DNS sunucusu değil veya düzgün çalıştığını bağımsız olarak, DNS sunucusunu kullanır. Müşteri'nin sanal ağ için DNS sunucusu düzenini değiştirme, DNS sunucularını listeden kaldırmak ve bunları geri sırada o müşteri eklemek istiyor. DNS "CIA" güvenlik Üçlü kullanılabilirlik yönünü destekler.

### <a name="azure-dns"></a>Azure DNS
[Etki alanı adı sistemi](https://technet.microsoft.com/library/bb629410.aspx), veya DNS, çevirmek için sorumlu (veya çözme) bir Web sitesi ya da hizmet adı IP adresine. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) DNS etki alanı, Microsoft Azure altyapı kullanılarak ad çözümlemesi sağlamak için bir barındırma hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. DNS "CIA" güvenlik Üçlü kullanılabilirlik yönünü destekler.
### <a name="log-analytics-nsgs"></a>Günlük analizi Nsg'ler
Aşağıdaki tanılama günlük kategorileri için Nsg'ler etkinleştirebilirsiniz:
-   Olay: için hangi NSG kuralları VM'ler ve örnek rolleriniz MAC adresine dayalı uygulanır girdiler içeriyor. Bu kurallar durumunun her 60 saniyede toplanır.

-   Kuralları sayaç: girişleri için kaç kez her NSG kuralının uygulandığı Reddet izin verme veya trafiği içerir.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur ve görünürlük artan sağlar ve Azure kaynaklarınızın güvenlik üzerinden, denetim. Azure aboneliklerinize arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur. Ağ önerileri Merkezi güvenlik duvarları, ağ güvenlik grupları, yapılandırma gelen trafiği kuralları ve daha fazla etrafında.

Kullanılabilir ağ önerileri aşağıdaki gibidir:

-   [Yeni nesil güvenlik duvarı ekleme](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) bir Microsoft iş ortağı güvenlik korumaları artırmak için İleri nesil Güvenlik Duvarı (NGFW) eklemek önerir

-   [Rota yalnızca trafiği NGFW aracılığıyla](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall#route-traffic-through-ngfw-only) yapılandırdığınız önerir ağ, VM, NGFW üzerinden gelen trafiğe zorla güvenlik grubu (NSG) kuralları.

-   [Ağ güvenlik grupları alt ağları veya sanal makinelerde etkinleştir](https://docs.microsoft.com/azure/security-center/security-center-enable-network-security-groups) Nsg'ler alt ağları veya VM'ler etkinleştirmenizi önerir.

-   [Uç nokta Internet'e aracılığıyla erişimi kısıtlamak](https://docs.microsoft.com/azure/security-center/security-center-restrict-access-through-internet-facing-endpoints) için Nsg'ler gelen trafik kurallarını yapılandırmak önerir.


## <a name="compute"></a>İşlem

Bu bölüm bu alan ve Özet bilgilerini bu özellikleri hakkında temel özellikleri ile ilgili ek bilgiler sağlar.

### <a name="antimalware--antivirus"></a>Kötü amaçlı yazılımdan koruma & virüsten koruma
Azure Iaas ile sanal makinelerinizi kötü amaçlı dosyalar, reklam ve diğer tehditlerden korumak için Microsoft, Symantec, eğilim mikro, McAfee ve Kaspersky gibi güvenlik satıcılardan kötü amaçlı yazılımdan koruma yazılımı kullanabilirsiniz. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) Azure Cloud Services ve sanal makineler için belirlemek ve virüsler, casus yazılım ve diğer kötü amaçlı yazılımları kaldırmak yardımcı olan bir koruma özelliğidir. Microsoft Antimalware kötü amaçlı veya istenmeyen yazılım kendini yüklemeye veya Azure sistemleriniz üzerinde çalışmaya girişiminde bilinen yapılandırılabilir uyarır sağlar. Microsoft Antimalware ayrıca Azure Güvenlik Merkezi kullanılarak dağıtılabilir

### <a name="hardware-security-module"></a>Donanım güvenlik modülü
Anahtarlar kendilerini korunan sürece şifreleme ve kimlik doğrulama güvenliği değil. Bunları depolayarak güvenlik kritik parolaları ve anahtarlar ve Yönetimi basitleştirebilir [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Anahtar kasası anahtarlarınızı FIPS 140-2 Düzey 2 standartları sertifikalı donanım güvenlik modülleri (HSM'ler) içinde depolama seçeneği sağlar. SQL Server şifreleme anahtarları için yedekleme veya [saydam veri şifreleme](https://msdn.microsoft.com/library/bb934049.aspx) herhangi bir anahtar veya gizli, uygulamalardan ile anahtar kasasına tüm depolanabilir. İzinleri ve korumalı bu öğelere erişimi üzerinden yönetilir [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Sanal makine yedekleme
[Azure yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) sıfır sermaye yatırımı ve en az uygulama verilerinizi koruyan bir çözüm işletim maliyetlerini. Uygulama hataları, veri bozulmasına neden ve insan hataları güvenlik sorunlarına yol açabilir, uygulamalara hatalar ortaya çıkarabilir. Azure Backup ile Windows ve Linux çalıştıran sanal makinelerinizi korunur.

### <a name="azure-site-recovery"></a>Azure Site Recovery
Kuruluşunuzun önemli bir kısmını [iş sürekliliği/olağanüstü durum kurtarma (BCDR)](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) stratejisi kurumsal iş yüklerini ve uygulamaları planlı sürekli çalışmasını nasıl açık ve planlanmayan kesintiler. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) yardımcı olur, çoğaltma, yük devretme ve kurtarma iş yükleri ve uygulamalar, birincil konumunuzun çökmesi durumunda ikincil konum kullanılabilir; böylece düzenlemek.

### <a name="sql-vm-tde"></a>SQL VM TDE'NİN
[Saydam veri şifreleme (TDE)](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault) ve sütun düzeyinde şifreleme (Temizle) SQL server şifreleme özellikleri. Bu şifreleme biçimi şifreleme için kullandığınız şifreleme anahtarlarını depolamak ve yönetmek müşteriler gerektirir.

Azure anahtar kasası (AKV) hizmeti bu anahtarların güvenli ve yüksek oranda kullanılabilir bir konumda yönetim ve güvenlik artırmak için tasarlanmıştır. SQL Server Bağlayıcısı Bu anahtarları Azure anahtar Kasası'ndan kullanmak için SQL Server sağlar.

Şirket içi makineler ile SQL Server çalışıyorsa, şirket içi SQL Server makinenizden Azure anahtar kasası erişmek için izleyebileceğiniz adımlar vardır. Ancak Azure vm'lerinde SQL Server için Azure anahtar kasası tümleştirmeyi özelliğini kullanarak zaman kazanabilirsiniz. Bu özelliği etkinleştirmek için birkaç Azure PowerShell cmdlet'leri ile bir SQL VM anahtar kasanızı erişmek gerekli yapılandırmayı otomatik hale getirebilirsiniz.

### <a name="vm-disk-encryption"></a>VM Disk şifrelemesi
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) yardımcı olan yeni bir özellik, Windows ve Linux Iaas sanal makine diskleriniz şifrelemek değil. Endüstri Standart BitLocker özelliği, Windows ve Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için DM-Crypt özelliği uygular. Çözüm denetlemek ve disk şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde yönetmenize yardımcı olmak için Azure anahtar kasası ile tümleşiktir. Çözüm, aynı zamanda sanal makine disklerdeki tüm veriler Azure depolama alanınızı bekleyen şifrelenmesini sağlar.

### <a name="virtual-networking"></a>Sanal ağ
Sanal makinelerin ağ bağlantısı gerekir. Bu gereksinimi desteklemek için sanal makinelerin bir Azure sanal ağına bağlı Azure gerektirir. Bir Azure sanal ağ üzerinde fiziksel Azure ağ doku yerleşik mantıksal bir yapıdır. Her mantıksal [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) tüm diğer Azure sanal ağlardan yalıtılır. Bu yalıtım dağıtımlarınızı ağ trafiğini diğer Microsoft Azure müşterilerine erişilebilir değil güvence altına yardımcı olur.

### <a name="patch-updates"></a>Düzeltme eki güncelleştirir
Düzeltme eki güncelleştirmeleri bulma ve olası sorunları düzeltmek için temel oluşturur ve yazılım güncelleştirme yönetimi işlemi kuruluşunuza dağıtmadan yazılım güncelleştirme sayısını azaltarak ve uyumluluğu izlemek yeteneğinizi artırarak basitleştirin.

### <a name="security-policy-management-and-reporting"></a>Güvenlik İlkesi Yönetimi ve Raporlama
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) , engellemek, algılamak, yanıt Tehditler ve, artırılmış görünürlük sağlar ve denetim Azure kaynaklarınızın güvenlik üzerinden, yardımcı olur. Azure aboneliklerinize arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

## <a name="identify-and-access-management"></a>Tanımlama ve yönetim erişme

Sistemleri, uygulamaları ve verileri güvenli hale getirme kimlik tabanlı erişim denetimlerini ile başlar. Microsoft iş ürünleri ve Hizmetleri yerleşik kimlik ve erişim yönetimi özellikleri, kurumsal ve kişisel bilgilerinizi yasal kullanıcılara ne zaman ve ihtiyaç duydukları her yerde kullanıma sırasında yetkisiz erişime karşı korunmasına yardımcı.

### <a name="secure-identity"></a>Güvenli kimlik
Microsoft birden çok güvenlik uygulamaları ve teknolojileri, ürün ve hizmetleri arasında kimlik ve erişim yönetmek için kullanır.
-   [Çok faktörlü kimlik doğrulaması](https://azure.microsoft.com/services/multi-factor-authentication/) kullanıcıların erişim, şirket içi ve buluttaki birden çok yöntem kullanmasını gerektirir. Basit bir oturum açma işlemini kullanıcılarla destekleme sırasında güçlü kimlik doğrulaması kolay doğrulama seçeneklerini aralıklı sağlar.

-   [Microsoft Authenticator](https://aka.ms/authenticator) Microsoft Azure Active Directory ve Microsoft hesapları ile çalışır ve wearables ve parmak izi tabanlı onayları desteği içeren kullanıcı dostu bir çok faktörlü kimlik doğrulaması deneyimi sağlar.

-   [Parola ilkesi zorlama](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) düzenli döndürme ve hesap kilitleme başarısız kimlik doğrulaması denemesinden sonra zorlanan uzunluğu ve karmaşıklık gereksinimleri koyarak geleneksel parolaların güvenliğini artırır.

-   [Belirteç tabanlı kimlik doğrulaması](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) Active Directory Federasyon Hizmetleri (AD FS) aracılığıyla kimlik doğrulaması veya üçüncü taraf güvenli belirteç sistemlerde sağlar.

-   [Rol tabanlı erişim denetimi (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) kullanıcılara yalnızca, iş görevlerini gerçekleştirmek için ihtiyaç duydukları erişim miktarını verebilirsiniz kolaylaşır kullanıcının atanan rolüne göre erişim sağlar. Kuruluşunuzun iş modeli ve risk toleransınıza RBAC özelleştirebilirsiniz.

-   [Tümleşik Kimlik Yönetimi (karma kimlik)](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) , kimlik doğrulama ve yetkilendirme tüm kaynaklar için tek bir kullanıcı kimliğini oluşturma iç veri merkezleri ile bulut platform genelinde kullanıcıların erişim denetim tutmanızı sağlar.

### <a name="secure-apps-and-data"></a>Güvenli uygulamalar ve veriler
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/), kapsamlı bir kimlik ve erişim yönetimi bulut çözüm, site uygulamaları ve bulut verilere güvenli erişim yardımcı olur ve kullanıcılar ve gruplar yönetimini basitleştirir. Kimlik Yönetimi, güvenlik ve uygulamaya erişim yönetimi, Gelişmiş çekirdek dizin hizmetlerini birleştirir ve geliştiricilerin uygulamalarını kimlik ilke tabanlı yönetime oluşturmasını kolaylaştırır. Azure Active Directory'yi geliştirmek için Azure Active Directory temel, Premium P1 ve Premium P2 sürümleri kullanılarak Ücretli özellikleri ekleyebilirsiniz.

| Ücretsiz / ortak özellikler     | Temel özellikler    |Premium P1 özellikleri |Premium P2 özellikleri | Azure Active Directory katılım – Windows 10 yalnızca ilgili özellikleri|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Dizin nesnelerini](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#directory-objects), [kullanıcı/Grup Yönetimi (ekleme/güncelleştirme/silme) / kullanıcı tabanlı sağlama, cihaz kaydı](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#usergroup-management-addupdatedelete-user-based-provisioning-device-registration), [çoklu oturum açma (SSO)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#single-sign-on-sso), [bulut kullanıcıları için Self Servis parola değişikliğini](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-change-for-cloud-users), [Connect (Azure Active Directory ile şirket içi dizinlere genişletir eşitleme altyapısı)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-sync-engine-that-extends-on-premises-directories-to-azure-active-directory), [güvenlik / kullanım raporları](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#securityusage-reports)       |     [Grup tabanlı erişim yönetimi / sağlama](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#group-based-access-managementprovisioning), [bulut kullanıcıları için Self Servis parola sıfırlama](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-reset-for-cloud-users), [şirket markası (oturum açma sayfaları/erişim paneli özelleştirme)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#company-branding-logon-pagesaccess-panel-customization), [uygulama proxy'si](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#application-proxy), [% SLA 99,9](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#sla-999) |  [Self Servis grup ve uygulama yönetimi/Self-Servis uygulama eklemeleri/dinamik gruplar](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-group), [Self Servis parola sıfırlama/Değiştir/Unlock ile şirket içi sonradan yazma](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-resetchangeunlock-with-on-premises-write-back), [(Bulut ve şirket içi (MFA sunucusu)) çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#multi-factor-authentication-cloud-and-on-premises-mfa-server), [MIM CAL + MIM sunucusu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mim-cal-mim-server), [Cloud App Discovery](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#cloud-app-discovery), [Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-health), [grup hesapları için otomatik parola geçişi](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#automatic-password-rollover-for-group-accounts)|     [Kimlik koruması](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection), [Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-configure)|    [Bir aygıt için Azure AD, Masaüstü SSO Microsoft Passport Azure AD, yönetici Bitlocker kurtarma katılma](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#join-a-device-to-azure-ad-desktop-sso-microsoft-passport-for-azure-ad-administrator-bitlocker-recovery), [MDM otomatik kayıt, Self Servis Bitlocker kurtarma, Azure AD katılım aracılığıyla Windows 10 cihazlarına ek yerel Yöneticiler](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mdm-auto-enrollment)|


- [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-cloudappdiscovery-whatis) kuruluşunuzdaki çalışanlar tarafından kullanılan bulut uygulamalarını belirlemenize olanak sağlayan Azure Active Directory premium özelliğidir.

- [Azure Active Directory kimlik koruması](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) risk olaylarına ve kuruluşunuzun kimlikleri etkileyebilecek olası güvenlik açıklarını birleştirilmiş görünüme sağlamak için Azure Active Directory anomali algılama özellikleri kullanan bir güvenlik hizmetidir.

- [Azure Active Directory etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/) Azure VM'ler etki alanı denetleyicileri dağıtmak zorunda kalmadan bir etki alanına olanak tanır. Kullanıcılar bu Vm'lere Kurumsal Active Directory kimlik bilgilerini kullanarak oturum açın ve kaynaklara sorunsuz bir şekilde erişebilir.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) ölçeklendirmek için yüz milyonlarca kimlikleri ve mobil arasında tümleştirme tüketiciye yönelik uygulamalar için yüksek oranda kullanılabilir, genel kimlik yönetim hizmeti ve web platformlar. Müşterilerinizin tüm uygulamalarınızın var olan sosyal medya hesaplarını kullanmak özelleştirilebilir deneyimler aracılığıyla oturum açarak veya yeni tek başına kimlik bilgileri oluşturabilirsiniz.

- [Azure Active Directory B2B işbirliği](https://aka.ms/aad-b2b-collaboration) şirket uygulamaları ve verileri seçmeli olarak kendi kendine yönetilen kimliklerini kullanarak erişmek üzere iş ortakları etkinleştirerek, şirketler arası ilişkilerinizi destekler bir güvenli iş ortağı tümleştirme çözümüdür.

- [Azure Active Directory katılım](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) bulut işlevlerini Windows 10 cihazlarına Merkezi Yönetimi genişletmenizi sağlar. Kullanıcıların şirket veya kuruluş bulut Azure Active Directory üzerinden bağlanmasına olanak sağlar ve uygulamalarına ve kaynaklarına erişimi basitleştirir.

- [Azure Active Directory Uygulama proxy'si](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) şirket içinde barındırılan web uygulamalarını SSO ve güvenli uzaktan erişim sağlar.

## <a name="next-steps"></a>Sonraki Adımlar
- [Microsoft Azure Security ile çalışmaya başlama](https://docs.microsoft.com/azure/security/azure-security-getting-started)

Azure’daki hizmet ve verilerinizin güvenliğini sağlamanıza yardımcı olması için kullanabileceğiniz Azure hizmetleri ve özellikleri

- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)

Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim sayesinde tehditleri önleyin, algılayın ve bu tehditlere yanıt verin

- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](https://docs.microsoft.com/azure/security-center/security-center-monitoring)

İlkelerle uyumluluğu izlemek için izleme olanakları Azure Güvenlik Merkezi'nde.
