---
title: "SQL Data Warehouse verilerinizi geçirme | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse verilerinizi geçirme ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 0d156bc2eecf8220bd5ff4eb811d91482f216837
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-data"></a>Verilerinizi geçirme
Veri çeşitli araçları ile SQL veri ambarında farklı kaynaklardan yeniden taşınabilir.  ADF kopyalama, SSIS ve bcp tüm bu hedefe ulaşmak için kullanılabilir. Ancak, veri artar miktarda veri geçiş işlemi adımlara bölmek hakkında düşünmelisiniz. Bu, her adım performans ve esnekliği kesintisiz veri geçişini sağlamak için en iyi duruma getirme olanağı ortaya koymaktadır.

Bu makalede, ilk ADF kopyalama, SSIS ve bcp basit geçiş senaryoları açıklanmaktadır. Ardından, görünüm nasıl geçiş iyileştirilebilir içine biraz daha derin.

## <a name="azure-data-factory-adf-copy"></a>Azure veri fabrikası (ADF) kopyalama
[ADF kopyalama] [ ADF Copy] parçası olan [Azure Data Factory][Azure Data Factory]. ADF kopya verileriniz Azure blob depolama alanındaki veya SQL Data Warehouse doğrudan uzak düz dosyalara yerel depolama bulunan düz dosyalar dışarı aktarmak için kullanabilirsiniz.

Verilerinizi düz dosyalarda başlar, sonra önce bir yük başlatmadan önce Azure storage blobuna aktarmak gerekir, SQL Data Warehouse içine. Azure blob depolama alanına aktarılan verileri sonra kullanmayı seçebilirsiniz [ADF kopyalama] [ ADF Copy] yeniden SQL Data Warehouse'a veri göndermek için.

PolyBase verilerin yüklenmesi için yüksek performanslı seçeneği de sağlar. Ancak, iki tane yerine araçlarla anlamına gelmez. En iyi performansı gereksinim sonra PolyBase kullanmak istiyorsanız. Bir tek aracı deneyimi istediğiniz (ve veri yoğun değil) sonra ADF yanıtınız olur.


> 
> 

Bazı harika için aşağıdaki makaleye üzerinden baş [ADF örnekleri][ADF samples].

## <a name="integration-services"></a>Tümleştirme Hizmetleri
Integration Services (SSIS) karmaşık iş akışları, veri dönüştürme ve birkaç veri yükleme seçeneklerini destekleyen bir güçlü ve esnek ayıklamak dönüştürme ve yükleme (ETL) aracıdır. SSIS yalnızca Azure veya daha geniş bir geçişin parçası olarak veri aktarmak için kullanın.

> [!NOTE]
> SSIS dosyasındaki bayt sırası işareti olmadan UTF-8 olarak dışarı aktarabilirsiniz. Yapılandırmak için bu ilk gerekir türetilmiş sütun bileşen 65001 UTF-8 kod sayfasını kullanmak üzere veri akışındaki karakter verileri dönüştürmek için kullanın. Sütunları dönüştürüldükten sonra 65001 dosyası için kod sayfası olarak da seçilmiş sağlama düz dosya hedef bağdaştırıcı için veri yazma.
> 
> 

Bu SQL Server dağıtımı için yalnızca bağlandığınız gibi SSIS SQL veri ambarı'na bağlanır. Ancak, bağlantılarınızı bir ADO.NET Bağlantı Yöneticisi'ni kullanarak gerekir. Aynı zamanda "kullanım toplu ekleme kullanılabilir olduğunda" yapılandırmak için dikkatli verimliliği en üst düzeye çıkarmak için ayarı. Lütfen [ADO.NET hedef bağdaştırıcı] [ ADO.NET destination adapter] bu özellik hakkında daha fazla bilgi için makalenin

> [!NOTE]
> OLEDB kullanarak Azure SQL Data Warehouse'a bağlanma desteklenmiyor.
> 
> 

Ayrıca, var. her zaman bir paket son azaltma veya ağ sorunları için başarısız olabilir olasılığı Tasarım paketleri bunlar olmadan hatası noktasında ettirilebilir şekilde yineleme, iş, arızadan önce tamamlandı.

Daha fazla bilgi için [SSIS belgelerine][SSIS documentation].

## <a name="bcp"></a>bcp
BCP düz dosya verileri içeri ve dışarı aktarma için tasarlanmış bir komut satırı yardımcı programıdır. Bazı dönüştürme veri dışa aktarma sırasında gerçekleşebilir. Basit dönüşümleri gerçekleştirmek için seçin ve verileri dönüştürmek için bir sorgu kullanın. Dışarı sonra düz dosyalar ardından hedef doğrudan SQL Data Warehouse veritabanı yüklenebilir.

> [!NOTE]
> Kaynak sistemdeki bir görünümdeki verileri dışa aktarma sırasında kullanılan dönüşümleri kapsülleyen genellikle iyi bir fikirdir. Bu mantığı korunur ve tekrarlanabilir bir işlemdir sağlar.
> 
> 

Bcp avantajları şunlardır:

* Basitlik. BCP komutları oluşturmak ve çalıştırmak basit
* Yeniden başlatılabilir yükleme işlemi. Bir kez dışarı aktarılan yük olabilir kez herhangi bir sayıda yürütülebilir.

Bcp sınırlamaları vardır:

* BCP tabloda düz dosyalarıyla yalnızca çalışır. Xml veya JSON gibi dosyalarla çalışmıyor
* Veri dönüştürme özellikleri yalnızca verme aşama sınırlıdır ve doğası gereği basit
* BCP verilerin internet üzerinden yüklenirken sağlam olması uyarlanmıştır değil. Ağ kararsızlığa yük hataya neden olabilir.
* BCP yük önce hedef veritabanında var olan şema kullanır

Daha fazla bilgi için bkz: [verileri SQL Data Warehouse'a veri yüklemek için bcp kullanın][Use bcp to load data into SQL Data Warehouse].

## <a name="optimizing-data-migration"></a>Veri geçişi en iyi duruma getirme
Bir SQLDW veri taşıma işlemi, üç ayrı adımları etkili bir şekilde ayrılabilir:

1. Kaynak verileri dışa aktarma
2. Azure veri aktarımı
3. Hedef SQLDW veritabanına yükleyin

Her adım, her adım performansı en üst düzeye çıkaran güçlü, yeniden başlatılabilir ve esnek geçiş işlemi oluşturmak için ayrı ayrı iyileştirilebilir.

## <a name="optimizing-data-load"></a>En iyi duruma getirme veri yükleme
Bu bir süre için ters sırada bakarak; PolyBase veri yüklemek için en hızlı yoludur. Bunu anlamanın en iyisidir PolyBase yükleme işlemi için en iyi duruma getirme Önkoşullar önceki adımları ön yerleştirir. Bunlar:

1. Veri dosyaları kodlama
2. Veri dosyalarının biçimi
3. Veri dosyalarının konumu

### <a name="encoding"></a>Encoding
PolyBase, veri dosyalarını UTF-8 veya UTF-16FE olmasını gerektirir. 



### <a name="format-of-data-files"></a>Veri dosyalarının biçimi
PolyBase sabit satır Sonlandırıcı \n ya da yeni satır olması zorunlu tutulmuştur. Veri dosyalarınızı bu standardına uygun olmalıdır. Dize veya sütun sonlandırıcılar üzerinde herhangi bir kısıtlamanın değil.

PolyBase, dış tablosunda bir parçası olarak, dosyadaki her sütun tanımlamak gerekecektir. Dışarı aktarılan tüm sütunları gereklidir ve türleri gerekli standartlarına uygun emin olun.

Lütfen geri bakın [şemanızı geçir] makale desteklenen veri türleri hakkında ayrıntılı bilgi için.

### <a name="location-of-data-files"></a>Veri dosyalarının konumu
SQL veri ambarı PolyBase verileri Azure Blob depolama alanından özel olarak yüklemek için kullanır. Sonuç olarak, veriler ilk blob depolama alanına aktarıldı gerekir.

## <a name="optimizing-data-transfer"></a>En iyi duruma getirme veri aktarımı
En yavaş bölümleri veri geçişinin Azure veri aktarımını biridir. Yalnızca ağ bant genişliği bir sorun olabilir ancak aynı zamanda ağ güvenilirlik ciddi ilerlemesini. Verileri Azure'a geçirme varsayılan internet üzerinden olduğundan oluşan aktarımı hatalar olasılığını makul olasıdır. Ancak, bu hataları tamamen veya kısmen yeniden gönderilecek verileri gerektirebilir.

Neyse ki hızı ve bu işlemin esnekliği artırmak için birkaç seçeneğiniz vardır:

### <a name="expressrouteexpressroute"></a>[ExpressRoute][ExpressRoute]
Kullanmayı göz önünde [ExpressRoute] [ ExpressRoute] aktarma işlemini hızlandırmak için. [ExpressRoute] [ ExpressRoute] bağlantı genel internet üzerinden geçmez şekilde Azure'a ile özel bir bağlantı sağlar. Bu halinde zorunlu bir adımdır. Ancak, bu üretilen işi veri bir şirket içinden Azure'a dağıtmaya zaman artıracaktır veya ortak yerleşim tesisi.

Kullanmanın avantajları [ExpressRoute] [ ExpressRoute] şunlardır:

1. Daha fazla güvenilirlik
2. Daha hızlı ağ hızı
3. Alt ağ gecikmesi
4. daha yüksek ağ güvenliği

[ExpressRoute] [ ExpressRoute] çeşitli senaryoları; için yararlıdır yalnızca geçiş.

İlginizi çekiyor mu? Lütfen fiyatlandırma ve daha fazla bilgi için ziyaret [ExpressRoute belgeleri][ExpressRoute documentation].

### <a name="azure-import-and-export-service"></a>Azure içeri ve dışarı aktarma hizmeti
Azure içeri ve dışarı aktarma hizmeti büyük (GB ++ için) yoğun (TB ++) veri aktarımlarını Azure içine için tasarlanmış bir veri aktarım işlemi değil. Verilerinizi diske yazma ve bir Azure veri merkezine sevkiyat içerir. Disk içeriği sizin adınıza ardından Azure Storage Bloblarında yüklenir.

İçeri aktarma dışa aktarma işlemi üst düzey bir görünümünü aşağıdaki gibidir:

1. Verileri almak için bir Azure Blob Storage kapsayıcısı yapılandırın
2. Yerel depolama alanına verilerinizi dışarı aktarma
3. 3,5 inç SATA II/III sabit disk sürücülerine [Azure içeri/dışarı aktarma aracı] kullanarak veri kopyalama
4. Azure içeri ve dışarı aktarma [Azure içeri/dışarı aktarma aracı] tarafından oluşturulan günlük dosyalarının sağlayan hizmet kullanarak bir içeri aktarma işi oluşturma
5. Önerilen Azure veri merkezinizin diskleri sevk
6. Verileriniz Azure Blob Storage kapsayıcısına aktarılır
7. Polybase'i kullanarak SQLDW veri yükleme

### <a name="azcopyazcopy-utility"></a>[AZCopy][AZCopy] yardımcı programı
[AZCopy][AZCopy] Azure Storage Bloblarında aktarılan verilerinizi almak için harika bir aracı programıdır. Bu işlem için küçük (MB ++) çok büyük (GB ++) veri aktarımları için tasarlanmıştır. [AZCopy] ayrıca Azure ve bunu veri aktarımı yaparken iyi dayanıklı işleme veri aktarımı adım için harika bir seçim sağlamak için tasarlanmıştır. Bir kez aktarılan PolyBase kullanarak SQL Data Warehouse'a veri yükleme. Ayrıca, bir "Yürütme işlemi" görevini kullanarak, SSIS paketleri AZCopy dahil edebilirsiniz.

AZCopy kullanmak için önce indirip yüklemeniz gerekir. Var olan bir [üretim sürümü] [ production version] ve [Önizleme sürümü] [ preview version] kullanılabilir.

Bir dosyayı, dosya sisteminden yüklemek için komutu aşağıdaki gibi gerekir:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Bir üst düzey işlem özeti aşağıdakilerden biri olabilir:

1. Verileri almak için bir Azure depolama blob kapsayıcısını yapılandırın
2. Yerel depolama alanına verilerinizi dışarı aktarma
3. AZCopy verilerinizi Azure Blob Storage kapsayıcısı
4. Verileri PolyBase kullanarak SQL Data Warehouse'a veri yükleme

Tam belgeleri kullanılabilir: [AZCopy][AZCopy].

## <a name="optimizing-data-export"></a>En iyi duruma getirme verileri dışarı aktarma
Verme PolyBase tarafından düzenlendiği gereksinimlerine uyan sağlamaya ek olarak, ayrıca dışarı aktarma işlemi daha fazla artırmak için verilerin en iyi duruma getirmek arama.



### <a name="data-compression"></a>Veri sıkıştırma
PolyBase gzip sıkıştırılmış verileri okuyabilir. Ardından verilerinizi gzip dosyaları sıkıştır tamamlayabilirseniz ağ üzerinden gönderilen veri miktarını en aza indirgenecektir.

### <a name="multiple-files"></a>Birden çok dosya
Çeşitli dosyaları büyük tablolara bölme yalnızca verme hızını artırmak için yardımcı olur, ayrıca aktarımı re-startability ve bir kez Azure blob depolama alanındaki verilerin genel yönetilebilirlik yardımcı olur. PolyBase iyi özelliklerinin çoğu bir klasör içindeki tüm dosyaları okuma ve bir tablosu olarak kabul olduğunu biridir. Dosyaları her tablo için kendi klasörüne yalıtmak için bu nedenle iyi bir fikirdir.

PolyBase "özyinelemeli klasör geçişinin" bilinen bir özellik de destekler. Daha fazla veri yönetimini geliştirmek için dışarı aktarılan verileri organizasyonu artırmak için bu özelliği kullanın.

PolyBase ile veri yükleme hakkında daha fazla bilgi için bkz: [kullanım verileri SQL Data Warehouse'a veri yüklemek için PolyBase][Use PolyBase to load data into SQL Data Warehouse].

## <a name="next-steps"></a>Sonraki adımlar
Geçiş hakkında daha fazla bilgi için bkz: [çözümünüzü SQL veri ambarına geçirme][Migrate your solution to SQL Data Warehouse].
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/v1/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/v1/data-factory-samples.md
[ADF Copy examples]: ../data-factory/v1/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution to SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp to load data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase to load data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
