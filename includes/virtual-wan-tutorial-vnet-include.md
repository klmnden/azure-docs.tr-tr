---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 660bbf50e1a8ae73bd7bbe1f7c42691ed62d276a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552988"
---
Sanal ağınız yoksa PowerShell kullanarak hızlıca bir tane oluşturabilirsiniz. Azure portalı kullanarak da sanal ağ oluşturabilirsiniz.

* Oluşturduğunuz sanal ağın adres alanının bağlanmak istediğiniz diğer sanal ağların adres aralıklarıyla veya şirket içi ağ adres alanlarıyla çakışmadığından emin olun. 
* Sanal ağınız varsa gerekli ölçütleri karşıladığından ve sanal ağ geçidi bulunmadığından emin olun.

Bu makaledeki "Deneyin" düğmesine tıklayıp PowerShell konsolu açarak sanal ağınızı kolayca oluşturabilirsiniz. Değerleri ayarlayın ve ardından komutları kopyalayıp konsol penceresine yapıştırabilirsiniz.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PowerShell komutlarını ayarlayın ve ardından bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

PowerShell komutlarını ayarlayarak ortamınızla uyumlu bir sanal ağ oluşturun.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```