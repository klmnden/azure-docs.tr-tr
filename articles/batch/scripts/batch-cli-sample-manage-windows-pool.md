---
title: "Azure CLI Betik Örneği - Batch’te Windows Havuzu | Microsoft Docs"
description: "Azure CLI Betik Örneği - Batch’te Windows havuzu oluşturma ve yönetme"
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: b1fe98169e47c5f5386867cf2b6c23f8bc3bfb65
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI örneği: Azure Batch’te bir Windows havuzu oluşturma ve yönetme

Bu betikte, Azure Batch’te Windows işlem düğümleri havuzu oluşturmaya ve yönetmeye yönelik Azure CLI’de kullanılabilir komutlardan bazıları gösterilir. Bir Windows havuzu iki şekilde yapılandırılabilir, bir Cloud Services yapılandırmasıyla veya Sanal Makine yapılandırmasıyla. Bu örnek, bir Windows havuzunu Cloud Services yapılandırmasıyla oluşturmayı gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Batch hesabını oluşturur. |
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Daha fazla CLI etkileşimi için belirtilen Batch hesabına karşı kimlik doğrulaması yapar. |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | İşlem düğümleri havuzu oluşturur.  |
| [az batch pool set](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Havuzun özelliklerini güncelleştirir.  |
| [az batch pool autoscale enable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Bir havuzda otomatik ölçeklendirmeyi etkinleştirir ve bir formül uygular.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Havuzun özelliklerini görüntüler.  |
| [az batch pool autoscale disable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Bir havuzda otomatik ölçeklendirmeyi devre dışı bırakır. |
| [az group delete](/cli/azure/group#az_group_delete) | Tüm iç içe kaynakların dahil olduğu bir kaynak grubunu siler. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure/overview).
