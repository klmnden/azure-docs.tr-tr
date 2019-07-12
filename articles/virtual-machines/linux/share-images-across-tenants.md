---
title: Azure'da kiracılar galeri görüntüleri paylaşın | Microsoft Docs
description: Azure kiracılar genelinde paylaşılan resim galerileri kullanarak VM görüntüleri paylaşacağınızı öğrenin.
services: virtual-machines-linux
author: cynthn
manager: gwallace
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: cbf42de406ecb0caed67b77743f376284ab64608
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706548"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Azure kiracılar genelinde galeri VM görüntülerini paylaşma

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

> [!IMPORTANT]
> Portal, başka bir azure kiracısı bir görüntüden bir VM dağıtmak için kullanamazsınız. Kiracılar arasında paylaşılan bir görüntüden bir VM oluşturmak için Azure CLI kullanın veya [Powershell](../windows/share-images-across-tenants.md).

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