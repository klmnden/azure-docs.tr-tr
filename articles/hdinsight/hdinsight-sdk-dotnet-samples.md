---
title: 'Azure HDInsight: .NET örnekleri'
description: Bulma C# .NET HDInsight SDK'sını kullanarak ortak görevleri için github'da .NET örnekleri.
author: hrasheed-msft
ms.service: hdinsight
ms.topic: sample
ms.date: 04/15/2019
ms.author: hrasheed
ms.openlocfilehash: f589c5e23be105086d2908fb7db72e69c781e726
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678267"
---
# <a name="azure-hdinsight-net-samples"></a>Azure HDInsight: .NET örnekleri

> [!div class="op_single_selector"]
> * [.NET Örnekleri](hdinsight-sdk-dotnet-samples.md)
> * [Python Örnekleri](hdinsight-sdk-python-samples.md)
> * [Java Örnekleri](hdinsight-sdk-java-samples.md)
<!-- * [Go Examples](hdinsight-sdk-go-samples.md)-->

Bu makalede aşağıdakiler sunulmaktadır:

* Küme oluşturma görevleri için örneklere bağlantılar.
* Diğer yönetim görevleri için başvuru içeriği bağlar.

Yapabilecekleriniz [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay krediler sunar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [.NET için Azure HDInsight SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight#sdk-installation)

## <a name="cluster-management---creation"></a>Küme yönetimi - oluşturma

* [Kafka kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateKafkaClusterSample.cs)
* [Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateSparkClusterSample.cs)
* [Azure Data Lake depolama 2. nesil ile bir Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateHadoopClusterWithAdlsGen2Sample.cs)
* [Kurumsal güvenlik paketi (ESP) ile bir Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateEspClusterSample.cs)

.NET için bu örnekleri kopyalayarak alabilirsiniz [hdınsight-dotnet-sdk-samples](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples) GitHub deposu.

[!INCLUDE [hdinsight-sdk-additional-functionality](../../includes/hdinsight-sdk-additional-functionality.md)]

Bu ek SDK işlev bulunabilir için kod parçacıkları [HDInsight .NET SDK başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet).