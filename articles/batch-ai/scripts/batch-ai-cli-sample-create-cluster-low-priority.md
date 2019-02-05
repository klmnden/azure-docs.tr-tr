---
title: Azure CLI Betik Örneği - Düşük öncelikli Batch AI kümesi oluşturma| Microsoft Docs
description: Azure CLI betik örneği - Düşük öncelikli düğümlerin (sanal makineler) Batch AI kümesini oluşturma ve yönetme
services: batch-ai
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.devlang: azurecli
ms.topic: sample
ms.tgt-pltfrm: multiple
ms.workload: na
ms.date: 07/26/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: e1a37104a5dc6e89b147c8bb9e14b4eda36d8eef
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697866"
---
# <a name="cli-example-create-and-manage-a-batch-ai-cluster-of-low-priority-nodes"></a>CLI örneği: Düşük öncelikli düğümler, bir Batch AI küme oluşturma ve yönetme

[!INCLUDE [batch-ai-retiring](../../../includes/batch-ai-retiring.md)]

Bu betik, düşük öncelikli düğümlerden (sanal makineler) oluşan bir Batch AI kümesi oluşturmak ve yönetmek için Azure CLI’sinde bulunan bazı komutları gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.38 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-low-priority.sh "Create Batch AI cluster - low-priority nodes")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak gruplarını ve kaynak gruplarıyla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutları çalıştırın.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup

# Remove resource group for the auto-storage account.
az group delete --name batchaiautostorage
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Batch AI çalışma alanı oluşturur. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Batch AI kümesi oluşturur. |
| [az batchai cluster show](/cli/azure/batchai/cluster) | Batch AI kümesi hakkındaki bilgileri gösterir. |
| [az batchai cluster node list](/cli/azure/batchai/cluster/node) | Batch AI kümesindeki düğümleri listeler. |
| [az batchai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize) | Batch AI kümesi yeniden boyutlandırır.  |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).
