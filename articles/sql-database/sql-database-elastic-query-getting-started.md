---
title: Ölçeği genişletilen bulut veritabanlarında (yatay bölümleme) rapor | Microsoft Docs
description: Çapraz veritabanı veritabanı sorguları için rapor birden fazla veritabanında kullanın.
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
ms.date: 12/18/2018
ms.openlocfilehash: a73938c98ebaea310875f0db8b665d0f1aed55e8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60556271"
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>(Önizleme) ölçeği genişletilen bulut veritabanlarında raporlama

Bir tek bağlantı noktası kullanarak birden çok Azure SQL veritabanından raporlar oluşturabilirsiniz bir [esnek sorgu](sql-database-elastic-query-overview.md). (Ayrıca "olarak parçalı" bilinen) yatay olarak bu veritabanlarını bölümlenmesi gerekir.

Mevcut bir veritabanı varsa, bkz: [ölçeği genişletilmiş veritabanları için mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md).

Sorgu için gerekli olan SQL nesneleri anlamak için bkz: [sorgu yatay olarak bölünmüş veritabanlarında](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="prerequisites"></a>Önkoşullar

İndirme ve çalıştırma [esnek veritabanı araçları örnek ile kullanmaya](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Parça eşleme Yöneticisi örnek uygulaması kullanarak oluşturma
Burada bir parça eşleme Yöneticisi tarafından veri ekleme parçalara ardından birkaç parçalar ile birlikte oluşturur. Parçalı verileri parçalar Kurulumu zaten içeren seçerseniz, aşağıdaki adımları atlayın ve sonraki bölüme Taşı.

1. Derleme ve çalıştırma **esnek veritabanı araçları ile çalışmaya başlama** örnek uygulama. Bölümündeki 7. adım kadar adımları [örnek uygulamasını indirme ve çalıştırma](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Adım 7 sonunda, aşağıdaki komut istemi görürsünüz:

    ![Komut İstemi][1]
2. Komut penceresinde "1" yazın ve basın **Enter**. Parça eşleme Yöneticisi oluşturur ve iki parça sunucusuna ekler. Ardından, "3" yazın ve basın **Enter**; eylemi dört kez tekrarlayın. Bu örnek veri satırları, parçalarda ekler.
3. [Azure portalında](https://portal.azure.com) üç yeni veritabanı sunucunuzun göstermelidir:

   ![Visual Studio onayı][2]

   Bu noktada, platformlar arası sorguları, elastik veritabanı istemci kitaplıkları aracılığıyla desteklenir. Örneğin, komut penceresinde 4 seçeneğini kullanın. Çok parçalı sorgusundan gelen sonuçları her zaman olan bir **UNION ALL** sonuçlarına ilişkin tüm parçalar.

   Sonraki bölümde, verilerin parçalar arasında daha zengin sorgulama destekleyen bir örnek veritabanı uç nokta oluşturacağız.

## <a name="create-an-elastic-query-database"></a>Esnek sorgu veritabanı oluşturma
1. Açık [Azure portalında](https://portal.azure.com) ve oturum açın.
2. Parça kurulumunuzu ile aynı sunucuda yeni bir Azure SQL veritabanı oluşturun. Adı ' % s'veritabanı "ElasticDBQuery."

    ![Azure portalı ve fiyatlandırma katmanı][3]

    > [!NOTE]
    > Varolan bir veritabanını kullanabilirsiniz. Bunu, sorgularınızı yürütmek istediğiniz parçalar birini olmamalıdır. Bu veritabanı, esnek veritabanı sorgusu için meta veri nesneleri oluşturmak için kullanılır.
    >

## <a name="create-database-objects"></a>Veritabanı nesneleri oluşturma
### <a name="database-scoped-master-key-and-credentials"></a>Veritabanı kapsamlı ana anahtarı hem de kimlik bilgileri
Bunlar, parça eşleme Yöneticisi ve parçalar bağlanmak için kullanılır:

1. SQL Server Management Studio veya SQL Server veri araçları, Visual Studio'da açın.
2. ElasticDBQuery veritabanına bağlanmak ve aşağıdaki T-SQL komutlarını çalıştırın:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "username" ve "password" olmalıdır 6. adımında kullanılan oturum açma bilgileri aynı [örnek uygulamasını indirme ve çalıştırma](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) içinde [esnek veritabanı araçları ile çalışmaya başlama](sql-database-elastic-scale-get-started.md).

### <a name="external-data-sources"></a>Dış veri kaynakları
Bir dış veri kaynağı oluşturmak için ElasticDBQuery veritabanı üzerinde aşağıdaki komutu yürütün:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 Parça Haritası ve parça eşleme Yöneticisi esnek veritabanı araçları örneğini kullanarak oluşturduysanız, "CustomerIDShardMap" parça eşlemesi adıdır. Ancak, bu örnek için özel kurulumunuzu kullandıysanız, uygulamanızda seçtiğiniz parça eşleme adı olması gerekir.

### <a name="external-tables"></a>Dış tablolar
ElasticDBQuery veritabanında aşağıdaki komutu yürüterek parçaları Müşteriler tablosunda eşleşen bir dış tablo oluşturun:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Esnek veritabanı örnek T-SQL sorgusu yürütme
Dış veri kaynağı ve dış tablolarınızı tanımladıktan sonra tam T-SQL artık dış tablolarınızı kullanabilirsiniz.

Bu sorgu ElasticDBQuery veritabanında yürütün:

    select count(CustomerId) from [dbo].[Customers]

Sorgu sonuçları tüm parçadan toplayan ve şu çıktıyı verir olduğunu göreceksiniz:

![Çıkış Ayrıntıları][4]

## <a name="import-elastic-database-query-results-to-excel"></a>Excel için elastik veritabanı sorgusu sonuçlarını Al
 Gelen bir sorgunun sonuçlarının bir Excel dosyasına aktarabilirsiniz.

1. Excel 2013'ün başlatın.
2. Gidin **veri** Şerit.
3. Tıklayın **diğer kaynaklardan** tıklatıp **SQL Server'dan**.

   ![Excel Import diğer kaynaklardan][5]
4. İçinde **Veri Bağlantı Sihirbazı'nı** sunucu adını ve oturum açma kimlik bilgilerini yazın. Ardından **İleri**'ye tıklayın.
5. İletişim kutusunda **istediğiniz verileri içeren veritabanını seçin**seçin **ElasticDBQuery** veritabanı.
6. Seçin **müşteriler** tıklayın ve liste görünümünde tablo **sonraki**. Ardından **son**.
7. İçinde **verileri içeri aktarma** formunda, altında **bu verileri çalışma kitabınızı görüntüleme istediğiniz şekli seçin**seçin **tablo** tıklatıp **Tamam**.

Tüm satırların **müşteriler** tabloda, farklı parçalarda depolanan Excel sayfası doldurun.

Artık, Excel'in güçlü veri görselleştirme işlevleri kullanabilirsiniz. BI ve veri tümleştirme araçlarınızı esnek sorgu veritabanına bağlanmak için sunucu adını, veritabanı adı ve kimlik bilgileri ile bağlantı dizesini kullanabilirsiniz. SQL Server'ın aracınız için bir veri kaynağı olarak desteklendiğinden emin olun. Herhangi bir SQL Server veritabanı gibi dış tablolar ve aracınızla bağlanacağı SQL Server tabloları ve esnek sorgu veritabanı başvurabilirsiniz.

### <a name="cost"></a>Maliyet
Elastik veritabanı sorgusu özelliğini kullanmak için ek ücret yoktur.

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz. [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey bölümleme öğreticisi için bkz. [(dikey bölümlendirme) veritabanları arası sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Dikey olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama dikey olarak bölümlenmiş verileri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama yatay olarak bölümlenmiş veriler)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) parçalarda bir yatay bölümleme düzeni olarak hizmet veren bir veritabanları kümesi veya bir uzak tek Azure SQL veritabanı Transact-SQL deyimini yürütür bir saklı yordam için.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
