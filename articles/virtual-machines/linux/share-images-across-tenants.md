---
title: Azure'da kiracılar galeri görüntüleri paylaşın | Microsoft Docs
description: Azure kiracılar genelinde paylaşılan resim galerileri kullanarak VM görüntüleri paylaşacağınızı öğrenin.
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: 1578ba840c6dca93feb68754863439811d7ef099
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158738"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Azure kiracılar genelinde galeri VM görüntülerini paylaşma

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]


## <a name="create-a-vm-using-azure-cli"></a>Azure CLI kullanarak VM oluşturma

1 uygulama kimliği, uygulama anahtarı ve 1 Kiracı Kimliğini kullanarak Kiracı için hizmet sorumlusu olarak oturum açın. Kullanabileceğiniz `az account show --query "tenantId"` gerektiğinde Kiracı kimliklerini almak için.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
2 uygulama kimliği, uygulama anahtarı ve 2 Kiracı Kimliğini kullanarak Kiracı için hizmet sorumlusu oturum açma:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

Bir VM oluşturun. Örnekte bilgileri kendi değerlerinizle değiştirin.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir sorunla varsa [paylaşılan resim galerileri sorun giderme](troubleshooting-shared-images.md).