---
title: U-SQL betikleri Azure Data Lake Analytics Python ile genişletme
description: Python kodu kullanarak Azure Data Lake Analytics U-SQL betiklerini çalıştırma hakkında bilgi edinin
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: 0a49cbdb4caf474d0628fea3679ce712d37886e7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813407"
---
# <a name="extend-u-sql-scripts-with-python-code-in-azure-data-lake-analytics"></a>Python kodunda Azure Data Lake Analytics U-SQL betikleri genişletin

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Azure Data Lake Analytics hesabınızda Python uzantıları yüklü emin olun.

* Azure portalında, Data Lake Analytics hesabının gidin
* Soldaki menüde altında **BAŞLARKEN** tıklayarak **örnek betikler**
* Tıklayın **yükleyin U-SQL uzantıları** ardından **Tamam**

## <a name="overview"></a>Genel Bakış 

U-SQL Python uzantıları, geliştiricilerin yüksek düzeyde Paralel yürütme Python kod gerçekleştirmek sağlar. Aşağıdaki örnekte, temel adımlar gösterilmektedir:

* Kullanım `REFERENCE ASSEMBLY` Python uzantıları için U-SQL betiği etkinleştirme bildirimi
* Kullanarak `REDUCE` anahtarındaki giriş verileri bölümlere işlemi
* U-SQL Python uzantıları yerleşik Azaltıcı içerir (`Extension.Python.Reducer`), Python kod üzerinde çalıştığı için Azaltıcı atanan her köşe
* U-SQL betiği çağrılan bir işlev olan katıştırılmış Python kodu içerir `usqlml_main` bir pandas DataFrame girdi olarak kabul eder ve bir pandas DataFrame çıktı olarak verir.

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a>Python U-SQL ile nasıl tümleştirildiğini

### <a name="datatypes"></a>Veri türleri

* U-SQL dizesi ve sayısal sütunları olarak dönüştürülür-Panda ile U-SQL
* Pandas gelen ve U-SQL null değerlere dönüştürülür `NA` değerleri

### <a name="schemas"></a>Şemalar

* U-SQL dizin vektörleri Pandas içinde desteklenmez. Python işlevindeki tüm giriş veri çerçevelerini 64-bit sayısal dizin 0 eksi 1 satır sayısı ile her zaman vardır. 
* U-SQL veri kümelerinde yinelenen sütun adlarına sahip olamaz
* Dize olmayan U-SQL veri kümeleri sütun adları. 

### <a name="python-versions"></a>Python sürümleri
Yalnızca Python 3.5.1 (Windows için derlenmiş) desteklenir. 

### <a name="standard-python-modules"></a>Standart Python modüllerde
Tüm standart Python modüllerde dahil edilir.

### <a name="additional-python-modules"></a>Ek Python modüllerini
Standart Python kitaplıkları yanı sıra birçok yaygın olarak kullanılan python kitaplıkları dahil edilir:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Özel durum iletileri
Genel köşe hata olarak şu anda Python kodu bir özel durum gösterilir. Gelecekte, U-SQL işi hata iletileri Python özel durum iletisi görüntülenir.

### <a name="input-and-output-size-limitations"></a>Giriş ve çıkış boyutu sınırlamaları
Her köşe sınırlı miktarda bellek atandığını sahiptir. Şu anda bu sınırı AU için 6 GB ' dir. Python kodunu bellekte girdi ve çıktı veri çerçevelerini varolması gerektiğinden, giriş ve çıkış toplam boyutunu 6 GB aşamaz.

## <a name="see-also"></a>Ayrıca bkz.
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Data Lake Analytics işleri için U-SQL pencere işlevlerini kullanma](data-lake-analytics-use-window-functions.md)
* [Visual Studio Code için Azure Data Lake Araçları'nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
