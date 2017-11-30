---
title: "Azure CLI komut dosyası örneği - döndürme depolama hesabı erişim anahtarlarını | Microsoft Docs"
description: "Bir Azure depolama hesabı oluşturma sonra almak ve kendi hesabı erişim anahtarlarını döndürün."
services: storage
documentationcenter: na
author: tamram
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
ms.openlocfilehash: 743c7b01e82721e855b4c33bf2d36714ca8c6468
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Depolama hesabı oluşturma ve kendi hesabı erişim anahtarlarını döndürün

Bu komut dosyası Azure depolama hesabı oluşturur, yeni depolama hesabının erişim anahtarlarını görüntüler ve ardından yeniler (döndüğü) anahtarları.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, depolama hesabı ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, depolama hesabı oluşturmak ve almak ve erişim anahtarlarını döndürmek için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her öğe.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](/cli/azure/storage/account#create) | Bir Azure Storage hesabı içinde belirtilen kaynak grubu oluşturur. |
| [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#list) | Belirtilen hesap için depolama hesabı erişim anahtarlarını görüntüler. |
| [az depolama hesabı anahtarlarını yenileme](/cli/azure/storage/account/keys#renew) | Birincil veya ikincil depolama hesabının erişim anahtarı yeniden oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](/cli/azure/overview).

Ek depolama alanı CLI kod örnekleri bulunabilir [Azure Blob Depolama için Azure CLI örnek](../blobs/storage-samples-blobs-cli.md).
