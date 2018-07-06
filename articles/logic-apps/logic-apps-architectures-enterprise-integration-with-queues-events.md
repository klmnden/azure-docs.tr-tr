---
title: Azure tümleştirme hizmetleri başvuru mimarisi
description: Logic Apps, API Management, Service Bus ve Event Grid ile bir kurumsal tümleştirme düzeni nasıl uygulayacağınıza karar gösteren başvuru mimarisi
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
ms.openlocfilehash: 7d14bbd587b5086348026c41f6551b10809b939a
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37859651"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Kuyruklar ve olayları ile Kurumsal tümleştirme: başvuru mimarisi

## <a name="overview"></a>Genel Bakış

Bu başvuru mimarisi, bir dizi Azure tümleştirme hizmetleri kullanan bir tümleştirme uygulaması için kendini kanıtlamış yöntemleri gösterir. Bu mimari HTTP API'lerini, iş akışı ve orchestration gerektiren birçok farklı uygulama desenleri bu temel olarak hizmet verebilir.

*Uygulamasından bir basit noktası için noktası tam bir enterprise service bus tümleştirme teknolojileri birçok olası uygulamaları vardır. Bu mimari serisi Genel tümleştirme uygulaması oluşturmak için uygulanabilir yeniden kullanılabilir bileşen parçalarından kullanıma ayarlar – mimarları uygulamalarını ve altyapısını için uygulanması gerekir hangi belirli bileşenlerin göz önünde bulundurmanız gerekir.*

![Mimari diyagramı - sıralar ve olaylar ile Kurumsal tümleştirme](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Mimari

Bu mimari **yapılar** [basit Kurumsal tümleştirme](logic-apps-architectures-simple-enterprise-integration.md) mimarisi. [Basit bir kurumsal mimari önerileri](logic-apps-architectures-simple-enterprise-integration.md#recommendations) de burada geçerli, ancak gelen atlanmış [önerileri](#recommendations) kısa tutmak için bu belgedeki. Bunu, aşağıdaki bileşenlere sahiptir:

- Kaynak grubu. [Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), Azure kaynakları için mantıksal bir kapsayıcıdır.
- Azure API Management. [Azure API Management](https://docs.microsoft.com/azure/api-management/) yayımlama, güvenliğini sağlama ve HTTP API'lerini dönüştürme için tam olarak yönetilen bir platformdur.
- Azure API Management Geliştirici Portal'ı seçin. Her bir Azure API Management örneği ile birlikte gelen bir [Geliştirici Portalı](https://docs.microsoft.com/azure/api-management/api-management-customize-styles), erişim için belgeler, kod örnekleri ve bir API'yi test etme olanağı sağlar.
- Azure Logic Apps. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) kurumsal iş akışı ve tümleştirme oluşturmak için sunucusuz bir platformdur.
- Bağlayıcılar. [Bağlayıcılar](https://docs.microsoft.com/azure/connectors/apis-list) Logic Apps tarafından yaygın olarak kullanılan hizmetlerine bağlanmak için kullanılır. Logic Apps farklı bağlayıcıları yüzlerce zaten var ancak bunlar aynı zamanda özel bir bağlayıcı kullanılarak oluşturulabilir.
- Azure hizmet veri yolu. [Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) güvenli ve güvenilir Mesajlaşma sağlar. Mesajlaşma uygulamaları birbirinden XML'deki birleştirin ve ileti tabanlı diğer sistemlerle tümleştirmek için kullanılabilir.
- Azure Event grid'i. [Event Grid](https://docs.microsoft.com/azure/event-grid/overview) yayımlama ve uygulama olayları teslim için sunucusuz bir platformdur.
- IP adresi. Azure API Management hizmeti sabit bir ortak sahip [IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) ve bir etki alanı adı. Azure-api.net contoso.azure-api.net gibi bir alt etki alanı adıdır. Logic Apps ve Service Bus bir genel IP adresi de; Ancak, bu mimaride biz yalnızca IP adresi, API Management Logic apps Uç noktalara (güvenlik için) çağırmak için erişimi kısıtlayın. Service Bus çağrıları tarafından paylaşılan erişim imzası güvenlidir.
- Azure DNS. [Azure DNS](https://docs.microsoft.com/azure/dns/) DNS etki alanları, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamak için bir barındırma hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. Bir özel etki alanı adı (contoso.com gibi) kullanmak için özel etki alanı adını IP adresine eşleyen DNS kayıtları oluşturun. Daha fazla bilgi için Azure API Management'te özel etki alanı yapılandırma konusuna bakın.
- Azure Active Directory (Azure AD). Kullanım [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/) veya kimlik doğrulaması için başka bir kimlik sağlayıcısı. Azure AD kimlik doğrulaması için erişim API uç noktaları sağlar (geçirerek bir [API Management için JSON Web belirteci](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) doğrulamak için) ve API Management Geliştirici Portalı'nı (yalnızca standart ve Premium Katmanlar) erişim güvenliğini sağlayabilirsiniz.

Bu mimari, işlemi için bazı temel desenleri sahiptir:

1. Mevcut arka uç HTTP API, API Geliştirici geliştiricilerin, Yönetim Portalı aracılığıyla yayımlanır (ya da iç kuruluşunuz, dış veya her ikisi de) bu API'lere giden çağrıların uygulamalara tümleştirmek için.
2. Birleşik API, Logic Apps kullanarak yerleşiktir; SAAS sistemleri çağrıları işlemlerini, Azure Hizmetleri ve tüm API'leri için API Management yayımladı. [Logic Apps ayrıca yayımlanan](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) API Management Geliştirici Portalı aracılığıyla.
3. Uygulamaları [bir OAuth 2.0 güvenlik belirteci almak](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) Azure Active Directory'yi kullanarak bir API'ye erişim kazanmak için gerekli.
4. Azure API Management [güvenlik belirtecini doğrular](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)ve isteği arka uç API/mantıksal uygulama geçirir.
5. Service Bus kuyruklarını alışkın olduğunuz [ayrıştırmak](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) uygulama etkinlik ve [kesintisiz yük artış](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). 3. taraf uygulamalar, Logic Apps tarafından sıraya eklenen ya da (Resimsiz) iletileri kuyruğa API Management aracılığıyla bir HTTP API'si olarak yayımlayarak.
6. Bir Service Bus kuyruğuna ileti eklendiğinde, bir olayı tetikler. Mantıksal uygulama, bu olay tarafından tetiklendikten ve iletiyi işler.
7. Benzer şekilde, diğer Azure Hizmetleri (örneğin, Blob Depolama, olay hub'ı) de Event Grid olaylarını yayımlar. Bu etkinliğin ve sonraki eylemleri gerçekleştirmek için Logic Apps tetikleyin.

## <a name="recommendations"></a>Öneriler

Belirli gereksinimlerinizi genel burada açıklanan mimariden farklı olabilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="service-bus-tier"></a>Hizmet veri yolu katmanı

Service Bus premium katman şu anda bu event grid bildirimleri destekleyen olarak kullanın (tüm katmanlarında destek beklenir). Bkz: [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Event Grid fiyatlandırması

Event Grid kullanarak sunucusuz bir model çalışır: işlemler (olay yürütme) sayısına göre faturalandırma dayalı olarak hesaplanır. Bkz: [Event Grid fiyatlandırma](https://azure.microsoft.com/pricing/details/event-grid/) daha fazla bilgi için. Şu anda Event Grid için hiçbir katman hakkında önemli noktalar vardır.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>Service Bus iletileri kullanmaya PeekLock kullanın

Logic Apps, Service Bus iletileri tüketmeye oluştururken [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) iletiler grubunu erişmek için bu mantıksal uygulama içinde. Mantıksal uygulama, her bir iletiyi tamamlamak ya da durdurmadan önce doğrulamaya yönelik adımları daha sonra gerçekleştirebilirsiniz. Bu yaklaşım, yanlışlıkla ileti kaybına karşı korur.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Bir Event Grid Tetikleyici etkinleştirildiğinde, birden çok nesne için işaretleyin

Olay Kılavuzu Tetikleyicileri "en az 1 koca oldu" cevabıdır tetikleme. Event Grid görünen bir Service Bus kuyruğuna bir ileti üzerinde bir mantıksal uygulama tetiklendiğinde, örneğin, mantıksal uygulama her zaman bir veya daha fazla iletileri işlemek için uygun olabilir varsaymanız gerekir.

### <a name="region"></a>Bölge

Ağ gecikmesini en aza indirmek için API Management, Logic Apps ve Service Bus ile aynı bölgede sağlayın. Genellikle, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu dağıtım meta verileri nerede depolandığını belirtir, bir bölgede de vardır ve nereye dağıtım şablonu yürütülmediyse. Kaynak grubu ve kaynakları aynı bölgeye yerleştirin. Bu, dağıtım sırasında kullanılabilirliği iyileştirebilir.

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Azure Service Bus premium katmanı daha yüksek ölçeklenebilirlik elde etmek için Mesajlaşma birimi sayısını ölçeklendirme. Premium 1, 2 veya 4 Mesajlaşma birimine sahip olabilir. Azure Service Bus'ı ölçeklendirme hakkında daha ayrıntılı yönergeler için bkz. [Service Bus Mesajlaşma kullanarak performans geliştirme en iyi](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Makalenin yazıldığı sırada, Azure API Management hizmet düzeyi sözleşmesi (SLA) temel, standart ve Premium katmanları için % 99,9 ' dir. Premium katmanında en az bir birim iki veya daha fazla bölgede dağıtımını yapılandırmalarla % 99,95 oranında SLA'sı vardır.

Makalenin yazıldığı sırada, Azure Logic Apps için hizmet düzeyi sözleşmesi (SLA) % 99,9 ' dir.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Coğrafi olağanüstü durum kurtarma önemli bir kesinti durumunda yük devretmeyi etkinleştirmek için Service Bus premium, uygulamayı düşünün. Daha fazla bilgi edinin [Azure Service Bus Geo-olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Üretim, geliştirme için ayrı kaynak grupları oluşturun ve test ortamları. Bu, dağıtımları yönetmeyi, test dağıtımlarını silmeyi ve erişim haklarını atamayı kolaylaştırır.
Kaynakları kaynak gruplarına atarken, aşağıdakileri göz önünde bulundurun:

- Yaşam döngüsü. Genel olarak, aynı yaşam döngüsüne sahip kaynakları aynı kaynak grubuna yerleştirin.
- Erişim. Kullanabileceğiniz [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) bir grup içindeki kaynaklara erişim ilkelerini uygulamak için (RBAC).
- Faturalandırma. Kaynak grubu için aktarılmış maliyetleri görüntüleyebilirsiniz.
- Fiyatlandırma katmanı için API Yönetimi – geliştirme ve test ortamları için geliştirici katmanı kullanılması önerilir. Üretim öncesi için üretim ortamınızın bir çoğaltma dağıtma testleri çalıştırma ve maliyeti en aza indirmek için ardından kapatılmasını öneririz.

Daha fazla bilgi için [kaynak grubu](../azure-resource-manager/resource-group-overview.md) genel bakış.

### <a name="deployment"></a>Dağıtım

Kullanmanızı öneririz [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md) Azure API Management, Logic Apps, Event Grid ve Service Bus'ı dağıtmak için. Şablonlar, PowerShell veya Azure komut satırı arabirimi (CLI) aracılığıyla dağıtımları otomatik hale getirmek kolaylaştırır.

Azure API Management, herhangi tek Logic Apps, olay ızgarası konu başlıkları ve Service Bus ad alanları, kendi ayrı bir Resource Manager şablonlarında koyarak öneririz. Bu, onları kaynak denetimi sistemlerini içinde depolamasını olanak tanır. Bu şablonlar birlikte veya tek tek sürekli tümleştirme/sürekli (CI/CD) dağıtım işleminin bir parçası olarak dağıtılabilir.

### <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

Service Bus, API Management ve Logic Apps gibi Azure İzleyicisi'ni kullanarak izlenebilir. Azure İzleyici varsayılan olarak etkindir ve her hizmet için yapılandırılmış farklı ölçümleri temel alan bilgiler sağlar.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Paylaşılan erişim imzası kullanarak Service Bus güvenli hale getirin. [SAS kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) belirli haklar ile Service Bus kaynakları için kullanıcı erişim sağlar. Daha fazla bilgi edinin [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Ayrıca, Service Bus kuyruğuna (posta yeni iletiler izin vermek için) bir HTTP uç noktası sağlamak gerekir API Management, uç nokta cephesinde tarafından güvenli hale getirmek için kullanılmalıdır. Bunun ardından sertifikaları veya OAuth ile uygun şekilde güvenli hale getirilebilir. Bunu yapmanın en kolay yolu, aracı olarak bir istek/yanıt HTTP tetikleyicisi olan bir mantıksal uygulama kullanıyor.

Event Grid olay teslimi bir doğrulama kodu aracılığıyla güvenliğini sağlar. Olay kullanılacağı LogicApps kullanırsanız, bu otomatik olarak gerçekleştirilir. Daha fazla ayrıntı görmek [Event Grid güvenliğini ve kimlik doğrulaması](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Basit Kurumsal tümleştirme](logic-apps-architectures-simple-enterprise-integration.md)