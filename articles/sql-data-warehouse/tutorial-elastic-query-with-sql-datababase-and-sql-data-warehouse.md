---
title: 'Öğretici: Azure SQL veri ambarı ile esnek sorgu | Microsoft Docs'
description: Bu öğretici, Azure SQL veritabanından sorgu Azure SQL Data Warehouse esnek sorgu özelliğini kullanır.
services: sql-data-warehouse
author: hirokib
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/14/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: a31f035b5ec086a046028956c4a9c0de0d6a313d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-use-elastic-query-to-access-data-in-azure-sql-data-warehouse-from-azure-sql-database"></a>Öğretici: Kullanım esnek sorgudan Azure SQL veri ambarındaki verilere erişmek için Azure SQL veritabanı

Bu öğretici, Azure SQL veritabanından sorgu Azure SQL Data Warehouse esnek sorgu özelliğini kullanır. 

## <a name="prerequisites-for-the-tutorial"></a>Öğreticisi için Önkoşullar

Öğretici başlamadan önce aşağıdaki önkoşullara sahip olmalıdır:

1. Yüklü SQL Server Management Studio'yu (SSMS).
2. Bir Azure SQL server veritabanı ve veri ambarının bu sunucu içinde oluşturulur.
3. Azure SQL Server'a erişmek için güvenlik duvarı kurallarını ayarlayın.

## <a name="set-up-connection-between-sql-data-warehouse-and-sql-database-instances"></a>SQL veri ambarı ve SQL veritabanı örnekleri arasında bağlantı kurma 

1. SSMS veya başka bir sorgu istemcisi kullanarak veritabanı için yeni bir sorgu açın **ana** mantıksal sunucunuzda.

2. Oturum açma ve SQL veritabanına veri ambarı bağlantısı temsil eden kullanıcısı oluşturun.

   ```sql
   CREATE LOGIN SalesDBLogin WITH PASSWORD = 'aReallyStrongPassword!@#';
   ```

3. SSMS veya başka bir sorgu istemcisi kullanarak açmak için yeni bir sorgu **SQL veri ambarı örneği** mantıksal sunucunuzda.

4. Veri ambarı örneği 2. adımda oluşturduğunuz oturum açma bilgileriyle bir kullanıcı oluşturun

   ```sql
   CREATE USER SalesDBUser FOR LOGIN SalesDBLogin;
   ```

5. Kullanıcıya adım 4 ile SQL veritabanı yürütmek istediğiniz gibi yaptığınız izinleri. Bu örnekte, yalnızca seçin nasıl biz sorguları SQL veritabanından belirli bir etki alanına kısıtlayabilir gösteren belirli bir şema üzerinde izni. 

   ```sql
   GRANT SELECT ON SCHEMA :: [dbo] TO SalesDBUser;
   ```

6. SSMS veya başka bir sorgu istemcisi kullanarak açmak için yeni bir sorgu **SQL veritabanı örneği** mantıksal sunucunuzda.

7. Zaten mevcut olmayan bir ana anahtar oluşturun. 

   ```sql
   CREATE MASTER KEY; 
   ```

8. 2. adımda oluşturduğunuz kimlik bilgilerini kullanarak bir veritabanı kapsamlı kimlik bilgisi oluşturun.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL SalesDBElasticCredential
   WITH IDENTITY = 'SalesDBLogin',
   SECRET = 'aReallyStrongPassword@#!';
   ```

9. Veri ambarı örneği için işaret eden bir dış veri kaynağı oluşturun.

   ```sql
   CREATE EXTERNAL DATA SOURCE EnterpriseDwSrc WITH 
       (TYPE = RDBMS, 
       LOCATION = '<SERVER NAME>.database.windows.net', 
       DATABASE_NAME = '<SQL DATA WAREHOUSE NAME>', 
       CREDENTIAL = SalesDBElasticCredential, 
   ) ;
   ```

10. Artık bu dış veri kaynağına başvuran dış tablolara oluşturabilirsiniz. Bu tablolar kullanarak sorguları, işlenen ve veritabanı örneğine gönderilen veri ambarı örneği için gönderilir.


## <a name="elastic-query-from-sql-database-to-sql-data-warehouse"></a>Esnek sorguya SQL veri ambarı SQL veritabanından

Sonraki birkaç adımda bir tablo, veri ambarı örneği birkaç değerlerle oluşturacağız. Ardından veritabanı örneğinde veri ambarı örneğinden sorgulamak için bir dış tablosu oluşturmak göstereceğiz.

1. SSMS veya başka bir sorgu istemcisi kullanarak açmak için yeni bir sorgu **SQL Data Warehouse** mantıksal sunucunuzda.

2. Aşağıdaki sorgu oluşturmak için bir **OrdersInformation** , veri ambarı örneği tablosunda.

   ```sql
   CREATE TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL 
   ,   [CustomerID] [int] NOT NULL 
   ) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)
   ```

3. SSMS veya başka bir sorgu istemcisi kullanarak açmak için yeni bir sorgu **SQL veritabanı** mantıksal sunucunuzda.

4. İşaret eden bir dış tablo tanımı oluşturmak için aşağıdaki sorgu **OrdersInformation** veri ambarı örneği tablosunda.

   ```sql
   CREATE EXTERNAL TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL
   ,   [CustomerID] [int] NOT NULL 
   ) 
   WITH 
   (
        DATA_SOURCE = EnterpriseDwSrc
   ,    SCHEMA_NAME = N'dbo'
   ,    OBJECT_NAME = N'OrderInformation'
   )
   ```

5. Şimdi bir dış tablo tanımındaki olduğunuz inceleyin, **SQL veritabanı örneği**.

   ![Esnek sorgu dış tablo tanımı](media/sql-data-warehouse-elastic-query-with-sql-database/elastic-query-external-table.png)


6. Veri ambarı örneği sorgular aşağıdaki sorguyu gönderin. 2. adımda eklediğiniz beş değerleri almanız gerekir. 

```sql
SELECT * FROM [dbo].[OrderInformation];
```

> [!NOTE]
>
> Birkaç değerleri rağmen dikkat edin, bu sorgu döndürmek için önemli ölçüde zaman alır. Esnek sorgu veri ambarına kullanırken, bir sorgu işleme ve taşıma genel giderlerinden kablo üzerinden göz önünde bulundurmanız gerekir. İşlem gücü, gecikme süresi yok önceliği olduğunda esnek sorgu uzaktan yürütme kullanın.

Tebrikler, esnek sorgu çok temelleri ayarlayın. 

## <a name="next-steps"></a>Sonraki adımlar
Önerileri için bkz: [en iyi yöntemler kullanarak Azure SQL Data Warehouse ile esnek sorgu için](how-to-use-elastic-query-with-sql-data-warehouse.md).