---
title: Ölçeği genişletilen bulut veritabanlarında raporlama | Microsoft Docs
description: Yatay bölümler üzerinde esnek sorguları ayarlama
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
ms.date: 01/03/2019
ms.openlocfilehash: 3b2b472407175df307c569704d4c7611737c4ea1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60694353"
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>(Önizleme) ölçeği genişletilen bulut veritabanlarında raporlama

![Parça arasında sorgu][1]

Parçalı veritabanları dağıtmak satırlar arasında ölçeği genişletilmiş veri katmanı. Yatay bölümleme olarak da bilinen katılan tüm veritabanlarında şema aynıdır. Elastik sorgu kullanarak, parçalı bir veritabanındaki tüm veritabanlarına yayılan raporlar oluşturabilirsiniz.

Hızlı Başlangıç için bkz: [ölçeği genişletilen bulut veritabanlarında raporlama](sql-database-elastic-query-getting-started.md).

Parçalı olmayan veritabanları için bkz: [sorgu farklı şemalarla bulut veritabanlarında](sql-database-elastic-query-vertical-partitioning.md).

## <a name="prerequisites"></a>Önkoşullar

* Elastik veritabanı istemci kitaplığını kullanarak bir parça eşlemesi oluşturun. bkz: [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md). Veya örnek uygulama kullanma [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).
* Alternatif olarak, [ölçeği genişletilmiş veritabanları için mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md).
* Kullanıcı, ALTER ANY dış veri kaynağı iznine sahip olması gerekir. Bu izne ALTER DATABASE izni dahil edilir.
* Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

## <a name="overview"></a>Genel Bakış

Bu deyimler, esnek sorgu veritabanında parçalı veriler katmanınızın meta veri gösterimine oluşturun.

1. [ANA ANAHTAR OLUŞTURUN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [OLUŞTURMA VERİTABANI KAPSAMLI KİMLİK BİLGİLERİ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [DIŞ VERİ KAYNAĞI OLUŞTURMA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [DIŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx)

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 kapsamlı veritabanı ana anahtarı ve kimlik bilgileri oluşturma

Kimlik bilgisi tarafından esnek sorgu, uzak veritabanlarına bağlanmak için kullanılır.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Emin olun *"\<kullanıcıadı\>"* içermez *"\@servername"* soneki.

## <a name="12-create-external-data-sources"></a>1.2 dış veri kaynakları oluşturma

Sözdizimi:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>,
            SHARD_MAP_NAME = ‘<shardmapname>’
                   ) [;]

### <a name="example"></a>Örnek

    CREATE EXTERNAL DATA SOURCE MyExtSrc
    WITH
    (
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net',
        DATABASE_NAME='ShardMapDatabase',
        CREDENTIAL= SMMUser,
        SHARD_MAP_NAME='ShardMap'
    );

Geçerli dış veri kaynakları listesini alın:

    select * from sys.external_data_sources;

Dış veri kaynağı, parça eşlemesi başvuruyor. Elastik sorgu ardından dış veri kaynağı ve temel alınan parça eşlemesi veri katmanında katılan veritabanları numaralandırmak için kullanır.
Aynı kimlik bilgileri, parça eşlemesi okuyun ve elastik sorgu işlenmesi sırasında parçalar verilere erişmek için kullanılır.

## <a name="13-create-external-tables"></a>1.3 dış tablolar oluşturma

Sözdizimi:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::=
      DATA_SOURCE = <External_Data_Source>,
      [ SCHEMA_NAME = N'nonescaped_schema_name',]
      [ OBJECT_NAME = N'nonescaped_object_name',]
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Örnek**

    CREATE EXTERNAL TABLE [dbo].[order_line](
         [ol_o_id] int NOT NULL,
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL,
         [ol_number] tinyint NOT NULL,
         [ol_i_id] int NOT NULL,
         [ol_delivery_d] datetime NOT NULL,
         [ol_amount] smallmoney NOT NULL,
         [ol_supply_w_id] int NOT NULL,
         [ol_quantity] smallint NOT NULL,
         [ol_dist_info] char(24) NOT NULL
    )

    WITH
    (
        DATA_SOURCE = MyExtSrc,
         SCHEMA_NAME = 'orders',
         OBJECT_NAME = 'order_details',
        DISTRIBUTION=SHARDED(ol_w_id)
    );

Geçerli veritabanından dış tabloların listesini alın:

    SELECT * from sys.external_tables;

Dış tablolar bırakmak için:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Açıklamalar

VERİ\_kaynak yan tümcesi için dış tablo kullanılan dış veri kaynağı (parça eşlemesi) tanımlar.  

ŞEMA\_adı ve nesne\_adı yan tümceleri bir tabloda farklı bir şeması için dış tablo tanımındaki eşleme. Belirtilmezse, şema uzak nesnenin "dbo" olarak kabul edilir ve adını tanımlanmakta olan dış tablo adıyla aynı olduğu varsayılır. Bu, uzak tablo adı zaten veritabanında bir dış tablo oluşturmak istediğiniz alınmışsa yararlı olur. Örneğin, Katalog görünümleri birleşik bir görünümünü elde etmek için bir dış tablo tanımlamak istediğiniz veya ölçeği genişletilmiş veri çubuğunda Dmv'leri katmanı. Katalog görünümleri ve Dmv'leri zaten yerel olarak mevcut olduğundan, kendi adları için dış tablo tanımındaki kullanamazsınız. Bunun yerine, farklı bir ad kullanın ve Katalog görünümün veya DMV'ın şema adı\_adı ve/veya nesne\_adı yan tümceleri. (Aşağıdaki örneğe bakın.)

Dağıtım yan tümcesi Bu tablo için kullanılan veri dağılımını belirtir. Sorgu işlemcisi dağıtım yan tümcesinde en verimli sorgu planlarına oluşturmak için sağlanan bilgileri kullanır.

1. **PARÇALI** veri veritabanları arasında yatay olarak bölümlenir anlamına gelir. Veri dağıtımı için bölümleme anahtarıdır **< sharding_column_name >** parametresi.
2. **ÇOĞALTILAN** tablo eşdeğer kopyalarını her veritabanında mevcut olduğu anlamına gelir. Bu veritabanları arasında çoğaltmaları aynı olduğundan emin olmak sizin sorumluluğunuzdur.
3. **YUVARLAK\_ROBIN** uygulama bağımlı dağıtım yöntemi kullanarak, tabloyu yatay olarak bölümlenen anlamına gelir.

**Veri katmanı başvuru**: Dış tablo DDL bir dış veri kaynağına başvuruyor. Dış tablo bütün veritabanlarını veri katmanınızın bulmak gereken bilgileri sağlayan bir parça eşlemesi dış veri kaynağını belirtir.

### <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Dış tablo erişimi olan kullanıcılar temel alınan uzak tablolar dış veri kaynağı tanımına verilen kimlik bilgisi altında otomatik olarak erişin. Dış veri kaynağının kimlik bilgisi üzerinden istenmeyen ayrıcalık kaçının. Yalnızca işlevmiş gibi olağan bir tablo için dış tablo GRANT veya REVOKE kullanın.  

Dış veri kaynağı ve dış tablolarınızı tanımlandıktan sonra tam T-SQL artık dış tablolarınızı kullanabilirsiniz.

## <a name="example-querying-horizontal-partitioned-databases"></a>Örnek: yatay bölümlenmiş veritabanlarını sorgulama

Aşağıdaki sorgu, ambar, sipariş ve satış siparişi arasında üç yönlü birleştirme gerçekleştirir ve birkaç toplamlar ve seçmeli bir filtre kullanır. (1) yatay bölümleme (parçalama) ve ambar, sipariş ve satış siparişi ambarı kimlik sütunu örneğe göre parçalı olduğunu (2) ve esnek sorgu birleştirmeler parçalar üzerinde bir arada tutun ve parçalar üzerindeki sorgu pahalı parçası işleme varsayılır Paralel.

```sql
    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount,
         min(ol_delivery_d) as min_deliv_date
    from warehouse
    join orders
    on w_id = o_w_id
    join order_line
    on o_id = ol_o_id and o_w_id = ol_w_id
    where w_id > 100 and w_id < 200
    group by w_id, o_c_id
```

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Saklı yordamı uzaktan T-SQL yürütme için: sp\_execute_remote

Esnek sorgu, parçalar doğrudan erişim sağlayan bir saklı yordam da tanıtılmaktadır. Saklı yordamı çağrılır [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) ve uzak veritabanlarında uzak saklı yordamları ya da T-SQL kodunu çalıştırmak için kullanılabilir. Bunu, aşağıdaki parametreleri alır:

* Veri kaynağı adı (nvarchar): RDBMS türündeki dış veri kaynağının adı.
* Sorgu (nvarchar): Her parça üzerinde yürütülecek T-SQL sorgusu.
* Parametre bildirimi (nvarchar) - isteğe bağlı: Dize (gibi sp_executesql) sorgu parametresi olarak kullanılan parametreler için tür tanımları ile verileri.
* Parametre değeri listesi - isteğe bağlı: Virgülle ayrılmış listesi (gibi sp_executesql) parametre değerleri.

Sp\_yürütme\_uzaktan uzak veritabanlarında belirli T-SQL deyimi yürütmek için çağırma parametreleri sağlanan dış veri kaynağı kullanır. Dış veri kaynağının kimlik bilgisi shardmap manager veritabanını hem de uzak veritabanlarına bağlanmak için kullanır.  

Örnek:

```sql
    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse'
```

## <a name="connectivity-for-tools"></a>Bağlantı için Araçlar

Uygulamanızı bağlamak için normal SQL Server bağlantı dizelerini kullanma dış tablo tanımlarınızı veritabanıyla, BI ve veri tümleştirme araçları. SQL Server'ın aracınız için bir veri kaynağı olarak desteklendiğinden emin olun. Yerel tablolar değilmiş gibi ardından başvuru gibi herhangi bir SQL Server veritabanı esnek sorgu veritabanı aracı ve kullanım dış tablolar için aracı ya da uygulama bağlı.

## <a name="best-practices"></a>En iyi uygulamalar

* Esnek sorgu bitiş noktası veritabanı erişim shardmap veritabanı ve SQL DB güvenlik duvarları üzerinden tüm parçalar verildi emin olun.  
* Doğrulama veya dış tablo tarafından tanımlanan veri dağıtım zorlamaz. Gerçek veri dağıtımınız farklıysa, tablo tanımında belirtilen dağıtım sorgularınızı beklenmeyen sonuçlara neden olabilir.
* Esnek sorgu doğrulamaları parçalama anahtarı üzerinden güvenli bir şekilde işlenmesini bazı parçalar hariç tutmak için izin parça ortadan kaldırılması şu anda gerçekleştirmez.
* Esnek sorgu parçalar üzerinde hesaplama çoğunu burada yapılabilir sorgular için en iyi şekilde çalışır. Normalde en iyi sorgu performansını değerlendirilebilir seçmeli filtre koşullarla parçalar ya da birleştirme bölümleme anahtarlarınızın bir bölüme göre hizalı şekilde tüm parçalar üzerinde gerçekleştirilebilir alabilirsiniz. Diğer sorgu desenleri sonlanmayacağından ve büyük miktarlarda verinin parçadan baş düğüme yük gerekebilir

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz. [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey bölümleme öğreticisi için bkz. [(dikey bölümlendirme) veritabanları arası sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Dikey olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama dikey olarak bölümlenmiş verileri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) bir öğretici için bkz. [yatay bölümleme (parçalama) için esnek sorgu kullanmaya başlama](sql-database-elastic-query-getting-started.md).
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) parçalarda bir yatay bölümleme düzeni olarak hizmet veren bir veritabanları kümesi veya bir uzak tek Azure SQL veritabanı Transact-SQL deyimini yürütür bir saklı yordam için.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
