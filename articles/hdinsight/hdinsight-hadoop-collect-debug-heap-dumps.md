---
title: Hata ayıklama ve Hadoop Hizmetleri yığın dökümleri - Azure ile analiz | Microsoft Docs
description: Otomatik olarak yığın dökümleri Hadoop Hizmetleri için toplamak ve hata ayıklama ve analiz için Azure Blob Depolama hesabı içine yerleştirin.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e4ec4ebb-fd32-4668-8382-f956581485c4
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 0721d20987008e5cc6370c6e853440ce93edcfa9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Hata ayıklama ve Hadoop Hizmetleri çözümlemek için Blob depolama alanına toplama yığın dökümleri
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Yığın dökümleri döküm oluşturulduğunda değişkenlerin değerleri de dahil olmak üzere uygulamanın belleğin anlık görüntüsünü içerir. Bu nedenle bunlar çalışma zamanında oluşan sorunları tanılamak için kullanışlıdır. Yığın dökümleri ve otomatik olarak Hadoop Hizmetleri için toplanan kullanıcı HDInsightHeapDumps altında Azure Blob Depolama hesabına içine yerleştirilen /.

Çeşitli hizmetler için yığın dökümleri koleksiyonunu ayrı kümelerde Hizmetleri için etkinleştirilmesi gerekir. Bu özellik için bir küme için devre dışı olmasını varsayılandır. Koleksiyon etkinleştirildikten sonra bunlar kaydediliyor Blob storage hesabı izlemek için önerilir; böylece bu yığın dökümleri büyük olabilir.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Bu makaledeki bilgiler yalnızca Windows tabanlı Hdınsight için geçerlidir. Linux tabanlı Hdınsight hakkında daha fazla bilgi için bkz: [etkinleştir yığın dökümleri Linux tabanlı hdınsight'ta Hadoop Hizmetleri için](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Uygun hizmetler için yığın dökümleri
Aşağıdaki hizmetler için yığın dökümleri etkinleştirebilirsiniz:

* **hcatalog** -tempelton
* **Hive** -hiveserver2, meta depo, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, iş

## <a name="configuration-elements-that-enable-heap-dumps"></a>Yığın dökümleri etkinleştirmek yapılandırma öğeleri
Bir hizmet için yığın dökümleri etkinleştirmek için bu hizmet tarafından belirtilen bölümünde uygun yapılandırma öğelerini ayarlamanız gerekir **servıce_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Değeri **servıce_name** hizmetlerden herhangi biri burada listelenebilir: tempelton, hiveserver2 meta depo, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, veya iş.

## <a name="enable-using-azure-powershell"></a>Azure PowerShell kullanarak etkinleştirme
Örneğin, yığın dökümleri üzerinde jobhistoryserver için Azure PowerShell kullanarak etkinleştirmek için aşağıdaki komut dosyasını kullanabilirsiniz:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>.NET SDK kullanarak etkinleştirin
Örneğin, yığın dökümleri üzerinde jobhistoryserver için Azure Hdınsight .NET SDK'sını kullanarak etkinleştirmek için aşağıdaki kodu kullanabilirsiniz:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
