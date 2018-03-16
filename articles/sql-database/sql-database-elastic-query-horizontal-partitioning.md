---
title: "Ölçeklendirilen bulut veritabanları arasında raporlama | Microsoft Docs"
description: "Yatay bölümleri esnek sorgulamalarını ayarlama"
services: sql-database
documentationcenter: 
manager: craigg
author: MladjoA
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 05/27/2016
ms.author: mlandzic
ms.openlocfilehash: ec47a10fcfcb3ef52810ba2b3da9599b65db375a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Raporlama ölçeklendirilmiş bulut veritabanları arasında (Önizleme)
![Parça sorgulama][1]

Parçalı veritabanları dağıtmak satırları genişletilmiş bir veri katmanı. Şema yatay bölümleme olarak da bilinen tüm katılımcı veritabanlarında aynıdır. Esnek bir sorgu kullanarak, tüm veritabanları parçalı bir veritabanında span raporlar oluşturabilirsiniz.

Hızlı Başlangıç için bkz: [ölçeklendirilmiş bulut veritabanları arasında raporlama](sql-database-elastic-query-getting-started.md).

Parçalı dışı veritabanları için bkz: [sorgu farklı şemalarda bulut veritabanları arasında](sql-database-elastic-query-vertical-partitioning.md). 

## <a name="prerequisites"></a>Önkoşullar
* Esnek veritabanı istemci kitaplığı kullanılarak bir parça Haritası oluşturun. bkz: [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md). Veya örnek uygulama kullanma [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).
* Alternatif olarak, bkz: [ölçeklendirilmiş veritabanları için var olan veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md).
* Kullanıcı ALTER ANY dış veri KAYNAĞINA iznine sahip olması gerekir. Bu izin ALTER DATABASE izniyle dahil edilir.
* Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

## <a name="overview"></a>Genel Bakış
Bu ifadeler, parçalı veri katmanı meta veri gösterimi esnek sorgu veritabanında oluşturun. 

1. [ANA ANAHTAR OLUŞTURMA](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VERİTABANI KAPSAMLI OLUŞTURMAK KİMLİK BİLGİSİ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [DIŞ VERİ KAYNAĞI OLUŞTURUN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [DIŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 kapsamlı veritabanı ana anahtarı ve kimlik bilgileri oluşturun
Kimlik bilgisi esnek sorgu tarafından uzak veritabanlarına bağlanmak için kullanılır.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Olduğundan emin olun *"\<kullanıcıadı\>"* içermez *"@servername"* soneki. 
> 
> 

## <a name="12-create-external-data-sources"></a>1.2 dış veri kaynakları oluşturun
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

Geçerli dış veri kaynaklarının listesini alın: 

    select * from sys.external_data_sources; 

Parça haritanızı dış veri kaynağına başvuruyor. Esnek bir sorgu sonra dış veri kaynağı ve arka plandaki parça eşleme veri katmanı katılan veritabanları numaralandırmak için kullanır. Aynı kimlik bilgilerini parça eşleme okumak için ve esnek sorgu işleme sırasında parça verilere erişmek için kullanılır. 

## <a name="13-create-external-tables"></a>1.3 dış tabloları oluşturma
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

Dış tablo listesini geçerli veritabanından: 

    SELECT * from sys.external_tables; 

Dış tablolara bırakmak için:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Açıklamalar
VERİ\_SOURCE yan tümcesinde dış tablo için kullanılan dış veri kaynağı (parça eşleme) tanımlar.  

ŞEMA\_adı ve nesne\_adı yan tümceleri farklı bir şema tablosuna dış tablo tanımındaki eşleyin. Belirtilmezse, şema uzak nesnesinin "dbo" olarak kabul edilir ve adını tanımlanmakta dış tablo adıyla aynı olduğu varsayılır. Bu, uzak tablo adı zaten veritabanında dış tablo oluşturmak istediğiniz alınmışsa yararlı olur. Örneğin, Katalog görünümleri birleşik bir görünümünü almak için bir dış tablo tanımlamak istediğiniz veya genişletilmiş verileriniz üzerinde Dmv'leri katmanı. Katalog görünümleri ve Dmv'leri zaten yerel olarak mevcut olduğundan, dış tablo tanımı için adları kullanamazsınız. Bunun yerine, farklı bir ad kullanın ve Katalog görünümün veya DMV'ın adını şemasında\_adı ve/veya nesne\_adı yan tümceleri. (Aşağıdaki örneğe bakın.) 

Dağıtım yan tümcesi Bu tablo için kullanılan veri dağılımını belirtir. Sorgu işlemcisi dağıtım yan tümcesinde en verimli sorgu planları oluşturmak için sağlanan bilgileri kullanır.  

1. **PARÇALI** veri veritabanları arasında yatay olarak bölümlenmiş anlamına gelir. Veri dağıtımı için bölümleme anahtar **< sharding_column_name >** parametresi.
2. **ÇOĞALTILAN** tablonun aynı kopyasını her veritabanını mevcut olduğu anlamına gelir. Çoğaltmaları veritabanları arasında aynı olduğundan emin olmak için sizin sorumluluğunuzdadır değil.
3. **YUVARLAK\_deneme** bir uygulama bağımlı dağıtım yöntemi kullanarak, tablonun yatay bölümlenen anlamına gelir. 

**Veri katmanı başvuru**: dış tablo DDL bir dış veri kaynağına başvuruyor. Dış veri kaynağı, dış tablo, veri katmanındaki tüm veritabanları bulmak gereken bilgileri sağlayan bir parça eşleme belirtir. 

### <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
Dış tablo erişimi olan kullanıcılar otomatik olarak temel uzak tablolar dış veri kaynağı tanımında belirtilen kimlik bilgileri altında erişim kazanır. Dış veri kaynağının kimlik bilgisi aracılığıyla istenmeyen ayrıcalık kaçının. Yalnızca dizinindeymiş gibi olağan bir tablo için bir dış tablo GRANT veya REVOKE kullanın.  

Dış veri kaynağınızda ve dış tablolarınızı tanımladıktan sonra artık dış tablolar üzerindeki tam T-SQL kullanabilirsiniz.

## <a name="example-querying-horizontal-partitioned-databases"></a>Örnek: yatay bölümlenmiş veritabanlarını sorgulama
Aşağıdaki sorgu ambarları, sipariş ve sipariş satırları arasında üç yönlü birleştirme gerçekleştirir ve birkaç toplar ve seçmeli bir filtre kullanır. (1) yatay bölümleme (parçalama) ve ambarları, sipariş ve sipariş satırları ambar kimliği sütuna göre parçalı olduğunu (2) ve esnek sorgu birleştirmeler parça üzerinde birlikte bulunmasına ve parça üzerindeki sorgu pahalı parçası işlem varsayar Paralel. 

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

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Saklı yordamı uzaktan T-SQL yürütmesi için: sp\_execute_remote
Esnek sorgu aynı zamanda parça doğrudan erişim sağlayan bir saklı yordam sunar. Saklı yordam adlı [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) ve uzak saklı yordam veya T-SQL kodunu uzak veritabanlarına yürütmek için kullanılabilir. Aşağıdaki parametreleri alır: 

* Veri kaynağı adı (nvarchar): türü RDBMS dış veri kaynağının adı. 
* Sorgu (nvarchar): her parça yürütülmek üzere T-SQL sorgusu. 
* Parametre bildirimi (nvarchar) - isteğe bağlı: dize veri türü tanımları (gibi sp_executesql) Sorgu parametresinde kullanılan parametreler için. 
* Parametre değeri listesi - isteğe bağlı: parametre değerleri (gibi sp_executesql) virgülle ayrılmış listesi.

Sp\_yürütme\_uzaktan uzak veritabanlarına verilen T-SQL deyimi yürütmek için çağrı parametrelerini sağlanan dış veri kaynağı kullanır. Shardmap manager veritabanı ve uzak veritabanlarına bağlanmak için dış veri kaynağının kimlik bilgilerini kullanır.  

Örnek: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Bağlantı için araçları
Uygulamanızın, bağlanmak için normal SQL Server bağlantı dizelerini kullanın, BI ve veri tümleştirme araçları, dış tablo tanımları içeren veritabanı. SQL Server, aracı için bir veri kaynağı olarak desteklendiğinden emin olun. Yerel tabloları değilmiş gibi sonra başvuru herhangi bir SQL Server veritabanı gibi esnek sorgu veritabanı aracı ve kullanım dış tablolar için aracı ya da uygulama bağlı. 

## <a name="best-practices"></a>En iyi uygulamalar
* Esnek sorgu uç veritabanı erişim shardmap veritabanı ve SQL DB güvenlik duvarı üzerinden tüm parça verildi emin olun.  
* Dış tablo tarafından tanımlanan veri dağıtım zorunlu veya doğrulayın. Gerçek veri dağıtımınız farklıysa, tablo tanımında belirtilen dağıtım sorgularınızı beklenmeyen sonuçlar. 
* Esnek sorgu koşulları parçalama anahtarı üzerinden güvenli bir şekilde işlenmesini belirli parça hariç tutmak için izin parça eleme şu anda gerçekleştirmez.
* Esnek sorgu hesaplama çoğunu üzerinde parça burada yapılabilir sorguları için en iyi şekilde çalışır. Genellikle en iyi sorgu performansını değerlendirilebilir seçmeli filtre koşulları ile parça veya birleştirmeler üzerinde bir bölüme göre hizalı şekilde tüm parça üzerinde gerçekleştirilen bölümleme anahtarları üzerinden alırsınız. Diğer sorgu desenlerine kötü gerçekleştirebilir ve büyük miktarlarda verinin parça baş düğümü yük gerekebilir

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz: [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey bölümleme öğretici için bkz: [(dikey bölümleme) veritabanları arası sorgusu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Dikey olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [dikey sorgulama bölümlenmiş veri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) bir öğretici için bkz: [yatay (parçalama) bölümleme için esnek sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started.md).
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) tek uzaktan Azure SQL veritabanı ya da yatay bölümleme düzenindeki parça olarak hizmet veren bir veritabanları kümesi üzerinde bir Transact-SQL deyimini yürütür bir saklı yordam için.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
