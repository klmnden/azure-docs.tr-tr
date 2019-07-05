---
title: Azure SQL veritabanı esnek sorgu genel bakış | Microsoft Docs
description: Esnek sorgu, birden çok veritabanını kapsayan bir Transact-SQL sorgusu çalıştırmanızı sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
manager: craigg
ms.date: 07/01/2019
ms.openlocfilehash: 5188862c50895c8e3f1bdecb4e08d39409bb5f9e
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491661"
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Azure SQL veritabanı esnek sorgu genel bakış (Önizleme)

Esnek sorgu özelliği (önizlemede), Azure SQL veritabanında birden çok veritabanını kapsayan bir Transact-SQL sorgusunu çalıştırmayı sağlar. Uzak tablolara erişen ve Microsoft ve üçüncü taraf araçları (Excel, Power BI, Tableau, vb.) birden fazla veritabanıyla veri katmanlarında sorgulamak için bağlanmak için veritabanları arası sorgular gerçekleştirmenize olanak tanır. Bu özelliği kullanarak, SQL veritabanı'nda büyük veri katmanları için sorgu ölçeği ve iş zekası (BI) raporlarında sonuçları görselleştirebilirsiniz.

## <a name="why-use-elastic-queries"></a>Elastik sorgular neden kullanılmalıdır

### <a name="azure-sql-database"></a>Azure SQL Veritabanı

T-SQL de tamamen Azure SQL veritabanlarında sorgulama yapma. Bu salt okunur uzak veritabanlarını sorgulama için sağlar ve üç ve dört part adları ya da bağlantılı sunucuya SQL DB kullanarak uygulama konusunda geçerli şirket içi SQL Server müşterileri için bir seçenek sunar.

### <a name="available-on-standard-tier"></a>Standart katmanda kullanılabilir

Esnek sorgu standart ve Premium hizmet katmanlarda desteklenir. Daha düşük hizmet katmanlarına yönelik performans sınırlamaları üzerinde Önizleme sınırlamaları bölümüne bakın.

### <a name="push-parameters-to-remote-databases"></a>Uzak veritabanı için anında iletme parametreleri

Elastik sorgular yürütme için Uzak veritabanı için SQL parametreleri artık gönderebilirsiniz.

### <a name="stored-procedure-execution"></a>Saklı yordam yürütme

Uzak saklı yordam çağrıları veya uzak işlevleri kullanarak yürütme [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714).

### <a name="flexibility"></a>Esneklik

Dış tablolar ile esnek sorgu, farklı bir şema veya tablo adı ile uzak tablolara başvurabilir.

## <a name="elastic-query-scenarios"></a>Esnek sorgu senaryoları

Burada birden çok veritabanını tek bir genel sonuçta satırları katkıda sorgulanırken senaryoları kolaylaştırmak için hedeftir. Sorgu ya da kullanıcı veya uygulamaya doğrudan veya dolaylı olarak veritabanına bağlı Araçlar üzerinden oluşabilir. Bu rapor, ticari BI veya değiştirilemez herhangi bir uygulama veya veri tümleştirme araçlarını kullanarak oluştururken kullanışlıdır. İle esnek sorgu, tanıdık SQL Server bağlantı deneyimi Excel, Power BI, Tableau veya Cognos gibi araçları kullanarak birkaç veritabanlarında sorgulama yapabilirsiniz.
Elastik sorgu veritabanlarını SQL Server Management Studio veya Visual Studio tarafından verilen sorgu yoluyla koleksiyonunun tamamını bir kolayca erişmenizi sağlar ve Entity Framework ya da diğer ORM ortamlarda çapraz veritabanı sorgulama kolaylaştırır. Şekil 1'burada mevcut bir izin ver bulut uygulaması bir senaryo gösterilmektedir (kullanan [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)) yapılar ölçeklendirilmiş veri katmanı ve elastik sorgu çapraz veritabanı raporlama için kullanılır.

**Şekil 1** ölçeklendirilmiş veri katmanı üzerinde kullanılan esnek sorgu

![Ölçeği genişletilmiş veri katmanında kullanılan esnek sorgu][1]

Müşteri senaryoları için esnek sorgu, aşağıdaki topolojilerden tarafından belirlenir:

* **Dikey bölümleme - veritabanları arası sorgular** (topoloji 1): Veri birkaç veri katmanındaki veritabanları arasında dikey olarak bölümlenmiş. Genellikle, farklı veritabanları üzerinde tablolar farklı kümesi bulunur. Şema farklı veritabanları üzerinde farklı olduğu anlamına gelir. Hesap oluşturma ile ilgili tüm tabloları üzerinde ikinci bir veritabanı örneği için envanteri için tüm tabloları bir veritabanında bağlıdır. Bu topoloji ile genel kullanım örnekleri arasında sorgu ya da birden fazla veritabanı tablolarında arasında raporlar derlemek için gerektirir.
* **Yatay bölümleme - parçalama** (topoloji 2): Veri bölümlenmiş yatay satır ölçeği genişletilmiş veri arasında dağıtmak için katmanı. Bu yaklaşımda, katılan tüm veritabanlarında şema aynıdır. Bu yaklaşım, "parçalama" olarak da adlandırılır. Parçalama gerçekleştirilebilir ve kitaplıkları veya (2) self-parçalama yönetilen kullanarak (1) esnek veritabanı araçları. Elastik sorgu, sorgu veya rapor birçok parça arasında derleme için kullanılır.

> [!NOTE]
> Esnek sorgu, işlem (filtreleme, toplama) çoğu dış kaynak tarafında nereye gerçekleştirilebilir senaryoları raporlama için en iyi çalışır. Büyük miktarda veri uzak veritabanlarından burada aktarıldığı ETL işlemleri için uygun değildir. Raporlama ağır iş yükleri veya veri ambarı senaryoları daha karmaşık sorgular ile için ayrıca kullanmayı [Azure SQL veri ambarı](https://azure.microsoft.com/services/sql-data-warehouse/).
>  

## <a name="vertical-partitioning---cross-database-queries"></a>Dikey bölümleme - veritabanları arası sorgular

Kodlamaya başlamak için bkz: [(dikey bölümlendirme) veritabanları arası sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).

Elastik sorgu, diğer SQL veritabanları için kullanılabilen bir SQL veritabanı'nda bulunan verilere yapmak için kullanılabilir. Bu, diğer herhangi bir uzak SQL veritabanı tablolarında başvurmak için bir veritabanı sorgularından sağlar. İlk adım, her uzak veritabanı için bir dış veri kaynağı tanımlamaktır. Dış veri kaynağı, Uzak veritabanı üzerinde bulunan tabloları erişmesini istediğiniz yerel veritabanında tanımlanır. Uzak veritabanı üzerinde herhangi bir değişiklik gereklidir. Farklı veritabanlarındaki farklı şemalar sahip olduğu dikey bölümleme için tipik senaryolar, esnek sorgular başvuru verilerine erişim gibi ortak kullanım durumları uygulamak ve veritabanları arası sorgulama için kullanılabilir.

> [!IMPORTANT]
> ALTER ANY dış veri kaynağı iznine sahip olması gerekir. Bu izne ALTER DATABASE izni dahil edilir. Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.
>

**Başvuru verileri**: Topoloji başvuru verileri yönetimi için kullanılır. Aşağıdaki çizimde, iki tablo (T1 ve T2) başvuru verileriyle adanmış bir veritabanında tutulur. Elastik sorgu kullanarak, artık tabloları T1 ve T2 uzaktan diğer veritabanlarındaki verileri aşağıdaki şekilde gösterildiği gibi erişebilirsiniz. Kullanım topoloji başvuru tabloları küçük ya da uzak sorguları başvuru tablosu varsa 1 seçmeli koşullar vardır.

**Şekil 2** dikey bölümleme - elastik sorgu için sorgu başvuru verilerini kullanma

![Dikey bölümleme - elastik sorgu için sorgu başvuru verilerini kullanma][3]

**Çapraz veritabanı sorgulama**: Elastik sorgular birkaç SQL veritabanlarında sorgulama gerektiren durumlara etkinleştirin. Şekil 3'te, dört farklı veritabanı gösterir: CRM, Envanter, İK ve ürünler. Veritabanlarından birini gerçekleştirilen sorgular ayrıca bir veya tüm diğer veritabanlarına erişim gerekir. Elastik sorgu kullanarak, birkaç basit DDL deyimleri her dört veritabanı çalıştırarak veritabanınızı bu çalışması için yapılandırabilirsiniz. Bu tek seferlik yapılandırmadan sonra uzak tabloya erişim, T-SQL sorguları ya da BI araçları, yerel bir tabloya başvuran olarak kadar kolaydır. Uzak sorgular büyük sonuç döndürmezse bu yaklaşım önerilir.

**Şekil 3** dikey bölümleme - çeşitli veritabanı arasında esnek sorgu için sorgu kullanma

![Dikey bölümleme - çeşitli veritabanı arasında esnek sorgu için sorgu kullanma][4]

Uzak SQL veritabanlarında aynı şemaya bulunan bir tabloya erişim gerektiren dikey bölümleme senaryolar için elastik veritabanı sorgusu aşağıdakileri yapılandırın:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [CREATE/DROP dış veri kaynağı](https://msdn.microsoft.com/library/dn935022.aspx) gelen veriKaynağım'a türü **RDBMS**
* [Dış tablo oluşturma/bırakma](https://msdn.microsoft.com/library/dn935021.aspx) mytable

İşlevmiş gibi yerel bir tablo DDL deyimleri çalıştırdıktan sonra uzak tablo "mytable" erişebilirsiniz. Azure SQL veritabanı otomatik olarak bir uzak veritabanına bağlantı açar, Uzak veritabanı üzerinde talebinizi ve sonuçları döndürür.

## <a name="horizontal-partitioning---sharding"></a>Yatay bölümleme - parçalama

Raporlama görevleri parçalı gerçekleştirmek için elastik sorgu kullanarak, diğer bir deyişle, yatay, veri katmanı gerektirir bölümlenmiş bir [esnek veritabanı parça eşlemesi](sql-database-elastic-scale-shard-map-management.md) veri katmanındaki veritabanlarını temsil etmek için. Genellikle, bu senaryoda yalnızca bir tek parça eşlemesi kullanılır ve bir adanmış veritabanı esnek sorgu özellikleri (baş düğüm) ile sorguları raporlama için giriş noktası işlevi görür. Bu adanmış veritabanı, parça eşlemesi erişmesi gerekir. Şekil 4, bu topoloji ve esnek sorgu veritabanı ve parça Haritası ile yapılandırmasını gösterir. Veri katmanındaki veritabanlarını, tüm Azure SQL veritabanı sürümü veya sürümü olabilir. Elastik veritabanı istemci kitaplığı ve parça Haritası oluşturma hakkında daha fazla bilgi için bkz. [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md).

**Şekil 4** yatay bölümleme - parçalı veriler katmanı raporlama için elastik sorgu kullanarak

![Yatay bölümleme - parçalı veriler katmanı raporlama için elastik sorgu kullanarak][5]

> [!NOTE]
> Elastik sorgu veritabanı (baş düğüm), ayrı bir veritabanı veya parça eşlemesi barındıran aynı veritabanı olabilir.
> Seçin, söz konusu hizmet katmanı emin olun ve sonra işlem, veritabanı boyutu ne olursa olsun beklenen miktarı oturum açma/sorgu isteği işlemek için yeterince yüksek bir yapılandırmadır.

Aşağıdaki adımları (genellikle) birden çok uzak SQL veritabanlarında bulunan tabloları kümesi erişmesi yatay bölümleme senaryoları için esnek veritabanı sorguları yapılandırın:

* [CREATE MASTER KEY](https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql) mycredential
* Oluşturma bir [parça eşlemesi](sql-database-elastic-scale-shard-map-management.md) elastik veritabanı istemci kitaplığını kullanarak veri katmanınızın temsil eden.
* [CREATE/DROP dış veri kaynağı](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql) gelen veriKaynağım'a türü **SHARD_MAP_MANAGER**
* [Dış tablo oluşturma/bırakma](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) mytable

Bu adımları gerçekleştirdikten sonra işlevmiş gibi yerel bir tablo yatay olarak bölümlenmiş tabloda "mytable" erişebilirsiniz. Azure SQL veritabanı otomatik olarak birden çok paralel tablolarda fiziksel olarak depolandığı uzak veritabanlarına bağlantı açar, uzak veritabanlarında isteklerini işleyen ve sonuçları döndürür.
Yatay bölümleme senaryo bulunabilir için gereken adımlar hakkında daha fazla bilgi [yatay bölümleme için esnek sorgu](sql-database-elastic-query-horizontal-partitioning.md).

Kodlamaya başlamak için bkz: [yatay bölümleme (parçalama) için esnek sorgu kullanmaya başlama](sql-database-elastic-query-getting-started.md).

## <a name="t-sql-querying"></a>T-SQL sorgulama

Dış veri kaynaklarınızı ve dış tablolarınızı tanımlandıktan sonra dış tabloların tanımlandığı veritabanlarına bağlanmak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. Ardından T-SQL deyimleriyle dış tablolarınızı bu bağlantıda aşağıda verilen kısıtlamalarla çalıştırabilirsiniz. Daha fazla bilgi ve örnek T-SQL sorguları belgeleri konusundan bulabilirsiniz [yatay bölümleme](sql-database-elastic-query-horizontal-partitioning.md) ve [dikey bölümleme](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Bağlantı için Araçlar

Uygulamalar ve BI veya veri tümleştirme araçlarına sahip dış tablolar veritabanlarına bağlanmak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. SQL Server'ın aracınız için bir veri kaynağı olarak desteklendiğinden emin olun. Bağlandıktan sonra esnek sorgu veritabanına başvurun ve bu veritabanında olduğu gibi dış tablolar, aracı ile bağlanmanız herhangi bir SQL Server veritabanı ile yaptığınız.

> [!IMPORTANT]
> Elastik sorgular kullanmaya Azure Active Directory'yi kullanarak kimlik doğrulaması şu anda desteklenmiyor.

## <a name="cost"></a>Maliyet

Esnek sorgu, Azure SQL veritabanı veritabanları maliyetini dahil edilir. Uzak veritabanlarınızı nerede daha esnek sorgu uç nokta bir başka veri merkezindeki topolojileri desteklenir, ancak uzak veritabanlarından veri çıkışı düzenli olarak ücret unutmayın [Azure ücretleri](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Çalıştıran ilk esnek sorgunuzu alabilir standart hizmet katmanında işlem birkaç dakika için. Bu süre, elastik sorgu işlevini yüklemek gereklidir; daha yüksek hizmet katmanları ve bilgi işlem boyutlarına ile yükleme performansını artırır.
* Dış veri kaynaklarına veya dış tablolar SSMS veya SSDT betik henüz desteklenmiyor.
* İçeri/dışarı aktarma SQL DB, dış veri kaynakları ve dış tablolar henüz desteklemiyor. İçeri/dışarı aktarma kullanmanız gerekiyorsa, dışarı aktarmadan önce bu nesneleri bırakın ve sonra bunları içeri aktardıktan sonra yeniden oluşturun.
* Esnek sorgu şu anda yalnızca dış tablolar yalnızca okuma erişimi destekler. Ancak, tüm T-SQL işlevleri veritabanında dış tablo tanımlandığı kullanabilirsiniz. Örneğin, geçici sonuçları kullanarak, örneğin kalıcı, < local_table > < column_list >'i seçin, veya dış tablolara başvuran esnek sorgu veritabanında saklı yordamlar tanımlamak için yararlı olabilir.
* Nvarchar(max) dışında dış tablo tanımlarında LOB türleri (uzamsal türler dahil) desteklenmez. Geçici bir çözüm olarak, nvarchar(max) LOB türü bıraktığı Uzak veritabanı üzerinde bir görünüm oluşturun, bir görünüm yerine temel tablo üzerinden, dış tablo tanımlayabilir ve ardından sorgularınızdaki özgün LOB türe geri dönüştürme.
* Sonuç kümesi devre dışı olarak nvarchar(Maks.) veri türü sütunlar büyüklük kertesinde için sorgu performansını etkileyebilir ve esnek sorgu uygulamasında kullanılan teknikleri toplu işleme Gelişmiş ya da büyük olduğunda bile iki kat kurallı olmayan, kullanım örnekleri miktarı Toplu olmayan veriler sorgu sonucu olarak aktarılır.
* Dış tablolara sütun istatistikleri şu anda desteklenmemektedir. Tablo istatistikleri desteklenir, ancak el ile oluşturulması gerekir.
* Esnek sorgu, yalnızca Azure SQL veritabanı ile çalışır. Şirket içi SQL Server veya SQL Server bir sanal makinede sorgulama için kullanılamaz.

## <a name="feedback"></a>Geri Bildirim

Deneyiminiz hakkında geri bildirim aşağıdaki bizimle birlikte, MSDN Forumlarında veya Stack overflow'da esnek sorgular ile paylaşın. (Hatalar, pürüzleri özellik boşluklarına) hizmeti hakkında geri bildirim her türlü ilgileniriz.

## <a name="next-steps"></a>Sonraki adımlar

* Dikey bölümleme öğreticisi için bkz. [(dikey bölümlendirme) veritabanları arası sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Dikey olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama dikey olarak bölümlenmiş verileri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) bir öğretici için bkz. [yatay bölümleme (parçalama) için esnek sorgu kullanmaya başlama](sql-database-elastic-query-getting-started.md).
* Yatay olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama yatay olarak bölümlenmiş veriler)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) parçalarda bir yatay bölümleme düzeni olarak hizmet veren bir veritabanları kümesi veya bir uzak tek Azure SQL veritabanı Transact-SQL deyimini yürütür bir saklı yordam için.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
