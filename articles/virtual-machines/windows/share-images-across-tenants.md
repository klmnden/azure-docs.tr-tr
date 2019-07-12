---
title: Azure'da kiracılar galeri görüntüleri paylaşın | Microsoft Docs
description: Azure kiracılar genelinde paylaşılan resim galerileri kullanarak VM görüntüleri paylaşacağınızı öğrenin.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: c26abe948fa415c780d543c615c34af2091cfbc7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709170"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Azure kiracılar genelinde galeri VM görüntülerini paylaşma

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]


> [!IMPORTANT]
> Portal, başka bir azure kiracısı bir görüntüden bir VM dağıtmak için kullanamazsınız. Kiracılar arasında paylaşılan bir görüntüden VM oluşturma için kullanmanız gerekir [Azure CLI](../linux/share-images-across-tenants.md) veya Powershell.

## <a name="create-a-vm-using-powershell"></a>PowerShell kullanarak VM oluşturma


Uygulama Kimliği'ni kullanarak hem kiracının oturum gizli ve Kiracı kimliği. 

```azurepowershell-interactive
$applicationId = '<App ID>'
$secret = <Secret> | ConvertTo-SecureString -AsPlainText -Force
$tenant1 = "<Tenant 1 ID>"
$tenant2 = "<Tenant 2 ID>"
$cred = New-Object -TypeName PSCredential -ArgumentList $applicationId, $secret
Clear-AzContext
Connect-AzAccount -ServicePrincipal -Credential $cred  -Tenant "<Tenant 1 ID>"
Connect-AzAccount -ServicePrincipal -Credential $cred -Tenant "<Tenant 2 ID>"
```

VM üzerinde uygulama kayıt izni olan bir kaynak grubu oluşturun. Bu örnekte bilgileri kendi değerlerinizle değiştirin.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$image = "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>"
New-AzVm `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVMfromImage" `
   -Image $image `
   -Location "South Central US" `
   -VirtualNetworkName "myImageVnet" `
   -SubnetName "myImageSubnet" `
   -SecurityGroupName "myImageNSG" `
   -PublicIpAddressName "myImagePIP" `
   -OpenPorts 3389
```

## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan görüntü Galerisi kaynakları kullanarak da oluşturabilirsiniz [Azure portalında](shared-images-portal.md).