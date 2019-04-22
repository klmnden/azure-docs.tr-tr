---
title: 'Azure HDInsight: Java örnekleri'
description: Java örnekleri, Java için HDInsight SDK'sını kullanarak ortak görevleri için Github'da bulabilirsiniz.
author: hrasheed-msft
ms.service: hdinsight
ms.topic: sample
ms.date: 04/15/2019
ms.author: hrasheed
ms.openlocfilehash: 971af370425f733649f0b8d0079baaf93cc72129
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681548"
---
# <a name="azure-hdinsight-java-samples"></a>Azure HDInsight: Java örnekleri

> [!div class="op_single_selector"]
> * [Java Örnekleri](hdinsight-sdk-java-samples.md)
> * [.NET Örnekleri](hdinsight-sdk-dotnet-samples.md)
> * [Python Örnekleri](hdinsight-sdk-python-samples.md)
<!-- * [Go Examples](hdinsight-sdk-dotnet-samples.md)-->

Bu makalede aşağıdakiler sunulmaktadır:

* Küme oluşturma görevleri için örneklere bağlantılar.
* Diğer yönetim görevleri için başvuru içeriği bağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [Java için Azure HDInsight SDK](https://docs.microsoft.com/java/api/overview/azure/hdinsight#sdk-installation)

## <a name="cluster-management---creation"></a>Küme yönetimi - oluşturma

* [Kafka kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateKafkaClusterSample.java)
* [Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateSparkClusterSample.java)
* [Azure Data Lake depolama 2. nesil ile bir Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateHadoopClusterWithAdlsGen2Sample.java)
* [Kurumsal güvenlik paketi (ESP) ile bir Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateEspClusterSample.java)

Java için bu örnekleri kopyalayarak alabilirsiniz [hdınsight-java-sdk-samples](https://github.com/Azure-Samples/hdinsight-java-sdk-samples) GitHub deposu.

[!INCLUDE [hdinsight-sdk-additional-functionality](../../includes/hdinsight-sdk-additional-functionality.md)]

Bu ek SDK işlev bulunabilir için kod parçacıkları [Java başvuru belgeleri için HDInsight SDK'sı](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-preview).