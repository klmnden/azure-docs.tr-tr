---
title: .NET ile HDInsight - Azure Linux tabanlı Hadoop MapReduce kullanma
description: .NET uygulamalarında Linux tabanlı HDInsight MapReduce akış için nasıl kullanılacağı hakkında bilgi edinin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: 8650bad4d980efba78fe753200bca364bda26488
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595986"
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a>.NET çözümlerini Linux tabanlı HDInsight için Windows tabanlı HDInsight için geçirme

Linux tabanlı HDInsight kümeleri kullanım [Mono (https://mono-project.com) ](https://mono-project.com) .NET uygulamaları çalıştırmak için. Mono Linux tabanlı HDInsight ile MapReduce uygulamaları gibi .NET bileşenlerini kullanmanıza olanak tanır. Bu belgede, .NET çözümlerini Linux tabanlı HDInsight üzerinde Mono çalışmak Windows tabanlı HDInsight kümeleri için oluşturulan geçirmeyi öğrenin.

## <a name="mono-compatibility-with-net"></a>.NET Mono uyumluluğu

HDInsight sürümü 3.6 ile Mono sürüm 4.2.1 dahildir. HDInsight ile dahil Mono sürümü hakkında daha fazla bilgi için bkz. [HDInsight bileşen sürümü](hdinsight-component-versioning.md). Mono belirli bir sürümünü yüklemek için bkz [Mono yükleme veya güncelleştirme](hdinsight-hadoop-install-mono.md) belge.

Mono ve .NET uyumluluğu hakkında daha fazla bilgi için bkz. [Mono uyumluluğu (http://www.mono-project.com/docs/about-mono/compatibility/) ](http://www.mono-project.com/docs/about-mono/compatibility/) belge.

> [!IMPORTANT]
> SCP.NET çerçevesi, Mono ile uyumludur. SCP.NET Mono ile kullanma hakkında daha fazla bilgi için bkz. [HDInsight üzerinde Apache Storm için C# topolojileri geliştirme için Visual Studio](storm/apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Otomatik taşınabilirlik analizi

[.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) uygulamanızı ve Mono arasında uyumsuzluk, rapor oluşturmak için kullanılabilir. Uygulamanız Mono taşınabilirlik için denetlenecek Çözümleyicisi'ni yapılandırmak için aşağıdaki adımları kullanın:

1. Yükleme [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Yükleme sırasında kullanmak için Visual Studio sürümünü seçin.

2. Visual Studio 2015'ten seçin __Çözümle__ > __taşınabilirlik Çözümleyicisi ayarları__, emin olun __4.5__ iade __Mono__ bölümü.

    ![Mono bölümünde Çözümleyicisi ayarları için kontrol 4.5](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Seçin __Tamam__ yapılandırmayı kaydetmek için.

3. Seçin __analiz__ > __derleme taşınabilirlik analiz__. Çözümünüzü içeren derlemeyi seçin ve ardından __açık__ analize başlamak için.

4. Analiz tamamlandıktan sonra seçin __Çözümle__ > __analiz raporları görüntüleme__. İçinde __taşınabilirlik analiz sonuçları__seçin __raporunu Aç__ bir raporu açın.

    ![Taşınabilirlik Çözümleyicisi sonuçları iletişim](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> Çözümünüzü her sorun Çözümleyicisi yakalayamaz. Örneğin, bir dosya yolu `c:\temp\file.txt` Mono Windows üzerinde çalışıyorsa Tamam olarak değerlendirilir. Aynı yol bir Linux platformunda geçerli değil.

## <a name="manual-portability-analysis"></a>El ile Taşınabilirlik analizi

El ile bir denetim bilgileri kullanarak kodunuzun gerçekleştirmek [uygulama taşınabilirliğini (http://www.mono-project.com/docs/getting-started/application-portability/) ](http://www.mono-project.com/docs/getting-started/application-portability/) belge.

## <a name="modify-and-build"></a>Derleme ve değiştirme

Visual Studio için HDInsight .NET çözümlerinizi oluşturmak için kullanmaya devam edebilirsiniz. Ancak, proje .NET Framework 4.5 kullanmak için yapılandırıldığından emin olmanız gerekir.

## <a name="deploy-and-test"></a>Dağıtma ve test etme

.NET Portability Analyzer veya el ile çözümleme önerileri kullanarak çözümünüzü oluşturduktan sonra HDInsight ile test etmeniz gerekir. Linux tabanlı HDInsight kümesi üzerinde çözüm test ediliyor, düzeltilmesi gereken Zarif sorunlarını gösterilmesine neden olabilir. Sınama sırasında uygulamanızda ek günlükler etkinleştirmenizi öneririz.

Günlüklerine erişme hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Linux tabanlı HDInsight’ta YARN uygulama günlüklerine erişme](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight MapReduce ile C# kullanma](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Hive ve Pig ile kullanıcı tanımlı C# işlevlerini kullanma](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [HDInsight üzerinde Storm için C# topolojileri geliştirme](storm/apache-storm-develop-csharp-visual-studio-topology.md)
