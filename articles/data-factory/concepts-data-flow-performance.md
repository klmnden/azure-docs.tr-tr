---
title: Azure Data Factory'de veri akışını performans eşleme ve ayarlama Kılavuzu | Microsoft Docs
description: Eşleme bir veri akışı kullandığınızda, Azure Data factory'deki veri akışlarının performansını etkileyen anahtar Etkenler hakkında bilgi edinin.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: bbbc2bc5c47821469ecf15a27195b1bf0c12e6e5
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190617"
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

## <a name="monitor-data-flow-performance"></a>Veri akışı performansını izleme

Tarayıcıda akış eşleme verilerinizi tasarlama olsa da, birim testi tek tek her dönüştürme altındaki ayarlar bölmesini her dönüştürme için veri Önizleme sekmesine tıklayarak gerçekleştirebilirsiniz. Atmanız gereken sonraki adım, veri akışı için uçtan uca işlem hattı tasarımcısında test sağlamaktır. Bir yürütme veri akışı etkinliği eklemek ve veri akışınız performansını test etmek için hata ayıklama düğmesini kullanın. İşlem hattı penceresinin alt bölmede saydamlaşabilir simge "Eylemler" altında görürsünüz:

![Veri akışı izleme](media/data-flow/mon002.png "veri akışı izleme 2")

Bu simgeye tıklayarak sonraki performans profili, veri akışı ve yürütme planı görüntüler. Veri akışınızı farklı boyutlu veri kaynaklarına karşı performansını tahmin etmek için bu bilgileri kullanabilirsiniz. Küme iş yürütme Kurulum süresi 1 dakika, genel performans hesaplamalarınızda varsayılır ve ' % s'varsayılan Azure Integration Runtime'ı kullanıyorsanız, küme döndürme süresi de 5 dakika eklemeniz gerekebilir unutmayın.

![Veri akışı izleme](media/data-flow/mon003.png "veri akışı izleme 3")

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Azure SQL veritabanı ve Azure SQL veri ambarı için en iyi duruma getirme

![Kaynak bölümü](media/data-flow/sourcepart3.png "kaynak bölümü")

### <a name="partition-your-source-data"></a>Veri bölümleme

* "En"İyileştir gidin ve "Kaynak"'ı seçin. Belirli bir tablo sütunu ya da bir tür bir sorgu ayarlayın.
* Ardından "sütun" i seçerseniz, bölüm sütunu seçin.
* Ayrıca, Azure SQL DB için en fazla bağlantı sayısını ayarlayın. Veritabanınıza paralel bağlantıları sağlamak için daha yüksek bir ayar deneyebilirsiniz. Ancak, bazı durumlarda daha hızlı performans bağlantıları sınırlı sayıda ile sonuçlanabilir.
* Kaynak veritabanı tabloları bölümlenmiş olması gerekmez.
* Kaynak dönüşümünüzü, veritabanı tablosunun bölümleme düzeni ile eşleşen bir sorgu ayarlama kaynak veritabanı altyapısı'bölüm eleme kullanmasına izin verir.
* Kaynak zaten bölümlenmiş değil, ADF yine de veri kaynağı dönüşümünde seçtiğiniz anahtarı temel alan Spark dönüşüm ortamında bölümleme kullanın.

### <a name="set-batch-size-and-query-on-source"></a>Toplu iş boyutu ve sorgu kaynağında ayarlayın

![Kaynak](media/data-flow/source4.png "kaynak")

* Toplu iş boyutu ayarı-satır yerine bellekte kümelerinde veri depolamak için ADF yenilemelerini ister. İsteğe bağlı bir ayardır ve doğru boyutlandırılmadığında bırakılırsa kaynaklar yetersiz ve işlem düğümleri üzerinde çalışabilir.
* Bir sorgu ayarlama, hatta veri akışı için daha hızlı ilk veri alım yapabilirsiniz işlenmek ulaştıkları önce kaynak satırları sağ filtrelemek izin verebilirsiniz.
* Bir sorgu kullanırsanız, Azure SQL DB için yani READ UNCOMMITTED isteğe bağlı bir sorgu ipuçları ekleyebilirsiniz.

### <a name="set-isolation-level-on-source-transformation-settings-for-sql-datasets"></a>SQL veri kümeleri için kaynak dönüştürme ayarlarını yalıtım düzeyi ayarlama

* READ UNCOMMITTED kaynak dönüşümü daha hızlı sorgu sonuçlarını sağlar

![Yalıtım düzeyi](media/data-flow/isolationlevel.png "yalıtım düzeyi")

### <a name="set-sink-batch-size"></a>Havuz batch boyutunu ayarlama

![Havuz](media/data-flow/sink4.png "havuz")

* Satır veri akışlarınızı işlenmesini önlemek için Azure SQL DB için havuz ayarları "toplu iş boyutu" ayarlayın. Bu işlem, sağlanan boyutuna göre toplu işlem veritabanına ADF Yazar bildirir.

### <a name="set-partitioning-options-on-your-sink"></a>Bölümleme havuzunuzu seçeneklerini ayarlayın

* Verilerinizi hedef Azure SQL DB tabloları bölümlenmiş yoksa bile İyileştir sekmesine gidin ve bölümleme ayarlayın.
* Çok sık, daha hızlı bir şekilde veri tek bir düğüm/bölüm gelen tüm bağlantıları zorlamak yerine yükleme sonuçları Spark yürütme kümelerinde bölümleme hepsini kullanmak için ADF yalnızca bildiriyor.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Azure Integration Runtime, bilgi işlem altyapısı boyutunu artırın

![Yeni IR](media/data-flow/ir-new.png "yeni IR")

* Düğümlerin sayısını artırmak ve sorgu ve Azure SQL DB'ye yazılmak üzere daha fazla işlemci gücü sağlamak, çekirdek sayısını artırın.
* Daha fazla kaynak, işlem düğümlerine uygulamak için "İşlem için iyileştirilmiş" ve "Bellek için iyileştirilmiş" seçenekleri deneyin.

### <a name="unit-test-and-performance-test-with-debug"></a>Birim testi ve performans testi ile hata ayıklama

* Birim test veri akışı olarak ayarlandığında "Veri akışı Debug" düğmesini ON.
* Veri Akışı Tasarımcısı içinde dönüştürme mantığınızı sonuçlarını görüntülemek için dönüşümler veri Önizleme sekmesini kullanın.
* İşlem hattı tasarım üzerinde bir veri akışı etkinliği yerleştirerek işlem hattı Tasarımcısı'ndan veri akışları birim testi tuval ve test etmek için "Debug" düğmesini kullanın.
* Hata ayıklama modunda test etmek için tam zamanında küme döndürme-up beklemek zorunda kalmadan Canlı warmed küme ortamında karşı çalışır.

### <a name="disable-indexes-on-write"></a>Dizin üzerinde yazma devre dışı bırak
* Havuzunuzu yazıldığını, hedef tablolarda dizinler devre dışı bırakır, veri akışı etkinliği önce bir ADF işlem hattı depolanan yordam etkinliği kullanın.
* Veri akışı, etkinlik bu dizinleri etkinleştirilmiş başka bir saklı yordam etkinliği ekleyin.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Azure SQL DB boyutunu artırın
* Yeniden boyutlandırma kaynağını zamanlayabilir ve Azure SQL DB, aktarım hızını artırmak ve DTU ulaştıktan sonra Azure azaltma en aza indirmek için işlem hattınızı sınırlar çalıştırmanız önce havuz.
* Veritabanlarınızı geri normal, çalışma oranı, işlem hattı yürütme işlemi tamamlandıktan sonra yeniden boyutlandırabilirsiniz.

## <a name="optimizing-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için en iyi duruma getirme

### <a name="use-staging-to-load-data-in-bulk-via-polybase"></a>Polybase aracılığıyla toplu veri yükleme için hazırlık kullanın

* Satır veri akışlarınızı işlenmesini önlemek için ADF DW'ye satır ekler önlemek için Polybase yararlanabilir, böylece havuz ayarları "Hazırlama" seçeneğini ayarlayın. Bu, Polybase verilerin toplu olarak yüklenmesi için kullanmak üzere ADF yenilemelerini ister.
* Veri akış etkinliğinizi yürüttüğünüzde hazırlama ile bir işlem hattı açık olduğundan, hazırlama verilerinizi toplu yükleme için Blob Depolama konumu seçmek gerekir.

### <a name="increase-the-size-of-your-azure-sql-dw"></a>Azure SQL DW boyutunu artırın

* Yeniden boyutlandırma kaynağını zamanlayabilir ve verimliliği artırmak ve DWU limitlerini ulaştıktan sonra Azure azaltma en aza indirmek için işlem hattınızı çalıştırmadan önce Azure SQL DW havuz.

* Veritabanlarınızı geri normal, çalışma oranı, işlem hattı yürütme işlemi tamamlandıktan sonra yeniden boyutlandırabilirsiniz.

## <a name="optimize-for-files"></a>Dosyalar için en iyi duruma getirme

* ADF kullanacağı kaç bölümler denetleyebilirsiniz. Her kaynak ve havuz dönüştürme, aynı zamanda tek tek her dönüştürme, bölümleme düzeni ayarlayabilirsiniz. Daha küçük dosyalar için "Tek bölüm" seçerek bazen daha iyi ve Spark'ın küçük dosyalarınızı bölüm isteyen daha hızlı bir şekilde çalışabilir bulabilirsiniz.
* Kaynak verileriniz hakkında yeterli bilgiye sahip değilseniz, "Bölüm sayısını ayarlayın ve bölümleme hepsini bir kez" seçebilirsiniz.
* Verilerinizi Keşfetmenin yanı sıra iyi karma anahtarların olabilir sütunlar vardır, karma seçeneği bölümleme kullanın.

### <a name="file-naming-options"></a>Dosya adlandırma seçenekleri

* ADF eşleme veri akışları'nda dönüştürülmüş verileri yazma varsayılan yapısı, bir Blob veya ADLS bağlı hizmeti bir veri kümesi yazmaktır. Bu veri kümesi için bir klasörü veya kapsayıcı, adlandırılmış bir dosya değil işaret edecek şekilde ayarlamanız gerekir.
* Açık veri akışları kullanın ya da Spark bölümleme varsayılan Azure Databricks Spark çıkışınızı göre birden çok dosya üzerinden bölünür anlamına gelir, yürütme için veya bölümleme şeması, seçtiğiniz.
* Çok yaygın bir işlemi ADF veri akışları'ndaki tüm çıkış bölümü dosyalarınızı birlikte bir tek çıkış dosyasında birleştirilen böylece "tek dosya çıktısı" seçmektir.
* Ancak, bu işlem, çıkış tek bir küme düğümünde tek bir bölüme azaltır gerektirir.
* Bu popüler seçeneğini seçerken bunu aklınızda bulundurun. Tek çıkış dosyası bölüme çok büyük kaynak dosyaları birleştiriyorsanız küme düğümünde kaynaklarını dışında çalıştırabilirsiniz.
* İşlem düğümünde kaynaklarını tüketme önlemek için varsayılan veya açık bir bölümleme düzeni performans için en iyi duruma getirir, ADF içinde tutun ve sonra bir sonraki bölümü tüm birleştiren işlem hattının kopyalama etkinliği için yeni bir tek bir çıkış klasöründeki dosyaları ekleyin. dosya. Esas olarak, bu teknik, dosya birleştirme öğesinden dönüştürme eylemi ayırır ve "çıktıyı tek dosyaya" ayarı olarak aynı sonucu veren.

## <a name="next-steps"></a>Sonraki adımlar
Performansla ilgili diğer veri akışı makalelere bakın:

- [Veri akışı için sekmesinde en iyi duruma](concepts-data-flow-optimize-tab.md)
- [Veri akışı etkinliği](control-flow-execute-data-flow-activity.md)
- [Veri akışı performansını izleme](concepts-data-flow-monitoring.md)
