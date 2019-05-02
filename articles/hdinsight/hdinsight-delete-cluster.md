---
title: Bir HDInsight kümesi - Azure silme
description: Bir HDInsight kümesini silebilirsiniz çeşitli yollar hakkında bilgiler.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: eca7b4f8bd7e91bc8dcb9bcc49ed3b981010aaee
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64721024"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesini silme

HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Bu belgede, kullanarak küme silme hakkında bilgi edinin [Azure portalında](https://portal.azure.com), [Az Azure PowerShell Modülü](https://docs.microsoft.com/powershell/azure/overview)ve [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

> [!IMPORTANT]  
> Azure depolama hesapları HDInsight küme silme silmez veya Data Lake Storage kümeyle ilişkili. Gelecekte bu Hizmetleri'nde depolanan verileri yeniden kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol menüden gidin **tüm hizmetleri** > **Analytics** > **HDInsight kümeleri** ve kümenizi seçin.

3. Varsayılan görünümünden seçim **Sil** simgesi. Kümenizi sildiğinizden için istemi izleyin.
   
    ![Sil simgesi](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell-az-module"></a>Azure PowerShell Az Modülü

Değiştirin `CLUSTERNAME` aşağıdaki kod, HDInsight kümenizin adıyla. Bir PowerShell isteminden kümeyi silmek için aşağıdaki komutu girin:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure CLI

Değiştirin `CLUSTERNAME` HDInsight kümenizin adıyla ve `RESOURCEGROUP` aşağıdaki kod, kaynak grubunuzda adı.  Bir komut istemi'nden kümeyi silmek için şunu girin:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```
