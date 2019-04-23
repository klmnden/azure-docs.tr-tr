---
title: Oluşturma ve basit R betikleri çalıştırma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) basit R betikleri çalıştırın.
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
ms.openlocfilehash: ada09959391c551a9eff4d96b186be29c1e3b7a8
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013269"
---
# <a name="create-and-run-simple-r-scripts-in-azure-sql-database-machine-learning-services-preview"></a>Oluşturma ve Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) basit R betikleri çalıştırma

Bu hızlı başlangıçta, oluşturacak ve bir dizi genel önizlemesini kullanarak basit R betikleri çalıştırma [Machine Learning Hizmetleri (R ile) Azure SQL veritabanı'nda](sql-database-machine-learning-services-overview.md). Saklı yordam, doğru biçimlendirilmiş bir R betiği kaydırma öğreneceksiniz [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) bir SQL veritabanı'nda bu betiği yürütün.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa, [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

- Bu alıştırmalarda örnek kodu çalıştırmak için öncelikle etkin bir Azure SQL veritabanı ile Machine Learning Hizmetleri (R) sahip olması gerekir. Genel Önizleme sırasında Microsoft tarafından ekleyin ve machine learning, mevcut veya yeni bir veritabanı için etkinleştirin. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

- En son yüklediğinizden emin olun [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Diğer veritabanı yönetim veya sorgu Araçları'nı kullanarak R betikleri çalıştırabilir ancak bu hızlı başlangıçta SSMS kullanacaksınız.

- Bu hızlı başlangıçta, bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırmanız gerekir. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz: [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-server-level-firewall-rule.md).

## <a name="run-a-simple-script"></a>Basit bir betik çalıştırma

Bir R betiğini çalıştırmak için bu bağımsız değişken olarak sistem saklı yordama ileteceksiniz [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

Aşağıdaki adımlarda, SQL veritabanı'nda bu örnek R betik çalıştıracaksınız:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. **SQL Server Management Studio**’yu açın ve SQL veritabanınıza bağlanın.

   Bağlanma yardıma ihtiyacınız varsa bkz [hızlı başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak için SQL Server Management Studio'yu kullanın](sql-database-connect-query-ssms.md).

1. Tam R komut dosyasına geçirmek [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamı.

   Komut dosyası aracılığıyla geçirilir `@script` bağımsız değişken. İçindeki her şey `@script` bağımsız değişken, geçerli R kodu olmalıdır.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

   Hata alırsanız Machine Learning Services (R ile) genel önizleme sürümü SQL veritabanınız için etkinleştirilmemiş olabilir. Bkz: [önkoşulları](#prerequisites) yukarıda.

   > [!NOTE]
   > Yöneticiyseniz, harici kod otomatik olarak çalıştırabilir. Komutunu kullanarak diğer kullanıcılara izni verebilirsiniz:
   <br>**GRANT herhangi dış BETİK YÜRÜTÜLECEK**  *\<kullanıcıadı\>*.

2. Doğru sonucu hesaplanır ve R `print` işlevi sonucu döndürür **iletileri** penceresi.

   şunun gibi görünmelidir.

    **Sonuçlar**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Hello World betik çalıştırma

Örnek betik yalnızca "Hello World" dizesi çıkarır biridir. Aşağıdaki komutu çalıştırın.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Bu saklı yordam için giriş içerir:

| | |
|-|-|
|*@language* | Bu durumda, R çağırmak için dil uzantısını tanımlar |
|*@script* | R çalışma için geçirilen komut tanımlar. Tüm R betiğinizi bu bağımsız değişken Unicode metin alınmalıdır. Metin türünde bir değişkene ekleyebilirsiniz **nvarchar** ve ardından değişkeni çağırın |
|*@input_data_1* | bir veri çerçevesi olarak SQL Server için veri döndüren ve R çalışma zamanının geçirilen sorgu tarafından döndürülen veriler |
|SONUÇ KÜMELERİ | yan tümcesi, SQL Server, "Hello World" sütun adı eklemek için döndürülen veriler tablonun şemasını tanımlar **int** veri türü için |

Komutu aşağıdaki metni çıkardığından:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Kullanım girişler ve çıkışlar

Varsayılan olarak, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) tek bir veri kümesi, geçerli bir SQL sorgusu biçiminde genellikle sağladığınız girdi olarak kabul eder. Ardından tek bir R veri çerçevesine çıktısı olarak döndürür.

Yalnızca bir giriş veri kümesi parametre olarak iletilebilir ve yalnızca bir veri kümesini döndürebilirsiniz. Ancak R kodunuzdan diğer tüm veri kümelerini çağırabilir ve veri kümesine ek olarak diğer türlerin çıkışlarını döndürebilirsiniz. Ayrıca OUTPUT anahtar sözcüğünü herhangi bir parametreye ekleyerek sonuçlarla birlikte döndürülmesini sağlayabilirsiniz.

Şimdilik şimdi varsayılan giriş ve çıkış değişkenleri [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql): **Inputdataset** ve **OutputDataSet**.

1. Test verilerinin küçük bir tablo oluşturun.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Kullanım `SELECT` tabloyu sorgulamak üzere deyimi.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Sonuçlar**

    ![RTestData tablosunun içeriği](./media/sql-database-connect-query-r/select-rtestdata.png)

1. Aşağıdaki R betiği çalıştırın. Tablo kullanarak veri alan `SELECT` deyimi, R çalışma geçirir ve verileri bir veri çerçevesi olarak döndürür. `WITH RESULT SETS` Yan tümcesi, döndürülen veriler tablonun şeması sütun adı ekleyerek SQL veritabanı için tanımlar *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Sonuçlar**

    ![Tablodan veri döndüren R betiğinin çıktısı](./media/sql-database-connect-query-r/r-output-rtestdata.png)

1. Artık giriş ve çıkış değişkenlerinin adları değiştirelim. Varsayılan giriş ve çıkış değişken adlarının **Inputdataset** ve **OutputDataSet**, bu komut adlarını değiştirdiğinde **SQL_in** ve **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    R büyük küçük harfe duyarlı olduğunu unutmayın. R betik kullanılan giriş ve çıkış değişkenleri (**SQL_out**, **SQL_in**) ile tanımlanan değerlerle eşleşen gerek `@input_data_1_name` ve `@output_data_1_name`, durumu dahil.

   > [!TIP]
   > Yalnızca bir giriş veri kümesi parametre olarak iletilebilir ve yalnızca bir veri kümesini döndürebilirsiniz. Ancak R kodunuzdan diğer tüm veri kümelerini çağırabilir ve veri kümesine ek olarak diğer türlerin çıkışlarını döndürebilirsiniz. Ayrıca OUTPUT anahtar sözcüğünü herhangi bir parametreye ekleyerek sonuçlarla birlikte döndürülmesini sağlayabilirsiniz.

1. Yalnızca giriş veri içermeyen R betiğini kullanarak değerleri de oluşturabilirsiniz (`@input_data_1` ayarlanır boş).

   Aşağıdaki komut, "hello" metni ve "world" çıkarır.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Sonuçlar**

    ![Giriş olarak @script kullanarak sonuçları sorgulama](./media/sql-database-connect-query-r/r-data-generated-output.png)

## <a name="check-r-version"></a>R sürümünü denetleme

SQL veritabanı'nda yüklü R sürümünü görmek istiyorsanız, aşağıdaki betiği çalıştırın.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

R `print` işlevi, sürümü **İletiler** penceresinde döndürür. Aşağıdaki örnek çıktıda, SQL veritabanı sürümü yüklü 3.4.4 R bu durumda olduğunu görebilirsiniz.

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

Microsoft, SQL veritabanınızda önceden Machine Learning Services ile birlikte yüklenmiş bir dizi R paketi sunar.

' In hangi R paketleri, sürüm, bağımlılıkları, lisans ve kitaplık yol bilgisi de dahil olmak üzere yüklü olan bir listesini görmek için aşağıdaki betiği çalıştırın.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

Çıkış dandır `installed.packages()` r ile ve sonuç olarak kümesi döndürülür.

**Sonuçlar**

![R içindeki yüklü paketler](./media/sql-database-connect-query-r/r-installed-packages.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL veritabanı'nda R kullanarak makine öğrenme modeli oluşturmak için bu hızlı başlangıcı takip:

> [!div class="nextstepaction"]
> [Oluşturma ve r ile Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) ile Tahmine dayalı bir model eğitip](sql-database-quickstart-r-train-score-model.md)

Machine Learning hizmetleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın. Bu makaleler bazıları için SQL Server olsa da, ilgili bilgilerin çoğunu da Machine Learning Hizmetleri (R ile) Azure SQL veritabanı için geçerlidir.

- [Azure SQL veritabanı Machine Learning Hizmetleri (R ile)](sql-database-machine-learning-services-overview.md)
- [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Öğretici: SQL Server'da R kullanarak veritabanında analizini öğrenin](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)
- [R ve SQL Server için uçtan uca veri bilimi kılavuzu](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough)
- [Öğretici: SQL Server verileri ile RevoScaleR R işlevlerini kullanma](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages)
