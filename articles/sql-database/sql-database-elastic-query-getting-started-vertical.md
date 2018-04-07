---
title: Veritabanları arası sorgular (dikey bölümleme) ile çalışmaya başlama | Microsoft Docs
description: Esnek veritabanı sorgusu dikey olarak bölümlenmiş veritabanları ile nasıl kullanılır
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: c7bf6816b457f7e193f53336c48f5e205722067e
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Veritabanları arası sorgular (dikey bölümleme) ile çalışmaya başlama (Önizleme)
Esnek veritabanı sorgu (Önizleme) Azure SQL veritabanı için bir tek bağlantı noktası kullanarak birden çok veritabanı span T-SQL sorguları çalıştırmanıza olanak sağlar. Bu konuda uygulandığı [veritabanları'dikey olarak bölümlenmiş](sql-database-elastic-query-vertical-partitioning.md).  

Tamamlandığında, şunları yapacaksınız: yapılandırmak ve birden çok ilişkili veritabanlarını span sorguları gerçekleştirmek için bir Azure SQL veritabanını kullan öğrenin. 

Esnek veritabanı sorgu özelliği hakkında daha fazla bilgi için lütfen bkz [Azure SQL Database esnek veritabanı sorgu genel bakış](sql-database-elastic-query-overview.md). 

## <a name="prerequisites"></a>Önkoşullar

ALTER ANY dış veri KAYNAĞINA iznine sahip olması gerekir. Bu izin ALTER DATABASE izniyle dahil edilir. Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

## <a name="create-the-sample-databases"></a>Örnek veritabanları oluşturma
İle başlamak iki veritabanı oluşturmak ihtiyacımız **müşteriler** ve **siparişleri**, aynı veya farklı bir mantıksal sunucuya ya da.   

Üzerinde aşağıdaki sorguları yürütün **siparişleri** oluşturmak için veritabanında **OrderInformation** tablo ve örnek verileri girdi. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Sorgu şimdi yürütmek **müşteriler** oluşturmak için veritabanında **CustomerInformation** tablo ve örnek verileri girdi. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Veritabanı nesneleri oluşturma
### <a name="database-scoped-master-key-and-credentials"></a>Veritabanı kapsamlı ana anahtar ve kimlik bilgileri
1. SQL Server Management Studio veya SQL Server veri araçları Visual Studio'da açın.
2. Siparişleri veritabanına bağlanmak ve aşağıdaki T-SQL komutları yürütün:
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    "Parola" ve "username" kullanıcı adı ve parola kullanılan olmalıdır müşteriler veritabanına oturum açmak için.
    Esnek sorgularıyla Azure Active Directory'yi kullanarak kimlik doğrulaması şu anda desteklenmiyor.

### <a name="external-data-sources"></a>Dış veri kaynakları
Dış veri kaynağı oluşturmak için siparişler veritabanı üzerinde şu komutu çalıştırın: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Dış tablolar
Bir dış tablo CustomerInformation tablosunun tanımını eşleşen siparişleri veritabanı oluşturun:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Bir örnek esnek veritabanı T-SQL sorgusu yürütme
Dış veri kaynağınızda ve dış tablolarınızı tanımladıktan sonra dış tablolara sorgulamak için T-SQL artık kullanabilirsiniz. Bu sorgu siparişler veritabanında yürütün: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Maliyet
Şu anda, esnek veritabanı sorgu Özelliği Azure SQL veritabanınıza maliyet dahil edilir.  

Fiyatlandırma bilgileri için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database). 

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz: [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [dikey sorgulama bölümlenmiş veri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) bir öğretici için bkz: [yatay (parçalama) bölümleme için esnek sorgu ile çalışmaya başlama](sql-database-elastic-query-getting-started.md).
* Yatay olarak bölümlenmiş verilere ilişkin söz dizimi ve örnek sorgular için bkz: [yatay sorgulama bölümlenmiş veri)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) tek uzaktan Azure SQL veritabanı ya da yatay bölümleme düzenindeki parça olarak hizmet veren bir veritabanları kümesi üzerinde bir Transact-SQL deyimini yürütür bir saklı yordam için.
