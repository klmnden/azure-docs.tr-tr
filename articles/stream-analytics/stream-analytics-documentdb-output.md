---
title: Cosmos DB Azure Stream Analytics çıkışı
description: Bu makalede Azure akış analizi veri arşivleme ve düşük gecikme süreli sorguları yapılandırılmamış JSON verileri için JSON çıktı için çıktı Azure Cosmos DB'sine kaydetmek için nasıl kullanılacağını açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: ff2071a703b0b5e94cd68122a878b51e9d97669a
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112760"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Stream Analytics çıktı Azure Cosmos DB'de  
Akış analizi hedef [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) veri arşivleme ve düşük gecikme süreli sorguları yapılandırılmamış JSON verileri JSON çıktısını için etkinleştirme. Bu belgede, bu yapılandırmayı uygulamak için bazı en iyi yöntemler kapsar.

Kişiler için Cosmos DB ile iyi tanımıyorsanız, bir göz atalım [Azure Cosmos veritabanı öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/documentdb/) başlamak için. 

> [!Note]
> Şu anda Azure akış analizi yalnızca Azure Cosmos DB kullanarak bağlantı destekler **SQL API**.
> Diğer Azure Cosmos DB API'leri henüz desteklenmiyor. Noktası Azure akış analizi Azure Cosmos DB hesaplarına diğer API'leri ile oluşturduysanız, verilerin düzgün depolanabilir değil. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Cosmos DB bir çıktı hedefi olarak temelleri
Stream Analytics Azure Cosmos DB çıktısında sonuçları, Cosmos DB collection(s) JSON çıkış olarak işleme akışınızı yazılmasını etkinleştirir. Akış analizi, bunun yerine, bunları önceden oluşturmak gerektiren koleksiyonlar veritabanınızdaki oluşturmaz. Böylece Cosmos DB koleksiyonların fatura maliyetleri sizin tarafınızdan denetlenir ve böylece performans, tutarlılık ve kapasite kullanarak doğrudan koleksiyonlarınızı ince ayar budur [Cosmos DB API'leri](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

Cosmos DB toplama seçeneklerini bazıları aşağıda açıklanmıştır.

## <a name="tune-consistency-availability-and-latency"></a>Tutarlılık, kullanılabilirlik ve gecikme süresini ayarlama
Uygulama gereksinimlerinizi eşleşecek şekilde Azure Cosmos DB ince koleksiyonları ve veritabanı ayarlamak ve dengelemeler tutarlılık, kullanılabilirlik ve gecikme süresi arasında yapmak sağlar. Hangi düzeyde okuma tutarlılığı bağlı olarak senaryo gereksinimlerinize göre okuma ve veritabanı hesabınızdaki tutarlılık düzeyi seçebilirsiniz gecikme, yazma. Ayrıca varsayılan olarak, zaman uyumlu her CRUD işlemi koleksiyonunuz için dizin Azure Cosmos DB sağlar. Azure Cosmos veritabanı yazma/okuma performans denetlemek için yararlı başka bir seçenek budur. Daha fazla bilgi için gözden [, veritabanı ve sorgu tutarlılık düzeylerini değiştirme](../cosmos-db/consistency-levels.md) makalesi.

## <a name="upserts-from-stream-analytics"></a>Stream analytics'ten Upserts
Stream Analytics tümleştirme Azure Cosmos DB ile eklemek veya belirli bir belge kimliği sütununa dayalı koleksiyonunuzdaki kayıtlarını güncelleştirmek sağlar. Bu ayrıca olarak adlandırılır bir *Upsert*.

Akış analizi, bir belge kimliği çakışma ekleme başarısız olduğunda burada güncelleştirmeleri yalnızca yapılır bir iyimser upsert yaklaşımı kullanır. Bu güncelleştirme, düzeltme eki, başka bir deyişle belgeye kısmi güncelleştirmeler sağlar şekilde, yeni özellikleri veya mevcut bir özellik artımlı olarak gerçekleştirilen değiştirme gerçekleştirilir. Ancak, dizideki üzerine, tüm diğer bir deyişle, dizi JSON belgesi sonucundaki dizi özelliklerinin değerlerini değişiklikleri değil birleştirilir.

## <a name="data-partitioning-in-cosmos-db"></a>Cosmos DB'de veri bölümlendirme
Azure Cosmos DB [sınırsız](../cosmos-db/partition-data.md) , verilerinizi Azure Cosmos DB otomatik olarak bölümlendirme, iş yüküne göre bölümleri ölçekler için önerilen yaklaşım. Sınırsız kapsayıcılara yazarken, Stream Analytics sayıda paralel yazıcılarının önceki sorgu adımı veya bölümleme giriş kullanır.

Sabit Azure Cosmos DB koleksiyonlar için Stream Analytics tam bir kez veya ölçeklendirme olanağı sağlar. Bir üst sınır 10 GB ve 10. 000'ru / s verimlilik sahiptirler.  Bir sınırsız kapsayıcısına (örneğin, bir en az 1.000 RU/s ve bölüm anahtarı) sabit bir kapsayıcı verileri geçirmek için kullanmanız gerekir [veri geçiş aracı](../cosmos-db/import-data.md) veya [değişiklik akış Kitaplığı](../cosmos-db/change-feed.md).

Birden çok sabit kapsayıcı yazma kullanım dışıdır ve, Stream Analytics işi ölçeklendirmeye yönelik önerilen yaklaşım değildir. Makaleyi [bölümleme ve Cosmos DB'de ölçeklendirme](../cosmos-db/sql-api-partition-data.md) daha fazla ayrıntı sağlar.

## <a name="cosmos-db-settings-for-json-output"></a>JSON çıktısını cosmos DB ayarları
Stream Analytics bir çıkış olarak Cosmos DB oluşturma aşağıda görüldüğü gibi bilgileri için bir istem oluşturur. Bu bölümde özellikler tanımının bir açıklama sağlar.


![documentdb stream analytics çıkış ekranı](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

Alan           | Açıklama 
-------------   | -------------
Çıkış Diğer Adı    | Bu çıktı ASA Sorgunuzdaki başvurmak için bir diğer ad   
Hesap Adı    | Adı veya uç nokta Azure Cosmos DB hesabının URI 
Hesap Anahtarı     | Azure Cosmos DB hesabı için paylaşılan erişim anahtarı
Database        | Azure Cosmos DB veritabanı adı
Koleksiyon Adı | Kullanılacak koleksiyonu için koleksiyon adı. `MyCollection` Örnek geçerli bir giriş - adlı bir koleksiyon olduğu `MyCollection` mevcut olması gerekir.  
Belge Kimliği     | İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayanmalıdır benzersiz anahtar olarak kullanılan çıkış olaylarındaki sütun adı. Boş bırakılırsa, tüm olayları hiçbir güncelleştirme seçeneğiyle eklenir.
