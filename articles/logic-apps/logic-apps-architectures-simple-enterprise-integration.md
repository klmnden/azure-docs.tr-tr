---
title: Azure tümleştirme hizmetleri - basit Kurumsal tümleştirme
description: Azure Logic Apps ile Azure API Management basit Kurumsal tümleştirme desen uygulamak nasıl gösteren Referans Mimarisi
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
ms.openlocfilehash: 17f591a470bf65d3c9365bca9c5ae2d5a6c9574f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232543"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Basit Kurumsal tümleştirme - başvuru mimarisi

## <a name="overview"></a>Genel Bakış

Bu başvuru mimarisinin Azure tümleştirme hizmetleri kullanan bir tümleştirme uygulama için kanıtlanmış yöntemler kümesi gösterir. Bu mimari, bu HTTP API'leri, iş akışı ve düzenleme gerektiren birçok farklı uygulama düzenleri temel olarak hizmet verebilir.

*Tümleştirme teknolojinin birçok olası uygulama uygulamasından basit noktası için noktası bir tam Kurumsal hizmet veri yolu vardır. Bu mimari seri herhangi bir tümleştirme uygulaması oluşturmaya yönelik yeniden kullanılabilir bileşen parçalarını çıkışı ayarlar – mimarları uygulamalarını ve altyapısını için gerekir bileşenleri göz önünde bulundurmanız gerekir.*

   ![Mimari diyagramı - basit Kurumsal tümleştirme](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Mimari

Mimari aşağıdaki bileşenlere sahiptir:

- Kaynak grubu. Bir kaynak grubu, Azure kaynakları için mantıksal bir kapsayıcıdır.
- Azure API Management. Azure API Management, yayımlama, güvenli hale getirme ve HTTP API'leri dönüştürme için tam olarak yönetilen bir platformdur.
- Azure API Management Geliştirici Portalı. Her Azure API Management örneği belgeler, kod örnekleri ve bir API test olanağı erişim veren bir Geliştirici Portalı birlikte gönderilir.
- Azure mantıksal uygulamaları. Logic Apps, kurumsal iş akışı ve tümleştirme oluşturmak için sunucusuz bir platformdur.
- Bağlayıcılar. Bağlayıcılar mantıksal uygulamalar tarafından yaygın olarak kullanılan hizmetlerine bağlanmak için kullanılır. Logic Apps farklı bağlayıcılar yüzlerce zaten var, ancak bunlar da özel bir bağlayıcı kullanılarak oluşturulabilir.
- IP adresi. Azure API Management hizmeti, sabit bir ortak IP adresi ve bir etki alanı adı vardır. Azure-api.net, contoso gibi bir etki alanının alt etki alanı adıdır. Azure-api.net. Logic Apps ayrıca bir ortak IP adresi vardır; Ancak, bu mimaride biz yalnızca IP adresi, API Management Logic apps Uç noktalara (güvenlik için) çağırmak için erişimi kısıtlayın.
- Azure DNS. Azure DNS barındırma için bir DNS etki alanı, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. Özel etki alanı adı (contoso.com gibi) kullanmak için özel etki alanı adı IP adresine eşleyen DNS kayıtlarını oluşturun. Daha fazla bilgi için Azure API Management'te özel etki alanı adı yapılandırma konusuna bakın.
- Azure Active Directory (Azure AD). Kimlik doğrulaması için Azure AD veya başka bir kimlik sağlayıcısı kullanın. Azure AD (bir JSON Web belirteci doğrulamak API Management geçirerek) erişim API uç noktaları için kimlik doğrulaması sağlar ve API yönetim Geliştirici Portalı'nı (yalnızca standart ve Premium katmanları) erişimi güvenli hale getirebilirsiniz.

Bu mimari çalışması için bazı temel desenleri vardır:

1. Varolan arka uç HTTP API'leri API Management Geliştirici geliştiriciler izin vererek portalı üzerinden, yayımlanan (ya da iç kuruluşunuz, dış veya her ikisi de) Bu API çağrıları uygulamalarıyla bütünleştirmek için.
2. Bileşik API'leri Logic Apps kullanılarak oluşturulan; SAAS sistemleri çağrıları yönetme, Azure Hizmetleri ve tüm API'leri API Management yayımladı. Logic Apps, API Management Geliştirici Portalı üzerinden de yayımlanır.
3. Uygulamaları Azure Active Directory'yi kullanarak bir API'ye erişim kazanmak için gerekli bir OAuth 2.0 güvenlik belirtecini almak.
4. Azure API Management güvenlik belirteci doğrular ve isteği arka uç API'si/mantıksal uygulama geçirir.

## <a name="recommendations"></a>Öneriler

Gereksinimleriniz, burada açıklanan mimariden farklı olabilir. Bu bölümdeki önerileri bir başlangıç noktası olarak kullanın.

### <a name="azure-api-management-tier"></a>Azure API Management katmanı

Temel kullanmak için standart veya Premium katmanlarını çünkü bunlar üretim SLA sunar ve Azure bölgesi içinde genişleme desteği (birim sayısı değişir katmanı tarafından). Premium katmanı da birçok Azure bölgesinde genişleme destekler. Gerekli verimlilik düzeyinizi üzerinde seçtiğiniz katmanı temel ve özelliğini ayarlayın. Daha fazla bilgi için bkz: [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).

Bunlar çalıştırırken tüm API Management örnekler için sizden ücret kesilir. Ölçeklendirilmiş ve olması gerekmez, bu düzeyde performans her zaman API Management'ın saatlik faturalandırma ve ölçek yararlanarak göz önünde bulundurun.

### <a name="logic-apps-pricing"></a>Logic Apps fiyatlandırma

Logic Apps çalışır olarak bir [sunucusuz](logic-apps-serverless-overview.md) modeli – faturalama temel eylem ve bağlayıcı yürütmeye göre hesaplanır. Bkz: [Logic Apps fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/) daha fazla bilgi için. Şu anda Logic Apps için hiçbir katmanı noktalar da vardır.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps için zaman uyumsuz API çağrıları

Logic Apps, düşük gecikme süresi – örneğin zaman uyumsuz gerektirmeyen senaryoları veya semi uzun süreli API çağrıları en iyi şekilde çalışır. Düşük gecikme gerekiyorsa (örneğin, bir kullanıcı arabirimi blokları bir çağrı) Bu API veya işlemi farklı bir teknoloji, örneğin Azure işlevleri ya da uygulama hizmeti kullanılarak dağıtılan Web API kullanarak uygulama öneririz. Bu API olmasını öneririz hala API tüketicilere API Yönetimi'ni kullanarak fronted.

### <a name="region"></a>Bölge

API Management ve Logic Apps aynı bölgede ağ gecikmesini en aza indirmek için sağlama. Genellikle, kullanıcılarınıza en yakın bölgeyi seçin.

Kaynak grubu dağıtım meta verileri nerede depolanacağını belirtir, bir bölge da sahiptir ve burada dağıtım şablonu yürütüldüğünde gelen. Kaynak grubu ve kaynakları aynı bölgeye yerleştirin. Bu, dağıtım sırasında kullanılabilirliği iyileştirebilir.

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

API Management yöneticileri ekleme [önbelleğe alma ilkeleri](../api-management/api-management-howto-cache.md) hizmetinin ölçeklenebilirliği artırmak ve arka uç hizmetlerini yükünü azaltmak için uygun olduğunda.

Azure API Management temel, standart ve Premium katmanlar ile daha fazla kapasite sunmak için bir Azure bölgesinde genişletilebilir. Yöneticiler, kendi hizmet kullanımını çözümlemek ve ölçeği veya uygun şekilde ölçeklendirmeyi azaltın için kapasite ölçüm ölçümleri menü içinde kullanabilirsiniz.

Bir API Management hizmeti ölçeklendirmeye yönelik öneriler:

- Trafik düzenlerini – daha volatile trafik düzenlerini müşterilerle dikkate alması gerekiyor ölçeklendirme artan kapasite büyük gereksinimini sahip olur.
- % 66 yukarıda tutarlı kapasite ölçeği gerek gösteriyor olabilir.
- % 20 aşağıda tutarlı kapasite ölçekleme fırsatı gösteriyor olabilir.
- Yüklemek için her zaman önerilen üretimde etkinleştirmeden önce API Management hizmetiniz bir temsili yük ile test edin.

Premium katmanı hizmetlerini birçok Azure bölgesinde genişletilebilir. Müşteriler bu şekilde dağıtma (%99,95 % 99,9 aksine) daha yüksek bir SLA elde edersiniz ve birden çok bölgelerdeki kullanıcılar yakınında olmak hizmetleri sağlayabilirsiniz.

Logic Apps sunucusuz modeli Yöneticiler hizmeti ölçeklenebilirlik sağlamak için ek değerlendirme yapmak zorunda kalmazsınız anlamına gelir; Servis talebi karşılamak üzere otomatik olarak ölçeklendirir.

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Yazma zaman Azure API Management hizmet düzeyi sözleşmesi (SLA) temel, standart ve Premium katmanları için % 99,9 ' dir. Premium katmanı yapılandırmaları iki veya daha fazla bölge içinde en az bir birim dağıtımınızla birlikte %99,95 SLA'sı vardır.

Yazma zaman Azure mantıksal uygulamaları için hizmet düzeyi sözleşmesi (SLA) % 99,9 ' dir.

### <a name="backups"></a>Yedeklemeler

Azure API Management yapılandırma olmalıdır [düzenli olarak yedeklenen](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (uygun şekilde değişiklik düzenli üzerinde göre) ve bir konum veya hizmetin bulunduğu farklı Azure bölgesini depolanan yedek dosyaları. Müşteriler daha sonra kendi DR stratejisi için iki seçenekten birini seçebilirsiniz:

1. DR olay yeni bir API Management örneği sağlandığında, yedekleme geri ve DNS kayıtlarını repointed.
2. Bir pasif kopyayı başka bir Azure bölgesi (ek masraf) yedeklemeler kendi hizmetinin düzenli olarak geri yüklenir kendisine müşteriler devam eder. Bir DR olayı, hizmeti geri yüklemek için yalnızca DNS kayıtlarını repointed.

Logic Apps çok hızlı bir şekilde yeniden oluşturulabilir ve sunucusuz gibi bunların ilişkili Azure Resource Manager şablonunun bir kopyasını kaydederek yedeklenir. Bunlar kaydedilmiş bir müşterilerin sürekli tümleştirme/sürekli dağıtımı (CI/CD) işlemine denetim/tümleşik kaynağına.

API Management yayımlanan uygulamalar mantığı güncelleştirilmiş konumlarına gerekir farklı veri merkezine taşımanız gerekir. Bu API arka uç özelliğini güncelleştirmek için basit bir PowerShell Betiği gerçekleştirilebilir.

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler

Üretim, geliştirme, ayrı kaynak grupları oluşturmak ve test ortamları. Bu, dağıtımları yönetmeyi, test dağıtımlarını silmeyi ve erişim haklarını atamayı kolaylaştırır.
Kaynakları kaynak gruplarına atarken, aşağıdakileri göz önünde bulundurun:

- Yaşam döngüsü. Genel olarak, aynı yaşam döngüsüne sahip kaynakları aynı kaynak grubuna yerleştirin.
- Erişim. Kullanabileceğiniz [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) bir grup içindeki kaynaklara erişim ilkeleri uygulamak için (RBAC).
- Faturalandırma. Kaynak grubu için aktarılmış maliyetleri görüntüleyebilirsiniz.
- Fiyatlandırma katmanı için API Management – Geliştirici katmanı geliştirme ve test ortamları için kullanmanızı öneririz. Üretim öncesi için üretim ortamınızın çoğaltmasını dağıtma testleri çalıştırmak ve maliyeti en aza indirmek için sonra kapatılıyor öneririz.

Daha fazla bilgi için bkz: [kaynak grubu](../azure-resource-manager/resource-group-overview.md) genel bakış.

### <a name="deployment"></a>Dağıtım

Kullanmanızı öneririz [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md) Azure API Management ve Azure mantıksal uygulamaları dağıtmak için. Şablonları, PowerShell veya Azure komut satırı arabirimi (CLI) aracılığıyla dağıtımlarını otomatikleştirin kolaylaştırır.

Azure API Management ve tek tek tüm mantığı uygulamaları kendi ayrı Resource Manager şablonları koyma öneririz. Bu, kaynak denetim sistemleri depolama olanak tanır. Bu şablonlar birlikte veya ayrı ayrı bir sürekli tümleştirme/sürekli (CI/CD) dağıtım işleminin bir parçası olarak dağıtılabilir.

### <a name="versions"></a>Sürümler

Bir yapılandırma yaptığınız her zaman bir mantıksal uygulama değiştirin (veya bir güncelleştirme aracılığıyla bir Resource Manager şablonu dağıtma), bu sürümün bir kopyasını (çalıştırma geçmişini olan tüm sürümleri tutulacak) size kolaylık sağlamak için tutulur. Geçmiş değişiklikleri izlemek ve ayrıca mantıksal uygulama geçerli yapılandırmasını olması için bir sürüme yükseltmek için bu sürümleri kullanabilirsiniz; Bunu anlamına gelir yapılması, etkin bir mantıksal uygulama, örneğin geri alma.

API Management sahiptir (ancak ücretsiz) ayrı iki [sürüm oluşturma kavramları](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Bunlar tüketebilir API tercihi API tüketicileriniz sağlamak için kullanılan sürümleri (örneğin v1, v2 veya beta, üretim) kendi gereksinimlerine göre.
- API güvenle API'ye değişiklikleri yapın ve isteğe bağlı yorumlar olan kullanıcılara dağıtmak yöneticilerin verme düzeltmeler.

Dağıtım – bağlamında API Management düzeltmelerini güvenli bir şekilde değişiklik, değişiklik geçmişini korumak ve API tüketicileri bu değişikliklerin farkında olun için bir yol olarak düşünülmelidir. Bir düzeltme geliştirme ortamında oluşturulur ve Resource Manager şablonları kullanarak diğer ortamlar arasında dağıtılır.

Düzeltmeler bir API 'geçerli' yapılmadan önce sınamak için kullanılır ve kullanıcılar için erişilebilir hale adımında yük veya tümleştirme sınaması için bu düzenek kullanmanızı önermiyoruz – bunun yerine ayrı bir test veya üretim öncesi ortamlar kullanılmalıdır.

### <a name="configuration"></a>Yapılandırma

Parolalar, erişim tuşları veya bağlantı dizeleri kaynak denetimi için hiçbir zaman denetleme. Gerekli olurlarsa dağıtmak ve bu değerleri güvenliğini sağlamak için uygun yöntemi kullanın. 

Logic Apps içinde (Bu bağlantı formunda oluşturulamıyor) mantıksal uygulama içinde gerekecek hassas değerleri Azure anahtar kasası depolanabilir ve Resource Manager şablonu başvurulan. Her ortam için parametre dosyaları ile birlikte dağıtım şablonu parametrelerini kullanarak öneririz. Daha fazla yönergeler [parametreleri ve iş akışı içinde girişleri güvenliğini sağlama](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

API Yönetimi'nde, gizli adlı değerleri/özellikleri olarak adlandırılan nesneleri kullanılarak yönetilir. Bunları güvenli bir şekilde erişilebilir değerleri API yönetimi ilkelerini depolayabilir. Bkz: nasıl yapılır [gizli API Management'te yönetme](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Tanılama ve izleme

Her ikisi de [API Management](../api-management/api-management-howto-use-azure-monitor.md) ve [Logic Apps](logic-apps-monitor-your-logic-apps.md) aracılığıyla işletimsel izleme desteği [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure İzleyici varsayılan olarak etkindir ve her hizmet için yapılandırılan farklı ölçümleri temel bilgileri sağlayacaktır.

Ayrıca, daha fazla seçeneği vardır her hizmet için:

- Logic Apps günlükleri için gönderilebilir [günlük analizi](logic-apps-monitor-your-logic-apps-oms.md) daha derin çözümleme ve dashboarding.
- API Management geliştirme Ops izleme için yapılandırma Application Insights destekler.
- API Management'ı destekleyen [özel API analiz için Power BI çözüm şablonu](http://aka.ms/apimpbi). Bu çözüm şablonu iş kullanıcıları için Power BI'da kullanılabilir raporları ile birlikte, kendi özel analiz çözümü oluşturmasına olanak tanır.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Bu bölümde açıklandığı gibi mimarisinde dağıtılan bu makalede açıklanan Azure hizmetlerine belirli güvenlik konuları listelenmiştir. En iyi güvenlik uygulamalarının tam bir listesi değildir.

- Uygun düzeyde kullanıcıların erişimini sağlamak için rol tabanlı erişim denetimi (RBAC) kullanın.
- OAuth/açık IDConnect kullanarak API Management Genel API uç noktalarını güvenli hale getirin. Bir kimlik sağlayıcısı yapılandırma ve bir JWT doğrulama ilkesi ekleyerek bunu.
- Arka uç hizmetlerine API karşılıklı sertifikalar kullanılarak Yönetimi'nden bağlanın
- HTTP tetikleyicisi tabanlı Logic Apps, API Management için IP adresini işaret eden bir IP adresi beyaz listesi oluşturarak güvenliğini sağlayın. Bu API Management geçmeden genel internet'ten mantıksal uygulama çağırma önler.

Bu başvuru mimarisinin Azure tümleştirme hizmetlerini kullanarak basit Kurumsal tümleştirme platformu oluşturmak nasıl oluşturulacağını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sıralar ve olaylar ile Kurumsal tümleştirme](logic-apps-architectures-enterprise-integration-with-queues-events.md)
