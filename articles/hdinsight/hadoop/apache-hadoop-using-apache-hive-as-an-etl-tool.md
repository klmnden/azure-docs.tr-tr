---
title: Apache Hive bir ETL aracı - Azure HDInsight kullanma
description: Ayıklama, dönüştürme ve yükleme (ETL) verileri Azure HDInsight için Apache Hive'ı kullanın.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.openlocfilehash: f8fb036eaca35e41d89b0a9610ebcd68e65f40f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60343379"
---
# <a name="use-apache-hive-as-an-extract-transform-and-load-etl-tool"></a>Apache Hive ayıklama, dönüştürme ve yükleme (ETL) aracı olarak kullanma

Genelde temizlemek ve analiz için uygun bir hedef içine yüklemeden önce gelen verileri dönüştürmek gerekir. Ayıklama, dönüştürme ve yükleme (ETL) işlemleri, veri hazırlama ve veri hedefe yüklemek için kullanılır.  HDInsight üzerinde Apache Hive yapılandırılmamış verileri okuma, gerektiği gibi verileri işlemek ve ardından karar destek sistemleri için bir ilişkisel veri ambarı'na veri yükleme. Bu yaklaşımda, veri kaynağından ayıklanan ve ölçeklenebilir depolama, Azure depolama blobları veya Azure Data Lake Storage gibi depolanır. Veriler ardından Hive sorguları bir dizi kullanarak dönüştürülür ve içinde Hive toplu hedef veri deposuna yükleme hazırlığı kapsamında son hazırlanır.

## <a name="use-case-and-model-overview"></a>Büyük/küçük harf ve model genel bakış kullanın

Kullanım örneği ve ETL otomasyon modeli genel bakış aşağıdaki şekilde gösterilmektedir. Giriş verileri, uygun çıktı üretmek için dönüştürülür.  Bu dönüştürme sırasında veri şekli, veri türü ve hatta dil değiştirebilirsiniz.  ETL işlemleri Imperial dönüştürmek için ölçüm, saat dilimini değiştirme ve duyarlık hedef mevcut verilerle düzgün hizalamak için geliştirin.  ETL işlemleri ayrıca raporlama güncel tutmak için ya da daha fazla var olan verileri bir anlayış sağlamak için mevcut verileri yeni verilerle birleştirebilirsiniz.  Raporlama araçları ve Hizmetleri gibi uygulamaları, ardından bu verileri istediğiniz biçimde kullanabilir.

![Apache Hive ETL olarak](./media/apache-hadoop-using-apache-hive-as-an-etl-tool/hdinsight-etl-architecture.png)

Hadoop genellikle metin dosyaları (örneğin, csv) veya daha küçük ancak sayı metin dosyası ya da her ikisini de sık değişen büyük sayı alma ETL işlemlerinde kullanılır.  Hive veri hedefe yüklemeden önce verileri hazırlamak için kullanmak için harika bir araçtır.  Hive, bir şema üzerinde CSV oluşturun ve verilerle etkileşimde bulunmak MapReduce programlarını oluşturmak için SQL benzeri bir dil kullanmanıza olanak sağlar. 

Hive'ı kullanarak ETL gerçekleştirmeyi için tipik adımları aşağıdaki gibidir:

1. Verileri Azure Data Lake Storage veya Azure Blob Depolama yükleyin.
2. Kendi şemaları depolanırken Hive tarafından kullanılmak üzere (Azure SQL veritabanı kullanarak) bir meta veri Store veritabanı oluşturun.
3. Bir HDInsight kümesi oluşturma ve veri deposuna bağlanın.
4. Veri deposundaki veriler üzerinde okuma zamanında uygulamak için bir şema tanımlayın:

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

5. Veri dönüştürme ve hedefe yükle.  Hive dönüştürme ve yükleme sırasında kullanmak için birkaç yol vardır:

    * Sorgu, Hive kullanarak verileri hazırlama ve Azure Data Lake Storage veya Azure blob Depolama'nde bir CSV olarak kaydedin.  Ardından bu Csv'leri almak ve verileri SQL Server gibi ilişkisel bir hedef veritabanı yüklemek için SQL Server Integration Services (SSIS) gibi bir araç kullanın.
    * Verileri doğrudan Excel veya Hive ODBC sürücüsünü kullanarak C# sorgu.
    * Kullanım [Apache Sqoop](apache-hadoop-use-sqoop-mac-linux.md) hazırlanmış düz CSV dosyaları okumak ve bunları hedef ilişkisel veritabanı'na yükler.

## <a name="data-sources"></a>Veri kaynakları

Veri kaynakları, veri deposundaki mevcut veriler için örneğin eşleştirilebildiği genellikle dış veri şunlardır:

* Sosyal medya veri, günlük dosyaları, algılayıcılar ve veri dosyaları oluşturan uygulamalar.
* Veri kümeleri, hava durumu istatistikleri veya satıcı gibi veri sağlayıcıları satış rakamları elde.
* Veri akış yakalanan, filtre ve uygun aracı veya çerçeve işlenir.

<!-- TODO: (see Collecting and loading data into HDInsight). -->

## <a name="output-targets"></a>Çıkış hedefleri

Hedefler dahil olmak üzere çeşitli veri çıkışı Hive'ı kullanabilirsiniz:

* SQL Server veya Azure SQL veritabanı gibi ilişkisel bir veritabanı.
* Azure SQL veri ambarı gibi bir veri ambarı.
* Excel.
* Azure tablosu ve blob depolama.
* Belirli türlerdeki bilgi yapısı uygulamalar veya belirli biçimlere işlenecek verileri gerektiren ya da dosyalar gibi hizmetler içerir.
* Bir JSON belge Store ister <a href="https://azure.microsoft.com/services/cosmos-db/">CosmosDB</a>.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

ETL model, genellikle, istediğinizde kullanılır:

* Veri akışı veya büyük hacimli yarı yapılandırılmış veya yapılandırılmamış veriler dış kaynaklardan varolan bir veritabanına veya bilgi sistemi yükleyin.
* Temizleme, dönüştürme ve yüklemeden önce verileri doğrulamak, belki de aracılığıyla küme birden fazla dönüştürme kullanarak geçirin.
* Raporlar ve düzenli olarak güncelleştirilen görselleştirmeler oluşturun.  Örneğin, raporda günde oluşturmak için çok uzun sürerse geceleri çalıştırmak üzere rapor zamanlayabilirsiniz.  Otomatik olarak bir Hive sorgusu çalıştırmak için Azure Zamanlayıcı'yı ve PowerShell'i kullanabilirsiniz.

Hedef veriler için bir veritabanı değilse sorgusu, örneğin bir CSV içinde uygun biçimdeki bir dosya oluşturabilir. Bu dosyayı Excel veya Power BI aktarılabilir.

ETL işleminin bir parçası veriler üzerinde çeşitli işlemleri yürütmek gerekiyorsa, yönettiğiniz nasıl göz önünde bulundurun. İşlemleri dış bir program tarafından denetleniyorsa, yerine çözüm içinde iş akışı olarak, bazı işlemler paralel olarak yürütülüp yürütülemeyeceğini karar verin ve her bir iş tamamlandığında algılamak için gerekir. Hadoop içinde Oozie gibi bir iş akışı mekanizması kullanarak bir dizi işlem özel program veya dış betikler kullanarak düzenlemek çalışmaktan daha kolay olabilir. Oozie hakkında daha fazla bilgi için bkz: [iş akışı ve iş düzenleme](https://msdn.microsoft.com/library/dn749829.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* [Uygun ölçekte ETL](apache-hadoop-etl-at-scale.md)
* [Veri işlem hattı kullanıma hazır hale getirme](../hdinsight-operationalize-data-pipeline.md)

<!-- * [ETL Deep Dive](../hdinsight-etl-deep-dive.md) -->
