---
title: "Azure CLI komut dosyası örneği - Sil kapsayıcıları ön eke göre | Microsoft Docs"
description: "Kapsayıcı adı ön ekini temel alarak Azure Storage blobu kapsayıcıları silin."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: marsma
ms.openlocfilehash: ff30037c7aba4e0e9c6b4a1829a0769093dce0ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Kapsayıcı adı ön ekini temel alarak kapsayıcıları Sil

Bu komut dosyası ilk Azure Blob depolama alanına birkaç örnek kapsayıcılarını oluşturur ve sonra bir kapsayıcı adı ön ekini temel kapsayıcıları bazıları siler.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kapsayıcılar, kalan kaynak grubunu kaldırmak için aşağıdaki komutu çalıştırın ve ilişkili tüm kaynakları.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası kapsayıcı adı ön ekini temel alarak kapsayıcıları silmek için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her öğe.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](/cli/azure/storage/account#create) | Bir Azure Storage hesabı içinde belirtilen kaynak grubu oluşturur. |
| [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container#create) | Azure Blob depolama alanına bir kapsayıcı oluşturur. |
| [az depolama kapsayıcısı listesi](/cli/azure/storage/container#list) | Bir Azure depolama hesabında kapsayıcıları listeler. |
| [az depolama kapsayıcısı Sil](/cli/azure/storage/container#delete) | Bir Azure depolama hesabında kapsayıcıları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](/cli/azure/overview).

Ek depolama alanı CLI kod örnekleri bulunabilir [Azure depolama için Azure CLI örnek](../blobs/storage-samples-blobs-cli.md).
