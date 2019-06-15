---
title: Sık sorulan sorular (SSS) - Azure Search
description: Microsoft Azure arama hizmeti, Microsoft Azure üzerinde barındırılan bulut arama hizmeti hakkında sık sorulan soruların yanıtlarını alın.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/03/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: c77f26187914b2c6e52426bb2a07303b22ccb2b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65023988"
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Azure arama - sık sorulan sorular (SSS)

 Kavramları, kod ve Azure Search için ilgili senaryolar hakkında sık sorulan soruların yanıtlarını bulun.

## <a name="platform"></a>Platform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Azure Search my DBMS tam metin aramasını farklı mı?

Azure Search'ü destekleyen birden çok veri kaynağına [birçok dil için dil analizi](https://docs.microsoft.com/rest/api/searchservice/language-support), [özel analize ilgi çekici ve olağan dışı verisi girişleri](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), ara sıra denetimlerde [Puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)ve kullanıcı deneyimi özellikleri typeahead, isabet vurgulama ve çok yönlü gezinme gibi. Ayrıca eş anlamlı sözcükler ve zengin sorgu sözdizimi gibi diğer özellikleri içerir, ancak bunlar genellikle özellikleri farklılaştırılması değil.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>Azure Search Elasticsearch arasındaki fark nedir?

Arama teknolojileri karşılaştırılırken, müşterilerin Azure Search ile Elasticsearch'e nasıl karşılaştırması özellikleri için sık isteyin. Bir anahtar görevi daha kolay yaptık veya diğer Microsoft teknolojileri ile yerleşik tümleştirme ihtiyaç duydukları çünkü bunu müşteriler, Azure Search Elasticsearch kendi arama uygulaması projelerinde genellikle seçin:

+ Azure arama, (2 çoğaltma yönelik okuma erişimi, okuma-yazma için 3 çoğaltması) yeterli yedeklilik sağlarken % 99,9 Hizmet düzeyi sözleşmeleri (SLA) tam olarak yönetilen bulut hizmetiyle özelliğidir.
+ Microsoft'un [doğal dili işlemcileri](https://docs.microsoft.com/rest/api/searchservice/language-support) önde gelen dil analizi sunar.  
+ [Azure Search dizin oluşturucularında](search-indexer-overview.md) çeşitli Azure veri kaynaklarından ilk ve artımlı dizinleme gezinebileceği.
+ Sorgu veya dizin oluşturma birimleri dalgalanmaların hızlı yanıt gerekiyorsa, kullanabileceğiniz [kaydırıcı denetimleri](search-manage.md#scale-up-or-down) Azure portal ya da çalışma bir [PowerShell Betiği](search-manage-powershell.md), doğrudan parça yönetim atlama.  
+ [Puanlama ve ayarlama özellikleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) etkileyen için yol arama ne tek başına bir arama motoru sağlayabilir ötesinde derecelendirme puanlarını sağlayın.

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Azure Search hizmeti duraklatma ve miyim faturalandırmayı durdurmak?

Hizmeti duraklatılamıyor. Hizmet oluşturulduğunda hesaplama ve depolama kaynakları, özel kullanım için ayrılır. Yayın ve bu kaynakları isteğe bağlı geri kazanmak mümkün değildir.

## <a name="indexing-operations"></a>Dizin oluşturma işlemleri

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Yedekleme ve geri yükleme (veya yükleme ve taşıma) dizinleri veya dizin anlık görüntü?

Ancak [dizin tanımını Al](https://docs.microsoft.com/rest/api/searchservice/get-index) herhangi bir zamanda Dizin ayıklama, anlık görüntü veya yoktur karşıdan yüklemek için Yedekleme Geri Yükleme özelliği bir *doldurulmuş* bulutta çalışan yerel bir sisteme dizini veya Bu, başka bir Azure Search hizmetine taşıma.

Dizin oluşturulur ve yazma ve yalnızca Azure arama bulutta çalışan koddan doldurulur. Genellikle, başka bir hizmete bir dizine geçmek isteyen müşterilerin yeni bir uç noktası kullanmak için kodlarını düzenleyerek bunu ve yeniden dizin oluşturma. Bir anlık görüntüsünü alın veya bir dizin Yedekleme olanağı istiyorsanız, bir oy cast [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-restore-my-index-or-service-once-it-is-deleted"></a>Silindiğinde miyim Belgelerim dizini veya hizmet geri yükleyebilirim?

Hayır, dizinleri veya hizmetler geri yükleyemezsiniz. Azure Search dizini silerseniz, işlemi kalıcıdır ve kaynak dizin kurtarıldı. Azure Search Hizmeti sildiğinizde, hizmet tüm dizinlerde kalıcı olarak silinir. Ayrıca, bir veya daha fazla Azure Search hizmetlerini içeren bir Azure kaynak grubunu silerseniz, tüm hizmetleri kalıcı olarak silinir.  

Dizin, dizin oluşturucular, veri kaynakları ve uzmanlık becerileri gibi kaynakları geri koddan bunları yeniden gerektirir. Dizinler söz konusu olduğunda, dış kaynaklardan gelen verileri yeniden gerekir. Bu nedenle, bir ana kopya veya Azure SQL veritabanı veya Cosmos DB gibi başka bir veri deposuna özgün veri yedeklemesini korumak kesinlikle önerilir.

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>SQL veritabanı çoğaltmaları dizine ekleyebilir (uygulandığı [Azure SQL veritabanı dizin oluşturucular](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

Kısıtlama yoktur kullanılması ile ilgili çoğaltması birincil veya ikincil bir veri kaynağı olarak sıfırdan bir dizin oluşturma sırasında. Ancak, bir dizin (değiştirilen kayıtlarını temel alarak) artımlı Güncelleştirmeler ile yenileme, birincil çoğaltma gerektirir. Bu gereksinim SQL veritabanı, değişiklik izleme yalnızca birincil çoğaltmalara üzerinde hangi garanti gelir. İkincil çoğaltmalar dizin yenileme iş yükü için kullanarak çalışırsanız, tüm verileri alma garantisi yoktur.

## <a name="search-operations"></a>Arama işlemleri

### <a name="can-i-search-across-multiple-indexes"></a>Birden çok dizin arasında arama?

Hayır, bu işlem desteklenmiyor. Arama için tek bir dizinde her zaman kapsamlıdır.

### <a name="can-i-restrict-search-index-access-by-user-identity"></a>Kullanıcı kimliğine göre arama dizini erişimi kısıtlama?

Uygulayabileceğiniz [güvenlik filtreleri](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) ile `search.in()` filtre. Filtre ile ölçeklemesini [gibi Azure Active Directory (aad) Kimlik Yönetimi Hizmetleri](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search-with-aad) göre arama sonuçları kırpmak için kullanıcının grup üyeliğini tanımlanmış.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Neden sıfır vardır, geçerli olması için biliyorum koşullarınızda eşleşir?

En yaygın durumda her bir sorgu türü farklı arama davranışlarını ve dil analizleri düzeylerini destekler bilmektir değil. Baskın iş yükü olan tam metin arama terimleri kök formlar altına keser bir dil analysis aşaması içerir. Çeşitleri daha fazla sayıda parçalanmış teriminin eşleştiği için sorgu ayrıştırma bu yönü olası eşleşmeler üzerinde daha geniş bir ağ uygular.

Joker karakter, belirsiz ve normal ifade sorguları, ancak normal terimini veya tümceciğini sorgular gibi analiz değildir ve sorguyu analiz edilen formun arama dizini bir sözcüğün eşleşmiyorsa için kötü bir geri çağırma açabilir. Sorgu ayrıştırma ve çözümleme hakkında daha fazla bilgi için lütfen bkz [sorgu mimarisi](https://docs.microsoft.com/azure/search/search-lucene-query-architecture).

### <a name="my-wildcard-searches-are-slow"></a>My joker karakter aramalarını yavaş yükleniyor.

Çoğu gibi önek, belirsiz joker arama sorguları ve normal ifade, yazılan dahili olarak arama dizini terimleriyle eşleşen ile. Arama dizini tarama ek bu işlem, gecikme süresini artırır. Daha fazla, geniş bir arama sorguları, gibi `a*` gibi olan büyük olasılıkla pek çok şartlarını yazılması çok yavaş. Yüksek performanslı joker aramalar için tanımlama göz önünde bulundurun. bir [özel çözümleyici](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search).

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Neden arama sıralamasını 1.0 için her isabet eşit veya sabit bir puan mi?

Varsayılan olarak, arama sonuçlarını göre puanlanır [sözcüklerle eşleşen istatistiksel özelliklerin](search-lucene-query-architecture.md#stage-4-scoring)ve sıralı yüksekten düşüğe sonuç kümesi. Ancak, bazı türleri (joker karakter öneki, normal ifade) sorgu her zaman sabit bir puan genel belge puana katkıda bulunun. Bu davranış tasarım gereğidir. Azure arama sonuçlarında derecelendirme etkilemeden dahil edilecek eşleşme sorgu genişletme bulunamadı izin vermek için sabit bir puan uygular.

Örneğin, "turları", "tourettes" ve "tourmaline" eşleşme "Turu *" joker Arama girdisi üretir varsayalım. Bu sonuçları gereği, makul hangi koşulları diğerlerine göre daha değerli çıkarsamak için hiçbir yolu yoktur. Bu nedenle, biz türleri joker karakter, önek ve normal ifade sorgularda sonuçlarını Puanlama, terim frekansları yoksayın. Kısmi bir girişini temel alarak arama sonuçlarını büyük olasılıkla beklenmeyen eşleşme doğrultusunda sapması önlemek için sabit bir puanı verilir.

## <a name="design-patterns"></a>Tasarım desenleri

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Yerelleştirilmiş arama uygulamak için en iyi yaklaşım nedir?

Aynı dizinde farklı yerel ayarlar (dil) desteklemek için söz konusu olduğunda çoğu müşteri bir koleksiyon üzerinde özel alanları seçin. Yerel ayara özgü alanlar uygun bir çözümleyici atamak mümkün kılar. Örneğin, Microsoft Fransızca Çözümleyicisi Fransızca dizeler içeren bir alanına atandı. Filtreleme basitleştirir. Bir sorgu, fr-fr sayfasında başlatılan biliyorsanız, bu alan arama sonuçlarını sınırlayabilir. Veya, oluşturun bir [Puanlama profili](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) alan daha fazla göreli ağırlık vermek için. Azure Search'ü destekleyen üzerinden [50 dil Çözümleyicileri](https://docs.microsoft.com/azure/search/search-language-support) seçilecek.

## <a name="next-steps"></a>Sonraki adımlar

Eksik bir özellik veya işlev hakkında sorunuz var mı? Bu özellik için istekte [User Voice web sitesi](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Ayrıca bkz.

 [StackOverflow: Azure arama](https://stackoverflow.com/questions/tagged/azure-search)   
 [Metin arama Azure Search'te tam nasıl çalışır](search-lucene-query-architecture.md)  
 [Azure Search nedir?](search-what-is-azure-search.md)
