---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1fae3c3889242dfbf8f270d3762ea7434ceda6da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004194"
---
Sanal ağınız yoksa PowerShell kullanarak hızlıca bir tane oluşturabilirsiniz. Azure portalı kullanarak da sanal ağ oluşturabilirsiniz.

* Oluşturduğunuz sanal ağın adres alanının bağlanmak istediğiniz diğer sanal ağların adres aralıklarıyla veya şirket içi ağ adres alanlarıyla çakışmadığından emin olun. 
* Sanal ağınız varsa gerekli ölçütleri karşıladığından ve sanal ağ geçidi bulunmadığından emin olun.

Bu makaledeki "Deneyin" düğmesine tıklayıp PowerShell konsolu açarak sanal ağınızı kolayca oluşturabilirsiniz. Değerleri ayarlayın ve ardından komutları kopyalayıp konsol penceresine yapıştırabilirsiniz.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PowerShell komutlarını ayarlayın ve ardından bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

PowerShell komutlarını ayarlayarak ortamınızla uyumlu bir sanal ağ oluşturun.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```