---
title: Verilerinizi SQL veri ambarı'na geçirme | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL veri ambarı'na verilerinizi geçirme hakkında ipuçları.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: 6a2acf602252ee4319f9a5eccef53a25d8e2dd7f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60748195"
---
# <a name="migrate-your-data"></a>Verilerinizi geçirme
Farklı kaynaktaki verileri çeşitli araçlarla, SQL veri ambarı'na taşınabilir.  ADF kopyalama, SSIS ve bcp tüm bu hedefe ulaşmak için kullanılabilir. Ancak, veri arttıkça miktarda veri geçiş işlemi adımlara bölmek hakkında düşünmelisiniz. Her adım için performans ve esneklik kesintisiz veri geçişini sağlamak için en iyi duruma getirme olanağı verir.

Bu makalede, ilk ADF kopyalama, SSIS ve bcp basit geçiş senaryoları ele alınmaktadır. Ardından nasıl geçiş iyileştirilebilir içine biraz daha ayrıntılı bir görünüm.

## <a name="azure-data-factory-adf-copy"></a>Azure Data Factory (ADF) kopyalama
[ADF kopyalama] [ ADF Copy] parçasıdır [Azure Data Factory][Azure Data Factory]. ADF kopyalama, verilerinizi Azure blob depolama veya SQL veri ambarı'na doğrudan tutulan uzak düz dosyaları için yerel depolamada bulunan düz dosyalar aktarmak için kullanabilirsiniz.

Verilerinizi düz dosyaları başlar, sonra Yük başlatmadan önce Azure storage blobuna aktarmak önce gerekir, bu SQL veri ambarı'na. Verileri Azure blob depolama alanına devredildikten sonra kullanmayı seçebilirsiniz [ADF kopyalama] [ ADF Copy] verileri SQL veri ambarı'na yeniden gönderin.

PolyBase Ayrıca, veri yükleme için yüksek performanslı seçeneği sağlar. Ancak, bir yerine iki araçlarını kullanarak geliyor. En iyi performansa gerek sonra PolyBase kullanma Tek bir araç deneyimi istediğiniz (ve veri çok büyük değilse) sonra ADF aradığınız cevaptır.

İzleyin [Bu öğreticide](../data-factory/load-azure-sql-data-warehouse.md) ADF verileri veri ambarınıza yüklemek için nasıl kullanılacağını öğrenin.

## <a name="integration-services"></a>Tümleştirme Hizmetleri
Integration Services (SSIS) karmaşık iş akışları, veri dönüştürme ve birkaç veri yükleme seçeneklerini destekleyen bir güçlü ve esnek ayıklama, dönüştürme ve yükleme (ETL) aracıdır. SSIS yalnızca azure'a ya da daha geniş bir geçişin parçası olarak veri aktarmak için kullanın.

> [!NOTE]
> SSIS dosyasında Unicode bayt sırası işareti olmadan UTF-8 olarak dışarı aktarabilirsiniz. Yapılandırmak için bu ilk gerekir türetilmiş sütun bileşeni UTF-8 kod sayfası 65001 kullanmak için veri akışında karakter verileri dönüştürmek için kullanın. Sütunları dönüştürüldükten sonra verileri sağlama 65001 kod sayfası dosyası için de seçildi düz dosya hedef bağdaştırıcı yazma.
> 
> 

Bu bir SQL Server dağıtımı için yalnızca bağlandığınız gibi SSIS SQL veri ambarı'na bağlanır. Ancak, bağlantılarınızı bir ADO.NET Bağlantı Yöneticisi'ni kullanarak gerekir. Ayrıca Al "kullanmak toplu ekleme kullanılabilir olduğunda" yapılandırmak için dikkat etmelisiniz aktarım hızını en üst düzeye çıkarmak için ayarı. Lütfen [ADO.NET hedef bağdaştırıcı] [ ADO.NET destination adapter] bu özellik hakkında daha fazla bilgi edinmek için makaleyi

> [!NOTE]
> OLEDB kullanarak Azure SQL veri ambarı'na bağlanması desteklenmiyor.
> 
> 

Ayrıca, olasılığı vardır her zaman, bir paket azaltma veya ağ sorunları nedeniyle ayıklayamayabilir. Tasarım, bunlar olmadan hata noktasında sürdürülebilir şekilde paketleri yineleme, iş, hatasından önce tamamlandı.

Daha fazla bilgi için [SSIS belgelerinde][SSIS documentation].

## <a name="bcp"></a>bcp
BCP düz dosya verileri içeri ve dışarı aktarma için tasarlanan bir komut satırı yardımcı programıdır. Bazı dönüştürme verileri dışarı aktarma sırasında gerçekleşir. Basit dönüştürme gerçekleştirmek için seçin ve verileri dönüştürmek için bir sorgu kullanın. Dışarı sonra düz dosyalar ardından hedef doğrudan SQL veri ambarı veritabanı yüklenebilir.

> [!NOTE]
> Kaynak sistemdeki bir görünümde verileri dışarı aktarma sırasında kullanılan dönüştürmeleri kapsüllemek için genellikle iyi bir fikirdir. Bu mantığı korunur ve tekrarlanabilir bir işlemdir sağlar.
> 
> 

Bcp avantajları şunlardır:

* Basitlik. BCP komut derlemek ve çalıştırmak basit
* Yeniden başlatılabilir yükleme işlemi. Bir kez verilen yük olabilir herhangi bir sayıda kez yürütüldü

Bcp sınırlamaları vardır:

* BCP sekmeli düz dosyaları ile yalnızca çalışır. Xml veya JSON gibi dosyaları ile çalışmıyor
* Veri dönüştürme özelliklerini verme aşaması sınırlıdır ve doğası gereği basit
* BCP verilerini internet üzerinden yüklerken güçlü olacak şekilde uyarlandığı değil. Yükleme hatası ağ kararsızlığa neden olabilir.
* BCP yüklenmesinden önce hedef veritabanında mevcut olan şemayı kullanır

Daha fazla bilgi için [SQL veri ambarı'na veri yüklemek için bcp kullanın][Use bcp to load data into SQL Data Warehouse].

## <a name="optimizing-data-migration"></a>Veri geçişi en iyi duruma getirme
SQLDW veri geçiş işlemi, üç ayrı adımlar etkili bir şekilde ayrılabilir:

1. Kaynak verileri dışarı aktarma
2. Azure veri aktarımı
3. Hedef SQLDW veritabanı'na yükleme

Her adım tek tek her bir adımı performans en üst düzeye bir sağlam, yeniden başlatılabilir ve esnek geçiş işlemi oluşturmak için de iyileştirilebilir.

## <a name="optimizing-data-load"></a>En iyi duruma getirme veri yükleme
Bu kısa bir süre için ters sırada bakarak; PolyBase verileri yüklemek için en hızlı yoludur. Bunu anlamanın en iyisidir PolyBase yükleme işlemi için en iyi duruma getirme önkoşulları önceki adımları ön yerleştirir. Bunlar:

1. Veri dosya kodlama
2. Veri dosyalarının biçimi
3. Veri dosyalarının konumu

### <a name="encoding"></a>Encoding
PolyBase UTF-8 veya UTF-16FE veri dosyaları gerektirir. 



### <a name="format-of-data-files"></a>Veri dosyalarının biçimi
PolyBase bir sabit satır Sonlandırıcı \n veya yeni satır zorunlu kılar. Veri dosyalarınızı bu standardına uygun olmalıdır. Dize veya sütun sonlandırıcılar herhangi bir kısıtlama değildir.

PolyBase, dış tablonuzdaki bir parçası olarak, dosyada her sütun tanımlaması gerekir. Dışarı aktarılan tüm sütunları gereklidir ve türleri için gerekli standartlara uygun emin olun.

Lütfen kiracıurl [şemanızın geçişini yapın] makale desteklenen veri türleri hakkında daha fazla ayrıntı için.

### <a name="location-of-data-files"></a>Veri dosyalarının konumu
SQL veri ambarı PolyBase, verileri Azure Blob depolama alanından özel olarak yüklemek için kullanır. Sonuç olarak, verileri ilk blob depolama alanına aktarıldı gerekir.

## <a name="optimizing-data-transfer"></a>En iyi duruma getirme veri aktarımı
Veri geçişi en yavaş bölümlerini Azure veri aktarımını biridir. Yalnızca ağ bant genişliği bir sorun olabilir, ancak aynı zamanda ağ güvenilirliğini ciddi ilerlemesini. Verileri Azure'a geçirmeyi varsayılan olarak internet üzerinden olduğundan oluşan aktarım hataları olasılığını makul ölçüde yüksektir. Ancak, bu hataları tamamen veya kısmen yeniden gönderilecek verileri gerektirebilir.

Neyse ki bu işlemin esnekliği ve hızını artırmak için birkaç seçeneğiniz vardır:

### <a name="expressrouteexpressroute"></a>[ExpressRoute][ExpressRoute]
Göz önünde bulundurmak isteyebileceğiniz [ExpressRoute] [ ExpressRoute] aktarımını hızlandırmak için. [ExpressRoute] [ ExpressRoute] bağlantısını genel internet üzerinden geçmez şekilde Azure'a ile kurulan bir özel bağlantı sağlar. Bu olmadığı göre Hayır zorunlu bir adımdır. Ancak, bu aktarım hızı bir şirket içinden Azure'a veri gönderirken artar veya ortak yerleşim tesisindeki.

Kullanmanın avantajları [ExpressRoute] [ ExpressRoute] şunlardır:

1. Daha fazla güvenilirlik
2. Daha hızlı ağ hızı
3. Daha düşük ağ gecikme süresi
4. daha yüksek ağ güvenliği

[ExpressRoute] [ ExpressRoute] birçok senaryoda; için yararlıdır yalnızca geçiş.

İlginizi çekiyor mu? Lütfen fiyatlandırma ve daha fazla bilgi için ziyaret [ExpressRoute belgeleri][ExpressRoute documentation].

### <a name="azure-import-and-export-service"></a>Azure içeri ve dışarı aktarma hizmeti
Azure içeri aktarma ve dışarı aktarma hizmeti, büyük (GB ++ için) çok büyük (TB ++) veri aktarımlarını Azure için tasarlanan bir veri aktarım işlemi olduğundan. Bu, verilerinizi diske yazma ve bunları bir Azure veri merkezine sevkiyat içerir. Disk içeriğinin sonra sizin adınıza Azure depolama Blobları yüklenir.

Üst düzey görünümü içeri aktarma dışarı aktarma işlemi aşağıdaki gibidir:

1. Verileri almak için bir Azure Blob Depolama kapsayıcısında yapılandırın
2. Verilerinizi yerel depolamaya Aktar
3. 3,5 inç SATA II/III sabit disk sürücülerine [Azure içeri/dışarı aktarma aracı] kullanarak veri kopyalama
4. Azure içeri ve dışarı aktarma [Azure içeri/dışarı aktarma aracı] tarafından oluşturulan günlük dosyalarının sağlama hizmeti kullanarak içeri aktarma işi oluşturma
5. Diskleri, önerilen Azure veri merkezine gönderin
6. Verilerinizi Azure Blob Depolama kapsayıcınızı aktarılır.
7. PolyBase kullanarak SQLDW veri yükleme

### <a name="azcopyazcopy-utility"></a>[AZCopy][AZCopy] yardımcı programı
[AZCopy][AZCopy] yardımcı programı, Azure depolama Blobları aktarılan verilerinizi almak için harika bir araçtır. Bu, küçük (MB ++) çok büyük (GB ++) veri aktarımları için tasarlandı. [AZCopy] Azure ve bunu veri aktarımı yaparken iyi dayanıklı işleme veri aktarımı adımı için ideal bir tercih sağlamak için tasarlanmıştır. Bir kez aktarılan PolyBase kullanarak SQL Data Warehouse'a veri yükleme. Ayrıca, bir "Yürütme işlemi" görevi'ni kullanarak SSIS paketlerinizi AZCopy birleştirebilirsiniz.

Azcopy'yi kullanma, ilk kez indirip yüklemeniz gerekir. Var olan bir [üretim sürümü] [ production version] ve [Önizleme sürümü] [ preview version] kullanılabilir.

Dosya sisteminizden bir dosyayı karşıya yüklemek için komutu aşağıdaki gibi ihtiyacınız olacak:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Üst düzey bir işlem özeti aşağıdakilerden biri olabilir:

1. Verileri almak için bir Azure depolama blob kapsayıcısını yapılandırma
2. Verilerinizi yerel depolamaya Aktar
3. AZCopy, verileri Azure Blob Depolama kapsayıcısında
4. Verileri PolyBase kullanarak SQL Data Warehouse'a veri yükleme

Kullanılabilir tüm belgeler: [AZCopy][AZCopy].

## <a name="optimizing-data-export"></a>En iyi duruma getirme verileri dışarı aktarma
Dışarı aktarma PolyBase tarafından belirtilen gereksinimlere uyan sağlamaya ek olarak, ayrıca verilerin dışarı aktarılmasını işlem daha da geliştirmek için en iyi duruma getirmek arama.



### <a name="data-compression"></a>Veri sıkıştırma
PolyBase, gzip sıkıştırılmış verisi okuyabilirsiniz. Sonra verilerinizi gzip dosyaları sıkıştır olanağınız varsa ağ üzerinden gönderilen veri miktarını en aza.

### <a name="multiple-files"></a>Birden çok dosya
Çeşitli dosyalara büyük tablolarını bozucu yalnızca verme hızını artırmaya yardımcı olur, ayrıca aktarım re-startability ve verilerin Azure blob depolama alanındaki bir kez genel yönetilebilirlik yardımcı olur. Bu klasör içindeki tüm dosyaları okuma ve bir tablo olarak davranma, PolyBase güzel özellikleri olmasıdır. Bu nedenle kendi klasörüne dosyalar her tablo için yalıtmak için iyi bir fikirdir.

PolyBase Ayrıca, "özyinelemeli klasör geçişi" bilinen bir özelliği destekler. Bu özellik, veri yönetimini iyileştirmek için dışarı aktarılan verileri organizasyonu daha da geliştirmek için kullanabilirsiniz.

PolyBase ile veri yükleme hakkında daha fazla bilgi için bkz. [verileri SQL Data Warehouse'a veri yüklemek için PolyBase kullanma][Use PolyBase to load data into SQL Data Warehouse].

## <a name="next-steps"></a>Sonraki adımlar
Geçiş hakkında daha fazla bilgi için bkz. [çözümünüzü SQL veri ambarı'na geçirme][Migrate your solution to SQL Data Warehouse].
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[AzCopy]: ../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/load-azure-sql-data-warehouse.md 
[ADF Copy examples]: ../data-factory/quickstart-create-data-factory-dot-net.md
[development overview]: sql-data-warehouse-overview-develop.md
[şemanızın geçişini yapın]: sql-data-warehouse-migrate-schema.md
[Migrate your solution to SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp to load data into SQL Data Warehouse]: /sql/tools/bcp-utility
[Use PolyBase to load data into SQL Data Warehouse]: load-data-wideworldimportersdw.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: https://azure.microsoft.com/services/data-factory/
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: https://azure.microsoft.com/documentation/services/expressroute/

[production version]: https://aka.ms/downloadazcopy/
[preview version]: https://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
