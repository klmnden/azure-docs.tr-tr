---
title: Azure CLI ile Azure HDInsight, Apache Spark kümesi oluşturma
description: Bu hızlı başlangıçta, Azure HDInsight Apache Spark kümesi oluşturmak için Azure CLI kullanma gösterilmektedir.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 05/09/2019
ms.author: hrasheed
ms.openlocfilehash: b9478ca8e1b31c1761e063a6789e96043f9a2c68
ms.sourcegitcommit: 9e8dfa1169a55c3c8af93a6c5f4e0dace4de48b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65556835"
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight-with-azure-cli"></a>Azure CLI ile Azure HDInsight, Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure CLI kullanarak Azure HDInsight Apache Spark kümesi oluşturma işlemini öğrenin. Apache Spark, bellek içi işleme kullanarak hızlı veri analizi ve küme hesaplama sağlar. [Azure komut satırı arabirimi (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) Azure kaynaklarını yönetmek için Microsoft'un platformlar arası komut satırı deneyimidir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure CLI. Azure CLI'yi yüklemediyseniz, bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) adımlar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-an-apache-spark-cluster"></a>Bir Apache Spark kümesi oluşturma

1. Azure aboneliğinizde oturum açın. Azure Cloud Shell'i kullanmak sonra seçmeniz yeterlidir planlıyorsanız **deneyin** kod bloğunun sağ üst köşedeki. Aksi takdirde, aşağıdaki komutu girin:

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

2. Ortam değişkenlerini ayarlayın. Bu hızlı başlangıçta değişkenlerini üzerinde Bash temel alır. Küçük farklılıklar diğer ortamları için gereklidir. Kod parçacığında RESOURCEGROUPNAME, konum, CLUSTERNAME, STORAGEACCOUNTNAME ve parola istediğiniz değerlerle değiştirin. Ardından ortam değişkenlerini ayarlamak için CLI komutlarını girin.

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
    export clusterType=spark
    export componentVersion=Spark=2.3
    ```

3. Aşağıdaki komutu girerek bir kaynak grubu oluşturun:

    ```azurecli-interactive
    az group create \
        --location $location \
        --name $resourceGroupName
    ```

4. Aşağıdaki komutu girerek bir Azure depolama hesabı oluşturun:

    ```azurecli-interactive
    az storage account create \
        --name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

5. Azure depolama hesabından birincil anahtarını ayıklayın ve aşağıdaki komutu girerek bir değişkende depolayın:

    ```azurecli-interactive
    export AZURE_STORAGE_KEY=$(az storage account keys list \
        --account-name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

6. Aşağıdaki komutu girerek bir Azure depolama kapsayıcısı oluşturun:

    ```azurecli-interactive
    az storage container create \
        --name $AZURE_STORAGE_CONTAINER \
        --account-key $AZURE_STORAGE_KEY \
        --account-name $AZURE_STORAGE_ACCOUNT
    ```

7. Apache Spark kümesi, aşağıdaki komutu girerek oluşturun:

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

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

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure CLI kullanarak Azure HDInsight Apache Spark kümesi oluşturma hakkında bilgi edindiniz.  HDInsight Spark kümesini kullanarak örnek veriler üzerinde etkileşimli sorgular çalıştırma hakkında daha fazla bilgi edinmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark üzerinde etkileşimli sorguları çalıştırma](./apache-spark-load-data-run-query.md)
