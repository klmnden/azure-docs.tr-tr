---
title: NVA faaliyetidir için bir Azure sanal WAN sanal hub yönlendirme tablosu oluşturma | Microsoft Docs
description: Sanal WAN sanal hub rota tablosu trafiği ağ sanal Gereci ilerletebilir.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to work with routing tables for NVA.
ms.openlocfilehash: fc8dd6770efa1c057a56374ddc0094c2d88d2eb5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60457614"
---
# <a name="create-a-virtual-hub-route-table-to-steer-traffic-to-a-network-virtual-appliance"></a>Bir ağ sanal Gerecinin trafik faaliyetidir için sanal Hub yönlendirme tablosu oluşturma

Bu makalede bir ağ sanal gerecine trafiği sanal Hub'ından faaliyetidir gösterilmektedir. 

![Sanal WAN diyagramı](./media/virtual-wan-route-table/vwanroute.png)

Bu makalede şunları öğreneceksiniz:

* WAN oluşturma
* Hub oluşturma
* Sanal ağ bağlantıları hub'ı oluşturma
* Hub yönlendirme oluşturma
* Yönlendirme tablosu oluşturma
* Rota tablosunu Uygula

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki ölçütleri karşıladığınızı doğrulayın:

1. Bir ağ sanal Gereci (NVA) var. Seçtiğiniz Azure Marketi'nden bir sanal ağda genellikle sağlandığında bir üçüncü taraf yazılım budur.
2. NVA ağ arabirimine atanan bir özel IP var. 
3. NVA sanal hub'ı dağıtılamıyor. Ayrı bir Vnet'te dağıtılması gerekir. Bu makale için NVA sanal ağ 'DMZ VNet' adlandırılır.
4. Bir 'DMZ VNet' olabilir veya birçok sanal ağa bağlı. Bu makalede, bu sanal ağ 'Dolaylı bağlı sanal ağ' denir. Bu sanal ağlar DMZ VNet eşlemesi kullanarak sanal ağa bağlanabilir.
5. 2 sanal ağ zaten oluşturulmuş olduğunu doğrulayın. Bu uç sanal ağları kullanılır. Bu makale için sanal ağ uç adres alanlarının 10.0.2.0/24 ve 10.0.3.0/24 var. Sanal ağ oluşturma hakkında bilgi gerekirse bkz [PowerShell kullanarak sanal ağ oluşturma](../virtual-network/quick-create-powershell.md).
6. Tüm sanal ağlarda hiçbir sanal ağ geçitleri olduğundan emin olun.

## <a name="signin"></a>1. Oturum aç

Resource Manager PowerShell cmdlet'lerinin en son sürümünü yüklediğinizden emin olun. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-az-ps). Bu önemlidir, çünkü cmdlet’lerin daha önceki sürümleri bu alıştırma için gereken geçerli değerleri içermez.

1. PowerShell Konsolunuzu yükseltilmiş ayrıcalıklarla açın ve Azure hesabınızda oturum açın. Bu cmdlet, oturum açma kimlik bilgilerini ister. Azure PowerShell için kullanılabilir olacak şekilde oturum açtıktan sonra hesap ayarlarınızı indirir.

   ```powershell
   Connect-AzAccount
   ```
2. Azure aboneliklerinizin bir listesini alın.

   ```powershell
   Get-AzSubscription
   ```
3. Kullanmak istediğiniz aboneliği belirtin.

   ```powershell
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```

## <a name="rg"></a>2. Kaynak oluşturma

1. Bir kaynak grubu oluşturun.

   ```powershell
   New-AzResourceGroup -Location "West US" -Name "testRG"
   ```
2. Sanal WAN oluşturun.

   ```powershell
   $virtualWan = New-AzVirtualWan -ResourceGroupName "testRG" -Name "myVirtualWAN" -Location "West US"
   ```
3. Bir sanal hub'ı oluşturun.

   ```powershell
   New-AzVirtualHub -VirtualWan $virtualWan -ResourceGroupName "testRG" -Name "westushub" -AddressPrefix "10.0.1.0/24"
   ```

## <a name="connections"></a>3. Bağlantılar oluşturun

Hub sanal ağ bağlantıları sanal hub'ına dolaylı uç sanal ağ ve DMZ sanal ağ oluşturun.

  ```powershell
  $remoteVirtualNetwork1= Get-AzVirtualNetwork -Name "indirectspoke1" -ResourceGroupName "testRG"
  $remoteVirtualNetwork2= Get-AzVirtualNetwork -Name "indirectspoke2" -ResourceGroupName "testRG"
  $remoteVirtualNetwork3= Get-AzVirtualNetwork -Name "dmzvnet" -ResourceGroupName "testRG"

  New-AzVirtualHubVnetConnection -ResourceGroupName "testRG" -VirtualHubName "westushub" -Name  "testvnetconnection1" -RemoteVirtualNetwork $remoteVirtualNetwork1
  New-AzVirtualHubVnetConnection -ResourceGroupName "testRG" -VirtualHubName "westushub" -Name  "testvnetconnection2" -RemoteVirtualNetwork $remoteVirtualNetwork2
  New-AzVirtualHubVnetConnection -ResourceGroupName "testRG" -VirtualHubName "westushub" -Name  "testvnetconnection3" -RemoteVirtualNetwork $remoteVirtualNetwork3
  ```

## <a name="route"></a>4. Sanal hub yönlendirme oluşturma

Bu makalede 10.0.2.0/24 ve 10.0.3.0/24 dolaylı uç sanal ağ adres alanları: ve DMZ NVA ağ arabiriminin özel IP adresi 10.0.4.5 olan.

```powershell
$route1 = New-AzVirtualHubRoute -AddressPrefix @("10.0.2.0/24", "10.0.3.0/24") -NextHopIpAddress "10.0.4.5"
```

## <a name="applyroute"></a>5. Sanal hub yönlendirme tablosu oluşturma

Bir sanal hub yol tablosu oluşturun ve ardından oluşturulan Yönlendirme uygulayabilir.
 
```powershell
$routeTable = New-AzVirtualHubRouteTable -Route @($route1)
```

## <a name="commit"></a>6. Değişiklikleri

Sanal hub'ıyla değişiklikleri uygulayın.

```powershell
Update-AzVirtualHub -VirtualWanId $virtualWan.Id -ResourceGroupName "testRG" -Name "westushub" -RouteTable $routeTable
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
