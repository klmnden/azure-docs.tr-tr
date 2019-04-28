---
title: Azure CLI Betiği Örneği - Blob kapsayıcısı boyutunu hesaplama | Microsoft Docs
description: Azure Blob depolama alanındaki bir kapsayıcının boyutunu, kapsayıcıdaki blob’ların boyutunu toplayarak hesaplayın.
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
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: 3cb1e35617a58fcde7968ab45d437d865c91f983
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61245080"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Blob depolama kapsayıcısının boyutunu hesaplama

Bu betik, Azure Blob depolama alanındaki bir kapsayıcının boyutunu, kapsayıcıdaki blob’ların boyutunu toplayarak hesaplar.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Bu CLI betiği, kapsayıcının tahmini boyutunu sağladığından fatura hesaplamaları için kullanılmamalıdır.

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, kapsayıcıyı ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, Blob depolama kapsayıcısının boyutunu hesaplamak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage blob upload](/cli/azure/storage/account) | Yerel dosyaları bir Azure Blob depolama kapsayıcısına yükler. |
| [az storage blob list](/cli/azure/storage/account/keys) | Bir Azure Blob depolama kapsayıcısındaki blob’ları listeler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek depolama CLI betiği örnekleri, [Azure Blob depolama için Azure CLI örneklerinde](../blobs/storage-samples-blobs-cli.md) bulunabilir.
