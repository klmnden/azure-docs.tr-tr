---
title: 'Öğretici: R ile Tahmine dayalı model dağıtma'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Öğreticinin bu üç bölümlük üç, Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile R Tahmine dayalı bir model dağıtacaksınız.
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
ms.date: 05/02/2019
ms.openlocfilehash: 17b68f71f4034e5eb637d40b975cc22d94438fb7
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978698"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: R ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı model dağıtma

Öğreticinin bu üç bölümlük üç, Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile R Tahmine dayalı bir model dağıtacaksınız.

Bir saklı yordamı ile modelini kullanarak tahminlerde bir katıştırılmış bir R betiği oluşturacaksınız. Modelinizin, Azure SQL veritabanı'nda yürütüldüğünden, veritabanında depolanan verilere kolayca düşünürler.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Bir veritabanı tablosu, Tahmine dayalı bir model Store
> * Modeli oluşturan bir saklı yordam oluşturma
> * Modeli kullanarak tahminlerde bir saklı yordam oluşturma
> * Yeni veri modeliyle yürütün

İçinde [Birinci bölümde](sql-database-tutorial-predictive-model-prepare-data.md), örnek veritabanını bir Azure SQL veritabanına içeri aktarmak ve ardından r'deki Tahmine dayalı bir modeli eğitmek için kullanılacak veri hazırlama hakkında bilgi edindiniz

İçinde [bölüm iki](sql-database-tutorial-predictive-model-build-compare.md), oluşturma ve birden çok modeli eğitmek ve sonra en doğru olanı seçin hakkında bilgi edindiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Bu öğretici serisinin üç bölümünü varsayar tamamladığınızdan [ **Birinci bölümde** ](sql-database-tutorial-predictive-model-prepare-data.md) ve [ **bölüm iki**](sql-database-tutorial-predictive-model-build-compare.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Modeli oluşturan bir saklı yordam oluşturma

Bu öğretici serisinin kısmında iki, karar ağacı (dtree) modeli en doğru olduğunu verdi. Artık bir saklı yordam oluşturma (`generate_rental_rx_model`) eğitir ve RevoScaleR paketinden rxDTree kullanarak dtree modeli oluşturur.

Azure veri Studio veya SSMS aşağıdaki komutları çalıştırın.

```sql
-- Stored procedure that trains and generates an R model using the rental_data and a decision tree algorithm
DROP PROCEDURE IF EXISTS generate_rental_rx_model;
GO
CREATE PROCEDURE generate_rental_rx_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
model_dtree <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Bir veritabanı tablosu, model Store

Tutorialdb'yi veritabanında bir tablo oluşturun ve tabloya modeli kaydedin.

1. Bir tablo oluşturun (`rental_rx_models`) modelini depolamak için.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_rx_models;
    GO
    CREATE TABLE rental_rx_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Modelin model adı "rxDTree" ile ikili bir nesne olarak tabloya kaydedin.

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_rx_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_rx_model @model OUTPUT;
    
    INSERT INTO rental_rx_models (
          model_name
        , model
        )
    VALUES (
         'rxDTree'
        , @model
        );
    
    SELECT *
    FROM rental_rx_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Öngörüler sağlayan bir saklı yordam oluşturma

Bir saklı yordam oluşturma (`predict_rentalcount_new`) eğitilen model ve yeni veri kümesi kullanarak tahmin sağlar.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @rx_model VARBINARY(MAX) = (
            SELECT model
            FROM rental_rx_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
require("RevoScaleR");

#Convert types to factors
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(rx_model);

#Call prediction function
rental_predictions <- rxPredict(rental_model, rentals);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@rx_model varbinary(max)'
        , @rx_model = @rx_model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Yeni veri modeliyle yürütün

Saklı yordam artık `predict_rentalcount_new` yeni verilerden kiralama sayısını tahmin etmek için.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'rxDTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

Aşağıdakine benzer bir sonuç görmeniz gerekir.

```results
RentalCount_Predicted
332.571428571429
```

Başarıyla oluşturulan, eğitim ve bir modeli bir Azure SQL veritabanı'nda dağıtılır. Ardından değerleri yeni verileri temel alan tahmin etmek için bu modelin bir saklı yordamda kullanılan.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tutorialdb'yi veritabanı kullanmayı bitirdiğinizde, Azure SQL veritabanı sunucunuzdan silin.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalında sol taraftaki menüden seçin **tüm kaynakları** veya **SQL veritabanları**.
1. İçinde **ada göre Filtrele...**  alanına **Tutorialdb'yi**ve aboneliğinizi seçin.
1. Tutorialdb'yi veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde Bu öğretici serisinin üç adımları tamamladınız:

* Bir veritabanı tablosu, Tahmine dayalı bir model Store
* Modeli oluşturan bir saklı yordam oluşturma
* Modeli kullanarak tahminlerde bir saklı yordam oluşturma
* Yeni veri modeliyle yürütün

R kullanarak Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) hakkında daha fazla bilgi için bkz:

* [Machine Learning Hizmetleri (Önizleme) kullanarak Azure SQL veritabanı içinde Gelişmiş R işlevler yazma](sql-database-machine-learning-services-functions.md)
* [R ve SQL Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) verileri ile çalışma](sql-database-machine-learning-services-data-issues.md)
* [Azure SQL veritabanı Machine Learning hizmetlerini (Önizleme) bir R paketi ekleme](sql-database-machine-learning-services-add-r-packages.md)
