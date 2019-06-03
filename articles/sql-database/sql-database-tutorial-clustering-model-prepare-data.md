---
title: 'Öğretici: R içinde kümelemeyi gerçekleştirmek için verileri hazırlama'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) içinde kümelemeyi gerçekleştirmek için bir Azure SQL veritabanından veri bölümünde bu üç bölümlü öğretici serisinin birinci hazırlamanız.
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
ms.openlocfilehash: 83ef25f04012933c2665e63e4617d480eb336f7b
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419464"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) içinde kümelemeyi gerçekleştirmek için verileri hazırlama

R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) içinde kümelemeyi gerçekleştirmek için bir Azure SQL veritabanından veri bölümünde bu üç bölümlü öğretici serisinin birinci hazırlamanız.

*Kümeleme* bir grubun üyesi olduğu şekilde benzer gruplar halinde veri düzenleme olarak açıklanabilir.
Kullanacağınız **K-ortalamaları** algoritması, bir veri kümesindeki ürününün müşterilerin kümeleme gerçekleştirmek için satın alma ve döndürür. Müşteriler Kümelemesi tarafından pazarlama etkinliklerinize daha etkili bir şekilde belirli grupları hedefleyerek odaklanabilirsiniz.
K-ortalamaları kümeleme olduğu bir *Denetimsiz öğrenme* verilerdeki desenleri arar algoritması üzerinde benzerlikler tabanlı.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Örnek bir veritabanı bir Azure SQL veritabanı'na aktarma
> * Farklı müşteriler farklı boyutlarda boyunca
> * Verileri Azure SQL veritabanı'ndan R kullanarak bir veri çerçevesine yükleyin.

İçinde [bölüm iki](sql-database-tutorial-clustering-model-build.md), K-ortalamaları kümeleme modeli eğitmek ve oluşturma hakkında bilgi edineceksiniz.

İçinde [üçüncü kısmı](sql-database-tutorial-clustering-model-deploy.md), yeni verileri temel alan kümeleme yapabilmek için bir Azure SQL veritabanında bir saklı yordam oluşturma öğreneceksiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa, azure aboneliği - [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

* Machine Learning Hizmetleri ile Azure SQL veritabanı sunucusu etkin - genel Önizleme süresince Microsoft ekleme ve mevcut veya yeni veritabanlarınız için makine öğrenimi etkinleştir. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

* RevoScaleR package - bakın [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) için bu paketi yerel olarak yüklemek Seçenekler.

* R IDE - Bu öğreticide [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* Bu öğreticide SQL sorgu aracı - varsayılır kullanmakta olduğunuz [Azure veri Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="import-the-sample-database"></a>Örnek veritabanını içeri aktarma

Bu öğreticide kullanılan örnek veri kümesi için kaydedilmiş bir **.bacpac** veritabanı yedek dosyasını indirin ve kullanın. Bu veri kümesi türetilir [tpcx bb](http://www.tpc.org/tpcx-bb/default.asp) tarafından sağlanan bir veri kümesi [hareket işleme performans Council (TPC)](http://www.tpc.org/default.asp).

1. Dosya indirme [tpcxbb_1gb.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bacpac).

1. Bölümündeki yönergeleri izleyin [bir Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-import), bu ayrıntıları kullanarak:

   * Alınacak **tpcxbb_1gb.bacpac** indirdiğiniz dosya
   * Genel Önizleme sırasında seçin **5. nesil/sanal çekirdek** yeni veritabanı için yapılandırma
   * "Tpcxbb_1gb" Yeni veritabanı adı

## <a name="separate-customers"></a>Ayrı müşteriler

RStudio içinde yeni bir RScript dosyası oluşturun ve aşağıdaki betiği çalıştırın.
SQL sorgusunda aşağıdaki boyutlar boyunca müşteriler ayırma:

* **orderRatio** = dönüş sırası oranı (kısmen veya tamamen siparişler toplam sayısı döndürülen siparişler toplam sayısı)
* **itemsRatio** dönüş öğesi oranı (toplam satın alınan öğelerin sayısı ile döndürülen öğe sayısını) =
* **monetaryRatio** = iade miktarı oranı (toplam parasal miktarını satın alınan miktarı döndürülen öğe)
* **Sıklık** dönüş sıklığı =

İçinde **yapıştırın** işlev, değiştirin **sunucu**, **UID**, ve **PWD** kendi bağlantı bilgileriyle.

```r
# Define the connection string to connect to the tpcxbb_1gb database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=tpcxbb_1gb",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");


#Define the query to select data from SQL Server
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
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
"
```

## <a name="load-the-data-into-a-data-frame"></a>Verileri bir veri çerçevesine yükleyin

Artık bir R verileri kullanarak çerçeve sorgunun sonuçları döndürmek için aşağıdaki betiği kullanın **rxSqlServerData** işlevi.
İşleminin bir parçası olarak, Seçili sütunlar (türleri için r doğru bir şekilde aktarılır emin olmak için colClasses kullanarak) için türü tanımlarsınız.

```r
# Query SQL Server using input_query and get the results back
# to data frame customer_returns
# Define the types for selected columns (using colClasses),
# to make sure that the types are correctly transferred to R
customer_returns <- rxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);

# Transform the data from an input dataset to an output dataset
customer_data <- rxDataStep(customer_returns);

# Take a look at the data just loaded from SQL Server
head(customer_data, n = 5);
```

Aşağıdakine benzer bir sonuç görmeniz gerekir.

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

***Bu öğretici ile devam etmek için gideceğinizi değil,***, Azure SQL veritabanı sunucunuzdan tpcxbb_1gb veritabanını silin.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalında sol taraftaki menüden seçin **tüm kaynakları** veya **SQL veritabanları**.
1. İçinde **ada göre Filtrele...**  alanına **tpcxbb_1gb**ve aboneliğinizi seçin.
1. Seçin, **tpcxbb_1gb** veritabanı.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Serinin Bu öğretici serisinde, bu adımlar tamamlandı:

* Örnek bir veritabanı bir Azure SQL veritabanı'na aktarma
* Farklı müşteriler farklı boyutlarda boyunca
* Verileri Azure SQL veritabanı'ndan R kullanarak bir veri çerçevesine yükleyin.

Bir machine learning bu müşteri verilerini modeli oluşturmak için Bu öğretici serisinin oluşan izleyin:

> [!div class="nextstepaction"]
> [Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir model oluşturma](sql-database-tutorial-clustering-model-build.md)
