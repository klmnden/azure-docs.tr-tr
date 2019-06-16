---
title: Azure HDInsight’ta güvenli aktarım depolama hesapları ile Hadoop kümesi oluşturma
description: Güvenli aktarım özellikli Azure depolama hesapları ile HDInsight kümeleri oluşturma hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/24/2018
ms.openlocfilehash: 10ec4b55bab741f19adaf193295659b7876fe02c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64685214"
---
# <a name="create-apache-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Apache Hadoop kümesi ile güvenli aktarım depolama hesapları Azure HDInsight oluşturma

[Güvenli aktarım gereklidir](../storage/common/storage-require-secure-transfer.md) özelliği, güvenli bir bağlantı üzerinden tüm istekleri hesabınıza uygulayarak Azure Depolama hesabınızın güvenliğini artırır. Bu özellik ve wasbs şeması yalnızca HDInsight kümesi 3.6 veya sonraki sürümlerde desteklenir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce

* **Azure aboneliği**: Bir aylık ücretsiz bir deneme hesabı oluşturmak için Gözat [azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Güvenli aktarım özellikli bir Azure depolama hesabı**. Yönergeler için bkz. [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md) ve [Güvenli aktarım isteme](../storage/common/storage-require-secure-transfer.md).
* **Depolama hesabındaki bir Blob kapsayıcı**.

## <a name="create-cluster"></a>Küme oluşturma

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Bu bölümde, [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-template-deploy.md) kullanarak HDInsight'ta Hadoop kümesi oluşturacaksınız. Şablonuna [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). Bu öğreticiyi kullanmak için Resource Manager şablonuyla deneyim sahibi olmak gerekli değildir. Diğer küme oluşturma yöntemleri ve bu öğreticide kullanılan özellikler hakkında bilgi edinmek için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

1. Aşağıdaki resme tıklayarak Azure'da oturum açın ve Azure portalında Resource Manager şablonunu açın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Aşağıdaki özelliklerle kümeyi oluşturmak için yönergeleri izleyin: 

    - HDInsight sürümü 3.6’yı belirtin. 3\.6 veya daha yeni bir sürüm gereklidir.
    - Güvenli aktarım özellikli bir depolama hesabı belirtin.
    - Depolama hesabı için kısa bir ad kullanın.
    - Hem depolama hesabı hem de blob kapsayıcı önceden oluşturulmalıdır.

      Yönergeler için bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).

Kendi yapılandırma dosyalarınızı sağlamak için betik eylemi kullanıyorsanız, aşağıdaki ayarlarda wasbs kullanmanız gerekir:

- fs.defaultFS (çekirdek-site)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Başka depolama hesapları ekleme

Güvenli aktarım özellikli başka depolama hesapları eklemek için birkaç seçenek vardır:

- Son bölümdeki Azure Resource Manager şablonunu değiştirin.
- [Azure portalını](https://portal.azure.com) kullanarak bir küme oluşturun ve bağlı depolama hesabını belirtin.
- Var olan bir HDInsight kümesine güvenli aktarım özellikli başka depolama hesapları eklemek için betik eylemini kullanın. Daha fazla bilgi için bkz. [HDInsight’a başka depolama hesapları ekleme](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide bir HDInsight kümesi oluşturmayı ve depolama hesaplarına güvenli aktarımı etkinleştirmeyi öğrendiniz.

HDInsight ile veri çözümleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Kullanma hakkında daha fazla bilgi edinmek için [Apache Hive](https://hive.apache.org/) Visual Studio'dan Hive sorguları gerçekleştirme dahil, HDInsight ile bkz [HDInsight ile Hive kullanma Apache][hdinsight-use-hive].
* Hakkında bilgi edinmek için [Apache Pig](https://pig.apache.org/), verileri dönüştürmek için kullanılan bir dil bakın [HDInsight ile Apache Pig kullanma][hdinsight-use-pig].
* Hakkında bilgi edinmek için [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), hadoop'ta verileri işleyen programları yazmanın bir yöntemi bkz [HDInsight ile Apache Hadoop MapReduce kullanma][hdinsight-use-mapreduce].
* HDInsight üzerinde verileri çözümlemek için Visual Studio için HDInsight araçları kullanma hakkında bilgi edinmek için [HDInsight için Visual Studio Apache Hadoop araçlarını kullanmaya başlama](hadoop/apache-hadoop-visual-studio-tools-get-started.md).

HDInsight’ın verileri nasıl depoladığı veya HDInsight’a verilerin nasıl alındığı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* HDInsight’ın Azure Depolama’yı nasıl kullandığı hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Depolama kullanma](hdinsight-hadoop-use-blob-storage.md).
* HDInsight’a veril yükleme hakkında daha fazla bilgi için bkz. [Verileri HDInsight’a yükleme][hdinsight-upload-data].

HDInsight kümesi oluşturma ve yönetme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Linux tabanlı HDInsight kümenizi yönetme hakkında daha fazla bilgi için bkz: [Apache Ambari kullanarak HDInsight yönetme kümelerini](hdinsight-hadoop-manage-ambari.md).
* HDInsight kümesi oluştururken tercih edebileceğiniz seçenekler hakkında daha fazla bilgi için bkz. [Özel seçenekleri kullanarak Linux’ta HDInsight oluşturma](hdinsight-hadoop-provision-linux-clusters.md).
* Linux ve Apache Hadoop ile ilgili bilgi sahibi olduğunuz, ancak Hadoop'a HDInsight ilişkin teknik özellikleri öğrenmek istiyorsanız, bkz. [Linux'ta HDInsight ile çalışma](hdinsight-hadoop-linux-information.md). Bu makale aşağıdaki gibi bilgiler sağlar:

  * Gibi kümede barındırılan hizmetler için URL'leri [Apache Ambari](https://ambari.apache.org/) ve [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat)
  * Konumunu [Apache Hadoop](https://hadoop.apache.org/) dosyalarının ve örneklerin yerel dosya sisteminde
  * ' % S'kullanmak, Azure Storage (WASB) yerine [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) varsayılan veri depolama

[1]: ../HDInsight/hadoop/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
