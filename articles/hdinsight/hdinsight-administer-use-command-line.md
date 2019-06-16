---
title: Azure CLI kullanarak Azure HDInsight kümelerini yönetme
description: Azure HDInsight kümelerini yönetmek için Azure CLI'yı kullanmayı öğrenin. Küme türleri, Apache Hadoop, Spark, HBase, Storm, Kafka, Interactive Query ve ML hizmetleri içerir.
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tyfox
ms.openlocfilehash: 7c12831c43762ddc776e8d5701f002be97992cbc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65859958"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Azure CLI kullanarak Azure HDInsight kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Nasıl kullanacağınızı öğrenin [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) Azure HDInsight kümelerini yönetmek için. Azure komut satırı arabirimi (CLI), Azure kaynaklarını yönetmek için Microsoft tarafından sunulan platformlar arası komut satırı deneyimidir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Azure CLI. Azure CLI'yi yüklemediyseniz, bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) adımlar.

* HDInsight üzerinde Apache Hadoop kümesi. Bkz: [Linux'ta HDInsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Azure aboneliğinizde oturum açın. Azure Cloud Shell'i kullanmak sonra seçmeniz yeterlidir planlıyorsanız **deneyin** kod bloğunun sağ üst köşedeki. Aksi takdirde, aşağıdaki komutu girin:

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>Kümeleri listeleme

Kullanım [az hdınsight listesi](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) kümeleri listeleme için. Değiştirerek aşağıdaki komutları düzenleyin `RESOURCE_GROUP_NAME` kaynak grubunuzun adıyla, ardından komutları girin:

```azurecli-interactive
# List all clusters in the current subscription
az hdinsight list

# List only cluster name and its resource group
az hdinsight list --query "[].{Cluster:name, ResourceGroup:resourceGroup}" --output table

# List all cluster for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME

# List all cluster names for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME --query "[].{clusterName:name}" --output table
```

## <a name="show-cluster"></a>Kümenin Göster

Kullanım [az hdınsight show](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) bilgi belirtilen bir küme için gösterilecek. Aşağıdaki komutta değiştirerek Düzenle `RESOURCE_GROUP_NAME`, ve `CLUSTER_NAME` ile ilgili bilgileri, sonra komutu girin:

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>Kümeleri Sil

Kullanım [az hdınsight Sil](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) belirtilen küme silinemedi. Aşağıdaki komutta değiştirerek Düzenle `RESOURCE_GROUP_NAME`, ve `CLUSTER_NAME` ile ilgili bilgileri, sonra komutu girin:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

Kümeyi içeren kaynak grubunu silerek bir küme de silebilirsiniz. Not: Bu varsayılan depolama hesabı dahil olmak üzere grubundaki tüm kaynakları siler.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme

Kullanım [az hdınsight yeniden boyutlandırma](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) belirtilen HDInsight kümesi için belirtilen boyut yeniden boyutlandırabilirsiniz. Aşağıdaki komutta değiştirerek Düzenle `RESOURCE_GROUP_NAME`, ve `CLUSTER_NAME` ile ilgili bilgiler. Değiştirin `TARGET_INSTANCE_COUNT` ile istenen kümeniz için çalışan düğümü sayısı. Kümeleri ölçeklendirme hakkında daha fazla bilgi için bkz. [ölçek HDInsight kümeleri](./hdinsight-scaling-best-practices.md). Aşağıdaki komutu girin:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --target-instance-count TARGET_INSTANCE_COUNT
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, farklı HDInsight küme yönetim görevlerinin nasıl gerçekleştirileceğini öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md)
* [HDInsight, Azure PowerShell kullanarak yönetme](hdinsight-administer-use-powershell.md)
* [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure CLI ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
