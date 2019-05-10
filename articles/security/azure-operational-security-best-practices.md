---
title: Varlıklarınızı - Microsoft Azure korumak için en iyi güvenlik uygulamaları
description: Bu makalede, verilerinizi, uygulamaları ve diğer Azure varlıklarını koruma için işletimsel en iyi yöntemler kümesi sağlar.
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
ms.date: 05/06/2019
ms.author: terrylan
ms.openlocfilehash: 4a4677b5db730001df75d201d8e6d3149cb928e6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409840"
---
# <a name="azure-operational-security-best-practices"></a>Azure operasyonel güvenlik en iyi uygulamaları
Bu makalede, verilerinizi, uygulamaları ve diğer Azure varlıklarını koruma için işletimsel en iyi yöntemler kümesi sağlar.

Bir fikrim fikir birliği üzerinde en iyi uygulamaları temel alır ve geçerli Azure platform özellikleriyle çalışma ve özellik kümeleri. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="define-and-deploy-strong-operational-security-practices"></a>Tanımlama ve güçlü işletimsel güvenlik uygulamaları dağıtma
Azure operasyonel güvenlik hizmetleri, denetimleri ve kullanıcılara sunulan özellikleri verilerini, uygulamalarını ve diğer Azure varlıklarını korumak için ifade eder. Azure operasyonel güvenlik Microsoft'a özgü özellikleri aracılığıyla edinilen bilgileri Bilgi Bankası'na dahil olan bir çerçeve üzerine kurulmuştur dahil olmak üzere [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl), [Microsoft Güvenlik Yanıt Merkezi](https://www.microsoft.com/msrc?rtc=1) program ve siber güvenlik tehditleri hakkındaki ayrıntılı tanıma.

## <a name="manage-and-monitor-user-passwords"></a>Yönetme ve kullanıcı parolalarını izleme
Aşağıdaki tabloda, kullanıcı parolalarını yönetmeyle ilgili en iyi yöntemlerden bazıları listelenmektedir:

**En iyi yöntem**: Parola koruması uygun düzeyde bulutta olduğundan emin olun.   
**Ayrıntı**: Sunulan yönergeleri [Microsoft parola yönergeleri](https://www.microsoft.com/research/publication/password-guidance/), Microsoft Identity platformları (Microsoft Azure Active Directory ve Active Directory hesabı) kullanıcıları için kapsamlı.

**En iyi yöntem**: Kullanıcı hesaplarınızla ilgili kuşkulu eylemleri izleyin.   
**Ayrıntı**: İçin izleme [risk altındaki kullanıcılar](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk) ve [riskli oturum açma işlemleri](../active-directory/reports-monitoring/concept-risk-events.md) kullanarak Azure AD güvenlik raporları.

**En iyi yöntem**: Otomatik olarak algılamak ve riskli parolaları düzeltin.   
**Ayrıntı**: [Azure AD kimlik koruması](../active-directory/identity-protection/overview.md) olanak tanıyan Azure AD Premium P2 sürümünü özelliğidir:

- Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılama
- Otomatik yanıtları, kuruluşunuzun kimliklerini ilgili algılanan kuşkulu eylemleri için yapılandırma
- Şüpheli olayları araştırmanıza ve bunları çözmek için gereken eylemleri gerçekleştirin

## <a name="receive-incident-notifications-from-microsoft"></a>Microsoft olay bildirimleri alma
Güvenlik operasyon ekibiniz, Microsoft'tan Azure olay bildirimlerini aldığından emin olun. Güvenlik ekibiniz, hızlı yanıt ve olası güvenlik riskleri düzeltmek için Azure kaynaklarını tehlikeye bildiğiniz bir olay bildirim sağlar.

Azure kayıt Portalı'nda güvenlik işlemleri bildirim ayrıntılarını yönetici iletişim bilgilerini içeren emin olabilirsiniz. Bir e-posta adresi ve telefon numarası iletişim bilgileridir.

## <a name="organize-azure-subscriptions-into-management-groups"></a>Yönetim gruplarına Azure abonelikleri düzenleme
Kuruluşunuzun fazla aboneliğiniz varsa, erişim ilkeleri ve bu Aboneliklerdeki uyumluluğunun verimli bir şekilde yönetmek için bir yol gerekebilir. [Azure Yönetim grupları](../governance/management-groups/create.md) Aboneliklerde kapsam düzeyini sağlamak. Yönetim grupları denir kapsayıcılarına abonelikleri düzenlemenize ve yönetim gruplarına, idare koşullar geçerlidir. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan koşulları devralır.

Esnek yapısını Yönetim grupları ve abonelikler bir dizine oluşturabilirsiniz. Her dizin, bir üst düzey yönetim grubunun kök yönetim grubu adı verilir. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Kök yönetim grubu, genel ilkeler ve RBAC atamaları dizin düzeyinde uygulanmasına olanak sağlar.

Yönetim gruplarını kullanmak için bazı en iyi uygulamalar şunlardır:

**En iyi yöntem**: Yeni abonelikler eklendikçe idare öğeleri gibi ilkeler ve izinler uygulamak emin olun.   
**Ayrıntı**: Tüm Azure varlıklarına uygulanan kuruluş genelindeki güvenlik öğelerinin atamak için kök yönetim grubu kullanın. İlkeler ve izinler öğeleri örnekleridir.

**En iyi yöntem**: Her bir kesim içinde denetim ve ilke tutarlılık noktası sağlamak için yönetim gruplarıyla segmentlere ayırma stratejisi üst düzeyde hizalayın.   
**Ayrıntı**: Kök yönetim grubundaki her bir kesim için bir tek yönetim grubu oluşturun. Diğer yönetim gruplarına kökü altındaki oluşturmayın.

**En iyi yöntem**: Yönetim grubu derinliği hem işlemleri hem de güvenlik hampers Karışıklığı önlemek için sınırlar.   
**Ayrıntı**: Kök dahil olmak üzere üç düzeyi hiyerarşinize sınırlayın.

**En iyi yöntem**: Tüm kuruluş için kök yönetim grubuna uygulamak için hangi öğelerin dikkatle seçin.   
**Ayrıntı**: Kök yönetim grubu öğelerinin her bir kaynağın uygulanması gerekiyor açık olması ve düşük etki olmadıklarını emin olun.

İyi adaylar şunlardır:

- Bir açık iş etkisine (örneğin, veri özerkliğiyle ilgili kısıtlamaları) sahip yasal gereksinimleri
- Neredeyse hiç olası negatif gereksinimleriyle denetim etkisiyle veya RBAC ilkesiyle gibi işlemler dikkatlice gözden izin atamalarını etkiler.

**En iyi yöntem**: Dikkatle planlayın ve tüm Kurumsal Çapta değişiklik kök yönetim grubunda (ilke, RBAC modelinde vb.) uygulamadan önce test edin.   
**Ayrıntı**: Değişiklikler kök yönetim grubundaki her kaynak azure'da etkileyebilir. Kuruluş genelinde tutarlılık sağlamak için güçlü bir yol sağlarlar, ancak hata veya yanlış kullanımı üretim işlemleri olumsuz yönde etkileyebilir. Kök yönetim grubu için tüm değişiklikleri bir test laboratuvarı ya da üretim test pilot.

## <a name="streamline-environment-creation-with-blueprints"></a>Ortam oluşturma planlar ile daha verimli hale getirin
[Azure şemaları](../governance/blueprints/overview.md) hizmeti, bulut mimarları ve merkezi bilgi teknolojisi grupları uygular ve kuruluşun standartları, desenleri ve gereksinimleri aynılarını tekrarlanabilir Azure kaynakları tanımlamak sağlar. Azure bir Blueprint'i mümkün kılar geliştirme ekiplerinin hızla oluşturmak ve yeni ortamlarla kümesi yerleşik bileşenlerinin ve bu ortamlarda Kurumsal uyumluluğu içinde oluşturmakta olduğunuz güvenilirlik'kurmak bekleme.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Depolama Hizmetleri için beklenmeyen davranış değişikliklerini izleme
Bir bulut ortamında barındırılan dağıtılmış bir uygulamadaki sorunlarını giderme ve Tanılama, geleneksel ortamlarda olandan daha karmaşık olabilir. Uygulamaları bir PaaS veya Iaas altyapısında şirket içi mobil cihaz veya bu ortamların birleşiminde dağıtılabilir. Uygulamanızın ağ trafiği ortak ve özel ağlar çapraz ve uygulamanız birden çok depolama teknolojisi kullanabilirsiniz.

Beklenmeyen değişiklikleri davranış (örneğin, daha yavaş yanıt süresi), uygulamanızın kullandığı depolama hizmetleri sürekli olarak izlemeniz gerekir. Günlük kaydı, ayrıntılı verileri toplamak ve derinlik olası bir sorunu çözümlemek için kullanın. İzleme ve günlüğe kaydetme elde tanılama bilgileri, uygulamanızın karşılaşılan sorunun kök nedenini belirlemek için yardımcı olur. Ardından sorunu gidermek ve çözmek için gerekli uygun adımları belirlemek.

[Azure depolama analizi](../storage/storage-analytics.md) günlüğe kaydetme işlemlerini gerçekleştiren ve bir Azure depolama hesabı için ölçüm verileri sağlar. Bu veri istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızla ilgili sorunları tanılayın kullanmanızı öneririz.

## <a name="prevent-detect-and-respond-to-threats"></a>Önleyin, algılayın ve tehditlere yanıt verin
[Azure Güvenlik Merkezi](../security-center/security-center-intro.md) , önlemenize, algılamanıza ve Artırılmış görünürlük sağlayarak tehditlere yanıt verin (ve üzerinde denetim) yardımcı olur, Azure kaynaklarınızın güvenlik. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan geçebilir tehditleri ve çeşitli güvenlik çözümleri ile çalışır algılamaya yardımcı olur.

Güvenlik Merkezi'nin ücretsiz katmanı, yalnızca Azure kaynaklarınız için sınırlı bir güvenlik sunar. Standart katman, şirket içinde ve diğer bulutlarda bu özelliklerini genişletir. Güvenlik Merkezi standart bulmak ve güvenlik açıklarını düzeltin, kötü amaçlı etkinliği engellemek, analiz ve zeka kullanarak tehditleri algılamak için erişim ve uygulama denetimleri uygulayın ve saldırı altındayken hızlıca yanıt yardımcı olur. Güvenlik Merkezi Standart katmanını ilk 60 gün boyunca hiçbir ücret ödemeden deneyebilirsiniz. Olmasını öneririz, [Azure aboneliğinizi Güvenlik Merkezi standart sürümüne yükseltme](../security-center/security-center-get-started.md).

Güvenlik Merkezi, tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü elde etmek için kullanın. Bir bakışta, uygun güvenlik denetimlerinin uygulandığını ve doğru yapılandırılmış ve dikkat gerektiren tüm kaynakları hızla tanımlayın doğrulayın.

Güvenlik Merkezi de tümleşir [Windows Defender Gelişmiş tehdit Koruması (ATP)](../security-center/security-center-wdatp.md), kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sağlar. Windows Defender ATP ile tümleştirme, prosesler nokta. Ayrıca, algılamak ve Güvenlik Merkezi tarafından izlenen bir sunucu uç noktaları Gelişmiş saldırıları yanıtlar.

Neredeyse tüm kuruluşlar, günlük bilgilerini farklı sinyal toplama cihazlardan birleştirerek ortaya çıkan tehditleri belirlemenize yardımcı olması için bir güvenlik bilgileri ve Olay yönetimi (SIEM) sistemi vardır. Günlükleri "ilginç" tüm günlük toplama ve analiz çözümleri kaçınılmazdır gürültüden nedir belirlemenize yardımcı olması için bir veri analizi sistemi tarafından analiz edilir.

[Azure Sentinel](../sentinel/overview.md) bir ölçeklenebilir, buluta özgü güvenlik bilgileri ve Olay yönetimi (SIEM) ve güvenlik otomatik düzenleme yanıt (oranında ARTIRDI) çözümüdür. Uyarı algılama, tehdit görünürlük, proaktif avcılık ve otomatik tehdit yanıt aracılığıyla akıllı güvenlik analiz ve tehdit zekasını Azure Sentinel sağlar.

Önlemek, algılamak ve tehditlere yanıt için bazı en iyi yöntemler şunlardır:

**En iyi yöntem**: Bulut tabanlı bir SIEM kullanarak hız ve SIEM çözümünüze ölçeklenebilirliğini artırın.   
**Ayrıntı**: Özelliklerini ve yeteneklerini araştırmak [Azure Gözcü](../sentinel/overview.md) ve bunları ne şu anda işiniz ile ilgili özellikleri karşılaştırma kullanarak şirket içi. Kuruluşunuzun SIEM gereksinimlerini karşılıyorsa Gözcü Azure'ı benimsemeyi göz önünde bulundurun.

**En iyi yöntem**: Araştırma önceliğini belirleyebilmek en önemli güvenlik açıkları bulun.   
**Ayrıntı**: Gözden geçirme, [Azure güvenli puanı](../security-center/security-center-secure-score.md) girişimler Azure Güvenlik Merkezi ile oluşturulmuş ve Azure ilkeleri kaynaklanan önerileri görmek için. Bu öneriler, güvenlik güncelleştirmeleri, uç nokta koruması, şifreleme, güvenlik yapılandırmalarını, eksik WAF, İnternet'e bağlı VM'ler ve çok daha fazlası gibi adres üst riskleri yardımcı olur.

Kıyaslama dış kaynaklara karşı kuruluşunuzun Azure Güvenlik Merkezi'nde Internet güvenliği (CIS) denetimleri için temel alınan güvenli puanı sağlar. Dış doğrulama doğrulamak ve takımınızın güvenlik stratejisi zenginleştirin yardımcı olur.

**En iyi yöntem**: Makineler, ağlar, depolama ve veri hizmetlerini ve uygulamalarını keşfedin ve olası güvenlik sorunlarını önceliklerini belirlemek için güvenlik durumunu izleyin.  
**Ayrıntı**: İzleyin [güvenlik önerilerini](../security-center/security-center-recommendations.md) Güvenlik Merkezi, en yüksek öncelik öğelerinin ile başlatırken.

**En iyi yöntem**: Güvenlik bilgileri ve Olay yönetimi (SIEM) çözümünüze Güvenlik Merkezi uyarılarını tümleştirme.   
**Ayrıntı**: Çoğu kuruluş bir SIEM ile bir analist yanıt gerektiren güvenlik uyarıları için merkezi bir clearinghouse kullanın. Güvenlik Merkezi tarafından oluşturulan işlenen olaylar biri Azure İzleyici kullanılabilir günlükleri, Azure etkinlik günlüğü yayımlanır. Azure İzleyici, tüm izleme verilerinizi bir SIEM aracına yönlendirme için birleştirilmiş bir işlem hattı sunar. Bkz: [Güvenlik Merkezi'nde güvenlik çözümlerini tümleştirme](../security-center/security-center-partner-integration.md#exporting-data-to-a-siem) yönergeler için. Azure Gözcü kullanıyorsanız, bkz. [Connect Azure Güvenlik Merkezi](../sentinel/connect-azure-security-center.md).

**En iyi yöntem**: Azure tümleştirme günlüklerini sıem sistemlerinizden alınabileceği gibi ile.   
**Ayrıntı**: Kullanım [toplayın ve veri dışarı aktarmak için Azure İzleyici](../azure-monitor/overview.md#integrate-and-export-data). Bu uygulama, güvenlik olay araştırması etkinleştirmek için önemlidir ve çevrimiçi günlük tutma sınırlıdır. Azure Gözcü kullanıyorsanız, bkz. [veri kaynağına bağlanın](../sentinel/connect-data-sources.md).

**En iyi yöntem**: Araştırma ve avcılık işlemleri hızlandırın ve uç nokta algılama ve yanıt (EDR) özellikleri, saldırı araştırmasını tümleştirerek hatalı pozitif sonuçları azaltmak.   
**Ayrıntı**: [Windows Defender ATP tümleştirmesini etkinleştirme](../security-center/security-center-wdatp.md#enable-windows-defender-atp-integration) , Güvenlik Merkezi Güvenlik İlkesi aracılığıyla. Tehdit aramaya ve olay yanıtı için Azure Gözcü kullanmayı düşünün.

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
**Ayrıntı**: Azure DevOps projelerinize yapılandırabileceğiniz [otomatik olarak oluşturma ve dağıtma](https://docs.microsoft.com/azure/devops/pipelines/index?azure-devops) Azure web apps veya cloud services için. Azure DevOps bir derleme kodunu her iade Azure'a yaptıktan sonra ikili dosyaları otomatik olarak dağıtır. Paket oluşturma işlemi, Visual Studio'da paket komut eşdeğerdir ve yayımlama adımları Visual Studio'da Yayımla komutunu eşdeğerdir.

**En iyi yöntem**: Yayın yönetimini otomatikleştirin.  
**Ayrıntı**: [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/index?azure-devops) birden çok aşama dağıtımı otomatik hale getirme ve yayın işlemini yönetmek için bir çözümdür. Hızlı, kolay ve sık yayımlamak üzere yönetilen sürekli dağıtım işlem hatları oluşturun. Azure işlem hatları, sürüm işlemini otomatik hale getirebilirsiniz ve onay iş akışlarını önceden tanımlanmış. Şirket içinde dağıtın ve buluta genişletin ve gerektiği gibi özelleştirin.

**En iyi yöntem**: Başlatın veya güncelleştirmeleri üretim ortamına dağıtmak için önce uygulamanızın performansını kontrol edin.  
**Ayrıntı**: Bulut tabanlı çalıştırma [yük testleri](https://docs.microsoft.com/azure/devops/test/load-test/overview.md?view=azure-devops#alternatives) için:

- Uygulamanızda performans sorunlarını bulun.
- Dağıtım kalitesini geliştirin.
- Uygulamanızı her zaman kullanılabilir olduğundan emin olun.
- Uygulamanızı bir sonraki başlatma veya Pazarlama kampanyanız için trafiği işleyebildiğinden emin olun.

[Apache JMeter](https://jmeter.apache.org/) yedekleme güçlü bir topluluk ile ücretsiz, popüler açık kaynak araçtır.

**En iyi yöntem**: Uygulama performansını izleme.  
**Ayrıntı**: [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) olan birden çok platformda web geliştiricilerine yönelik bir Genişletilebilir uygulama performans yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için Application Insights'ı kullanın. Bunu, performans anormalliklerini otomatik olarak algılar. Bu sorunları tanılamanıza yardımcı olur ve kullanıcıların gerçekten uygulamanızla neler anlamak için analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

## <a name="mitigate-and-protect-against-ddos"></a>Azaltmak ve DDoS karşı koruyun
Dağıtılmış engelleme (DDoS) hizmetinin, uygulama kaynaklarını tüketebilir dener saldırı türüdür. Uygulama kullanılabilirliği ve meşru istekler işleyebilme etkileyen olmaktır. Bu saldırıların daha karmaşık ve daha büyük boyuta ve etkisi gelmektedir. İnternet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedefleyebilir.

Tasarlama ve oluşturma için DDoS dayanıklılık, planlama ve tasarlama için hata modlarını çeşitli gerektirir. Azure'da DDoS dayanıklı hizmetler oluşturmaya yönelik en iyi uygulamalar aşağıda verilmiştir.

**En iyi yöntem**: Güvenlik, tasarım ve uygulamadan dağıtım ve işletime kadar uygulamanın tüm yaşam döngüsü boyunca bir öncelik olduğundan emin olun. Uygulamalar düşük bir çok fazla kaynak, bir hizmet kesintisi kaynaklanan kullanmak için istek hacmi izin hataları olabilir.  
**Ayrıntı**: Microsoft Azure'da çalışan bir hizmetin korumaya yardımcı olmak için uygulama mimarinizin iyi anlamış olmanız ve gerekir odaklanmak [, yazılım kalitesinin beş yapı taşına](https://docs.microsoft.com/azure/architecture/guide/pillars). Normal trafik birimler, uygulama ve diğer uygulamalar ve genel internet'e bağlı hizmet uç noktaları arasında bağlantı modeli bilmeniz gerekir.

Uygulamanın bir uygulama tarafından hedeflenen hizmet reddi işlemek için dayanıklı olduğundan emin olmanın en çok önemlidir. Güvenlik ve gizlilik yerleşik olarak Azure platformu ile başlayarak, [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl). SDL her geliştirme aşaması sırasında güvenlik yöneliktir ve Azure sürekli daha güvenli hale getirmek için güncelleştirilmesini sağlar.

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

## <a name="enable-azure-policy"></a>Azure İlkesi'ni etkinleştir
[Azure İlkesi](../governance/policy/overview.md) bir oluşturmak, atamak ve ilkelerini yönetmek için kullandığınız bir Azure hizmetidir. Bu kaynakları, Kurumsal standartlarınız ve hizmet düzeyi sözleşmeleri ile uyumlu kalmasını sağlar bu ilkeler, kaynaklarınız üzerinden kuralları ve etkileri uygular. Azure İlkesi, uyumsuzluk atanan ilkelerle kaynaklarınızı değerlendirerek bu gereksinimini karşılar.

Azure İlkesi, izlemek ve kuruluşunuzun yazılı ilkeyi uygulamak etkinleştirin. Bu uyumluluk şirketin veya yasal güvenlik gereksinimleriyle, hibrit bulut iş yüklerinde güvenlik ilkelerini merkezi olarak yöneterek sağlayacaktır. Bilgi edinmek için nasıl [uyumluluğu zorlamak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md). Bkz: [Azure İlkesi tanım yapısı](../governance/policy/concepts/definition-structure.md) ilkesinin öğeleri'ne genel bakış.

Azure İlkesi benimseyin sonra izlemek için bazı en iyi güvenlik uygulamaları şunlardır:

**En iyi yöntem**: İlke türlerinden etkileri destekler. Bunlar hakkında bilgi edinebilirsiniz [Azure İlkesi tanım yapısı](../governance/policy/concepts/definition-structure.md#policy-rule). İşletme işlemleri olumsuz etkilenebilir tarafından **Reddet** etkisi ve **düzeltme** etkisi yoktur, bu nedenle başlayın **denetim** negatif bir etkiye riskini sınırlamak için etkin ilke.   
**Ayrıntı**: [İlke dağıtımı denetim modunda başlatmak](../governance/policy/concepts/definition-structure.md#policy-rule) ve daha sonra için ilerleme **Reddet** veya **düzeltme**. Test ve geçmeden önce denetim etkisiyle sonuçlarını gözden geçirmek **Reddet** veya **düzeltme**.

Daha fazla bilgi için bkz. [oluşturma ve yönetme uyumluluğu zorlamak için ilke](../governance/policy/tutorials/create-and-manage.md).

**En iyi yöntem**: İlke ihlalleri için izlemekten sorumlu rolleri tanımlamak ve doğru düzeltme eylemi sağlayarak hızlı bir şekilde alınır.   
**Ayrıntı**: Atanan role uyumluluğunu izleme aracılığıyla sahip [Azure portalında](../governance/policy/how-to/get-compliance-data.md#portal) veya aracılığıyla [komut satırı](../governance/policy/how-to/get-compliance-data.md#command-line).

**En iyi yöntem**: Azure İlkesi, bir kuruluşun yazılı ilkeleri teknik bir gösterimidir. Tüm Azure ilkeleri Karışıklığın ve tutarlılık artırmak için kuruluş ilkeleri eşleyin.   
**Ayrıntı**: Belge eşlemesi kuruluşunuzun belgeleri veya Azure'da kuruluş ilkesine başvuru ekleyerek Azure İlkesi kendisini [ilke açıklaması](../governance/policy/concepts/definition-structure.md#display-name-and-description) veya Azure İlkesi [girişim](../governance/policy/concepts/definition-structure.md#initiatives) açıklaması.

## <a name="monitor-azure-ad-risk-reports"></a>Azure AD İzleyici risk raporları
Güvenlik ihlallerini büyük çoğunluğu göz önüne bir yerde saldırganların bir kullanıcının kimliğini çalarak bir ortama erişimi elde edin. Tehlikeye atılmış kimlik keşfetme hiçbir kolay bir görevdir. Azure AD, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılamak için Uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Her kuşkulu eylem adlı bir kayıt depolanır algılanan bir [risk olayı](../active-directory/reports-monitoring/concept-risk-events.md). Risk olayları, Azure AD güvenlik kaydedilir raporlar. Hakkında daha fazla bilgi için okuma [risk altındaki kullanıcılar güvenlik raporu](../active-directory/reports-monitoring/concept-user-at-risk.md) ve [riskli oturum açma işlemleri güvenlik raporu](../active-directory/reports-monitoring/concept-risky-sign-ins.md).

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.

Aşağıdaki kaynaklar, Azure güvenliği ve ilgili Microsoft Hizmetleri hakkında daha fazla genel bilgi sağlamak kullanılabilir:
* [Azure güvenlik ekibi blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği en güncel bilgi için
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar dahil Microsoft güvenlik açıklarının bildirilebileceği veya e-posta aracılığıyla secure@microsoft.com
