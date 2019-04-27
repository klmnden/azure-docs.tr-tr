---
title: Azure operasyonel güvenlik en iyi yöntemler | Microsoft Docs
description: Bu makalede Azure işletimsel güvenlik için en iyi yöntemler kümesi sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: terrylan
ms.openlocfilehash: e2678eb7d75921f43a1e51b6a8cefc9925a9adc1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60587028"
---
# <a name="azure-operational-security-best-practices"></a>Azure operasyonel güvenlik en iyi uygulamaları
Azure operasyonel güvenlik hizmetleri, denetimleri ve kullanıcılara sunulan özellikleri verilerini, uygulamalarını ve diğer Azure varlıklarını korumak için ifade eder. Azure operasyonel güvenlik Microsoft'a özgü özellikleri aracılığıyla edinilen bilgileri Bilgi Bankası'na dahil olan bir çerçeve üzerine kurulmuştur dahil olmak üzere [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl), [Microsoft Güvenlik Yanıt Merkezi](https://www.microsoft.com/msrc?rtc=1) program ve siber güvenlik tehditleri hakkındaki ayrıntılı tanıma.

Bu makalede, en iyi güvenlik uygulamaları koleksiyonunu ele alır. Bu en iyi uygulamaları, Azure veritabanı ile güvenliği deneyimlerimizden türetilmiştir ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklar:
-   En iyi nedir
-   Bu en iyi etkinleştirmek istediğiniz neden
-   En iyi etkinleştirme başarısız olursa ne sonuç olabilir
- Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu Azure işletimsel güvenlik en iyi yöntemler makalesi bir fikir birliğine varılmış fikrim, Azure platformu özellikleri ve özellik kümeleri üzerinde temel alır. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Depolama Hizmetleri için beklenmeyen davranış değişikliklerini izleme
Bir bulut ortamında barındırılan dağıtılmış bir uygulamadaki sorunlarını giderme ve Tanılama, geleneksel ortamlarda olandan daha karmaşık olabilir. Uygulamaları bir PaaS veya Iaas altyapısında şirket içi mobil cihaz veya bu ortamların birleşiminde dağıtılabilir. Uygulamanızın ağ trafiği ortak ve özel ağlar çapraz ve uygulamanız birden çok depolama teknolojisi kullanabilirsiniz.

Beklenmeyen değişiklikleri davranış (örneğin, daha yavaş yanıt süresi), uygulamanızın kullandığı depolama hizmetleri sürekli olarak izlemeniz gerekir. Günlük kaydı, ayrıntılı verileri toplamak ve derinlik olası bir sorunu çözümlemek için kullanın. İzleme ve günlüğe kaydetme elde tanılama bilgileri, uygulamanızın karşılaşılan sorunun kök nedenini belirlemek için yardımcı olur. Ardından sorunu gidermek ve çözmek için gerekli uygun adımları belirlemek.

[Azure depolama analizi](../storage/storage-analytics.md) günlüğe kaydetme işlemlerini gerçekleştiren ve bir Azure depolama hesabı için ölçüm verileri sağlar. Bu veri istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızla ilgili sorunları tanılayın kullanmanızı öneririz.

## <a name="prevent-detect-and-respond-to-threats"></a>Önleyin, algılayın ve tehditlere yanıt verin
[Azure Güvenlik Merkezi](../security-center/security-center-intro.md) , önlemenize, algılamanıza ve Artırılmış görünürlük ile tehditleri (ve üzerinde denetim) yardımcı olur, Azure kaynaklarınızın güvenlik. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan geçebilir tehditleri ve çeşitli güvenlik çözümleri ile çalışır algılamaya yardımcı olur.

Güvenlik Merkezi'nin ücretsiz katmanı, yalnızca Azure kaynaklarınız için sınırlı bir güvenlik sunar. Standart katman, şirket içinde ve diğer bulutlarda bu özelliklerini genişletir. Güvenlik Merkezi Standart katmanı; güvenlik açıklarını bulup gidermenize, zararlı etkinlikleri engellemek için erişim ve uygulama denetimleri uygulamanıza, analizden ve bilgilerden yararlanarak tehditleri algılamanıza ve saldırı altındayken hızlıca yanıt vermenize yardımcı olur. Güvenlik Merkezi Standart katmanını ilk 60 gün boyunca hiçbir ücret ödemeden deneyebilirsiniz. Olmasını öneririz, [yerleşik Azure aboneliğinizi Güvenlik Merkezi standart](../security-center/security-center-get-started.md).

Güvenlik Merkezi, tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü elde etmek için kullanın. Bir bakışta, uygun güvenlik denetimlerinin uygulandığını ve doğru yapılandırılmış ve dikkat gerektiren tüm kaynakları hızla tanımlayın doğrulayın.

## <a name="monitor-end-to-end-scenario-based-network-monitoring"></a>Uçtan uca senaryo tabanlı ağ izleme izleyin
Müşteriler, azure'da bir uçtan uca ağ, sanal ağ, ExpressRoute, Application Gateway gibi ağ kaynaklarını birleştirerek yapı ve yük Dengeleyiciler. İzleme kaynakların her biri ağ üzerinde kullanılabilir.

[Azure Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md) bölgesel bir hizmettir. İçinde azure'a veya azure'dan ağ senaryosu düzeyinde koşulları izlemek ve tanılamak için tanılama ve görselleştirme araçları kullanın.

Ağ izleme ve mevcut araçları için en iyi uygulamalar aşağıda verilmiştir.

**En iyi yöntem**: Paket yakalama ile uzaktan ağ izlemeyi otomatikleştirin.  
**Ayrıntı**: İzleyin ve Vm'lerinizi Ağ İzleyicisi'ni kullanarak oturum açmadan ağ sorunlarını tanılayın. Tetikleyici [paket yakalaması](../network-watcher/network-watcher-alert-triggered-packet-capture.md) göre uyarılar ayarlanması ve paket düzeyinde gerçek zamanlı performans bilgilerine erişin. Bir sorunla karşılaştığınızda, daha iyi tanılama için ayrıntılı olarak inceleme yapabilirsiniz.

**En iyi yöntem**: Akış günlüklerini kullanarak ağ trafiğiniz hakkında kazanç Öngörüler.  
**Ayrıntı**: Ağınızın daha derin bir anlayış kullanarak trafik düzenlerini oluşturma [ağ güvenlik grubu akış günlüklerini](../network-watcher/network-watcher-nsg-flow-logging-overview.md). Akış günlüklerindeki bilgiler, uyumluluk, Denetim ve ağ güvenliği profiliniz izleme verileri toplamanıza yardımcı olur.

**En iyi yöntem**: VPN bağlantı sorunlarını tanılayabilirsiniz.  
**Ayrıntı**: Ağ İzleyicisi [en yaygın VPN Gateway ve bağlantı sorunlarını tanılamak](../network-watcher/network-watcher-diagnose-on-premises-connectivity.md). Yalnızca sorunu tanımla ancak daha fazla araştırmak için ayrıntılı günlükleri de kullanabilirsiniz.

## <a name="secure-deployment-by-using-proven-devops-tools"></a>Kendini kanıtlamış DevOps araçlarını kullanarak güvenli dağıtım
Kurumsal ve takımlar ve verimli olmasını sağlamak için aşağıdaki DevOps en iyi yöntemleri kullanın.

**En iyi yöntem**: Derleme ve Dağıtım Hizmetleri otomatik hale getirin.  
**Ayrıntı**: [Kod olarak altyapı](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code) teknikleri kümesidir ve, BT uzmanlarının yardımcı yöntemler günlük derleme yükünü ve modüler altyapının kaldırın. BT uzmanlarının oluşturmasına ve bunların nasıl geliştiricilerine oluşturmasına ve uygulama kodu korumasına gibi bir şekilde modern sunucu ortamında korumasına olanak sağlar.

Kullanabileceğiniz [Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) bildirim temelli bir şablon kullanarak uygulamalarınıza sağlamak için. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanın.

**En iyi yöntem**: Otomatik olarak oluşturun ve Azure web Apps'e dağıtma veya Bulut Hizmetleri.  
**Ayrıntı**: Azure işlem hatları için kullanabileceğiniz [otomatik olarak oluşturma ve dağıtma](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts) Azure web apps veya cloud services için. Azure işlem hatları otomatik olarak dağıtır ikili bir derleme kodunu her iade Azure'a yaptıktan sonra. Paket oluşturma işlemi, Visual Studio'da paket komut eşdeğerdir ve yayımlama adımları Visual Studio'da Yayımla komutunu eşdeğerdir.

**En iyi yöntem**: Sürekli dağıtımı kullanın.  
**Ayrıntı**: [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts) birden çok aşama dağıtımı otomatik hale getirme ve yayın işlemini yönetmek için bir çözümdür. Hızlı, kolay ve sık yayımlamak üzere yönetilen sürekli dağıtım işlem hatları oluşturun. Azure işlem hatları, sürüm işlemini otomatik hale getirebilirsiniz ve onay iş akışlarını önceden tanımlanmış. Şirket içinde dağıtın ve buluta genişletin ve gerektiği gibi özelleştirin.

**En iyi yöntem**: Başlatın veya güncelleştirmeleri üretim ortamına dağıtmak için önce uygulamanızın performansını kontrol edin.  
**Ayrıntı**: Bulut tabanlı çalıştırma [yük testleri](https://docs.microsoft.com/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) Test planlarına Azure kullanarak:

- Uygulamanızda performans sorunlarını bulun.
- Dağıtım kalitesini geliştirin.
- Uygulamanızı her zaman kullanılabilir olduğundan emin olun.
- Uygulamanızı bir sonraki başlatma veya Pazarlama kampanyanız için trafiği işleyebildiğinden emin olun.

**En iyi yöntem**: Uygulama performansını izleme.  
**Ayrıntı**: [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) olan birden çok platformda web geliştiricilerine yönelik bir Genişletilebilir uygulama performans yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için Application Insights'ı kullanın. Bunu, performans anormalliklerini otomatik olarak algılar. Bu sorunları tanılamanıza yardımcı olur ve kullanıcıların gerçekten uygulamanızla neler anlamak için analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

## <a name="mitigate-and-protect-against-ddos"></a>Azaltmak ve DDoS karşı koruyun
Dağıtılmış engelleme (DDoS) hizmetinin, uygulama kaynaklarını tüketebilir dener saldırı türüdür. Uygulama kullanılabilirliği ve meşru istekler işleyebilme etkileyen olmaktır. Bu saldırıların daha karmaşık ve daha büyük boyuta ve etkisi gelmektedir. İnternet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedefleyebilir.

Tasarlama ve oluşturma için DDoS dayanıklılık, planlama ve tasarlama için hata modlarını çeşitli gerektirir.

Azure'da DDoS dayanıklı hizmetler oluşturmaya yönelik en iyi uygulamalar aşağıda verilmiştir.

**En iyi yöntem**: Güvenlik, tasarım ve uygulamadan dağıtım ve işletime kadar uygulamanın tüm yaşam döngüsü boyunca bir öncelik olduğundan emin olun. Uygulamalar düşük bir çok fazla kaynak, bir hizmet kesintisi kaynaklanan kullanmak için istek hacmi izin hataları olabilir.  
**Ayrıntı**: Microsoft Azure'da çalışan bir hizmetin korumaya yardımcı olmak için uygulama mimarinizin iyi anlamış olmanız ve gerekir odaklanmak [, yazılım kalitesinin beş yapı taşına](https://docs.microsoft.com/azure/architecture/guide/pillars). Normal trafik birimler, uygulama ve diğer uygulamalar ve genel internet'e bağlı hizmet uç noktaları arasında bağlantı modeli bilmeniz gerekir.

Uygulamanın bir uygulama tarafından hedeflenen hizmet reddi işlemek için dayanıklı olduğundan emin olmanın en çok önemlidir. Güvenlik ve gizlilik yerleşik olarak Azure platformu ile başlayarak, [Security Development Lifecycle (SDL)](https://www.microsoft.com/en-us/sdl). SDL her geliştirme aşaması sırasında güvenlik yöneliktir ve Azure sürekli daha güvenli hale getirmek için güncelleştirilmesini sağlar.

**En iyi yöntem**: Uygulamalarınızı tasarlamak [yatay olarak genişletmek](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) özellikle bir DDoS saldırısının olması durumunda, yükseltilmiş bir yük talebi karşılamak üzere. Uygulamanızın bağımlı hizmetinin tek bir örneği, bir tek hata noktası oluşturur. Birden fazla sağlama sisteminize karşı daha dayanıklı ve daha ölçeklenebilir hale getirir.  
**Ayrıntı**: İçin [Azure App Service](../app-service/app-service-value-prop-what-is.md)seçin bir [App Service planı](../app-service/overview-hosting-plans.md) , birden çok örneği sunar.

Azure Cloud Services için her rolünüz kullanmak için yapılandırma [birden çok örneği](../cloud-services/cloud-services-choose-me.md).

İçin [Azure sanal makineler](../virtual-machines/windows/overview.md), VM Mimarinizi birden fazla VM içerir ve her VM'nin dahil olun bir [kullanılabilirlik kümesi](../virtual-machines/virtual-machines-windows-manage-availability.md). Sanal makine ölçek kümeleri için otomatik ölçeklendirme özellikleri öneririz.

**En iyi yöntem**: Bir uygulamada güvenlik savunmaları katmanlama başarılı bir saldırı olasılığını azaltır. Azure platformunun yerleşik özellikleri kullanarak uygulamalarınız için güvenli bir tasarım uygulayın.  
**Ayrıntı**: Uygulama boyutu (yüzey) ile saldırı riskini artırır. Kullanıma sunulan IP adres alanı kapatmak için beyaz listeye ekleme özelliğini kullanarak ve dinleme load balancer'ları üzerinde gerekli olmayan bağlantı noktaları'nın yüzey alanını azaltabilirsiniz ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) ve [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)).

[Ağ güvenlik grupları](../virtual-network/security-overview.md) saldırı yüzeyini azaltmak için başka bir yoludur. Kullanabileceğiniz [hizmet etiketleri](../virtual-network/security-overview.md#service-tags) ve [uygulama güvenlik grupları](../virtual-network/security-overview.md#application-security-groups) güvenlik kuralları oluşturmadan ve ağ güvenliğini uygulamanın yapısının doğal bir uzantısı yapılandırmak için karmaşıklığını en aza indirmek için.

Azure hizmetlerini dağıtmanız gerekir bir [sanal ağ](../virtual-network/virtual-networks-overview.md) mümkün olduğunda. Bu yöntem, özel IP adresleri aracılığıyla iletişim kurmak hizmet kaynakları sağlar. Azure hizmet trafiği sanal ağınızdan genel IP adresleri, varsayılan olarak kaynak IP adresleri olarak kullanır.

Kullanarak [hizmet uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md) anahtarları hizmet trafiği kaynak IP adresleri olarak sanal ağ özel adreslerini bir sanal ağdan Azure hizmetine erişirken kullanılacak.

Genellikle müşterilerin şirket kaynakları Azure kaynaklarını birlikte Saldırıya uğrayan görüyoruz. Bir şirket içi ortamı Azure'da bağlanıyorsanız, şirket içi kaynaklara genel İnternet'e riskini en aza indirin.

Azure sahip iki DDoS [hizmet](../virtual-network/ddos-protection-overview.md) ağ saldırılarına karşı koruma sağlayın:

- Temel korumayı, ek ücret ödemeden varsayılan olarak Azure'a tümleşiktir. Ölçeklendirme ve küresel çapta dağıtılan Azure ağ kapasitesini her zaman açık trafik izleme ve gerçek zamanlı azaltma aracılığıyla ortak ağ katmanı saldırılarına karşı koruma sağlar. Temel yapılandırma veya uygulama herhangi bir kullanıcı değişiklik gerektirmez ve tüm Azure Hizmetleri, Azure DNS gibi PaaS hizmetlerine korunmasına yardımcı olur.
- Standart koruma, ağ saldırılarına karşı Gelişmiş DDoS düzeltme özellikleri sağlar. Ayrıca, belirli Azure kaynaklarınızı korumak için otomatik olarak ayarlanmıştır. Koruma, sanal ağlar oluşturma sırasında etkinleştirmek basit bir işlemdir. Oluşturulduktan sonra yapılabilir ve herhangi bir uygulamayı veya kaynağı bir değişiklik gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.

Aşağıdaki kaynaklar, Azure güvenliği ve ilgili Microsoft Hizmetleri hakkında daha fazla genel bilgi sağlamak kullanılabilir:
* [Azure güvenlik ekibi blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği en güncel bilgi için
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar dahil Microsoft güvenlik açıklarının bildirilebileceği veya e-posta aracılığıyla secure@microsoft.com
