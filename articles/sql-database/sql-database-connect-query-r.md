---
title: "Hızlı başlangıç: Azure SQL Veritabanı'nda Machine Learning Services'i (R ile) kullanma (önizleme) | Microsoft Docs"
description: Bu konu başlığında Azure SQL Veritabanı'nda Machine Learning Services'i kullanma, ve büyük ölçekli gelişmiş analizler sunmak için R betiklerini çalıştırmanın yanı sıra hesaplama ve işlem süreçlerini verilerin bulunduğu yere getirerek verileri ağ üzerinden çekme ihtiyacını ortadan kaldırma adımları gösterilmektedir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.reviewer: ''
manager: cgronlun
ms.date: 04/04/2019
ms.openlocfilehash: f5d43794ba80e4791c8bf24ad404069a8329518e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59051232"
---
# <a name="quickstart-use-machine-learning-services-with-r-in-azure-sql-database-preview"></a>Hızlı Başlangıç: Machine Learning Hizmetleri (R ile) Azure SQL veritabanı (Önizleme) kullanma

Bu makalede, genel önizlemeye sunulduğunu nasıl kullanabileceğini açıklar [Machine Learning Hizmetleri (R ile) Azure SQL veritabanı'nda](sql-database-machine-learning-services-overview.md). Bir SQL veritabanı ile R arasında veri alışverişi yapmanın temel adımları anlatılmaktadır. Ayrıca bir SQL veritabanında makine öğrenmesi modeli oluşturmak, eğitmek ve kullanmak için [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamında doğru oluşturulmuş R kodu sarmalama süreçleri açıklanmaktadır.

Gelişmiş analiz ve makine öğrenimi veritabanında sunmak üzere R dilinin gücünü kullanın. Bu yetenek, hesaplamaları ve verilerin bulunduğu işlem, ağ üzerinden veri çekmek için ihtiyacını getirir. Ayrıca, ölçeğinde Gelişmiş analiz sağlamak üzere Kurumsal R paketleri gücünden yararlanın.

Machine Learning Hizmetleri ile Kurumsal R paketleri Microsoft gelen yayılan r, temel bir dağıtım içerir. Microsoft R işlevleri ve algoritmaları, ölçek ve Tahmine dayalı analiz, modelleme, veri görselleştirmeleri ve öncü makine öğrenimi algoritması sunmaya yardımcı programı için tasarlanmıştır.

Azure aboneliğiniz yoksa, [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

> [!IMPORTANT]
> Azure SQL veritabanı Machine Learning Hizmetleri şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

## <a name="prerequisites"></a>Önkoşullar

Bu alıştırmalardaki örnek kodu çalıştırmak için Machine Learning Services (R ile) özelliğinin etkinleştirilmiş olduğu bir SQL veritabanına sahip olmanız gerekir. Genel Önizleme sırasında Microsoft tarafından ekleyin ve machine learning, mevcut veya yeni bir veritabanı için etkinleştirin. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

SQL Veritabanı bağlantısı yapabilen ve T-SQL sorgusu veya saklı yordam çalıştırabilen tüm veritabanı yönetim veya sorgu araçlarıyla SQL Veritabanı'na bağlanabilir ve R betiklerini çalıştırabilirsiniz. Bu hızlı başlangıçta [SQL Server Management Studio](sql-database-connect-query-ssms.md) kullanılmıştır.

[Paket ekleme](#add-package) alıştırması için yerel bilgisayarınıza ayrıca [R](https://www.r-project.org/) ve [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) uygulamalarını da yüklemeniz gerekecektir.

Bu hızlı başlangıç ayrıca sunucu düzeyinde bir güvenlik duvarı kuralı yapılandırması gerektirir. Bunun nasıl yapılacağını gösteren bir hızlı başlangıç için bkz: [Sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-server-level-firewall-rule.md).

## <a name="verify-r-exists"></a>R ortamının var olduğundan emin olun

Machine Learning Services (R ile) özelliklerinin SQL veritabanınızda etkinleştirildiğini onaylayabilirsiniz. Aşağıdaki adımları izleyin.

1. SQL Server Management Studio’yu açın ve SQL veritabanınıza bağlanın. Bağlanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak için SQL Server Management Studio'yu kullanın](sql-database-connect-query-ssms.md).

1. Aşağıdaki kodu çalıştırın. 

    ```sql
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(31 + 11)';
    GO
    ```
    Her şey yolunda giderse aşağıdaki gibi bir sonuç iletisi görmeniz gerekir.

    ```text
    STDOUT message(s) from external script: 
    42
    ```

1. Hata alırsanız Machine Learning Services (R ile) genel önizleme sürümü SQL veritabanınız için etkinleştirilmemiş olabilir. Genel önizleme için kaydolma adımlarını yukarıda bulabilirsiniz.

## <a name="grant-permissions"></a>İzinleri verme

Bir yöneticiyseniz, harici kod otomatik olarak çalıştırabilir. Diğer herkes iznine sahip olmanız gerekir.

Değiştirin `<username>` komutu çalıştırmadan önce geçerli bir veritabanı kullanıcı oturum açma ile.

```sql
GRANT EXECUTE ANY EXTERNAL SCRIPT TO <username>
```

## <a name="basic-r-interaction"></a>Temel R etkileşimi

SQL Veritabanı'nda R kodu çalıştırmak için iki seçeneğiniz vardır:

+ Sistem saklı yordam bağımsız değişken olarak R betiği eklemek [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).
+ [Uzak R istemcisinden](https://docs.microsoft.com/sql/advanced-analytics/r/set-up-a-data-science-client) SQL veritabanınıza bağlanma ve kodu çalıştırırken SQL Veritabanı'nı işlem bağlamı olarak kullanma.

Aşağıdaki alıştırma, ilk etkileşim modeline odaklanmıştır: R kodunu bir saklı yordama geçirme.

1. Nasıl SQL veritabanınızda bir R betiği yürüten görmek için basit bir komut dosyasını çalıştırın.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R',
    @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

2. Her şeyi doğru bir şekilde ayarladığınızı kabul edersek doğru sonuç hesaplanır ve R `print` işlevi sonucu **İletiler** penceresine döndürür.

    **Sonuçlar**

    ```text
    STDOUT message(s) from external script: 
    0.5 2
    ```

    **stdout** iletileri almak kodunuzu test etme aşamasında faydalı olsa da genellikle sonuçların bir uygulamada kullanabilmek veya bir tabloya yazabilmek için tablo biçiminde döndürülmesini istersiniz. Daha fazla bilgi için aşağıdaki girişler ve çıkışlar bölümüne bakın.

`@script` bağımsız değişkeninin içindeki her şeyin geçerli R kodu olması gerektiğini unutmayın.

## <a name="inputs-and-outputs"></a>Girişler ve çıkışlar

Varsayılan olarak [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) tek bir giriş veri kümesi kabul eder ve bu da genellikle geçerli bir SQL sorgusu biçiminde verilir. Diğer giriş türleri SQL değişkenleri olarak geçirilebilir.

Saklı yordam, çıkış olarak tek bir R veri çerçevesi döndürür ancak değişken olarak skaler değerlerin ve modellerin de çıkış olarak döndürülmesini sağlayabilirsiniz. Örneğin eğitilmiş bir modeli ikili değişken olarak çıkarabilir ve modeli bir tabloya yazmak için bir T-SQL INSERT deyimine geçirebilirsiniz. Ayrıca çizimler (ikili biçimde) veya skaler değerler (tarih ve saat gibi ayrı değerler, modeli eğitmek için harcanan süre gibi) oluşturabilirsiniz.

Şu an için yalnızca varsayılan sp_execute_external_script giriş ve çıkış değişkenlerine bakalım: `InputDataSet` ve `OutputDataSet`.

1. Aşağıdaki T-SQL deyimini çalıştırarak test verilerini içeren küçük bir tablo oluşturun:

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    INSERT INTO RTestData VALUES (1);
    INSERT INTO RTestData VALUES (10);
    INSERT INTO RTestData VALUES (100);
    GO
    ```

    Oluşturulduktan sonra aşağıdaki deyimi kullanarak tabloyu sorgulayın:
  
    ```sql
    SELECT * FROM RTestData
    ```

    **Sonuçlar**

    ![RTestData tablosunun içeriği](./media/sql-database-connect-query-r/select-rtestdata.png)

2. Tablo verilerini R betiğinizin girişi olarak kullanabilirsiniz. Aşağıdaki deyimi çalıştırın. Bu deyim tablodaki verileri alır, R çalışma zamanından gidiş dönüş gerçekleştirir ve değerleri *NewColName* sütun adıyla döndürür.

    Sorgu tarafından döndürülen değerler R çalışma zamanına geçirilir ve o da verileri SQL Veritabanı'na veri çerçevesi olarak döndürür. WITH RESULT SETS yan tümcesi SQL Veritabanı için döndürülen veri tablosunun şemasını tanımlar.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    **Sonuçlar**

    ![Tablodan veri döndüren R betiğinin çıktısı](./media/sql-database-connect-query-r/r-output-rtestdata.png)

3. Şimdi giriş veya çıkış değişkenlerinin adını değiştirelim. Yukarıdaki betikte varsayılan giriş ve çıkış değişkeni adları olan _InputDataSet_ ve _OutputDataSet_ kullanılmıştı. İle ilişkili giriş verileri tanımlamak için _InputDatSet_, kullandığınız  *\@input_data_1* değişkeni.

    Bu betikte saklı yordam çıkış ve giriş değişkenlerinin adları *SQL_out* ve *SQL_in* olarak değiştirilmiştir:

    ```sql
    EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N' SQL_out <- SQL_in;'
      , @input_data_1 = N' SELECT 12 as Col;'
      , @input_data_1_name  = N'SQL_in'
      , @output_data_1_name =  N'SQL_out'
      WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    R ortamının büyük/küçük harfe duyarlı olduğu unutmayın. Bu durumda `@input_data_1_name` ve `@output_data_1_name` içindeki giriş ve çıkış değişkenlerinin büyük/küçük harf durumunun `@script` içindeki R kodunda bulunanlarla aynı olması gerekir. 

    Ayrıca parametre sırası da önemlidir. Gerekli parametreleri belirtmeniz gerekir  *\@input_data_1* ve  *\@output_data_1* isteğe bağlı parametreler kullanabilmek için ilk olarak,  *\@ input_data_1_name* ve  *\@output_data_1_name*.

    Yalnızca bir giriş veri kümesi parametre olarak iletilebilir ve yalnızca bir veri kümesini döndürebilirsiniz. Ancak R kodunuzdan diğer tüm veri kümelerini çağırabilir ve veri kümesine ek olarak diğer türlerin çıkışlarını döndürebilirsiniz. Ayrıca OUTPUT anahtar sözcüğünü herhangi bir parametreye ekleyerek sonuçlarla birlikte döndürülmesini sağlayabilirsiniz. 

    `WITH RESULT SETS` deyimi SQL Veritabanı'nda kullanılan verilerin şemasını tanımlar. R ortamından döndürdüğünüz her sütun için SQL ile uyumlu veri türlerini kullanmanız gerekir. R veri çerçevesindeki sütun adlarını kullanmanız gerekmediğinden şema tanımını kullanarak yeni sütun adları da belirtebilirsiniz.

4. Ayrıca R betiği kullanarak değer oluşturabilir ve _@input_data_1_ içindeki giriş sorgusu dizesini boş bırakabilirsiniz.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N' mytextvariable <- c("hello", " ", "world");
            OutputDataSet <- as.data.frame(mytextvariable);'
        , @input_data_1 = N''
    WITH RESULT SETS (([Col1] CHAR(20) NOT NULL));
    ```

    **Sonuçlar**

    ![Giriş olarak @script kullanarak sonuçları sorgulama](./media/sql-database-connect-query-r/r-data-generated-output.png)

## <a name="check-r-version"></a>R sürümünü denetleme

SQL veritabanınızda yüklü R sürümünü görmek istiyorsanız şunu yapın:

1. Aşağıdaki betiği SQL veritabanınızda çalıştırın.

    ```SQL
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(version)';
    GO
    ```

2. R `print` işlevi, sürümü **İletiler** penceresinde döndürür. Aşağıdaki örnek çıktıda SQL veritabanında yüklü R sürümünün 3.4.4 olduğunu görebilirsiniz.

    **Sonuçlar**

    ```text
    STDOUT message(s) from external script:
                   _
    platform       x86_64-w64-mingw32
    arch           x86_64
    os             mingw32
    system         x86_64, mingw32
    status
    major          3
    minor          4.4
    year           2018
    month          03
    day            15
    svn rev        74408
    language       R
    version.string R version 3.4.4 (2018-03-15)
    nickname       Someone to Lean On
    ```

## <a name="list-r-packages"></a>R paketlerinin listesi

Microsoft, SQL veritabanınızda önceden Machine Learning Services ile birlikte yüklenmiş bir dizi R paketi sunar. Sürüm, bağımlılıklar, lisans ve kitaplık yolu bilgileri dahil olmak üzere yüklü R paketlerinin bir listesini görmek için aşağıdaki adımları izleyin. Daha fazla paket eklemek için [paket ekleme](#add-package) bölümüne bakın.

1. Aşağıdaki betiği SQL veritabanınızda çalıştırın.

    ```SQL
    EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
    OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
    WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
        , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
    ```

2. Çıkış R içindeki `installed.packages()` tarafından bir sonuç kümesi olarak döndürülür.

    **Sonuçlar**

    ![R içindeki yüklü paketler](./media/sql-database-connect-query-r/r-installed-packages.png)

## <a name="create-a-predictive-model"></a>Tahmine dayalı model oluşturma

R kullanarak model eğitebilir ve bu modeli SQL veritabanınızda bir tablo olarak kaydedebilirsiniz. Bu alıştırmada bir arabanın hızına göre durma mesafesini tahmin eden basit bir regresyon modeli eğiteceksiniz. Küçük ve anlaşılması kolay olduğundan R ile birlikte gelen `cars` veri kümesini kullanacaksınız.

1. İlk olarak eğitim verilerinin kaydedileceği tabloyu oluşturun.

    ```sql
    CREATE TABLE dbo.CarSpeed (speed INT NOT NULL, distance INT NOT NULL)
    GO
    INSERT INTO dbo.CarSpeed (speed, distance)
    EXEC sp_execute_external_script
    @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

    R çalışma zamanı küçük-büyük birçok veri kümesi içerir. R ile birlikte yüklenen veri kümelerinin listesini almak için R komut istemine `library(help="datasets")` yazın.

2. Bir regresyon modeli oluşturun. Araba hız verileri ikisi de sayısal olan `dist` ve `speed` sütunlarına sahiptir. Bazı hızlarda birden fazla gözlem vardır. Bu verileri kullanarak araba hızı ile arabanın durması için gerekli mesafe arasındaki ilişkiyi anlatan doğrusal bir regresyon modeli oluşturacaksınız.

    Doğrusal modelin gereksinimleri oldukça basittir:

   - `speed` bağımlı değişkeni ile `distance` bağımsız değişkeni arasındaki ilişkiyi anlatan bir formül tanımlayın.

   - Modelin eğitilmesi için kullanılacak giriş verilerini sağlayın.

     > [!TIP]
     > Bir Doğrusal Model tazelemek gerekiyorsa, bu öğreticide rxLinMod kullanarak bir model sığdırma işlemi açıklanır öneririz: [Doğrusal modelleri sığdırma](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-linear-model)

     Modeli oluşturmak için formülü R kodunuzda tanımlar ve verileri giriş parametresi olarak geçirirsiniz.

     ```sql
     DROP PROCEDURE IF EXISTS generate_linear_model;
     GO
     CREATE PROCEDURE generate_linear_model
     AS
     BEGIN
       EXEC sp_execute_external_script
       @language = N'R'
       , @script = N'lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
           trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));'
       , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
       , @input_data_1_name = N'CarsData'
       , @output_data_1_name = N'trained_model'
       WITH RESULT SETS ((model VARBINARY(max)));
     END;
     GO
     ```

     rxLinMod için ilk bağımsız değişken, mesafeyi hıza bağımlı olan bir değer olarak tanımlayan *formula* parametresidir. Giriş verileri SQL sorgusu tarafından doldurulan `CarsData` değişkeninde depolanır. Giriş verilerinize belirli bir ad atamazsanız varsayılan değişken adı _InputDataSet_ olur.

2. Ardından yeniden eğitmek veya tahmin için kullanmak üzere modeli depolayacağınız bir tablo oluşturun. R paketinin model oluşturan çıkışı genellikle bir **ikili nesnedir**. Bu nedenle tabloda **VARBINARY(max)** türünde bir sütun bulunması gerekir.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
    );
    ```
3. Modeli kaydetmek için saklı yordamı çağıran, modeli oluşturan ve bir tabloya kaydeden aşağıdaki Transact-SQL deyimini çağırın.

    ```sql
    INSERT INTO dbo.stopping_distance_models (model)
    EXEC generate_linear_model;
    ```

    Bu kodu ikinci kez çalıştırdığınızda şu hatayı alırsınız:

    ```text
    Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object dbo.stopping_distance_models
    ```

    Bu hatadan kaçınmanın yollarından biri, adı her yeni model için güncelleştirmektir. Örneğin adı daha açıklayıcı bir değer olarak değiştirebilir ve model türü, oluşturduğunuz tarih gibi bilgileri ekleyebilirsiniz.

    ```sql
    UPDATE dbo.stopping_distance_models
    SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
    WHERE model_name = 'default model'
    ```

4. [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamından döndürülen R çıkışı genellikle tek bir veri çerçevesiyle sınırlıdır.

    Ancak veri çerçevesine ek olarak skaler değer gibi diğer türlerdeki çıkışları da döndürebilirsiniz.

    Örneğin bir model eğitmek istediğinizi ancak modelin katsayılarından oluşan bir tabloyu hemen görüntülemek istediğinizi düşünelim. Katsayı tablosunu ana sonuç kümesi olarak oluşturabilir ve eğitilen modelin çıkışını bir SQL değişkeni olarak yapabilirsiniz. Değişkeni çağırarak modeli hemen yeniden kullanabilir veya modeli burada gösterilen şekilde bir tabloya kaydedebilirsiniz.

    ```sql
    DECLARE @model VARBINARY(max), @modelname VARCHAR(30)
    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
            speedmodel <- rxLinMod(distance ~ speed, CarsData)
            modelbin <- serialize(speedmodel, NULL)
            OutputDataSet <- data.frame(coefficients(speedmodel));'
        , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
        , @input_data_1_name = N'CarsData'
        , @params = N'@modelbin varbinary(max) OUTPUT'
        , @modelbin = @model OUTPUT
        WITH RESULT SETS (([Coefficient] FLOAT NOT NULL));

    -- Save the generated model
    INSERT INTO dbo.stopping_distance_models(model_name, model)
    VALUES ('latest model', @model)
    ```

    **Sonuçlar**

    ![Ek çıktıya sahip eğitilen model](./media/sql-database-connect-query-r/r-train-model-with-additional-output.png)

## <a name="predict"></a>Tahmin etme

Önceki bölümde oluşturduğunuz modeli kullanarak yeni verilerle ilgili tahminlerde bulunabilirsiniz. Yeni verileri kullanarak _puanlama_ gerçekleştirmek için tablodan eğitilmiş modellerden birini alın ve tahmin gerçekleştirmek istediğiniz yeni veri kümesini çağırın. Puanlama bazen veri biliminde tahminler, olasılıklar veya eğitilmiş bir modele aktarılan yeni verilere göre farklı değerler elde etmek için kullanılan bir terimdir.

1. İlk olarak yeni hız verilerine sahip bir tablo oluşturun. Özgün eğitim verilerindeki en yüksek hızın saatte 25 mil olduğunu fark ettiniz mi? Bunun nedeni özgün verilerin 1920'de yapılan bir deneyden alınmış olmasıdır! 1920'lerde yapılmış bir otomobilin saatte 60 mil veya 100 mil hıza çıkabilmesi durumunda ne kadar mesafede durabileceğini merak ediyor olabilirsiniz. Bu soruyu yanıtlamak için bazı yeni değerler belirtmeniz gerekir.

    ```sql
    CREATE TABLE dbo.NewCarSpeed(speed INT NOT NULL,
        distance INT NULL
    )
    GO
    INSERT dbo.NewCarSpeed(speed)
    VALUES (40), (50), (60), (70), (80), (90), (100)
    ```

    Bu örnekte modeliniz **RevoScaleR** paketinin bir parçası olarak sunulan **rxLinMod** algoritmasını temel aldığından genel R `predict` işlevi yerine [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) işlevini çağırmanız gerekir.

    ```sql
    DECLARE @speedmodel varbinary(max) = 
        (SELECT model FROM dbo.stopping_distance_models WHERE model_name = 'latest model');

    EXEC sp_execute_external_script
        @language = N'R'
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
    WITH RESULT SETS ((new_speed INT, predicted_distance INT));
    ```

    Yukarıdaki betik aşağıdaki adımları gerçekleştirir:

   + SELECT deyimini kullanarak tablodan tek bir model alır ve giriş parametresi olarak geçirir.

   + Tablodan modeli aldıktan sonra modelde `unserialize` işlevini çağırır.

       > [!TIP] 
       > RevoScaleR tarafından sunulan ve gerçek zamanlı puanlama desteğine sahip yeni [serileştirme işlevlerini](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) de inceleyin.
   + `rxPredict` işlevini uygun bağımsız değişkenlerle modele uygular ve yeni giriş verileri sağlar.

   + Örnekte test aşamasında R tarafından döndürülen verilerin şemasını kontrol etmek için `str` işlevi eklenmiştir. Deyimi daha sonra kaldırabilirsiniz.

   + R betiğinde kullanılan sütun adlarının, saklı yordam çıkışına geçirilenlerle aynı olması şart değildir. Burada yeni sütun adları tanımlamak için WITH RESULTS yan tümcesini kullandık.

     **Sonuçlar**

     ![Durdurma mesafesini tahmin etmek için sonuç kümesi](./media/sql-database-connect-query-r/r-predict-stopping-distance-resultset.png)

     Kayıtlı modeli temel alan bir tahmini değer veya puan oluşturmak için [Transact-SQL içinde PREDICT](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) de kullanılabilir.

<a name="add-package"></a>

## <a name="add-a-package"></a>Paket ekleme

SQL veritabanınızda yüklü olmayan bir paketi kullanmanız gerekiyorsa [sqlmlutils](https://github.com/Microsoft/sqlmlutils) ile yükleyebilirsiniz. Paketi yüklemek için aşağıdaki adımları izleyin.

1. **sqlmlutils** zip dosyasının son sürümünü [github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist) adresinden yerel bilgisayarınıza indirin. Dosyanın sıkıştırmasını açmanız gerekmez.

1. R yüklü değilse [www.r-project.org](https://www.r-project.org/) adresinden indirin ve yerel bilgisayarınıza yükleyin. R; Windows, MacOS ve Linux platformlarında kullanılabilir. Bu örnekte Windows kullanılmıştır.

1. İlk olarak **sqlmlutils** önkoşulu olan **RODBCext** paketini yükleyin. **RODBCext** ayrıca **RODBC** paketi bağımlılığını da yükler. **Komut İstemi** açın ve şu komutu çalıştırın:

    ```
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    ```

    Şu hatayı alırsanız, "'R' tanınmayan bir iç ya da dış komut, çalıştırılabilir program veya toplu iş dosyası", büyük olasılıkla R.exe yolu içinde bulunmaz geldiğini, **yolu** Windows ortam değişkeni. Dizin için ortam değişkenine ekleyin veya komut isteminde dizinine gidin (örneğin `cd C:\Program Files\R\R-3.5.1\bin`) komutunu çalıştırmadan önce.

1. **R CMD INSTALL** komutunu kullanarak **sqlmlutils** dosyasını yükleyin. Zip dosyasını indirdiğiniz dizini ve dosyanın adını belirtin. Örneğin:

    ```
    R CMD INSTALL C:\Users\youruser\Downloads\sqlmlutils_0.5.0.zip
    ```

    Gördüğünüz çıktının aşağıdakine benzer olması gerekir:

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/youruser/Documents/R/win-library/3.5'
    package 'sqlmlutils' successfully unpacked and MD5 sums checked
    ```

1. Bu örnekte IDE olarak RStudio Desktop kullanılmıştır. İsterseniz başka bir IDE de kullanabilirsiniz. RStudio Desktop uygulaması yüklü değilse [www.rstudio.com/products/rstudio/download/](https://www.rstudio.com/products/rstudio/download/) adresinden indirip yükleyin.

1. **RStudio**'yu açın ve yeni bir **R Script** dosyası oluşturun. Aşağıdaki R kodunu kullanarak sqlmlutils ile bir paket yükleyin. Aşağıdaki örnekte bir dizeyi biçimlendirebilen ve ilişkilendirebilen [glue](https://cran.r-project.org/web/packages/glue/) paketini yükleyeceksiniz.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!NOTE]
    > **scope** için **PUBLIC** veya **PRIVATE** değerini kullanabilirsiniz. Genel kapsam, veritabanı yöneticisinin tüm kullanıcıların kullanabileceği paketleri yüklemesi açısından yararlıdır. Özel kapsamda paket yalnızca yükleyen kullanıcı tarafından kullanılabilir. Kapsam değeri belirtmezseniz **PRIVATE** varsayılan kapsamı kullanılır.

1. Şimdi **glue** paketinin yüklenip yüklenmediğini doğrulayın.

    ```R
    r<-sql_installed.packages(connectionString = connection, fields=c("Package", "LibPath", "Attributes", "Scope"))
    View(r)
    ```

    **Sonuçlar**

    ![RTestData tablosunun içeriği](./media/sql-database-connect-query-r/r-verify-package-install.png)


1. Paket yüklendikten sonra **sp_execute_external_script** aracılığıyla R betiğinizde kullanabilirsiniz. **SQL Server Management Studio**’yu açın ve SQL veritabanınıza bağlanın. Şu betiği çalıştırın:

    ```sql
    EXEC sp_execute_external_script @language = N'R'
    , @script = N'
    library(glue)

    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
        ''my age next year is {age + 1}, '',
        ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    print(text)
    ';
    ```

    İletiler sekmesinde aşağıdaki sonucu görürsünüz.

    **Sonuçlar**

    ```text
    STDOUT message(s) from external script:
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

1. Paketi kaldırmak istiyorsanız yerel bilgisayarınızda **RStudio**'da aşağıdaki R betiğini çalıştırın.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC") 
    ```

> [!NOTE]
> R paketlerini SQL veritabanınıza yüklemenin bir diğer yolu da [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) ile bayt dizisinden R paketini yüklemektir.

## <a name="next-steps"></a>Sonraki adımlar

Machine Learning hizmetleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın. Bu makaleler bazıları için SQL Server olsa da, ilgili bilgilerin çoğunu da Machine Learning Hizmetleri (R ile) Azure SQL veritabanı için geçerlidir.

- [Azure SQL veritabanı Machine Learning Hizmetleri (R ile)](sql-database-machine-learning-services-overview.md)
- [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Öğretici: SQL Server'da R kullanarak veritabanında analizini öğrenin](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)
- [R ve SQL Server için uçtan uca veri bilimi kılavuzu](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough)
- [Öğretici: SQL Server verileri ile RevoScaleR R işlevlerini kullanma](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages)
