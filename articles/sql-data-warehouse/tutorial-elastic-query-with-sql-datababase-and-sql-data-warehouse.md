---
title: 'Öğretici: Azure SQL veri ambarı ile esnek sorgu | Microsoft Docs'
description: Bu öğreticide, Azure SQL veritabanı'ndan Azure SQL veri ambarı sorgusu için esnek sorgu özelliği kullanılır.
services: sql-data-warehouse
author: hirokib
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/14/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: 4dc0be045bceaaac4b71c653d82f7f9db834c3ec
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486076"
---
# <a name="tutorial-use-elastic-query-to-access-data-in-azure-sql-data-warehouse-from-azure-sql-database"></a>Öğretici: Azure SQL veri ambarı'nda verilerine erişmek için Azure SQL veritabanı'ndan esnek sorgu kullanın

Bu öğreticide, Azure SQL veritabanı'ndan Azure SQL veri ambarı sorgusu için esnek sorgu özelliği kullanılır. 

## <a name="prerequisites-for-the-tutorial"></a>Öğreticisi için Önkoşullar

Öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmalıdır:

1. Yüklü SQL Server Management Studio'yu (SSMS).
2. Bu sunucu içinde bir veritabanı ve veri ambarı ile Azure SQL sunucusu oluşturdunuz.
3. Azure SQL sunucusuna erişim için güvenlik duvarı kurallarını ayarlayın.

## <a name="set-up-connection-between-sql-data-warehouse-and-sql-database-instances"></a>SQL veri ambarı ve SQL veritabanı örnekleri arasında bağlantı kurma 

1. SSMS veya başka bir sorgu istemcisini kullanarak, veritabanı için yeni bir sorgu açın **ana** mantıksal sunucunuzdaki.

2. Oturum açma ve SQL veritabanı veri ambarı bağlantıyı temsil eden bir kullanıcı oluşturun.

   ```sql
   CREATE LOGIN SalesDBLogin WITH PASSWORD = 'aReallyStrongPassword!@#';
   ```

3. SSMS veya başka bir sorgu istemcisini kullanarak açmak için yeni bir sorgu **SQL veri ambarı örneği** mantıksal sunucunuzdaki.

4. Veri ambarı örneği 2. adımda oluşturduğunuz oturum açma bilgileriyle bir kullanıcı oluşturun

   ```sql
   CREATE USER SalesDBUser FOR LOGIN SalesDBLogin;
   ```

5. Kullanıcı için SQL veritabanı yürütmek istediğiniz gibi yaptığınız adım 4 izinler verir. Bu örnekte, yalnızca seçin nasıl biz sorguları SQL veritabanı'ndan belirli bir etki alanına kısıtlayabilir gösteren, belirli bir şemada izni. 

   ```sql
   GRANT SELECT ON SCHEMA :: [dbo] TO SalesDBUser;
   ```

6. SSMS veya başka bir sorgu istemcisini kullanarak açmak için yeni bir sorgu **SQL veritabanı örneği** mantıksal sunucunuzdaki.

7. Zaten bir tamamlamadıysanız, bir ana anahtar oluşturun. 

   ```sql
   CREATE MASTER KEY; 
   ```

8. 2. adımda oluşturduğunuz kimlik bilgilerini kullanarak bir veritabanı kapsamlı kimlik bilgileri oluşturun.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL SalesDBElasticCredential
   WITH IDENTITY = 'SalesDBLogin',
   SECRET = 'aReallyStrongPassword@#!';
   ```

9. Veri ambarı örneğine işaret eden bir dış veri kaynağı oluşturun.

   ```sql
   CREATE EXTERNAL DATA SOURCE EnterpriseDwSrc WITH 
       (TYPE = RDBMS, 
       LOCATION = '<SERVER NAME>.database.windows.net', 
       DATABASE_NAME = '<SQL DATA WAREHOUSE NAME>', 
       CREDENTIAL = SalesDBElasticCredential, 
   ) ;
   ```

10. Artık bu dış veri kaynağı başvurusu dış tablolar oluşturabilirsiniz. Bu tablolar kullanarak sorgular, işlenmesi ve veritabanı örneğine gönderilen için veri ambarı örneği için gönderilir.


## <a name="elastic-query-from-sql-database-to-sql-data-warehouse"></a>SQL veri ambarı SQL veritabanı esnek sorgu

Sonraki birkaç adımda bir tablo, veri ambarı Örneğinizde birkaç değerlerle oluşturacağız. Biz, ardından veri ambarı örneği veritabanı örneğinden sorgulamak için bir dış tablosu oluşturmak nasıl sürdürebileceğiniz gösterilecek.

1. SSMS veya başka bir sorgu istemcisini kullanarak açmak için yeni bir sorgu **SQL veri ambarı** mantıksal sunucunuzdaki.

2. Oluşturmak için aşağıdaki sorguyu Gönder bir **OrdersInformation** veri ambarı Örneğinizde bir tablo.

   ```sql
   CREATE TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL, 
       [CustomerID] [int] NOT NULL 
   ) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)
   ```

3. SSMS veya başka bir sorgu istemcisini kullanarak açmak için yeni bir sorgu **SQL veritabanı** mantıksal sunucunuzdaki.

4. İşaret eden bir dış tablo tanımındaki oluşturmak için aşağıdaki sorguyu Gönder **OrderInformation** tabloda veri ambarı örneği.

   ```sql
   CREATE EXTERNAL TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL,
       [CustomerID] [int] NOT NULL 
   ) 
   WITH 
   (
        DATA_SOURCE = EnterpriseDwSrc,
    SCHEMA_NAME = N'dbo',
    OBJECT_NAME = N'OrderInformation'
   )
   ```

5. Artık bir dış tablo tanımındaki mümkün olduğunu, **SQL veritabanı örneği**.

   ![Dış tablo tanımındaki esnek sorgu](media/sql-data-warehouse-elastic-query-with-sql-database/elastic-query-external-table.png)


6. Veri ambarı örneği sorgular aşağıdaki sorgu gönderin. 2. adımda eklediğiniz beş değerleri almanız gerekir. 

```sql
SELECT * FROM [dbo].[OrderInformation];
```

> [!NOTE]
>
> Az sayıda değer rağmen bu sorgu döndürmek için büyük miktarda zaman sağladığına dikkat edin. Veri ambarı ile esnek sorgu kullanarak, bir sorgu işleme taşıma ve yüksek maliyetleri kablo üzerinden düşünmelisiniz. Esnek sorgu uzaktan yürütme gecikme değil, işlem gücünü önceliği olduğunda kullanın.

Tebrikler, ayarladığınız çok esnek sorgu temelleri. 

## <a name="next-steps"></a>Sonraki adımlar
Öneriler için bkz. [elastik sorgu kullanarak Azure SQL veri ambarı için en iyi yöntemler](how-to-use-elastic-query-with-sql-data-warehouse.md).
