---
title: Cosmos DB için Azure Stream Analytics çıkışı
description: Bu makalede, çıkış veri arşivleme ve düşük gecikme süreli sorgular yapılandırılmamış JSON verileri için bir JSON çıkışı için Azure Cosmos DB'ye kaydetmek için Azure Stream Analytics kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 11/21/2017
ms.openlocfilehash: 9bdb012db2e7502d765fd342a636591bbbcb2c6c
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311747"
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
> 0.0.0.0 izin verilen IP listesi için Azure Cosmos DB Güvenlik Duvarı'nı eklemeniz gerekir.

Cosmos DB koleksiyonu seçeneklerden bazıları aşağıda açıklanmıştır.

## <a name="tune-consistency-availability-and-latency"></a>Tutarlılık, kullanılabilirlik ve gecikme süresini ayarlama
Uygulama gereksinimlerinize eşleştirmek için ince ayar koleksiyonu ve veritabanı ayarlama ve tutarlılık, kullanılabilirlik ve gecikme süresi arasındaki denge sağlamak Azure Cosmos DB sağlar. Okuma tutarlılık düzeylerini bağlı olarak senaryo gereksinimlerinize yönelik Okuma ve yazma gecikme süresi, tutarlılık düzeyi veritabanı hesabınızı seçin. Ayrıca varsayılan olarak, Azure Cosmos DB koleksiyonunuza her CRUD işlemi zaman uyumlu dizin oluşturmayı etkinleştirir. Azure Cosmos DB'de yazma/okuma performans denetlemek için kullanışlı başka bir seçenek budur. Daha fazla bilgi için gözden [, veritabanı ve sorgu tutarlılık düzeylerini değiştirme](../cosmos-db/consistency-levels.md) makalesi.

## <a name="upserts-from-stream-analytics"></a>Stream Analytics'ten alınan upsert eder
Azure Cosmos DB ile Stream Analytics tümleştirmesi ekleme veya güncelleştirme belirli bir belge kimliği sütunu temel alarak koleksiyonunuzdaki kayıtlarını sağlar. Bu da verilir bir *Upsert*.

Stream Analytics ile belge kimliği çakışması ekleme başarısız olduğunda burada güncelleştirmeleri yalnızca işiniz bir iyimser upsert yaklaşımı kullanır. Bu güncelleştirme, düzeltme ekini, diğer bir deyişle belgeye kısmi güncelleştirmeler etkinleştirir, yeni özellikler veya varolan bir özellik kademeli olarak gerçekleştirilen değiştirme ek gerçekleştirilir. Ancak, değişiklikler, JSON belge sonucu üzerine tüm dizi diğer bir deyişle, dizi içinde dizi özelliklerin değerlerini değil birleştirilir.

Gelen JSON belgesini alan otomatik olarak Cosmos DB belge kimliği sütunu olarak kullanılır ve herhangi bir sonraki yazma, bu nedenle, bunlardan biri için önde gelen işlenir varolan bir kimliği alanı, varsa:
- eklemek için benzersiz bir kimlik sağlama
- Yinelenen kimlikleri ve 'ID' için ayarlanmış ' Belge Kimliği' upsert müşteri adayları
- Yinelenen kimlikleri ve 'Belge Kimliği' değil kümesi müşteri adayları hata için ilk belge sonra

Kaydetmek istiyorsanız <i>tüm</i> yinelenen bir Kimliğe sahip olanlar da dahil olmak üzere Belge Kimliği alanı sorgunuzda (AS anahtar sözcüğü ile) yeniden adlandırın ve Kimliği alanı oluşturun veya başka bir sütunun değeri ile Kimliğini değiştirin Cosmos DB sağlar (AS anahtar sözcüğü kullanılarak veya 'Belge Kimliği' ayarı kullanarak).

## <a name="data-partitioning-in-cosmos-db"></a>Cosmos DB'de bölümleme verileri
Azure Cosmos DB [sınırsız](../cosmos-db/partition-data.md) bölümler, iş yüküne göre ölçeklenen, verilerinizi otomatik Azure Cosmos DB bölümleme için önerilen yaklaşım. Sınırsız kapsayıcılar için yazarken, Stream Analytics sayıda paralel yazıcılar önceki bir sorgu adımına veya bölümleme düzeni giriş kullanır.
> [!Note]
> Şu anda Azure Stream Analytics yalnızca sınırsız koleksiyonu en üst düzeyde bölüm anahtarları ile destekler. Örneğin, `/region` desteklenir. İç içe bölüm anahtarları (örneğin `/region/name`) desteklenmez. 

Sabit Azure Cosmos DB koleksiyonları için Stream Analytics tam olduğunuzda artırmaya veya genişletmeye ölçeklendirme olanağı sağlar. 10 GB ve 10.000 RU/sn aktarım hızı üst sınır sahiptirler.  Sınırsız bir kapsayıcıya (örneğin, bir en az 1.000 RU/sn ve bölüm anahtarı) için sabit bir kapsayıcıdan verileri geçirmek için kullanmanız gerekir [veri geçiş aracı](../cosmos-db/import-data.md) veya [değişiklik akışı Kitaplığı](../cosmos-db/change-feed.md).

Birden çok sabit kapsayıcı yazma kullanımdan kaldırılıyor ve Stream Analytics işinizi ölçeklendirmeye yönelik önerilen yaklaşım değildir. Makaleyi [bölümleme ve ölçeklendirme Cosmos DB'de](../cosmos-db/sql-api-partition-data.md) hakkında daha fazla ayrıntı sağlar.

## <a name="cosmos-db-settings-for-json-output"></a>Çıkış JSON için cosmos DB ayarları
Stream analytics'te bir çıkış olarak Cosmos DB oluşturma, aşağıda görüldüğü gibi bilgileri için bir istem oluşturur. Bu bölüm, özellikleri tanımının bir açıklama sağlar.


![documentdb stream analytics çıkış ekranı](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

Alan           | Açıklama 
-------------   | -------------
Çıktı Diğer Adı    | Bu çıkış ASA sorgunuzda başvurmak için bir diğer ad   
Hesap Adı    | Adı veya URI'si Azure Cosmos DB hesabının uç noktası 
Hesap Anahtarı     | Azure Cosmos DB hesabı için paylaşılan erişim anahtarı
Database        | Azure Cosmos DB veritabanı adı
Koleksiyon Adı | Kullanılacak bir koleksiyon için koleksiyon adı. `MyCollection` Örnek geçerli bir giriş - adlı bir koleksiyon olduğundan `MyCollection` mevcut olması gerekir.  
Belge Kimliği     | İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayanması benzersiz bir anahtar kullanılan çıkış olaylarındaki sütun adı. Boş bırakılırsa, tüm olayları güncelleştirme seçeneği ile eklenir.
