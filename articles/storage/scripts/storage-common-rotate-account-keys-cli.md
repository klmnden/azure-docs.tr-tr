---
title: Azure CLI Betiği Örneği - Depolama hesabı erişim anahtarlarını döndürme | Microsoft Docs
description: Bir Azure Depolama hesabı oluşturun, sonra da hesap erişim anahtarlarını alıp döndürün.
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
ms.openlocfilehash: ac58886225221677aa003833167ff58cd578255d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228656"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Bir depolama hesabı oluşturma ve hesap erişim anahtarlarını döndürme

Bu betik, bir Azure Depolama hesabı oluşturur, yeni depolama hesabının erişim anahtarlarını görüntüler ve ardından anahtarları yeniler (döndürür).

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, depolama hesabını ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu komut, depolama hesabını oluşturmak ve erişim anahtarlarını alıp döndürmek için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account) | Belirtilen kaynak grubunda bir Azure Depolama hesabı oluşturur. |
| [az storage account keys list](/cli/azure/storage/account/keys) | Belirtilen hesap için depolama hesabı erişim anahtarlarını görüntüler. |
| [az storage account keys renew](/cli/azure/storage/account/keys) | Birincil veya ikincil depolama hesabı erişim anahtarını yeniden oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek depolama CLI betiği örnekleri, [Azure Blob depolama için Azure CLI örneklerinde](../blobs/storage-samples-blobs-cli.md) bulunabilir.
