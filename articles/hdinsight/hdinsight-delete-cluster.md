---
title: Hdınsight kümesi - Azure silme | Microsoft Docs
description: Hdınsight kümesi silebilmek için çeşitli yollar hakkında bilgiler.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 01c59d4970bca54417c9b860ec177ecf5a37d8c5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31398811"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Tarayıcınız, PowerShell veya Azure CLI kullanarak bir Hdınsight kümesini Sil

Hdınsight küme faturalandırma bir küme oluşturulur ve küme silindiğinde durdurur sonra başlar. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Bu belgede, Azure portalı, Azure PowerShell ve Azure CLI 1.0 kullanarak küme silme hakkında bilgi edinin.

> [!IMPORTANT]
> Data Lake Store kümeyle ilişkilendirilmiş veya bir Hdınsight kümesi siliniyor Azure depolama hesapları silmez. Gelecekte bu services içinde depolanan verileri yeniden kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portalına

1. Oturum [Azure portal](https://portal.azure.com) ve Hdınsight kümenize seçin. Hdınsight kümenize panoya sabitlenmemişse için arama alanı kullanarak ada göre arama yapabilirsiniz.
   
    ![Portal arama](./media/hdinsight-delete-cluster/navbar.png)

2. Küme ayarlarından seçim **silmek** simgesi. İstendiğinde, seçin **Evet** kümesini silmek için.
   
    ![Sil simgesi](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Bir PowerShell isteminden kümesini silmek için aşağıdaki komutu kullanın:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

**CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.

## <a name="azure-cli-10"></a>Azure CLI 1.0

Bir isteminden kümesini silmek için aşağıdakileri kullanın:

    azure hdinsight cluster delete CLUSTERNAME

**CLUSTERNAME** değerini HDInsight kümenizin adıyla değiştirin.

> [!NOTE]
> Azure CLI 2.0 silme Hdınsight kümeleri şu anda (23 Ekim 2017) desteklemez.
