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
ms.openlocfilehash: dff9420eb0f91652cc134a37d1b248e2e5b2b681
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004545"
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
