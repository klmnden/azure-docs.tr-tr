---
title: "Azure CLI betik örnek - hesaplamak blob kapsayıcı boyutu | Microsoft Docs"
description: "Azure Blob depolamada kapsayıcı boyutu BLOB kapsayıcısında boyutunu toplanmasıyla hesaplayın."
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
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: 21d49f1ef2f0f9e93e72dcd2a1667033b234bab6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Bir Blob Depolama kapsayıcısını boyutu hesaplanamadı

Bu komut, BLOB kapsayıcısında boyutunu toplanmasıyla Azure Blob depolamada kapsayıcı boyutu hesaplar.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, kapsayıcı ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, Blob Depolama kapsayıcısını boyutunu hesaplamak için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her öğe.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama blob karşıya yükleme](/cli/azure/storage/account#create) | Yerel dosyaları bir Azure Blob storage kapsayıcısına yükler. |
| [az depolama blob listesi](/cli/azure/storage/account/keys#list) | Bir Azure Blob Depolama kapsayıcısı içinde BLOB'ları listeler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](/cli/azure/overview).

Ek depolama alanı CLI kod örnekleri bulunabilir [Azure Blob Depolama için Azure CLI örnek](../blobs/storage-samples-blobs-cli.md).
