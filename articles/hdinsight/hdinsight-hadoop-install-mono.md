---
title: HDInsight - Azure üzerinde Mono yükleme veya güncelleştirme
description: HDInsight kümesi ile Mono belirli bir sürümünü kullanmayı öğrenin. Mono, Linux tabanlı HDInsight kümelerinde .NET uygulamaları çalıştırmak için kullanılır.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: hrasheed
ms.custom: hdinsightactive
ms.openlocfilehash: 9daf0d45c9bb24192750991e7bf84fa3614c970d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011043"
---
# <a name="install-or-update-mono-on-hdinsight"></a>HDInsight üzerinde Mono yükleme veya güncelleştirme

Belirli bir sürümünü yüklemeyi öğrenin [Mono](https://www.mono-project.com) HDInsight 3.4 veya üzeri sürümde.

Mono HDInsight 3.4 ve üzeri yüklenir ve .NET uygulamaları çalıştırmak için kullanılır. Mono her HDInsight sürümü ile birlikte verilen sürümü hakkında daha fazla bilgi için bkz: [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md). Kümenizde farklı bir sürümünü yüklemek için bu belgede betik eylemi kullanın. 

## <a name="how-it-works"></a>Nasıl çalışır?

Bu betik, aşağıdaki parametre kabul eder:

* __Mono sürüm numarası__: yüklemek için Mono sürümü. Sürüm listesinden kullanılabilir olması [ https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/ ](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

Betik aşağıdaki Mono paketleri yükler:

* __Mono tamamlama__

* __ca-certificates-mono__

## <a name="the-script"></a>Komut dosyası

__Betik konumu__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Gereksinimleri__:

* Betik üzerinde uygulanmalıdır __baş düğümlerine__ ve __çalışan düğümleri__.

## <a name="to-use-the-script"></a>Betiği kullanmak için

Bu betik, HDInsight ile kullanma hakkında daha fazla bilgi için bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) belge. Azure portalı, Azure PowerShell veya Azure Klasik CLI aracılığıyla betiği kullanabilirsiniz.

Betik eylemi belgesi aşağıdaki çalışırken, aşağıdaki URI kullanın:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

Mono yüklenen sürümü belirlemek için sürüm numarasını kullanın __parametreleri__ alan. Örneğin, `5.4` Mono 5.4 yüklemek için.

> [!NOTE]
> HDInsight ile bu betik yapılandırırken, komut dosyası olarak işaretle __kalıcı__. HDInsight betik işlemleri ölçeklendirme aracılığıyla eklenen çalışan düğümlerine uygulamak bu ayarı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight üzerinde Mono belirli bir sürümünü yüklemek veya yükseltmek öğrendiniz. HDInsight üzerinde Mono ile .NET uygulamalarını kullanma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight MapReduce akış için .NET kullanma](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Hive ve Pig HDInsight üzerinde .NET kullanma](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)
* [C# HDInsight üzerinde Storm ile çözümleri geliştirin](storm/apache-storm-develop-csharp-visual-studio-topology.md)
* [.NET çözümlerini Linux tabanlı HDInsight için geçirme](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Betik eylemlerini kullanarak daha fazla bilgi için bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md)
