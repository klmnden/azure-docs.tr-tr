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
ms.openlocfilehash: de5febaeecd176a8718364720132d3fa4433c57f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443619"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Cosmos DB için Azure Stream Analytics çıkışı  
Stream Analytics hedefleyebilir [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) yapılandırılmamış JSON verileri üzerinde veri arşivleme ve düşük gecikme süreli sorgular için JSON çıkışında, etkinleştirme. Bu belge, bu yapılandırmayı uygulamak için bazı en iyi uygulamaları kapsar.

Kişiler için Cosmos DB ile bilginiz, göz atın [Azure Cosmos DB'nin öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/documentdb/) kullanmaya başlamak için. 

> [!Note]
> Şu anda Azure Stream Analytics'i kullanarak Azure Cosmos DB bağlantı yalnızca destekler **SQL API**.
> Diğer Azure Cosmos DB API henüz desteklenmiyor. Noktası Azure Stream Analytics Azure Cosmos DB hesaplarına diğer API'lerle oluşturduysanız, verileri düzgün bir şekilde depolanabilir değil. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Bir çıkış hedefi olarak Cosmos DB ile ilgili temel bilgiler
Stream Analytics, Azure Cosmos DB çıktı sonuçları, Cosmos DB kapsayıcı JSON çıktısını işleme akışınız yazılmasını etkinleştirir. Stream Analytics, bunun yerine, ön maliyet oluşturmanızı gerektiren kapsayıcıları veritabanınızdaki oluşturmaz. Cosmos DB kapsayıcıları fatura maliyetlerini sizin tarafınızdan denetlenir ve böylece performans, tutarlılık ve kapsayıcılarınızı kullanarak doğrudan kapasitesini ayarlama budur [Cosmos DB API'leri](https://msdn.microsoft.com/library/azure/dn781481.aspx).

> [!Note]
> Azure Cosmos DB güvenlik duvarını 0.0.0.0 izin verilen IP listesine eklemeniz gerekir.

Cosmos DB kapsayıcısı seçeneklerden bazıları aşağıda açıklanmıştır.

## <a name="tune-consistency-availability-and-latency"></a>Tutarlılık, kullanılabilirlik ve gecikme süresini ayarlama
Uygulama gereksinimlerinize eşleştirmek için Azure Cosmos DB veritabanı ve kapsayıcıları ince ayar yapma ve tutarlılık, kullanılabilirlik, gecikme süresi ve aktarım hızı arasındaki denge sağlamak sağlar. Okuma tutarlılık düzeylerini bağlı olarak senaryo gereksinimlerinize yönelik Okuma ve yazma gecikme süresi, tutarlılık düzeyi veritabanı hesabınızı seçin. Aktarım hızı kapsayıcı üzerindeki istek Units(RUs) ölçeklendirme tarafından geliştirilebilir. Ayrıca varsayılan olarak, Azure Cosmos DB kapsayıcınız için her bir CRUD işlemi zaman uyumlu dizin oluşturmayı sağlar. Azure Cosmos DB'de yazma/okuma performans denetlemek için kullanışlı başka bir seçenek budur. Daha fazla bilgi için gözden [, veritabanı ve sorgu tutarlılık düzeylerini değiştirme](../cosmos-db/consistency-levels.md) makalesi.

## <a name="upserts-from-stream-analytics"></a>Stream Analytics'ten alınan upsert eder
Azure Cosmos DB ile Stream Analytics tümleştirmesi, eklemek veya kapsayıcınızda belirtilen bir belge kimliği sütunu temel alarak kayıtları güncelleştirmek sağlar. Bu da verilir bir *Upsert*.

Stream Analytics ile belge kimliği çakışması ekleme başarısız olduğunda burada güncelleştirmeleri yalnızca işiniz bir iyimser upsert yaklaşımı kullanır. Uyumluluk düzeyi 1.0 ile bu güncelleştirme düzeltme ekini, diğer bir deyişle belgeye kısmi güncelleştirmeler etkinleştirir, yeni özellikler veya varolan bir özellik kademeli olarak gerçekleştirilen değiştirme ek gerçekleştirilir. Ancak, değişiklikler, JSON belge sonucu üzerine tüm dizi diğer bir deyişle, dizi içinde dizi özelliklerin değerlerini değil birleştirilir. 1\.2 ile upsert davranışı eklemek veya belgeyi değiştirmek için değiştirildi. Bu uyumluluk düzeyi 1.2 bölümünde daha ayrıntılı açıklanmıştır.

Gelen JSON belgesini alan otomatik olarak Cosmos DB belge kimliği sütunu olarak kullanılır ve herhangi bir sonraki yazma, bu nedenle, bunlardan biri için önde gelen işlenir varolan bir kimliği alanı, varsa:
- eklemek için benzersiz bir kimlik sağlama
- Yinelenen kimlikleri ve 'ID' için ayarlanmış ' Belge Kimliği' upsert müşteri adayları
- Yinelenen kimlikleri ve 'Belge Kimliği' değil kümesi müşteri adayları hata için ilk belge sonra

Kaydetmek istiyorsanız <i>tüm</i> yinelenen bir Kimliğe sahip olanlar da dahil olmak üzere Belge Kimliği alanı sorgunuzda (AS anahtar sözcüğü ile) yeniden adlandırın ve Kimliği alanı oluşturun veya başka bir sütunun değeri ile Kimliğini değiştirin Cosmos DB sağlar (AS anahtar sözcüğü kullanılarak veya 'Belge Kimliği' ayarı kullanarak).

## <a name="data-partitioning-in-cosmos-db"></a>Cosmos DB'de bölümleme verileri
Azure Cosmos DB [sınırsız](../cosmos-db/partition-data.md) bölümler, iş yüküne göre ölçeklenen, verilerinizi otomatik Azure Cosmos DB bölümleme için önerilen yaklaşım kapsayıcı görevi görür. Sınırsız kapsayıcıların için yazma, Stream Analytics gibi çok sayıda paralel yazıcılar önceki sorgu veya girdisi'nden olay adımına bölümleme düzeni kullanır.
> [!NOTE]
> Şu anda Azure Stream Analytics yalnızca sınırsız kapsayıcılar bölüm anahtarları en üst düzeyde destekler. Örneğin, `/region` desteklenir. İç içe bölüm anahtarları (örneğin `/region/name`) desteklenmez. 

Kendi seçtiğiniz bölüm anahtarına bağlı olarak, bu alabilirsiniz _uyarı_:

`CosmosDB Output contains multiple rows and just one row per partition key. If the output latency is higher than expected, consider choosing a partition key that contains at least several hundred records per partition key.`

Çok sayıda farklı değerler sahiptir ve iş yükünüz bu değerleri arasında eşit bir şekilde dağıtmak olanak tanır bölüm anahtar özelliği seçmek önemlidir. Bölümleme doğal yapıt aynı bölüm anahtarını içeren istekleri tek bir bölüm en fazla aktarım hızı ile sınırlıdır. Ayrıca, depolama boyutu için aynı bölüm anahtarına ait belgeler için 10 GB ile sınırlıdır. İdeal bir bölüm anahtarı, çözümünüzün ölçeklenebilir olduğundan emin olmak için yeterli kardinalite sık sorgularınızı filtre olarak görünür ve biridir.

Bir bölüm anahtarı için Documentdb'nin saklı yordamları ve Tetikleyicileri işlemlerde de sınırıdır. Böylece aynı bölüm anahtarı değeri işlemlerde birlikte oluşan belgelerini de paylaşamaz bölüm anahtarı seçmeniz gerekir. Makaleyi [Cosmos DB'de bölümleme](../cosmos-db/partitioning-overview.md) bir bölüm anahtarı seçme hakkında daha fazla ayrıntı sağlar.

Sabit Azure Cosmos DB kapsayıcıları için Stream Analytics tam olduğunuzda artırmaya veya genişletmeye ölçeklendirme olanağı sağlar. 10 GB ve 10.000 RU/sn aktarım hızı üst sınır sahiptirler.  Sınırsız bir kapsayıcıya (örneğin, bir en az 1.000 RU/sn ve bölüm anahtarı) için sabit bir kapsayıcıdan verileri geçirmek için kullanmanız gerekir [veri geçiş aracı](../cosmos-db/import-data.md) veya [değişiklik akışı Kitaplığı](../cosmos-db/change-feed.md).

Birden çok sabit kapsayıcılara yazma olanağı kullanımdan kaldırılıyor ve Stream Analytics işinizi ölçeklendirme için önerilmez.

## <a name="improved-throughput-with-compatibility-level-12"></a>Uyumluluk düzeyi 1.2 ile iyi aktarım hızı
Stream Analytics destekler yerel tümleştirme toplu uyumluluk düzeyi 1.2 ile Cosmos DB'ye yazın. Bu Cosmos DB, aktarım hızı ve verimli bir şekilde tanıtıcı azaltma istekleri en üst düzeye ile etkili bir şekilde yazılmasını sağlar. Geliştirilmiş yazma mekanizması upsert bir davranışı fark nedeniyle yeni bir uyumluluk düzeyi altında kullanılabilir.  1\.2 önce upsert ekleyin veya belgeyi birleştirmek için bir davranıştır. 1\.2 ile upsert eder davranışı eklemek veya belgeyi değiştirmek için değiştirildi. 

1\.2 önce toplu işlem olarak yazıldığı, Cosmos DB içine toplu upsert belgelere bölüm anahtarı başına özel saklı yordam kullanır. Tek bir kaydı geçici bir hata (azaltma) ulaştığında, bile, tüm toplu işlem yeniden denenmelidir. Bu senaryolar bile makul nispeten daha yavaş azaltma ile yapılan. Karşılaştırma aşağıdaki gibi işler 1.2 ile nasıl davranacaktır gösterir.

Aşağıdaki örnek, aynı olay hub'ı girişten okuma iki özdeş Stream Analytics işi gösterir. Her iki Stream Analytics işleri, [tam olarak bölümlenmiş](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) geçiş sorgu ve aynı CosmosDB kapsayıcılarına yazma. Uyumluluk düzeyi 1.0 ile yapılandırılmış iş ölçümleri soldaki arasındadır ve sağ taraftaki olanlara 1.2 ile yapılandırılır. Bir Cosmos DB kapsayıcının bölüm anahtarı, giriş olaydan gelen benzersiz bir GUID değeridir.

![Stream analytics ölçümleri karşılaştırma](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Olay Hub'ındaki gelen olay hızı azaltma Cosmos DB'de beklendiği şekilde Cosmos DB kapsayıcıları (20 bin RU) işleme için yapılandırılmış olan çok daha yüksek x 2 ' dir. Ancak, 1.2, iş tutarlı bir şekilde yazmak daha yüksek bir aktarım hızı (çıkış olay/dakika) ve bir alt ortalama SU kullanım yüzdesi. Ortamınızda bu fark üzerinde birkaç seçenek olay biçimi, giriş olay iletisi boyut, bölüm anahtarları, sorgu vb. gibi daha fazla faktörlere bağlıdır.

![cosmos db ölçümleri karşılaştırma](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

1\.2 ile Stream Analytics'in Cosmos DB'de kullanılabilen aktarım hızı azaltma hız sınırlaması gelen çok az sayıda resubmissions ile % 100'ü kullanarak daha akıllı bulunur. Bu sorguları aynı anda kapsayıcıdaki çalıştırma gibi diğer iş yükleri için daha iyi bir deneyim sağlar. Bir havuz için 1 k için 10 k iletiler/saniye olarak nasıl ASA kullanıma Cosmos DB ile ölçeklenir kullanıma denemek gerektiği durumlarda, işte bir [azure örnekleri proje](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) olanak tanıyan, yapın.
Cosmos DB çıkış aktarım hızı 1.0 ve 1.1 ile aynı olduğunu unutmayın. 1\.2 şu anda varsayılan olmadığından yapabilecekleriniz [uyumluluk düzeyi ayarlayın](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) portalını kullanarak ya da kullanarak bir Stream Analytics işine ilişkin [oluşturma işi REST API çağrısı](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Sahip *önemle tavsiye* Cosmos DB ile uyumluluk düzeyini 1.2 ASA ile kullanılacak. 



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
|Kapsayıcı adı | Kullanılacak kapsayıcı adı. `MyContainer` bir örnek geçerli bir giriş - adlı bir kapsayıcı `MyContainer` mevcut olması gerekir.  |
|Belge Kimliği     | İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayanması benzersiz bir anahtar kullanılan çıkış olaylarındaki sütun adı. Boş bırakılırsa, tüm olayları güncelleştirme seçeneği ile eklenir.|
