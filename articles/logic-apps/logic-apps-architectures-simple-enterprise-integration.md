---
title: Azure tümleştirme hizmetleri - basit Kurumsal tümleştirme
description: Bir Azure Logic Apps ve Azure API Management ile basit Kurumsal tümleştirme düzeni nasıl uygulayacağınıza karar gösteren başvuru mimarisi
author: mattfarm
manager: jonfan
editor: ''
services: logic-apps api-management
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/15/2018
ms.author: LADocs; estfan
ms.openlocfilehash: df86ca5aed405ab5eda05c1dd207f0b6656bfd96
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860206"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Basit Kurumsal tümleştirme - başvuru mimarisi

## <a name="overview"></a>Genel Bakış

Bu başvuru mimarisi, bir dizi Azure tümleştirme hizmetleri kullanan bir tümleştirme uygulaması için kendini kanıtlamış yöntemleri gösterir. Bu mimari HTTP API'lerini, iş akışı ve orchestration gerektiren birçok farklı uygulama desenleri bu temel olarak hizmet verebilir.

*Uygulamasından bir basit noktası için noktası tam bir enterprise service bus tümleştirme teknolojileri birçok olası uygulamaları vardır. Bu mimari serisi Genel tümleştirme uygulaması oluşturmak için uygulanabilir yeniden kullanılabilir bileşen parçalarından kullanıma ayarlar – mimarları uygulamalarını ve altyapısını için uygulanması gerekir hangi belirli bileşenlerin göz önünde bulundurmanız gerekir.*

   ![Mimari diyagramı - basit Kurumsal tümleştirme](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Mimari

Mimari aşağıdaki bileşenlere sahiptir:

- Kaynak grubu. [Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), Azure kaynakları için mantıksal bir kapsayıcıdır.
- Azure API Management. [Azure API Management](https://docs.microsoft.com/azure/api-management/) yayımlama, güvenliğini sağlama ve HTTP API'lerini dönüştürme için tam olarak yönetilen bir platformdur.
- Azure API Management Geliştirici Portal'ı seçin. Her bir Azure API Management örneği ile birlikte gelen bir [Geliştirici Portalı](https://docs.microsoft.com/azure/api-management/api-management-customize-styles), erişim için belgeler, kod örnekleri ve bir API'yi test etme olanağı sağlar.
- Azure Logic Apps. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) kurumsal iş akışı ve tümleştirme oluşturmak için sunucusuz bir platformdur.
- Bağlayıcılar. [Bağlayıcılar](https://docs.microsoft.com/azure/connectors/apis-list) Logic Apps tarafından yaygın olarak kullanılan hizmetlerine bağlanmak için kullanılır. Logic Apps farklı bağlayıcıları yüzlerce zaten var ancak bunlar aynı zamanda özel bir bağlayıcı kullanılarak oluşturulabilir.
- IP adresi. Azure API Management hizmeti sabit bir ortak sahip [IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) ve bir etki alanı adı. Azure-api.net contoso.azure-api.net gibi bir alt etki alanı adıdır. Logic Apps ve Service Bus bir genel IP adresi de; Ancak, bu mimaride biz yalnızca IP adresi, API Management Logic apps Uç noktalara (güvenlik için) çağırmak için erişimi kısıtlayın. Service Bus çağrıları tarafından paylaşılan erişim imzası güvenlidir.
- Azure DNS. [Azure DNS](https://docs.microsoft.com/azure/dns/) DNS etki alanları, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamak için bir barındırma hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. Bir özel etki alanı adı (contoso.com gibi) kullanmak için özel etki alanı adını IP adresine eşleyen DNS kayıtları oluşturun. Daha fazla bilgi için Azure API Management'te özel etki alanı yapılandırma konusuna bakın.
- Azure Active Directory (Azure AD). Kullanım [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/) veya kimlik doğrulaması için başka bir kimlik sağlayıcısı. Azure AD kimlik doğrulaması için erişim API uç noktaları sağlar (geçirerek bir [API Management için JSON Web belirteci](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) doğrulamak için) ve API Management Geliştirici Portalı'nı (yalnızca standart ve Premium Katmanlar) erişim güvenliğini sağlayabilirsiniz.

Bu mimari, işlemi için bazı temel desenleri sahiptir:

2. Birleşik API, Logic Apps kullanarak yerleşiktir; SAAS sistemleri çağrıları işlemlerini, Azure Hizmetleri ve tüm API'leri için API Management yayımladı. [Logic Apps ayrıca yayımlanan](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) API Management Geliştirici Portalı aracılığıyla.
3. Uygulamaları [bir OAuth 2.0 güvenlik belirteci almak](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) Azure Active Directory'yi kullanarak bir API'ye erişim kazanmak için gerekli.
4. Azure API Management [güvenlik belirtecini doğrular](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)ve isteği arka uç API/mantıksal uygulama geçirir.

## <a name="recommendations"></a>Öneriler

Belirli gereksinimlerinizi genel burada açıklanan mimariden farklı olabilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="azure-api-management-tier"></a>Azure API Management katmanı

Temel kullanmak için standart veya Premium katmanlarını çünkü bunlar bir üretim SLA sunar ve Azure bölgesi içinde ölçeği genişletme desteği (birim sayısını katmana göre değişiklik gösterir). Premium katmanı, birçok Azure bölgesinde genişleme da destekler. Temel düzeyinizi gerekli aktarım hızının seçtiğiniz katman ve özelliğini ayarlayın. Daha fazla bilgi için [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).

Çalışırken tüm API Management örnekleri için ücretlendirilir. Ölçeklendirilebilir ve gerekmez, bu düzeyde performans her zaman, API Management'ın saatlik faturalama ve ölçek avantajlarını göz önünde bulundurun.

### <a name="logic-apps-pricing"></a>Logic Apps fiyatlandırma

Logic Apps çalışıyor olarak bir [sunucusuz](logic-apps-serverless-overview.md) modeli – fatura, temel üzerinde eylem ve bağlayıcı yürütme hesaplanır. Bkz: [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/) daha fazla bilgi için. Şu anda mantıksal uygulamalar için hiçbir katman hakkında önemli noktalar vardır.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Zaman uyumsuz API çağrıları için Logic Apps

Logic Apps, düşük gecikme süresi – örneğin zaman uyumsuz gerektirmeyen senaryolar veya hedeflenen uzun süreli API çağrıları en iyi şekilde çalışır. Düşük gecikme süresi gerekiyorsa (örneğin, bir kullanıcı arabirimi engelleyen bir çağrı) Bu API veya işlemi farklı bir teknoloji, örneğin, Azure işlevleri ya da App Service kullanılarak dağıtılan Web API'si kullanarak uygulama öneririz. Bu API olmasını öneririz yine de API Management kullanarak API tüketicilerini fronted.

### <a name="region"></a>Bölge

Ağ gecikmesini en aza indirmek için API Management'ı ve Logic Apps ile aynı bölgede sağlayın. Genellikle, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu dağıtım meta verileri nerede depolandığını belirtir, bir bölgede de vardır ve nereye dağıtım şablonu yürütülmediyse. Kaynak grubu ve kaynakları aynı bölgeye yerleştirin. Bu, dağıtım sırasında kullanılabilirliği iyileştirebilir.

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

API Management yöneticileri eklemelidir [önbelleğe alma ilkeleri](../api-management/api-management-howto-cache.md) hizmetinin ölçeklenebilirliği artırmak ve arka uç hizmetlerini yükünü azaltmak için uygun olduğunda.

Azure API Management temel, standart ve Premium katmanları ile daha yüksek kapasite sunmak için bir Azure bölgesinde genişletilebilir. Yöneticiler, hizmet kullanımını analiz edin ve ölçeğini artırabilir veya uygun şekilde için kapasite ölçüm ölçümleri menü içinde kullanabilirsiniz.

API Management hizmeti ölçeklendirmeye yönelik öneriler:

- Trafik düzenlerini – trafik düzenlerini daha geçici olan müşteriler, hesaba katması gerekiyor ölçeklendirme, artan kapasite büyük gereksinimini sahip olur.
- % 66 yukarıda tutarlı kapasite ölçeğinizi gerek gösterebilir.
- % 20 tutarlı kapasitenin ölçeğini fırsatı gösterebilir.
- Yüklemek için her zaman önerilen üretimde etkinleştirmeden önce API Management hizmetiniz temsili bir yük testi.

Premium katmanı hizmetlerini Azure bölgelerinde kullanıma ölçeklendirilebilir. Bu şekilde dağıtan müşteriler, (% 99,95 oranında % 99,9 aksine) daha yüksek bir SLA elde ve kullanıcılar birden çok bölgede yakın hizmetleri sağlayabilirsiniz.

Logic Apps, sunucusuz modeli, Yöneticiler, hizmet ölçeklenebilirliği ek meselesi yapmak gerekmez. anlamına gelir; Servis talebi karşılamak üzere otomatik olarak ölçeklendirir.

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Makalenin yazıldığı sırada, Azure API Management hizmet düzeyi sözleşmesi (SLA) temel, standart ve Premium katmanları için % 99,9 ' dir. Premium katmanında en az bir birim iki veya daha fazla bölgede dağıtımını yapılandırmalarla % 99,95 oranında SLA'sı vardır.

Makalenin yazıldığı sırada, Azure Logic Apps için hizmet düzeyi sözleşmesi (SLA) % 99,9 ' dir.

### <a name="backups"></a>Yedeklemeler

Azure API Management'ın bir yapılandırma olmalıdır [düzenli olarak yedeklenen](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (uygun şekilde üzerinde değişiklik düzenli göre) ve bir konum veya hizmetin bulunduğu farklı Azure bölgesi içinde depolanan yedek dosyaları. Müşteriler, ardından bunların DR stratejisi için iki seçenekten birini seçebilirsiniz:

1. Bir DR olay yeni bir API Management örneği sağlanır ve yedek geri DNS kayıtlarını repointed.
2. Bir pasif kopyayı başka bir Azure bölgesi (ek ücret alınıyor) yedeklemeler, hizmeti düzenli olarak geri yüklenir için müşterilerin devam eder. Bir DR olayı, hizmeti geri yüklemek için yalnızca DNS kayıtları repointed.

Logic Apps çok hızlı bir şekilde yeniden oluşturulabilir ve sunucusuz gibi bunlar ilişkili Azure Resource Manager şablonunun bir kopyasını kaydederek yedeklenir. Bunlar kaydedilebilmesi için kaynak denetim/tümleşik bir müşterilerin sürekli tümleştirme/sürekli dağıtım (CI/CD) işlem için.

Mantıksal API Management aracılığıyla yayımlanan uygulamalar, güncelleştirilmiş konumlarına gerekir farklı bir veri merkezine taşımanız gerekir. Bu API için arka uç özelliği güncelleştirmek için basit bir PowerShell Betiği gerçekleştirilebilir.

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Üretim, geliştirme için ayrı kaynak grupları oluşturun ve test ortamları. Bu, dağıtımları yönetmeyi, test dağıtımlarını silmeyi ve erişim haklarını atamayı kolaylaştırır.
Kaynakları kaynak gruplarına atarken, aşağıdakileri göz önünde bulundurun:

- Yaşam döngüsü. Genel olarak, aynı yaşam döngüsüne sahip kaynakları aynı kaynak grubuna yerleştirin.
- Erişim. Kullanabileceğiniz [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) bir grup içindeki kaynaklara erişim ilkelerini uygulamak için (RBAC).
- Faturalandırma. Kaynak grubu için aktarılmış maliyetleri görüntüleyebilirsiniz.
- Fiyatlandırma katmanı için API Yönetimi – geliştirme ve test ortamları için geliştirici katmanı kullanılması önerilir. Üretim öncesi için üretim ortamınızın bir çoğaltma dağıtma testleri çalıştırma ve maliyeti en aza indirmek için ardından kapatılmasını öneririz.

Daha fazla bilgi için [kaynak grubu](../azure-resource-manager/resource-group-overview.md) genel bakış.

### <a name="deployment"></a>Dağıtım

Kullanmanızı öneririz [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md) hem Azure API Management ve Azure Logic Apps dağıtılacak. Şablonlar, PowerShell veya Azure komut satırı arabirimi (CLI) aracılığıyla dağıtımları otomatik hale getirmek kolaylaştırır.

Azure API Management ile tek tek tüm Logic Apps, kendi ayrı bir Resource Manager şablonlarında yerleştirme öneririz. Bu, onları kaynak denetimi sistemlerini içinde depolamasını olanak tanır. Bu şablonlar birlikte veya tek tek sürekli tümleştirme/sürekli (CI/CD) dağıtım işleminin bir parçası olarak dağıtılabilir.

### <a name="versions"></a>Sürümler

Bir yapılandırması yaptığınız her zaman bir mantıksal uygulama için değiştirme (veya bir update Resource Manager şablonu aracılığıyla dağıtma), bu sürümünün bir kopyasını (çalıştırma geçmişi olan tüm sürümleri tutulur) size kolaylık sağlamak için tutulur. Geçmiş değişiklikleri izlemek ve ayrıca mantıksal uygulamanın geçerli yapılandırma için bir sürüme yükseltmek için bu sürümleri kullanabilirsiniz; Bunu yaparsanız, etkili bir şekilde bir mantıksal uygulama, örneğin geri alma.

API Management vardır (ancak ücretsiz) farklı iki [sürüm oluşturma kavramları](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Bunlar kullanabilecek API yönteminizi kullanarak API tüketicilerini sağlamak için kullanılan sürümleri, (örneğin v1, v2 veya beta, üretim), ihtiyaçlarını temel alarak.
- Güvenli bir şekilde bir API için değişiklik ve isteğe bağlı bir yorum ile kullanıcılara dağıtmak API yöneticilerinin düzeltmeler.

Dağıtım – bağlamında API Management düzeltmeler değişiklikleri güvenle yapmak, değişiklik geçmişini tutmak ve API tüketicilerini bu değişikliklerin farkında yapmak için bir yol olarak düşünülmelidir. Bir değişiklik, bir geliştirme ortamında oluşturulmuş ve Resource Manager şablonlarını kullanarak diğer ortamlar arasında dağıtılır.

Düzeltme 'geçerli' yapılmadan önce bir API'yi test etme için kullanılan ve kullanıcılar için erişilebilir hale artırabileceksiniz yük veya tümleştirme testi için bu mekanizma kullanılması önerilmez: bunun yerine ayrı bir test veya üretim öncesi ortamlar kullanılmalıdır.

### <a name="configuration"></a>Yapılandırma

Hiçbir zaman parolaları, erişim anahtarlarını veya bağlantı dizelerini kaynak denetimine denetleyin. Gerekirse, dağıtmak ve bu değerleri güvenliğini sağlamak için uygun bir tekniğe kullanın. 

Logic Apps'te, (yani bir bağlantı biçiminde oluşturulamıyor) mantıksal uygulama içinde gereken herhangi bir hassas değerleri Azure Key Vault'ta depolanabilir ve bir Resource Manager şablonundan başvurulan. Ayrıca, her ortam için parametre dosyaları ile birlikte dağıtım şablonu parametrelerini kullanmanızı öneririz. Diğer yönergeleri, [parametreleri ve bir iş akışındaki girişleri güvenli hale getirme](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

API Yönetimi'nde, gizli dizileri, adlandırılmış değerler/özellikler olarak adlandırılan nesneleri kullanılarak yönetilir. Bunlar erişilebilen değerleri API Management ilkelerini güvenli bir şekilde depolayın. Bkz. nasıl [parolalarını API Management'te yönetme](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

Her ikisi de [API Management](../api-management/api-management-howto-use-azure-monitor.md) ve [Logic Apps](logic-apps-monitor-your-logic-apps.md) aracılığıyla işletimsel izleme desteği [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure İzleyici varsayılan olarak etkindir ve her hizmet için yapılandırılmış farklı ölçümleri temel alan bilgiler sağlar.

Ayrıca, daha fazla seçenek vardır her hizmet için:

- Logic Apps günlükleri gönderilebilir [Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) daha ayrıntılı analiz ve yönelik Kompozit için.
- API Management, geliştirme işlemleri izlemek için Application ınsights'ı yapılandırmayı destekler.
- API Management'ı destekleyen [özel API analiz için Power BI çözüm şablonu](http://aka.ms/apimpbi). Bu çözüm şablonu, kendi özel analytics çözümü, iş kullanıcıları için Power BI'da kullanılabilen raporları ile oluşturmasına olanak tanır.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Bu bölümde açıklandığı gibi mimaride dağıtılabilir bu makalede açıklanan Azure hizmetlerine özel güvenlik konuları listelenmiştir. En iyi güvenlik uygulamalarının tam bir listesi değildir.

- Uygun kullanıcılar için erişim düzeyini sağlamak için rol tabanlı erişim denetimi (RBAC) kullanın.
- API Management kullanarak OAuth/açık IDConnect Genel API uç noktalarını güvenli hale getirin. Bir kimlik sağlayıcısı yapılandırmak ve JWT doğrulama ilke ekleyerek bunu.
- Karşılıklı sertifikalar kullanarak API Management'tan arka uç hizmetlerine bağlanın
- HTTP tetikleyicisi tabanlı Logic Apps, API yönetimi için IP adresini işaret eden bir IP beyaz listesi oluşturarak güvenliğini sağlayın. Bu, API Management aracılığıyla geçmeden genel internet'ten mantıksal uygulamayı çağırmak engeller.

Bu başvuru mimarisi, Azure tümleştirme hizmetlerini kullanarak basit bir kurumsal tümleştirme platformu oluşturun nasıl oluşturulacağını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kuyruklar ve olayları ile Kurumsal tümleştirme](logic-apps-architectures-enterprise-integration-with-queues-events.md)
