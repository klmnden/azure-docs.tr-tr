---
title: Azure Klasik CLI - Azure HDInsight'ı kullanarak Apache Hadoop kümeleri oluşturma
description: Platformlar arası Azure Klasik CLI kullanarak HDInsight kümeleri oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 21985b009694dc5a21c65d4c9dc9536cf6c01a0e
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128728"
---
# <a name="create-hdinsight-clusters-using-the-azure-classic-cli"></a>Klasik Azure CLI kullanarak HDInsight kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu belgeyi kılavuz Klasik Azure CLI kullanarak bir HDInsight 3.5 kümesi oluşturma adımları.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure Klasik CLI**. Bu belgedeki adımlarda en son Azure Klasik CLI sürümü 0.10.14 ile test edilmiştir.

## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

Konusunda belgelenen adımları [bir Azure aboneliğine bağlanma Azure komut satırı Arabirimi'nden](/cli/azure/authenticate-azure-cli) ve kullanarak aboneliğinze bağlanın **oturum açma** yöntemi.

## <a name="create-a-cluster"></a>Küme oluşturma

PowerShell veya Bash gibi bir komut satırından aşağıdaki adımlar gerçekleştirilmelidir.

1. Azure aboneliğiniz için kimlik doğrulaması yapmak için aşağıdaki komutu kullanın:

        azure login

    Adı ve parola sağlamanız istenir. Birden çok Azure aboneliğiniz varsa, `azure account set <subscriptionname>` Klasik CLI konutlarını kullanır aboneliği ayarlamak için.

2. Şu komutu kullanarak Azure Resource Manager moduna geçin:

        azure config mode arm

3. Bir kaynak grubu oluşturun. Bu kaynak grubu HDInsight kümesini içerir ve ilişkili depolama hesabı.

        azure group create groupname location

    * Değiştirin `groupname` grubu için benzersiz bir ada sahip.

    * Değiştirin `location` grup içinde oluşturmak istediğiniz coğrafi bölgede ile.

       Geçerli konumlar listesi için kullanmak `azure location list` komutunu ve ardından konumlardan birini kullanın `Name` sütun.

4. Depolama hesabı oluşturma. Bu depolama hesabı, HDInsight kümesi için varsayılan depolama alanı olarak kullanılır.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Değiştirin `groupname` önceki adımda oluşturulan grup adı.

    * Değiştirin `location` önceki adımda kullanılan aynı konuma sahip.

    * Değiştirin `storagename` depolama hesabı için benzersiz bir ada sahip.

        > [!NOTE]  
        > Bu komutta kullanılan parametreler hakkında daha fazla bilgi için `azure storage account create -h` bu komut için Yardım görüntülemek için.

5. Depolama hesabına erişmek için kullanılan anahtarı alın.

        azure storage account keys list -g groupname storagename

    * Değiştirin `groupname` kaynak grubu adına sahip.
    * Değiştirin `storagename` depolama hesabı adı ile.

      Döndürülen verilerde Kaydet `key` değerini `key1`.

6. Bir HDInsight kümesi oluşturun.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Değiştirin `groupname` kaynak grubu adına sahip.

    * Değiştirin `Hadoop` oluşturmak istediğiniz küme türüne sahip. Örneğin, `Hadoop`, `HBase`, `Kafka`, `Spark`, veya `Storm`.

      > [!IMPORTANT]  
      > HDInsight kümeleri, çeşitli türleri, küme için ayarlanan teknoloji ve iş yükü karşılık gelen. Bir küme üzerinde Storm ve HBase gibi birden birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur.

    * Değiştirin `location` önceki adımlarda kullandığınız konumun aynısını ile.

    * Değiştirin `storagename` ile depolama hesabı adı.

    * Değiştirin `storagekey` önceki adımda elde edilen anahtarına sahip.

    * İçin `--defaultStorageContainer` parametre, kullanım, aynı adı küme için kullanma.

    * Değiştirin `admin` ve `httppassword` adı ve parola HTTPS kümeye erişirken kullanmak istediğiniz.

    * Değiştirin `sshuser` ve `sshuserpassword` kullanıcı adı ve SSH kullanarak kümeye erişirken kullanmak için parola

      > [!IMPORTANT]  
      > Bu örnekte, iki çalışan düğümleri ile bir küme oluşturur. Çalışan düğümü sayısı ayrıca Küme oluşturulduktan sonra ölçeklendirme işlemleri gerçekleştirerek de değiştirebilirsiniz. 32'den fazla çalışan düğümleri kullanmayı planlıyorsanız, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir. Baş düğüm boyutunu kullanarak ayarlayabilirsiniz `--headNodeSize` küme oluşturma sırasında parametre.
      >
      > Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).
      
      Bu, küme oluşturma işleminin tamamlanması birkaç dakika sürebilir. Genellikle yaklaşık 15.

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Klasik CLI kullanarak bir HDInsight kümesi başarıyla oluşturuldu, kümenizi ile çalışma hakkında bilgi almak için aşağıdakileri kullanın:

### <a name="apache-hadoop-clusters"></a>Apache Hadoop kümelerini

* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri

* [HDInsight üzerinde Apache HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm kümeleri

* [HDInsight üzerinde Apache Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Apache Storm, Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Apache Storm topolojileri dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)
