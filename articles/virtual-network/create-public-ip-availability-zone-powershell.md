---
title: "PowerShell ile zoned bir genel IP adresi oluşturma | Microsoft Docs"
description: "PowerShell ile bir kullanılabilirlik bölgesindeki bir genel IP oluşturun."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: bfeba57036338b4e325d2f122443f2cde0373eed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-powershell"></a>PowerShell ile bir kullanılabilirlik bölgesindeki bir ortak IP adresi oluştur

Bir ortak IP adresi Azure kullanılabilirlik bölgesinde (Önizleme) dağıtabilirsiniz. Bir Azure bölgesi fiziksel olarak ayrı bir bölgede bir kullanılabilirlik bölgedir. Şunları nasıl yapacağınızı öğrenin:

> * Bir kullanılabilirlik bölgesinde bir ortak IP adresi oluşturma
> * Kullanılabilirlik bölgede oluşturulan ilgili kaynakları belirleyin
  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu makalede sürüm 4.4.0 veya daha yüksek AzureRM modülün yüklü olmasını gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. AzureRM modülünden en son sürümünü yüklemek veya yükseltmek gerekiyorsa, yükleme [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM).

> [!NOTE]
> Kullanılabilirlik bölgeleri önizlemede ve geliştirme için hazır olduğunu ve test senaryoları. Destek select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```
## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu örnekte, bir kaynak grubu adında *myResourceGroup* oluşturulan *westeurope* bölge. *westeurope* kullanılabilirlik bölgeyi önizlemede destekleyen Azure bölgeleri biridir.

```powershell
New-AzureRmResourceGroup -Name AzTest -Location westeurope
```

## <a name="create-a-zonal-public-ip-address"></a>Zonal bir ortak IP adresi oluşturun

Bir ortak IP adresi aşağıdaki komutu kullanarak, kullanılabilirlik bölge oluşturun:

```powershell
    New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroup `
        -Name myPublicIp `
        -Location westeurope `
        -AllocationMethod Static `
        -Zone 2
```

> [!NOTE]
> Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.

## <a name="get-zone-information-about-a-public-ip-address"></a>Bölge Genel bir IP adresi hakkında bilgi edinin

Bölge bilgileri aşağıdaki komutu kullanarak genel bir IP adresi alın:

```powershell
Get-AzureRmPublicIpAddress ` 
    -ResourceGroup myResourceGroup `
    -Name myPublicIp
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [kullanılabilirlik bölgeleri](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)
- Daha fazla bilgi edinmek [genel IP adresleri](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 