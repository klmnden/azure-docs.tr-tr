---
title: Komut satırı-Azure HDInsight'ı kullanarak Hadoop kümeleri oluşturma
description: Platformlar arası Azure CLI 1.0 kullanarak HDInsight kümeleri oluşturmayı öğrenin.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: 523c2a85929d8474c283055a8ae38d489cbd4b12
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090983"
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Azure CLI kullanarak HDInsight kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu belgeyi kılavuz Azure CLI 1.0 kullanarak bir HDInsight 3.5 kümesi oluşturma adımları.

> [!IMPORTANT]
> Bu konuda, bir HDInsight kümesi oluşturmak için Azure CLI 1.0 kullanmayı açıklar. CLI'ın bu sürümü kullanım dışı ve Destek HDInsight kümelerini oluşturmak için Azure CLI 2.0 ile eklenmedi.
>
> HDInsight küme oluşturmak ve yönetmek için Azure PowerShell de kullanabilirsiniz. Daha fazla bilgi için [oluşturma HDInsight kümeleri Azure PowerShell kullanarak](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) belge.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI**. Bu belgedeki adımlarda en son Azure CLI Sürüm 0.10.14 ile test edilmiştir.

    > [!IMPORTANT]
    > Azure CLI 1.0 kullanım dışıdır ve HDInsight kümeleri oluşturma desteği, Azure CLI 2.0 ile eklenmedi.

## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

[Azure Komut Satırı Arabirimi'nden (Azure CLI) bir Azure aboneliğine bağlanma](/cli/azure/authenticate-azure-cli) konusunda belgelenen adımları izleyin ve **oturum açma** yöntemini kullanarak aboneliğinze bağlanın.

## <a name="create-a-cluster"></a>Küme oluşturma

PowerShell veya Bash gibi bir komut satırından aşağıdaki adımlar gerçekleştirilmelidir.

1. Azure aboneliğiniz için kimlik doğrulaması yapmak için aşağıdaki komutu kullanın:

        azure login

    Adı ve parola sağlamanız istenir. Birden çok Azure aboneliğiniz varsa, `azure account set <subscriptionname>` Azure CLI konutlarını kullanır aboneliği ayarlamak için.

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

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI kullanarak bir HDInsight kümesi başarıyla oluşturuldu, kümenizi ile çalışma hakkında bilgi almak için aşağıdakileri kullanın:

### <a name="hadoop-clusters"></a>Hadoop kümeleri

* [HDInsight ile Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase kümeleri

* [HDInsight üzerinde HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm kümeleri

* [HDInsight üzerinde Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Storm Python bileşenlerini kullanın](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Storm topolojileri dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)
