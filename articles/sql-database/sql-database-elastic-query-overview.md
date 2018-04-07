---
title: Azure SQL Database esnek sorgu genel bakış | Microsoft Docs
description: Esnek sorgu birden fazla veritabanı yayılan bir Transact-SQL sorgusu çalıştırmanıza olanak sağlar.
services: sql-database
manager: craigg
author: MladjoA
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: mlandzic
ms.openlocfilehash: 6367418fb07b2ab5b425609540c653678a207ebc
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Azure SQL Database esnek sorgu genel bakış (Önizleme)
Esnek sorgu özelliği (Önizleme), Azure SQL veritabanında birden çok veritabanı yayılan bir Transact-SQL sorgusu çalıştırmanızı sağlar. Veritabanları arası sorgular uzak tablolara erişilen ve Microsoft ve üçüncü taraf araçları (Excel, Powerbı, Tableau, vb.) birden çok veritabanı veri katmanlarıyla sorgulama bağlanmak için gerçekleştirmenizi sağlar. Bu özelliği kullanarak, SQL veritabanında büyük veri katmanlarını sorguları ölçeğini ve iş zekası (BI) raporlarında sonuçları görselleştirebilirsiniz.


## <a name="why-use-elastic-queries"></a>Esnek sorgular neden kullanılır?

**Azure SQL Veritabanı**

Azure SQL veritabanlarında tamamen T-SQL sorgulama. Bu, uzak veritabanlarına salt okunur sorgulamaya izin verir. Bu, üç ve dört part adları ya da bağlantılı sunucuya SQL DB kullanarak uygulamaları geçirmek geçerli şirket içi SQL Server müşterileri için bir seçenek sağlar.

**Standart katmanda kullanılabilir**

Esnek sorgu yanı sıra Premium performans katmanı standart performans katmanı desteklenir. Daha düşük performans katmanı için performans sınırlamaları üzerinde Önizleme sınırlamaları bölümüne bakın.

**Uzak veritabanlarına bildirme**

Esnek sorgular için yürütme uzak veritabanlarına SQL parametreleri artık gönderebilir.

**Saklı yordam yürütme**

Uzak saklı yordam çağrıları ya da uzak işlevleri kullanarak yürütme [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714).

**Esneklik**

Esnek sorgu ile dış tablolar şimdi farklı şema veya tablo adı ile uzak tablolar başvurabilirsiniz.

## <a name="elastic-query-scenarios"></a>Esnek sorgu senaryoları

Hedef birden çok veritabanı satırları tek bir genel sonuç burada katkıda sorgulanırken senaryoları kolaylaştırmaktır. Sorgu kullanıcı veya uygulamanın doğrudan veya dolaylı olarak veritabanına bağlı araçları aracılığıyla ya da oluşabilir. Bu rapor, ticari BI veya veri tümleştirme araçları kullanarak oluştururken özellikle yararlı olacaktır — ya da değiştirilemez herhangi bir uygulama. Esnek bir sorgu ile tanıdık SQL Server bağlantı deneyimi Excel, Powerbı, Tableau veya Cognos gibi araçları kullanarak birden fazla veritabanı üzerinden sorgulayabilirsiniz.
Esnek bir sorgu veritabanlarını SQL Server Management Studio veya Visual Studio tarafından verilen sorguları aracılığıyla koleksiyonunun tamamını kolay erişim sağlar ve veritabanları arası Entity Framework veya diğer ORM ortamları sorgulanmasını kolaylaştırır. Şekil 1, burada varolan izin ver bulut uygulaması bir senaryo gösterir (kullanan [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)) derlemeleri ölçeklendirilmiş veri katmanı ve esnek sorgu veritabanları arası raporlaması için kullanılır.

**Şekil 1** ölçeklendirilmiş veri katmanını kullanılan esnek sorgu

![Ölçeklendirilen veri katmanını kullanılan esnek sorgu][1]

Müşteri senaryoları için esnek sorgu aşağıdaki topolojilerden tarafından belirlenir:

* **Dikey bölümleme - veritabanları arası sorguları** (topoloji 1): bir veri katmanı veritabanlarında sayısı arasında veri dikey olarak bölümlenmiş. Genellikle, farklı veritabanlarını tabloları farklı kümesi bulunur. Şema farklı veritabanlarında farklı olduğu anlamına gelir. Örneğin, tüm hesap ilişkili tabloları ikinci bir veritabanı üzerinde çalışırken tüm tablolar için stok bir veritabanında ' dir. Bu topoloji ile ortak kullanım durumları sorgulama ya da birden fazla veritabanı tablolarında arasında raporları derlemek için gerektirir.
* **Yatay bölümleme - parçalama** (topolojisi 2): veri bölümlenmiş yatay satır arasında genişletilmiş verileri dağıtmak için katmanı. Bu yaklaşımda, şema tüm katılan veritabanları aynıdır. Bu yaklaşım, "parçalama" olarak da adlandırılır. Parçalama gerçekleştirilebilir ve yönetilen (1 esnek veritabanı kullanarak kitaplıkları veya (2) self-parçalama araçları. Esnek bir sorgu, sorgu veya fazla parça raporları derlemek için kullanılır.

> [!NOTE]
> Esnek sorgu işlemlerin çoğunu veri katmanını burada gerçekleştirilebilir zaman raporlama senaryoları için en iyi şekilde çalışır. Raporlama yoğun iş yükleri veya veri depolama senaryolarında ile daha karmaşık sorgular için de kullanmayı [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
>  

## <a name="vertical-partitioning---cross-database-queries"></a>Dikey bölümleme - veritabanları arası sorguları

Kodlamaya başlamak için bkz: [(dikey bölümleme) veritabanları arası sorgusu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).

Esnek bir sorgu, diğer SQL veritabanları için kullanılabilir bir SQL veritabanında bulunan verilere yapmak için kullanılabilir. Bu, diğer herhangi bir uzak SQL veritabanı tablolarında başvurmak için bir veritabanı sorgularından sağlar. İlk adım, her uzak veritabanı için dış veri kaynağına tanımlamaktır. Dış veri kaynağı, Uzak veritabanı üzerinde bulunan tabloları erişmek istediğiniz yerel veritabanında tanımlanır. Uzak veritabanı üzerinde hiçbir değişiklik gereklidir. Farklı veritabanlarına farklı şemalar sahip olduğu dikey bölümleme için tipik senaryolar, esnek sorgular başvuru verileri için erişim gibi ortak kullanım durumları uygulamak ve veritabanları arası sorgulama için kullanılabilir.

> [!IMPORTANT]
> ALTER ANY dış veri KAYNAĞINA iznine sahip olması gerekir. Bu izin ALTER DATABASE izniyle dahil edilir. Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.
>

**Başvuru verileri**: topoloji başvuru veri yönetimi için kullanılır. Aşağıdaki şekilde iki tablolarla (T1 ve T2) başvuru verileri ayrılmış bir veritabanında tutulur. Esnek bir sorgu kullanarak, artık tabloları T1 ve T2 uzaktan diğer veritabanlarından şekilde gösterildiği gibi erişebilirsiniz. Başvuru tabloları küçük ya da uzak sorguları başvuru tablosu varsa 1 topolojisini kullan seçmeli koşullar vardır.

**Şekil 2** dikey bölümleme - esnek sorgu için sorgu başvuru verileri kullanma

![Dikey bölümleme - esnek sorgu için sorgu başvuru verileri kullanma][3]

**Veritabanları arası sorgulama**: esnek sorgular birkaç SQL veritabanları arasında sorgulama gerektiren etkinleştir kullanım örnekleri. Şekil 3, dört farklı veritabanları gösterir: CRM, Envanter, İK ve ürünler. Veritabanlarından birini gerçekleştirilen sorguları da birini veya hepsini diğer veritabanlarına erişim gerekir. Esnek bir sorgu kullanarak, her bir dört veritabanı birkaç basit DDL deyimleri çalıştırarak bu durumda, veritabanınızı yapılandırabilirsiniz. Bu tek seferlik yapılandırma sonra uzak tabloya erişim T-SQL sorgularınızı veya BI araçları yerel tabloya başvuran olarak kadar basittir. Uzak sorgular büyük sonuç döndürmezse bu yaklaşım önerilir.

**Şekil 3** dikey bölümleme - çeşitli veritabanları arasında esnek sorgu için sorgu kullanma

![Dikey bölümleme - çeşitli veritabanları arasında esnek sorgu için sorgu kullanma][4]

Esnek veritabanı sorguları uzak SQL veritabanları aynı şema ile üzerinde bulunan bir tabloya erişim gerektiren dikey bölümleme senaryolar için aşağıdaki adımları yapılandırın:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [CREATE/DROP dış veri KAYNAĞINA](https://msdn.microsoft.com/library/dn935022.aspx) gelen veriKaynağım'a türü **RDBMS**
* [CREATE/DROP dış tablo](https://msdn.microsoft.com/library/dn935021.aspx) mytable

DDL deyimleri çalıştırdıktan sonra uzak tablo "mytable" dizinindeymiş gibi yerel bir tabloya erişebilirsiniz. Azure SQL veritabanı otomatik olarak uzak veritabanına bağlantı açar, isteğiniz Uzak veritabanı üzerinde işler ve sonuçları döndürür.

## <a name="horizontal-partitioning---sharding"></a>Yatay bölümleme - parçalama
Esnek sorgu kullanarak başka bir deyişle, yatay olarak bölümlenmiş, raporlama görevleri parçalı üzerinde gerçekleştirmek için veri katmanı gerektiren bir [esnek veritabanı parça eşleme](sql-database-elastic-scale-shard-map-management.md) veri katmanı veritabanları temsil etmek için. Genellikle, yalnızca bir tek parça eşleme Bu senaryoda kullanılan ve esnek sorgu özellikleri (baş düğüm) ile ayrılmış bir veritabanı sorguları raporlama için giriş noktası olarak görev yapar. Bu adanmış veritabanı parça eşleme erişimi olmalıdır. Şekil 4, bu topoloji ve esnek sorgu veritabanı ve parça eşleme ile yapılandırması gösterilmektedir. Veri katmanı veritabanlarında herhangi bir Azure SQL veritabanı sürümü veya edition olabilir. Esnek veritabanı istemci kitaplığı ve parça eşlemeleri oluşturma hakkında daha fazla bilgi için bkz: [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md).

**Şekil 4** yatay bölümleme - parçalı veriler katmanları raporlama için esnek sorgu kullanma

![Yatay bölümleme - parçalı veriler katmanları raporlama için esnek sorgu kullanma][5]

> [!NOTE]
> Esnek sorgu veritabanı (baş düğüm), ayrı veritabanı veya parça eşleme barındıran aynı veritabanı olabilir. Seçtiğiniz herhangi bir yapılandırma emin olun, veritabanının bu hizmet ve performans katmanı beklenen sürede oturum açma/sorgu isteği işlemek için yüksek.

Aşağıdaki adımları (genellikle) birkaç uzak SQL veritabanlarında bulunan tablo kümesini erişim gerektiren yatay bölümleme senaryoları için esnek veritabanı sorguları yapılandırın:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* Oluşturma bir [parça eşleme](sql-database-elastic-scale-shard-map-management.md) esnek veritabanı istemci kitaplığı kullanılarak, veri katmanı temsil eden.   
* [CREATE/DROP dış veri KAYNAĞINA](https://msdn.microsoft.com/library/dn935022.aspx) gelen veriKaynağım'a türü **SHARD_MAP_MANAGER**
* [CREATE/DROP dış tablo](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Bu adımları gerçekleştirdikten sonra dizinindeymiş gibi yerel bir tabloya yatay olarak bölümlenmiş tabloda "mytable" erişebilir. Azure SQL veritabanı otomatik olarak tabloları fiziksel olarak nerede depolandığı uzak veritabanları için birden çok paralel bağlantı açar, uzak veritabanlarında isteklerini işler ve sonuçları döndürür.
Yatay bölümleme senaryo bulunabilir için gerekli adımları hakkında daha fazla bilgi [yatay bölümleme için esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md).

Kodlamaya başlamak için bkz: [yatay (parçalama) bölümleme için esnek sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started.md).

## <a name="t-sql-querying"></a>T-SQL sorgulama
Dış veri kaynakları ve dış tablolarınızı tanımladıktan sonra dış tablolara tanımlandığı veritabanlarına bağlanmak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. Sonra T-SQL deyimlerini dış tablolar üzerindeki bu bağlantıda özetlenen kısıtlamalarla çalıştırabilirsiniz. İçin belgeleri konularda daha fazla bilgi ve T-SQL sorgularını örnekleri bulabilirsiniz [yatay bölümleme](sql-database-elastic-query-horizontal-partitioning.md) ve [dikey bölümleme](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Bağlantı için araçları
Uygulamaları ve BI veya veri tümleştirme araçları dış tablolar veritabanlarına bağlanmak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. SQL Server, aracı için bir veri kaynağı olarak desteklendiğinden emin olun. Yalnızca aracı ile bağlanmak herhangi bir SQL Server veritabanı ile yaptığınız gibi bağlandıktan sonra esnek sorgu veritabanını ve bu veritabanındaki dış tablolara bakın.

> [!IMPORTANT]
> Esnek sorgularıyla Azure Active Directory'yi kullanarak kimlik doğrulaması şu anda desteklenmiyor.
> 
> 

## <a name="cost"></a>Maliyet
Esnek sorgu Azure SQL veritabanı veritabanları maliyet dahil edilir. Uzak veritabanlarınızı nerede esnek sorgu uç nokta değerinden farklı veri merkezindeki topolojileri desteklenir, ancak uzak veritabanlarından veri çıkışı normal ücretlendirilirsiniz Not [Azure oranları](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Önizleme sınırlamaları
* İlk esnek sorgunuzu çalıştıran alabilir standart performans katman birkaç dakika. Bu süre esnek sorgu işlevini yüklemek gereklidir; Performans yüklenirken daha yüksek performans katmanlarıyla artırır.
* Dış veri kaynakları veya dış tablolara SSMS veya SSDT komut dosyası yazma henüz desteklenmiyor.
* İçeri/dışarı aktarma SQL DB için dış veri kaynakları ve dış tablolar henüz desteklemiyor. İçeri/dışarı aktarma kullanmanız gerekiyorsa, dışarı aktarmadan önce bu nesneleri bırakın ve ardından bunları aldıktan sonra yeniden oluşturun.
* Esnek sorgu şu anda yalnızca dış tablolara salt okunur erişimi destekler. Ancak, T-SQL işlevlerini veritabanı üzerinde dış tablo tanımlandığı kullanabilirsiniz. Bu, örn., geçici sonuçları kullanarak, örneğin kalıcı olması, < column_list > < local_table > seçin, veya dış tablolara başvuran esnek sorgu veritabanı üzerinde saklı yordamlar tanımlamak için yararlı olabilir.
* Dış tablo tanımlarında nvarchar(max) hariç LOB türleri desteklenmez. Geçici bir çözüm olarak, nvarchar(max) LOB türe çevirir Uzak veritabanı üzerinde bir görünüm oluşturun, temel tablo yerine görünüm üzerinden, dış tablo tanımlayabilir ve ardından özgün LOB türü sorgularınızda cast.
* Sütun istatistikleri dış tablolara şu anda desteklenmemektedir. Tablo istatistikleri desteklenir, ancak el ile oluşturulması gerekir.

## <a name="feedback"></a>Geri Bildirim
Lütfen deneyiminizi geribildirim esnek sorgularıyla bizimle Livefyre aşağıdaki MSDN Forumları veya Stackoverflow paylaşın. Hizmet (kusurları, pürüzleri özelliği boşluklar) hakkındaki görüşlerinizi her türlü ilginizi duyuyoruz.

## <a name="next-steps"></a>Sonraki adımlar

* Dikey bölümleme öğretici için bkz: [(dikey bölümleme) veritabanları arası sorgusu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Dikey olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [dikey sorgulama bölümlenmiş veri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) bir öğretici için bkz: [yatay (parçalama) bölümleme için esnek sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started.md).
* Yatay olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [yatay sorgulama bölümlenmiş veri)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) tek uzaktan Azure SQL veritabanı ya da yatay bölümleme düzenindeki parça olarak hizmet veren bir veritabanları kümesi üzerinde bir Transact-SQL deyimini yürütür bir saklı yordam için.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
