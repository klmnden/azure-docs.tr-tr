---
title: Hata ayıklama ve Apache Hadoop Hizmetleri yığın dökümlerini - Azure ile analiz
description: Otomatik olarak Apache Hadoop Hizmetleri için yığın dökümlerini toplama ve hata ayıklama ve analiz için Azure Blob Depolama hesabı içinde yerleştirin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 1b4ca22faf8ef01cab4b2e7231fea8ed49f0fcb3
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494583"
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-apache-hadoop-services"></a>Hata ayıklama ve Apache Hadoop Hizmetleri çözümlemek için Blob Depolama alanında toplama yığın dökümleri
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Yığın dökümlerini değişkenlerin değerleri döküm oluşturulduğu zaman dahil olmak üzere, uygulamanın bellek anlık görüntüsünü içerir. Bu nedenle bunlar çalıştırma zamanında gerçekleşen sorunları tanılamak için kullanışlıdır. Yığın dökümlerini otomatik olarak toplanmasını için [Apache Hadoop](https://hadoop.apache.org/) Hizmetleri ve Azure Blob Depolama hesabı HDInsightHeapDumps altında bir kullanıcının içinde yer /.

Çeşitli Hizmetleri için yığın dökümlerini koleksiyonunu tek tek Küme Hizmetleri için etkinleştirilmesi gerekir. Bu özellik için bir küme için devre dışı olmasını varsayılandır. Toplama etkinleştirildikten sonra nereye kaydedildiklerini Blob Depolama hesabı izleme önerilir, bu nedenle bu yığın dökümlerini büyük olabilir.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Bu makaledeki bilgiler, yalnızca Windows tabanlı HDInsight için geçerlidir. Linux tabanlı HDInsight hakkında daha fazla bilgi için bkz: [etkin yığın dökümleri Linux tabanlı HDInsight üzerinde Apache Hadoop Hizmetleri için](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Uygun hizmetler için yığın dökümlerini
Aşağıdaki hizmetleri için yığın dökümlerini etkinleştirebilirsiniz:

* **Apache hcatalog** -tempelton
* **Apache hive** -hiveserver2, meta veri deposu, derbyserver
* **mapreduce** -jobhistoryserver
* **Apache yarn** -resourcemanager, nodemanager, timelineserver
* **Apache hdfs** -datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Yapılandırma öğeleri, yığın dökümlerini etkinleştirme
Bir hizmet için yığın dökümlerini etkinleştirmek için bölümündeki tarafından belirtilen aydaki hizmet için uygun yapılandırma öğelerini ayarlamanız gerekir **servıce_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Değerini **servıce_name** hizmetlerinden herhangi birinin burada listelenir: tempelton, hiveserver2 meta veri deposu, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, veya namenode.

## <a name="enable-using-azure-powershell"></a>Azure PowerShell'i kullanarak etkinleştirme
Örneğin, yığın dökümlerini üzerinde jobhistoryserver için Azure PowerShell kullanarak etkinleştirmek için aşağıdaki betiği kullanabilirsiniz:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>.NET SDK kullanarak etkinleştirin
Örneğin, yığın dökümlerini üzerinde jobhistoryserver için Azure HDInsight .NET SDK'sını kullanarak etkinleştirmek için aşağıdaki kodu kullanabilirsiniz:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
