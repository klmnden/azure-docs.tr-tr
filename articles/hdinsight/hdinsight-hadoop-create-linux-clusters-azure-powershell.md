---
title: "PowerShell - Azure Hdınsight kullanarak Hadoop kümeleri oluşturma | Microsoft Docs"
description: "Hadoop, HBase, Storm ve Spark kümeleri Linux'ta Hdınsight için Azure PowerShell kullanarak nasıl oluşturulacağını öğrenin."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: adfc1158a907156ffddd27cd4eabc25c81930476
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Azure PowerShell kullanarak Hdınsight'ta Linux tabanlı kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell denetlemek ve dağıtımını ve yönetimini Microsoft Azure, iş yüklerini otomatikleştirmek için kullanabileceğiniz güçlü bir komut dosyası ortamıdır. Bu belge, Azure PowerShell kullanarak Linux tabanlı Hdınsight kümesi oluşturma hakkında bilgi sağlar. Ayrıca, bir örnek komut dosyası içerir.

> [!NOTE]
> Azure PowerShell yalnızca Windows istemcileri üzerinde kullanılabilir. Linux, Unix ya da Mac OS X istemci kullanıyorsanız, bkz: [Azure CLI kullanarak bir Linux tabanlı Hdınsight kümesi oluşturma](hdinsight-hadoop-create-linux-clusters-azure-cli.md) bir küme oluşturmak için Azure CLI kullanma hakkında bilgi.

## <a name="prerequisites"></a>Önkoşullar
Bu yordama başlamadan önce şunlara sahip olmanız gerekir:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışı bırakılmış** ve 1 Ocak 2017 tarihinde kaldırılmıştır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Lütfen adımları [Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Azure PowerShell'in en son sürümünü yüklemek için. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="create-cluster"></a>Küme oluşturma

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Azure PowerShell kullanarak bir Hdınsight kümesi oluşturmak için aşağıdaki yordamları tamamlamanız gerekir:

* Bir Azure kaynak grubu oluşturun
* Azure Depolama hesabı oluşturma
* Bir Azure Blob kapsayıcı oluşturun
* Hdınsight kümesi oluşturma

Aşağıdaki komut dosyasını yeni bir küme oluşturmak nasıl gösterir:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

Küme oturum açma için belirttiğiniz değerleri kümesi için Hadoop kullanıcı hesabı oluşturmak için kullanılır. Web Uı'lar veya REST API'leri gibi küme üzerinde barındırılan hizmetlere bağlanmak için bu hesabı kullanın.

SSH kullanıcı için belirlediğiniz değerler, küme için SSH kullanıcı oluşturmak için kullanılır. Küme üzerinde uzak SSH oturumu başlatmak ve işlerini çalıştırmak için bu hesabı kullanın. Daha fazla bilgi için [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

> [!IMPORTANT]
> 32'den fazla alt düğüm (ya da küme oluşturma veya Küme oluşturulduktan sonra ölçeklendirme tarafından) kullanmayı planlıyorsanız, ayrıca bir baş düğüm boyutu, en az 8 çekirdek ve 14 GB RAM belirtmeniz gerekir.
>
> Düğümü boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

Bir küme oluşturmak için 20 dakika sürebilir.

## <a name="create-cluster-configuration-object"></a>Küme oluşturma: Yapılandırma nesnesi

Ayrıca bir Hdınsight yapılandırma nesnesi kullanarak oluşturabileceğiniz `New-AzureRmHDInsightClusterConfig` cmdlet'i. Kümeniz için ek yapılandırma seçeneklerini etkinleştirmek için bu yapılandırma nesnesi daha sonra değiştirebilirsiniz. Son olarak, `-Config` parametresinin `New-AzureRmHDInsightCluster` yapılandırmasını kullanmak için cmdlet.

Aşağıdaki betiği Hdınsight küme türü üzerinde bir R Server yapılandırmak için bir yapılandırma nesnesi oluşturur. Yapılandırma, bir edge düğümünü, Rstudio'dan ve ek depolama alanı hesabı etkinleştirir.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> Hdınsight kümesi farklı bir konumda bir depolama hesabıyla desteklenmiyor. Bu örnek kullanırken, sunucu ile aynı konumda ek depolama alanı hesabı oluşturun.

## <a name="customize-clusters"></a>Kümeleri özelleştirme

* Bkz: [önyükleme kullanarak özelleştirme Hdınsight kümelerini](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight kümesi başarıyla oluşturuldu, kümenizi ile çalışmayı öğrenmek için aşağıdaki kaynakları kullanın.

### <a name="hadoop-clusters"></a>Hadoop kümeleri

* [HDInsight ile Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hadoop/hdinsight-use-pig.md)
* [Hdınsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase kümeleri

* [HDInsight üzerinde HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Hdınsight'ta HBase için Java uygulamaları geliştirme](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm kümeleri

* [Hdınsight üzerinde Storm için Java topolojisi geliştirme](storm/apache-storm-develop-java-topology.md)
* [Hdınsight üzerinde Storm Python bileşenleri kullanma](storm/apache-storm-develop-python-topology.md)
* [Dağıtma ve hdınsight'ta Storm topolojileri izleme](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark kümeleri

* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)

