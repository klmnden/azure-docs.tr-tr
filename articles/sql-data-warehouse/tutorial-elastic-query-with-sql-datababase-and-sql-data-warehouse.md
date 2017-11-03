---
title: "Azure SQL veri ambarı ile esnek sorgu Öğreticisi | Microsoft Docs"
description: "Azure SQL Data Warehouse ile esnek sorgu kullanmayı öğrenin "
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 09/18/2017
ms.author: elbutter
ms.openlocfilehash: 8698dace1b7308fc60178d97e134cb708ff02255
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-elastic-query-with-sql-data-warehouse"></a>Esnek sorgusu ile SQL veri ambarı yapılandırma

Bu öğreticide, esnek sorgu SQL veri ambarı SQL veritabanından sorguya göndermek için nasıl kullanılacağını öğreneceksiniz. Esnek sorgu Azure SQL ürünleri arasında bulunan bir işlevdir. Kavram olarak esnek sorgu hakkında daha fazla bilgi için bkz: [ **esnek sorgusu ile SQL veri ambarı kullanmak nasıl**][How to use Elastic Query with SQL Data Warehouse].

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
   (DATA_SOURCE = EnterpriseDwSrc)
   ```

5. Şimdi bir dış tablo tanımındaki olduğunuz inceleyin, **SQL veritabanı örneği**.

   ![Esnek sorgu dış tablo tanımı](./media/sql-data-warehouse-elastic-query-with-sql-database/elastic-query-external-table.png)


6. Veri ambarı örneği sorgular aşağıdaki sorguyu gönderin. 2. adımda eklediğiniz beş değerleri almanız gerekir. 

```sql
SELECT * FROM [dbo].[OrderInformation];
```

> [!NOTE]
>
> Birkaç değerleri rağmen dikkat edin, bu sorgu döndürmek için önemli ölçüde zaman alır. Esnek sorgu veri ambarına kullanırken, bir sorgu işleme ve taşıma genel giderlerinden kablo üzerinden göz önünde bulundurmanız gerekir. İşlem gücü, gecikme süresi yok önceliği olduğunda esnek sorgu uzaktan yürütme kullanın.

Tebrikler, esnek sorgu çok temelleri ayarlayın. 




<!--Image references-->

<!--Article references-->

[How to use Elastic Query with SQL Data Warehouse]: ./how-to-use-elastic-query-with-sql-data-warehouse.md

<!--MSDN references-->

<!--Other Web references-->