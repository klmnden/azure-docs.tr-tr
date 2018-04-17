---
title: Apache Hive bir ETL aracı - Azure Hdınsight kullanma | Microsoft Docs
description: Ayıklamak, dönüştürme ve yükleme (ETL) verileri Azure hdınsight'ta Apache Hive'ı kullanın.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 06e06d87abd66c80deb2c8731f68bb8171da574b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-apache-hive-as-an-extract-transform-and-load-etl-tool"></a>Apache Hive bir çıkartma, dönüştürme ve yükleme (ETL) aracı olarak kullanın

Genelde temizlemek ve gelen veri analizi için uygun bir hedef içine yüklemeden önce dönüştürmek gerekir. Ayıklama, dönüştürme ve yükleme (ETL) işlemlerini verileri hazırlamak ve verileri hedef yüklemek için kullanılır.  Hdınsight'ta Hive yapılandırılmamış verileri okuyun, gerektiğinde verileri işlemek ve karar destek sistemleri için bir ilişkisel veri ambarında verileri yükleme. Bu yaklaşımda, veri kaynağından ayıklanan ve Azure Storage bloblarında veya Azure Data Lake Store gibi ölçeklenebilir depolama alanında depolanır. Veriler, ardından Hive sorguları bir dizi kullanılarak dönüştürülen ve toplu hedef veri deposuna yükleme için hazırlık Hive içinde son hazırlanır.

## <a name="use-case-and-model-overview"></a>Büyük/küçük harf ve model genel bakış kullanın

Aşağıdaki şekil ETL Otomasyon için model ve kullanım durumu özetini gösterir. Uygun çıktı üretmek için giriş verileri dönüştürüldüğünde.  Bu dönüştürme sırasında veri şekli, veri türü ve hatta dil değiştirebilirsiniz.  ETL işlemleri için ölçüm Imperial dönüştürmek, saat dilimleri değiştirin ve hedef var olan verilerle düzgün hizalamak için duyarlık artırmak.  ETL işlemleri ayrıca güncel raporlama saklamak veya daha fazla var olan verileri bir anlayış sağlamak için mevcut verileri yeni verilerle birleştirebilirsiniz.  Raporlama araçları ve Hizmetleri gibi uygulamalar, ardından bu verileri istediğiniz biçimde kullanabilir.

![Apache Hive ETL olarak](./media/apache-hadoop-using-apache-hive-as-an-etl-tool/hdinsight-etl-architecture.png)

Hadoop genellikle metin dosyaları (örneğin, csv) veya daha küçük ancak metin dosyası veya her ikisi de sayısı sık değişen yoğun numarasına içeri ETL işlemlerde kullanılır.  Hive veri hedefe yüklemeden önce verileri hazırlamak için kullanmak için harika bir araçtır.  Hive, CSV üzerinde bir şema oluşturun ve veri ile etkileşim kuran MapReduce programlar oluşturmak için SQL benzeri bir dil kullanın olanak sağlar. 

Hive kullanarak ETL gerçekleştirmek için tipik adımları aşağıdaki gibidir:

1. Verileri Azure Data Lake Store veya Azure Blob Storage yükleyin.
2. Şemalar Depolama yığını tarafından kullanılmak üzere bir meta veri deposu veritabanı (Azure SQL veritabanı kullanarak) oluşturun.
3. Hdınsight kümesi oluşturmak ve veri deposu bağlanın.
4. Veri deposunda veriler üzerinde okuma-aynı anda uygulamak için şema tanımlayın:

    ```
    DROP TABLE IF EXISTS hvac;

    --create the hvac table on comma-separated sensor data stored in Azure Storage blobs
    
    CREATE EXTERNAL TABLE hvac(`date` STRING, time STRING, targettemp BIGINT,
        actualtemp BIGINT, 
        system BIGINT, 
        systemage BIGINT, 
        buildingid BIGINT)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    STORED AS TEXTFILE LOCATION 'wasb://{container}@{storageaccount}.blob.core.windows.net/HdiSamples/SensorSampleData/hvac/';
    ```

5. Verileri dönüştürmek ve hedefe yükleyin.  Hive dönüştürme ve yükleme sırasında kullanmak için birkaç yol vardır:

    * Sorgulamak ve Hive kullanarak verileri hazırlamak ve Azure Data Lake Store veya Azure blob depolamada CSV olarak kaydedin.  Ardından bu csv edinmeli ve bir hedef ilişkisel veritabanı SQL Server gibi içine verileri yüklemek için SQL Server Integration Services (SSIS) gibi bir araç kullanın.
    * Verileri doğrudan Excel veya Hive ODBC sürücüsünü kullanarak C# sorgusu.
    * Kullanım [Apache Sqoop](apache-hadoop-use-sqoop-mac-linux.md) hazırlıklı düz CSV dosyalarını okuması ve bunları hedef ilişkisel veritabanına yükleyin.

## <a name="data-sources"></a>Veri kaynakları

Veri kaynakları, veri deposunda mevcut verilere örneğin eşleştirilmesi genellikle dış veri şunlardır:

* Sosyal medya veri, günlük dosyaları, algılayıcılar ve veri dosyalarını oluşturmak uygulamalar.
* Veri kümeleri hava durumu istatistikleri veya satıcı gibi veri sağlayıcılardan satış sayıları elde.
* Veri akış yakalanan, filtre ve uygun aracı veya framework işlenebilir.

<!-- TODO: (see Collecting and loading data into HDInsight). -->

## <a name="output-targets"></a>Çıktı hedefleri

Hive hedefleri de dahil olmak üzere çeşitli çıktı verileri için kullanabilirsiniz:

* SQL Server veya Azure SQL veritabanı gibi ilişkisel bir veritabanı.
* Azure SQL veri ambarı gibi bir veri ambarı.
* Excel.
* Azure tablo ve blob depolama.
* Uygulamalar veya hizmetler belirli biçimlere işlenecek verileri gerektiren ya da dosyalar gibi belirli türde bilgileri yapısı içerir.
* Bir JSON belgesi depolama ister <a href="https://azure.microsoft.com/services/cosmos-db/">CosmosDB</a>.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdakileri yapmak istediğinizde ETL modeli genellikle kullanılır:

* Veri akışı veya yarı yapılandırılmış veya yapılandırılmamış veriler büyük miktarda dış kaynaklardan varolan bir veritabanına veya bilgi sistemi yükleyin.
* Temizleme, dönüştürme ve yüklemeden önce veri doğrulama, belki de birden fazla dönüşüm kullanarak küme boyunca geçirin.
* Raporlar ve düzenli olarak güncelleştirilen görselleştirmeleri oluşturur.  Örneğin, raporun gün boyunca oluşturmak için çok uzun sürerse raporunu gece çalışacak şekilde zamanlayabilirsiniz.  Otomatik olarak bir Hive sorgusu çalıştırmak için PowerShell ve Azure Scheduler'ı kullanabilirsiniz.

Hedef veri için bir veritabanı değilse, sorgu, örneğin bir CSV içinde uygun biçimdeki bir dosya oluşturabilirsiniz. Bu dosya daha sonra Excel veya Power BI aktarılabilir.

Veriler üzerinde çeşitli işlemler ETL işleminin bir parçası çalıştırmak gerekiyorsa, yönettiğiniz nasıl göz önünde bulundurun. İşlem harici bir program tarafından denetleniyorsa, yerine çözüm içinde iş akışı olarak, bazı işlemler paralel olarak gerçekleştirilip gerçekleştirilmediğini karar verin ve her bir iş tamamlandığında algılamak için gerekir. Hadoop içinde Oozie gibi bir iş akışı mekanizmasını kullanarak dış komut dosyaları veya özel programları kullanarak işlem sırasını düzenlemek çalışırken daha kolay olabilir. Oozie hakkında daha fazla bilgi için bkz: [iş akışı ve iş orchestration](https://msdn.microsoft.com/library/dn749829.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* [ETL ölçekte](apache-hadoop-etl-at-scale.md)
* [Veri ardışık faaliyete](../hdinsight-operationalize-data-pipeline.md)
<!-- * [ETL Deep Dive](../hdinsight-etl-deep-dive.md) -->
