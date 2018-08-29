---
title: Kurumsal tümleştirme mimari deseni - Azure tümleştirme hizmetleri
description: Bu mimari başvurusu, Azure Logic Apps, Azure API Management, Azure Service Bus ve Azure Event Grid ile bir kurumsal tümleştirme düzeni nasıl uygulayacağınıza dair gösterir
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 2ffb1f7edef0cf92cbbf7adc4314967858bcfeb1
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128652"
---
# <a name="enterprise-integration-architecture-with-queues-and-events"></a>Kuyruklar ve olayları ile Kurumsal tümleştirme mimarisi

Bu makalede, Azure tümleştirme hizmetleri kullanırken tümleştirme uygulamaya uygulayabilirsiniz kendini kanıtlamış yöntemleri kullanan bir kurumsal tümleştirme mimarisi açıklanmaktadır. Bu mimari HTTP API'leri, iş akışı ve orchestration gerektiren birçok farklı uygulama desenleri için temel olarak kullanabilirsiniz.

![Mimari diyagramı - sıralar ve olaylar ile Kurumsal tümleştirme](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

Bu seri, genel tümleştirme uygulaması oluşturmak için geçerli olabilecek yeniden kullanılabilir bileşen parçalarına açıklar. Tüm kurumsal Azure hizmet veri yolu uygulamaları için basit bir noktadan noktaya uygulamalardan arasında çok sayıda olası uygulama tümleştirme teknolojileri sahip olduğundan, uygulamalarınızı ve altyapınızı için uygulamanız gereken hangi bileşenlerin göz önünde bulundurun.

## <a name="architecture-components"></a>Mimari bileşenler

Bu mimari makalesinde açıklanan ve mimarinin geliştirir [başvuru mimarisi: basit bir kurumsal tümleştirme](../logic-apps/logic-apps-architectures-simple-enterprise-integration.md). Bu mimarisinin [önerileri](../logic-apps/logic-apps-architectures-simple-enterprise-integration.md#recommendations) burada da geçerli olur, ancak konuyu uzatmamak amacıyla, bu makalede bu önerileri atlar [önerileri](#recommendations) bölümü. Bu Kurumsal tümleştirme mimarisi şu bileşenleri içerir:

- **Kaynak grubu**: A [kaynak grubu](../azure-resource-manager/resource-group-overview.md) Azure kaynakları için mantıksal bir kapsayıcıdır.

- **Azure API Management**: [API Management](https://docs.microsoft.com/azure/api-management/) yayımlama, güvenliğini sağlama ve HTTP API'lerini dönüştürme için tam olarak yönetilen bir platform hizmetidir.

- **Azure API Management Geliştirici Portalı**: her bir Azure API Management örneğinin erişim sağlayan [Geliştirici Portalı](../api-management/api-management-customize-styles.md). Bu portal, belgeler ve kod örneklerine erişmenizi sağlar. Geliştirici portalındaki API'ler ayrıca test edebilirsiniz.

- **Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) kurumsal iş akışları ve tümleştirmeler oluşturmak için sunucusuz bir platformdur.

- **Bağlayıcılar**: Logic Apps kullanan [Bağlayıcılar](../connectors/apis-list.md) hizmetleri kullanılan yaygın olarak bağlanmak için. Logic Apps bağlayıcıları yüzlerce sunar, ancak bir özel bağlayıcı oluşturabilirsiniz.

- **Azure Service Bus**: [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) güvenli ve güvenilir Mesajlaşma sağlar. Mesajlaşma uygulamalarını ayırma ve diğer ileti tabanlı sistemlerle tümleştirmek için kullanabilirsiniz.

- **Azure Event Grid**: [Event Grid](../event-grid/overview.md) yayımlama ve uygulama olayları teslim için sunucusuz bir platformdur.

- **IP adresi**: Azure API Yönetimi hizmeti, sabit bir ortak sahip [IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve bir etki alanı adı. Azure-api.net, örneğin, contoso.azure-api.net, alt etki alanı varsayılan etki alanı adıdır, ancak ayrıca [özel etki alanları](../api-management/configure-custom-domain.md). Logic Apps ve Service Bus bir genel IP adresi de. Ancak, güvenlik için bu mimari yalnızca IP adresi API Management'ın Logic Apps uç noktalarını çağırma için erişimi kısıtlar. Service Bus çağrıları bir paylaşılan erişim imzası (SAS) korunur.

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) DNS etki alanları için bir barındırma hizmetidir. Azure DNS, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlar. Etki alanlarınızı azure'da barındırarak aynı kimlik bilgilerini, API'leri, araçları kullanarak ve faturalama, diğer Azure Hizmetleri için kullandığınız DNS kayıtlarınızı yönetebilirsiniz. Özel etki alanı, contoso.com gibi kullanılacak özel etki alanı adını IP adresine eşleyen DNS kayıtları oluşturun. Daha fazla bilgi için [API Yönetimi'nde bir özel etki alanı adı yapılandırma](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: kullanabileceğiniz [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/) veya kimlik doğrulaması için başka bir kimlik sağlayıcısı. Azure AD, API uç noktalarını geçirerek erişim için kimlik doğrulaması sağlar bir [API Management için JSON Web belirteci](../api-management/policies/authorize-request-based-on-jwt-claims.md) doğrulamak için. Standart ve Premium katmanları için Azure AD, API Management Geliştirici portalına erişim güvenliğini sağlayabilirsiniz.

## <a name="patterns"></a>Desenler 

Bu mimari, işlemi için temel bazı desenleri kullanır:

* Mevcut arka uç HTTP API API Management Geliştirici Portalı aracılığıyla yayımlanır. Portalda, ya da olan geliştiriciler, kuruluşunuz, dış veya her ikisi de iç  
Bu API'lere giden çağrıların uygulamalarınızla tümleştirebilirsiniz.

* Birleşik API, hizmet (SaaS) sistemleri, Azure Hizmetleri ve API Management'a yayımlanan herhangi bir API çağrısına yazılım düzenleyin, logic apps kullanarak oluşturulur. Mantıksal uygulamalar, ayrıca [API Management Geliştirici Portalı aracılığıyla yayımlanan](../api-management/import-logic-app-as-api.md).

* Uygulamaları kullanmak için Azure AD [bir OAuth 2.0 güvenlik belirtecini alma](../api-management/api-management-howto-protect-backend-with-aad.md) , gerekli olan bir API'ye erişim elde etmek için.

* Azure API Management [güvenlik belirtecini doğrular](../api-management/api-management-howto-protect-backend-with-aad.md) isteği arka uç API veya mantıksal uygulamaya geçirir.

* Azure Service Bus Kuyrukları için kullanılan [ayırma](../service-bus-messaging/service-bus-messaging-overview.md) uygulama etkinlik ve [yük artışlarını düzeltme](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Logic apps, üçüncü taraf uygulamalar tarafından sıraya eklenen ya da (gösterilmemiştir) iletileri kuyruğa API Management aracılığıyla bir HTTP API'si olarak yayımlayarak.

* Bir Service Bus kuyruğuna ileti eklendiğinde, bir olayı tetikler. Ardından iletiyi işleyen bir mantıksal uygulama olayı tetikler.

* Azure Blob Depolama ve Azure Event Hubs gibi diğer Azure Hizmetleri, ayrıca olayları Event Grid'e yayımlayın. Bu hizmetler, bir olay alırsınız ve ardından sonraki eylemleri gerçekleştirmek için logic apps'i tetikleyin.

## <a name="recommendations"></a>Öneriler

Gereksinimlerinize göre bu makalede açıklanan genel mimarisinin farklılık gösterebilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="service-bus-tier"></a>Hizmet veri yolu katmanı

Service Bus Premium katmanı, Event Grid bildirimlerini destekleyen kullanın. Daha fazla bilgi için [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Event Grid fiyatlandırması

Event Grid, sunucusuz bir model kullanır. Faturalandırma işlemleri (olay yürütme) sayısına göre hesaplanır. Daha fazla bilgi için [Event Grid fiyatlandırma](https://azure.microsoft.com/pricing/details/event-grid/). Şu anda Event Grid için hiçbir katman hakkında önemli noktalar vardır.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Service Bus iletileri tüketmeye PeekLock kullanın

Service Bus iletileri kullanmak için bir mantıksal uygulama oluşturduğunuzda, mantıksal uygulamanızı kullanmak sahip [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) iletiler grubunu erişmek için. Mantıksal uygulama, PeekLock kullandığınızda her iletiyi tamamlamak ya da ileti durdurmadan önce doğrulamak için adımları gerçekleştirebilirsiniz. Bu yaklaşım, yanlışlıkla ileti kaybına karşı korur.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Bir Event Grid Tetikleyici etkinleştirildiğinde, birden çok nesne için işaretleyin

Bir Event Grid Tetikleyici etkinleştirildiğinde bu eylemi yalnızca "bunlardan en az birini gerçekleştiğini." anlamına gelir Event Grid görünür bir Service Bus kuyruğuna bir ileti üzerinde bir mantıksal uygulama tetiklendiğinde, örneğin, mantıksal uygulama her zaman bir veya daha fazla iletileri işlemek için uygun olabilir varsaymanız gerekir.

### <a name="region"></a>Bölge

Ağ gecikmesini en aza indirmek için API Management, Logic Apps ve Service Bus için aynı bölgeyi seçin. Genel olarak, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu da bir bölge vardır. Bu bölge, dağıtım meta verileri depolamak nereden ve nereye dağıtım şablonu yürütmek belirtir. Dağıtım sırasında kullanılabilirliği artırmak için söz konusu kaynak grubu ve kaynakları aynı bölgeye yerleştirin.

## <a name="scalability"></a>Ölçeklenebilirlik

Service Bus Premium katman, daha yüksek ölçeklenebilirlik elde etmek için Mesajlaşma birimi sayısının ölçeğini ölçeklendirebilirsiniz. Premium katmanı yapılandırmaları bir, iki veya dört Mesajlaşma birimine sahip olabilir. Service Bus'ı ölçeklendirme hakkında daha fazla bilgi için bkz. [Service Bus Mesajlaşması kullanarak en iyi uygulamalar için performans iyileştirmeleri](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Kullanılabilirlik

* Azure API Management hizmet düzeyi sözleşmesi (SLA), temel, standart ve Premium katmanları için % 99,9 şu anda aşamasındadır. En az bir birimin iki veya daha fazla bölgede bulunan bir dağıtım ile Premium katmanı yapılandırmaları için % 99,95 oranında SLA var.

* Şu anda Azure Logic Apps için SLA % 99,9 ' dir.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Önemli bir kesinti oluşursa yük devretmeyi etkinleştirmek için Service Bus Premium coğrafi olağanüstü durum kurtarma uygulama göz önünde bulundurun. Daha fazla bilgi için [Azure Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Yönetilebilirlik

Üretim, geliştirme için ayrı kaynak grupları oluşturun ve test ortamları. Ayrı kaynak gruplarında dağıtımları yönetmek, test dağıtımlarını silmeyi ve erişim haklarını atamayı kolaylaştırır.

Kaynakları kaynak gruplarına atadığınızda, şu etmenleri dikkate alın:

* **Yaşam döngüsü**: genel olarak, aynı kaynak grubunda aynı yaşam döngüsüne sahip kaynakları yerleştirin.

* **Erişim**: Grup içindeki kaynaklara erişim ilkeleri uygulamak için kullanabileceğiniz [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md).

* **Faturalama**: kaynak grubu için toplama maliyetleri görüntüleyebilirsiniz.

* **API Yönetimi fiyatlandırma katmanı**: Geliştirici katmanı geliştirme ve test ortamları için kullanın. Ön üretim sırasında maliyetleri en aza indirmek için üretim ortamınızın bir çoğaltma dağıtma, testlerinizi çalıştırmak ve kapatın.

Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Dağıtım

* API Management, Logic Apps, Event Grid ve Service Bus'ı dağıtmak için [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md). Şablonlar, PowerShell veya Azure CLI kullanarak otomatikleştirerek dağıtımları daha kolay olun.

* API Management, herhangi tek bir mantıksal uygulamalar, olay ızgarası konu başlıkları ve Service Bus ad alanları, kendi ayrı bir Resource Manager şablonlarında yerleştirin. Ayrı bir şablon kullanarak kaynakları kaynak denetimi sistemlerini depolayabilirsiniz. Bu şablonlar ardından, birlikte veya tek tek sürekli tümleştirme/sürekli dağıtım (CI/CD) işleminin bir parçası olarak da dağıtabilirsiniz.

## <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

API Management ve Logic Apps gibi Azure izleme, varsayılan olarak etkin kullanarak Service Bus izleyebilirsiniz. Azure İzleyici, her hizmet için yapılandırılmış ölçümleri temel alan bilgiler sağlar. 

## <a name="security"></a>Güvenlik

Service Bus güvenliğini sağlamak için paylaşılan erişim imzası (SAS) kullanın. Örneğin, kullanarak Service Bus kaynaklarını belirli haklarına sahip bir kullanıcı erişim izni verebilirsiniz [SAS kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md). Daha fazla bilgi için [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Service Bus kuyruğuna bir HTTP uç noktası olarak kullanıma sunmak ihtiyacınız varsa, örneğin, yeni ileti göndermek için API Management sıra uç nokta cephesinde tarafından güvenli hale getirmek için kullanın. Ardından, sertifikalar veya uygun şekilde OAuth kimlik doğrulaması ile uç nokta güvenliğini sağlayabilirsiniz. En kolay yolu, bir uç nokta güvenli bir mantıksal uygulama, aracı olarak bir HTTP istek/yanıt tetikleyicisi ile kullanıyor.

Event Grid hizmeti aracılığıyla bir doğrulama kodu olay teslimi güvenliğini sağlar. Olay kullanmak için Logic Apps kullanıyorsanız, doğrulama otomatik olarak gerçekleştirilir. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [basit Kurumsal tümleştirme](logic-apps-architectures-simple-enterprise-integration.md)
