---
title: "Hdınsight'ta Hadoop Pig kullanma | Microsoft Docs"
description: "Hdınsight'ta Hadoop ile pig kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: cca748eafc547d808a3bee6bb8dedcec821f6cb5
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>Hdınsight'ta Hadoop ile pig kullanma

Nasıl kullanacağınızı öğrenin [Apache Pig](http://pig.apache.org/) Hdınsight ile...

Pig olan bir platform olarak bilinen bir yordam dilini kullanarak Hadoop için programlar oluşturma için *Pig Latin*. Pig alternatiftir Java için oluşturmak için *MapReduce* çözümleri ve bunu Azure Hdınsight ile birlikte. Hdınsight ile Pig çeşitli yolları kullanılabilir bulmak için aşağıdaki tabloyu kullanın:

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, UNIX, Mac OS X veya Windows |
| [REST API](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux veya Windows |Windows (için şimdi) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux veya Windows |Windows |
| [Uzak Masaüstü](apache-hadoop-use-pig-remote-desktop.md) (Hdınsight 3.2 ve 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Pig neden kullanılır?

Hadoop MapReduce kullanarak veri işleme zorluklarından işleme mantığı yalnızca bir eşleme ve azaltma işlevini kullanarak uygular. Karmaşık işleme için sık işleme Zincirli olan birden çok MapReduce işlemlere ayırmak elinizde birlikte istenen sonucu elde etmek için.

Pig, istenen çıkış üretmek için aracılığıyla veri akışları dönüşümleri bir dizi olarak işleme tanımlamanızı sağlar.

Pig Latin dili istenen çıkış üretmek için bir veya daha fazla dönüşümleri aracılığıyla ham giriş veri akışından açıklamak sağlar. Pig Latin programlar bu genel model izleyin:

* **Yük**: dosya sisteminden yönetilebilmesini veri okuma

* **Dönüştürme**: verileri işlemek

* **Döküm veya mağaza**: çıktı verilerini ekranına veya işleme için mağaza

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Pig Latin kullanıcı tanımlı işlevler (UDF), Pig Latin modelinde zordur mantığı uygulamanız dış bileşenlere çağırma olanak tanıyan de destekler.

Pig Latin hakkında daha fazla bilgi için bkz: [Pig Latin başvuru el ile 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) ve [Pig Latin başvuru el ile 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

UDF'ler ile Pig kullanma örneği için aşağıdaki belgelere bakın:

* [Pig hdınsight'ta ile DataFu kullanmak](apache-hadoop-use-pig-datafu-udf.md) -DataFu Apache tarafından tutulan yararlı UDF'ler koleksiyonudur
* [Python Pig ve hdınsight'ta Hive kullanma](python-udf-hdinsight.md)
* [C# Hive veya Pig hdınsight'ta ile kullanma](apache-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Örnek veri

Hdınsight içinde depolanan çeşitli örnek veri kümeleri, sağlar `/example/data` ve `/HdiSamples` dizinleri. Bu dizinleri, kümeniz için varsayılan depolama arasındadır. Bu belge Pig örnekte kullanır *log4j* dosya `/example/data/sample.log`.

İçeren bir dizi alanlarının her günlük dosyası içindeki oluşan bir `[LOG LEVEL]` türünün ve önem örneğin göstermek için alan:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Önceki örnekte, günlük düzeyi hatasıdır.

> [!NOTE]
> Log4j dosyasını kullanarak da oluşturabilirsiniz [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) aracı günlüğe kaydetme ve bu dosya, blob karşıya yükleyin. Bkz: [karşıya verileri hdınsight'a](../hdinsight-upload-data.md) yönergeler için. BLOB'ları Azure depolama alanında Hdınsight ile nasıl kullanıldığı konusunda daha fazla bilgi için bkz: [kullanım Azure Blob Storage Hdınsight ile](../hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Örnek Proje

Aşağıdaki Pig Latin iş yükler `sample.log` Hdınsight kümeniz için varsayılan depolama biriminden dosya. Ardından, bir dizi giriş verilerini her günlük düzeyi oluştu kaç kez sayısını sonucunda dönüşümleri gerçekleştirir. Sonuçları STDOUT yazılır.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Aşağıdaki resimde verileri her bir dönüşüm yaptığı bir özeti gösterilir.

![Grafik gösterimi dönüşümleri][image-hdi-pig-data-transformation]

## <a id="run"></a>Pig Latin işini çalıştır

Hdınsight, Pig Latin işleri çeşitli yöntemler kullanarak çalıştırabilirsiniz. Hangi yöntemi sizin için uygun olduğuna karar vermek için aşağıdaki tabloyu kullanın, sonra bir anlatım için bağlantıyı izleyin.

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ....from bu **istemci** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, UNIX, Mac OS X veya Windows |
| [Curl](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux veya Windows |Windows (için şimdi) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux veya Windows |Windows |
| [Uzak Masaüstü](apache-hadoop-use-pig-remote-desktop.md) (Hdınsight 3.2 ve 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="pig-and-sql-server-integration-services"></a>Pig ve SQL Server Integration Services

Pig işi çalıştırmak için SQL Server Integration Services (SSIS) kullanabilirsiniz. Azure Feature Pack SSIS için Hdınsight'ta Pig işlerle çalışma aşağıdaki bileşenleri sağlar.

* [Azure Hdınsight Pig görevi][pigtask]

* [Azure aboneliği Bağlantı Yöneticisi][connectionmanager]

Azure Feature Pack hakkında daha fazla bilgi için SSIS [burada][ssispack].

## <a id="nextsteps"></a>Sonraki adımlar
Hdınsight ile Pig kullanma aktarmayı öğrendiniz, Azure Hdınsight ile çalışmak için diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [Hdınsight'a verileri yükleme](../hdinsight-upload-data.md)
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [Hdınsight ile Sqoop kullanma](hdinsight-use-sqoop.md)
* [Oozie Hdınsight ile kullanma](../hdinsight-use-oozie.md)
* [Hdınsight ile MapReduce işleri kullanma][hdinsight-use-mapreduce]

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
