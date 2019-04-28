---
title: Azure SaaS uygulama teklif teknik yapılandırması | Microsoft Docs
description: Azure Marketi'nde SaaS uygulaması teklif için teknik bilgileri yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 891d9b7b34e3d30efb46b69ef1aa75566fe634c4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60594319"
---
# <a name="saas-application-technical-info-tab"></a>SaaS uygulaması teknik bilgileri sekmesi

Teknik bilgi sekmesi teknik yapılandırma formu sağlar. Oluşturmakta olduğunuz SaaS uygulama (uygulama) türünü seçin ve müşterilerin uygulamanızı nasıl sağlandığını yapılandırmak için bu formu kullanın.

![Teknik yapılandırma formu](./media/saas-techinfo-techconfig.png)

## <a name="technical-configuration-form"></a>Teknik yapılandırma formu

Bu formu 2 alan vardır: Ürün ve eylem çağrısı.

### <a name="product-field"></a>Ürün alanı

Bir SaaS uygulaması hem de aşağıdaki vitrinler sağlayabilirsiniz:
- Seçerek bir iş kullanıcısı için **listeleme** seçeneği.
- Seçerek bir BT yöneticisi kullanıcının **Microsoft satış**.
SaaS uygulama türünü hedeflediğiniz platformun karar vermenize yardımcı olmak için okuma [vitrin seçimi anlamak](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type#understand-storefront-selection).

#### <a name="sell-through-microsoft"></a>Microsoft satış yapın
Bu deneyim oluşturmak için şu bilgilere yapılandırmanız gerekir:

- SaaS hizmeti siteniz ile Microsoft'un SaaS API'lerine bağlanın. [Azure – API'leri ile SaaS satış](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-subscription-apis) makalede bu bağlantının nasıl oluşturulacağını açıklar.
- Azure üzerinde bulut iş ortağı Portalı aracılığıyla satış teknik yapılandırma formunda etkinleştirin ve gerekli bilgileri sağlayın. Bu faturalama modeli ve nasıl uygulandığı hakkında daha fazla bilgi için bkz. [SaaS – Azure üzerinden satın](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-offer-subscriptions).

  ![Microsoft formu satış yapın](./media/saas-techinfo-sellthrough-ms.png)

Aşağıdaki tabloda, Microsoft aracılığıyla satış için gerekli alanları açıklar.

|  **Alan adı**   |  **Açıklama**  |
|  ---------------  |  ---------------  |
|    Önizleme abonelik kimlikleri               |    Genel olarak kullanılabilir hale gelmeden önce önizleme aşamasında teklifinizi sınamak için kullanılan tüm Azure abonelik tanımlayıcıları içerir.               |
|     Başlarken yönergeleri              |   SaaS uygulamanızı bağlama yardımcı olmak için müşterilerinizle paylaşmak için yönergeleri izleyin. Temel HTML etiketleri izin verilir, örneğin: &lt;p&gt;, &lt;h1&gt;, &lt;li&gt;vb.                |
|    Giriş sayfası URL'si  |   Müşterilerinizin aldıktan sonra Azure Portalı'ndan gelen yönlendirerek, site URL'si. Bu URL, aynı zamanda Microsoft ticaret kolaylaştırmak için API bağlantı alma uç noktası olacaktır.                |
|  Bağlantı Web kancası    |  Microsoft, müşteri adına göndermek için gereken tüm zaman uyumsuz olaylar için (örnek: Azure aboneliği geçmiş geçersiz), bağlantı Web kancası sağlayın isteriz. Yerinde bir Web kancası sistemine sahip değilseniz, en basit yapılandırmadır, kendisine gönderilmesini meydana gelen olayları dinler ve ardından uygun şekilde işlemesine bir HTTP uç noktası mantıksal uygulama sağlamaktır. Daha fazla bilgi için <a href="https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint">çağrı, tetikleyici veya iç içe iş akışları ile HTTP uç noktalarını logic apps'teki</a>                |
|  Azure AD Kiracı kimliği ve uygulama kimliği      |   Azure portalı içinde bizim ikisi arasında bağlantı doğrulayabilmemiz için bir Active Directory uygulaması oluşturma zorunlu kılarız bir kimliği doğrulanmış iletişim hizmetleridir. Bu alanlar için bir AD uygulaması oluşturma ve karşılık gelen Kiracı kimliğini yapıştırın ve uygulama kimliği gereklidir. Uygulama kimliği, Publisherıd için ilişkili olduğunu unutmayın. Bu nedenle, tüm teklifleri olduğu gibi emin aynı uygulama kimliği olun.             |


Son olarak, seçerseniz **Microsoft satış**, adlı başka bir yeni teklif sekmesi **planları**. 

[Planlar sekmesine](./cpp-plans-tab.md) belirli planları ve SaaS uygulamanızın desteklediği karşılık gelen fiyatları listeler. Bugünden itibaren aylık, 1 - veya 3 ay için ücretsiz erişim izin vermek için bir oluşturabilmesini fiyatlandırma için izin verin. Bu planlarını ve fiyatlarını tam planları ve kendi SaaS uygulama sitede sahip fiyatları eşleşmesi gerekir.

>[!NOTE] 
>Planları, Microsoft aracılığıyla satış seçerseniz yalnızca gereklidir.

### <a name="call-to-action-field"></a>Eylem alanı arama

Eylem alanı çağrısı ürününüzün edinme düğme üzerinde görünen ileti seçmenize olanak sağlar. Aşağıdaki seçenekler kullanılabilir:

- Ücretsiz – bu seçeneği seçerseniz, deneme müşteriler, SaaS uygulamasına erişimi edinebileceğiniz bir URL girmeniz istenir. Örneğin, https://contoso.com/trial
- Ücretsiz deneme – ise, bu seçenek, istenir deneme müşteriler, SaaS uygulamasına erişimi edinebileceğiniz bir URL girmeniz seçin. Örneğin, https://contoso.com/trial
- Benimle iletişim kurun

Yayımlama seçeneği çağrı eylem seçenekleri hakkında daha fazla bilgi için bkz.

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](../../cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı.

## <a name="next-steps"></a>Sonraki adımlar

- [Planlar sekmesine (isteğe bağlı)](./cpp-plans-tab.md)
- [Kanal Bilgileri sekmesi](./cpp-channel-info-tab.md)
