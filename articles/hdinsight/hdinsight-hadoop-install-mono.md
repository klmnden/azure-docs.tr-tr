---
title: Veya hdınsight'ta - Azure Mono güncelleştirmesini | Microsoft Docs
description: Hdınsight kümesi ile Mono belirli bir sürümü kullanmayı öğrenin. Mono Linux tabanlı Hdınsight kümelerinde .NET uygulamalarını çalıştırmak için kullanılır.
services: hdinsight
documentationCenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/10/2018
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: 165f1d8175c7c7b58a5eec02a208b81fe73cb5f9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-or-update-mono-on-hdinsight"></a>Yükleme veya Mono hdınsight'ta güncelleştir

Belirli bir sürümünü yüklemeyi öğrenin [Mono](https://www.mono-project.com) Hdınsight 3.4 veya daha yüksek.

Mono Hdınsight 3.4 veya üstü yüklü ve .NET uygulamalarını çalıştırmak için kullanılır. Her Hdınsight sürümü ile birlikte verilen Mono sürümü hakkında daha fazla bilgi için bkz: [Hdınsight bileşen sürümü oluşturma](hdinsight-component-versioning.md). Kümenizde farklı bir sürümünü yüklemek için bu belgede betik eylemi kullanın. 

## <a name="how-it-works"></a>Nasıl çalışır?

Bu komut dosyasını aşağıdaki parametresini kabul eder:

* __Mono sürüm numarası__: yüklemek için Mono sürümü. Sürüm listesinden kullanılabilir olması [ https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/ ](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

Komut dosyasını aşağıdaki Mono paketleri yükler:

* __Mono tamamlayın__

* __ca-certificates-mono__

## <a name="the-script"></a>Komut dosyası

__Komut dosyası konumu__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Gereksinimleri__:

* Komut dosyası üzerinde uygulanması gereken __baş düğümler__ ve __çalışan düğümleri__.

## <a name="to-use-the-script"></a>Betik kullanmak için

Bu komut dosyasının Hdınsight ile nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) belge. Azure portalı, Azure PowerShell veya Azure CLI aracılığıyla komut dosyasını kullanabilirsiniz.

Betik eylemi belge aşağıdaki çalışırken, aşağıdaki URI kullanın:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

Yüklü Mono sürüm belirtmek için sürüm numarasını kullanın __parametreleri__ alan. Örneğin `5.4` Mono 5.4 yüklemek için.

> [!NOTE]
> Hdınsight ile bu komut dosyası yapılandırırken, komut dosyası olarak işaretlemek __kalıcı__. Bu ayar, komut dosyası işlemleri ölçeklendirme aracılığıyla eklenen çalışan düğümlerine uygulamak Hdınsight sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Yükseltme veya Hdınsight'ta Mono belirli bir sürümünü yüklemek öğrendiniz. .NET uygulamaları hdınsight'ta Mono ile kullanma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Hdınsight üzerinde MapReduce akış için .NET kullanın](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Hive veya Pig hdınsight'ta ile .NET kullanın](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)
* [C# hdınsight'ta Storm çözümleri geliştirme](storm/apache-storm-develop-csharp-visual-studio-topology.md)
* [Linux tabanlı Hdınsight .NET çözümleri geçirme](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Betik eylemleri kullanma hakkında daha fazla bilgi için bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md)