---
title: Azure tümleştirme hizmetleri kurumsal tümleştirme başvuru mimarisi
description: Logic Apps, API Management, Service Bus ve Event Grid kullanarak bir kurumsal tümleştirme deseni uygulamak gösteren başvuru mimarisini açıklar.
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
ms.openlocfilehash: a86c4c4227795a712dd51ace1fbefe9d2b96518a
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39116121"
---
# <a name="reference-architecture-enterprise-integration-with-queues-and-events"></a>Başvuru mimarisi: sıralar ve olaylar ile Kurumsal tümleştirme

Aşağıdaki başvuru mimarisi, bir Azure tümleştirme hizmetleri kullanan bir tümleştirme uygulaması için uygulayabileceğiniz kanıtlanmış yöntemler kümesi gösterir. Mimari HTTP API'leri, iş akışı ve orchestration gerektiren birçok farklı uygulama desenleri için temel olarak hizmet verebilir.

![Mimari diyagramı - sıralar ve olaylar ile Kurumsal tümleştirme](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

*Tümleştirme teknolojileri için birçok olası uygulama vardır. Azure Service Bus uygulaması tam kuruluş basit noktadan noktaya uygulamasından aralığı. Mimari serisi Genel tümleştirme uygulaması oluşturmak için geçerli olabilecek yeniden kullanılabilir bileşen parçalarına açıklar. Mimarları, kendi uygulama ve altyapı için uygulamak için ihtiyaç duydukları hangi bileşenlerin göz önünde bulundurmanız gerekir.*

## <a name="architecture"></a>Mimari

Mimari *yapılar* [basit Kurumsal tümleştirme](logic-apps-architectures-simple-enterprise-integration.md) mimarisi. [Basit bir kurumsal mimari için öneriler](logic-apps-architectures-simple-enterprise-integration.md#recommendations) burada geçerli olur. Gelen atlanmış [önerileri](#recommendations) kısa tutmak için bu makaledeki. 

Mimari aşağıdaki bileşenlere sahiptir:

- **Kaynak grubu**. [Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), Azure kaynakları için mantıksal bir kapsayıcıdır.
- **Azure API Management**. [API Management](https://docs.microsoft.com/azure/api-management/) yayımlamak, güvenli ve HTTP API'lerini dönüştürmek için kullanılan tam olarak yönetilen bir platformdur.
- **Azure API Management Geliştirici Portalı**. Erişim için her bir Azure API Management örneğinin birlikte [Geliştirici Portalı](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). API Management Geliştirici Portalı, belgeler ve kod örneklerine erişmenizi sağlar. Geliştirici portalındaki API'ler test edebilirsiniz.
- **Azure Logic Apps**. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) ve kurumsal tümleştirme oluşturmak için kullanılan bir sunucusuz platformudur.
- **Bağlayıcılar**. Logic Apps kullanan [Bağlayıcılar](https://docs.microsoft.com/azure/connectors/apis-list) Hizmetleri için yaygın olarak bağlanmak için kullanılır. Logic Apps farklı bağlayıcıları yüzlerce zaten sahip, ancak bir özel bağlayıcı oluşturabilirsiniz.
- **Azure Service Bus**. [Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) güvenli ve güvenilir Mesajlaşma sağlar. Mesajlaşma uygulamaları ayırmanıza ve ileti tabanlı diğer sistemlerle tümleştirmek için kullanılabilir.
- **Azure Event grid'i**. [Event Grid](https://docs.microsoft.com/azure/event-grid/overview) yayımlama ve uygulama olayları teslim etmek için kullanılan sunucusuz bir platformdur.
- **IP adresi**. Azure API Management hizmeti sabit bir ortak sahip [IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) ve bir etki alanı adı. Azure-api.net contoso.azure-api.net gibi bir alt etki alanı adıdır. Logic Apps ve Service Bus bir genel IP adresi de. Ancak, bu mimaride, biz yalnızca IP adresi API Management'ın Logic Apps Uç noktalara (güvenlik için) çağırmaya yönelik erişimi kısıtlayın. Service Bus çağrıları bir paylaşılan erişim imzası (SAS) korunur.
- **Azure DNS**. [Azure DNS](https://docs.microsoft.com/azure/dns/) DNS etki alanları için bir barındırma hizmetidir. Azure DNS, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlar. Etki alanlarınızı azure'da barındırarak aynı kimlik bilgilerini, API'leri, araçları kullanarak ve faturalama, diğer Azure Hizmetleri için kullandığınız DNS kayıtlarınızı yönetebilirsiniz. Özel etki alanı adı (ör. contoso.com) kullanmak için özel etki alanı adını IP adresine eşleyen DNS kayıtları oluşturun. Daha fazla bilgi için [API Yönetimi'nde bir özel etki alanı adı yapılandırma](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Kullanım [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/) veya kimlik doğrulaması için başka bir kimlik sağlayıcısı. Azure AD, API uç noktalarını geçirerek erişim için kimlik doğrulaması sağlar bir [API Management için JSON Web belirteci](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) doğrulamak için. Azure AD (yalnızca standart ve Premium Katmanlar) API Management Geliştirici portalına erişim güvenliğini sağlayabilirsiniz.

Mimari, işlemi için temel bazı desenleri sahiptir:

* Mevcut arka uç HTTP API API Management Geliştirici Portalı aracılığıyla yayımlanır. Portalda, geliştiriciler (ya da iç kuruluşunuz, dış veya her ikisi de) bu API'lere giden çağrıların uygulamalarınızla tümleştirebilirsiniz.
* Birleşik API, logic apps kullanarak ve bu çağrıları yazılım olarak hizmet (SaaS) sistemleri, Azure Hizmetleri ve API Management'a yayımlanan herhangi bir API işlemlerini tarafından oluşturulur. [Logic apps ayrıca yayımlanan](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) API Management Geliştirici Portalı aracılığıyla.
- Uygulamaları Azure AD'ye kullanın [bir OAuth 2.0 güvenlik belirteci almak](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) , gerekli olan bir API'ye erişim elde etmek için.
- API Management [güvenlik belirtecini doğrular](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) isteği arka uç API veya mantıksal uygulamaya geçirir.
- Service Bus kuyruklarını alışkın olduğunuz [ayrıştırmak](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) uygulama etkinlik ve [kesintisiz yük artış](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Logic apps, üçüncü taraf uygulamalar tarafından sıraya eklenen ya da (Resimsiz) iletileri kuyruğa API Management aracılığıyla bir HTTP API'si olarak yayımlayarak.
- Bir Service Bus kuyruğuna ileti eklendiğinde, bir olayı tetikler. Mantıksal uygulama tarafından bir olay tetiklenir. Mantıksal uygulama, ardından iletiyi işler.
- Diğer Azure Hizmetleri (örneğin, Azure Blob Depolama ve Azure Event Hubs), ayrıca olayları Event Grid'e yayımlayın. Bu hizmetler, bir olay alırsınız ve ardından sonraki eylemleri gerçekleştirmek için logic apps'i tetikleyin.

## <a name="recommendations"></a>Öneriler

Bu makalede açıklanan genel mimarisinin gereksinimlerinizi farklılık gösterebilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="service-bus-tier"></a>Hizmet veri yolu katmanı

Service Bus Premium katmanı kullanın. Premier katman Event Grid bildirimleri destekler. Daha fazla bilgi için [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Event Grid fiyatlandırması

Event Grid, sunucusuz bir model kullanır. Faturalandırma işlemleri (olay yürütme) sayısına göre hesaplanır. Daha fazla bilgi için [Event Grid fiyatlandırma](https://azure.microsoft.com/pricing/details/event-grid/). Şu anda Event Grid için hiçbir katman hakkında önemli noktalar vardır.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Service Bus iletileri tüketmeye PeekLock kullanın

Service Bus iletileri kullanmak için bir mantıksal uygulama oluşturduğunuzda, kullanmak [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) iletiler grubunu erişmek için mantıksal uygulamadaki. Mantıksal uygulama, PeekLock kullandığınızda her iletiyi tamamlamak ya da ileti durdurmadan önce doğrulamak için adımları gerçekleştirebilirsiniz. Bu yaklaşım, yanlışlıkla ileti kaybına karşı korur.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Bir Event Grid Tetikleyici etkinleştirildiğinde, birden çok nesne için işaretleyin

Bir Event Grid Tetikleyici etkinleştirildiğinde, yalnızca "bunlardan en az birini gerçekleştiğini." anlamına gelir Event Grid görünür bir Service Bus kuyruğuna bir ileti üzerinde bir mantıksal uygulama tetiklendiğinde, örneğin, mantıksal uygulama her zaman bir veya daha fazla iletileri işlemek için uygun olabilir varsaymanız gerekir.

### <a name="region"></a>Bölge

Ağ gecikmesini en aza indirmek için API Management, Logic Apps ve Service Bus ile aynı bölgede sağlayın. Genellikle, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu da bir bölge vardır. Bölge, dağıtım meta verileri nerede depolandığını ve nereye dağıtım şablonu, gelen yürütür belirtir. Kaynak grubu ve kaynaklarını dağıtım sırasında kullanılabilirliği geliştirmek için aynı bölgeye yerleştirin.

## <a name="scalability"></a>Ölçeklenebilirlik

Service Bus Premium katman, daha yüksek ölçeklenebilirlik elde etmek için Mesajlaşma birimi sayısının ölçeğini ölçeklendirebilirsiniz. Premium katmanı yapılandırmaları bir, iki veya dört Mesajlaşma birimine sahip olabilir. Service Bus'ı ölçeklendirme hakkında daha fazla bilgi için bkz. [Service Bus Mesajlaşması kullanarak en iyi uygulamalar için performans iyileştirmeleri](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Kullanılabilirlik

Şu anda, Azure API Management hizmet düzeyi sözleşmesi (SLA), temel, standart ve Premium katmanları için % 99,9 aşamasındadır. Premium katmanında en az bir birim iki veya daha fazla bölgede dağıtımını yapılandırmalarla % 99,95 oranında SLA'sı vardır.

Şu anda, Azure Logic Apps için SLA % 99,9 aşamasındadır.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Coğrafi olağanüstü durum kurtarma önemli bir kesinti oluşursa yük devretmeyi etkinleştirmek için Service Bus Premium uygulamayı düşünün. Daha fazla bilgi için [Azure Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Yönetilebilirlik

Üretim, geliştirme için ayrı kaynak grupları oluşturun ve test ortamları. Ayrı kaynak gruplarında kolaylaştırır dağıtımları yönetmek, test dağıtımlarını silmeyi ve erişim hakları atayın.

Kaynakları kaynak gruplarına atadığınızda, aşağıdaki faktörleri göz önünde bulundurun:

- **Yaşam döngüsü**. Genel olarak, aynı kaynak grubunda aynı yaşam döngüsüne sahip kaynakları yerleştirin.
- **Erişim**. Kullanabileceğiniz [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) bir grup içindeki kaynaklara erişim ilkelerini uygulamak için (RBAC).
- **Faturalama**. Kaynak grubu için toplama maliyetleri görüntüleyebilirsiniz.
- **API Yönetimi fiyatlandırma katmanı**. Biz, geliştirme için geliştirici katmanı kullanmanızı öneririz ve test ortamları. Üretim öncesi için üretim ortamınızın bir çoğaltma dağıtma testleri çalıştırma ve maliyeti en aza indirmek için ardından kapatılmasını öneririz.

Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Dağıtım

Kullanmanızı öneririz [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md) API Management, Logic Apps, Event Grid ve Service Bus'ı dağıtmak için. Şablonlar, PowerShell veya Azure CLI aracılığıyla dağıtımları otomatik hale getirmek kolaylaştırır.

API Management, herhangi tek bir mantıksal uygulamalar, olay ızgarası konu başlıkları ve Service Bus ad alanları, kendi ayrı bir Resource Manager şablonlarında koyarak öneririz. Ayrı şablonları kullandığınızda, kaynaklar kaynak denetimi sistemlerini depolayabilirsiniz. Bu şablonlar ardından, birlikte veya tek tek sürekli tümleştirme/sürekli dağıtım (CI/CD) işleminin bir parçası olarak da dağıtabilirsiniz.

### <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

API Management ve Logic Apps gibi Azure İzleyicisi'ni kullanarak Service Bus izleyebilirsiniz. Azure İzleyici, her hizmet için yapılandırılmış ölçümleri temel alan bilgiler sağlar. Azure İzleyici varsayılan olarak etkindir.

## <a name="security"></a>Güvenlik

Service Bus SAS kullanarak güvenli hale getirin. Kullanabileceğiniz [SAS kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) hizmet veri yolu kaynaklarını belirli haklarına sahip bir kullanıcı erişimi vermek için. Daha fazla bilgi için [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Service Bus kuyruğuna (yeni ileti gönderme için) bir HTTP uç noktası olarak kullanıma sunulan gerekiyorsa, uç nokta cephesinde tarafından güvenli hale getirmek için API Yönetimi kullanmanız gerekir. Uç nokta sonra sertifikaları veya OAuth ile uygun şekilde güvenli hale getirilebilir. Bir uç nokta güvenliğini sağlamak için en kolay yolu bir mantıksal uygulama bir istek/yanıt HTTP tetikleyicisi olan bir aracı olarak kullanmaktır.

Event Grid olay teslimi bir doğrulama kodu aracılığıyla güvenliğini sağlar. Olay kullanmak için Logic Apps kullanıyorsanız, doğrulama otomatik olarak gerçekleştirilir. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [basit Kurumsal tümleştirme](logic-apps-architectures-simple-enterprise-integration.md).