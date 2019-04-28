---
title: En son Azure arama hizmeti REST API'si sürüme - Azure Search yükseltin
description: API sürümleri farklılıkları gözden geçirin ve hangi işlemlerin en yeni Azure Search Hizmeti REST API sürümü için mevcut kodu geçirilmesi için gereken bilgi edinin.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 23003859b9a75fb986fe65f5528004f3dd150f9b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61127000"
---
# <a name="upgrade-to-the-latest-azure-search-service-rest-api-version"></a>En son Azure Search Hizmeti REST API sürümüne yükseltme
Önceki bir sürümünü kullanıyorsanız, [Azure arama hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice/), bu makalede, uygulamanızın en son yükseltme yardımcı olacak genel kullanıma sunulan API Sürüm 2017-11-11.

REST API Sürüm 2017-11-11 bazı değişiklikler daha önceki sürümlerin içerir. Bu çoğunlukla geriye dönük uyumlu yayımlanır; dolayısıyla, kod değiştirme önce kullandığınız bağlı olarak hangi sürümün yalnızca en az çaba istemeniz gerekir. Bkz: [yükseltme adımları](#UpgradeSteps) yeni API sürümünü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

> [!NOTE]
> Azure Search Hizmeti örneğinizi en son dahil olmak üzere çeşitli REST API sürümlerini destekler. Artık en son değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirme öneririz bir sürümünü kullanmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2017-11-11"></a>Sürüm 2017-11-11 ' yenilikler
Sürüm 2017-11-11 olan en son Azure arama hizmeti REST API'si genel kullanıma sunulan sürümü. Bu API sürümündeki yeni özellikler içerir:

* [Eş Anlamlılar](search-synonyms.md). Eşdeğer terimler tanımlayın ve sorgunun kapsamını genişletmek yeni eş anlamlılar özelliğini sağlar.
* [Verimli bir şekilde metin bloblarını dizine ekleme desteği](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#IndexingPlainText). Yeni `text` modu Azure Blob dizin oluşturucular için önemli ölçüde ayrıştırma, dizin oluşturma performansı artırır.
* [Hizmet istatistikleri API](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics). Bu yeni API ile Azure Search'te kaynaklarının sınırlarını ve geçerli kullanım görüntüleyin.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
Bir GA sürümden yükseltiyorsanız, 2015-02-28 ya da 2016-09-01, büyük olasılıkla kodunuza dışında sürüm numarasını değiştirmek için değişiklik gerekmez. Kodu değiştirmek gerekebilir yalnızca durumlar ne zaman şunlardır:

* Tanınmayan özellikleri içinde bir API yanıt döndürüldüğünde kod başarısız olur. Varsayılan olarak, uygulamanızın anlamadığı özellikleri yok saymanız gerekir.
* Kodunuzu API isteklerinin devam ediyorsa ve bunları yeni API sürümüne yeniden dener. Uygulamanızı arama API'den döndürülen devamlılık belirteçleri devam ederse, örneğin, bu gerçekleşebilir (daha fazla bilgi için Ara `@search.nextPageParameters` içinde [arama API'si başvurusu](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Bu durumlarda ya da sizin için geçerliyse, kodunuzu buna göre değiştirmeniz gerekebilir. Kullanmaya başlamak istemediğiniz sürece Aksi takdirde, hiçbir değişiklik gerekli olmamalıdır [yeni özellikler](#WhatsNew) sürüm 2017-11-11.

Bir önizleme API sürümünden yükseltiyorsanız, yukarıdaki de geçerlidir ancak de bazı Önizleme özellikleri sürüm 2017-11-11 kullanılabilir olmadığını bilmeniz gerekir:

* CSV dosyaları ve blobları JSON dizileri içeren bir Azure Blob Depolama dizin oluşturucu desteği.
* "Bu gibi daha fazla" sorguları

Kodunuzu bu özellikleri kullanıyorsa, bunların kullanım kaldırmadan 2017-11-11'e yükseltmek mümkün olmayacaktır.

> [!IMPORTANT]
> Lütfen unutmayın, Önizleme API'leri, test ve değerlendirme içindir ve üretim ortamlarında kullanılmamalıdır.
> 
> 

## <a name="conclusion"></a>Sonuç
Azure arama hizmeti REST API'si kullanma hakkında daha fazla ayrıntı gerekiyorsa, yeni güncelleştirilen bkz [API Başvurusu](https://docs.microsoft.com/rest/api/searchservice/) MSDN'de.

Bildirimleriniz Azure Search'te bizim için değerli. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure Search MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) veya [StackOverflow](https://stackoverflow.com/). Azure Search hakkında bir soru StackOverflow isteyen, kendisiyle etiketlemek emin `azure-search`.

Azure Search kullandığınız için teşekkür ederiz!

