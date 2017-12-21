---
title: "Akış analizi için JSON çıktısını | Microsoft Docs"
description: "Stream Analytics Veri arşivleme ve düşük gecikme süreli sorguları yapılandırılmamış JSON verileri için JSON çıktı için Azure Cosmos DB nasıl hedefleyebilirsiniz öğrenin."
keywords: "JSON çıktısını"
documentationcenter: 
services: stream-analytics,documentdb
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 5d2a61a6-0dbf-4f1b-80af-60a80eb25dd1
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeanb
ms.openlocfilehash: 29be0f5100aabe8374a26e6548effe20ccb9ac86
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="target-azure-cosmos-db-for-json-output-from-stream-analytics"></a>Hedef Azure Stream Analytics JSON çıktısını Cosmos DB
Akış analizi hedef [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) veri arşivleme ve düşük gecikme süreli sorguları yapılandırılmamış JSON verileri JSON çıktısını için etkinleştirme. Bu belgede, bu yapılandırmayı uygulamak için bazı en iyi yöntemler kapsar.

Kişiler için Cosmos DB ile iyi tanımıyorsanız, bir göz atalım [Azure Cosmos veritabanı öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/documentdb/) başlamak için. 

> [!Note]
> Şu anda Azure akış analizi yalnızca CosmosDB kullanarak bağlantı destekler **SQL API**.
> Diğer Azure Cosmos DB API'leri henüz desteklenmiyor. Noktası Azure akış analizi Azure Cosmos DB hesaplarına diğer API'leri ile oluşturduysanız, verilerin düzgün depolanabilir değil. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Cosmos DB bir çıktı hedefi olarak temelleri
Stream Analytics Azure Cosmos DB çıktısında sonuçları, Cosmos DB collection(s) JSON çıkış olarak işleme akışınızı yazılmasını etkinleştirir. Akış analizi koleksiyonları, veritabanınızda, bunun yerine, bunları önceden oluşturmak gerektiren oluşturmaz. Cosmos DB koleksiyonların fatura maliyetleri size saydam; böylece ve böylece performans, tutarlılık ve kapasite kullanarak doğrudan koleksiyonlarınızı ince ayar budur [Cosmos DB API'leri](https://msdn.microsoft.com/library/azure/dn781481.aspx). Koleksiyonunuz için bir iş akışında mantıksal olarak ayırmak için iş akışında başına tek bir Cosmos DB veritabanı kullanmanızı öneririz.

Cosmos DB toplama seçeneklerini bazıları aşağıda açıklanmıştır.

## <a name="tune-consistency-availability-and-latency"></a>Tutarlılık, kullanılabilirlik ve gecikme süresini ayarlama
Uygulama gereksinimlerinizi eşleşecek şekilde Cosmos DB ince koleksiyonları ve veritabanı ayarlamak ve dengelemeler tutarlılık, kullanılabilirlik ve gecikme süresi arasında yapmak sağlar. Hangi düzeyde okuma tutarlılığı bağlı olarak senaryo gereksinimlerinize göre okuma ve veritabanı hesabınızdaki tutarlılık düzeyi seçebilirsiniz gecikme, yazma. Ayrıca varsayılan olarak, zaman uyumlu her CRUD işlemi koleksiyonunuz için dizin Cosmos DB sağlar. Cosmos DB yazma/okuma performans denetlemek için yararlı başka bir seçenek budur. Bu konu hakkında daha fazla bilgi için gözden [, veritabanı ve sorgu tutarlılık düzeylerini değiştirme](../cosmos-db/consistency-levels.md) makalesi.

## <a name="upserts-from-stream-analytics"></a>Stream analytics'ten Upserts
Stream Analytics tümleştirme Cosmos DB ile eklemek veya belirli bir belge kimliği sütununa dayalı Cosmos DB koleksiyonunuzdaki kayıtlarını güncelleştirmek sağlar. Bu ayrıca olarak adlandırılır bir *Upsert*.

Akış analizi Ekle bir belge kimliği çakışma nedeniyle başarısız olduğunda burada güncelleştirmeleri yalnızca yapılır bir iyimser Upsert yaklaşımı kullanır. Bu güncelleştirme, yani yeni özellikleri veya mevcut bir özellik artımlı olarak gerçekleştirilen değiştirme eklenmesi belgede kısmi güncelleştirmeleri etkinleştirir şekilde akış analizi tarafından bir düzeltme eki gerçekleştirilir. Dizideki üzerine tüm yani dizi JSON belgesi sonucundaki dizi özelliklerinin değerlerini değişiklikleri Not birleştirilmedi.

## <a name="data-partitioning-in-cosmos-db"></a>Cosmos DB'de veri bölümlendirme
Cosmos DB [bölümlenmiş koleksiyonlar](../cosmos-db/partition-data.md) verilerinizi bölümleme için önerilen yaklaşım şunlardır. 

Akış analizi, tek Cosmos DB koleksiyonlar için hem sorgu desenlerine hem de, uygulamanızın performans gereksinimlerine göre verilerinizi bölümlemek hala sağlar. Her koleksiyon 10 GB veri (maksimum) içerebilir ve şu anda Yukarı (veya taşması koleksiyonuna) bir yolu yoktur. Ölçeğini için akış analizi, belirli bir önek ile birden çok koleksiyon yazmak sağlar (aşağıda kullanım ayrıntılarını bakın). Stream Analytics kullanır tutarlı [karma bölüm çözümleyici](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) kullanıcıyı temel alarak stratejisi sağlanan çıkış kayıtlarını bölümlemek için PartitionKey sütun. Belirtilen öneke sahip bir koleksiyon akış işin başlangıç saatinde sayısı, iş yazacağı için paralel olarak çıkış bölüm sayısı olarak kullanılır (Cosmos DB koleksiyonları çıkış bölümleri =). Yavaş dizin ile tek bir koleksiyon için yalnızca eklemeleri yapılması, MB/sn üretilen iş yazma 0.4 hakkında beklenebilir. Birden çok koleksiyonları kullanarak, daha yüksek verimlilik ve daha yüksek kapasite elde etmek izin verebilirsiniz.

Bölüm sayısı gelecekte artırmak istiyorsanız, işi durdurma, varolan koleksiyonlarınızı yeni koleksiyonlara verileri bölümlendirmek ve akış analizi işi yeniden başlatmak gerekebilir. Örnek kod ile birlikte yeniden bölümlendirme ve PartitionResolver kullanma hakkında daha fazla bilgi dahil izleme postasına edilir. Makaleyi [bölümleme ve Cosmos DB'de ölçeklendirme](../cosmos-db/sql-api-partition-data.md) de bu ayrıntıları sağlar.

## <a name="cosmos-db-settings-for-json-output"></a>JSON çıktısını cosmos DB ayarları
Stream Analytics bir çıkış olarak Cosmos DB oluşturma aşağıda görüldüğü gibi bilgileri için bir istem oluşturur. Bu bölümde özellikler tanımının bir açıklama sağlar.

Bölümlenmiş koleksiyonu | Birden çok "Tek bölüm" koleksiyonları
---|---
![documentdb stream analytics çıkış ekranı](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png) |  ![documentdb stream analytics çıkış ekranı](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)


  
> [!NOTE]
> **Birden çok "Tek bölüm" koleksiyonları** senaryo bölüm anahtarı gerektirir ve desteklenen bir yapılandırmadır. 

* **Diğer ad çıktı** – Bu çıktı ASA Sorgunuzdaki başvurmak için diğer bir  
* **Hesap adı** – adını veya bitiş noktası Cosmos DB hesabının URI'si.  
* **Anahtar hesap** – Cosmos DB hesabı için paylaşılan erişim anahtarı.  
* **Veritabanı** – Cosmos DB veritabanı adı.  
* **Koleksiyon adı deseni** – koleksiyon adını veya kendi deseni kullanılacak koleksiyonlar için. Koleksiyon adı biçimi, burada bölümlerin 0'dan başlar isteğe bağlı {partition} belirteci kullanılarak oluşturulabilir. Örnek geçerli girişler şunlardır:  
  1\) MyCollection – "MyCollection" adlı bir koleksiyon bulunmalıdır.  
  2\) – böyle koleksiyonları bulunmalıdır – MyCollection {partition} "MyCollection0", "MyCollection1", "MyCollection2" ve benzeri.  
* **Anahtar bölüm** – isteğe bağlıdır. Bu, yalnızca, koleksiyon adı deseni {partition} belirteci kullanıyorsanız gereklidir. Çıkışın koleksiyonlar üzerinde bölümlenmesine yönelik anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı. Tek koleksiyon çıktı için herhangi bir rastgele çıkış sütunu olabilir örneğin PartitionID kullanılır.  
* **Belge Kimliği** – isteğe bağlıdır. Hangi ekleme veya güncelleştirme işlemleri dayalı birincil anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı.  
