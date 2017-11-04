---
title: "Azure Search Hizmeti REST API'si için sürüm 2016-09-01 yükseltme | Microsoft Docs"
description: "Azure Search Hizmeti REST API'si için sürüm 2016-09-01 yükseltme"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: f6a189c2e314b91c490583a86d8bacca8ec78a0f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="upgrading-to-the-azure-search-service-rest-api-version-2016-09-01"></a>Azure Search Hizmeti REST API'si için sürüm 2016-09-01 yükseltme
Sürüm 2015-02-28 veya 2015-02-28-Önizleme birini kullanıyorsanız, [Azure Search Hizmeti REST API'si](https://msdn.microsoft.com/library/azure/dn798935.aspx), bu makalede 2016-09-01 sonraki genel olarak kullanılabilir API sürümü kullanmak için uygulamanızı yükseltmenize yardımcı olur.

REST API sürümü 2016-09-01 önceki sürümlerinden bazı değişiklikler içerir. Kodunuzu değiştirmeden önce kullandığınız bağlı olarak hangi sürümün yalnızca en az çaba istemeniz gerekir böylece bunlar çoğunlukla geriye dönük uyumludur. Bkz: [yükseltme adımları](#UpgradeSteps) yönelik yeni bir API sürümü kullanmak yönergeler kodunuzu değiştirmek.

> [!NOTE]
> Azure Search Hizmeti örneğinizi son de dahil olmak üzere birkaç REST API sürümlerini destekler. En son artık değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirmek öneririz bir sürümünü kullanmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a>Sürüm 2016-09-01 yenilikler
Sürüm 2016-09-01 ikinci genel olarak kullanılabilir Azure Search Hizmeti REST API'si sürümüdür. Bu API sürümündeki yeni özellikler içerir:

* [Özel çözümleyiciler](https://aka.ms/customanalyzers), denetimini ele dizine ve aranabilir belirteçlere metin dönüştürme işlemi sağlar.
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) ve [Azure Table Storage](search-howto-indexing-azure-tables.md) kolayca verileri Azure depolama alanından Azure Search bir zamanlama veya isteğe bağlı almak izin dizin oluşturucular.
* [Alan eşlemeleri](search-indexer-field-mappings.md), hangi dizin oluşturucular verileri Azure Search nasıl içeri özelleştirmenize olanak sağlar.
* Etag'ler, dizinler, dizin oluşturucular ve veri kaynaklarını tanımlarını bir eşzamanlılık güvenli şekilde güncelleştirmenize olanak sağlar. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
2015-02-28 sürümünden yükseltme yapıyorsanız, kodunuz için dışında sürüm numarasını değiştirmek için değişiklik gerekmez. Kodu değiştirmeniz gerekebilir yalnızca durumlar ne zaman şunlardır:

* Tanınmayan özellikleri bir API yanıt döndürüldüğünde kod başarısız olur. Varsayılan olarak, uygulamanızın anlamadığı özellikleri yok saymanız gerekir.
* Kodunuzu API istekleri devam ederse ve yeni API sürümü yeniden göndermeyi dener. Uygulamanızın arama API'den döndürülen devamlılık belirteçleri devam ederse, örneğin, bu durum oluşabilir (daha fazla bilgi için Ara `@search.nextPageParameters` içinde [arama API Başvurusu](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).

Bu durumlarda ya da sizin için geçerli değilse, kodunuzu uygun şekilde değiştirmeniz gerekebilir. Kullanmaya başlamak istemediğiniz sürece Aksi halde, hiçbir değişiklik gerekli olmalıdır [yeni özellikler](#WhatsNew) 2016-09-01 sürümü.

Sürüm 2015-02-28-Önizlemesi'nden yükseltiyorsanız, yukarıdaki de geçerlidir, ancak bazı Önizleme özellikleri 2016-09-01 sürümünde mevcut olmayan farkında olmanız da gerekir:

* CSV dosyaları ve JSON dizileri içeren BLOB'lar için Azure Blob Storage dizin oluşturucu desteği.
* Eş anlamlıları
* "Bu gibi daha fazla" sorguları

Kodunuzu bu özellikleri kullanıyorsa, bunları kullanımınızı kaldırmadan 2016-09-01 yükseltmeniz mümkün olmaz.

> [!IMPORTANT]
> Lütfen unutmayın, Önizleme API'leri sınama ve değerlendirme için tasarlanmıştır ve üretim ortamlarında kullanılmamalıdır.
> 
> 

## <a name="conclusion"></a>Sonuç
Azure Search Hizmeti REST API'sini kullanarak hakkında daha fazla ayrıntı ihtiyacınız varsa, yeni güncelleştirilen bkz [API Başvurusu](https://msdn.microsoft.com/library/azure/dn798935.aspx) konusuna bakın.

Azure Search'te bildiriminiz bizim. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure arama MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) veya [StackOverflow](http://stackoverflow.com/). Azure Search hakkında bir soru üzerinde StackOverflow soran, kendisiyle etiketlemek emin olun `azure-search`.

Azure Search kullandığınız için teşekkür ederiz!

