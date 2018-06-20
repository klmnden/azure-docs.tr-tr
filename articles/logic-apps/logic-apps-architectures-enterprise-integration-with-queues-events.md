---
title: Azure Integration Services başvuru mimarisi
description: Logic Apps, API Management, hizmet veri yolu ve olay kılavuz ile bir kurumsal tümleştirme desen uygulamak nasıl gösteren Referans Mimarisi
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
ms.openlocfilehash: db3350b3c70f6e6f35949e8423334061849b7b37
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232587"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Sıralar ve olaylar ile Kurumsal tümleştirme: başvuru mimarisi

## <a name="overview"></a>Genel Bakış

Bu başvuru mimarisinin Azure tümleştirme hizmetleri kullanan bir tümleştirme uygulama için kanıtlanmış yöntemler kümesi gösterir. Bu mimari, bu HTTP API'leri, iş akışı ve düzenleme gerektiren birçok farklı uygulama düzenleri temel olarak hizmet verebilir.

*Tümleştirme teknolojinin birçok olası uygulama uygulamasından basit noktası için noktası bir tam Kurumsal hizmet veri yolu vardır. Bu mimari seri herhangi bir tümleştirme uygulaması oluşturmaya yönelik yeniden kullanılabilir bileşen parçalarını çıkışı ayarlar – mimarları uygulamalarını ve altyapısını için gerekir bileşenleri göz önünde bulundurmanız gerekir.*

![Mimari diyagramı - sıraları & olayları ile Kurumsal tümleştirme](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Mimari

Bu mimari gösterildiği bir derlemeler [basit Kurumsal tümleştirme](logic-apps-architectures-simple-enterprise-integration.md). Aşağıdaki bileşenleri içerir:

- Kaynak grubu. Bir kaynak grubu, Azure kaynakları için mantıksal bir kapsayıcıdır.
- Azure API Management. Azure API Management, yayımlama, güvenli hale getirme ve HTTP API'leri dönüştürme için tam olarak yönetilen bir platformdur.
- Azure API Management Geliştirici Portalı. Her Azure API Management örneği belgeler, kod örnekleri ve bir API test olanağı erişim veren bir Geliştirici Portalı birlikte gönderilir.
- Azure mantıksal uygulamaları. Logic Apps, kurumsal iş akışı ve tümleştirme oluşturmak için sunucusuz bir platformdur.
- Bağlayıcılar. Bağlayıcılar mantıksal uygulamalar tarafından yaygın olarak kullanılan hizmetlerine bağlanmak için kullanılır. Logic Apps farklı bağlayıcılar yüzlerce zaten var, ancak bunlar da özel bir bağlayıcı kullanılarak oluşturulabilir.
- Azure hizmet veri yolu. Service Bus, güvenli ve güvenilir Mesajlaşma sağlar. Mesajlaşma uygulamaları birbirinden XML'deki eşleştiği ve ileti tabanlı diğer sistemlerle tümleştirmek için kullanılabilir.
- Azure Event kılavuz. Olay kılavuz yayımlama ve uygulama olayları teslim sunucusuz bir platformdur.
- IP adresi. Azure API Management hizmeti, sabit bir ortak IP adresi ve bir etki alanı adı vardır. Azure-api.net contoso.azure api.net gibi bir etki alanının alt etki alanı adıdır. Mantıksal uygulamalar ve hizmet veri yolu da genel bir IP adresi vardır; Ancak, bu mimaride biz yalnızca IP adresi, API Management Logic apps Uç noktalara (güvenlik için) çağırmak için erişimi kısıtlayın. Hizmet veri yolu çağrıları tarafından paylaşılan erişim imzası güvenli hale getirilir.
- Azure DNS. Azure DNS barındırma için bir DNS etki alanı, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. Özel etki alanı adı (contoso.com gibi) kullanmak için özel etki alanı adı IP adresine eşleyen DNS kayıtlarını oluşturun. Daha fazla bilgi için Azure API Management'te özel etki alanı adı yapılandırma konusuna bakın.
- Azure Active Directory (Azure AD). Kimlik doğrulaması için Azure AD veya başka bir kimlik sağlayıcısı kullanın. Azure AD (bir JSON Web belirteci doğrulamak API Management geçirerek) erişim API uç noktaları için kimlik doğrulaması sağlar ve API yönetim Geliştirici Portalı'nı (yalnızca standart ve Premium katmanları) erişimi güvenli hale getirebilirsiniz.

Bu mimari çalışması için bazı temel desenleri vardır:

1. Varolan arka uç HTTP API'leri API Management Geliştirici geliştiriciler izin vererek portalı üzerinden, yayımlanan (ya da iç kuruluşunuz, dış veya her ikisi de) Bu API çağrıları uygulamalarıyla bütünleştirmek için.
2. Bileşik API'leri Logic Apps kullanılarak oluşturulan; SAAS sistemleri çağrıları yönetme, Azure Hizmetleri ve tüm API'leri API Management yayımladı. Logic Apps, API Management Geliştirici Portalı üzerinden de yayımlanır.
3. Uygulamaları Azure Active Directory'yi kullanarak bir API'ye erişim kazanmak için gerekli bir OAuth 2.0 güvenlik belirtecini almak.
4. Azure API Management güvenlik belirteci doğrular ve isteği arka uç API'si/mantıksal uygulama geçirir.
5. Service Bus kuyrukları, uygulama etkinlik ve kesintisiz ani yük aynı şekilde kullanılır. 3. taraf uygulamaları, Logic Apps tarafından sıraya eklenen ya da (resim değil) iletileri bir HTTP API API Yönetimi yoluyla olarak sıranın yayımlayarak.
6. Service Bus kuyruğuna ileti eklendiğinde, bir olay gönderir. Bir mantıksal uygulama tarafından bu olay tetiklenir ve iletiyi işler.
7. Benzer şekilde, diğer Azure hizmetleriyle (ör. Blob Storage, olay hub'ı) olay kılavuza de olayları yayımlar. Bu olay almak ve sonraki eylemleri gerçekleştirmek için Logic Apps tetikler.

## <a name="recommendations"></a>Öneriler

Gereksinimleriniz, burada açıklanan mimariden farklı olabilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="service-bus-tier"></a>Hizmet veri yolu katmanı

Bu olay kılavuz bildirimleri şu anda desteklediği Service Bus premium katmanı kullanın (tüm katmanlar arasında destek bekleniyor). Bkz: [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Kılavuz olay fiyatlandırma

Olay kılavuz sunucusuz bir model kullanarak çalışma – işlemleri (olay yürütme) sayısına faturalama dayalı olarak hesaplanır. Bkz: [olay kılavuz fiyatlandırma](https://azure.microsoft.com/pricing/details/event-grid/) daha fazla bilgi için. Şu anda olay kılavuz için hiçbir katmanı noktalar da vardır.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>Hizmet veri yolu ileti tüketen PeekLock kullanın

Hizmet veri yolu ileti tüketmeye Logic Apps oluştururken kullanmak [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) iletiler grubunu erişmek için bu mantıksal uygulama içinde. Mantıksal uygulama adımları tamamlayarak veya durdurmadan önce her ileti doğrulamak için daha sonra gerçekleştirebilirsiniz. Bu yaklaşım yanlışlıkla ileti kaybına karşı korur.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Bir olay kılavuz tetikleyicisi başlatıldığında birden çok nesnenin onay

Olay kılavuz Tetikleyicileri tetikleme yalnızca "en az 1 bunlardan hiçbiri oldu" anlamına gelir. Bir Service Bus kuyruktaki görünen bir ileti üzerinde bir mantıksal uygulama olay kılavuz harekete geçirdiğinde, örneğin, mantıksal uygulama her zaman bir veya daha fazla iletileri işlemek kullanılabilir olmasını varsayın.

### <a name="region"></a>Bölge

API Management, Logic Apps ve Service Bus aynı bölgede ağ gecikmesini en aza indirmek için sağlayın. Genellikle, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu dağıtım meta verileri nerede depolanacağını belirtir, bir bölge da sahiptir ve burada dağıtım şablonu yürütüldüğünde gelen. Kaynak grubu ve kaynakları aynı bölgeye yerleştirin. Bu, dağıtım sırasında kullanılabilirliği iyileştirebilir.

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Azure Service Bus premium katmanı daha yüksek ölçeklenebilirlik elde etmek için Mesajlaşma birimlerinin sayısı genişletme. Premium 1, 2 veya 4 Mesajlaşma birimi olabilir. Azure Service Bus ölçekleme konusunda daha ayrıntılı yönergeler için bkz: [performans iyileştirmeleri Service Bus Mesajlaşma hizmeti kullanmak için en iyi uygulamaları](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Yazma zaman Azure API Management hizmet düzeyi sözleşmesi (SLA) temel, standart ve Premium katmanları için % 99,9 ' dir. Premium katmanı yapılandırmaları iki veya daha fazla bölge içinde en az bir birim dağıtımınızla birlikte %99,95 SLA'sı vardır.

Yazma zaman Azure mantıksal uygulamaları için hizmet düzeyi sözleşmesi (SLA) % 99,9 ' dir.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Yük devretme durumunda ciddi bir kesinti etkinleştirmek için Service Bus premium içinde coğrafi olağanüstü durum kurtarma uygulama göz önünde bulundurun. Daha fazla bilgi edinin [Azure Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Üretim, geliştirme, ayrı kaynak grupları oluşturmak ve test ortamları. Bu, dağıtımları yönetmeyi, test dağıtımlarını silmeyi ve erişim haklarını atamayı kolaylaştırır.
Kaynakları kaynak gruplarına atarken, aşağıdakileri göz önünde bulundurun:

- Yaşam döngüsü. Genel olarak, aynı yaşam döngüsüne sahip kaynakları aynı kaynak grubuna yerleştirin.
- Erişim. Kullanabileceğiniz [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) bir grup içindeki kaynaklara erişim ilkeleri uygulamak için (RBAC).
- Faturalandırma. Kaynak grubu için aktarılmış maliyetleri görüntüleyebilirsiniz.
- Fiyatlandırma katmanı için API Management – Geliştirici katmanı geliştirme ve test ortamları için kullanmanızı öneririz. Üretim öncesi için üretim ortamınızın çoğaltmasını dağıtma testleri çalıştırmak ve maliyeti en aza indirmek için sonra kapatılıyor öneririz.

Daha fazla bilgi için bkz: [kaynak grubu](../azure-resource-manager/resource-group-overview.md) genel bakış.

### <a name="deployment"></a>Dağıtım

Kullanmanızı öneririz [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md) Azure API Management, Logic Apps, olay kılavuz ve hizmet veri yolu dağıtma. Şablonları, PowerShell veya Azure komut satırı arabirimi (CLI) aracılığıyla dağıtımlarını otomatikleştirin kolaylaştırır.

Azure API Management, tüm tek tek Logic Apps, olay kılavuz konuları ve hizmet veri yolu ad kendi ayrı Resource Manager şablonları koyma öneririz. Bu, bunları kaynak denetim sistemleri için içine depolama olanak tanır. Bu şablonlar birlikte veya ayrı ayrı bir sürekli tümleştirme/sürekli (CI/CD) dağıtım işleminin bir parçası olarak dağıtılabilir.

### <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

Hizmet veri yolu, API Management ve Logic Apps gibi Azure İzleyicisi'ni kullanarak izlenebilir. Azure İzleyici varsayılan olarak etkindir ve her hizmet için yapılandırılan farklı ölçümleri temel bilgileri sağlayacaktır.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Paylaşılan erişim imzası kullanarak hizmet veri yolu güvenliğini sağlayın. [SAS kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) belirli haklar ile Service Bus kaynaklarına kullanıcı erişimi sağlar. Daha fazla bilgi edinin [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Ayrıca, Service Bus kuyruğuna (yeni iletiler nakil izin vermek için) bir HTTP uç noktası, açığa çıkarılması gerekir API Management, uç nokta fronting tarafından güvenli hale getirmek için kullanılmalıdır. Bundan sonra sertifikalar veya OAuth ile uygun şekilde güvenli hale getirilebilir. Bunu yapmanın en kolay yolu, bir aracı gibi birlikte bir istek/yanıt HTTP tetikleyicisi bir mantıksal uygulama kullanıyor.

Olay kılavuz olay teslimi bir doğrulama kodu aracılığıyla güvenliğini sağlar. Olay tüketmeye LogicApps kullanırsanız, bu otomatik olarak gerçekleştirilir. Daha fazla ayrıntı görmek [olay kılavuz güvenlik ve kimlik doğrulama](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Basit Kurumsal tümleştirme](logic-apps-architectures-simple-enterprise-integration.md)
