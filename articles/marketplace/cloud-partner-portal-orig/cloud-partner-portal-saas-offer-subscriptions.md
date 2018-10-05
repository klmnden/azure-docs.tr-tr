---
title: SaaS - Azure üzerinden satın | Microsoft Docs
description: Abonelik faturalama modeli SaaS uygulamaları ve nasıl açıklar.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: f193138fbc5e779c3a6671757320d8918e08db46
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811644"
---
<a name="saas---sell-through-azure"></a>SaaS - Azure üzerinden satış yapın
=========================

Bu makalede, SaaS uygulamaları ve bulut iş ortağı Portalı'nda nasıl abonelik fatura modelini açıklar.


<a name="overview"></a>Genel Bakış
--------

Azure marketi, SaaS tekliflerini Azure Marketi üzerinden gelir elde edin ve yayımlama sağlar. Müşterileriniz için bunlar artık üzerinden ücretlendirilen Azure faturalama doğrudan, ve yalnızca bir fatura Azure'dan tüm kaynakları ve Hizmetleri etkin için alırlar. Yayımlama için SaaS abonelikleri aşağıdaki avantajlara sahiptir:

-   **Tutarlı yayımlama deneyimi** --zaten SaaS teklifleri yayımlama veya aynı yayımlama portalı başka bir teklife Azure Market'te yayımlama deneyimi.
-   **Bulma için yeni mağaza** --yayımlanan tüm teklifleri hem de dış Azure Marketi vitrini yanı sıra Azure portalı içinde Azure Market uzantısı görünür olur.
-   **Tümleşik faturalandırma** -artık, burada Azure satışını yaptığı tüm bölgelerde satabilecekleri ve Azure ile faturalandırma sizin için halleder.
-   **Müşteri adayı oluşturma tümleşik** --SaaS hizmetinizin Azure marketini kullanarak bir Azure kullanıcısı abone olduğunda, artık otomatik olarak müşteri adayları CRM tercih ettiğiniz azure'dan alabilir.
-   **Microsoft satıcılarıyla ortak satış** --için çift yönlü potansiyel müşteri paylaşımı, öncelikli katalog listelemelerine kazanabilir ve yan yana Microsoft satıcılarıyla etkileşim kurun ve bunlarda kapanış fırsatı ilgilenir.

<a name="billing-models"></a>Faturalama modelleri
--------------

Yalnızca şu anda desteklenen faturalandırma modeli, SaaS hizmetinizin abonelik başına aylık sabit ücrettir.

**Not:** ek faturalandırma modelleri kullanılabilir gelecek bir zamanda.

Her SaaS teklifi (örneğin, Basic veya Premium) bir veya daha fazla plan olabilir. Her planda planıyla ilişkili fiyat birlikte onunla ilişkili bazı temel meta verileri yok.

Bir plan tanımı üzerinde tam denetime sahiptir. Örneğin, hangi temel bir plan oluşturur? Plan, tarafından tanımlanır ve yayımlama planınızın bir parçası olarak tanımlamak için gerekli metni sağlayabilir.

Fiyat planı ile ilişkili Azure kullanıcıları, hizmeti kullanmak için ödeme yaparsınız aylık sabit ücrettir.

### <a name="what-is-not-supported-today"></a>Ne şu anda desteklenmediğini?

-   Örneğin, istek sayısına göre fiyat karşılığında özel birimlerde--üzerinden faturalandırılır.
-   --Lisans tahsisine dayalı faturalama gibi kullanıcı sayısına göre lisans satın almak, Azure kullanıcılarının izin verin.

<a name="end-to-end-flow"></a>Uçtan uca akış
---------------

SaaS abonelik akış öncelikle bir son kullanıcı açısından sunar

**Not:** Bu akış açıklaması, 'Contoso tanıtım SaaS' adlı Azure Market'te yayımlanan SaaS teklifinizi varsayar.

![SaaS abonelik kullanıcı akışı](media/saas-offer-subscription/saas-subscription-user-flow.png)

Bir Azure kullanıcısı ile SaaS hizmetinizin etkileşimleri aşağıdaki sahip olabilir:

-   Azure marketi, SaaS hizmetinizin keşfedin
-   SaaS hizmetinizin azure'da abone olun
-   Azure portalından SaaS hizmetine gidin
-   SaaS hizmetinde bir hesap oluşturun ve SaaS hesabında (değişiklik planı/silme) yönetme
-   Azure Portalı'ndan SaaS hizmetine aboneliği iptal et

<a name="discover-your-saas-service-in-azure-marketplace"></a>Azure marketi, SaaS hizmetinizin keşfedin
-----------------------------------------------

Kullanıcılar, Azure marketi, Azure portalında başlattığında, 'Yazılım (SaaS) hizmet olarak' adında bir kategoriyi görürler.

![Kategori menüsü kullanılarak SaaS keşfedin](media/saas-offer-subscription/saas-category-menu.png)

Kullanıcıların, SaaS hizmeti için arama da yapabilirsiniz.

![Arama'yı kullanarak SaaS keşfedin](media/saas-offer-subscription/saas-cpp-search.png)

Kullanıcılar daha sonra hizmetinizin ayrıntılarını görüntülemek ve ardından **Oluştur**.

![Bir SaaS aboneliği oluşturuluyor](media/saas-offer-subscription/saas-create-subscription.png)

### <a name="subscribe-to-saas-service-in-azure"></a>Azure'da SaaS hizmetine abone olun

Kullanıcı daha sonra Azure'dan SaaS hizmetine abone olabilirsiniz.

![Bir SaaS hizmetine abone olma](media/saas-offer-subscription/saas-subscribe-signup.png)

SaaS hizmetinizin bir kullanıcı abone olduğunda, kullanıcı, aşağıdaki bilgileri sağlar.

-   --Kullanıcılar bulmak veya bu SaaS abonelik örneğini azure'da yönetme adı.
-   Abonelik--SaaS hizmeti için fatura bilgilerini ilişkilendirmek istediğiniz aboneliği bağlamı.
-   Plan--abone olmak istediğiniz SaaS hizmeti planı.

Kullanıcı SaaS hizmetine abone olmadan önce teklifini yayımlamanın bir parçası olarak sunulan kullanım koşulları belgesi da kullanıcıya gösterilir.

Kullanıcı artık hizmete tıklayarak abone olabilirsiniz **abone ol**.
Abonelik tamamlandıktan sonra azure portalında bir bildirim gönderir.

### <a name="navigate-to-the-saas-service-from-azure-portal"></a>Azure Portalı'ndan SaaS hizmetine gidin

Kullanıcılar ardından **kaynağa Git** kendi SaaS abonelik örneği yönetmek veya görüntülemek için.

![SaaS örneğinizin yönetmek veya görüntülemek](media/saas-offer-subscription/saas-go-to-resource.png)

Bunlar hesap SaaS hizmeti ilk yapılandırmanız gerektiğini kullanıcı bilgilendirilir. Kullanıcı SaaS Hizmeti sitesinde bir hesap oluşturduğunda, faturalandırma başlatmak için Azure SaaS hizmeti bildirir sonra bunların faturalandırma ettiğiniz günde başlatılır.

![SaaS örneğinizin yapılandırın](media/saas-offer-subscription/saas-configure-account.png)

Kullanıcı tıkladığında üzerinde **yapılandırma hesabı**, SaaS hizmet uç noktanıza yeniden yönlendirilir. Bu yeniden yönlendirme sırasında bir belirteç boyunca bir sorgu parametresi olarak geçirilir. Örneğin:

![SaaS belirteci hesap](media/saas-offer-subscription/saas-account-token.png)

Bu belirteç değerini not edin. Bu belirteç kısa süreli bir ve abonelik tanımlayıcısı Azure'da almak için çözülmesi gerekiyor.

<a name="creating-a-saas-offer-subscription"></a>Bir SaaS teklif aboneliği oluşturuluyor
----------------------------------

Bu deneyim oluşturmak için iki parçalı gerekli olan bir işin vardır:

-   Microsoft ile SaaS hizmeti sitenize bağlanmak\'SaaS API'lerine s. Bu belge [Azure - API'leri ile SaaS satış](./cloud-partner-portal-saas-subscription-apis.md) bu bağlantıyı oluşturmak nasıl açıklar.  
-   Etkinleştirme **Azure üzerinden satın** bulut iş ortağı Portalı'nda üzerinde **teknik bilgi** bölümü ve tüm yapılandırma bilgilerinizi sağlayın.

![Yeni SaaS teknik bilgi sağlar.](media/cpp-creating-saas-offers/saas-new-offer-technical-info-2.png)

Tüm gerekli alanlar için bir açıklama aşağıdadır **teknik bilgi** bölümü:

|  **Teklif alanları**                 |  **Açıklama**                                   |
|  ----------------                 |  -------------------------------------             |
| Önizleme abonelik kimlikleri          | Genel olarak kullanılabilir hale gelmeden önce önizleme aşamasında teklifinizi sınamak için kullanılan tüm Azure abonelik tanımlayıcıları içerir. |
| Başlarken yönergeleri      | SaaS uygulamanızı bağlama yardımcı olmak için müşterilerinizle paylaşmak için yönergeleri izleyin. Temel HTML kullanım verilir, bu etiketleri farklı `<p>`, `<h1>`, `<li>`vb.  |
| Giriş sayfası URL'si                  | Müşterilerinizin aldıktan sonra Azure Portalı'ndan gelen yönlendirerek, site URL'si. Bu URL, aynı zamanda Microsoft ticaret kolaylaştırmak için API bağlantı alma uç noktası olacaktır.  |
| Bağlantı Web kancası                | Microsoft, müşteri adına göndermek için gereken tüm zaman uyumsuz olaylar için (örnek: Azure aboneliği geçersiz geçmiş), bağlantı Web kancası sağlayın isteriz. Yerinde bir Web kancası sistemine sahip değilseniz, en basit yapılandırmadır, kendisine gönderilmesini meydana gelen olayları dinler ve ardından uygun şekilde işlemesine bir HTTP uç noktası mantıksal uygulama sağlamaktır.  Daha fazla bilgi için [çağrı, tetikleyici veya iç içe iş akışları ile HTTP uç noktalarını logic apps'teki](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint). |
| Azure AD Kiracı kimliği ve uygulama kimliği     | Azure portalı içinde bizim ikisi arasında bağlantı doğrulayabilmemiz için bir Active Directory uygulaması oluşturma zorunlu kılarız bir kimliği doğrulanmış iletişim hizmetleridir. Bu alanlar için bir AD uygulaması oluşturma ve karşılık gelen Kiracı kimliğini yapıştırın ve uygulama kimliği gereklidir. |
|  |  |


Son olarak, seçerseniz **Azure üzerinden satın**, adlı ek bir bölümü **planları**. Planları, yalnızca Azure üzerinden satış seçili değilse gereklidir. Bu bölümde belirli planları ve SaaS uygulamanızın desteklediği karşılık gelen fiyatları listelenir. Bugünden itibaren aylık, 1 - veya 3 ay için ücretsiz erişim izin vermek için bir oluşturabilmesini fiyatlandırma için izin verin. Bu planlarını ve fiyatlarını tam planları ve kendi SaaS uygulama sitede sahip fiyatları eşleşmesi gerekir.
