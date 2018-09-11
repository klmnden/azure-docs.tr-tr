---
title: Azure operasyonel güvenlik en iyi yöntemler | Microsoft Docs
description: Bu makalede Azure işletimsel güvenlik için en iyi yöntemler kümesi sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: d8f6ad48c62ff2021c91e593cee44dd6f5551247
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297918"
---
# <a name="azure-operational-security-best-practices"></a>Azure operasyonel güvenlik en iyi uygulamaları
Azure operasyonel güvenlik hizmetleri, denetimleri ve kullanıcılara sunulan özellikleri verilerini, uygulamalarını ve diğer varlıklardan Microsoft azure'da korumak için ifade eder. Azure çalışma Güvenliği aracılığıyla edinilen Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center programı da dahil olmak üzere Microsoft'a özgü çeşitli özellikleri bilgi içeren bir çerçevesi üzerine inşa edilmiştir, ve siber güvenlik tehditleri hakkındaki ayrıntılı tanıma.

Bu makalede, en iyi güvenlik uygulamaları, Azure veritabanı koleksiyonu ele alır. Bu en iyi uygulamaları, Azure veritabanı ile güvenliği deneyimlerimizden türetilmiştir ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklar:
-   En iyi nedir
-   Bu en iyi etkinleştirmek istediğiniz neden
-   En iyi etkinleştirme başarısız olursa ne sonuç olabilir
- Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu Azure işletimsel güvenlik en iyi yöntemler makalesi bir fikir birliğine varılmış fikrim, Azure platformu özellikleri ve özellik kümeleri üzerinde temel alır. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure işletimsel güvenlik en iyi uygulamalar şunlardır:

-   İzleme, yönetme ve bulut altyapısını koruma
-   Kimlik Yönetimi ve çoklu oturum açma (SSO) uygulama
-   İstekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve sorunları tanılayın
-   Merkezi bir izleme çözümü ile hizmetlerini izleme
-   Önleyin, algılayın ve tehditlere yanıt verin
-   Uçtan uca senaryo tabanlı ağ izleme
-   Kendini kanıtlamış DevOps araçlarını kullanarak güvenli dağıtım

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>İzleme, yönetme ve bulut altyapısını koruma
BT işlemleri, veri merkezi altyapı, uygulamaları ve kararlılığını ve bu sistemlerin güvenliğini de dahil olmak üzere verileri yönetmek için sorumludur. Ancak, karmaşık BT ortamları arasında genellikle artan güvenlik öngörü kuruluşların verileri birden çok güvenlik ve yönetim sistemlerinden birlikte cobble gerektirir.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan Microsoft'un bulut tabanlı BT yönetim çözümüdür.

[OMS güvenlik ve denetim çözümü](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) BT'nin yardımcı olabilecek tüm kaynaklar, etkin bir şekilde izlemek için güvenlik olayların etkisini en aza indirin. OMS güvenlik ve denetim kaynakları izlemek için kullanılan güvenlik etki alanları vardır.

OMS hakkında daha fazla bilgi için bkz. [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

Önlemenize, algılamanıza ve bu, tehditleri yardımcı olması için [Operations Management Suite (OMS) güvenlik ve denetim çözümü](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) toplar ve işler, kaynaklarınız hakkında aşağıdakileri içeren verileri:

-   Güvenlik olay günlüğü
-   Windows için Olay İzleme (ETW) olayları
-   AppLocker denetim olayları
-   Windows Güvenlik Duvarı günlüğü
-   Gelişmiş Threat Analytics olayları
-   Temel değerlendirmesinin sonuçları
-   Kötü amaçlı yazılımdan koruma değerlendirmesinin sonuçları
-   Güncelleştirme/düzeltme eki değerlendirmesinin sonuçları
-   Aracı üzerinde açıkça etkinleştirilen Syslog akışları


## <a name="manage-identity-and-implement-single-sign-on"></a>Çoklu oturum açmayı uygulamak ve Kimlik Yönetimi
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir.

[Azure AD](https://azure.microsoft.com/services/active-directory/) ayrıca eksiksiz içerir [Kimlik Yönetimi](https://docs.microsoft.com/azure/security/security-identity-management-overview) gibi özellikleri [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), cihaz kaydı, Self Servis parola yönetimi Self Servis Grup Yönetimi, ayrıcalıklı hesap yönetimi, rol tabanlı erişim denetimi, uygulama kullanımını izleme, zengin denetim ve güvenlik izleme ve uyarılar.

Aşağıdaki özellikleri bulut tabanlı uygulamaların güvenliğini sağlamak, BT süreçlerini kolaylaştırabilirsiniz, maliyetleri kesebilir ve kurumsal uyumluluk hedeflerinizi karşılandığından emin olun yardımcı olur:

-   Bulut için kimlik ve erişim yönetimi
-   Tüm bulut uygulamalarına kullanıcı erişimini basitleştirin
-   Hassas verileri ve uygulamaları koruyun
-   Çalışanlarınıza self-servis olanağı sağlayın
-   Azure Active Directory ile tümleştirme

### <a name="identity-and-access-management-for-the-cloud"></a>Bulut için kimlik ve erişim yönetimi
Azure Active Directory (Azure AD), kapsamlı bir [kimlik ve erişim yönetimi bulut çözümü](https://www.microsoft.com/cloud-platform/identity-management), size sağlayan bir dizi güçlü özellik kullanıcıları ve grupları yönetmek için. Güvenli erişim için şirket içi ve bulut uygulamalarına kadar Microsoft dışı yazılım ve Office 365 gibi Microsoft web Hizmetleri olarak hizmet (SaaS) uygulamaları dahil olmak üzere, yardımcı olur.
Azure ad kimlik Koruması'nı etkinleştirmek nasıl daha fazla bilgi için bkz: [etkinleştirme Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-to-any-cloud-app"></a>Tüm bulut uygulamalarına kullanıcı erişimini basitleştirin
[Çoklu oturum açmayı etkinleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) Windows, Mac, Android ve iOS cihazlarından binlerce bulut uygulamasına kullanıcı erişimini kolaylaştırmak için. Kullanıcılar uygulamaları kişiselleştirilmiş web tabanlı erişim panelinden veya mobil uygulama, şirket kimlik bilgilerini kullanarak başlatabilirsiniz. SaaS uygulamalarının ötesine geçebilir ve yüksek düzeyde güvenli uzaktan erişim ve çoklu oturum açma sağlamak için şirket içi web uygulamaları yayımlamak için Azure AD uygulama ara sunucusu modülünü kullanın.

### <a name="protect-sensitive-data-and-applications"></a>Hassas verileri ve uygulamaları koruyun
Etkinleştirme [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) ek bir kimlik doğrulama düzeyi sağlayarak bulut uygulamalarına ve şirket içi yetkisiz erişimi önlemek için. Tutarsız erişim düzenlerini belirleyen güvenli izleme, uyarılar ve makine öğrenme tabanlı güvenlik raporları ile işinizi koruyun ve olası tehditleri azaltın.

### <a name="enable-self-service-for-your-employees"></a>Çalışanlarınıza self-servis olanağı sağlayın
Çalışanlarınıza parolaları sıfırlama, grupları oluşturma ve yönetme gibi önemli görevler verin. [Self Servis parola değişimi etkinleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)sıfırlama ve Self Servis Grup Yönetimi Azure AD ile.

### <a name="integrate-with-azure-active-directory"></a>Azure Active Directory ile tümleştirme
Genişletme [Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) ve diğer dizinleri tüm bulut tabanlı uygulamalar için çoklu oturum açmayı etkinleştirmek için Azure AD'ye şirket. Kullanıcı öznitelikleri, tüm şirket içi dizin türlerindeki bulut dizininizle otomatik olarak eşitlenebilir.

Nasıl etkinleştirileceğini ve Azure Active Directory Tümleştirmesi hakkında daha fazla bilgi edinmek için lütfen bu makaleyi okuyun [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>İstekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve sorunları tanılayın
[Azure depolama analizi](https://docs.microsoft.com/azure/storage/storage-analytics) günlüğe kaydetme işlemlerini gerçekleştiren ve bir depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz.

Storage Analytics ölçümleri, yeni depolama hesapları için varsayılan olarak etkindir. Günlüğe kaydetmeyi etkinleştirme ve ölçüm ve Azure portalında oturum açmayı yapılandırın; Ayrıntılar için bkz [Azure portalında depolama hesabı izleme](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Depolama analizi, her hizmet için ayrı ayrı etkinleştirmek için hizmeti özelliklerini ayarla işlemine kullanın.

Kapsamlı bir kılavuz tanımlamak, tanılamak ve Azure depolama ile ilgili sorunları gidermek için depolama analizi ve diğer araçları kullanma hakkında bilgi için bkz: [izleme, tanılama ve sorun giderme Microsoft Azure depolama](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Bunun nasıl etkinleştirileceğini ve Azure Active Directory Tümleştirmesi hakkında daha fazla bilgi edinmek için makaleyi okuyun [etkinleştirme ve yapılandırma depolama analizi](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>İzleme Hizmetleri
Bulut uygulamaları ile birçok hareketli parçadan karmaşıktır. İzleme, uygulama güncel kalıp emin olmak için veri ve sağlam bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur. Ayrıca, uygulamanızı daha ayrıntılı Öngörüler elde etmek için izleme verilerini kullanabilirsiniz. Hakkında bilgi sahibi uygulama performans ve sürdürülebilirliği geliştirmek için yardımcı veya aksi halde el ile müdahale gerektiren eylemleri otomatikleştirme.

### <a name="monitor-azure-resources"></a>Azure kaynaklarını izleme
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) Azure kaynaklarını izlemeye yönelik tek bir kaynak sağlayan platform hizmetidir. Azure İzleyici ile Azure’daki kaynaklardan gelen ölçüm ve günlükleri görüntüleyebilir, sorgulayabilir, yönlendirebilir ve bunlar üzerinde işlem yapabilirsiniz. İzleyici portal dikey penceresi, [İzleyici PowerShell Cmdlet’leri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), [Platformlar Arası CLI](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples) veya [Azure İzleyici REST API’leri](https://msdn.microsoft.com/library/dn931943.aspx) kullanarak bu verilerle çalışabilirsiniz.

### <a name="enable-autoscale-with-azure-monitor"></a>Azure İzleyici ile otomatik ölçeklendirmeyi etkinleştir
Etkinleştirme [Azure İzleyici otomatik ölçeklendirme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) yalnızca sanal makine ölçek kümeleri (VMSS), bulut Hizmetleri, app service planları ve app service ortamları için geçerlidir.

### <a name="manage-roles-permissions-and-security"></a>Rolleri yönetme izinleri ve güvenlik
Birçok ekip kesinlikle gerek [izlemeye erişim Devletlerde](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) veriler ve ayarlar. Özel İzleme (destek mühendisleri, devops mühendislerine) üzerinde çalışan takım üyeleri sahipseniz veya yönetilen hizmet sağlayıcısı kullanıyorsanız, bunları oluşturmak için kendi yeteneği sınırlandırırken yalnızca izleme verilerine erişimi vermek isteyebilirsiniz, örneğin, değiştirme, veya kaynakları silin.

Bu hızlı bir şekilde bir kullanıcının azure'da yerleşik bir izleme RBAC rolü uygulamak veya izleme sınırlı izinlere ihtiyaç duyan bir kullanıcı için kendi özel rol oluşturma işlemini göstermektedir. Ardından, Azure İzleyici ile ilgili kaynaklarınızı ve içerdikleri verilere erişimi nasıl sınırlamak için güvenlik konuları açıklanmaktadır.

## <a name="prevent-detect-and-respond-to-threats"></a>Önleyin, algılayın ve tehditlere yanıt verin
Güvenlik Merkezi tehdit algılaması Azure kaynakları, ağ ve bağlı iş ortağı çözümlerinden güvenlik bilgileri otomatik olarak toplayarak çalışır. Bu, genellikle tehditleri tanımlamak için birden fazla kaynaktan bilgileri ilişkilendirerek, bu bilgileri analiz eder. Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur.

-   [Güvenlik İlkesi yapılandırma](https://docs.microsoft.com/azure/security-center/security-center-policies) Azure aboneliğiniz için.
-   Kullanım [Güvenlik Merkezi'nde öneriler](https://docs.microsoft.com/azure/security-center/security-center-recommendations) Azure kaynaklarınızı korumanıza yardımcı olacak.
-   Gözden geçirin ve geçerli yönetme [güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim yoluyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Güvenlik Merkezi, Azure'da yerleşik olan kullanımı kolay ve etkili tehdit önleme, saptama ve yanıtlama işlevlerini sunar. Temel işlevler şunlardır:

-   Bulut güvenlik durumunu anlayın
-   Bulut güvenliğinin denetimini elinize alın
-   Tümleşik bulut güvenliği çözümlerini kolayca dağıtın
-   Tehditleri algılayın ve hızlı yanıt verin

### <a name="understand-cloud-security-state"></a>Bulut güvenlik durumunu anlayın
Azure Güvenlik Merkezi’ni kullanarak tüm Azure kaynaklarınızın güvenlik durumuna ilişkin genel bir görünüm elde edin. Bir bakışta, uygun güvenlik denetimlerinin uygulandığını ve doğru bir şekilde yapılandırıldığını doğrulayın ve dikkat gerektiren tüm kaynakları hızla tanımlayın.

### <a name="take-control-of-cloud-security"></a>Bulut güvenliğinin denetimini elinize alın
Tanımlama [güvenlik ilkeleri](https://docs.microsoft.com/azure/security-center/security-center-policies) uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına göre Azure Abonelikleriniz için şirketinizin bulutla ilgili güvenlik ihtiyaçlarına göre uyarlanmış. Kaynak sahiplerine gerekli denetimleri uygulama sürecinde rehberlik sağlamak üzere ilke odaklı öneriler kullanarak bulut güvenliğinde tahmine yer bırakmayın.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Tümleşik bulut güvenliği çözümlerini kolayca dağıtın
[Güvenlik çözümlerini etkinleştirme](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) Microsoft ve ortaklarından, sektör lideri güvenlik duvarları ve kötü amaçlı yazılımdan koruma da dahil olmak üzere. Güvenlik çözümleri dağıtmak için basitleştirilmiş hazırlamadan yararlanın; ağ değişiklikleri bile sizin için yapılandırılır. İş ortaklarının çözümlerinden alınan güvenlik olaylarınız, analiz ve uyarı amacıyla otomatik olarak toplanır.

### <a name="detect-threats-and-respond-fast"></a>Tehditleri algılayın ve hızlı yanıt verin
Tümleşik ve analiz odaklı bir yaklaşımla mevcut ve gelişmekte olan bulut tehditlerinin önüne geçin. Microsoft Genel birleştirerek [tehdit bilgileri](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) ve Öngörüler uzmanlığını Azure dağıtımlarınızda güvenlikle ilgili olayları bulut, Güvenlik Merkezi, hatalı pozitif sonuçları azaltmak ve gerçek tehditleri erkenden algılamanıza yardımcı olur. Bulut güvenliği uyarıları saldırılara yönelik olarak ilgili olaylar ve etkilenen kaynaklar dahil olmak üzere Öngörüler sağlar ve sorunların düzeltilip hızla kurtarılması için yöntemler önerir.

## <a name="end-to-end-scenario-based-network-monitoring"></a>Uçtan uca senaryo tabanlı ağ izleme
Müşteriler azure'da bir uçtan uca ağ sanal ağ, ExpressRoute, Application Gateway, yük Dengeleyiciler ve diğer çeşitli ayrı ağ kaynaklarını oluşturma ve düzenleme oluşturun. İzleme kaynakların her biri ağ üzerinde kullanılabilir.

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) koşulları ağ senaryosu, izlemenizi ve tanılamanızı sağlayan bir bölgesel düzeyde, azure'a veya azure'dan hizmetidir. Ağ Tanılama ve görselleştirme araçları Ağ İzleyicisi ile kullanılabilen anlamanıza, tanılamanıza ve ağınıza azure'da Öngörüler elde etmeye yardımcı olur.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Paket yakalama ile uzaktan ağ izlemeyi otomatikleştirin
Ağ İzleyicisi aracılığıyla sanal makinelerinizde (VM’ler) oturum açmak zorunda kalmadan ağ sorunlarını izleyin ve tanılayın. Tetikleyici [paket yakalaması](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) göre uyarılar ayarlanması ve paket düzeyinde gerçek zamanlı performans bilgilerine erişin. Bir sorunla karşılaştığınızda, daha iyi tanılama için ayrıntılı olarak inceleme yapabilirsiniz.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Akış günlüklerini kullanarak ağ trafiğiniz hakkında ayrıntılı bilgi edinin
Ağ trafik desenini kullanarak, daha derin bir anlayış oluşturmak [ağ güvenlik grubu akış günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Akış günlüklerindeki bilgiler, uyumluluk, Denetim ve ağ güvenliği profiliniz izleme verileri toplamanıza yardımcı olur.

### <a name="diagnose-vpn-connectivity-issues"></a>VPN bağlantısıyla ilgili sorunları tanılayın
Ağ İzleyicisi yeteneği sağlar [en yaygın VPN Gateway ve bağlantı sorunlarını tanılamak](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Yalnızca değil sorunu tanımlamak için aynı zamanda daha fazla inceleme yapabilmeniz için oluşturulan ayrıntılı günlükleri kullanmayı sağlar.

Ağ İzleyicisi'ni yapılandırma ve bunun nasıl etkinleştirileceğini hakkında daha fazla bilgi edinmek için lütfen bu makaleyi okuyun [Ağ İzleyicisi'ni yapılandırma](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Kendini kanıtlamış DevOps araçlarını kullanarak güvenli dağıtım
Bu liste, Azure DevOps uygulamalarını kuruluşlar ve takımlar ve verimli hale getirir bu alandaki Microsoft Cloud bazılarıdır.

-   **Kod olarak altyapı (Iac):** kod olarak altyapı teknikleri kümesidir ve BT profesyonellerine yardımcı yöntemler, modüler altyapısının yönetim ve günlük derleme ile ilişkili yük kaldırın. BT oluşturmasına ve bunların nasıl geliştiricilerine oluşturmasına ve uygulama kodu korumasına gibi bir şekilde modern sunucu ortamında korumasına olanak sağlar. Azure için sahip olduğumuz [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) , bildirim temelli bir şablon kullanarak uygulamalarınıza sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.
-   **Sürekli tümleştirme ve dağıtım:** , Azure DevOps projeleri için yapılandırabileceğiniz [otomatik olarak oluşturma ve dağıtma](https://www.visualstudio.com/docs/build/overview) Azure web apps veya cloud services için. Azure DevOps bir derleme kodunu her iade Azure'a yaptıktan sonra ikili dosyaları otomatik olarak dağıtır. Burada açıklanan paket oluşturma işlemi, Visual Studio'da paket komut eşdeğerdir ve yayımlama adımları Visual Studio'da Yayımla komutunu eşdeğerdir.
-   **Sürüm Yönetimi:** Visual Studio [Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview) çok aşamalı dağıtımı otomatikleştirme ve yayın işlemini yönetmek için harika bir çözümdür. Hızlı, kolay ve sık yayımlamak üzere yönetilen sürekli dağıtım işlem hatları oluşturun. Release Management, biz sürüm işleminiz çok otomatikleştirebilirsiniz ve biz onay iş akışlarını tanımlanmış. Şirket içinde dağıtın ve buluta genişletin ve gerektiği gibi özelleştirin.
-   **Uygulama performansı izleme:** sorunları algılayın, problemleri çözmenize ve uygulamanızı sürekli geliştirin. Canlı uygulamanızdaki sorunları hemen tanılayın. Kullanıcılarınızın bununla neler yaptığını anlayın. Yapılandırma kolay sağlasa da JS kod ve webconfig girişi ekleme ve tüm ayrıntılarıyla portalında dakikalar içinde sonuçları görürsünüz. [App ınsights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) sorunları & düzeltme daha hızlı algılanması için kuruluşlara yardımcı olur.
-   **Test & otomatik ölçeklendirme yük:** dağıtım kalitesini artırmak için uygulamamızı bulabiliriz performans sorunlarını ve uygulamamızı her zaman en veya işletmeye gereksinimini karşılamak kullanılabilir olduğundan emin olmak için gerekiyor. Uygulamanızı bir sonraki başlatma veya Pazarlama kampanyanız için trafiği işleyebilir emin olun. Bulut tabanlı çalıştırmaya başlayın [yük testleri](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) Azure DevOps ile neredeyse hiçbir zaman.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure işletimsel güvenlik](https://docs.microsoft.com/azure/security/azure-operational-security).
- Daha fazla bilgi için [Operations Management Suite | Güvenlik ve Uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Operations Management Suite güvenlik ve denetim çözümünü kullanmaya başlama](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).
