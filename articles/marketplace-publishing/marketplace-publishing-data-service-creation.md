---
title: "Veri Hizmeti Market oluşturmaya Kılavuzu | Microsoft Docs"
description: "Oluşturma, sertifika ve bir veri hizmeti için dağıtma konusunda ayrıntılı yönergeler Azure Market satın alın."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: c0c9362f1c2e15c947aaaf7187f3383ad243140f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Veri Hizmeti Azure Market'e Kılavuzu yayımlama
> [!IMPORTANT]
> **Şu anda artık ekleme duyuyoruz herhangi yeni veri hizmeti yayımcılar. Yeni dataservices listesine onaylanmamış.** Üzerinde AppSource yayımlamak istediğiniz bir SaaS iş uygulaması varsa daha fazla bilgi bulabilirsiniz [burada](https://appsource.microsoft.com/partners). Bir Iaas uygulamalar varsa veya Azure marketi, yayımlamak istediğiniz Geliştirici hizmet, daha fazla bilgi bulabilirsiniz [burada](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Adım 1 ' tamamladıktan sonra [hesap oluşturma ve kayıt](marketplace-publishing-accounts-creation-registration.md), biz size kılavuzluk [genel teknik olmayan](marketplace-publishing-pre-requisites.md) ve [teknik gereksinimleri](marketplace-publishing-data-service-creation-prerequisites.md) veri hizmeti Azure Market sunar. Biz, üzerinde bir veri hizmeti teklifi oluşturmak için adımlarda size yol gösterecek artık [yayımlama portalında] [ link-pubportal] Azure Marketi için.

## <a name="1----login-to-the-publishing-portal"></a>1.    Yayımlama için oturum açma Portal.
Git [https://publish.windowsazure.com](https://publish.windowsazure.com.)

**Yayımlama portalında ilk oturum açma için şirketinizin satıcı profili Geliştirici Merkezi'nde kayıtlı olduğu aynı hesabı kullanın.**  (Daha sonra şirketinizin herhangi bir personel yayımlama portalında ortak yönetici olarak ekleyebilirsiniz).

Tıklayın **veri hizmetleri yayımlama** Bu yayımlama portalında ilk oturum ise döşeme.

## <a name="2----choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2.    Seçin **Veri Hizmetleri** sol taraftaki gezinti menüsünde.
  ![Çizim](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a>3.    Yeni bir veri hizmeti oluşturma
Başlık, yeni veri hizmeti sunmak için doldurun ve sağ taraftaki üzerinde "+"'i tıklatın.

  ![Çizim](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4.    Alt menü Gezinti menüsünde Yeni oluşturulan veri hizmeti altındaki gözden geçirin.
Tıklayın **izlenecek** sekmesinde ve Azure Market'te veri hizmeti düzgün şekilde yayımlamak için gerekli tüm gerekli adımları gözden geçirin.

> [!TIP]
> Her zaman "İzlenecek" sayfası bağlantıları tıklayın veya sol tarafındaki veri hizmeti teklif ait alt menüsünde sekmeleri kullanın.
> 
> 

## <a name="5----create-a-new-plan"></a>5.    Yeni bir Plan oluşturun.
### <a name="offers-plans-transactions"></a>, İşlemleri planları sunar.
Her teklif birden çok planına sahip olabilir, ancak en az birine (1) planı olması gerekir. Teklifiniz için son kullanıcıların abone olduğunuzda bunların teklif 's planı biri için abone olun. Her plan nasıl son kullanıcılar hizmetinizi kullanabilmek için tanımlar.

Şu anda Azure Marketi yalnızca aylık abonelik işlem tabanlı model verileri için Destek Hizmetleri, yani son kullanıcıların abone oldukları belirli plan fiyatı göre aylık ücret ödemeniz ve işlem her ay sayısı kullanamayabilir olacaktır plan tarafından tanımlanır.

Genelde, veri hizmeti döndürülecek kayıt sayısı tanımlanan her bir işlem hizmete gönderilen bağlı. Varsayılan değer 100'dür. Her sorgu döndürülen işlem sayısı sayı olacaktır 100 ve kadar en yakın tamsayıya yuvarlanan bölü kayıtların.

Bu her sorgu tarafından tüketilen işlem sayısı (ölçüm) izlemek için Azure Market hizmet katmanı sorumluluğundadır.

> [!IMPORTANT]
> Ay sırasında işlem sınırına son kullanıcılar kendi aylık abonelik döngüsünün sonuna kadar hizmetini kullanmaya devam etmesini engellenir.
> 
> Plan veya planları biri işlemleri sınırsız sayıda olabilir ama değil gerekir içerir.
> 
> 

### <a name="create-a-plan"></a>Bir plan oluşturun.
1. Tıklayın **"+"** "Yeni bir plan Ekle" yanındaki.
2. Seçeneklerden birini seçin: **sınırsız** veya **sınırlı** Bu plan için kullanım.  İşlem sayısı sınırlı sonra sağlarsanız, plan bir ay içinde kullanılmasına izin verir.
   
    ![Çizim](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    Portal yayımlama "Planı son kullanıcılara kullanıcı arabiriminde planının adı iletişim kurmak için kullanılan ve ayrıca belirlemek için markette hizmeti tarafından kullanılan tanımlayıcı" önerir. İsterseniz, "Plan tanımlayıcısı" değiştirebilirsiniz.
   
   > [!NOTE]
   > "Plan tanımlayıcısı" her teklif kapsamında benzersiz olmalıdır. Diğer birçok tanımlayıcıları yayımlama Portal planlama tanımlayıcıda kullanılan ilk yayımlama üretim ve bu tanımlayıcı değiştirmesi mümkün olmayacaktır sonra kilitlenir.
   > 
   > 
3. Tercih ettiğiniz kabul etmek için tıklatın.
4. Ardından, birkaç ek soruları yeni oluşturulan planınız istenir.
   
    ![Çizim](media/marketplace-publishing-data-service-creation/step-5.2.png)

| Soru | Anlamlı |
| --- | --- |
| **Bu Plan hazır ve kullanılabilir dünya çapında mi?** |Tamamen ücretsiz-in-ücret planı oluşturabilirsiniz. Bu teklif – yalnızca planlama ise "Ücretsiz teklif" markette yayımladığınız olduğunu anlamına gelir. Yalnızca biri (az) için ise planı, BT size hizmetiniz görece küçük bir aylık işlem sayısı ile ilgili daha fazla bilgi için son kullanıcılara sunmak için bir seçenek.  Yanıt "Evet" ise, daha fazla soru istenir. |

> [!NOTE]
> Son kullanıcıların her zaman Ücretli planlarına yükseltebilirsiniz.
> 
> 

| Soru | Anlamlı |
| --- | --- |
| **Ücretsiz deneme sürümü var mı?** |"Hayır deneme" arasında hiç seçin veya planınız "Bir ay" için kullanılacak bir seçenek sağlar. Son kullanıcılar için ücretsiz bir ay için önerinin avantajlarını öğrenmeniz olasılığını sağlamak için bu seçeneği kullanmak yayımcılar gibi. |

> [!IMPORTANT]
> Son kullanıcılar yalnızca ödeme yöntemini kredi kartı ör, Kurumsal Anlaşma kurduysanız ücretsiz deneme satın açabilirler.
> 
> Müşteri aboneliği iptal başlatılan sürece ücretsiz deneme sürümü, bir ay sonra abonelik tarih itibariyle fiyat müşteriler şarj Azure Marketi başlar. Özel bildirim son kullanıcılara sağlanır.
> 
> 

| Soru | Anlamlı |
| --- | --- |
| **Bu planı satın almak için bir promosyon kodu gerektiriyor?** |Yayımcılar belirli müşterilere "A Promocode" adlı bir özel kod sağlayarak kendi hizmet planları erişimi sınırlamak için bir seçeneğiniz vardır. Bu Promocode sahip son kullanıcılar plana abone kuramaz. "Hayır" ı seçin, sonra kabul durumunda herkes teklif olduğu kullanılabilir bölgesinden (bkz [Market pazarlama içerik Kılavuzu](marketplace-publishing-push-to-staging.md) daha fazla ayrıntı için) bu plana abone mümkün olacaktır. Daha fazla soru istenir. |
| **Ayrıca geçerli promosyon kodu yok herkes bu plandan Gizle?** |Önceki sorusunun yanıtını "Evet" ise yayımcının Market Arabiriminde görünen bu planı tamamen kaldırmak için bir seçenek vardır. Bu anlamına gelir, müşteriler bu teklif'in ayrıntılar sayfası planında görmez. Satın almak için bir promocode alacak son kullanıcılar erişebilir bu promocode kullanarak abone olun. |

## <a name="6----create-your-marketplace-marketing-content"></a>6.    İçerik pazarlama, Market oluşturma
Gerekli bilgileri sağlamak nasıl **pazarlama, fiyatlandırma, destek ve kategorileri** sekmeleri ziyaret edin [Market pazarlama içerik Kılavuzu](marketplace-publishing-push-to-staging.md) Azure yayımlanan tüm yapıları için ortak olan Market.  

## <a name="7----connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7.    Teklifiniz hizmetinize (SQL tabanlı Azure veya Web tabanlı hizmet) bağlayın.
Tıklayın **Veri Hizmetleri** alt menüsünde.

Sayfanın üst kısmında teklif 's sağlamak için istenir, **Namespace**.  

  ![Çizim](media/marketplace-publishing-data-service-creation/step-7.png)

Soru yayımcı Azure Marketi'nde yeni oluşturulan sunmaya teklif ne olacağını tanımlar. (Daha fazla bilgi için bkz [Veri Hizmetleri Teknik önkoşul Kılavuzu](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Çizim](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Temel veritabanı hizmeti yayımlama**

Tıklayın **veritabanı**. Aşağıdaki sayfası görünür:

  ![Çizim](media/marketplace-publishing-data-service-creation/step-7.3.png)

SQL Azure DB'de tabanlı veri kümesi için bir CSDL eşlemesi oluşturmak için:

  ![Çizim](media/marketplace-publishing-data-service-creation/step-7.4.png)

Ve ardından her tablo için

  ![Çizim](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Çizim](media/marketplace-publishing-data-service-creation/step-7.6.png)

Web hizmeti

  ![Çizim](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> Okuma [var olan eşleme web hizmeti CSDL aracılığıyla OData](marketplace-publishing-data-service-creation-odata-mapping.md) ayrıntılı yönergeler ve örnekler CSDL Web hizmeti oluşturmak için.
> 
> 

## <a name="next-steps"></a>Sonraki Adımlar
Veri Hizmeti teklifiniz oluşturduğunuza göre ' ndaki yönergeleri tamamlandığından emin olmak [Market pazarlama içerik Kılavuzu](marketplace-publishing-push-to-staging.md) İleri geçmeden önce [hazırlama,verihizmetisınama](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Ayrıca Bkz.
* [Başlarken: nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)
* Genel OData eşleme işlemi ve amacı anlamak ilgileniyorsanız, bu makaleyi okuyun [veri hizmeti OData eşleme](marketplace-publishing-data-service-creation-odata-mapping.md) tanımları, yapılar ve yönergeleri gözden geçirmek için.
* Öğrenme ve belirli düğümler ve bunların parametrelerini anlama ilgileniyorsanız, bu makaleyi okuyun [veri hizmeti OData eşleme düğümleri](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) tanımları ve açıklamalar, örnekler ve kullanım örneği bağlamı.
* Örnekler gözden geçirme ilgileniyorsanız, bu makaleyi okuyun [veri hizmeti OData eşleme örnekler](marketplace-publishing-data-service-creation-odata-mapping-examples.md) örnek kodu görmek ve kod sözdizimi ve bağlamı anlamak için.

[link-pubportal]:https://publish.windowsazure.com
