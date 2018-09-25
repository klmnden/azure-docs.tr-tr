---
title: Bir HDInsight kümesi - Azure silme
description: Bir HDInsight kümesini silebilirsiniz çeşitli yollar hakkında bilgiler.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: jasonh
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 39404ff74552b11e982cf5968c0eb131ea642e27
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979463"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>Tarayıcınız, PowerShell veya Azure Klasik CLI kullanarak bir HDInsight kümesini silme

HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Bu belgede, Azure portalı, Azure PowerShell ve Azure Klasik CLI'yı kullanarak küme silme hakkında bilgi edinin.

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
