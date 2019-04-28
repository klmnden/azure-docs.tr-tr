---
title: Bir Windows PC HDInsight - Azure üzerinde Hadoop ile kullanma
description: Bir Windows PC hadoop'ta HDInsight üzerinde çalışabilirsiniz. Sorgu Linux PowerShell ve Visual Studio Araçları ile kümeleri ve yönetin. .NET ile büyük veri çözümleri geliştirin.
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 05/17/2017
ms.openlocfilehash: 4d9d1ef6b7906ecebc399948a1ca0dcd590d5910
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63765823"
---
# <a name="work-in-the-apache-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Bir Windows bilgisayardan HDInsight üzerinde Apache Hadoop ekosistemindeki çalışma

Geliştirme ve HDInsight üzerinde Apache Hadoop ekosistemindeki çalışmak için Windows PC yönetimi seçenekleri hakkında bilgi edinin. 

HDInsight Apache Hadoop ve Hadoop bileşenleri, Linux üzerinde geliştirilen bir açık kaynak teknolojilerini temel alır. HDInsight sürüm 3.4 ve üzeri, Ubuntu Linux dağıtımı küme için temel işletim sistemi kullanır. Ancak, bir Windows istemci ya da Windows geliştirme ortamı kullanarak HDInsight ile çalışabilirsiniz.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Dağıtım ve yönetim görevleri için PowerShell kullanma
Azure PowerShell, denetlemek ve dağıtım ve HDInsight Windows gelen yönetim görevleri otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır.

PowerShell ile yapmak görev örnekleri:

* [PowerShell kullanarak küme oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).
* [PowerShell kullanarak Apache Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-powershell.md).
* [PowerShell ile küme yönetme](hdinsight-administer-use-powershell.md).

Adımlarını izleyin [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-az-ps) en son sürümünü almak için. Azure Resource Manager için yeni cmdlet'ler kullanmak üzere değiştirilmesi için gereken komut dosyalarınız varsa [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçiş](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Yardımcı programlar bir tarayıcıda çalıştırabilirsiniz.
Aşağıdaki yardımcı programlar, bir web tarayıcıda çalışan kullanıcı Arabirimi vardır:
* **[Azure Cloud Shell (Önizleme)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  tarayıcınızda ve içinden çalışır bir etkileşimli, komut satırı kabuğu olan Azure portalı.
* **[Apache Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md)**  yönetim ve izleme yardımcı programı gibi çeşitli işlemler yönetmek için kullanılan Azure portalında kullanılabilir:
    * [Apache Ambari REST API ile kullanma](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Görünüm Apache Ambari, Apache Hive](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [Apache Ambari, Apache Tez görüntüle](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Visual Studio için Data Lake (Hadoop) araçları
Storm topolojilerini dağıtma ve yönetme için Visual Studio için Data Lake Araçları'nı kullanın. Data Lake araçları, SCP.NET Visual Studio ile C# Storm topolojileri geliştirme olanak tanıyan SDK'yı da yükler.

Aşağıdaki örnekler için geçmeden önce [yükleyin ve Visual Studio için Data Lake araçları deneyin](hadoop/apache-hadoop-visual-studio-tools-get-started.md). 

Visual Studio için Data Lake araçları ile Visual Studio ile gerçekleştirebileceğiniz görevleri örnekleri:
* [Storm topolojilerini Visual Studio'dan yönetin ve dağıtın](storm/apache-storm-deploy-monitor-topology-linux.md)
* [Visual Studio kullanarak Storm için C# topolojileri geliştirme](storm/apache-storm-develop-csharp-visual-studio-topology.md). BITS, Azure Cosmos DB ile SQL veritabanı gibi veritabanlarına bağlanabilirsiniz Storm topolojilerini örnek şablonları içerir.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio ve .NET SDK'sı 

Visual Studio, .NET SDK'sı ile kümeleri yönetme ve büyük veri uygulamaları geliştirmek için kullanabilirsiniz. Aşağıdaki görevler için diğer Ide'leri kullanabilirsiniz, ancak Visual Studio'da örnekler gösterilmektedir.

Visual Studio'da .NET SDK'sı ile gerçekleştirebileceğiniz görevler örnekleri:
* [Kümeleri oluşturma ve bir .NET Framework uygulamasından HDInsight içinde iş](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md).
* [.NET SDK kullanarak Apache Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md).
* [Kullanım C# Apache Hive ve Apache Hadoop akış Apache Pig ile kullanıcı tanımlı işlevleri](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md).

> [!TIP]
> .NET çözümlerini Windows tabanlı HDInsight kümeleriyle çalıştırıyorsanız, Linux tabanlı kümeler için bir geçiş planı için iyi bir zamandır. Daha fazla bilgi için [geçirme .NET çözüm için Linux tabanlı HDInsight için Windows tabanlı HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij Idea ve Eclipse IDE için Spark kümeleri
Her ikisi de [Intellij Idea](https://www.jetbrains.com/idea/download) ve [Eclipse IDE'yi](https://www.eclipse.org/downloads/) yapmak için kullanılabilir:
* Geliştirin ve bir HDInsight Spark kümesi üzerinde bir Scala Spark uygulaması gönderin.
* Spark kümesi kaynaklarınıza erişin.
* Geliştirin ve yerel olarak Scala Spark uygulamasını çalıştırın.

Bu makaleler Göster nasıl: 
* Intellij IDEA: [Eklenti Intellij için Azure araç takımı ve Scala SDK'sını kullanarak Apache Spark uygulamaları oluşturun.](spark/apache-spark-intellij-tool-plugin.md)
* IDE veya Scala IDE için Eclipse eclipse: [Apache Spark uygulamaları ve Eclipse için Azure araç takımı oluşturun](spark/apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Not defterlerini Spark üzerinde veri uzmanları için 
Apache Spark kümeleri, HDInsight, Apache Zeppelin not defterlerini ve Jupyter not defterleri ile kullanılabilen çekirdekler içerir. 

* [Çekirdekler üzerinde Apache Spark kümeleri ile Jupyter not defterlerini Spark uygulamaları test etmek için nasıl kullanılacağını öğrenin](spark/apache-spark-zeppelin-notebook.md)
* [Spark işlerini çalıştırmak için Apache Spark kümeleri üzerinde Apache Zeppelin not defterlerini kullanma hakkında bilgi edinin](spark/apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Windows üzerinde Linux tabanlı araçlar ve teknolojiler çalıştırın

Burada bir aracı veya yalnızca Linux üzerinde kullanılabilir teknolojisi kullanmalıdır bir durumla karşılaşırsanız, aşağıdaki seçenekleri göz önünde bulundurun:

* **Windows 10 üzerinde bash (beta)** Windows üzerinde Linux alt sistemi sağlar. Bash doğrudan adanmış bir Linux yüklemesi korumak zorunda kalmadan Linux yardımcı programları çalıştırmanıza olanak tanır. [Yükleyin ve Windows 10 üzerinde Bash beta çalıştırın](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Windows için docker** çok sayıda Linux tabanlı araçlar erişim sağlar ve doğrudan Windows çalıştırılabilir. Örneğin, Beeline istemci Hive için doğrudan Windows çalıştırmak için Docker kullanabilirsiniz. Yerel Jupyter not defteri çalıştırmak için de Docker'ı kullanma ve HDInsight üzerinde Spark uzaktan bağlanın. [Docker için Windows ile çalışmaya başlama](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](https://mobaxterm.mobatek.net/)**  grafiksel bir SSH bağlantısı üzerinden küme dosya sistemine göz atmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Linux tabanlı kümelerde çalışmaya yeni başladıysanız aşağıdaki makalelere bakın:
* [Apache Hadoop, Apache Kafka, Apache Spark veya diğer kümeler ayarlama](hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight kümelerinde Linux için ipuçları](hdinsight-hadoop-linux-information.md)
