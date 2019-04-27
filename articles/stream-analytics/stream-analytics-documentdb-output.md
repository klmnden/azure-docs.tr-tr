---
title: Cosmos DB için Azure Stream Analytics çıkışı
description: Bu makalede, çıkış veri arşivleme ve düşük gecikme süreli sorgular yapılandırılmamış JSON verileri için bir JSON çıkışı için Azure Cosmos DB'ye kaydetmek için Azure Stream Analytics kullanmayı açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2019
ms.custom: seodec18
ms.openlocfilehash: 734cf09869e5a2df5f9a505a3cb8ccc7bc2338d5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60402318"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Cosmos DB için Azure Stream Analytics çıkışı  
Stream Analytics hedefleyebilir [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) yapılandırılmamış JSON verileri üzerinde veri arşivleme ve düşük gecikme süreli sorgular için JSON çıkışında, etkinleştirme. Bu belge, bu yapılandırmayı uygulamak için bazı en iyi uygulamaları kapsar.

Kişiler için Cosmos DB ile bilginiz, göz atın [Azure Cosmos DB'nin öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/documentdb/) kullanmaya başlamak için. 

> [!Note]
> Şu anda Azure Stream Analytics'i kullanarak Azure Cosmos DB bağlantı yalnızca destekler **SQL API**.
> Diğer Azure Cosmos DB API henüz desteklenmiyor. Noktası Azure Stream Analytics Azure Cosmos DB hesaplarına diğer API'lerle oluşturduysanız, verileri düzgün bir şekilde depolanabilir değil. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Bir çıkış hedefi olarak Cosmos DB ile ilgili temel bilgiler
Stream Analytics, Azure Cosmos DB çıktı sonuçları Cosmos DB koleksiyonlarınız JSON çıktısını işleme akışınız yazılmasını etkinleştirir. Stream Analytics, bunun yerine, ön maliyet oluşturmanızı gerektiren koleksiyonları veritabanınızdaki oluşturmaz. Cosmos DB koleksiyonları fatura maliyetlerini sizin tarafınızdan denetlenir ve böylece performans, tutarlılık ve koleksiyonlarınız kullanarak doğrudan kapasitesini ayarlama budur [Cosmos DB API'leri](https://msdn.microsoft.com/library/azure/dn781481.aspx).

> [!Note]
> Azure Cosmos DB güvenlik duvarını 0.0.0.0 izin verilen IP listesine eklemeniz gerekir.

Cosmos DB koleksiyonu seçeneklerden bazıları aşağıda açıklanmıştır.

## <a name="tune-consistency-availability-and-latency"></a>Tutarlılık, kullanılabilirlik ve gecikme süresini ayarlama
Uygulama gereksinimlerinize eşleştirmek için Azure Cosmos DB veritabanı ve koleksiyon ince ayar yapma ve tutarlılık, kullanılabilirlik ve gecikme süresi arasındaki denge sağlamak sağlar. Okuma tutarlılık düzeylerini bağlı olarak senaryo gereksinimlerinize yönelik Okuma ve yazma gecikme süresi, tutarlılık düzeyi veritabanı hesabınızı seçin. Ayrıca varsayılan olarak, Azure Cosmos DB koleksiyonunuza her CRUD işlemi zaman uyumlu dizin oluşturmayı etkinleştirir. Azure Cosmos DB'de yazma/okuma performans denetlemek için kullanışlı başka bir seçenek budur. Daha fazla bilgi için gözden [, veritabanı ve sorgu tutarlılık düzeylerini değiştirme](../cosmos-db/consistency-levels.md) makalesi.

## <a name="upserts-from-stream-analytics"></a>Stream Analytics'ten alınan upsert eder
Azure Cosmos DB ile Stream Analytics tümleştirmesi ekleme veya güncelleştirme belirli bir belge kimliği sütunu temel alarak koleksiyonunuzdaki kayıtlarını sağlar. Bu da verilir bir *Upsert*.

Stream Analytics ile belge kimliği çakışması ekleme başarısız olduğunda burada güncelleştirmeleri yalnızca işiniz bir iyimser upsert yaklaşımı kullanır. Uyumluluk düzeyi 1.0 ile bu güncelleştirme düzeltme ekini, diğer bir deyişle belgeye kısmi güncelleştirmeler etkinleştirir, yeni özellikler veya varolan bir özellik kademeli olarak gerçekleştirilen değiştirme ek gerçekleştirilir. Ancak, değişiklikler, JSON belge sonucu üzerine tüm dizi diğer bir deyişle, dizi içinde dizi özelliklerin değerlerini değil birleştirilir. 1.2 ile upsert davranışı eklemek veya belgeyi değiştirmek için değiştirildi. Bu uyumluluk düzeyi 1.2 bölümünde daha ayrıntılı açıklanmıştır.

Gelen JSON belgesini alan otomatik olarak Cosmos DB belge kimliği sütunu olarak kullanılır ve herhangi bir sonraki yazma, bu nedenle, bunlardan biri için önde gelen işlenir varolan bir kimliği alanı, varsa:
- eklemek için benzersiz bir kimlik sağlama
- Yinelenen kimlikleri ve 'ID' için ayarlanmış ' Belge Kimliği' upsert müşteri adayları
- Yinelenen kimlikleri ve 'Belge Kimliği' değil kümesi müşteri adayları hata için ilk belge sonra

Kaydetmek istiyorsanız <i>tüm</i> yinelenen bir Kimliğe sahip olanlar da dahil olmak üzere Belge Kimliği alanı sorgunuzda (AS anahtar sözcüğü ile) yeniden adlandırın ve Kimliği alanı oluşturun veya başka bir sütunun değeri ile Kimliğini değiştirin Cosmos DB sağlar (AS anahtar sözcüğü kullanılarak veya 'Belge Kimliği' ayarı kullanarak).

## <a name="data-partitioning-in-cosmos-db"></a>Cosmos DB'de bölümleme verileri
Azure Cosmos DB [sınırsız](../cosmos-db/partition-data.md) bölümler, iş yüküne göre ölçeklenen, verilerinizi otomatik Azure Cosmos DB bölümleme için önerilen yaklaşım kapsayıcı görevi görür. Sınırsız kapsayıcıların için yazma, Stream Analytics gibi çok sayıda paralel yazıcılar önceki sorgu veya girdisi'nden olay adımına bölümleme düzeni kullanır.
> [!Note]
> Şu anda Azure Stream Analytics yalnızca sınırsız koleksiyonu en üst düzeyde bölüm anahtarları ile destekler. Örneğin, `/region` desteklenir. İç içe bölüm anahtarları (örneğin `/region/name`) desteklenmez. 

Sabit Azure Cosmos DB koleksiyonları için Stream Analytics tam olduğunuzda artırmaya veya genişletmeye ölçeklendirme olanağı sağlar. 10 GB ve 10.000 RU/sn aktarım hızı üst sınır sahiptirler.  Sınırsız bir kapsayıcıya (örneğin, bir en az 1.000 RU/sn ve bölüm anahtarı) için sabit bir kapsayıcıdan verileri geçirmek için kullanmanız gerekir [veri geçiş aracı](../cosmos-db/import-data.md) veya [değişiklik akışı Kitaplığı](../cosmos-db/change-feed.md).

Birden çok sabit kapsayıcı yazma kullanımdan kaldırılıyor ve Stream Analytics işinizi ölçeklendirmeye yönelik önerilen yaklaşım değildir. Makaleyi [bölümleme ve ölçeklendirme Cosmos DB'de](../cosmos-db/sql-api-partition-data.md) hakkında daha fazla ayrıntı sağlar.

## <a name="improved-throughput-with-compatibility-level-12"></a>Uyumluluk düzeyi 1.2 ile iyi aktarım hızı
Stream Analytics destekler yerel tümleştirme toplu uyumluluk düzeyi 1.2 ile Cosmos DB'ye yazın. Bu Cosmos DB, aktarım hızı ve verimli bir şekilde tanıtıcı azaltma istekleri en üst düzeye ile etkili bir şekilde yazılmasını sağlar. Geliştirilmiş yazma mekanizması upsert bir davranışı fark nedeniyle yeni bir uyumluluk düzeyi altında kullanılabilir.  1.2 önce upsert ekleyin veya belgeyi birleştirmek için bir davranıştır. 1.2 ile upsert eder davranışı eklemek veya belgeyi değiştirmek için değiştirildi. 

1.2 önce toplu işlem olarak yazıldığı, Cosmos DB içine toplu upsert belgelere bölüm anahtarı başına özel saklı yordam kullanır. Tek bir kaydı geçici bir hata (azaltma) ulaştığında, bile, tüm toplu işlem yeniden denenmelidir. Bu senaryolar bile makul nispeten daha yavaş azaltma ile yapılan. Karşılaştırma aşağıdaki gibi işler 1.2 ile nasıl davranacaktır gösterir.

Kurulum aynı giriş (event hub) okuma iki özdeş Stream Analytics işi gösterir. Her iki Stream Analytics işleri, [tam olarak bölümlenmiş](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) geçiş sorgu ve aynı CosmosDB koleksiyonlara yazma. Uyumluluk düzeyi 1.0 ile yapılandırılmış iş ölçümleri soldaki arasındadır ve sağ taraftaki olanlara 1.2 ile yapılandırılır. Cosmos DB koleksiyonları bölüm anahtarı, giriş olaydan gelen benzersiz bir GUID değeridir.

![Stream analytics ölçümleri karşılaştırma](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Olay Hub'ındaki gelen olay hızı azaltma Cosmos DB'de beklendiği şekilde Cosmos DB koleksiyonları (20 bin RU) işleme için yapılandırılmış olan çok daha yüksek x 2 ' dir. Ancak, 1.2, iş tutarlı bir şekilde yazmak daha yüksek bir aktarım hızı (çıkış olay/dakika) ve bir alt ortalama SU kullanım yüzdesi. Ortamınızda bu fark üzerinde birkaç seçenek olay biçimi, giriş olay iletisi boyut, bölüm anahtarları, sorgu vb. gibi daha fazla faktörlere bağlıdır.

![cosmos db ölçümleri karşılaştırma](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

1.2 ile Stream Analytics'in Cosmos DB'de kullanılabilen aktarım hızı azaltma hız sınırlaması gelen çok az sayıda resubmissions ile % 100'ü kullanarak daha akıllı bulunur. Bu koleksiyonu aynı anda çalışan sorgular gibi diğer iş yükleri için daha iyi bir deneyim sağlar. Bir havuz için 1 k için 10 k iletiler/saniye olarak nasıl ASA kullanıma Cosmos DB ile ölçeklenir kullanıma denemek gerektiği durumlarda, işte bir [azure örnekleri proje](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) olanak tanıyan, yapın.
Cosmos DB çıkış aktarım hızı 1.0 ve 1.1 ile aynı olduğunu unutmayın. 1.2 şu anda varsayılan olmadığından yapabilecekleriniz [uyumluluk düzeyi ayarlayın](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) portalını kullanarak ya da kullanarak bir Stream Analytics işine ilişkin [oluşturma işi REST API çağrısı](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Sahip *önemle tavsiye* Cosmos DB ile uyumluluk düzeyini 1.2 ASA ile kullanılacak. 



## <a name="cosmos-db-settings-for-json-output"></a>Çıkış JSON için cosmos DB ayarları

Stream analytics'te bir çıkış olarak Cosmos DB oluşturma, aşağıda görüldüğü gibi bilgileri için bir istem oluşturur. Bu bölüm, özellikleri tanımının bir açıklama sağlar.

![documentdb stream analytics çıkış ekranı](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Alan           | Açıklama|
|-------------   | -------------|
|Çıktı diğer adı    | Bu çıktı ASA sorgunuzda başvurmak için bir diğer ad.|
|Abonelik    | Azure aboneliği seçin.|
|Hesap Kimliği      | Adı veya uç noktası URI'si, Azure Cosmos DB hesabı.|
|Hesap anahtarı     | Azure Cosmos DB hesabı için paylaşılan erişim anahtarı.|
|Database        | Azure Cosmos DB veritabanının adı.|
|Koleksiyon adı deseni | Kullanılacak bir koleksiyon için koleksiyon adı. `MyCollection` Örnek geçerli bir giriş - adlı bir koleksiyon olduğundan `MyCollection` mevcut olması gerekir.  |
|Belge Kimliği     | İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayanması benzersiz bir anahtar kullanılan çıkış olaylarındaki sütun adı. Boş bırakılırsa, tüm olayları güncelleştirme seçeneği ile eklenir.|
