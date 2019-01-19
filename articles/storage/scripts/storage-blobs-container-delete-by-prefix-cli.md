---
title: Azure CLI Betiği Örneği - Kapsayıcıları ön eke göre silme | Microsoft Docs
description: Azure Depolama blob’u kapsayıcılarını, bir kapsayıcı adı ön ekine göre silin.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: 41f026da8b961cec0ae200e6182a7baa7a849af7
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411790"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Kapsayıcıları, kapsayıcı adı ön ekine göre silme

Bu betik ilk olarak Azure Blob depolama alanında birkaç örnek kapsayıcı oluşturur, sonra da kapsayıcı adındaki bir ön eke göre bazı kapsayıcıları siler.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, kalan kapsayıcıları ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kapsayıcıları, kapsayıcı adı ön ekine göre silmek için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Belirtilen kaynak grubunda bir Azure Depolama hesabı oluşturur. |
| [az storage container create](/cli/azure/storage/container#az_storage_container_create) | Azure Blob depolama alanında bir kapsayıcı oluşturur. |
| [az storage container list](/cli/azure/storage/container#az_storage_container_list) | Bir Azure Depolama hesabındaki kapsayıcıları listeler. |
| [az storage container delete](/cli/azure/storage/container#az_storage_container_delete) | Bir Azure Depolama hesabındaki kapsayıcıları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek depolama CLI betiği örnekleri, [Azure Depolama için Azure CLI örneklerinde](../blobs/storage-samples-blobs-cli.md) bulunabilir.
