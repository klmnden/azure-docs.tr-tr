---
title: "Rapor (yatay bölümleme) ölçeklendirilmiş bulut veritabanları arasında | Microsoft Docs"
description: "Veritabanları arası veritabanı sorguları rapor için birden fazla veritabanı kullanın."
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: 996ad1d47ece592dcf03a6eb8ed1c1916ceba374
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>Ölçeklendirilen bulut veritabanları arasında (Önizleme) raporu
Tek bir bağlantı noktası kullanarak birden çok Azure SQL veritabanından raporlar oluşturmak bir [esnek sorgu](sql-database-elastic-query-overview.md). Veritabanlarını yatay (aynı zamanda "parçalı" olarak da bilinir) bölümlenmiş olması gerekir.

Var olan bir veritabanı varsa, bkz: [ölçeklendirilmiş veritabanları için var olan veritabanlarını taşıma](sql-database-elastic-convert-to-use-elastic-tools.md).

Sorgu için gerekli olan SQL nesneler anlamak için bkz: [sorgu yatay olarak bölümlenmiş veritabanları arasında](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="prerequisites"></a>Ön koşullar
İndirme ve çalıştırma [esnek veritabanı araçlarını örneği ile çalışmaya başlama](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Harita manager örnek uygulamasını kullanarak bir parça oluşturma
Burada birkaç parça parça veri ekleme tarafından izlenen, birlikte Yöneticisi bir parça eşleme oluşturur. Parçalı veriler parça Kurulumu zaten olması görülüyorsa, aşağıdaki adımları atlayın ve sonraki bölüme taşıyın.

1. Derleme ve çalıştırma **esnek veritabanı araçlarını kullanmaya başlama** örnek uygulama. Adım 7 bölümünde kadar adımları [örnek uygulamasını indirme ve çalıştırma](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Adım 7 sonunda, aşağıdaki komut istemi görürsünüz:

    ![Komut İstemi][1]
2. Komut penceresinde "1" yazın ve tuşuna basın **Enter**. Bu parça eşleme Yöneticisi oluşturur ve iki parça sunucusuna ekler. Daha sonra "3" yazın ve basın **Enter**; eylem dört kez yineler. Bu örnek verileri satır, parça ekler.
3. [Azure portal](https://portal.azure.com) üç yeni veritabanları sunucunuzun göstermesi gerekir:

   ![Visual Studio onayı][2]

   Bu noktada, veritabanları arası sorguları esnek veritabanı istemci kitaplıkları desteklenir. Örneğin, komut penceresinde 4 seçeneğini kullanın. Çok parça sorgusundan gelen sonuçları her zaman olan bir **UNION ALL** tüm parça sonuçları.

   Sonraki bölümde, verilerin parça arasında daha zengin sorgulama destekleyen bir örnek veritabanı uç noktası oluşturun.

## <a name="create-an-elastic-query-database"></a>Esnek sorgu veritabanı oluşturma
1. Açık [Azure portal](https://portal.azure.com) ve oturum açın.
2. Parça kurulumunuzu aynı sunucuda yeni bir Azure SQL veritabanı oluşturun. "ElasticDBQuery." veritabanı adı

    ![Azure portalı ve fiyatlandırma katmanı][3]

    > [!NOTE]
    > Varolan bir veritabanını kullanabilirsiniz. Bunu yapmak, sorgularınızı yürütmek istediğiniz parça birini olmamalıdır. Bu veritabanı, esnek veritabanı sorgusu için meta veri nesnesi oluşturmak için kullanılır.
    >

## <a name="create-database-objects"></a>Veritabanı nesneleri oluşturma
### <a name="database-scoped-master-key-and-credentials"></a>Veritabanı kapsamlı ana anahtar ve kimlik bilgileri
Bunlar, parça parça eşleme Yöneticisi'ni bağlamak için kullanılır:

1. SQL Server Management Studio veya SQL Server veri araçları Visual Studio'da açın.
2. ElasticDBQuery veritabanına bağlanmak ve aşağıdaki T-SQL komutları yürütün:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "username" ve "parola" olmalıdır yordamının 6. adımında kullanılan oturum açma bilgileri aynı [örnek uygulamasını indirme ve çalıştırma](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) içinde [esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md).

### <a name="external-data-sources"></a>Dış veri kaynakları
Dış veri kaynağı oluşturmak için ElasticDBQuery veritabanında aşağıdaki komutu yürütün:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 Esnek veritabanı araçlarını örnek kullanarak parça eşleme Yöneticisi ve parça eşleme oluşturduysanız "CustomerIDShardMap" parça eşleme adıdır. Ancak, bu örnek için özel kurulum kullandıysanız, uygulamanızda seçtiğiniz parça eşleme adı olması gerekir.

### <a name="external-tables"></a>Dış tablolar
ElasticDBQuery veritabanı üzerinde şu komutu yürüterek parça Müşteriler tablosunda eşleşen bir dış tablo oluşturun:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Bir örnek esnek veritabanı T-SQL sorgusu yürütme
Dış veri kaynağınızda ve dış tablolarınızı tanımladıktan sonra dış tablolar üzerindeki tam T-SQL artık kullanabilirsiniz.

Bu sorgu ElasticDBQuery veritabanında yürütün:

    select count(CustomerId) from [dbo].[Customers]

Sorgu sonuçları tüm parça toplar ve aşağıdaki çıkış verir görürsünüz:

![Çıkış Ayrıntıları][4]

## <a name="import-elastic-database-query-results-to-excel"></a>Esnek veritabanı sorgu sonuçları Excel'e Al
 Gelen bir sorgunun sonuçlarını bir Excel dosyasını içeri aktarabilirsiniz.

1. Excel 2013'ü başlatın.
2. Gidin **veri** Şerit.
3. Tıklatın **diğer kaynaklardan** tıklatıp **SQL Server'dan**.

   ![Excel Import diğer kaynaklardan][5]
4. İçinde **Veri Bağlantı Sihirbazı** sunucu adını ve oturum açma kimlik bilgilerini yazın. Ardından **İleri**'ye tıklayın.
5. İletişim kutusunda **istediğiniz verileri içeren bir veritabanı seçin**seçin **ElasticDBQuery** veritabanı.
6. Seçin **müşteriler** Tablo liste görünümünde ve tıklayın **sonraki**. Ardından **son**.
7. İçinde **veri içeri aktarma** formunda, altında **nasıl çalışma kitabınızı bu verileri görüntülemek istediğinizi seçin**seçin **tablo** tıklatıp **Tamam**.

Tüm satırların **müşteriler** tablo, farklı parça içinde depolanan doldurmak Excel sayfası.

Şimdi, Excel'in güçlü veri görselleştirme işlevlerini kullanabilirsiniz. BI ve veri tümleştirme araçlarınızı esnek sorgu veritabanına bağlanmak için sunucu adını, veritabanı adının ve kimlik bilgileri ile bağlantı dizesi kullanabilirsiniz. SQL Server, aracı için bir veri kaynağı olarak desteklendiğinden emin olun. Herhangi bir SQL Server veritabanı gibi dış tablolar ve aracı ile bağlanacağı SQL Server tablolarını ve esnek sorgu veritabanı başvurabilirsiniz.

### <a name="cost"></a>Maliyet
Esnek veritabanı sorgusu özelliğini kullanmak için ek ücret yoktur.

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz: [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey bölümleme öğretici için bkz: [(dikey bölümleme) veritabanları arası sorgusu ile çalışmaya başlama](sql-database-elastic-query-getting-started-vertical.md).
* Dikey olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [dikey sorgulama bölümlenmiş veri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [yatay sorgulama bölümlenmiş veri)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) tek uzaktan Azure SQL veritabanı ya da yatay bölümleme düzenindeki parça olarak hizmet veren bir veritabanları kümesi üzerinde bir Transact-SQL deyimini yürütür bir saklı yordam için.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
