---
title: Gelişmiş R işlevler yazma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Machine Learning Hizmetleri (Önizleme) kullanarak Azure SQL veritabanı içinde Gelişmiş istatistiksel işlemler bir R işlevi yazmayı öğrenin.
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
ms.openlocfilehash: 939798d5d9eb2843d7bbbbe74680342e4ce6ce95
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014784"
---
# <a name="write-advanced-r-functions-in-azure-sql-database-using-machine-learning-services-preview"></a>Machine Learning Hizmetleri (Önizleme) kullanarak Azure SQL veritabanı içinde Gelişmiş R işlevler yazma

Bu makalede R matematik ekleme ve yardımcı program işlevleri bir SQL saklı yordamı. T-SQL uygulamak karmaşık Gelişmiş istatistiksel işlevler R, yalnızca bir tek satır kod ile yapılabilir.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa, [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

- Bu alıştırmalarda örnek kodu çalıştırmak için öncelikle etkin bir Azure SQL veritabanı ile Machine Learning Hizmetleri (R) sahip olması gerekir. Genel Önizleme sırasında Microsoft tarafından ekleyin ve machine learning, mevcut veya yeni bir veritabanı için etkinleştirin. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

- En son yüklediğinizden emin olun [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Diğer veritabanı yönetim veya sorgu Araçları'nı kullanarak R betikleri çalıştırabilir ancak bu hızlı başlangıçta SSMS kullanacaksınız.

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Rastgele sayılar üretmeyi, bir saklı yordam oluşturma

Kolaylık olması için R kullanalım `stats` yüklü ve Machine Learning Hizmetleri (Önizleme) kullanarak Azure SQL veritabanı ile varsayılan olarak yüklenen bir paket. Pakette işlevleri aralarında istatistiksel ortak görevler için yüzlerce `rnorm` işlevi. Bu işlev, belirtilen sayıda verilen standart sapma ve anlamına gelir normal dağıtım kullanarak rastgele sayılar oluşturur.

Örneğin, aşağıdaki R kodu 50, 3 standart sapmasını verilen bir mean 100 sayıları döndürür.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Bu satırı R, T-SQL'den çağırmak için çalıştırma `sp_execute_external_script` ve R betiği parametresinde böyle R işlevi ekleyin:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Peki rastgele sayılar farklı bir dizi oluşturmak daha kolay hale getirmek istersiniz?

SQL ile birleştirildiğinde kolay olmasıdır. Kullanıcıdan bağımsız değişkenleri alır, ardından bu bağımsız değişkenleri olarak R betiğe geçirmek bir saklı yordam tanımlarsınız.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- İlk satırı, her biri saklı yordam yürütme sırasında gerekli olan SQL giriş parametrelerini tanımlar.

- İle satır başına `@params` R kodunu ve karşılık gelen SQL veri türleri tarafından kullanılan tüm değişkenleri tanımlar.

- Hemen izleyin satırları SQL parametre adları için ilgili R değişken adları eşleyin.

Saklı yordam, R işlevi sarmalanmış, kolayca işlevini çağırın ve bunun gibi farklı değerler geçirin:

```sql
EXECUTE MyRNorm @param1 = 100
    , @param2 = 50
    , @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Sorun giderme için R yardımcı işlevleri kullanın.

`utils` Paket, varsayılan olarak yüklenen çeşitli geçerli R ortamı araştırma için yardımcı işlevleri sağlar. Bu işlevler, SQL ve dış ortamlarda R kodunuzu gerçekleştirir şekilde tutarsızlıklar bulma durumlarda yararlı olabilir. Örneğin, R kullanabilirsiniz `memory.limit()` geçerli R ortam için bellek almak için işlevi.

Çünkü `utils` paket yüklü ancak varsayılan olarak yüklü değil, kullanmanız gereken `library()` önce yüklemek için işlevi.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
library(utils);
mymemory <- memory.limit();
OutputDataSet <- as.data.frame(mymemory);
'
    , @input_data_1 = N' ;'
WITH RESULT SETS(([Col1] INT NOT NULL));
```

> [!TIP]
> Çok sayıda kullanıcı, sistem zamanlama işlevleri gibi R kullanmak ister `system.time` ve `proc.time`, R işlemler tarafından kullanılan zaman yakalamak ve performans sorunlarını analiz edin.
