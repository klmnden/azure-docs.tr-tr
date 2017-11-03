---
title: "Sorgu farklı şemasıyla bulut veritabanları arasında | Microsoft Docs"
description: "veritabanları arası sorgulamalarını dikey bölümleri ayarlama"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 84c261f2-9edc-42f4-988c-cf2f251f5eff
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: d57f45066387f451463a38d76d3fe6adab77e41f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Bulut veritabanları farklı şemaları (Önizleme) ile sorgulama
![Farklı veritabanı tablolarında sorgulama][1]

Veritabanlarını dikey olarak bölümlenmiş tabloları kümesi farklı farklı veritabanlarını kullanır. Şema farklı veritabanlarında farklı olduğu anlamına gelir. Örneğin, tüm hesap ilişkili tabloları ikinci bir veritabanı üzerinde çalışırken tüm tablolar için stok bir veritabanında ' dir. 

## <a name="prerequisites"></a>Ön koşullar
* Kullanıcı ALTER ANY dış veri KAYNAĞINA iznine sahip olması gerekir. Bu izin ALTER DATABASE izniyle dahil edilir.
* Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Farklı yatay bölümleme bu DDL deyimleri parça eşlemesiyle esnek veritabanı istemci kitaplığı aracılığıyla veri katmanı tanımlama güvenmeyin.
>

1. [ANA ANAHTAR OLUŞTURMA](https://msdn.microsoft.com/library/ms174382.aspx)
2. [VERİTABANI KAPSAMLI OLUŞTURMAK KİMLİK BİLGİSİ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [DIŞ VERİ KAYNAĞI OLUŞTURUN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [DIŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Kapsamlı veritabanı ana anahtarı ve kimlik bilgileri oluşturun
Kimlik bilgisi esnek sorgu tarafından uzak veritabanlarına bağlanmak için kullanılır.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Emin `<username>` içermez **"@servername"** soneki. 
>

## <a name="create-external-data-sources"></a>Dış veri kaynakları oluşturun
Sözdizimi:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> TÜR parametresi ayarlanmalıdır **RDBMS**. 
>

### <a name="example"></a>Örnek
Aşağıdaki örnek, dış veri kaynakları için CREATE deyimi kullanımını gösterir. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

Geçerli dış veri kaynakları listesini almak için: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Dış tablolar
Sözdizimi:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Örnek
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Aşağıdaki örnek, geçerli veritabanından dış tablolar listesini almak gösterilmektedir: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Açıklamalar
Esnek sorgu türü RDBMS dış veri kaynaklarını kullanmak dış tablolara tanımlamak için var olan dış tablo sözdizimi genişletir. Dikey bölümleme için bir dış tablo tanımındaki aşağıdaki konuları içerir: 

* **Şema**: dış tablo DDL sorgularınızı kullanabileceğiniz bir şema tanımlar. Dış tablo tanımında belirtilen şema gerçek verilerinin depolandığı Uzak veritabanı tablolarında şeması ile eşleşmesi gerekir. 
* **Uzak veritabanı başvuru**: dış tablo DDL bir dış veri kaynağına başvuruyor. Dış veri kaynağı mantıksal sunucu adını ve gerçek tablo verilerinin depolandığı uzak veritabanının veritabanı adını belirtir. 

Bir dış veri kaynağı önceki bölümde özetlendiği gibi kullanarak, dış tablo oluşturma için söz dizimi aşağıdaki gibidir: 

DATA_SOURCE yan tümcesi, dış tablo için kullanılan dış veri kaynağı (dikey bölümleme durumunda yani Uzak veritabanı) tanımlar.  

SCHEMA_NAME ve OBJECT_NAME yan tümcelerini dış tablo tanımındaki bir tablodaki bir uzak veritabanı farklı bir şema veya farklı bir ada sahip bir tablo sırasıyla eşlemek için olanağı sunar. Bu, bir dış tablo katalog görünümünü veya DMV uzak veritabanınızı - veya burada uzak tablo adı zaten yerel olarak alınmış diğer durum tanımlamak istiyorsanız kullanışlıdır.  

Aşağıdaki DDL deyimi var olan bir dış tablo tanımındaki yerel Kataloğu'ndan bırakır. Uzak veritabanı etkilemez. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**CREATE/DROP dış tablo izinlerini**: dış tablo, ayrıca temel alınan veri kaynağına başvurmak için gerekli olan DDL için ALTER ANY dış veri kaynağı izinleri gerekiyor.  

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
Dış tablo erişimi olan kullanıcılar otomatik olarak temel uzak tablolar dış veri kaynağı tanımında belirtilen kimlik bilgileri altında erişim kazanır. Dış veri kaynağının kimlik bilgisi aracılığıyla istenmeyen ayrıcalık önlemek için dış tabloya erişim dikkatle yönetmelisiniz. Normal SQL izinleri VERMEK veya yalnızca dizinindeymiş gibi olağan bir tablo bir dış tablo erişimi iptal etmek için kullanılabilir.  

## <a name="example-querying-vertically-partitioned-databases"></a>Örnek: dikey sorgulama veritabanları bölümlenmiş
Aşağıdaki sorgu müşteriler için siparişleri ve sipariş satırlarını iki yerel tablolara ve uzak tablo arasında üç yönlü birleştirme gerçekleştirir. Bu, esnek bir sorgu için başvuru verileri kullanım örneği örneğidir: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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
BI ve veri tümleştirme araçlarınızı etkin esnek sorgu ve dış tablolara tanımlı olduğu SQL DB sunucusunda veritabanlarına bağlanmak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. SQL Server, aracı için bir veri kaynağı olarak desteklendiğinden emin olun. Esnek sorgu veritabanını ve aracı ile bağlanacağı yalnızca herhangi diğer SQL Server veritabanı gibi dış tabloları başvurun. 

## <a name="best-practices"></a>En iyi uygulamalar
* Esnek sorgu uç veritabanı erişim uzak veritabanına erişim için Azure Services SQL DB güvenlik duvarı yapılandırmasıyla etkinleştirerek verildiğinden emin olun. Ayrıca Dış veri kaynak tanımı'nda sağlanan kimlik bilgileri uzak veritabanına başarıyla oturum açabilir ve uzak tablo erişim izni olduğundan emin olun.  
* Esnek sorgu hesaplama çoğunu uzak veritabanlarına burada yapılabilir sorguları için en iyi şekilde çalışır. Genellikle en iyi sorgu performansını uzak veritabanları veya tamamen Uzak veritabanı üzerinde gerçekleştirilebilir birleştirmeler hesaplanan seçmeli filtre koşulları ile alırsınız. Diğer sorgu desenlerine kötü gerçekleştirebilir ve büyük miktarlarda verinin uzak veritabanından yüklemek gerekebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz: [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey bölümleme öğretici için bkz: [(dikey bölümleme) veritabanları arası sorgusu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Yatay bölümleme (parçalama) bir öğretici için bkz: [yatay (parçalama) bölümleme için esnek sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started.md).
* Yatay olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [yatay sorgulama bölümlenmiş veri)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) tek uzaktan Azure SQL veritabanı ya da yatay bölümleme düzenindeki parça olarak hizmet veren bir veritabanları kümesi üzerinde bir Transact-SQL deyimini yürütür bir saklı yordam için.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
