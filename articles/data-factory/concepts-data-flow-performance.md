---
title: Azure Data Factory'de veri akışını performans eşleme ve ayarlama Kılavuzu | Microsoft Docs
description: Eşleme bir veri akışı kullandığınızda, Azure Data factory'deki veri akışlarının performansını etkileyen anahtar Etkenler hakkında bilgi edinin.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: 7fca586083f70e0b0f7e593d5203392260cd2136
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172348"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Eşleme veri akışları performansı ve ayarlama Kılavuzu

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory eşleme veri akışları tasarlama, dağıtma ve uygun ölçekte veri dönüşümleri düzenlemek için bir tarayıcı kod gerektirmeyen arabirim sağlar.

> [!NOTE]
> Genel ADF eşleme veri akışları ile ilgili bilgi sahibi değilseniz, bkz. [veri akışlarına genel bakış](concepts-data-flow-overview.md) bu makaleyi okuduktan önce.
>

> [!NOTE]
> Tasarlama ve veri akışları ADF UI sınama veri akışlarınızı çalıştırabilmeniz için hata ayıklama anahtarı Isınma bir küme için beklemenize gerek kalmadan gerçek zamanlı etkinleştirmek emin olun.
>

![Düğme hata ayıklama](media/data-flow/debugb1.png "hata ayıklama")

## <a name="optimizing-for-azure-sql-database"></a>Azure SQL veritabanı için en iyi duruma getirme

![Kaynak bölümü](media/data-flow/sourcepart2.png "kaynak bölümü")

### <a name="you-can-match-spark-data-partitioning-to-your-source-database-partitioning-based-on-a-database-table-column-key-in-the-source-transformation"></a>Spark veri, kaynak veritabanı için bölümlendirme kaynak dönüşümü bir veritabanı tablosu sütunu anahtarında göre eşleşebilir.

* "En"İyileştir gidin ve "Kaynak"'ı seçin. Belirli bir tablo sütunu ya da bir tür bir sorgu ayarlayın.
* Ardından "sütun" i seçerseniz, bölüm sütunu seçin.
* Ayrıca, Azure SQL DB için en fazla bağlantı sayısını ayarlayın. Veritabanınıza paralel bağlantıları sağlamak için daha yüksek bir ayar deneyebilirsiniz. Ancak, bazı durumlarda daha hızlı performans bağlantıları sınırlı sayıda ile sonuçlanabilir.

### <a name="set-batch-size-and-query-on-source"></a>Toplu iş boyutu ve sorgu kaynağında ayarlayın

![Kaynak](media/data-flow/source4.png "kaynak")

* Toplu iş boyutu ayarı-satır yerine bellekte kümelerinde veri depolamak için ADF yenilemelerini ister. İsteğe bağlı bir ayardır ve doğru boyutlandırılmadığında bırakılırsa kaynaklar yetersiz ve işlem düğümleri üzerinde çalışabilir.
* Bir sorgu ayarlama, hatta veri akışı için daha hızlı ilk veri alım yapabilirsiniz işlenmek ulaştıkları önce kaynak satırları sağ filtrelemek izin verebilirsiniz.
* Bir sorgu kullanırsanız, Azure SQL DB için yani READ UNCOMMITTED isteğe bağlı bir sorgu ipuçları ekleyebilirsiniz.

### <a name="set-sink-batch-size"></a>Havuz batch boyutunu ayarlama

![Havuz](media/data-flow/sink4.png "havuz")

* Veri floes satır tarafından işlenmesini önlemek için Azure SQL DB için havuz ayarları "toplu iş boyutu" ayarlayın. Bu işlem, sağlanan boyutuna göre toplu işlem veritabanına ADF Yazar bildirir.

### <a name="set-partitioning-options-on-your-sink"></a>Bölümleme havuzunuzu seçeneklerini ayarlayın

* Verilerinizi hedef Azure SQL DB tabloları bölümlenmiş yoksa bile İyileştir sekmesine gidin ve bölümleme ayarlayın.
* Çok sık, daha hızlı bir şekilde veri tek bir düğüm/bölüm gelen tüm bağlantıları zorlamak yerine yükleme sonuçları Spark yürütme kümelerinde bölümleme hepsini kullanmak için ADF yalnızca bildiriyor.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Azure Integration Runtime, bilgi işlem altyapısı boyutunu artırın

![Yeni IR](media/data-flow/ir-new.png "yeni IR")

* Düğümlerin sayısını artırmak ve sorgu ve Azure SQL DB'ye yazılmak üzere daha fazla işlemci gücü sağlamak, çekirdek sayısını artırın.
* Daha fazla kaynak, işlem düğümlerine uygulamak için "İşlem için iyileştirilmiş" ve "Bellek için iyileştirilmiş" seçenekleri deneyin.

### <a name="disable-indexes-on-write"></a>Dizin üzerinde yazma devre dışı bırak
* Havuzunuzu yazıldığını, hedef tablolarda dizinler devre dışı bırakır, veri akışı etkinliği önce bir ADF işlem hattı depolanan yordam etkinliği kullanın.
* Veri akışı, etkinlik bu dizinleri etkinleştirilmiş başka bir saklı yordam etkinliği ekleyin.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Azure SQL DB boyutunu artırın
* Yeniden boyutlandırma kaynağını zamanlayabilir ve Azure SQL DB, aktarım hızını artırmak ve DTU ulaştıktan sonra Azure azaltma en aza indirmek için işlem hattınızı sınırlar çalıştırmanız önce havuz.
* Veritabanlarınızı geri normal, çalışma oranı, işlem hattı yürütme işlemi tamamlandıktan sonra yeniden boyutlandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Veri akışı diğer makalelere bakın:

- [Veri akışına genel bakış](concepts-data-flow-overview.md)
- [Veri akışı etkinliği](control-flow-execute-data-flow-activity.md)

