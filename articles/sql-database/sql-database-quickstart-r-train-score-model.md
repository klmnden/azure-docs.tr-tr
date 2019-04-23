---
title: Oluşturma ve r ile Tahmine dayalı bir model eğitip
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) kullanarak R basit Tahmine dayalı bir model oluşturmak ve ardından yeni verileri kullanarak bir sonuçlarını tahmin edin.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 97309a24c0ab12720f968409856a16cab4ff7ac7
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013263"
---
# <a name="create-and-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Oluşturma ve r ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir model eğitip

Bu hızlı başlangıçta, oluşturun ve R kullanarak Tahmine dayalı bir model eğitip, modeli, SQL veritabanı'nda bir tabloya kaydedin ve ardından genel önizlemesini kullanarak yeni verileri değerlerinden tahmin modelini kullanan [Machine Learning Hizmetleri (R ile) Azure SQL veritabanı'nda ](sql-database-machine-learning-services-overview.md). 

Bu hızlı başlangıçta kullanacaksınız modeli hızına göre bir araba durdurma uzaklık tahmin eden bir basit bir regresyon modelidir. Kullanacağınız **otomobiller** R ile küçük ve kolay anlaşılır olduğundan dahil veri kümesi.

> [!TIP]
> R çalışma zamanı küçük-büyük birçok veri kümesi içerir. R yüklü veri kümelerinin listesini almak için şunu yazın `library(help="datasets")` R komut isteminde.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa, [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

- Bu alıştırmalarda örnek kodu çalıştırmak için öncelikle etkin bir Azure SQL veritabanı ile Machine Learning Hizmetleri (R) sahip olması gerekir. Genel Önizleme sırasında Microsoft tarafından ekleyin ve machine learning, mevcut veya yeni bir veritabanı için etkinleştirin. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

- En son yüklediğinizden emin olun [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Diğer veritabanı yönetim veya sorgu Araçları'nı kullanarak R betikleri çalıştırabilir ancak bu hızlı başlangıçta SSMS kullanacaksınız.

- Bu hızlı başlangıçta, bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırmanız gerekir. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz: [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-server-level-firewall-rule.md).

## <a name="create-and-train-a-predictive-model"></a>Oluşturabilir ve Tahmine dayalı bir model eğitip

Araba hızı verileri **otomobiller** veri kümesini içeren iki sütunlu, iki sayısal: **dist** ve **hızı**. Veriler birden çok durdurma gözlemler farklı hızlarda içerir. Bu verilerden araba hız ve bir araba durdurmak için gerekli olan uzaklığı arasındaki ilişkiyi açıklar bir doğrusal regresyon modeli oluşturacaksınız.

Doğrusal modelin gereksinimleri oldukça basittir:
- Bağımlı değişkenin arasındaki ilişkiyi tanımlayan bir formül tanımla *hızı* ve bağımsız değişken *uzaklık*.
- Modelin eğitilmesi için kullanılacak giriş verilerini sağlayın.

> [!TIP]
> Doğrusal modellerinde tazelemek rxLinMod kullanarak bir model sığdırma işleminin açıklayan Bu öğreticiyi deneyin: [Doğrusal modelleri sığdırma](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model)

Eğitim verileri belirleyeceğim aşağıdaki adımlarda bir regresyon modeli oluşturun, eğitim verilerini kullanarak eğitin ve ardından bir SQL tablosunu modeli kaydedin.

1. **SQL Server Management Studio**’yu açın ve SQL veritabanınıza bağlanın.

   Bağlanma yardıma ihtiyacınız varsa bkz [hızlı başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak için SQL Server Management Studio'yu kullanın](sql-database-connect-query-ssms.md).

1. Oluşturma **CarSpeed** eğitim verilerini kaydetmek için tabloda.

    ```sql
    CREATE TABLE dbo.CarSpeed (
        speed INT NOT NULL
        , distance INT NOT NULL
        )
    GO
    
    INSERT INTO dbo.CarSpeed (
        speed
        , distance
        )
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

1. Kullanarak bir regresyon modeli oluşturun `rxLinMod`. 

   Model oluşturmak için formülü R kod içinde tanımlayın ve ardından eğitim verileri geçirin **CarSpeed** giriş parametresi olarak.

    ```sql
    DROP PROCEDURE IF EXISTS generate_linear_model;
    GO
    CREATE PROCEDURE generate_linear_model
    AS
    BEGIN
      EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N'
    lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
    trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));
    '
      , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
      , @input_data_1_name = N'CarsData'
      , @output_data_1_name = N'trained_model'
      WITH RESULT SETS ((model VARBINARY(max)));
    END;
    GO
    ```

     rxLinMod için ilk bağımsız değişken, mesafeyi hıza bağımlı olan bir değer olarak tanımlayan *formula* parametresidir. Giriş verileri SQL sorgusu tarafından doldurulan `CarsData` değişkeninde depolanır.

1. Tahmin için daha sonra kullanabilmeniz için model depoladığınız bir tablo oluşturun. 

   Bir model oluşturan bir R paketi genellikle çıktıdır bir **ikili nesne**tablo sütunu olmalıdır. Bu nedenle **VARBINARY(max)** türü.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
        );
    ```

1. Artık saklı yordamı çağırma, model oluşturmak ve bir tabloya kaydedin.

   ```sql
   INSERT INTO dbo.stopping_distance_models (model)
   EXECUTE generate_linear_model;
   ```

   Bu kodu ikinci kez çalıştırdığınızda şu hatayı alırsınız:

   ```text
   Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object bo.stopping_distance_models
   ```

   Bu hatayı önlemek için bir seçenek her yeni model adı güncelleştirmektir. Örneğin adı daha açıklayıcı bir değer olarak değiştirebilir ve model türü, oluşturduğunuz tarih gibi bilgileri ekleyebilirsiniz.

   ```sql
   UPDATE dbo.stopping_distance_models
   SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
   WHERE model_name = 'default model'
   ```

## <a name="view-the-table-of-coefficients"></a>Katsayılar tablosunu görüntüleyin

[sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamından döndürülen R çıkışı genellikle tek bir veri çerçevesiyle sınırlıdır. Ancak veri çerçevesine ek olarak skaler değer gibi diğer türlerdeki çıkışları da döndürebilirsiniz.

Örneğin, bir model eğitip ancak hemen tablosunu modelden katsayıları görüntülemek istediğinizi varsayalım. Bunu yapmak için ana sonucu kümesi ve eğitilen modele bir SQL değişken çıkış katsayıları tablosu oluşturun. Hemen modeli değişkeni çağrılarak yeniden kullanabilirsiniz veya burada gösterildiği gibi bir tablo modeli kaydedebilirsiniz.

```sql
DECLARE @model VARBINARY(max)
    , @modelname VARCHAR(30)

EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
speedmodel <- rxLinMod(distance ~ speed, CarsData)
modelbin <- serialize(speedmodel, NULL)
OutputDataSet <- data.frame(coefficients(speedmodel));
'
    , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
    , @input_data_1_name = N'CarsData'
    , @params = N'@modelbin varbinary(max) OUTPUT'
    , @modelbin = @model OUTPUT
WITH RESULT SETS(([Coefficient] FLOAT NOT NULL));

-- Save the generated model
INSERT INTO dbo.stopping_distance_models (
    model_name
    , model
    )
VALUES (
    'latest model'
    , @model
    )
```

**Sonuçlar**

![Ek çıktıya sahip eğitilen model](./media/sql-database-connect-query-r/r-train-model-with-additional-output.png)

## <a name="score-new-data-using-the-trained-model"></a>Eğitilen modeli kullanarak yeni verileri puanlamak

*Puanlama* veri bilimi Öngörüler, olasılıklar ve eğitilen modele beslenir yeni verilere bağlı olarak diğer değerleri oluşturma ortalama için kullanılan bir terimdir. Yeni veri karşı Öngörüler puanlamak için önceki bölümde oluşturduğunuz modeli kullanacağız.

Özgün eğitim verilerindeki en yüksek hızın saatte 25 mil olduğunu fark ettiniz mi? Bunun nedeni özgün verilerin 1920'de yapılan bir deneyden alınmış olmasıdır! Merak edebilirsiniz ne kadar süreyle bu 1920s 60 mil hızla veya hatta 100 mil hızla olarak hızlı şekilde başlayın, durdurmak için gelen bir otomobilin götürecek? Bu soruyu cevaplamak için bazı yeni hızı değerleri modelinize sağlayabilir.

1. Yeni Hızlı verileri bir tablo oluşturun.

   ```sql
    CREATE TABLE dbo.NewCarSpeed (
        speed INT NOT NULL
        , distance INT NULL
        )
    GO
    
    INSERT dbo.NewCarSpeed (speed)
    VALUES (40)
        , (50)
        , (60)
        , (70)
        , (80)
        , (90)
        , (100)
   ```

2. Bu yeni hızı değerleri durdurma mesafe tahmin edin.

   Modelinize bağlı olduğu **rxLinMod** parçası olarak sağlanan algoritması **RevoScaleR** paketi çağırmanızı [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) , genel R yerineişlevi`predict` işlevi.

   Bu örnek betik:
   - Tek bir model, tablo almak için bir SELECT deyimi kullanır.
   - Giriş parametresi olarak geçirir
   - Çağrıları `unserialize` modelini işlevi
   - Geçerli `rxPredict` modeline uygun bağımsız değişkenlerle işlevi
   - Yeni giriş verileri sağlar

   > [!TIP]
   > Gerçek zamanlı Puanlama için bkz: [serileştirme işlevleri](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) RevoScaleR tarafından sağlanan.

   ```sql
    DECLARE @speedmodel VARBINARY(max) = (
            SELECT model
            FROM dbo.stopping_distance_models
            WHERE model_name = 'latest model'
            );
    
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    current_model <- unserialize(as.raw(speedmodel));
    new <- data.frame(NewCarData);
    predicted.distance <- rxPredict(current_model, new);
    str(predicted.distance);
    OutputDataSet <- cbind(new, ceiling(predicted.distance));
    '
        , @input_data_1 = N'SELECT speed FROM [dbo].[NewCarSpeed]'
        , @input_data_1_name = N'NewCarData'
        , @params = N'@speedmodel varbinary(max)'
        , @speedmodel = @speedmodel
    WITH RESULT SETS((
                new_speed INT
                , predicted_distance INT
                ));
   ```

   **Sonuçlar**

   ![Durdurma mesafesini tahmin etmek için sonuç kümesi](./media/sql-database-connect-query-r/r-predict-stopping-distance-resultset.png)

> [!NOTE]
> Bu örnek betikte `str` r döndürülen veri şemasını denetlemek için sınama aşaması sırasında eklenen işlevi Deyimi daha sonra kaldırabilirsiniz.
>
> R betiğinde kullanılan sütun adlarının, saklı yordam çıkışına geçirilenlerle aynı olması şart değildir. Burada bazı yeni sütun adları ile sonuçları tümcesi tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

Machine Learning hizmetleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın. Bu makaleler bazıları için SQL Server olsa da, ilgili bilgilerin çoğunu da Machine Learning Hizmetleri (R ile) Azure SQL veritabanı için geçerlidir.

- [Azure SQL veritabanı Machine Learning Hizmetleri (R ile)](sql-database-machine-learning-services-overview.md)
- [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Öğretici: SQL Server'da R kullanarak veritabanında analizini öğrenin](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)
- [R ve SQL Server için uçtan uca veri bilimi kılavuzu](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough)
- [Öğretici: SQL Server verileri ile RevoScaleR R işlevlerini kullanma](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages)
