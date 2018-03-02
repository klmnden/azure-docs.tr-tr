---
title: "Komut satırı-Azure Hdınsight kullanarak Hadoop kümeleri oluşturma | Microsoft Docs"
description: "Platformlar arası Azure CLI 1.0 kullanarak Hdınsight kümelerini oluşturmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: 983e167d248d711efde9c64a70f59d5a9e81769a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Azure CLI kullanarak Hdınsight kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure CLI 1.0 kullanarak Hdınsight 3.5 küme oluşturma bu belgeyi gözden geçirme adımları.

> [!IMPORTANT]
> Bu konu, Hdınsight kümesi oluşturmak için Azure CLI 1.0 kullanmayı açıklar. CLI'ın bu sürümü kullanım dışıdır ve Hdınsight kümeleri oluşturma desteği için Azure CLI 2.0 eklenmedi.
>
> Azure PowerShell oluşturmak ve Hdınsight kümeleri yönetmek için de kullanabilirsiniz. Daha fazla bilgi için bkz: [Hdınsight kümeleri oluşturma Azure PowerShell kullanarak](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) belge.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI**. Bu belgede yer alan adımlar, en son Azure CLI Sürüm 0.10.14 test edilmiş.

    > [!IMPORTANT]
    > Azure CLI 1.0 kullanım dışıdır ve Hdınsight kümeleri oluşturma desteği için Azure CLI 2.0 eklenmedi.

## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

[Azure Komut Satırı Arabirimi'nden (Azure CLI) bir Azure aboneliğine bağlanma](/cli/azure/authenticate-azure-cli) konusunda belgelenen adımları izleyin ve **oturum açma** yöntemini kullanarak aboneliğinze bağlanın.

## <a name="create-a-cluster"></a>Küme oluşturma

Aşağıdaki adımlar, PowerShell veya Bash gibi bir komut satırından gerçekleştirilmelidir.

1. Azure aboneliğinize kimliğini doğrulamak için aşağıdaki komutu kullanın:

        azure login

    Adınızı ve parolanızı girmeniz istenir. Birden çok Azure aboneliğiniz varsa, kullanmak `azure account set <subscriptionname>` Azure CLI komutları kullanın aboneliği ayarlamak için.

2. Şu komutu kullanarak Azure Resource Manager moduna geçin:

        azure config mode arm

3. Bir kaynak grubu oluşturun. Bu kaynak grubu ve depolama hesabı ilişkili Hdınsight kümesi içerir.

        azure group create groupname location

    * Değiştir `groupname` grubu için benzersiz bir ada sahip.

    * Değiştir `location` grubunda oluşturmak istediğiniz coğrafi bölge ile.

       Geçerli konumların bir listesini için kullanmak `azure location list` komut ve konumlardan birini kullanın `Name` sütun.

4. Bir depolama hesabı oluşturun. Bu depolama hesabı Hdınsight kümesi için varsayılan depolama alanı olarak kullanılır.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Değiştir `groupname` önceki adımda oluşturulan grup adı.

    * Değiştir `location` önceki adımda kullanılan aynı konum.

    * Değiştir `storagename` depolama hesabı için benzersiz bir ada sahip.

        > [!NOTE]
        > Bu komutta kullanılan parametreler hakkında daha fazla bilgi için kullanmak `azure storage account create -h` bu komut için Yardım görüntülemek için.

5. Depolama hesabına erişmek için kullanılan anahtarı alır.

        azure storage account keys list -g groupname storagename

    * Değiştir `groupname` kaynak grubu adı.
    * Değiştir `storagename` depolama hesabı adı.

     Döndürülen verilerin Kaydet `key` değerini `key1`.

6. Hdınsight kümesi oluşturun.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Değiştir `groupname` kaynak grubu adı.

    * Değiştir `Hadoop` oluşturmak istediğiniz küme türüne sahip. Örneğin, `Hadoop`, `HBase`, `Kafka`, `Spark`, veya `Storm`.

     > [!IMPORTANT]
     > Hdınsight kümeleri gelen iş yükü veya küme için ayarlanmış teknoloji karşılık çeşitli türlerde. Bir küme üzerinde Storm ve HBase gibi birden çok tür birleştiren bir küme oluşturmak için desteklenen yöntem yoktur.

    * Değiştir `location` önceki adımlarda kullanılan aynı konum.

    * Değiştir `storagename` depolama hesabı adı ile.

    * Değiştir `storagekey` önceki adımda elde anahtara sahip.

    * İçin `--defaultStorageContainer` parametresi, kullanım, aynı adı küme için kullanma.

    * Değiştir `admin` ve `httppassword` adı ve parolayla istediğiniz küme HTTPS erişirken kullanılacak.

    * Değiştir `sshuser` ve `sshuserpassword` kullanıcı adı ve SSH kullanarak kümeye erişirken kullanmak istediğiniz parolayı

    > [!IMPORTANT]
    > Bu örnekte, iki alt düğümler ile küme oluşturur. Çalışan düğüm sayısı ayrıca Küme oluşturulduktan sonra ölçeklendirme işlemleri gerçekleştirerek de değiştirebilirsiniz. 32'den fazla çalışan düğümleri kullanmayı planlıyorsanız, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir. Baş düğüm boyutu kullanarak ayarlayabilirsiniz `--headNodeSize` küme oluşturma sırasında parametre.
    >
    > Düğümü boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

    Küme oluşturma işleminin tamamlanması birkaç dakika sürebilir. Genellikle yaklaşık 15.

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI kullanarak bir Hdınsight kümesi başarıyla oluşturuldu, kümenizi ile çalışmayı öğrenmek için aşağıdakileri kullanın:

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
