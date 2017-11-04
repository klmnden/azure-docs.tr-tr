---
title: ".NET Hadoop MapReduce hdınsight'ta Linux tabanlı - Azure ile kullanma | Microsoft Docs"
description: "Linux tabanlı Hdınsight üzerinde MapReduce akış için .NET uygulamaları kullanmayı öğrenin."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/04/2017
ms.author: larryfr
ms.openlocfilehash: 978606aa5f16842f8198ee67a65b476b4f560ab7
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a>Windows tabanlı Hdınsight Linux tabanlı hdınsight'a için .NET çözümleri geçirme

Linux tabanlı Hdınsight kümeleri kullanım [Mono (https://mono-project.com)](https://mono-project.com) .NET uygulamalarını çalıştırmak için. Mono, Linux tabanlı Hdınsight ile MapReduce uygulamalar gibi .NET bileşenleri kullanmanıza olanak sağlar. Bu belgede, Windows tabanlı Hdınsight kümeleri Mono Linux tabanlı Hdınsight üzerinde çalışmak oluşturulan .NET çözümlerin geçirmek öğrenin.

## <a name="mono-compatibility-with-net"></a>.NET ile Mono uyumluluk

Mono sürüm 4.2.1 Hdınsight sürüm 3.5 ile dahil edilir. Hdınsight ile dahil Mono sürümü hakkında daha fazla bilgi için bkz: [Hdınsight bileşen sürümü](hdinsight-component-versioning.md). Mono belirli bir sürümünü yüklemek için bkz: [veya Mono güncelleştirmesini](hdinsight-hadoop-install-mono.md) belge.

Mono ve .NET uyumluluğu hakkında daha fazla bilgi için bkz: [Mono uyumluluk (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) belge.

> [!IMPORTANT]
> SCP.NET framework Mono ile uyumludur. SCP.NET Mono ile kullanma hakkında daha fazla bilgi için bkz: [Hdınsight üzerinde Apache Storm için C# topolojileri geliştirme için Visual Studio](storm/apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Otomatik taşınabilirlik çözümleme

[.NET taşınabilirlik Çözümleyicisi](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) uygulama Mono arasındaki uyumsuzluklar raporunu oluşturmak için kullanılabilir. Mono taşınabilirlik için uygulamanızın denetlemek için Çözümleyicisi'ni yapılandırmak için aşağıdaki adımları kullanın:

1. Yükleme [.NET taşınabilirlik Çözümleyicisi](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Yükleme sırasında kullanmak için Visual Studio sürümünü seçin.

2. Visual Studio 2015'ten seçin __Çözümle__ > __taşınabilirlik Çözümleyicisi ayarları__, emin olun __4.5__ iade __Mono__ bölümü.

    ![Mono bölümünde Çözümleyicisi ayarları için kontrol 4.5](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Seçin __Tamam__ yapılandırmayı kaydetmek için.

3. Seçin __analiz__ > __derleme taşınabilirlik analiz__. Çözümünüzü içeren derlemenin seçin ve ardından __açık__ çözümleme işlemine başlamak için.

4. Analiz tamamlandığında seçin __Çözümle__ > __analiz raporları görüntülemek__. İçinde __taşınabilirlik çözümleme sonuçlarını__seçin __raporunu Aç__ bir raporu açın.

    ![Taşınabilirlik Çözümleyicisi sonuçları iletişim](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> Çözümleyici çözümünüzün her sorun catch olamaz. Örneğin, bir dosya yolu `c:\temp\file.txt` Mono Windows üzerinde çalışıyorsa, Tamam olarak kabul edilir. Aynı yolu Linux platformu üzerinde geçerli değil.

## <a name="manual-portability-analysis"></a>El ile Taşınabilirlik çözümleme

El ile denetim bilgileri kullanarak, kodunuzun gerçekleştirmek [uygulama taşınabilirliği (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) belge.

## <a name="modify-and-build"></a>Değiştirme ve oluşturma

Visual Studio için Hdınsight .NET çözümlerinizi oluşturmaya kullanmaya devam edebilirsiniz. Ancak projeyi .NET Framework 4.5 kullanacak şekilde yapılandırıldığından emin olmanız gerekir.

## <a name="deploy-and-test"></a>Dağıtma ve test etme

.NET taşınabilirlik Çözümleyicisi veya el ile çözümleme önerileri kullanarak çözümünüzü değiştirdiniz. sonra Hdınsight ile test etmeniz gerekir. Linux tabanlı Hdınsight kümesi çözümü test düzeltilmesi gereken Zarif sorun olduğunu gösterebilir. Sınama sırasında uygulamanızda ek günlüğü etkinleştirmenizi öneririz.

Günlükleri erişme ile ilgili daha fazla bilgi için aşağıdaki belgelere bakın:

* [Linux tabanlı HDInsight’ta YARN uygulama günlüklerine erişme](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Sonraki adımlar

* [C# ile MapReduce hdınsight'ta kullanma](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [C# kullanıcı tanımlı işlevler Hive veya Pig kullanın](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Hdınsight üzerinde Storm için C# topolojileri geliştirme](storm/apache-storm-develop-csharp-visual-studio-topology.md)