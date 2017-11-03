---
title: "Market veri hizmeti oluşturmak için teknik ön koşullar | Microsoft Docs"
description: "Dağıtma ve Azure Marketi satmak için veri hizmeti oluşturmak için gereksinimleri anlama"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: aaff609a-1cd1-4146-98f4-d04166b0fce0
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 52827723477677bc292c645e2390c435fbad3ee4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Veri Hizmeti oluşturmak için teknik ön koşullar için Azure Marketi teklifi
> [!IMPORTANT]
> **Şu anda artık ekleme duyuyoruz herhangi yeni veri hizmeti yayımcılar. Yeni dataservices listesine onaylanmamış.** Üzerinde AppSource yayımlamak istediğiniz bir SaaS iş uygulaması varsa daha fazla bilgi bulabilirsiniz [burada](https://appsource.microsoft.com/partners). Bir Iaas uygulamalar varsa veya Azure marketi, yayımlamak istediğiniz Geliştirici hizmet, daha fazla bilgi bulabilirsiniz [burada](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

İşlemi başlamadan önce baştan sona okuyun ve nerede ve neden her adım gerçekleştirilir anlayın. Mümkün olduğunca, şirket bilgilerinizi ve diğer verileri hazırlama, gerekli Araçları'nı indirmek ve/veya teknik bileşenleri teklifi oluşturma işlemi başlamadan önce oluşturun.

Aşağıdaki öğeler işlemine başlamadan önce hazır olmalıdır:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Hangi teknoloji veri hizmeti teklifiniz yayımlamak için kullanılan üzerinde kararı
Bir yayımcı veri hizmeti Azure Marketi'nde yayımlama sırasında birden çok teknolojileri arasında karar verebilirsiniz. Aşağıda açıklanan desteklenen ana teknolojileri. Ne olursa olsun hangi teknolojisi, veri hizmeti yayımlamak için kullanılır, son kullanıcı verilerine tüketir **OData akışı** Azure Market servisi tarafından açılmış. Tam üzerinde Bul OData hizmeti hakkında bilgi [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure veritabanı
Veri kümesi SQL Azure içinde hazır olan yayımcının sorumluluğundadır. Azure abone olmak için veritabanı uygun boyutta sağlamak ve verilerinizi SQL Azure Veritabanına karşıya gerekir. Yayımcı güncelleştirmesini verileri her zaman güncel tutmak için de sorumludur. Azure üzerinde bulabilir hizmetlerine abone olma hakkında daha fazla bilgi [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)

Verileri SQL Azure taşırken, Azure Marketi tabloları ve görünümleri getirebilir. Yayımcı, hangi tabloları/görünümlerini ve sütunları son kullanıcıya sunulan belirtebilirsiniz. Daha fazla içerik sağlayıcısı Ayrıca son kullanıcı tarafından hangi sütunların sorgulanabilir ve hangilerinin yalnızca yükte döndürülür belirtebilirsiniz. Bu, yüksek düzeyde bir veritabanındaki verileri hakkında açılmamalıdır esneklik sağlar. Bir veya daha fazla veritabanı dizinlerini yedeklenmesi sorgulanabilir sütunları gerekir.

## <a name="rest-based-web-service"></a>REST tabanlı web hizmeti
Desteklenen protokolü: **yalnızca HTTPS**

Varolan REST tabanlı hizmetler Azure Marketi'nde gösterilebilir. Veri kümesi için son kullanıcı bir OData akışı olarak her zaman açık olduğu için bir OData hizmeti eşlemek için Azure Marketi hizmet gereksinimleri hizmeti temel. Uç noktaları REST tabanlı Bunu yapmak için tüm parametreleri HTTP parametre olarak kullanıma sunmak gerekir.

Yükü bir ATOM yanıtına eşlenebilir. bir biçimde olması gerekir. Bu nedenle Hizmetleri XML biçiminde ve yalnızca can olması gerekiyor yanıttan (ör. kayıt kümesi) yükü değerleri içeren bir yinelenen öğe içeriyor. Azure Marketi'nden hizmet yinelenen düğüm ATOM ve yükü değerleri girişi düğüme girişi düğümü içinde özellik düğümleri içine eşler.

Yetkilendirme bilgileri (örneğin, kimlik doğrulama belirteci, vb. anahtar, API) HTTP parametresi olarak veya HTTP üstbilgisi (anahtar değer çifti) – temel kimlik doğrulaması da desteklenir sağlanan olması gerekir. Geçerli bir anahtar sağlanmalıdır ve Azure Market üzerinden tüm istekleri anahtara yapıldığını. İzleme ve faturalama kullanıcı Azure Marketi katmanında olur.

Hizmet tarafından döndürülen hataları, HTTP durum kodları eşlenmesi gerekir. HTTP durum kodları için Azure Marketi hizmeti tarafından eşlenmesi bu adımıdır durumunda hata içeren bir XML hizmeti döndürür.

## <a name="soap-based-web-services"></a>SOAP tabanlı web Hizmetleri
Protokol: **yalnızca HTTPS**

REST olduğu gibi aynı hizmet bölümü dayalı gereksinimleridir. Tek fark, parametreleri Publisher'ın hizmetine ile Azure Market üzerinden yapılan her isteği gönderiliyor bir XML gövdesinde de girilebilir ' dir. Bu, kullanıcı ön uç sağlar HTTP parametreleri XML öğelerine içerik sağlayıcının web hizmeti için istekle birlikte gönderilen XML belgesinin çevrildiğini anlamına gelir.

## <a name="odata-based-web-services"></a>OData tabanlı web Hizmetleri
Protokol: **yalnızca HTTPS**

Verileri Azure Marketi'nde bir OData hizmeti olarak gösterilebilir. Sistem hizmeti aracılığıyla geçirmek için giderek ve tüm sonraki çağrılar Azure Market üzerinden Git emin olmak için Azure Marketi hizmet kök – hizmetinin kökü değiştirir.

OData hizmetlerini yalnızca arka uç veritabanı karşı Git gerekmez. OData hizmet sürücü depolama veya iş mantığı her türlü destekler.

## <a name="next-steps"></a>Sonraki Adımlar
Ön koşulları gözden geçirdikten ve gerekli görevleri tamamlandı artık, ileriye doğru veri hizmeti teklifiniz içinde ayrıntılı olarak oluşturmayla taşıyabilirsiniz [veri hizmeti yayımlama Kılavuzu](marketplace-publishing-data-service-creation.md).

Veya genel işlem ve ilgili makaleler her yayımlama aşamaları için gözden geçirmek istiyorsanız, lütfen makalede ziyaret [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
