---
title: Azure tümleştirme hizmetleri basit Kurumsal tümleştirme başvuru mimarisi
description: Bir Azure Logic Apps ve Azure API Management ile basit Kurumsal tümleştirme düzeni nasıl uygulayacağınıza karar gösteren başvuru mimarisini açıklar.
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
ms.openlocfilehash: f73a9e59c0add664128b506172182afe566ca670
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444519"
---
# <a name="reference-architecture-simple-enterprise-integration"></a>Başvuru mimarisi: basit bir kurumsal tümleştirme

Aşağıdaki başvuru mimarisi, bir Azure tümleştirme hizmetleri kullanan bir tümleştirme uygulaması için uygulayabileceğiniz kanıtlanmış yöntemler kümesi gösterir. Mimari HTTP API'leri, iş akışı ve orchestration gerektiren birçok farklı uygulama desenleri için temel olarak hizmet verebilir.

![Mimari diyagramı - basit Kurumsal tümleştirme](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

*Tümleştirme teknolojileri için birçok olası uygulama vardır. Azure Service Bus uygulaması tam kuruluş basit noktadan noktaya uygulamasından aralığı. Mimari serisi Genel tümleştirme uygulaması oluşturmak için geçerli olabilecek yeniden kullanılabilir bileşen parçalarına açıklar. Mimarları, kendi uygulama ve altyapı için uygulamak için ihtiyaç duydukları hangi bileşenlerin göz önünde bulundurmanız gerekir.*

## <a name="architecture"></a>Mimari

Mimari aşağıdaki bileşenlere sahiptir:

- **Kaynak grubu**. [Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), Azure kaynakları için mantıksal bir kapsayıcıdır.
- **Azure API Management**. [API Management](https://docs.microsoft.com/azure/api-management/) yayımlamak, güvenli ve HTTP API'lerini dönüştürmek için kullanılan tam olarak yönetilen bir platformdur.
- **Azure API Management Geliştirici Portalı**. Erişim için her bir Azure API Management örneğinin birlikte [Geliştirici Portalı](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). API Management Geliştirici Portalı, belgeler ve kod örneklerine erişmenizi sağlar. Geliştirici portalındaki API'ler test edebilirsiniz.
- **Azure Logic Apps**. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) ve kurumsal tümleştirme oluşturmak için kullanılan bir sunucusuz platformudur.
- **Bağlayıcılar**. Logic Apps kullanan [Bağlayıcılar](https://docs.microsoft.com/azure/connectors/apis-list) Hizmetleri için yaygın olarak bağlanmak için kullanılır. Logic Apps farklı bağlayıcıları yüzlerce zaten sahip, ancak bir özel bağlayıcı oluşturabilirsiniz.
- **IP adresi**. Azure API Management hizmeti sabit bir ortak sahip [IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) ve bir etki alanı adı. Azure-api.net contoso.azure-api.net gibi alt etki alanı varsayılan etki alanı adıdır ancak [özel etki alanları](https://docs.microsoft.com/azure/api-management/configure-custom-domain) de yapılandırılabilir. Logic Apps ve Service Bus bir genel IP adresi de. Ancak, bu mimaride, biz yalnızca IP adresi API Management'ın Logic Apps Uç noktalara (güvenlik için) çağırmaya yönelik erişimi kısıtlayın. Service Bus çağrıları bir paylaşılan erişim imzası (SAS) korunur.
- **Azure DNS**. [Azure DNS](https://docs.microsoft.com/azure/dns/) DNS etki alanları için bir barındırma hizmetidir. Azure DNS, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlar. Etki alanlarınızı azure'da barındırarak aynı kimlik bilgilerini, API'leri, araçları kullanarak ve faturalama, diğer Azure Hizmetleri için kullandığınız DNS kayıtlarınızı yönetebilirsiniz. Özel etki alanı contoso.com gibi kullanılacak özel etki alanı adını IP adresine eşleyen DNS kayıtları oluşturun. Daha fazla bilgi için [API Yönetimi'nde bir özel etki alanı adı yapılandırma](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Kullanım [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/) veya kimlik doğrulaması için başka bir kimlik sağlayıcısı. Azure AD, API uç noktalarını geçirerek erişim için kimlik doğrulaması sağlar bir [API Management için JSON Web belirteci](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) doğrulamak için. Azure AD (yalnızca standart ve Premium Katmanlar) API Management Geliştirici portalına erişim güvenliğini sağlayabilirsiniz.

Mimari, işlemi için temel bazı desenleri sahiptir:

- Birleşik API, logic apps kullanarak oluşturulur. Bunlar, yazılım olarak hizmet (SaaS) sistemleri, Azure Hizmetleri ve API Management'a yayımlanan tüm API çağrılarını düzenleyin. [Logic apps ayrıca yayımlanan](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) API Management Geliştirici Portalı aracılığıyla.
- Uygulamaları Azure AD'ye kullanın [bir OAuth 2.0 güvenlik belirteci almak](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) , gerekli olan bir API'ye erişim elde etmek için.
- Azure API Management [güvenlik belirtecini doğrular](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) isteği arka uç API veya mantıksal uygulamaya geçirir.

## <a name="recommendations"></a>Öneriler

Bu makalede açıklanan genel mimarisinin gereksinimlerinizi farklılık gösterebilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="azure-api-management-tier"></a>Azure API Management katmanı

API Management temel, standart veya Premium katmanları kullanın. Katmanlar, bir üretim hizmet düzeyi sözleşmesi (SLA) sağlar ve ölçeklendirme (birim sayısını katmana göre değişiklik gösterir) için bir Azure bölgesindeki destekler. Premium katmanında ölçeği genişletme Azure bölgelerinde de destekler. Gerekli aktarım hızı ve özellik kümenize düzeyini seçtiğiniz katman temel alır. Daha fazla bilgi için [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).

Çalışırken tüm API Management örnekleri için ücretlendirilir. Ölçeklendirilebilir ve gerekmez, bu düzeyde performans her zaman ölçeği ve API Management saatlik faturalandırma avantajından yararlanmaya düşünün.

### <a name="logic-apps-pricing"></a>Logic Apps fiyatlandırma

Logic Apps kullanan bir [sunucusuz](logic-apps-serverless-overview.md) modeli. Faturalandırma üzerinde eylem ve bağlayıcı yürütme temel alınarak hesaplanır. Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/). Şu anda mantıksal uygulamalar için hiçbir katman hakkında önemli noktalar vardır.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Zaman uyumsuz API çağrıları için Logic Apps

Logic Apps, düşük gecikme süresi gerektirmeyen senaryolarda en iyi şekilde çalışır. Örneğin, bunun için en iyi zaman uyumsuz veya yarı uzun süre çalışan API çağrıları. (Örneğin, bir kullanıcı arabirimi engelleyen bir çağrı) düşük gecikme süresi gerekiyorsa, bu API veya işlemi farklı teknolojiyi kullanarak uygulama öneririz. Örneğin, Azure işlevleri veya Azure App Service'i kullanarak dağıttığınız bir Web API'sini kullanın. API Yönetimi'ni kullanarak API tüketicilerini ön hala öneririz.

### <a name="region"></a>Bölge

Ağ gecikmesini en aza indirmek için API Management'ı ve Logic Apps ile aynı bölgede sağlayın. Genellikle, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu da bir bölge vardır. Bölge, dağıtım meta verileri nerede depolandığını ve nereye dağıtım şablonu, gelen yürütür belirtir. Kaynak grubu ve kaynaklarını dağıtım sırasında kullanılabilirliği geliştirmek için aynı bölgeye yerleştirin.

## <a name="scalability"></a>Ölçeklenebilirlik

API Management yöneticileri eklemelidir [önbelleğe alma ilkeleri](../api-management/api-management-howto-cache.md) hizmetinin ölçeklenebilirliği artırmak için uygun olduğunda. Önbelleğe alma Ayrıca arka uç Hizmetleri yükünü azaltmaya yardımcı olur.

Azure API Management temel, standart ve Premium katmanlarında daha yüksek kapasite sunmak için bir Azure bölgesinde genişletilebilir. Yöneticiler **kapasite ölçüm** seçeneğini **ölçümleri** kendi hizmeti kullanımını analiz edin ve sonra ölçeği veya uygun şekilde ölçeği menüsü.

API Management hizmeti ölçeklendirmeye yönelik öneriler:

- Ölçeklendirme trafiği düzenlerinin hesaba katması gerekiyor. Trafik düzenlerini daha geçici olan müşteriler, artan kapasite büyük gerek.
- % 66 yukarıda tutarlı kapasite ölçeğinizi gerek gösterebilir.
- % 20 tutarlı kapasitenin ölçeğini fırsatı gösterebilir.
- Yük-API Yönetimi hizmetiniz bir temsili yük ile test üretim yük etkinleştirmeden önce için her zaman önerilir.

Premium katmanı hizmetlerini Azure bölgelerinde kullanıma ölçeklendirilebilir. Azure bölgelerinde Hizmetleri ölçekleyerek dağıtan müşteriler, daha yüksek bir SLA (% 99,95 oranında ve % 99,9) elde edin ve hizmetlerine yakın birden fazla bölgede kullanıcılara sağlayabilirsiniz.

Sunucusuz modeli yöneticilerin anlamına gelir. Logic Apps, hizmet ölçeklenebilirliği için planlama gerekmez. Servis talebi karşılamak üzere otomatik olarak ölçeklendirir.

## <a name="availability"></a>Kullanılabilirlik

Şu anda, Azure API Yönetimi SLA'sı, temel, standart ve Premium katmanları için % 99,9 aşamasındadır. Premium katmanında en az bir birim iki veya daha fazla bölgede dağıtımını yapılandırmalarla % 99,95 oranında SLA'sı vardır.

Şu anda, Azure Logic Apps için SLA % 99,9 aşamasındadır.

### <a name="backups"></a>Yedeklemeler

Azure API Management yapılandırmanızı olmalıdır [düzenli olarak yedeklenen](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (üzerinde değişiklik düzenli göre). Bir konum veya hizmetin bulunduğu farklı Azure bölgesinde yedekleme dosyalarının depolanması gerekir. Müşteriler, ardından kendi olağanüstü durum kurtarma stratejiniz için iki seçenekten birini seçebilirsiniz:

- Bir olağanüstü durum kurtarma olayı yeni bir API Management örneği sağlanır ve yeni örneğe yedek geri DNS kayıtlarını repointed.
- Müşteriler, kendi hizmet Pasif bir kopyasını (ek ücret alınıyor) başka bir Azure bölgesinde tutun. Yedeklemeleri, düzenli olarak kopyaya geri yüklenir. Hizmetini geri yüklemek için bir olağanüstü durum kurtarma olayı yalnızca DNS kayıtları repointed.

Logic apps, hızlı bir şekilde yeniden oluşturulabilir ve sunucusuz çünkü bunlar ilişkili Azure Resource Manager şablonunun bir kopyasını kaydederek yedeklenir. Kaynak denetimine kaydedilebilmesi için şablonları ve bir müşterinin sürekli tümleştirme/sürekli dağıtım (CI/CD) işlem ile tümleştirilebilir.

API Management aracılığıyla yayımlanan bir mantıksal uygulama için farklı bir veri merkezine taşınırsa, uygulamanın konumu güncelleştirin. Uygulamanın konumu güncelleştirmek için güncelleştirmek için temel bir PowerShell Betiği kullanmanız **arka uç** API'nin özellik.

## <a name="manageability"></a>Yönetilebilirlik

Üretim, geliştirme için ayrı kaynak grupları oluşturun ve test ortamları. Ayrı kaynak gruplarını kullanarak dağıtımları yönetin, test dağıtımlarını silmeyi ve erişim haklarını atamayı kolaylaştırır.

Kaynakları kaynak gruplarına atadığınızda, aşağıdaki faktörleri göz önünde bulundurun:

- **Yaşam döngüsü**. Genel olarak, aynı kaynak grubunda aynı yaşam döngüsüne sahip kaynakları yerleştirin.
- **Erişim**. Kullanabileceğiniz [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) bir grup içindeki kaynaklara erişim ilkelerini uygulamak için (RBAC).
- **Faturalama**. Kaynak grubu için toplama maliyetleri görüntüleyebilirsiniz.
- **API Yönetimi fiyatlandırma katmanı**. Biz, geliştirme için geliştirici katmanı kullanmanızı öneririz ve test ortamları. Üretim öncesi için üretim ortamınızın bir çoğaltma dağıtma testleri çalıştırma ve maliyeti en aza indirmek için ardından kapatılmasını öneririz.

Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Dağıtım

Kullanmanızı öneririz [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md) API Management ve Logic Apps dağıtılacak. Şablonlar, PowerShell veya Azure CLI aracılığıyla dağıtımları otomatik hale getirmek kolaylaştırır.

Azure API Management ve herhangi bir tek logic apps, kendi ayrı bir Resource Manager şablonlarında koyarak öneririz. Ayrı şablonları kullandığınızda, kaynaklar kaynak denetimi sistemlerini depolayabilirsiniz. Kaynakları birlikte veya tek tek bir CI/CD dağıtım işleminin bir parçası olarak da dağıtabilirsiniz.

### <a name="versions"></a>Sürümler

Bir yapılandırması yaptığınız her zaman bir mantıksal uygulama için değiştirme (veya bir update Resource Manager şablonu aracılığıyla dağıtma), bu sürümünün bir kopyasını (çalıştırma geçmişi olan tüm sürümleri tutulur) size kolaylık sağlamak için tutulur. Geçmiş değişiklikleri izlemek ve mantıksal uygulamanın geçerli yapılandırma için bir sürüme yükseltmek için bu sürümlerini kullanabilirsiniz. Örneğin, etkili bir şekilde bir mantıksal uygulama geri alabilirsiniz.

API Management vardır (ancak ücretsiz) farklı iki [sürüm oluşturma kavramları](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Sürümleri (örneğin, v1, v2 veya beta, üretim), ihtiyaçlarını temel alarak kullanabilecekleri API seçeneğiyle API tüketicilerinize sağlamak için kullanılır.
- API yöneticilere bir API'ye güvenli bir şekilde değişiklik yapma ve kullanıcıların isteğe bağlı bir yorum ile değişiklikleri dağıtmak düzeltmeler.

Dağıtım bağlamında, API Management düzeltmeleri, değişiklikleri güvenle yapmak, değişiklik geçmişini tutmak ve API tüketicilerini bu değişikliklerin farkında yapmak için bir yol olarak dikkate alınması gereken bir fikirdir. Bir düzeltme bir geliştirme ortamında oluşturulabilir ve Resource Manager şablonları kullanarak diğer ortamlar arasında dağıtılır.

"Mevcut" ve kullanıcılar için erişilebilir hale getirmeden önce bir API'yi test etme için düzeltmeler kullanabilseniz de, bu mekanizma yük veya tümleştirme testi kullanımı önerilmemektedir. Bunun yerine, ayrı bir test veya üretim öncesi ortamlarda kullanın.

### <a name="configuration"></a>Yapılandırma

Hiçbir zaman parolaları, erişim anahtarlarını veya bağlantı dizelerini kaynak denetimine denetleyin. Bu değerleri gerekiyorsa, dağıtmak ve bu değerleri güvenliğini sağlamak için uygun bir tekniğe kullanın. 

Logic Apps'te (bağlantı biçiminde oluşturulamıyor) mantıksal uygulamada, gerekli herhangi bir hassas değeri Azure Key Vault'ta depolanabilir ve bir Resource Manager şablonundan başvurulan. Ayrıca, her ortam için dağıtım şablonu parametreleri ve parametre dosyalarını kullanmanızı öneririz. Daha fazla bilgi için [parametreleri ve bir iş akışındaki girişleri güvenli hale getirme](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

API Yönetimi'nde, gizli dizileri adı verilen nesneleri kullanarak yönetilen *değerleri adlı* veya *özellikleri*. Nesneleri erişilebilen değerleri API Management ilkelerini güvenli bir şekilde depolayın. Daha fazla bilgi için [parolalarını API Management'te yönetme](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

[API Management](../api-management/api-management-howto-use-azure-monitor.md) ve [Logic Apps](logic-apps-monitor-your-logic-apps.md) her ikisi de aracılığıyla işletimsel izleme desteği [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure İzleyici, her hizmet için yapılandırılmış ölçümleri temel alan bilgiler sağlar. Azure İzleyici varsayılan olarak etkindir.

Bu seçenekler ayrıca her hizmet için kullanılabilir:

- Logic Apps günlükleri gönderilebilir [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) daha ayrıntılı analiz ve yönelik Kompozit için.
- API Management, DevOps izleme için yapılandırma Azure Application ınsights'ı destekler.
- API Management'ı destekleyen [özel API analiz için Power BI çözüm şablonu](http://aka.ms/apimpbi). Müşteriler, kendi özel analiz çözümü oluşturmak için çözüm şablonu kullanabilirsiniz. Raporlar Power BI'da iş kullanıcıları için kullanılabilir.

## <a name="security"></a>Güvenlik

Bu bölümde, bu makalede açıklanan ve açıklandığı gibi mimari dağıtılan Azure hizmetlerine özel güvenlik konuları listelenmiştir. En iyi güvenlik uygulamalarının tam bir listesi değildir.

- Uygun kullanıcılar için erişim düzeyini sağlamak için rol tabanlı erişim denetimi (RBAC) kullanın.
- OAuth/Openıd Connect kullanarak API Management Genel API uç noktalarını güvenli hale getirin. Genel API uç noktalarını güvenli hale getirmek için bir kimlik sağlayıcısı yapılandırmak ve bir JSON Web Token (JWT) doğrulama ilkesi ekleyin.
- Arka uç Hizmetleri için API Yönetimi'nden karşılıklı sertifikalar kullanarak bağlanın.
- HTTP tetikleyicisi tabanlı logic apps, API Management IP adresine işaret eden bir IP adresi beyaz listesi oluşturarak güvenliğini sağlayın. API Management aracılığıyla geçmeden genel internet'ten mantıksal uygulamayı çağırmak bir izin verilenler listesinde IP adresi engeller.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [sıralar ve olaylar ile Kurumsal tümleştirme](logic-apps-architectures-enterprise-integration-with-queues-events.md).
