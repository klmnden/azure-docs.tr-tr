---
title: En son Azure Search Hizmeti REST API'si sürüme yükseltme | Microsoft Docs
description: En son Azure Search Hizmeti REST API'si sürüme yükseltme
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 3848f317fd6d760961756f132edf9cbcb5f431ee
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32181979"
---
# <a name="upgrading-to-the-latest-azure-search-service-rest-api-version"></a>En son Azure Search Hizmeti REST API'si sürüme yükseltme
Önceki bir sürümünü kullanıyorsanız, [Azure Search Hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice/), bu makalede, en son kullanmak için uygulamanızı yükseltmenize yardımcı olur genel olarak kullanılabilir API sürümü, 2017 11 11.

REST API sürümü 2017-11-11 önceki sürümlerinden bazı değişiklikler içerir. Kodunuzu değiştirmeden önce kullandığınız bağlı olarak hangi sürümün yalnızca en az çaba istemeniz gerekir böylece bunlar çoğunlukla geriye dönük uyumludur. Bkz: [yükseltme adımları](#UpgradeSteps) yönelik yeni bir API sürümü kullanmak yönergeler kodunuzu değiştirmek.

> [!NOTE]
> Azure Search Hizmeti örneğinizi son de dahil olmak üzere birkaç REST API sürümlerini destekler. En son artık değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirmek öneririz bir sürümünü kullanmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2017-11-11"></a>Sürüm 2017 11 11 yenilikler
Sürüm 2017 11 11 olan en son Azure Search Hizmeti REST API sürümü genel olarak kullanılabilir. Bu API sürümündeki yeni özellikler içerir:

* [Eş anlamlıları](search-synonyms.md). Yeni eş anlamlıları özelliği, sorgunun kapsamını genişletmek ve eşdeğer koşullarını tanımlamak sağlar.
* [Verimli bir şekilde metin BLOB dizini oluşturma desteği](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#IndexingPlainText). Yeni `text` modu Azure Blob dizin oluşturucular için önemli ölçüde ayrıştırma dizin oluşturma performansı artırır.
* [Hizmet istatistikleri API](https://aka.ms/azure-search-stats). Azure Search'te bu yeni API ile kaynakları sınırlarını ve geçerli kullanım görüntüleyin.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
GA sürümünden yükseltme yapıyorsanız, 2015-02-28 veya 2016-09-01, büyük olasılıkla kodunuz için dışında sürüm numarasını değiştirmek için değişiklik gerekmez. Kodu değiştirmeniz gerekebilir yalnızca durumlar ne zaman şunlardır:

* Tanınmayan özellikleri bir API yanıt döndürüldüğünde kod başarısız olur. Varsayılan olarak, uygulamanızın anlamadığı özellikleri yok saymanız gerekir.
* Kodunuzu API istekleri devam ederse ve yeni API sürümü yeniden göndermeyi dener. Uygulamanızın arama API'den döndürülen devamlılık belirteçleri devam ederse, örneğin, bu durum oluşabilir (daha fazla bilgi için Ara `@search.nextPageParameters` içinde [arama API Başvurusu](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Bu durumlarda ya da sizin için geçerli değilse, kodunuzu uygun şekilde değiştirmeniz gerekebilir. Kullanmaya başlamak istemediğiniz sürece Aksi halde, hiçbir değişiklik gerekli olmalıdır [yeni özellikler](#WhatsNew) 2017 11 11 sürümü.

Bir önizleme API sürümünden yükseltme yapıyorsanız, yukarıdaki de geçerlidir, ancak bazı Önizleme özellikleri 2017 11 11 sürümünde mevcut olmayan farkında olmanız da gerekir:

* CSV dosyaları ve JSON dizileri içeren BLOB'lar için Azure Blob Storage dizin oluşturucu desteği.
* "Bu gibi daha fazla" sorguları

Kodunuzu bu özellikleri kullanıyorsa, bunları kullanımınızı kaldırmadan 2017-11-11'e yükseltmeniz mümkün olmaz.

> [!IMPORTANT]
> Lütfen unutmayın, Önizleme API'leri sınama ve değerlendirme için tasarlanmıştır ve üretim ortamlarında kullanılmamalıdır.
> 
> 

## <a name="conclusion"></a>Sonuç
Azure Search Hizmeti REST API'sini kullanarak hakkında daha fazla ayrıntı ihtiyacınız varsa, yeni güncelleştirilen bkz [API Başvurusu](https://docs.microsoft.com/rest/api/searchservice/) konusuna bakın.

Azure Search'te bildiriminiz bizim. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure arama MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) veya [StackOverflow](http://stackoverflow.com/). Azure Search hakkında bir soru üzerinde StackOverflow soran, kendisiyle etiketlemek emin olun `azure-search`.

Azure Search kullandığınız için teşekkür ederiz!

