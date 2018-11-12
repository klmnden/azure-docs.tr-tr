---
title: Bir Windows PC HDInsight - Azure üzerinde Hadoop ile kullanma
description: Bir Windows PC hadoop'ta HDInsight üzerinde çalışabilirsiniz. Sorgu Linux PowerShell ve Visual Studio Araçları ile kümeleri ve yönetin. .NET ile büyük veri çözümleri geliştirin.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 05/17/2017
ms.openlocfilehash: c80c7a075b8a7735a4e9d2438c94eaac89f2b50e
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51006976"
---
# <a name="work-in-the-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Bir Windows bilgisayardan HDInsight üzerinde Hadoop ekosistemindeki çalışma

Geliştirme ve HDInsight üzerinde Hadoop ekosistemindeki çalışmak için Windows PC yönetimi seçenekleri hakkında bilgi edinin. 

HDInsight Apache Hadoop ve Hadoop bileşenleri, Linux üzerinde geliştirilen bir açık kaynak teknolojilerini temel alır. HDInsight sürüm 3.4 ve üzeri, Ubuntu Linux dağıtımı küme için temel işletim sistemi kullanır. Ancak, bir Windows istemci ya da Windows geliştirme ortamı kullanarak HDInsight ile çalışabilirsiniz.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Dağıtım ve yönetim görevleri için PowerShell kullanma
Azure PowerShell, denetlemek ve dağıtım ve HDInsight Windows gelen yönetim görevleri otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır.

PowerShell ile yapmak görev örnekleri:

* [PowerShell kullanarak küme oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [PowerShell kullanarak Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-powershell.md)
* [PowerShell ile küme yönetme](hdinsight-administer-use-powershell.md)

Adımlarını izleyin [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) en son sürümünü almak için. Azure Resource Manager için yeni cmdlet'ler kullanmak üzere değiştirilmesi için gereken komut dosyalarınız varsa [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçiş](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Yardımcı programlar bir tarayıcıda çalıştırabilirsiniz.
Aşağıdaki yardımcı programlar, bir web tarayıcıda çalışan kullanıcı Arabirimi vardır:
* **[Azure Cloud Shell (Önizleme)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  tarayıcınızda ve içinden çalışır bir etkileşimli, komut satırı kabuğu olan Azure portalı.
* **[Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md)**  yönetim ve izleme yardımcı programı gibi çeşitli işlemler yönetmek için kullanılan Azure portalında kullanılabilir:
    * [Ambari REST API ile kullanma](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Ambari, Hive görünümü](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [Ambari Tez görünümü](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Visual Studio için Data Lake (Hadoop) araçları
Storm topolojilerini dağıtma ve yönetme için Visual Studio için Data Lake Araçları'nı kullanın. Data Lake araçları, SCP.NET Visual Studio ile C# Storm topolojileri geliştirme olanak tanıyan SDK'yı da yükler.

Aşağıdaki örnekler için geçmeden önce [yükleyin ve Visual Studio için Data Lake araçları deneyin](hadoop/apache-hadoop-visual-studio-tools-get-started.md). 

Visual Studio için Data Lake araçları ile Visual Studio ile gerçekleştirebileceğiniz görevleri örnekleri:
* [Storm topolojilerini Visual Studio'dan yönetin ve dağıtın](storm/apache-storm-deploy-monitor-topology-linux.md)
* [Visual Studio kullanarak Storm için C# topolojileri geliştirme](storm/apache-storm-develop-csharp-visual-studio-topology.md). BITS, Azure Cosmos DB ile SQL veritabanı gibi veritabanlarına bağlanabilirsiniz Storm topolojilerini örnek şablonları içerir.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio ve .NET SDK'sı 

Visual Studio, .NET SDK'sı ile kümeleri yönetme ve büyük veri uygulamaları geliştirmek için kullanabilirsiniz. Aşağıdaki görevler için diğer Ide'leri kullanabilirsiniz, ancak Visual Studio'da örnekler gösterilmektedir.

Visual Studio'da .NET SDK'sı ile gerçekleştirebileceğiniz görevler örnekleri:
* [Kümeleri oluşturma ve bir .NET Framework uygulamasından HDInsight içinde çalışma](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [.NET SDK kullanarak Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md)
* [Hive ve Pig, Hadoop'ta akış ile kullanıcı tanımlı C# işlevlerini kullanma](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

> İpucu Windows tabanlı HDInsight kümeleriyle .NET çözümlerini çalıştırıyorsanız, Linux tabanlı kümeler için bir geçiş planı için zamanı geldi. Daha fazla bilgi için [geçirme .NET çözüm için Linux tabanlı HDInsight için Windows tabanlı HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij Idea ve Eclipse IDE için Spark kümeleri
Her ikisi de [Intellij Idea](https://www.jetbrains.com/idea/download) ve [Eclipse IDE'yi](https://www.eclipse.org/downloads/) yapmak için kullanılabilir:
* Geliştirin ve bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulaması gönderin.
* Spark kümesi kaynaklarınıza erişin.
* Geliştirin ve yerel olarak Scala Spark uygulamasını çalıştırın.

Bu makaleler Göster nasıl: 
* Intellij Idea: [eklentisi Intellij için Azure araç takımı ve Scala SDK'sını kullanarak oluşturma Spark uygulamaları.](spark/apache-spark-intellij-tool-plugin.md)
* IDE veya Scala IDE için Eclipse eclipse: [oluşturma Spark uygulamaları ve Eclipse için Azure Araç Seti](spark/apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Not defterlerini Spark üzerinde veri uzmanları için 
HDInsight için Apache Spark kümelerinde Zeppelin not defterlerini ve Jupyter not defterleri ile kullanılabilen çekirdekler içerir. 

* [Çekirdekler üzerinde Spark kümeleri ile Jupyter not defterlerini Spark uygulamaları test etmek için nasıl kullanılacağını öğrenin](spark/apache-spark-zeppelin-notebook.md)
* [Zeppelin not defterlerini Spark işlerini çalıştırmak için Spark kümelerinde kullanmayı öğrenin](spark/apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Windows üzerinde Linux tabanlı araçlar ve teknolojiler çalıştırın

Burada bir aracı veya yalnızca Linux üzerinde kullanılabilir teknolojisi kullanmalıdır bir durumla karşılaşırsanız, aşağıdaki seçenekleri göz önünde bulundurun:

* **Windows 10 üzerinde bash (beta)** Windows üzerinde Linux alt sistemi sağlar. Bash doğrudan adanmış bir Linux yüklemesi korumak zorunda kalmadan Linux yardımcı programları çalıştırmanıza olanak tanır. [Yükleyin ve Windows 10 üzerinde Bash beta çalıştırın](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Windows için docker** çok sayıda Linux tabanlı araçlar erişim sağlar ve doğrudan Windows çalıştırılabilir. Örneğin, Beeline istemci Hive için doğrudan Windows çalıştırmak için Docker kullanabilirsiniz. Yerel Jupyter not defteri çalıştırmak için de Docker'ı kullanma ve HDInsight üzerinde Spark uzaktan bağlanın. [Docker için Windows ile çalışmaya başlama](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](http://mobaxterm.mobatek.net/)**  grafiksel bir SSH bağlantısı üzerinden küme dosya sistemine göz atmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Linux tabanlı kümelerde çalışmaya yeni başladıysanız aşağıdaki makalelere bakın:
* [Hadoop, Kafka, Spark veya diğer kümeler ayarlama](hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight kümelerinde Linux için ipuçları](hdinsight-hadoop-linux-information.md)
