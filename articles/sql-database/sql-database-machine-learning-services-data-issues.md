---
title: R ve SQL veri türleri ve nesneler ile çalışma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Veri türleri ve r ile Machine Learning, karşılaşabileceğiniz genel sorunları da dahil olmak üzere hizmetler (Önizleme) kullanarak Azure SQL veritabanı ile veri nesneleri ile çalışma hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 069a2a5b3b26bf517b57034f05ab7080ab392319
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014321"
---
# <a name="work-with-r-and-sql-data-in-azure-sql-database-machine-learning-services-preview"></a>R ve SQL Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) verileri ile çalışma

Bu makalede veri R ve SQL veritabanı'nda arasında taşırken karşılaşabileceği yaygın sorunlardan bazılarını ele alınmaktadır [Machine Learning Hizmetleri (R ile) Azure SQL veritabanı'nda](sql-database-machine-learning-services-overview.md). Bu alıştırmada elde deneyimi kendi betiğinizi verilerle çalışırken temel altyapıyı sağlar.

Karşılaşabileceğiniz genel sorunları şunlardır:

- Veri türleri bazen eşleşmiyor
- Örtük dönüştürmeleri yer alabilir
- Atama ve dönüştürme işlemlerini bazen gereklidir
- R ve SQL farklı veri nesneleri kullanma

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa, [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

- Bu alıştırmalarda örnek kodu çalıştırmak için öncelikle etkin bir Azure SQL veritabanı ile Machine Learning Hizmetleri (R) sahip olması gerekir. Genel Önizleme sırasında Microsoft tarafından ekleyin ve machine learning, mevcut veya yeni bir veritabanı için etkinleştirin. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

- En son yüklediğinizden emin olun [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Diğer veritabanı yönetim veya sorgu Araçları'nı kullanarak R betikleri çalıştırabilir ancak bu hızlı başlangıçta SSMS kullanacaksınız.

## <a name="working-with-a-data-frame"></a>Bir veri çerçevesi ile çalışma

Betiğinizi SQL R sonuçları döndürdüğünde, verileri olarak döndürmelidir bir **data.frame**. Saklı yordam sonuçlarının bir parçası çıktı istiyorsanız başka türde bir liste, faktör, vektör veya ikili veriler - olması gerekmediğini betiğinizde - oluşturan nesne için bir veri çerçevesi dönüştürülmesi gerekir. Neyse ki, bir veri çerçevesi için diğer nesneleri değiştirme desteklemek için birden fazla R işlevi vardır. Hatta, ikili bir modele serileştirir ve bu makalenin sonraki bölümlerinde gerçekleştireceğiniz bir veri çerçevesinde döndürür.

İlk olarak, şimdi bazı temel R nesneleriyle - vektörleri, matrislerde ve listeleri - deneme ve bir veri çerçevesi dönüştürme SQL geçirilen çıkışın nasıl değiştiğini bakın.

Bu iki r "Hello World" komut karşılaştırın Betikleri neredeyse aynı bakın, ancak tek bir ile üç sütun değeri her saniye döndürür ancak ilk üç değerlerinin tek bir sütun döndürür.

**Örnek 1**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
'
    , @input_data_1 = N'';
```

**Örnek 2**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
    , @input_data_1 = N'';
```

Neden sonuç çok farklı?

Yanıt genellikle R kullanarak bulunabilir `str()` komutu. İşlev ekleme `str(object_name)` herhangi bir veri için R betiğinizde belirtilen R nesnenin şema döndürülen amaçlı bir iletidir. İletileri görüntüleyebileceğiniz **iletileri** SSMS sekmesindedir.

Neden örnek 1 ve örnek 2 gibi farklı sonuçlar olduğunu anlamak için satır ekler `str(OutputDataSet)` sonunda `@script` böyle her bir deyimde değişken tanımı:

**Örnek 1 str işleviyle eklendi**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

**Örnek 2 str işleviyle eklendi**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(c("hello"), " ", c("world"));
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

Şimdi, metni gözden **iletileri** çıkış neden farklı olduğunu görmek için.

**Sonuçları - örnek 1**

```text
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Sonuçları - örnek 2**

```text
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Gördüğünüz gibi küçük bir değişiklik R sözdiziminde sonuçlarının şema üzerinde büyük bir etkiye vardı. Tüm Ayrıntılar için Ayrıntılar R veri türleri arasındaki farklılıklar açıklanmıştır *veri yapılarını* konusundaki ["Gelişmiş" R"Hadley Wickham tarafından](http://adv-r.had.co.nz).

Şu an için yalnızca R nesneleri veri çerçevelerine zorlama beklenen sonuçları denetimi gerektiğini unutmayın.

> [!TIP]
> R kimlik işlevleri gibi kullanabilir `is.matrix`, `is.vector`, iç veri yapısı hakkında bilgi döndürmek için.

## <a name="implicit-conversion-of-data-objects"></a>Veri nesneleri örtük dönüştürme

Her R veri nesnesi, iki veri nesnelerini boyutları aynı sayıda varsa diğer veri nesneleriyle birlikte kullanıldığında ya da herhangi bir veri nesnesi heterojen veri türleri içeriyorsa değerlerin nasıl işleneceğini için kendi kurallarına sahiptir.

Örneğin, r kullanarak matris çarpım gerçekleştirmek istediğinizi varsayalım. Dört değer olan bir dizi üç değerleri içeren tek sütunlu matris çarpmak ve sonuç olarak 4 x 3 matrisi beklediğiniz istiyorsunuz.

İlk olarak, test verilerinin küçük bir tablo oluşturun.

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

Artık aşağıdaki betiği çalıştırın.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:15);
OutputDataSet <- as.data.frame(x %*% y);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col1] INT
            , [Col2] INT
            , [Col3] INT
            , Col4 INT
            ));
```

Perde üç değerleri sütunu bir tek sütunlu matrise dönüştürülür. Matris bir özel durum R, dizi içinde dizi olduğundan `y` örtük olarak iki bağımsız değişkeni uygun hale getirmek için bir tek sütunlu matris durumunda bırakılması.

**Sonuçlar**

|Sütun1|Sütun2|Col3|Sütun4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1500|

Ancak, dizinin boyutu değiştirdiğinizde ne olacağını unutmayın `y`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:14);
OutputDataSet <- as.data.frame(y %*% x);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS(([Col1] INT));
```

Artık R sonucu olarak tek bir değer döndürür.

**Sonuçlar**
    
|Sütun1|
|---|
|1542|

Neden? Bu durumda, iki bağımsız değişken aynı uzunlukta vektörleri işlenebilmesini mümkün olduğundan, R iç ürün bir matris döndürür.  Beklenen davranış kurallarına göre doğrusal Cebir budur. Ancak, bir aşağı akış uygulaması hiçbir zaman değiştirmek için çıkış şeması bekliyorsa sorunlara neden olabilir!

## <a name="merge-or-multiply-columns-of-different-length"></a>Birleştirme ya da farklı uzunlukta sütunu Çarp

R, farklı boyutlardaki vektörleri ile çalışmak için ve bu sütun benzeri yapılar veri çerçevelerine birleştirmek için büyük esneklik sağlar. Vektör listesi gibi bir tabloya bakabilirsiniz, ancak bunlar veritabanı tabloları yöneten tüm kurallar izlemeyin.

Örneğin, aşağıdaki betik uzunluğu 6 sayısal bir dizisi tanımlanmaktadır ve R değişkeninde depolar `df1`. Sayısal dizi sonra tam sayılar (yukarıda oluşturulan) RTestData tablo ile birleştirilmiş yeni bir veri çerçevesi yapmak için üç (3) değerleri içeren `df2`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
df1 <- as.data.frame( array(1:6) );
df2 <- as.data.frame( c( InputDataSet , df1 ));
OutputDataSet <- df2;
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col2] INT NOT NULL
            , [Col3] INT NOT NULL
            ));
```

Veri çerçevesi doldurmak için öğelerin sayısı dizideki öğelerin sayısını uyacak şekilde kadar RTestData alınan R yinelenen `df1`.

**Sonuçlar**
    
|*Sütun2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Bir veri çerçevesi bir tablo gibi görünüyor, ancak vektörleri aslında bir listesi olduğunu unutmayın.

## <a name="cast-or-convert-sql-data"></a>Cast veya convert SQL verileri

Aynı veri türleri, R ve SQL kullanmayın, SQL veri almak ve daha sonra R çalışma zamanına geçirmek için bir sorgu çalıştırdığınızda, bazı tür için örtük dönüştürme genellikle yer alır. SQL R veri döndüğünüzde başka bir dizi dönüştürmeler gerçekleştirilir.

- SQL, R işleme sorgudan verileri gönderir ve daha fazla verimlilik için bir iç temsiline dönüştürür.
- R çalışma data.frame değişkene verileri yükler ve verilere kendi işlemleri gerçekleştirir.
- Veritabanı altyapısı, verileri güvenli bir iç bağlantı kullanarak SQL'e getirir ve verileri SQL veri türleri açısından sunar.
- Veriler SQL bağlanarak SQL sorguları göndermek ve sekmeli veri kümelerini işleme bir istemci ya da ağ kitaplığı kullanarak sahip olursunuz. Bu istemci uygulaması, potansiyel olarak verilerini farklı yollarla etkileyebilir.

Nasıl çalıştığını görmek için şunun gibi bir sorgu çalıştırın [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) veri ambarı. Bu görünüm tahminlerini oluşturmak için kullanılan satış verilerini döndürür.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
    , CAST(ModelRegion AS VARCHAR(50)) AS ProductSeries
    , Amount
FROM [AdventureWorksDW].[dbo].[vTimeSeries]
WHERE [ModelRegion] = 'M200 Europe'
ORDER BY ReportingDate ASC
```

> [!NOTE]
> AdventureWorks herhangi bir sürümünü kullanın veya farklı bir sorgu kullanarak kendinize ait bir veritabanı oluşturun. Metin, tarih/saat ve sayısal değerleri içeren bazı verileri işlemek için kullanılan noktasıdır.

Artık, saklı yordam için giriş olarak bu sorguyu kullanmayı deneyin.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
str(InputDataSet);
OutputDataSet <- InputDataSet;
'
    , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Bir hata alırsanız, sorgu metnini bazı düzenlemeler gerekecektir. Örneğin, dize çağrıldıysa WHERE yan tümcesinde iki tek tırnak işareti tarafından alınmalıdır.

Çalışma sorgu aldıktan sonra sonuçları gözden `str` R giriş verileri nasıl işler görmek için işlev.

**Sonuçlar**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- R veri türünü kullanarak bir tarih saat sütunlarında işlendikten **POSIXct**.
- Metin sütununu "ProductSeries" olarak tanımlanmıştı bir **faktörü**, Kategorik bir değişken anlamına gelir. Dize değerleri varsayılan olarak faktörleri ele alınır. R için bir dize geçirirseniz, iç kullanım için bir tamsayıya dönüştürülür ve ardından çıkış dizesine geri eşlenir.

## <a name="summary"></a>Özet

Kısa bile bu örneklerden SQL geçirme giriş olarak sorguladığında veri dönüştürme etkilerini kontrol etmek için gereken görebilirsiniz. Bazı SQL veri türleri R tarafından desteklenmediğinden, hataları önlemek için bu yolları göz önünde bulundurun:

- Verilerinizi önceden test etmek ve sütunlara veya değerlere R kodunu geçirildiğinde bir sorun, şemada doğrulayın.
- Giriş veri kaynağınız sütunlarda ayrı ayrı kullanmak yerine belirttiğiniz `SELECT *`ve her bir sütunun nasıl işleneceğini bildirin.
- İstenmeyen sürprizleri önleyin, girdi verilerini, hazırlanırken açık dönüştürmeleri gerekli olarak gerçekleştirin.
- Hatalara neden ve modelleme için yararlı olmayan verileri (örneğin, GUID'leri veya güncelleştiremezsiniz) geçirme sütunlarının kaçının.

Desteklenen ve desteklenmeyen R veri türleri hakkında daha fazla bilgi için bkz. [R kitaplıkları ve veri türleri](/sql/advanced-analytics/r/r-libraries-and-data-types.md).
