---
title: Bir HDInsight kümesi - Azure silme
description: Bir HDInsight kümesini silebilirsiniz çeşitli yollar hakkında bilgiler.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 707112f755e6846622d2fde9de466551276925b8
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494361"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>Tarayıcınız, PowerShell veya Azure Klasik CLI kullanarak bir HDInsight kümesini silme

HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Bu belgede, kullanarak küme silme hakkında bilgi edinin [Azure portalında](https://portal.azure.com), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)ve Azure Klasik CLI.

> [!IMPORTANT]
> Azure depolama hesapları HDInsight küme silme silmez veya Data Lake Store kümeyle ilişkili. Gelecekte bu Hizmetleri'nde depolanan verileri yeniden kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portal

1. Oturum [Azure portalında](https://portal.azure.com) ve HDInsight kümenizi seçin. HDInsight kümenizi panoya sabitlenmemişse için arama alanını kullanarak adına göre arama yapabilirsiniz.
   
    ![Portal arama](./media/hdinsight-delete-cluster/navbar.png)

2. Küme ayarları seçin **Sil** simgesi. Sorulduğunda, **Evet** kümeyi silmek için.
   
    ![Sil simgesi](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Bir PowerShell isteminden kümeyi silmek için aşağıdaki komutu kullanın:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

**CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.

## <a name="azure-classic-cli"></a>Klasik Azure CLI

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Bir isteminden kümeyi silmek için aşağıdakileri kullanın:

    azure hdinsight cluster delete CLUSTERNAME

**CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.
