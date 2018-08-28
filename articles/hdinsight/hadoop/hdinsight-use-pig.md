---
title: HDInsight, Hadoop Pig kullanma
description: HDInsight üzerinde Hadoop ile pig kullanma konusunda bilgi edinin.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.openlocfilehash: e97763adbe7998ed93e3ba8b87d89ffe8d8de6aa
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045454"
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>HDInsight üzerinde Hadoop ile Pig kullanma

Nasıl kullanacağınızı öğrenin [Apache Pig](http://pig.apache.org/) HDInsight ile.

Pig olarak bilinen bir yordam dilini kullanarak Hadoop için programlar oluşturmak için bir platform olan *Pig Latin*. Pig oluşturmak için Java alternatif olan *MapReduce* çözümleri ve bu, Azure HDInsight ile birlikte. HDInsight ile Pig çeşitli şekillerde kullanılabilir bulmak için aşağıdaki tabloyu kullanın:

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, UNIX, Mac OS X veya Windows |
| [REST API](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux veya Windows |Windows (şimdilik ile) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux veya Windows |Windows |

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Neden Pig kullanma

Yalnızca bir eşleme ve azaltma işlevi kullanarak Hadoop MapReduce kullanarak veri işleme sorunlarından biri, işleme mantığı uygulamaya koyuyor. Karmaşık bir işlem için genellikle işleme Zincirli birden fazla MapReduce işlemi kesmek zorunda birlikte istenen sonucu elde etmek için.

Pig işleme istenen çıkış oluşturmak için aracılığıyla veri akışları dönüştürmeler bir dizi olarak tanımlamanızı sağlar.

Pig Latin'i dil ham girişini, istenen çıkış oluşturmak için bir veya daha fazla dönüştürmeleri, veri akışından açıklamak sağlar. Pig Latin'i programlar bu genel bir desene uyar:

* **Yük**: dosya sisteminden yönetilebilmesini veri okuma

* **Dönüştürme**: verileri düzenleme

* **Döküm veya mağaza**: çıktı verilerini ekranına veya işleme için mağaza

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Pig Latin'i kullanıcı tanımlı işlevler (UDF), Pig Latin modelinde zordur mantığını dış bileşenleri çağırma olanak tanıyan da destekler.

Pig Latin'i hakkında daha fazla bilgi için bkz: [Pig Latin başvuru el ile 1](http://archive.cloudera.com/cdh/3/pig/piglatin_ref1.html) ve [Pig Latin başvuru el ile 2](http://archive.cloudera.com/cdh/3/pig/piglatin_ref2.html).

UDF ile Pig kullanma örneği için aşağıdaki belgelere bakın:

* [HDInsight, Pig ile DataFu kullanma](apache-hadoop-use-pig-datafu-udf.md) -DataFu olan Apache tarafından tutulan yararlı UDF'ler koleksiyonu
* [Pig ve Hive, HDInsight ile Python kullanma](python-udf-hdinsight.md)
* [Hive ve Pig, HDInsight ile C# kullanma](apache-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Örnek veri

HDInsight sağlar içinde depolanan çeşitli örnek veri kümeleri, `/example/data` ve `/HdiSamples` dizinleri. Bu dizinler, kümenin varsayılan depolama içindedir. Bu belgedeki Pig örnekte *log4j* dosya `/example/data/sample.log`.

Her günlük dosyası içindeki içeren bir dizi alanlarının oluşur bir `[LOG LEVEL]` alanı türünün ve önem örneğin göstermek için:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Önceki örnekte, günlük düzeyi hatadır.

> [!NOTE]
> Log4j dosyasını kullanarak da oluşturabilirsiniz [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) aracı günlüğe kaydetme ve ardından blobunuza bu dosyayı karşıya yükleyin. Bkz: [HDInsight verileri karşıya yükleme](../hdinsight-upload-data.md) yönergeler için. Azure Depolama'daki blobları HDInsight ile nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Blob Depolama kullanma](../hdinsight-hadoop-use-blob-storage.md).

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
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [HDInsight ile Sqoop kullanma](hdinsight-use-sqoop.md)
* [HDInsight ile Oozie kullanma](../hdinsight-use-oozie.md)
* [HDInsight ile MapReduce işleri kullanma][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
