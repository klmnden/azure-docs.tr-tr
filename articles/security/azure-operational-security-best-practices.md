---
title: "Azure işletimsel güvenlik en iyi uygulamalar | Microsoft Docs"
description: "Bu makale Azure işletimsel güvenlik için en iyi yöntemler kümesi sağlar."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: ced8ecde1f36c49b479c7b253a90614567783663
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="azure-operational-security-best-practices"></a>Azure işletimsel güvenlik en iyi uygulamalar
Azure işletimsel güvenlik hizmetleri, denetimleri ve kullanıcılar için kullanılabilir özellikler verilerini, uygulamaları ve diğer Microsoft Azure varlıkları korumak için ifade eder. Azure işlem güvenliği Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center programı da dahil olmak üzere Microsoft'a özgü çeşitli özellikleri aracılığıyla elde edilen bilgilerden içerir çerçevesi üzerine inşa edilmiştir, ve siber güvenlik tehdit derin farkındalığınızı.

Bu makalede, en iyi güvenlik uygulamaları, Azure veritabanı koleksiyonunu tartışın. Bu en iyi uygulamaları bizim deneyimlerden Azure veritabanı güvenliği ile elde edilen ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklamaktadır:
-   En iyi uygulama nedir
-   Bu en iyi uygulama etkinleştirmek istediğiniz neden
-   En iyi uygulama olarak etkinleştirmek başarısız olursa ne sonucu olabilir
- Nasıl en iyi uygulama olarak etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu Azure işletimsel güvenlik en iyi yöntemler makalesi anlaşma fikir ve Azure platformu özellikleri ve özellik kümeleri dayanır. Zaman içinde görüşlerini ve teknolojileri değiştirebilirsiniz ve bu makalede bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure işlem en iyi yöntemler şunlardır:

-   İzleme, yönetme ve bulut altyapısını koruma
-   Kimlik ve çoklu oturum açma (SSO) uygulama
-   Kullanım Eğilimleri çözümlemek ve tanılama sorunlarını izleyebilirsiniz istekleri
-   Hizmetleri Merkezi bir izleme çözümü ile izleme
-   Engellemenize, algılamanıza ve tehditlerine karşı yanıt
-   Uçtan uca senaryo tabanlı ağ izleme
-   Güvenli dağıtım başarısı kanıtlanmış DevOps araçlarını kullanma

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>İzleme, yönetme ve bulut altyapısını koruma
BT işlemleri veri merkezi altyapı, uygulamaları ve verileri kararlılığını ve bu sistemlerin güvenliğini dahil olmak üzere, yönetmekle sorumlu. Ancak, karmaşık BT ortamları arasında genellikle artırma güvenlik bilgileri elde birlikte birden çok güvenlik ve yönetim sistemlerine ait verileri cobble kuruluşların gerektirir.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) , yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür.

[OMS güvenlik ve denetim çözümü](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) etkinleştirir etkin olarak yardımcı olabilecek tüm kaynakları izlemek için BT güvenlik olaylarını etkisini en aza indirmek. OMS güvenlik ve denetim kaynakları izlemek için kullanılan güvenlik etki alanları sahiptir.

OMS hakkında daha fazla bilgi için bkz. [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

Engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olması için [Operations Management Suite (OMS) güvenlik ve denetim çözümü](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) içeren veri kaynaklarınızı ilgili işler ve toplar:

-   Güvenlik olay günlüğü
-   Windows için Olay İzleme (ETW) olayları
-   AppLocker denetim olayları
-   Windows Güvenlik Duvarı günlüğü
-   Gelişmiş Threat Analytics olayları
-   Temel değerlendirmesinin sonuçları
-   Kötü amaçlı yazılımdan koruma değerlendirmesinin sonuçları
-   Güncelleştirme/düzeltme eki değerlendirmesinin sonuçları
-   Aracı üzerinde açıkça etkin Syslog akışlar


## <a name="manage-identity-and-implement-single-sign-on"></a>Kimlik ve çoklu oturum açmayı uygulamak
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) Microsoft'un çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti.

[Azure AD](https://azure.microsoft.com/services/active-directory/) da tam dizisi içerir [Kimlik Yönetimi](https://docs.microsoft.com/azure/security/security-identity-management-overview) yetenekleri de dahil olmak üzere [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), cihaz kaydı, Self Servis parola yönetimi Self Servis Grup Yönetimi, ayrıcalıklı hesap yönetimi, rol tabanlı erişim denetimi, uygulama kullanımını izleme, zengin denetim ve güvenlik izleme ve uyarma.

Aşağıdaki özellikleri bulut tabanlı uygulamaların güvenliğini sağlamak, BT işlemlerini kolaylaştırır, maliyetleri kesebilir ve kurumsal uyumluluk hedefleri karşılandığından emin olun yardımcı olur:

-   Bulut için kimlik ve erişim yönetimi
-   Tüm bulut uygulamalarına kullanıcı erişimini basitleştirin
-   Hassas verileri ve uygulamaları koruyun
-   Çalışanlarınıza self-servis olanağı sağlayın
-   Azure Active Directory ile tümleştirin

### <a name="identity-and-access-management-for-the-cloud"></a>Bulut için kimlik ve erişim yönetimi
Azure Active Directory (Azure AD) şu kapsamlı bir [kimlik ve erişim yönetimi bulut çözümü](https://www.microsoft.com/cloud-platform/identity-management), sağlayan, güçlü bir kullanıcı ve grupları yönetmek için özellikler kümesi. Güvenli erişim için şirket içi ve bulut uygulamalarında hizmet (SaaS) uygulamaları olarak Office 365 ve kadar Microsoft dışı yazılımlar gibi Microsoft web hizmetlerini de dahil olmak üzere, yardımcı olur.
Azure AD kimlik korumayı etkinleştirmek nasıl daha fazla bilgi için bkz: [etkinleştirme Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-to-any-cloud-app"></a>Tüm bulut uygulamalarına kullanıcı erişimini basitleştirin
[Çoklu oturum açmayı etkinleştir](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) kullanıcı erişimi, Windows, Mac, Android ve iOS cihazlarından bulut uygulamalarını binlerce basitleştirmek için. Kullanıcıların kişiselleştirilmiş web tabanlı erişim paneli veya şirket kimlik bilgilerini kullanarak mobil uygulama uygulamalardan başlatabilirsiniz. SaaS uygulamaları gidin ve yüksek güvenlikli uzaktan erişim ve çoklu oturum açma sağlamak için şirket içi web uygulamaları yayımlamak için Azure AD uygulama proxy'si modülü kullanın.

### <a name="protect-sensitive-data-and-applications"></a>Hassas verileri ve uygulamaları koruyun
Etkinleştirme [Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) şirket içi yetkisiz erişimi engellemek ve bulut uygulamalarında ek bir kimlik doğrulama düzeyi sağlayarak. Tutarsız erişim düzenlerini belirleyen güvenli izleme, uyarılar ve makine öğrenme tabanlı güvenlik raporları ile işinizi koruyun ve olası tehditleri azaltın.

### <a name="enable-self-service-for-your-employees"></a>Çalışanlarınıza self-servis olanağı sağlayın
Çalışanlarınıza parolaları sıfırlama, grupları oluşturma ve yönetme gibi önemli görevler verin. [Self Servis parola değişikliğini etkinleştir](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)sıfırlayın ve Self Servis Grup Yönetimi Azure AD ile.

### <a name="integrate-with-azure-active-directory"></a>Azure Active Directory ile tümleştirin
Genişletme [Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) ve diğer tüm bulut tabanlı uygulamalar için çoklu oturum açmayı etkinleştirmek için Azure ad dizinleri şirket. Kullanıcı öznitelikleri, tüm şirket içi dizin türlerindeki bulut dizininizle otomatik olarak eşitlenebilir.

Azure Active Directory ve bunun nasıl etkinleştirileceğini tümleştirme hakkında daha fazla bilgi için lütfen makaleyi okuyun [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Kullanım Eğilimleri çözümlemek ve tanılama sorunlarını izleyebilirsiniz istekleri
[Azure Storage Analytics](https://docs.microsoft.com/azure/storage/storage-analytics) günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler veriler için bir depolama hesabı sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz.

Storage Analytics ölçümleri, yeni depolama hesapları için varsayılan olarak etkinleştirilir. Günlük kaydını etkinleştirmek ve ölçümleri ve Azure portalında oturum açmayı yapılandırın; Ayrıntılar için bkz [Azure portalında bir depolama hesabını izleme](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Storage Analytics REST API veya istemci kitaplığı yoluyla programlı olarak etkinleştirebilirsiniz. Storage Analytics her hizmet için ayrı ayrı etkinleştirmek için hizmet özelliklerini ayarlama işlemi kullanın.

Ayrıntılı bir kılavuz tanımlamak, tanılama ve Azure Storage ile ilgili sorunları gidermek için depolama çözümlemeleri ve diğer araçları kullanma hakkında bilgi için bkz: [izleme, tanılama ve Microsoft Azure Storage sorun giderme](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Azure Active Directory ve bunun nasıl etkinleştirileceğini tümleştirme hakkında daha fazla bilgi için makaleyi okuyun [etkinleştirme ve yapılandırma depolama çözümlemeleri](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>İzleme Hizmetleri
Bulut uygulamalarını birçok taşıma bölümleriyle karmaşıktır. İzleme, uygulamanızı kurma kalmasını sağlamak için veri ve sağlıklı bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur. Ayrıca, uygulamanız hakkında ayrıntılı Öngörüler elde etmek için izleme verilerini kullanabilirsiniz. Bu bilgi, uygulama performansı veya devamlılığını iyileştirmek için yardımcı veya aksi halde el ile müdahale gerektiren Eylemler otomatikleştirmek.

### <a name="monitor-azure-resources"></a>Azure kaynaklarını izleme
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) Azure kaynakları izlemek için tek bir kaynak sağlar platform hizmetidir. Azure İzleyici ile Azure’daki kaynaklardan gelen ölçüm ve günlükleri görüntüleyebilir, sorgulayabilir, yönlendirebilir ve bunlar üzerinde işlem yapabilirsiniz. İzleyici portal dikey penceresi, [İzleyici PowerShell Cmdlet’leri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), [Platformlar Arası CLI](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples) veya [Azure İzleyici REST API’leri](https://msdn.microsoft.com/library/dn931943.aspx) kullanarak bu verilerle çalışabilirsiniz.

### <a name="enable-autoscale-with-azure-monitor"></a>Otomatik ölçeklendirme ile Azure izleyicisini etkinleştirmek
Etkinleştirme [Azure İzleyici otomatik ölçeklendirme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) yalnızca sanal makine ölçek kümeleri (VMSS), bulut Hizmetleri, uygulama hizmeti planları ve app service ortamları için geçerlidir.

### <a name="manage-roles-permissions-and-security"></a>Rolleri yönetme izinleri ve güvenlik
Birçok ekip kesinlikle gerek [izleme erişimi düzenleyen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) veriler ve ayarlar. Örneğin, özel olarak (destek mühendisleri, devops mühendisleri) izleme üzerinde çalışan takım üyeleri sahipseniz veya bir yönetilen hizmet sağlayıcısı kullanıyorsanız, bunları oluşturmak için kendi yeteneği kısıtlama sırasında yalnızca izleme verilerine erişim vermek isteyebilirsiniz, değiştirmek veya kaynakları silin.

Bu hızlı bir şekilde bir kullanıcı Azure içinde yerleşik bir izleme RBAC rolü uygulamak veya kendi özel rol sınırlı izleme izinleri gereken kullanıcılar için yapı gösterilmektedir. Ardından Azure İzleyicisi ile ilgili kaynaklarınızın ve içerdikleri verilere erişimin nasıl sınırlandırmak için güvenlik konuları ele alınmıştır.

## <a name="prevent-detect-and-respond-to-threats"></a>Engellemenize, algılamanıza ve tehditlerine karşı yanıt
Güvenlik Merkezi tehdit algılaması Azure kaynakları, ağ ve bağlı iş ortağı çözümlerinden güvenlik bilgileri otomatik olarak toplayarak çalışır. Bu, genellikle tehditleri tanımlamak için birden fazla kaynaktan bilgileri ilişkilendirerek, bu bilgileri analizleri yaparken. Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur.

-   [Güvenlik ilkesini yapılandırma](https://docs.microsoft.com/azure/security-center/security-center-policies) Azure aboneliğiniz için.
-   Kullanım [Güvenlik Merkezi'nde öneriler](https://docs.microsoft.com/azure/security-center/security-center-recommendations) Azure kaynaklarınızı korumanıza yardımcı olmak için.
-   Gözden geçirin ve geçerli yönetmek [güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim yoluyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Güvenlik Merkezi, Azure'da yerleşik olan kullanımı kolay ve etkili tehdit önleme, saptama ve yanıtlama işlevlerini sunar. Temel işlevler şunlardır:

-   Bulut güvenlik durumu anlama
-   Bulut güvenliğinin denetimini elinize alın
-   Tümleşik bulut güvenliği çözümlerini kolayca dağıtın
-   Tehditleri algılayın ve hızlı yanıt verin

### <a name="understand-cloud-security-state"></a>Bulut güvenlik durumu anlama
Azure Güvenlik Merkezi’ni kullanarak tüm Azure kaynaklarınızın güvenlik durumuna ilişkin genel bir görünüm elde edin. Bir bakışta uygun güvenlik denetimleri yerinde olduğundan ve doğru şekilde yapılandırıldığını doğrulayın ve dikkat gerektiren tüm kaynaklar hızlı bir şekilde tanımlar.

### <a name="take-control-of-cloud-security"></a>Bulut güvenliğinin denetimini elinize alın
Tanımlamak [güvenlik ilkeleri](https://docs.microsoft.com/azure/security-center/security-center-policies) şirketinizin bulut güvenlik gereksinimlerine göre Azure aboneliklerinize için uyarlanmış uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına. Kaynak sahiplerine gerekli denetimleri uygulama sürecinde rehberlik sağlamak üzere ilke odaklı öneriler kullanarak bulut güvenliğinde tahmine yer bırakmayın.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Tümleşik bulut güvenliği çözümlerini kolayca dağıtın
[Güvenlik çözümlerle](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) Microsoft ve onun ortakları, endüstri lideri güvenlik duvarları ve kötü amaçlı yazılımdan koruma dahil olmak üzere. Güvenlik çözümleri dağıtmak için basitleştirilmiş hazırlamadan yararlanın; ağ değişiklikleri bile sizin için yapılandırılır. İş ortaklarının çözümlerinden alınan güvenlik olaylarınız, analiz ve uyarı amacıyla otomatik olarak toplanır.

### <a name="detect-threats-and-respond-fast"></a>Tehditleri algılayın ve hızlı yanıt verin
Tümleşik ve analiz odaklı bir yaklaşımla mevcut ve gelişmekte olan bulut tehditlerinin önüne geçin. Microsoft Genel birleştirerek [tehdit Intelligence](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) ve uzmanlık Öngörüler ile bulut güvenlikle ilgili olayları, Azure dağıtımlar arasında Güvenlik Merkezi, gerçek tehditleri erken algılamak ve hatalı pozitif sonuçları azaltmak yardımcı olur. Bulut güvenlik uyarıları ilgili olaylar ve etkilenen kaynakları da dahil olmak üzere saldırı kampanya Öngörüler verin ve sorunları çözmek ve hızlı bir şekilde kurtarmak için yöntemler önerir.

## <a name="end-to-end-scenario-based-network-monitoring"></a>Uçtan uca senaryo tabanlı ağ izleme
Müşterilerin bir uçtan uca ağ Azure VNet, ExpressRoute, uygulama ağ geçidi, yük Dengeleyiciler ve daha fazlasını gibi çeşitli tek tek ağ kaynaklarına oluşturma ve yönetme tarafından oluşturun. İzleme kaynakların her biri ağ üzerinde kullanılabilir.

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) izlemek ve bir ağ senaryosunda, koşullar tanılamak olanak tanıyan bir bölgesel düzeyde, için ve Azure hizmetidir. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Paket yakalama ile uzaktan ağ izlemeyi otomatikleştirin
Ağ İzleyicisi aracılığıyla sanal makinelerinizde (VM’ler) oturum açmak zorunda kalmadan ağ sorunlarını izleyin ve tanılayın. Tetikleyici [paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) göre uyarıları ayarlama ve gerçek zamanlı performans bilgilerini paket düzeyinde erişim sahibi olursunuz. Bir sorunla karşılaştığınızda, daha iyi tanılama için ayrıntılı olarak inceleme yapabilirsiniz.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Akış günlüklerini kullanarak ağ trafiğiniz hakkında ayrıntılı bilgi edinin
Ağ trafiği düzeni kullanarak, daha derin bir anlayış oluşturmak [ağ güvenlik grubu akış günlükleri](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Akış günlükleri tarafından sağlanan bilgiler, Denetim ve ağ güvenlik profili izleme uyumluluk için veri toplayacak yardımcı olur.

### <a name="diagnose-vpn-connectivity-issues"></a>VPN bağlantısıyla ilgili sorunları tanılayın
Ağ İzleyicisi yeteneği sağlar [VPN ağ geçidi ve ağ bağlantıları, en sık karşılaşılan sorunları tanılamak](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Yalnızca sorunu belirlemek için aynı zamanda daha fazla araştırmak yardımcı olmak için oluşturulan ayrıntılı günlükler kullanmak için izin verme.

Ağ İzleyicisi'ni yapılandırmak ve etkinleştirmek hakkında daha fazla bilgi için lütfen makaleyi okuyun [Ağ İzleyicisi'ni yapılandırma](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Güvenli dağıtım başarısı kanıtlanmış DevOps araçlarını kullanma
Bu liste, Azure DevOps uygulamalarının kuruluşlar ve ekipleri ve verimli hale getirir Microsoft Cloud alanındaki bazılarıdır.

-   **Kod (IaC) olarak altyapı:** kod olarak altyapı teknikleri kümesidir ve BT uzmanlarının yardımcı yöntemler modüler altyapısının yönetim ve günlük yapı ile ilişkili yük kaldırın. BT uzmanlarının oluşturmasına ve bunların nasıl geliştiricilerine oluşturmasına ve uygulama kodu korumasına gibi olacak şekilde modern sunucu ortamında korumasına olanak tanır. Azure için sahip olduğumuz [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) , bildirim temelli bir şablon kullanarak uygulamalarınızı sağlamanıza izin verir. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.
-   **Sürekli tümleştirme ve dağıtım:** Visual Studio Online takım projeleriniz yapılandırabilirsiniz [otomatik olarak oluşturma ve dağıtma](https://www.visualstudio.com/docs/build/overview) Azure web uygulamaları veya Bulut Hizmetleri. VSO kodu her iade sonra bir yapı için Azure yaptıktan sonra ikili dosyaları otomatik olarak dağıtır. Burada açıklanan paket oluşturma işlemi, Visual Studio Paketi komutu eşdeğerdir ve yayımlama adımlarını Visual Studio'da Yayımla komutunu eşdeğerdir.
-   **Yayın Yönetimi:** Visual Studio [yayın Yönetimi](https://msdn.microsoft.com/library/vs/alm/release/overview) çok aşamalı dağıtımı otomatikleştirme ve yayın işlemini yönetmek için harika bir çözümdür. Hızlı, kolay ve genellikle yayımlamayı yönetilen sürekli dağıtım komut zincirleri oluşturun. Sürüm yönetimi ile biz çok bizim yayın süreci otomatik hale getirebilirsiniz ve biz onay iş akışları önceden. Şirket içi dağıtma buluta genişletmek ve gerektiği gibi özelleştirin.
-   **Uygulama performansı izleme:** sorunları algılamak, sorunları çözmek ve uygulamanızı sürekli geliştirin. Canlı uygulamanızdaki sorunları hemen tanılayın. Kullanıcılarınızın bununla neler yaptığını anlayın. Yapılandırma JS kod ve webconfig girişi ekleme kolay konudur ve tüm ayrıntılar portalıyla dakika içinde sonuçlarını görebilirsiniz. [App ınsights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) kuruluşlar sorunları & düzeltme daha hızlı algılanması için yardımcı olur.
-   **Test & otomatik ölçeklendirme yük:** performans sorunlarını dağıtım kalitesini artırmak için uygulamamıza bulabilirsiniz ve uygulamamıza her zaman yukarı ya da iş gereksinimini karşılamak kullanılabilir olduğundan emin olmak için gerekir. Uygulamanızı bir sonraki başlatma veya pazarlama kampanyanızı için trafiği işleyebilir emin olun. Bulut tabanlı çalıştırma başlatın [yük testleri](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) Visual Studio Online ile neredeyse hiçbir zaman.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure işletimsel güvenlik](https://docs.microsoft.com/azure/security/azure-operational-security).
- Daha fazla bilgi için [Operations Management Suite | Güvenlik ve Uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Operations Management Suite güvenlik ve denetim çözümü ile çalışmaya başlama](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).
