---
title: Azure CLI Betik Örneği - bir yapılandırma dosyası ile Batch AI kümesi oluşturun | Microsoft Docs
description: Azure CLI betik örneği - bir JSON dosyasında yapılandırma ayarlarını belirterek Batch AI kümesi oluşturun.
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
ms.date: 08/16/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 41a3a801214ff00c01397034e26fde6946ab97f0
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407822"
---
# <a name="cli-example-create-a-batch-ai-cluster-using-a-cluster-configuration-file"></a>CLI örneği: Bir küme yapılandırma dosyası kullanarak bir Batch AI kümesi oluşturma

[!INCLUDE [batch-ai-retiring](../../../includes/batch-ai-retiring.md)]

Bu betik, Batch AI kümesinin ayarlarını belirtmek için JSON yapılandırma dosyasının nasıl kullanılacağını gösterir. `az batchai cluster create` için ilgili komut satırı parametreleri yerine bu ayarları kullanın. Küme düğümlerine birden çok dosya sistemi bağlamanız gerektiğinde veya birkaç kümede özdeş yapılandırma kullanmak istediğinizde bir yapılandırma dosyası yararlıdır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmaya karar verirseniz bu kılavuz için Azure CLI 2.0.38 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü belirlemek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-config-file.sh "Create Batch AI cluster - configuration file")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak gruplarını ve kaynak gruplarıyla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutları çalıştırın.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Bir depolama hesabı oluşturur. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Bir depolama hesabında dosya paylaşımı oluşturur. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Batch AI çalışma alanı oluşturur. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Batch AI kümesi oluşturur. |
| [az batchai cluster show](/cli/azure/batchai/cluster#az-batchai-cluster-show) | Batch AI kümesi hakkındaki bilgileri gösterir. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).
