---
title: Apache Pig - Azure HDInsight kullanma
description: HDInsight üzerinde Apache Hadoop ile pig kullanma konusunda bilgi edinin.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.openlocfilehash: 2b0f3a4394267a808b6e6e0a73abfaf1e698ff1c
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445033"
---
# <a name="use-apache-pig-with-apache-hadoop-on-hdinsight"></a>HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma

Nasıl kullanacağınızı öğrenin [Apache Pig](https://pig.apache.org/) HDInsight ile.

Apache Pig olarak bilinen bir yordam dilini kullanarak Apache Hadoop için programlar oluşturmak için bir platform olan *Pig Latin*. Pig oluşturmak için Java alternatif olan *MapReduce* çözümleri ve bu, Azure HDInsight ile birlikte. HDInsight ile Pig çeşitli şekillerde kullanılabilir bulmak için aşağıdaki tabloyu kullanın:

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, UNIX, Mac OS X veya Windows |
| [REST API](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux veya Windows |Windows (şimdilik ile) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux veya Windows |Windows |

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Neden Apache Pig kullanma

Yalnızca bir eşleme ve azaltma işlevi kullanarak Hadoop MapReduce kullanarak veri işleme sorunlarından biri, işleme mantığı uygulamaya koyuyor. Karmaşık bir işlem için genellikle işleme Zincirli birden fazla MapReduce işlemi kesmek zorunda birlikte istenen sonucu elde etmek için.

Pig işleme istenen çıkış oluşturmak için aracılığıyla veri akışları dönüştürmeler bir dizi olarak tanımlamanızı sağlar.

Pig Latin'i dil ham girişini, istenen çıkış oluşturmak için bir veya daha fazla dönüştürmeleri, veri akışından açıklamak sağlar. Pig Latin'i programlar bu genel bir desene uyar:

* **Yük**: Dosya sisteminden yönetilebilmesini verileri okuyamadı.

* **Dönüştürme**: Veri işleme.

* **Döküm veya mağaza**: Çıktı verilerini ekranına veya işleme için depolayın.

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Pig Latin'i kullanıcı tanımlı işlevler (UDF), Pig Latin modelinde zordur mantığını dış bileşenleri çağırma olanak tanıyan da destekler.

Pig Latin'i hakkında daha fazla bilgi için bkz: [Pig Latin başvuru el ile 1](https://archive.cloudera.com/cdh/3/pig/piglatin_ref1.html) ve [Pig Latin başvuru el ile 2](https://archive.cloudera.com/cdh/3/pig/piglatin_ref2.html).

UDF ile Pig kullanma örneği için aşağıdaki belgelere bakın:

* [HDInsight, Apache Pig ile DataFu Apache kullanma](apache-hadoop-use-pig-datafu-udf.md) -DataFu olan Apache tarafından tutulan yararlı UDF'ler koleksiyonu
* [Apache Pig ve Apache Hive, HDInsight ile Python kullanma](python-udf-hdinsight.md)
* [Kullanım C# Apache Hive ve Apache Pig, HDInsight ile](apache-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Örnek veri

HDInsight sağlar içinde depolanan çeşitli örnek veri kümeleri, `/example/data` ve `/HdiSamples` dizinleri. Bu dizinler, kümenin varsayılan depolama içindedir. Bu belgedeki Pig örnekte *log4j* dosya `/example/data/sample.log`.

Her günlük dosyası içindeki içeren bir dizi alanlarının oluşur bir `[LOG LEVEL]` alanı türünün ve önem örneğin göstermek için:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Önceki örnekte, günlük düzeyi hatadır.

> [!NOTE]  
> Log4j dosyasını kullanarak da oluşturabilirsiniz [Apache Log4j](https://en.wikipedia.org/wiki/Log4j) aracı günlüğe kaydetme ve ardından blobunuza bu dosyayı karşıya yükleyin. Bkz: [HDInsight verileri karşıya yükleme](../hdinsight-upload-data.md) yönergeler için. Azure Depolama'daki blobları HDInsight ile nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Blob Depolama kullanma](../hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Örnek Proje

Aşağıdaki Pig Latin iş yükler `sample.log` HDInsight kümeniz için varsayılan depolama biriminden dosya. Ardından bir dizi kaç kez giriş verilerini her günlük düzeyi oluştuğu sayısı neden dönüşümleri gerçekleştirir. Sonuçları STDOUT yazılır.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Aşağıdaki görüntüde verilere ne yaptığını her dönüştürme bir özeti gösterilir.

![Grafik gösterimi dönüştürmeleri][image-hdi-pig-data-transformation]

## <a id="run"></a>Pig Latin'i işini çalıştır

HDInsight Pig Latin işleri çeşitli yöntemler kullanarak çalıştırabilirsiniz. Hangi yöntemin size uygun olduğuna karar vermek için aşağıdaki tabloyu kullanın ve ardından bir kılavuz için bağlantıyı izleyin.

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ....from bu **istemci** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, UNIX, Mac OS X veya Windows |
| [Curl](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux veya Windows |Windows (şimdilik ile) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux veya Windows |Windows |

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="pig-and-sql-server-integration-services"></a>Pig ve SQL Server Integration Services

SQL Server Integration Services (SSIS), Pig işi çalıştırmak için kullanabilirsiniz. SSIS için Azure Feature Pack üzerinde HDInsight ile Pig işleri çalışan aşağıdaki bileşenleri sağlar.

* [Azure HDInsight Pig görevi][pigtask]

* [Azure aboneliği Bağlantı Yöneticisi][connectionmanager]

SSIS için Azure Feature Pack hakkında daha fazla bilgi edinin [burada][ssispack].

## <a id="nextsteps"></a>Sonraki adımlar
HDInsight ile Pig kullanma gerçekleştirmeyi öğrendiniz, Azure HDInsight ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [HDInsight için karşıya veri yükleme](../hdinsight-upload-data.md)
* [Apache Hive, HDInsight ile kullanma][hdinsight-use-hive]
* [HDInsight ile Apache Sqoop'u kullanma](hdinsight-use-sqoop.md)
* [HDInsight ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md)
* [HDInsight ile MapReduce işleri kullanma][hdinsight-use-mapreduce]

[apachepig-home]: https://pig.apache.org/
[putty]: https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: https://curl.haxx.se/
[pigtask]: https://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: https://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: https://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
