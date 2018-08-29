---
title: Basit Kurumsal tümleştirme mimari deseni - Azure tümleştirme hizmetleri
description: Bu mimari başvurusu, Azure Logic Apps ve Azure API Management'ı kullanarak basit bir kurumsal tümleştirme düzeni nasıl uygulayacağınıza dair gösterir
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 7081c9e4f6e6deee196255f04180a8f2cc792876
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122504"
---
# <a name="simple-enterprise-integration-architecture"></a>Basit Kurumsal tümleştirme mimarisi

Bu makalede, Azure tümleştirme hizmetleri kullanırken tümleştirme uygulamaya uygulayabilirsiniz kendini kanıtlamış yöntemleri kullanan bir kurumsal tümleştirme mimarisi açıklanmaktadır. Bu mimari HTTP API'leri, iş akışı ve orchestration gerektiren birçok farklı uygulama desenleri için temel olarak kullanabilirsiniz.

![Mimari diyagramı - basit Kurumsal tümleştirme](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

Bu seri, genel tümleştirme uygulaması oluşturmak için geçerli olabilecek yeniden kullanılabilir bileşen parçalarına açıklar. Tüm kurumsal Azure hizmet veri yolu uygulamaları için basit bir noktadan noktaya uygulamalardan arasında çok sayıda olası uygulama tümleştirme teknolojileri sahip olduğundan, uygulamalarınızı ve altyapınızı için uygulamanız gereken hangi bileşenlerin göz önünde bulundurun.

## <a name="architecture-components"></a>Mimari bileşenler

Bu Kurumsal tümleştirme mimarisi şu bileşenleri içerir:

- **Kaynak grubu**: A [kaynak grubu](../azure-resource-manager/resource-group-overview.md) Azure kaynakları için mantıksal bir kapsayıcıdır.

- **Azure API Management**: [API Management](https://docs.microsoft.com/azure/api-management/) yayımlama, güvenliğini sağlama ve HTTP API'lerini dönüştürme için tam olarak yönetilen bir platform hizmetidir.

- **Azure API Management Geliştirici Portalı**: her bir Azure API Management örneğinin erişim sağlayan [Geliştirici Portalı](../api-management/api-management-customize-styles.md). Bu portal, belgeler ve kod örneklerine erişmenizi sağlar. Geliştirici portalındaki API'ler ayrıca test edebilirsiniz.

- **Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) kurumsal iş akışları ve tümleştirmeler oluşturmak için sunucusuz bir platformdur.

- **Bağlayıcılar**: Logic Apps kullanan [Bağlayıcılar](../connectors/apis-list.md) hizmetleri kullanılan yaygın olarak bağlanmak için. Logic Apps bağlayıcıları yüzlerce sunar, ancak bir özel bağlayıcı oluşturabilirsiniz.

- **IP adresi**: Azure API Yönetimi hizmeti, sabit bir ortak sahip [IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve bir etki alanı adı. Azure-api.net, örneğin, contoso.azure-api.net, alt etki alanı varsayılan etki alanı adıdır, ancak ayrıca [özel etki alanları](../api-management/configure-custom-domain.md). Logic Apps ve Service Bus bir genel IP adresi de. Ancak, güvenlik için bu mimari yalnızca IP adresi API Management'ın Logic Apps uç noktalarını çağırma için erişimi kısıtlar. Service Bus çağrıları bir paylaşılan erişim imzası (SAS) korunur.

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) DNS etki alanları için bir barındırma hizmetidir. Azure DNS, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlar. Etki alanlarınızı azure'da barındırarak aynı kimlik bilgilerini, API'leri, araçları kullanarak ve faturalama, diğer Azure Hizmetleri için kullandığınız DNS kayıtlarınızı yönetebilirsiniz. Özel etki alanı, contoso.com gibi kullanılacak özel etki alanı adını IP adresine eşleyen DNS kayıtları oluşturun. Daha fazla bilgi için [API Yönetimi'nde bir özel etki alanı adı yapılandırma](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: kullanabileceğiniz [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/) veya kimlik doğrulaması için başka bir kimlik sağlayıcısı. Azure AD, API uç noktalarını geçirerek erişim için kimlik doğrulaması sağlar bir [API Management için JSON Web belirteci](../api-management/policies/authorize-request-based-on-jwt-claims.md) doğrulamak için. Standart ve Premium katmanları için Azure AD, API Management Geliştirici portalına erişim güvenliğini sağlayabilirsiniz.

## <a name="patterns"></a>Desenler 

Bu mimari, işlemi için temel bazı desenleri kullanır:

* Birleşik API, hizmet (SaaS) sistemleri, Azure Hizmetleri ve API Management'a yayımlanan herhangi bir API çağrısına yazılım düzenleyin, logic apps kullanarak oluşturulur. Mantıksal uygulamalar, ayrıca [API Management Geliştirici Portalı aracılığıyla yayımlanan](../api-management/import-logic-app-as-api.md).

* Uygulamaları kullanmak için Azure AD [bir OAuth 2.0 güvenlik belirtecini alma](../api-management/api-management-howto-protect-backend-with-aad.md) , gerekli olan bir API'ye erişim elde etmek için.

* Azure API Management [güvenlik belirtecini doğrular](../api-management/api-management-howto-protect-backend-with-aad.md) isteği arka uç API veya mantıksal uygulamaya geçirir.

## <a name="recommendations"></a>Öneriler

Gereksinimlerinize göre bu makalede açıklanan genel mimarisinin farklılık gösterebilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="azure-api-management-tier"></a>Azure API Management katmanı

API Management temel, standart veya Premium katmanları kullanın. Bu katmanlar, bir üretim hizmet düzeyi sözleşmesi (SLA) sağlar ve Azure bölgesi içinde genişletme desteği. Birim sayısını katmana göre değişir. Premium katmanı, birçok Azure bölgesinde genişletme de destekler. Özellik kümesi ve gerekli aktarım hızı düzeyi temelinde katmanınızı seçin. Daha fazla bilgi için [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).

Çalışırken tüm API Management örnekleri için ücretlendirilir. Ölçeklendirilebilir ve gerekmez, bu düzeyde performans her zaman ölçeği ve API Management saatlik faturalandırma avantajından yararlanmaya düşünün.

### <a name="logic-apps-pricing"></a>Logic Apps fiyatlandırma

Logic Apps kullanan bir [sunucusuz](../logic-apps/logic-apps-serverless-overview.md) modeli. Faturalandırma üzerinde eylem ve bağlayıcı yürütme temel alınarak hesaplanır. Daha fazla bilgi için [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/). Şu anda mantıksal uygulamalar için hiçbir katman hakkında önemli noktalar vardır.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Zaman uyumsuz API çağrıları için Logic Apps

Logic Apps, düşük gecikme süresi gerektirmeyen senaryolarda en iyi şekilde çalışır. Örneğin, Logic Apps için en iyi zaman uyumsuz veya yarı uzun süre çalışan API çağrıları. Düşük gecikme süresi gerekiyorsa, örneğin, bir kullanıcı arabirimi engelleyen bir çağrı uygulamak API veya işlemi farklı teknolojiyi kullanarak. Örneğin, Azure işlevleri veya Azure App Service'i kullanarak dağıttığınız bir Web API'sini kullanın. API, API tüketicilerini için ön için API Management'ı kullanın.

### <a name="region"></a>Bölge

Ağ gecikmesini en aza indirmek için API Management, Logic Apps ve Service Bus için aynı bölgeyi seçin. Genel olarak, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu da bir bölge vardır. Bu bölge, dağıtım meta verileri depolamak nereden ve nereye dağıtım şablonu yürütmek belirtir. Dağıtım sırasında kullanılabilirliği artırmak için söz konusu kaynak grubu ve kaynakları aynı bölgeye yerleştirin.

## <a name="scalability"></a>Ölçeklenebilirlik

API Management hizmeti yönetirken, ölçeklenebilirliği artırmak için ekleme [önbelleğe alma ilkeleri](../api-management/api-management-howto-cache.md) uygun olduğunda. Önbelleğe alma Ayrıca arka uç Hizmetleri yükünü azaltmaya yardımcı olur.

Daha fazla kapasite sunmak için bir Azure bölgesinde Azure API Management temel, standart ve Premium katmanlarda ölçeği genişletebilirsiniz. Hizmetinizin kullanım çözümlenecek **ölçümleri** menüsünde **kapasite ölçüm** seçeneğini ve sonra ölçeği veya uygun şekilde ölçeği azaltın.

API Management hizmeti ölçeklendirmeye yönelik öneriler:

- Trafik düzenlerini ölçeklendirme sırasında göz önünde bulundurun. Trafik düzenlerini daha geçici olan müşteriler, daha fazla kapasiteye ihtiyaç.

- % 66'dan büyük tutarlı kapasite ölçeğinizi gerek gösterebilir.

- % 20 altında olan tutarlı kapasite ölçeğini fırsatı gösterebilir.

- Üretim yük etkinleştirmeden önce her zaman yük API Yönetimi hizmetiniz temsili bir yük testi.

Premium katmanı hizmetlerini Azure bölgelerinde ölçeklendirebilirsiniz. Azure bölgelerinde Hizmetleri ölçekleyerek dağıtırsanız, daha yüksek bir SLA (% 99,95 oranında ve % 99,9) ve sağlama hizmetleri birden fazla bölgede kullanıcılara yakın elde edebilirsiniz.

Sunucusuz modeli yöneticilerin anlamına gelir. Logic Apps, hizmet ölçeklenebilirliği için planlama gerekmez. Servis talebi karşılamak üzere otomatik olarak ölçeklendirir.

## <a name="availability"></a>Kullanılabilirlik

* Azure API Management hizmet düzeyi sözleşmesi (SLA), temel, standart ve Premium katmanları için % 99,9 şu anda aşamasındadır. En az bir birimin iki veya daha fazla bölgede bulunan bir dağıtım ile Premium katmanı yapılandırmaları için % 99,95 oranında SLA var.

* Şu anda Azure Logic Apps için SLA % 99,9 ' dir.

### <a name="backups"></a>Yedeklemeler

Değişiklik, düzenli üzerinde temel [düzenli olarak yedekleyin](../api-management/api-management-howto-disaster-recovery-backup-restore.md) Azure API Management yapılandırma. Bir konum veya hizmetinizi bulunduğu farklı Azure bölgesine Yedekleme dosyalarınızı Store. Ardından, olağanüstü durum kurtarma stratejiniz iki seçenekten birini seçebilirsiniz:

* Bir olağanüstü durum kurtarma olayı içinde yeni bir API Management örneği sağlayın ve sonra yeni örneğe yedek geri yükleme, DNS kayıtlarını repoint.

* Ek ücret doğurur, başka bir Azure bölgesinde, hizmetinize Pasif bir kopyasını tutun. Düzenli yedeklemeleri bu kopyaya geri yükleyin. Hizmet olağanüstü durum kurtarma olayı sırasında geri yüklemek için yalnızca DNS kayıtları repoint.

Hızlı bir şekilde oluşturmanız için sunucusuz, mantıksal uygulamalar, bunları ilişkili Azure Resource Manager şablonunun bir kopyasını kaydederek yedekleyin. Kaynak denetiminde şablonları kaydedebilir ve şablonlar ile sürekli tümleştirme/sürekli dağıtım (CI/CD) işlem tümleştirebilirsiniz.

Azure API Management aracılığıyla bir mantıksal uygulama yayımladığınız ve bu mantıksal uygulama için farklı bir veri merkezine taşınır, uygulamanın konumu güncelleştirin. API'NİZİN güncelleştirebilirsiniz **arka uç** temel bir PowerShell komut dosyası kullanarak özellik.

## <a name="manageability"></a>Yönetilebilirlik

Üretim, geliştirme için ayrı kaynak grupları oluşturun ve test ortamları. Ayrı kaynak gruplarında dağıtımları yönetmek, test dağıtımlarını silmeyi ve erişim haklarını atamayı kolaylaştırır.

Kaynakları kaynak gruplarına atadığınızda, şu etmenleri dikkate alın:

* **Yaşam döngüsü**: genel olarak, aynı kaynak grubunda aynı yaşam döngüsüne sahip kaynakları yerleştirin.

* **Erişim**: Grup içindeki kaynaklara erişim ilkeleri uygulamak için kullanabileceğiniz [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md).

* **Faturalama**: kaynak grubu için toplama maliyetleri görüntüleyebilirsiniz.

* **API Yönetimi fiyatlandırma katmanı**: Geliştirici katmanı geliştirme ve test ortamları için kullanın. Ön üretim sırasında maliyetleri en aza indirmek için üretim ortamınızın bir çoğaltma dağıtma, testlerinizi çalıştırmak ve kapatın.

Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Dağıtım

* API Management ve Logic Apps'ı dağıtmak için [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md). Şablonlar, PowerShell veya Azure CLI kullanarak otomatikleştirerek dağıtımları daha kolay olun.

* API Management ve herhangi bir tek logic apps, kendi ayrı bir Resource Manager şablonlarında yerleştirin. Ayrı bir şablon kullanarak kaynakları kaynak denetimi sistemlerini depolayabilirsiniz. Bu şablonlar ardından, birlikte veya tek tek sürekli tümleştirme/sürekli dağıtım (CI/CD) işleminin bir parçası olarak da dağıtabilirsiniz.

### <a name="versions"></a>Sürümler

Bir mantıksal uygulamanın yapılandırmasını değiştirmenize veya bir Resource Manager şablonu aracılığıyla bir güncelleştirme dağıtımı her zaman Azure, size kolaylık olması için bu sürümünün bir kopyasını tutar ve çalıştırma geçmişi olan tüm sürümleri tutar. Geçmiş değişiklikleri izleme veya mantıksal uygulamanın geçerli yapılandırmasına bir sürüme yükseltmek için bu sürümlerini kullanabilirsiniz. Örneğin, etkili bir şekilde bir mantıksal uygulama geri alabilirsiniz.

Azure API Management bunlar farklı ancak tamamlayıcı sahiptir [sürüm oluşturma kavramları](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

* Örneğin, ihtiyaçlarını temel alan bir API sürümü seçmek için özellik, v1, v2, beta veya üretim API tüketicilerinize sağlayan sürümleri

* API yöneticilerin güvenli bir şekilde bir API'de değişiklikler yapmak ve ardından bu değişiklikleri isteğe bağlı bir yorum ile kullanıcılara dağıtmanıza olanak tanıyan düzeltmeler

Dağıtım için güvenli bir şekilde değişiklik yapmak için bir yol olarak API Management düzeltmeler değişiklik geçmişini tutmak ve bu değişiklikleri, API'NİZİN tüketicilere iletişim göz önünde bulundurun. Bir geliştirme ortamında bir değişiklik yapın ve diğer ortamlarda bu değişikliği Resource Manager şablonları kullanarak dağıtın.

"Mevcut" ve kullanıcılar bu değişiklikleri yapmadan önce bir API test etmek için düzeltmeler kullanabilirsiniz, ancak bu yöntem, tümleştirme testi veya yük için önerilmez. Bunun yerine, ayrı bir test veya üretim öncesi ortamlarda kullanın.

### <a name="configuration-and-sensitive-information"></a>Yapılandırma ve hassas bilgileri

Hiçbir zaman parolaları, erişim anahtarlarını veya bağlantı dizelerini kaynak denetimine dahil etmeyin. Bu değerleri gerekiyorsa, güvenli ve uygun teknikleri kullanarak bu değerleri dağıtın. 

Logic apps'teki bir mantıksal uygulama içinde bir bağlantı oluşturulamıyor. herhangi bir hassas değeri gerektiriyorsa, bu değerlerin Azure Key Vault'ta depolamak ve bunları bir Resource Manager şablonundan başvuru. Her ortam için dağıtım şablonu parametreleri ve parametre dosyalarını kullanın. Daha fazla bilgi için [parametreleri ve bir iş akışındaki girişleri güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

API Management adı verilen nesneleri kullanarak gizli dizileri yönetir *değerleri adlı* veya *özellikleri*. Bu nesneler, API Management ilkeleri erişebileceğiniz değerleri güvenli bir şekilde depolar. Daha fazla bilgi için [parolalarını API Management'te yönetme](../api-management/api-management-howto-properties.md).

## <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

Kullanabileceğiniz [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) işletimsel hem de izleme [API Management](../api-management/api-management-howto-use-azure-monitor.md) ve [Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md). Azure İzleyici, her hizmet için yapılandırılmış ölçümleri temel alan bilgiler sağlar ve varsayılan olarak etkindir.

Her hizmet ayrıca bu seçenekler vardır:

* Daha ayrıntılı analiz ve yönelik Kompozit için Logic Apps günlüklerini gönderebilir [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md).

* DevOps izleme için API yönetimi için Azure Application ınsights'ı yapılandırabilirsiniz.

* API Management'ı destekleyen [özel API analiz için Power BI çözüm şablonu](http://aka.ms/apimpbi). Bu çözüm şablonu, kendi analiz çözümü oluşturmak için kullanabilirsiniz. İş kullanıcıları için Power BI raporları kullanılabilir hale getirir.

## <a name="security"></a>Güvenlik

Bu liste tüm en iyi güvenlik uygulamaları tamamen açıklanmamaktadır ancak özellikle tarafından bu makalede açıklanan mimarisinde dağıtılan Azure Hizmetleri için geçerli olan bazı güvenlik konuları aşağıda verilmiştir:

* Kullanıcılar uygun erişim düzeylerine sahip olduğunuzdan emin olmak için rol tabanlı erişim denetimi (RBAC) kullanın.

* OAuth veya Openıd Connect kullanarak, API Management Genel API uç noktalarını güvenli hale getirin. Genel API uç noktalarını güvenli hale getirmek için bir kimlik sağlayıcısı yapılandırmak ve bir JSON Web Token (JWT) doğrulama ilkesi ekleyin.

* Arka uç Hizmetleri için API Yönetimi'nden karşılıklı sertifikalar kullanarak bağlanın.

* HTTP tetikleyicisi tabanlı logic apps, API Management IP adresine işaret eden bir IP adresi beyaz listesi oluşturarak güvenliğini sağlayın. API Management aracılığıyla geçmeden genel internet'ten mantıksal uygulamayı çağırmak bir izin verilenler listesinde IP adresi engeller.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [sıralar ve olaylar ile Kurumsal tümleştirme](../logic-apps/logic-apps-architectures-enterprise-integration-with-queues-events.md)
