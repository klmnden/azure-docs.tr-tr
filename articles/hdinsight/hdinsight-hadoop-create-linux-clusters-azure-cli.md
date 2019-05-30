---
title: -Azure HDInsight Azure CLI kullanarak Apache Hadoop kümeleri oluşturma
description: Platformlar arası Azure CLI kullanarak HDInsight kümeleri oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: hrasheed
ms.openlocfilehash: 0a278cd98b0dd6c6d8f0fe9bfee81e5bafd4f543
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597692"
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Azure CLI kullanarak HDInsight kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure CLI kullanarak bir HDInsight 3.6 kümesi oluşturarak bu belgeyi gözden geçirme adımları.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure CLI. Azure CLI'yi yüklemediyseniz, bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) adımlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-cluster"></a>Küme oluşturma

1. Azure aboneliğinizde oturum açın. Azure Cloud Shell'i kullanmak sonra seçmeniz yeterlidir planlıyorsanız **deneyin** kod bloğunun sağ üst köşedeki. Aksi takdirde, aşağıdaki komutu girin:

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

2. Ortam değişkenlerini ayarlayın. Bu makalede değişkenlerini üzerinde Bash temel alır. Küçük farklılıklar diğer ortamları için gereklidir. Bkz: [az hdınsight oluşturma](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) olası parametre küme oluşturma için tam bir listesi için.

    |Parametre | Açıklama |
    |---|---|
    |`--size`| Kümedeki çalışan düğümü sayısı. Bu makalede değişkenini kullanır `clusterSizeInNodes` geçirilen değeri olarak `--size`. |
    |`--version`| HDInsight küme sürümü. Bu makalede değişkenini kullanır `clusterVersion` geçirilen değeri olarak `--version`. Ayrıca bkz: [HDInsight sürümleri desteklenen](./hdinsight-component-versioning.md#supported-hdinsight-versions).|
    |`--type`| HDInsight kümesinin gibi yazın: hadoop, interactivehive, hbase, kafka, storm, spark, rserver, mlservices.  Bu makalede değişkenini kullanır `clusterType` geçirilen değeri olarak `--type`. Ayrıca bkz: [Küme türleri ve yapılandırma](./hdinsight-hadoop-provision-linux-clusters.md#cluster-types).|
    |`--component-version`|Boşlukla ayrılmış sürümlerinde çeşitli Hadoop bileşenleri, sürümleri ' bileşen sürümü =' biçimi. Bu makalede değişkenini kullanır `componentVersion` geçirilen değeri olarak `--component-version`. Ayrıca bkz: [Hadoop bileşenleri](./hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions).|

    Değiştirin `RESOURCEGROUPNAME`, `LOCATION`, `CLUSTERNAME`, `STORAGEACCOUNTNAME`, ve `PASSWORD` istenen değerleri. İstediğiniz gibi diğer değişkenlerin değerlerini değiştirin. Ardından, CLI komutları girin.

    ```azurecli-interactive
    export resourceGroupName=RESOURCEGROUPNAME
    export location=LOCATION
    export clusterName=CLUSTERNAME
    export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
    export httpCredential='PASSWORD'
    export sshCredentials='PASSWORD'
    
    export AZURE_STORAGE_CONTAINER=$clusterName
    export clusterSizeInNodes=1
    export clusterVersion=3.6
    export clusterType=hadoop
    export componentVersion=Hadoop=2.7
    ```

3. [Kaynak grubunu oluşturma](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) aşağıdaki komutu girerek:

    ```azurecli-interactive
    az group create \
        --location $location \
        --name $resourceGroupName
    ```

    Geçerli konumlar listesi için kullanmak `az account list-locations` komutunu ve ardından konumlardan birini kullanın `name` değeri.

4. [Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) aşağıdaki komutu girerek:

    ```azurecli-interactive
    # Note: kind BlobStorage is not available as the default storage account.
    az storage account create \
        --name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

5. [Azure depolama hesabından birincil anahtarı ayıklamak](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest#az-storage-account-keys-list) ve aşağıdaki komutu girerek bir değişkende saklayın:

    ```azurecli-interactive
    export AZURE_STORAGE_KEY=$(az storage account keys list \
        --account-name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

6. [Bir Azure depolama kapsayıcısı oluşturma](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) aşağıdaki komutu girerek:

    ```azurecli-interactive
    az storage container create \
        --name $AZURE_STORAGE_CONTAINER \
        --account-key $AZURE_STORAGE_KEY \
        --account-name $AZURE_STORAGE_ACCOUNT
    ```

7. [HDInsight kümesi oluşturma](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) aşağıdaki komutu girerek:

    ```azurecli-interactive
    az hdinsight create \
        --name $clusterName \
        --resource-group $resourceGroupName \
        --type $clusterType \
        --component-version $componentVersion \
        --http-password $httpCredential \
        --http-user admin \
        --location $location \
        --size $clusterSizeInNodes \
        --ssh-password $sshCredentials \
        --ssh-user sshuser \
        --storage-account $AZURE_STORAGE_ACCOUNT \
        --storage-account-key $AZURE_STORAGE_KEY \
        --storage-default-container $AZURE_STORAGE_CONTAINER \
        --version $clusterVersion
    ```

    > [!IMPORTANT]  
    > HDInsight kümeleri, çeşitli türleri, küme için ayarlanan teknoloji ve iş yükü karşılık gelen. Bir küme üzerinde Storm ve HBase gibi birden birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur.

    Bu, küme oluşturma işleminin tamamlanması birkaç dakika sürebilir. Genellikle yaklaşık 15.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Tüm veya bazı kaynakları kaldırmak için aşağıdaki komutları girin:

```azurecli-interactive
# Remove cluster
az hdinsight delete \
    --name $clusterName \
    --resource-group $resourceGroupName

# Remove storage container
az storage container delete \
    --account-name $AZURE_STORAGE_ACCOUNT \
    --name $AZURE_STORAGE_CONTAINER

# Remove storage account
az storage account delete \
    --name $AZURE_STORAGE_ACCOUNT \
    --resource-group $resourceGroupName

# Remove resource group
az group delete \
    --name $resourceGroupName
```

## <a name="troubleshoot"></a>Sorun gider

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](./hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI kullanarak bir HDInsight kümesi başarıyla oluşturuldu, kümenizi ile çalışma hakkında bilgi almak için aşağıdakileri kullanın:

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
