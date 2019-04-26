---
title: (Dikey bölümlendirme) veritabanları arası sorguları kullanmaya başlama | Microsoft Docs
description: dikey olarak bölümlenmiş veritabanları ile esnek veritabanı sorgusu kullanma
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 116a465a0ddc913e342e0ffcc1fb29f5bf969419
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60307453"
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>(Dikey bölümlendirme) veritabanları arası sorguları kullanmaya başlama (Önizleme)

Azure SQL veritabanı için elastik veritabanı sorgusu (Önizleme) bir tek bağlantı noktası kullanarak birden çok veritabanını kapsayan T-SQL sorguları çalıştırmanıza olanak tanır. Bu makalede açıklanan [veritabanları'dikey olarak bölümlenmiş](sql-database-elastic-query-vertical-partitioning.md).  

Tamamlandığında, şunları yapacaksınız: yapılandırmak ve birden çok ilgili veritabanlarına yayılan sorguları gerçekleştirmek için bir Azure SQL veritabanı'nı kullanma hakkında bilgi edinin.

Elastik veritabanı sorgusu özelliği hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı elastik veritabanı sorgusu genel bakış](sql-database-elastic-query-overview.md).

## <a name="prerequisites"></a>Önkoşullar

ALTER ANY dış veri kaynağı izni gereklidir. Bu izne ALTER DATABASE izni dahil edilir. Temel alınan veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

## <a name="create-the-sample-databases"></a>Örnek veritabanları oluşturma

İki veritabanı ile başlamak oluşturma **müşteriler** ve **siparişler**, ya da aynı veya farklı bir SQL veritabanı sunucuları.

Üzerinde aşağıdaki sorguları yürütün **siparişler** oluşturmak için veritabanında **OrderInformation** tablo ve örnek verileri girdi.

    CREATE TABLE [dbo].[OrderInformation](
        [OrderID] [int] NOT NULL,
        [CustomerID] [int] NOT NULL
        )
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)

Artık, şirket aşağıdaki sorguyu yürütün **müşteriler** oluşturmak için veritabanında **CustomerInformation** tablo ve örnek verileri girdi.

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

### <a name="database-scoped-master-key-and-credentials"></a>Veritabanı ana anahtarı hem de kimlik kapsamı

1. SQL Server Management Studio veya SQL Server veri araçları, Visual Studio'da açın.
2. Siparişler veritabanına bağlanmak ve aşağıdaki T-SQL komutlarını yürütün:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master_key_password>';
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';  

    Kullanıcı adı ve müşteri veritabanına oturum açmak için kullanılan parola "password" ve "username" olmalıdır.
    Elastik sorgular kullanmaya Azure Active Directory'yi kullanarak kimlik doğrulaması şu anda desteklenmiyor.

### <a name="external-data-sources"></a>Dış veri kaynakları

Bir dış veri kaynağı oluşturmak için sipariş veritabanı üzerinde aşağıdaki komutu yürütün:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
        (TYPE = RDBMS,
        LOCATION = '<server_name>.database.windows.net',
        DATABASE_NAME = 'Customers',
        CREDENTIAL = ElasticDBQueryCred,
    ) ;

### <a name="external-tables"></a>Dış tablolar

Bir dış tablo CustomerInformation tablo tanımı ile eşleşen sipariş veritabanı oluşturun:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
    ( [CustomerID] [int] NOT NULL,
      [CustomerName] [varchar](50) NOT NULL,
      [Company] [varchar](50) NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc)

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Esnek veritabanı örnek T-SQL sorgusu yürütme

Dış veri kaynağı ve dış tablolarınızı tanımladıktan sonra dış tablolarınızı sorgulamak için T-SQL artık kullanabilirsiniz. Bu sorgu siparişler veritabanında yürütün:

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company
    FROM OrderInformation
    INNER JOIN CustomerInformation
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID

## <a name="cost"></a>Maliyet

Şu anda elastik veritabanı sorgusu özelliği, Azure SQL veritabanı maliyetini dahil edilir.  

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database).

## <a name="next-steps"></a>Sonraki adımlar

* Esnek sorgu genel bakış için bkz. [esnek sorgu genel bakış](sql-database-elastic-query-overview.md).
* Dikey olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama dikey olarak bölümlenmiş verileri)](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) bir öğretici için bkz. [yatay bölümleme (parçalama) için esnek sorgu kullanmaya başlama](sql-database-elastic-query-getting-started.md).
* Yatay olarak bölümlenmiş veriler için söz dizimi ve örnek sorgular için bkz. [sorgulama yatay olarak bölümlenmiş veriler)](sql-database-elastic-query-horizontal-partitioning.md)
* Bkz: [sp\_yürütme \_uzak](https://msdn.microsoft.com/library/mt703714) parçalarda bir yatay bölümleme düzeni olarak hizmet veren bir veritabanları kümesi veya bir uzak tek Azure SQL veritabanı Transact-SQL deyimini yürütür bir saklı yordam için.
