---
title: Azure Cosmos DB'de destek akış değişiklik ile çalışma
description: Azure Cosmos DB değişiklik akışı desteği, belgelerdeki değişiklikleri izlemek ve Tetikleyicileri gibi olay tabanlı işleme ve önbelleğe alır ve analiz sistemlerinin güncel tutarak gerçekleştirmek için kullanın.
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.reviewer: sngun
ms.custom: seodec18
ms.openlocfilehash: 51a554586c67842ead40cd4a1bfaaa51bbdd8a18
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954404"
---
# <a name="change-feed-in-azure-cosmos-db---overview"></a>Değişiklik akışı Azure Cosmos DB'de - genel bakış

Azure Cosmos DB geliştirilme akış desteği, herhangi bir değişiklik için bir Azure Cosmos DB kapsayıcısı için dinleyerek değiştirin. Ardından, değiştirilmiş olan sırayla değiştirilen belgelerin sıralanmış listesini çıkarır. Değişiklikler kalıcı hale getirilir, zaman uyumsuz ve artırımlı olarak işlenebilir ve çıkış, paralel işleme için bir veya daha fazla tüketiciye dağıtılabilir. 

Azure Cosmos DB, perakende, oyun, IOT ve işlem günlüğü uygulamalar için uygundur. Bu uygulamalar bir ortak tasarım modelinde, ek eylemleri tetiklemek için verilerde yapılan değişiklikleri kullanmaktır. Ek eylem örnekleri şunlardır:

* Bir öğe eklendiğinde veya bir bildirim ya da bir API çağrısına tetikleniyor.
* Gerçek Zamanlı Akış, IOT veya gerçek zamanlı analiz işlem verilerinde işleme için işleme.
* Ek veri taşıma, bir önbellek veya bir arama motoru veya veri ambarı eşitleme veya soğuk depolama için veri arşivleme.

Değişiklik Azure Cosmos DB'de akışı, aşağıdaki görüntüde gösterildiği gibi her biri bu desenleri için etkili ve ölçeklenebilir çözümler oluşturmanıza olanak sağlar:

![Güç gerçek zamanlı analiz ve olay temelli bilgi işlem senaryoları kullanarak Azure Cosmos DB değişiklik akışı](./media/change-feed/changefeedoverview.png)

## <a name="supported-apis-and-client-sdks"></a>Desteklenen API'ları ve istemci SDK'ları

Bu özellik şu anda aşağıdaki Azure Cosmos DB API'ları ve istemci SDK'ları tarafından desteklenmektedir.

| **İstemci sürücüleri** | **Azure CLI** | **SQL API'Sİ** | **Cassandra API'si** | **Azure Cosmos DB'nin MongoDB API'si** | **Gremlin API**|**Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- |
| .NET | NA | Evet | Hayır | Hayır | Evet | Hayır |
|Java|NA|Evet|Hayır|Hayır|Evet|Hayır|
|Python|NA|Evet|Hayır|Hayır|Evet|Hayır|
|Düğüm/JS|NA|Evet|Hayır|Hayır|Evet|Hayır|

## <a name="change-feed-and-different-operations"></a>Değişiklik akışı ve farklı işlemler

Bugün, değişiklik akışı tüm işlemlerde bakın. Burada denetleyebilirsiniz işlevi yalnızca güncelleştirmeler ve değil ekler gibi henüz kullanılabilir akış, belirli işlemler için değiştirin. "Yumuşak işaret" öğesi güncelleştirmeleri ve üzerinde değişiklik akışı öğeleri işlerken göre filtre ekleyebilirsiniz. Değişiklik akışı siler oturum şu anda değil. Önceki örneğe benzer, yumuşak bir işaretçi, silinen öğeleri ekleyebilirsiniz, örneğin, böylece otomatik olarak silinebilir "silindi" adlı "true" olarak ayarlayın ve öğe üzerinde bir TTL ayarlamak öğesindeki bir öznitelik ekleyebilirsiniz. Değişiklik geçmiş öğeleri için örneğin, beş yıl önce eklenen öğeleri akışı okuyabilirsiniz. Öğe silinmedi, değişiklik okuyabilirsiniz kapsayıcınızı kaynağı sunulan ürünün kendinde akış.

### <a name="sort-order-of-items-in-change-feed"></a>Değişiklik akışı öğelerinin sıralama

Değişiklik akışı öğelerini değiştirme zamanlarının sırasına göre gelir. Bu sıralama düzeni mantıksal bölüm anahtarı garanti edilir.

### <a name="change-feed-in-multi-region-azure-cosmos-accounts"></a>Değişiklik akışı çok bölgeli Azure Cosmos hesaplar

Çok bölgeli bir Azure Cosmos hesabında üzerine yazma bölgesi başarısız olursa, değişiklik akışı el ile yük devretme işlemi çalışır ve bitişik olacaktır.

### <a name="change-feed-and-time-to-live-ttl"></a>Değişiklik akışı ve yaşam süresi (TTL)

Değişiklik akışı, bir TTL (yaşam süresi) özelliği bir öğe üzerinde -1 olarak ayarlarsanız, her zaman açık kalır. Veriler silinmez, değişiklik akışı kalır.  

### <a name="change-feed-and-etag-lsn-or-ts"></a>Değişiklik akışı ve _etag, _lsn veya _ts

_Etag biçimi dahili kullanım içindir ve dilediğiniz zaman değiştirebilirsiniz çünkü, bağımlılık üzerinde almamalıdır. _ts bir değişiklik ya da oluşturma zaman damgası ' dir. _Ts kronolojik bir karşılaştırması için kullanabilirsiniz. _lsn için değişiklik yalnızca akışı eklenen bir toplu iş kimliği:; Bu işlem kimliğini temsil eder Birçok öğe aynı _lsn olabilir. ETag FeedResponse üzerinde öğede gördüğünüz _etag farklıdır. _etag dahili bir tanımlayıcıdır ve eşzamanlılık için kullanılan denetim öğesi sürümü hakkında akışın sıralama için ETag kullanılırken söyler.

## <a name="change-feed-use-cases-and-scenarios"></a>Kullanım örnekleri ve senaryoları değişiklik akışı

Yüksek hacimli yazma ile büyük veri kümeleri işlem verimli etkinleştirir değişiklik akışı. Ayrıca, değişiklik akışı nelerin değiştiğini belirlemek için bir veri kümesinin tamamında sorgulama için bir alternatif sunar.

### <a name="use-cases"></a>Uygulama alanları

Örneğin, değişiklik akışı ile aşağıdaki görevleri verimli bir şekilde gerçekleştirebilirsiniz:

* Bir önbellek güncelleştirme, bir arama dizinini güncelleştirin veya Azure Cosmos DB'de depolanan verilerle bir veri ambarı'nı güncelleştirin.

* Bir uygulama düzeyi verileri katmanlama ve Arşiv uygulamak, örneğin, "Sık erişimli veriler" Azure Cosmos DB'de depolamak ve örneğin, "soğuk veri" diğer depolama sistemlerinde kullanıma yaş [Azure Blob Depolama](../storage/common/storage-introduction.md).

* Başka bir Azure Cosmos hesabı ya da başka bir Azure Cosmos kapsayıcı sıfır kapalı kalma süresini geçişler, farklı bir mantıksal bölüm anahtarı ile gerçekleştirin.

* Uygulama [lambda mimarisi](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) kullanarak Azure Cosmos DB, burada Azure Cosmos DB hem gerçek zamanlı olarak toplu hem de hizmet katmanları sorgu destekler, böylece lambda mimarisi düşük ile etkinleştirme.

* Kullanarak bu olaylar gerçek zamanlı olarak, örneğin, işlem almak ve olay verilerini cihazlar, algılayıcılar, altyapı ve uygulamalardan [Spark](../hdinsight/spark/apache-spark-overview.md).  Aşağıdaki görüntüde, değişiklik akışı ile Azure Cosmos DB kullanarak lambda mimarisi nasıl uygulayacağınıza dair gösterilmektedir:

![Kesintisiz alım ve sorgu için Azure Cosmos DB tabanlı lambda işlem hattı](./media/change-feed/lambda.png)

### <a name="scenarios"></a>Senaryolar

Değişiklik akışı ile kolayca uygulayabilirsiniz senaryolardan bazıları şunlardır:

* İçinde [sunucusuz](https://azure.microsoft.com/solutions/serverless/) web veya mobil uygulamaları, müşterinizin profili, tercihlerine veya konumlarını olayları gibi tüm değişiklikleri izlemek ve belirli eylemler, örneğin, cihazlarına anında iletme bildirimleri gönderme tetikleyin kullanarak [Azure işlevleri](change-feed-functions.md).

* Bir oyun oluşturmak için Azure Cosmos DB kullanıyorsanız, şunları yapabilirsiniz, örneğin, kullanım değişiklik akışı tamamlanmış oyunlardan puanları göre gerçek zamanlı puan tabloları uygulamak için.


## <a name="working-with-change-feed"></a>Değişiklik akışı ile çalışma

Aşağıdaki seçenekleri kullanarak değişiklik akışı ile çalışabilirsiniz:

* [Azure işlevleri ile akış Değiştir](change-feed-functions.md)
* [Değişiklik kullanarak değişiklik akışı işlemci kitaplığı içeren akış](change-feed-processor.md) 

Değişiklik akışı kapsayıcıdaki her bir mantıksal bölüm anahtarı için kullanılabilir ve, paralel işleme için bir veya daha fazla tüketicileri arasında aşağıdaki resimde gösterildiği gibi dağıtılabilir.

![Azure Cosmos DB değişiklik akışı, dağıtılan işleme](./media/change-feed/changefeedvisual.png)

## <a name="features-of-change-feed"></a>Değişiklik akışı özellikleri

* Değişiklik akışı, tüm Azure Cosmos hesaplar için varsayılan olarak etkindir.

* Kullanabileceğiniz, [sağlanan aktarım hızı](request-units.md) değişiklik akışı okumak için olduğu gibi herhangi diğer Azure Cosmos DB işleminde, Azure Cosmos veritabanınızla ilişkili bölgelerden.

* Değişiklik akışı, ekler ve kapsayıcı içindeki öğelerde yapılan güncelleştirme işlemlerini içerir. Siler yakalayabilirsiniz öğelerinizi (örneğin, belgeleri) içinde "geçici silme" bayrak ayarlayarak yerine siler. Alternatif olarak, sınırlı bir süre için öğelerinizle ayarlayabilirsiniz [TTL özelliği](time-to-live.md). Örneğin, 24 saat ve kullanım yakalamak için bu özelliğin değerini siler. Bu çözüm sayesinde, TTL sona erme süresinden daha kısa bir süre içinde değişiklikleri işleme gerekir. 

* Her değişiklik için bir öğe değişiklik akışı tam bir kez görünür ve istemcilerin denetim noktası oluşturma mantığı yönetmeniz gerekir. Kontrol noktalarını yönetme karmaşasından kaçınmak istiyorsanız, değişiklik akışı işlemci kitaplığı otomatik denetim noktası oluşturma ve "en az bir kez" semantiği sağlar. Bkz: [değişiklik akışa değişiklik akışı işlemci kitaplığı ile](change-feed-processor.md).

* Yalnızca belirli bir öğe en son değişikliğin değişiklik günlüğünde bulunur. Ara değişikliklerin kullanılamayabilir.

* Değişiklik akışı, her bir mantıksal bölüm anahtarı değeri içinde değişiklik sıraya göre sıralanır. Bölüm anahtarı değerlerine arasında yürütülme sırası yoktur.

* Değişiklikleri tüm-değişiklikler kullanılabilir sabit veri saklama dönemi yoktur olan belirli bir noktaya, eşitlenebilir.

* Değişiklikler, paralel bir Azure Cosmos kapsayıcının tüm mantıksal bölüm anahtarları için kullanılabilir. Bu özellik paralel olarak birden fazla tüketici tarafından işlenmek üzere büyük kapsayıcıları değişikliklerini tanır.

* Uygulamaları aynı anda birden çok değişiklik akışlarından aynı kapsayıcıda genericread isteyebilir. ChangeFeedOptions.StartTime ilk bir başlangıç noktası sağlamak için kullanılabilir. Örneğin, belirli bir saatin karşılık gelen devamlılık belirteci bulunamadı. ContinuationToken belirtilmişse StartTime ve StartFromBeginning değerlerin kazanır. ~ 5 saniye duyarlığını ChangeFeedOptions.StartTime olur. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de akış değiştirme hakkında daha fazla bilgi edinmek için şimdi geçebilirsiniz:

* [Değişiklik akışını okumak için seçenekleri](read-change-feed.md)
* [Azure işlevleri ile akış Değiştir](change-feed-functions.md)
* [Kullanarak değişiklik akışı işlemci kitaplığı](change-feed-processor.md)
