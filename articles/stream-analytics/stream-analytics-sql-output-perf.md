---
title: Azure SQL veritabanı için Azure Stream Analytics çıkışı
description: SQL Azure için Azure Stream Analytics'ten veri çıktısı hakkında bilgi edinin ve daha yüksek yazma aktarım hızı oranlarına ulaşamayabilir.
services: stream-analytics
author: chetang
ms.author: chetang
manager: katicad
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/21/2018
ms.openlocfilehash: 794e2f3db44c29707400f96970159578d9e83f2d
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54303284"
---
# <a name="azure-stream-analytics-output-to-azure-sql-database"></a>Azure SQL veritabanı için Azure Stream Analytics çıkışı

Bu makalede, Azure Stream Analytics'i kullanarak SQL Azure veritabanına veri yükleme zaman daha iyi yazma aktarım hızı performansı elde etmek için ipuçları açıklanır.

Azure Stream analytics'te SQL çıkış seçeneği olarak paralel yazılmasını destekler. Bu seçenek verir [tam olarak paralel](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) iş yeri birden çok çıkış bölüm yazma paralel hedef tabloya topolojiler,. Azure Stream analytics'te bu seçeneğin etkinleştirilmesi ancak tablo şemasını ve SQL Azure veritabanı yapılandırması üzerinde önemli ölçüde bağlı olarak daha yüksek aktarım hızı elde etmek yeterli olmayabilir. Dizinler, anahtar ve dizini Doldurma faktörü sıkıştırma Kümelemesi, tercih ettiğiniz tablolarından yükleme zamanında bir etkisi vardır. SQL Azure veritabanınızın sorgu performansını ve iç ölçümlerinde temel performans yüklemek için en iyi duruma getirme hakkında daha fazla bilgi için bkz. [SQL database performans rehberi](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance). Yazmayı sıralama paralel SQL Azure veritabanına yazılırken garanti edilmez.

Çözümünüzün genel performansını iyileştirmeye yardımcı olabilecek her hizmet içinde bazı yapılandırmalar şunlardır.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

- **Bölümleme devral** – bölümleme düzeni önceki bir sorgu adımına veya giriş devralan bu SQL çıktı yapılandırma seçeneği sağlar. Bu etkinleştirildiğinde, disk tabanlı tablo için yazma ve sahip bir [tam olarak paralel](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) , işiniz için topoloji beklediğiniz daha iyi aktarım hızı görmek. Bu bölümleme zaten otomatik olarak için diğer birçok olur [çıkarır](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#partitions-in-sources-and-sinks). Tablo (TABLOCK) kilitleme de bu seçenekle yapılan toplu eklemeler için devre dışı.

> [!NOTE] 
> 8'den fazla giriş bölümler olduğunda, bölümleme düzeni giriş devralan uygun bir seçim olmayabilir. Bu üst sınırı, bir tek bir kimlik sütunu ve kümelenmiş bir dizin olan bir tabloda gözlemlendi. Şema ve dizinleri, tercih ettiğiniz bağlı olarak, uygulamanızın gözlemler farklılık gösterebilir.

- **Yığın boyutu** -SQL çıkış yapılandırması hedef tablo/iş yükünüz doğası hakkında temel bir Azure Stream Analytics SQL çıkışında en yüksek toplu iş boyutu belirtmenize olanak verir. Toplu iş boyutu en fazla gönderilen her toplu ile kayıt sayısı işlem Ekle ' dir. Kümelenmiş columnstore dizinlerinde toplu etrafında boyutları [100 bin cinsinden](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-data-loading-guidance) daha fazla paralelleştirme, en az günlüğe kaydetme ve en iyi duruma getirme kilitleme için izin verilir. Daha yüksek toplu iş boyutu toplu ekleme sırasında kilit azaltımı tetikleyebilir disk tabanlı tablolarda, 10 K (varsayılan) veya alt çözümünüz için en iyi olabilir.

- **Giriş iletisi ayarlama** – kullanılarak iyileştirilmiş bölümleme ve toplu işlem boyutu devralır, giriş olayları her bölüm başına ileti sayısını artırmak daha fazla yazma düzeyinizi dağıtmaya yardımcı olur. Giriş iletisi ayarlama, belirtilen Batch böylece üretimini geliştirme boyuta kadar Azure Stream Analytics içinde batch boyutlara izin verir. Bu kullanarak gerçekleştirilebilir [sıkıştırma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-inputs) ya da daha büyük ileti boyutları Premium EventHub SKU'da kullanılabilir.

## <a name="sql-azure"></a>SQL Azure

- **Tablo ve dizinleri bölümlenmiş** – kullanarak bir [bölümlenmiş](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes?view=sql-server-2017) SQL tablosunu ve bölümlenmiş dizinlerde tabloyla aynı sütun (örneğin, PartitionID), bölüm anahtarı olarak önemli ölçüde azaltabilirsiniz arasındaki çakışmaları yazma sırasında bölümleri. Bölümlenmiş bir tablodaki oluşturmanız gerekecek bir [bölümleme işlevinin](https://docs.microsoft.com/sql/t-sql/statements/create-partition-function-transact-sql?view=sql-server-2017) ve [bölüm düzeni](https://docs.microsoft.com/sql/t-sql/statements/create-partition-scheme-transact-sql?view=sql-server-2017) birincil dosya çubuğunda. Yeni veriler yüklenirken bu var olan veri kullanılabilirliğini artırır. Günlük g/ç sınırı SKU yükselterek artırılabilir bölüm sayısına göre karşılaşabilirsiniz.

- **Benzersiz anahtar ihlalini önlemek** – alırsanız [birden çok anahtar ihlali uyarı iletilerini](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-common-troubleshooting-issues#handle-duplicate-records-in-azure-sql-database-output) Azure Stream Analytics etkinlik günlüğü'nde işinizi olması olasılığı olan benzersiz kısıtlama ihlali tarafından etkilenen olmadığından emin olun. Kurtarma çalışmaları sırasında. Bu ayarlayarak önlenebilir [Yoksay\_DUP\_anahtar](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-common-troubleshooting-issues#handle-duplicate-records-in-azure-sql-database-output) dizinlerinizi seçeneği.

## <a name="azure-data-factory-and-in-memory-tables"></a>Azure Data Factory ve bellek içi tablolar

- **Bellek içi tablosu geçici tablo olarak** – [bellek içi tablolar](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) çok yüksek hızlı veri yükler için izin ver, ancak veri belleğe sığması gerekir. Disk tabanlı tablo için bir bellek içi tablosundan Kıyaslama show toplu yükleme kıyasla yaklaşık 10 kat daha hızlı bir tek yazıcı disk tabanlı tablo bir kimlik sütunu ve bir kümelenmiş dizin kullanarak ekleme doğrudan toplu. Bu toplu INSERT performans yararlanmak üzere ayarlanan sahte bir [kullanarak Azure Data Factory kopyalama işini](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database) , veri bellek içi tablosundan disk tabanlı tablo için kopyalar.

## <a name="summary"></a>Özet

Özet olarak, SQL çıkış için Azure Stream analytics'te bölümlenmiş çıkış özelliğiyle hizalanmış paralelleştirme ile SQL Azure bölümlenmiş bir tablodaki işinizin size önemli performans geliştirmeleri vermesi gerekir. Disk tabanlı tablolara bir bellek içi tablodan veri taşıma işlemlerini için Azure Data Factory yararlanarak, büyüklük sırası üretilen iş kazanımlarını verebilirsiniz. Uygulanabiliyorsa ileti yoğunluklu iyileştirme de genel üretimini geliştirme önemli bir etken olabilir.
