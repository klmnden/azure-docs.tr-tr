---
title: Bir Windows PC Hdınsight - Azure Hadoop ile kullanma | Microsoft Docs
description: Bir Windows PC hadoop'ta Hdınsight üzerinde çalışır. PowerShell, Visual Studio ve Linux araçlarıyla sorgu kümeleri ve yönetin. .NET ile büyük veri çözümleri geliştirin.
services: hdinsight
keywords: windows, hadoop'ta windows için hadoop
author: cjgronlund
manager: jhubbard
ms.author: cgronlun
ms.date: 05/17/2017
ms.topic: article
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.openlocfilehash: c9e4476334db95c66650f663dc3d8d13df2c5b52
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="work-in-the-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Bir Windows PC hdınsight'ta Hadoop ekosistemi ile çalışma

Geliştirme ve hdınsight'ta Hadoop ekosistemine çalışmak için Windows bilgisayarda yönetimi seçenekleri hakkında bilgi edinin. 

Hdınsight Apache Hadoop ve Hadoop bileşenleri, açık kaynaklı teknolojileri Linux'ta geliştirilen temel alır. Hdınsight sürüm 3.4 ve daha yüksek Ubuntu Linux dağıtımı küme için temel işletim sistemi kullanır. Ancak, bir Windows istemci ya da Windows geliştirme ortamı Hdınsight ile çalışabilirsiniz.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Dağıtım ve yönetim görevleri için PowerShell kullanın
Azure PowerShell denetlemek ve dağıtım ve Windows'dan hdınsight'ta yönetim görevlerini otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır.

PowerShell ile gerçekleştirebileceğiniz görevleri örnekleri:

* [PowerShell kullanarak kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [PowerShell kullanarak Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-powershell.md)
* [PowerShell ile kümelerini yönetme](hdinsight-administer-use-powershell.md)

Adımlarını izleyin [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) en son sürümünü almak için. Yeni cmdlet'leri Azure Resource Manager için bkz için değiştirilmesi gereken komut dosyalarınız varsa [Hdınsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçirme](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Bir tarayıcıda çalıştırmak yardımcı programları
Aşağıdaki yardımcı programlar bir web tarayıcıda çalışan kullanıcı Arabirimi vardır:
* **[Azure bulut Kabuğu (Önizleme)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  tarayıcınızda ve içinden çalışır bir etkileşimli, komut satırı kabuğu olan Azure portalı.
* **[Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md)**  bir yönetim ve izleme yardımcı programı işleri, farklı türde gibi yönetmek için kullanılan Azure portalında kullanılabilir:
    * [Ambari REST API ile kullanma](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Ambari Hive görünümünü](hadoop/apache-hadoop-use-hive-ambari-view.md)
    * [Ambari Tez görünümünde](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Visual Studio için Data Lake (Hadoop) araçları
Visual Studio için Data Lake araçları, dağıtmak ve Storm topolojilerini yönetmek için kullanın. Data Lake araçları, Visual Studio ile C# Storm topolojileri geliştirme sayesinde SCP.NET SDK de yükler.

Aşağıdaki örneklerde, devam etmeden önce [yükleyin ve Visual Studio için Data Lake araçları deneyin](hadoop/apache-hadoop-visual-studio-tools-get-started.md). 

Visual Studio için Visual Studio ve Data Lake araçları ile gerçekleştirebileceğiniz görevleri örnekleri:
* [Dağıtma ve Storm topolojilerini Visual Studio'dan yönetme](storm/apache-storm-deploy-monitor-topology-linux.md)
* [Visual Studio kullanarak Storm için C# topolojileri geliştirme](storm/apache-storm-develop-csharp-visual-studio-topology.md). BITS Storm topolojilerini Azure Cosmos DB ve SQL veritabanı gibi veritabanlarına bağlanmak için örnek şablonları içerir.

## <a name="visual-studio-and-the-net-sdk"></a>Visual Studio ve .NET SDK'sı 

Kümeleri yönetmek ve büyük veri uygulamaları geliştirmek için Visual Studio .NET SDK'sı ile kullanabilirsiniz. Aşağıdaki görevler için diğer IDE kullanabilirsiniz, ancak Visual Studio örnekleri gösterilir.

Visual Studio'da .NET SDK'sı yerine getirebileceğiniz görevler örnekleri:
* [Veri kümeleri oluşturma ve bir .NET Framework uygulamasından Hdınsight'ta çalışma](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [.NET SDK kullanarak Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md)
* [C# kullanıcı tanımlı işlevler Hive veya Pig Hadoop akış ile kullanma](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

> İpucu Windows tabanlı Hdınsight kümeleri ile .NET çözümleri çalıştırıyorsanız, Linux tabanlı kümeler için geçiş planı için iyi bir zamandır değil. Daha fazla bilgi için bkz: [Windows tabanlı Hdınsight Linux tabanlı hdınsight'a geçirmek .NET çözümü](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij Idea ve Spark kümeleri için Eclipse IDE
Her ikisi de [Intellij Idea](https://www.jetbrains.com/idea/download) ve [Eclipse IDE](https://www.eclipse.org/downloads/) için kullanılabilir:
* Geliştirme ve Hdınsight Spark kümesi üzerinde bir Scala Spark uygulamasının gönderin.
* Spark küme kaynaklarını erişin.
* Geliştirme ve Scala Spark uygulama yerel olarak çalıştırın.

Bu makaleler Göster nasıl: 
* Intellij Idea: [Intellij eklenti için Azure araç seti ile Scala SDK'sı kullanarak Oluştur Spark uygulamaları.](spark/apache-spark-intellij-tool-plugin.md)
* IDE veya Scala IDE Eclipse için eclipse: [oluşturma Spark uygulamaları ve Eclipse için Azure Araç Seti](spark/apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Spark üzerinde veri bilimcilerine dizüstü bilgisayarlar 
Hdınsight'ta Apache Spark kümeleri Zeppelin not defterlerini ve Jupyter not defterleri ile kullanılabilir çekirdekler içerir. 

* [Tekrar Spark kümeleri Jupyter not defterleri ile üzerinde Spark uygulamaları test etmek için nasıl kullanacağınızı öğrenin](spark/apache-spark-zeppelin-notebook.md)
* [Spark işlerini çalıştırmak için Spark kümelerine Zeppelin not defterlerini kullanmayı öğrenin](spark/apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Linux tabanlı araçlar ve teknolojiler Windows'ta çalıştırma

Burada bir araç veya yalnızca Linux üzerinde kullanılabilir teknolojisi kullanmalıdır bir durumla karşılaşırsanız, aşağıdaki seçenekleri göz önünde bulundurun:

* **Windows 10 (beta) bash** Windows'da Linux alt sistemi sağlar. Bash doğrudan adanmış bir Linux yüklemesi bakımını yapmak zorunda kalmadan Linux yardımcı programları çalıştırmanıza olanak sağlar. [Yükleyin ve Windows 10 Bash beta çalıştırın](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Windows için docker** birçok Linux tabanlı araçlar erişim sağlar ve doğrudan Windows çalıştırabilirsiniz. Örneğin, doğrudan Windows Hive Beeline istemci çalıştırmak için Docker kullanabilirsiniz. Hdınsight'ta Spark uzaktan bağlanmak ve ayrıca yerel bir Jupyter not defteri çalıştırmak için Docker kullanın. [Docker için Windows ile çalışmaya başlama](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](http://mobaxterm.mobatek.net/)**  grafiksel bir SSH bağlantısı üzerinden küme dosya sistemi göz atmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Linux tabanlı kümelerde çalışmaya yeniyseniz, izleme makalelere bakın:
* [Hadoop, Kafka, Spark veya diğer kümeleri Kur](hdinsight-hadoop-provision-linux-clusters.md)
* [Linux'ta Hdınsight kümeleri için ipuçları](hdinsight-hadoop-linux-information.md)