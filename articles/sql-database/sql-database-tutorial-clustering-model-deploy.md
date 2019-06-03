---
title: 'Öğretici: R ile kümeleme model dağıtma'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bölümünde bu üç bölümlü öğretici serisinin üç r ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile bir kümeleme modeli dağıtacaksınız.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 05/17/2019
ms.openlocfilehash: 1fe9df6378884ba55cb1017da87522ae66edaff0
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419758"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile kümeleme model dağıtma

Bölümünde bu üç bölümlü öğretici serisinin üç r ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile bir kümeleme modeli dağıtacaksınız.

Kümeleme gerçekleştiren bir katıştırılmış R betiği ile bir saklı yordam oluşturacaksınız. Modelinizin, Azure SQL veritabanı'nda yürütüldüğünden, veritabanında depolanan verilere kolayca düşünürler.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Modeli oluşturan bir saklı yordam oluşturma
> * SQL veritabanı'nda kümeleme gerçekleştirin
> * Kümeleme bilgileri kullanın

İçinde [Birinci bölümde](sql-database-tutorial-clustering-model-prepare-data.md), r'de kümelemeyi gerçekleştirmek için bir Azure SQL veritabanından verileri hazırlama işleminin nasıl yapılacağını öğrendiniz

İçinde [bölüm iki](sql-database-tutorial-clustering-model-build.md), kümeleme gerçekleştirmek için bir K-ortalamaları modeli oluşturma işleminin nasıl yapılacağını öğrendiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Bu öğretici serisinin üç bölümünü varsayar tamamladığınızdan [ **Birinci bölümde** ](sql-database-tutorial-clustering-model-prepare-data.md) ve [ **bölüm iki**](sql-database-tutorial-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Modeli oluşturan bir saklı yordam oluşturma

Saklı yordam oluşturmak için aşağıdaki T-SQL betiğini çalıştırın. Yordamı bölümlerinde Bu öğretici serisinin birinci ve ikinci geliştirilen adımları yeniden oluşturur:

* kendi satın alan müşteriler sınıflandırmak ve geçmiş döndürür
* K-ortalamaları algoritması kullanan müşteriler, dört kümesi oluştur

Yordamı, veritabanı tablosu, elde edilen müşteri kümesi eşlemeleri depolar **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
            WHEN (returns_count IS NULL)
                THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string
connStr <- paste("Driver=SQL Server; Server=", instance_name,
               "; Database=", database_name,
               "; Trusted_Connection=true; ",
                  sep="" );

# Input customer data that needs to be classified.
# This is the result we get from the query.
customer_returns <- RxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);
# Output table to hold the customer cluster mappings
return_cluster = RxSqlServerData(table = "customer_return_clusters",
                                 connectionString = connStr);

# Set seed for random number generator for predictability
set.seed(10);

# Generate clusters using rxKmeans and output clusters to a table
# called "customer_return_clusters"
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters = 4,
                   outFile = return_cluster,
                   outColName = "cluster",
                   writeModelVars = TRUE ,
                   extraVarsToWrite = c("customer"),
                   overwrite = TRUE);
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering-in-sql-database"></a>SQL veritabanı'nda kümeleme gerçekleştirin

Saklı yordam oluşturduğunuza göre kümeleme gerçekleştirmek için aşağıdaki betiği çalıştırın.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Biz aslında müşteriler ve küme eşlemelerini listesine sahip ve çalışır durumda olduğunu doğrulayın.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Kümeleme bilgileri kullanın

Kümeleme yordamı veritabanında depolandığından, aynı veritabanında depolanan müşteri verilerine yönelik etkili bir şekilde kümeleme gerçekleştirebilirsiniz. Müşteri verilerinizin güncelleştirilir ve güncelleştirilmiş kümeleme bilgileri her yordamı yürütebilir.

Küme 3, daha etkin dönüş davranışı Grup müşterilerle promosyon e-posta göndermek istediğiniz varsayalım (dört kümeleri nasıl açıklanan gördüğünüz [bölüm iki](sql-database-tutorial-clustering-model-build.md#analyze-the-results)). Aşağıdaki kod, müşteriler e-posta adreslerini 3 kümede seçer.

```sql
USE [tpcxbb_1gb]

SELECT customer.[c_email_address],
    customer.c_customer_sk
FROM dbo.customer
JOIN [dbo].[customer_return_clusters] AS r ON r.customer = customer.c_customer_sk
WHERE r.cluster = 3
```

Değiştirebileceğiniz **r.cluster** diğer kümeler bulunan müşteriler için e-posta adreslerini döndürülecek değer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle tamamladığınızda, Azure SQL veritabanı sunucunuzdan tpcxbb_1gb veritabanı silebilirsiniz.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalında sol taraftaki menüden seçin **tüm kaynakları** veya **SQL veritabanları**.
1. İçinde **ada göre Filtrele...**  alanına **tpcxbb_1gb**ve aboneliğinizi seçin.
1. Seçin, **tpcxbb_1gb** veritabanı.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde Bu öğretici serisinin üç adımları tamamladınız:

* Modeli oluşturan bir saklı yordam oluşturma
* SQL veritabanı'nda kümeleme gerçekleştirin
* Kümeleme bilgileri kullanın

R kullanarak Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) hakkında daha fazla bilgi için bkz:

* [Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir modeli eğitmek için verileri hazırlama](sql-database-tutorial-predictive-model-prepare-data.md)
* [Machine Learning Hizmetleri (Önizleme) kullanarak Azure SQL veritabanı içinde Gelişmiş R işlevler yazma](sql-database-machine-learning-services-functions.md)
* [R ve SQL Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) verileri ile çalışma](sql-database-machine-learning-services-data-issues.md)
* [Azure SQL veritabanı Machine Learning hizmetlerini (Önizleme) bir R paketi ekleme](sql-database-machine-learning-services-add-r-packages.md)
