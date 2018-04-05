---
title: Power BI çalışma alanı koleksiyonu içerik Power BI Embedded geçirme | Microsoft Docs
description: Power BI çalışma koleksiyonlarından Power BI Embedded geçirmeyi öğrenme ve uygulamalardaki katıştırma Dengeleme ilerletir.
services: power-bi-embedded
documentationcenter: ''
author: guyinacube
manager: erikre
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 069f31c8213bd0d8586f7ca50e543acfdad8a2b3
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-migrate-power-bi-workspace-collection-content-to-power-bi-embedded"></a>Power BI çalışma alanı koleksiyonu içerik Power BI Embedded geçirme

Power BI çalışma koleksiyonlarından Power BI Embedded geçirmek öğrenin. Bu makalede Azure Power BI çalışma koleksiyonlarından Power BI Embedded geçirilmesine yönelik rehberlik sağlar. Biz de için uygulama değişiklikleri beklenmesi gerekenler bakın.

Power BI çalışma koleksiyonları kaynak, Power BI Premium genel kullanılabilirliğini yayımlandıktan sonra sınırlı bir süre için kullanılabilir olmaya devam eder. Bir kurumsal anlaşma altında müşteriler kendi varolan anlaşmaları sona erme tarihi kendi mevcut çalışma koleksiyonlarına erişimi. Power BI çalışma koleksiyonlar doğrudan ya da CSP kanalları edinilen müşteriler Premium'a genel kullanılabilirlik, Power BI bir yıl için erişim keyfini çıkarın.

> [!IMPORTANT]
> Geçiş bir bağımlılık Power BI hizmetini alır, ancak yok. bir bağımlılık Power BI, uygulamanızın kullanıcılar için kullanılırken bir **belirteci katıştırmak**. Power BI'ın katıştırılmış içerik uygulamanızda görüntülemek için kaydolmak gerekmez. Bu yaklaşım katıştırma, müşterileriniz için Power BI ekleme.

![Power BI Embedded akışı](media/migrate-from-power-bi-workspace-collections/powerbi-embed-flow.png)

## <a name="prepare-for-the-migration"></a>Geçiş için hazırlama

Power BI çalışma koleksiyonları hizmetinden üzerinden Power BI Embedded geçirmek için hazırlanmak için gerçekleştirmeniz gereken birkaç nokta vardır. Power BI Pro lisansına sahip bir kullanıcı ile birlikte kullanılabilen bir kiracı gerekir.

1. Bir Azure Active Directory (Azure AD) Kiracı erişimi olduğundan emin olun.

    Kullanmak için Kiracı?

    * Mevcut Kurumsal Power BI kiracınızın kullanılsın mı?
    * Ayrı bir kiracı, uygulamanız için kullanılsın mı?
    * Ayrı bir kiracı her müşteri için kullanılsın mı?

    Uygulamanızı ya da her müşteri için yeni bir kiracı oluşturmaya karar verirseniz, aşağıdakilerden birini bakın:

    * [Azure Active Directory kiracısı oluşturma](https://powerbi.microsoft.com/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)
    * [Azure Active Directory kiracısı alma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant).

2. Uygulama "ana" hesabı olarak işlem görür bu yeni Kiracı içinde bir kullanıcı oluşturun. Hesap Power BI'a kaydolmanız gerekir ve kendisine atanmış bir Power BI Pro lisansına sahip olması gerektiğini.

## <a name="accounts-within-azure-ad"></a>Azure AD içinde hesapları

Aşağıdaki hesapları Kiracı içinde bulunması gerekir.

> [!NOTE]
> Bu hesaplar, uygulama çalışma alanları kullanmak için Power BI Pro lisansına sahip olmanız gerekir.

1. Bir kiracı yönetici kullanıcı.

    Katıştırma uygulama çalışma alanının bir üyesi olarak listelenen Kiracı yönetici olduğuna önerilir.

2. İçerik oluşturma analistleri hesapları.

    Bu kullanıcılar, gerektiğinde uygulama çalışma alanlarına atanmalıdır.

3. Bir uygulama *ana* kullanıcı hesabı ya da hizmet hesabı.

    Uygulamaları arka uç bu hesabın kimlik bilgilerini depolar. Kullanmak *ana* hesabınızı Power BI REST API'leri ile kullanmak için bir Azure AD belirtecini alma için. Bu hesap, uygulama için ekleme belirteci üretmek için kullanılır. *Ana* hesabı katıştırmak için oluşturulan uygulama çalışma alanlarının bir yönetici olması gerekir.

    **Yalnızca normal bir kullanıcı hesabı kuruluşunuzdaki katıştırma amacıyla kullanılan hesaptır.**

## <a name="app-registration-and-permissions"></a>Uygulama kaydı ve izinleri

REST API çağrıları yapmak için bir uygulamayı Azure AD ile kaydedin. Ek yapılandırma Power BI uygulaması kayıt sayfası yanı sıra Microsoft Azure portalındaki uygulanır. Daha fazla bilgi için bkz: [Power BI içeriğini katıştırmak için Azure AD uygulama kaydetmeyi](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

Uygulama kullanarak uygulamayı kaydetmeniz önerilir **ana** hesabı.

## <a name="create-app-workspaces-required"></a>(Gerekli) uygulama çalışma alanları oluşturma

Uygulamanız birden çok müşteri bakım, daha iyi yalıtımı sağlamak için uygulama çalışma alanlarının yararlanabilir. Panolar ve raporlar müşterileriniz yalıtılmış olacaktır. Daha sonra Power BI hesabınız uygulama çalışma daha fazla uygulama deneyimleri müşterileriniz yalıtmak için kullanabilirsiniz, ancak basit tutmak için yalnızca bir hesap kullanabilirsiniz.

> [!IMPORTANT]
> Kişisel bir çalışma alanı kullanamaz (bir "Çalışma Alanım"), müşterilerinize katıştırma avantajlarından yararlanmak için.

Power BI içindeki bir uygulama çalışma alanı oluşturmak için bir Pro lisansa sahip bir kullanıcı gerekir. Uygulama çalışma alanı oluşturur Power BI kullanıcı varsayılan olarak, çalışma alanının bir yöneticidir. **Uygulama *ana* hesap çalışma alanının yönetici olması gerekir.**

## <a name="content-migration"></a>İçerik Geçişi

İçeriğinizi çalışma koleksiyonlarından Power BI Embedded geçirme geçerli çözümünüz için paralel yapılabilir ve kapalı kalma süresi gerektirmez.

A **geçiş aracı** içeriği Power BI çalışma koleksiyonlarından Power BI Embedded kopyalama ile yardımcı olmak için kullanmanız için kullanılabilir. Özellikle birçok rapor varsa. Daha fazla bilgi için bkz: [Power BI Embedded geçiş aracı](migrate-tool.md).

İçerik Geçişi çoğunlukla iki API'leri kullanır.

1. İndirme PBIX - bu API için Power BI Ekim 2016 sonrasında yüklenen PBIX dosyalarını indirebilirsiniz.
2. İçeri aktarma PBIX - bu API için Power BI herhangi PBIX yükler.

Kod parçacıkları ilgili bazı için bkz: [kod geçirmek için Power BI Embedded içerik parçacıkları](migrate-code-snippets.md).

### <a name="report-types"></a>Rapor türleri

Rapor, her farklı geçiş akış gerektiren çeşitli türleri vardır.

#### <a name="cached-dataset-and-report"></a>Önbelleğe alınmış veri kümesini ve raporu

Önbelleğe alınan veri kümelerini bir canlı veya DirectQuery bağlantısı aksine veriler içe PBIX dosyaları bakın.

**Akış**

1. Karşıdan PBIX API Power BI çalışma alanı koleksiyonu alanınızdan çağırın.
2. PBIX kaydedin.
3. İçeri aktarma PBIX için Power BI Embedded çalışma alanınızda çağırın.

#### <a name="directquery-dataset-and-report"></a>DirectQuery veri kümesini ve raporu

**Akış**

1. GET çağrı https://api.powerbi.com/v1.0/collections/{collection_id}/workspaces/{wid}/datasets/{dataset_id}/Default.GetBoundGatewayDataSources ve alınan bağlantı dizesini kaydedin.
2. Karşıdan PBIX API Power BI çalışma alanı koleksiyonu alanınızdan çağırın.
3. PBIX kaydedin.
4. İçeri aktarma PBIX için Power BI Embedded çalışma alanınızda çağırın.
5. Bağlantı dizesi tarafından arama - POST güncelleştir  https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.SetAllConnections
6. Çağırarak GW kimliği ve veri kaynağı kimliği get - Al https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.GetBoundGatewayDataSources
7. Kullanıcı kimlik bilgilerini çağırarak güncelleştirmesi - düzeltme eki https://api.powerbi.com/v1.0/myorg/gateways/{gateway_id}/datasources/{datasource_id}

#### <a name="old-dataset-and-reports"></a>Eski veri kümesinin ve raporlar

Ekim 2016 desteklemez önce karşıdan PBIX özelliği karşıya raporlar.

**Akış**

1. PBIX geliştirme ortamı (iç kaynak denetimi) alın.
2. İçeri aktarma PBIX için Power BI Embedded çalışma alanınızda çağırın.

#### <a name="push-dataset-and-report"></a>Veri kümesini ve raporu gönderme

Karşıdan PBIX desteklemiyor *anında API* veri kümeleri. Veri bağlantı noktalı API dataset Power BI çalışma koleksiyonlarından Power BI Embedded iletin.

**Akış**

1. "Veri kümesi oluşturma" API'si veri kümesi için Power BI Embedded çalışma alanınızı oluşturmak için Json veri kümesi ile çağırın.
2. Rapor için oluşturulan veri kümesi * yeniden oluşturun.

Anında iletme geçirmek için bazı geçici çözümler kullanarak mümkündür Power BI Embedded için Power BI çalışma koleksiyonlardan API rapor aşağıdaki deneyerek:

1. Bazı kukla PBIX Power BI çalışma alanı koleksiyonu çalışma alanınızı karşıya yükleniyor.
2. Anında iletme kopyalama API rapor ve adım 1'den kukla PBIX bağlayın.
3. Anında iletme kukla PBIX API raporla indirin.
4. Sahte PBIX Power BI Embedded alanınıza karşıya yükleyin.
5. Power BI Embedded çalışma alanınızda anında veri kümesi oluşturun.
6. API dataset göndermek için raporu yeniden bağlayın.

## <a name="create-and-upload-new-reports"></a>Oluşturun ve yeni raporları karşıya yükleme

Power BI çalışma koleksiyonlarından geçirilen içeriğin yanı sıra, raporlarınızı ve Power BI Desktop kullanarak veri kümeleri oluşturma ve ardından bu raporları bir uygulama çalışma alanına yayımlayın. Raporları Yayımlama son kullanıcı bir uygulama çalışma alanı'na yayımlamak için Power BI Pro bir lisansı olması gerekir.

## <a name="rebuild-your-application"></a>Uygulamanızı yeniden

1. Uygulamanızı Power BI REST API'leri ve powerbi.com içinde rapor konumu kullanacak şekilde değiştirin.

2. AuthN/AuthZ kimlik doğrulaması kullanarak yeniden *ana* hesap uygulamanız için. Kullanmanın yararlanabilir bir [belirteci katıştırmak](https://msdn.microsoft.com/library/mt784614.aspx) bu kullanıcının diğer kullanıcılar adına hareket izin vermek için.

3. Power BI Embedded gelen raporlarınızı uygulamanıza ekleyin. Daha fazla bilgi için bkz: [, Power BI panoları, raporları ve döşeme katıştırma](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

## <a name="map-your-users-to-a-power-bi-user"></a>Kullanıcılarınızın Power BI kullanıcısıyla

Uygulamanızdaki, uygulama içinde yönetmek kullanıcıları eşlemek bir *ana* uygulamanızı amaçları için Power BI kimlik bilgisi. Bu Power BI için kimlik bilgilerini *ana* hesabı uygulamanızdaki depolanır ve oluşturmak için kullanılan simgeleri ekleme.

## <a name="what-to-do-when-you-are-ready-for-production"></a>Üretim için hazır olduğunuzda yapmanız gerekenler

Üretime taşımak hazır olduğunuzda aşağıdakileri yapmanız gerekir:

- Geliştirme için ayrı bir kiracı kullanıyorsanız, panolar ve raporlar, birlikte uygulama alanlarınızı üretim ortamınızda kullanılabilir olduğundan emin olmak gerekir. Uygulamayı üretim kiracınız için Azure AD'de oluşturulan ve adım 1'de gösterildiği gibi doğru uygulama izinlerine emin olun.

- Gereksinimlerinize uyan bir kapasite satın alın. Kullanabileceğiniz [katıştırılmış analytics kapasite teknik incelemesi planlama](https://aka.ms/pbiewhitepaper) gereksinim duyabileceğiniz anlamanıza yardımcı olması için. Satın almak hazır olduğunuzda, Azure portalı içinde Power BI Embedded kaynak satın alabilirsiniz.

- Uygulama çalışma düzenleyin ve Gelişmiş altında bir kapasite atayın.

    ![Uygulama çalışma powerbi.com kapasitesiyle atayın](media/migrate-from-power-bi-workspace-collections/embedded-capacity.png)

- Güncelleştirilmiş uygulamanızı üretime dağıtmak ve Power BI Embedded raporlardan katıştırma başlayın.

## <a name="after-migration"></a>Geçişten sonra

Bazı temizleme Power BI çalışma alanı koleksiyonu içinde gereklidir.

- Azure hizmet Power BI çalışma alanı koleksiyonu içinde dağıtılan çözümü dışına tüm çalışma alanlarını kaldırın.
- Azure içinde mevcut herhangi bir çalışma alanı koleksiyonu silin.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler. Uygulamanız artık Power BI Embedded geçirilir. Power BI panoları, raporları ve veri kümelerini ekleme hakkında daha fazla bilgi için bkz: [, Power BI panoları, raporları ve döşeme katıştırma](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Başka sorunuz mu var? [Power BI topluluk isteyen deneyin](http://community.powerbi.com/)