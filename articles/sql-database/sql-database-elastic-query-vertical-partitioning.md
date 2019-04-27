---
title: Farklı şemalı bulut veritabanlarında Sorgu | Microsoft Docs
description: Çapraz veritabanı sorguları dikey bölümler üzerinde ayarlama
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sstein
manager: digimobile
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: e7ba8057cd22c5cc1080b4a6d95f17bf76d4acb2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60585424"
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Farklı şemalarla (Önizleme) bulut veritabanlarında sorgulama yapma

![Farklı veritabanlarındaki tabloları sorgulama][1]

Dikey olarak bölümlenmiş veritabanları farklı veritabanları üzerinde tablolar farklı kümesini kullanın. Şema farklı veritabanları üzerinde farklı olduğu anlamına gelir. Hesap oluşturma ile ilgili tüm tabloları üzerinde ikinci bir veritabanı örneği için envanteri için tüm tabloları bir veritabanında bağlıdır. 

## <a name="prerequisites"></a>Önkoşullar

* Kullanıcı, ALTER ANY dış veri kaynağı iznine sahip olması gerekir. Bu izne ALTER DATABASE izni dahil edilir.
* Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Farklı yatay bölümleme Bu deyimler bir parça eşlemesi elastik veritabanı istemci kitaplığı aracılığıyla veri katmanıyla tanımlama üzerinde bağımlı değildir.
>

1. [ANA ANAHTAR OLUŞTURUN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [OLUŞTURMA VERİTABANI KAPSAMLI KİMLİK BİLGİLERİ](https://msdn.microsoft.com/library/mt270260.aspx)
3. [DIŞ VERİ KAYNAĞI OLUŞTURMA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [DIŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Kapsamlı bir veritabanı ana anahtarı ve kimlik bilgileri oluşturma

Kimlik bilgisi tarafından esnek sorgu, uzak veritabanlarına bağlanmak için kullanılır.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'master_key_password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Emin `<username>` içermez **"\@servername"** soneki. 
>

## <a name="create-external-data-sources"></a>Dış veri kaynakları oluşturma

Sözdizimi:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = '<fully_qualified_server_name>',
                DATABASE_NAME = '<remote_database_name>',  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> TÜR parametresi ayarlanmalıdır **RDBMS**. 
>

### <a name="example"></a>Örnek

Aşağıdaki örnek, dış veri kaynakları için oluşturma deyimi kullanımını gösterir. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.chinacloudapi.cn', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

Geçerli bir dış veri kaynakları listesini almak için: 

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

```sql
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
```

Aşağıdaki örnek, geçerli veritabanından dış tabloların listesini almak gösterilmektedir: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Açıklamalar

Esnek sorgu RDBMS türündeki dış veri kaynakları kullanan dış tablolar tanımlamak için var olan dış tablo sözdizimi genişletir. Dikey bölümleme için bir dış tablo tanımındaki aşağıdaki konuları içerir: 

* **Şema**: Dış tablo DDL sorgularınızı kullanabileceğiniz bir şema tanımlar. Dış tablo Tanımınızda belirtilen şema, gerçek verilerin depolandığı Uzak veritabanı tablo şema ile eşleşmesi gerekiyor. 
* **Uzak veritabanı başvurusu**: Dış tablo DDL bir dış veri kaynağına başvuruyor. Dış veri kaynağı veritabanı adı gerçek tablo verilerinin nerede depolanacağını Uzak veritabanı ve SQL veritabanı sunucu adı belirtir. 

Önceki bölümde açıklandığı gibi bir dış veri kaynağı kullanarak dış tablolar oluşturmak için sözdizimi aşağıdaki gibidir: 

DATA_SOURCE yan tümcesi için dış tablo kullanılan dış veri kaynağı (dikey bölümleme durumunda yani Uzak veritabanı) tanımlar.  

SCHEMA_NAME ve OBJECT_NAME yan tümceleri bir tablodaki bir uzak veritabanı farklı bir şema veya farklı bir ada sahip bir tablo için dış tablo tanımındaki sırasıyla eşleme özelliğini sağlar. Bu, Uzak veritabanı - veya burada uzak tablo adı zaten yerel olarak alınmış durumda bir dış tablo Katalog görünümü veya DMV tanımlamak istiyorsanız kullanışlıdır.  

Aşağıdaki DDL deyimi var olan bir dış tablo tanımındaki yerel katalogdan bırakır. Uzak veritabanı etkilemez. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Dış tablo oluşturma bırakma izinlerini**: Dış tablo ayrıca temel alınan veri kaynağına başvurmak için gerekli olan DDL için ALTER ANY dış veri kaynağı izinleri gereklidir.  

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Dış tablo erişimi olan kullanıcılar temel alınan uzak tablolar dış veri kaynağı tanımına verilen kimlik bilgisi altında otomatik olarak erişin. Dış veri kaynağının kimlik bilgisi üzerinden istenmeyen ayrıcalık önlemek için dış tablo erişim dikkatli bir şekilde yönetmeniz gerekir. Normal SQL izinleri VERMEK veya yalnızca işlevmiş gibi olağan bir tablo bir dış tablo erişimi iptal etmek için kullanılabilir.  

## <a name="example-querying-vertically-partitioned-databases"></a>Örnek: sorgulama dikey olarak bölümlenmiş veritabanları

Aşağıdaki sorgu, müşteriler için siparişleri ve satış siparişi için iki yerel tabloları ve uzak tablo arasında üç yönlü birleştirme gerçekleştirir. Bu, başvuru veri kullanım örneği için esnek sorgu örneğidir: 

```sql
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
```

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Saklı yordamı uzaktan T-SQL yürütme için: sp\_execute_remote

Esnek sorgu, uzak veritabanına doğrudan erişim sağlayan bir saklı yordam da tanıtılmaktadır. Saklı yordamı çağrılır [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) ve Uzak veritabanı üzerinde uzak saklı yordamları ya da T-SQL kodunu çalıştırmak için kullanılabilir. Bunu, aşağıdaki parametreleri alır: 

* Veri kaynağı adı (nvarchar): RDBMS türündeki dış veri kaynağının adı. 
* Sorgu (nvarchar): Uzak veritabanı üzerinde yürütülecek T-SQL sorgusu. 
* Parametre bildirimi (nvarchar) - isteğe bağlı: Dize (gibi sp_executesql) sorgu parametresi olarak kullanılan parametreler için tür tanımları ile verileri. 
* Parametre değeri listesi - isteğe bağlı: Virgülle ayrılmış listesi (gibi sp_executesql) parametre değerleri.

Sp\_yürütme\_uzaktan Uzak veritabanı üzerinde belirli T-SQL deyimi yürütmek için çağırma parametreleri sağlanan dış veri kaynağı kullanır. Dış veri kaynağının kimlik bilgisi uzak veritabanına bağlanmak için kullanır.  

Örnek: 

```sql
    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 
```

## <a name="connectivity-for-tools"></a>Bağlantı için Araçlar

Elastik sorgu etkin ve tanımladığınız dış tablolar içeren SQL DB sunucudaki veritabanlarını, BI ve veri tümleştirme araçları bağlanmak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. SQL Server'ın aracınız için bir veri kaynağı olarak desteklendiğinden emin olun. Ardından Esnek sorgu veritabanı ve aracınızla bağlanacağı yalnızca herhangi diğer SQL Server veritabanı gibi dış tablolara bakın. 

## <a name="best-practices"></a>En iyi uygulamalar

* Esnek sorgu bitiş noktası veritabanı erişim uzak veritabanına SQL DB güvenlik duvarı yapılandırması içinde Azure Hizmetleri için erişim sağlayarak verildiğinden emin olun. Ayrıca Dış veri kaynağı tanımında sağlanan kimlik bilgileri uzak veritabanına başarıyla oturum açabilir ve uzak tabloya erişim izni olduğundan emin olun.  
* Esnek sorgu uzak veritabanlarında hesaplama çoğunu burada yapılabilir sorgular için en iyi şekilde çalışır. Normalde en iyi sorgu performansını uzak veritabanları veya tamamen Uzak veritabanı üzerinde gerçekleştirilebilir birleştirmeler değerlendirilen seçmeli filtre koşullarla alabilirsiniz. Diğer sorgu desenleri sonlanmayacağından ve büyük miktarlarda verinin uzak veritabanından yüklemek gerekebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz. [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey bölümleme öğreticisi için bkz. [(dikey bölümlendirme) veritabanları arası sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Yatay bölümleme (parçalama) bir öğretici için bkz. [yatay bölümleme (parçalama) için esnek sorgu kullanmaya başlama](sql-database-elastic-query-getting-started.md).
* Yatay olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama yatay olarak bölümlenmiş veriler)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) parçalarda bir yatay bölümleme düzeni olarak hizmet veren bir veritabanları kümesi veya bir uzak tek Azure SQL veritabanı Transact-SQL deyimini yürütür bir saklı yordam için.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->