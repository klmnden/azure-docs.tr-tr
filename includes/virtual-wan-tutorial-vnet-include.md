---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 04/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 40c8cb41ad3bcd46e9973a5f96134ff1bfd02fd2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150862"
---
Hızla bir VNet oluşturmak için "Try It" Bu makalede bir PowerShell konsolu açın tıklayabilirsiniz. Değerleri ayarlayın ve ardından komutları kopyalayıp konsol penceresine yapıştırabilirsiniz. Yeni Az modül ve AzureRM uyumluluğu hakkında daha fazla bilgi için bkz: [Karşınızda yeni Azure PowerShell Az modül](/powershell/azure/new-azureps-module-az). Az Modül yükleme yönergeleri için bkz. [Azure PowerShell yükleme](/powershell/azure/install-az-ps).

Oluşturduğunuz sanal ağın adres alanının bağlanmak istediğiniz diğer sanal ağların adres aralıklarıyla veya şirket içi ağ adres alanlarıyla çakışmadığından emin olun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Kullanmak istediğiniz bir kaynak grubu zaten yoksa, yeni bir tane oluşturun. Kullanmak istediğiniz kaynak grubu adı yansıtacak şekilde PowerShell komutlarını ayarlayın ve ardından aşağıdaki cmdlet'i çalıştırın:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

Ortamınız için uyumlu olan bir VNet oluşturmak için PowerShell komutlarını ayarlayın.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```