---
title: 'Sanal Ağ eşlemesi için VPN ağ geçidi geçişi yapılandırın: Azure Resource Manager | Microsoft Docs'
description: Sanal ağ eşlemesi için VPN ağ geçidi aktarımını yapılandırın.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/25/2018
ms.author: yushwang
ms.openlocfilehash: d5e62bf1838c8f07068208019d28d7273c28bd63
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59492354"
---
# <a name="configure-vpn-gateway-transit-for-virtual-network-peering"></a>Sanal ağ eşlemesi için VPN ağ geçidi aktarımını yapılandırma

Bu makale, sanal ağ eşlemesi için ağ geçidi aktarımını yapılandırmanıza yardımcı olur. [Sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md), iki Azure sanal ağını sorunsuzca bağlar ve bağlantı amacıyla iki sanal ağı tek bir ağda birleştirir. [Ağ geçidi aktarımı](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity), bir sanal ağın şirket içi ve dışı karışık bağlantı ya da sanal ağlar arası bağlantı için eşlenmiş sanal ağdaki VPN ağ geçidinden yararlanmasını sağlayan bir eşleme özelliğidir. Aşağıdaki diyagramda sanal ağ eşlemesi ile ağ geçidi aktarımının nasıl çalıştığı gösterilmiştir.

![gateway-transit](./media/vpn-gateway-peering-gateway-transit/gatewaytransit.png)

Diyagramda, ağ geçidi aktarımı eşlenmiş sanal ağların Hub-RM içinde Azure VPN ağ geçidi kullanmasına olanak tanır. S2S, P2S ve sanal ağlar arası bağlantılar dahil VPN ağ geçidinde kullanılabilen bağlantılar, üç sanal ağ için de geçerlidir. Aktarım seçeneği, aynı veya farklı dağıtım modelleri arasında eşleme için kullanılabilir. Tek kısıtlama, diyagramda gösterildiği gibi VPN ağ geçidinin yalnızca Resource Manager dağıtım modelini kullanarak sanal ağda olabilmesidir.

Merkez-uç ağ mimarisinde ağ geçidi aktarımı, uç sanal ağların her uç sanal ağdaki VPN ağ geçitlerini dağıtmak yerine merkezdeki VPN ağ geçidini paylaşmasına olanak tanır. Ağ geçidine bağlı sanal ağların veya şirket içi ağların yolları, ağ geçidi aktarımı kullanılarak eşlenmiş sanal ağların yönlendirme tablolarına yayılır. VPN ağ geçidinden otomatik yol yaymayı devre dışı bırakabilirsiniz. "**BGP yol yaymayı devre dışı bırak**" seçeneği ile bir yönlendirme tablosu oluşturun ve yönlendirme tablosunu alt ağlarla ilişkilendirerek bu alt ağlara yol dağıtımını engelleyin. Daha fazla bilgi için bkz. [Sanal ağ yönlendirme tablosu](../virtual-network/manage-route-table.md).

Bu belgede açıklanan iki senaryo vardır:

1. Her iki sanal ağ da Resource Manager dağıtım modelini kullanır
2. Uç sanal ağ klasiktir, ağ geçidini içeren merkez sanal ağ ise Resource Manager içinde bulunur

## <a name="requirements"></a>Gereksinimler

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu belgedeki örnek, aşağıdaki kaynakların oluşturulmasını gerektirir:

1. Bir VPN ağ geçidi ile Merkez-RM sanal ağı
2. Uç-RM sanal ağı
3. Klasik dağıtım modeli ile Uç-Klasik sanal ağı
4. Kullandığınız hesap, gerekli rol ve izinleri gerektirir. Ayrıntılar için bu makalenin [İzinler](#permissions) bölümüne bakın.

Yönergeler için aşağıdaki belgelere bakın:

1. [Bir sanal ağda VPN ağ geçidi oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
2. [Aynı dağıtım modeli ile sanal ağ eşlemesi oluşturma](../virtual-network/tutorial-connect-virtual-networks-portal.md)
3. [Farklı dağıtım modelleri ile sanal ağ eşlemesi oluşturma](../virtual-network/create-peering-different-deployment-models.md)

## <a name="permissions"></a>İzinler

Sanal ağ eşlemesi için kullandığınız hesaplar gerekli rol veya izinlere sahip olmalıdır. Aşağıdaki örnekte, Merkez-RM ve Uç-Klasik adlı iki sanal ağ eşliyorsanız hesabınız her sanal ağ için aşağıdaki rol veya izinlere sahip olmalıdır:
    
|Sanal ağ|Dağıtım modeli|Rol|İzinler|
|---|---|---|---|
|Merkez-RM|Resource Manager|[Ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klasik|[Klasik Ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Yok|
|Uç-Klasik|Resource Manager|[Ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klasik|[Klasik Ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

[Yerleşik roller](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ve [özel rollere](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (yalnızca Resource Manager) belirli izinlerin atanması hakkında daha fazla bilgi edinin.

## <a name="resource-manager-to-resource-manager-peering-with-gateway-transit"></a>Ağ geçidi aktarımı ile Resource Manager’dan Resource Manager’a eşleme

Ağ geçidi aktarımını etkinleştirmek üzere sanal ağ eşlemeleri oluşturmak veya güncelleştirmek için yönergeleri izleyin.

1. Azure portalında Uç-RM’den Merkez-RM’ye sanal ağ eşlemesi oluşturun veya güncelleştirin. Spoke-RM sanal ağ kaynağına gidin, "Eşlemeler" ve sonra "Ekle" öğesine tıklayın:
    - "Resource Manager" seçeneğini ayarlayın
    - İlgili abonelikte Merkez-RM sanal ağını seçin
    - "Sanal ağ erişimine izin ver" seçeneğinin "Etkin" olduğundan emin olun
    - "**Uzak ağ geçitleri kullan**" seçeneğini ayarlayın
    - "Tamam"’a tıklayın

      ![spokerm-to-hubrm](./media/vpn-gateway-peering-gateway-transit/spokerm-hubrm-peering.png)

2. Eşleme zaten oluşturulduysa eşleme kaynağına gidin, ardından adım (1)’de gösterilen ekran görüntüsüne benzer şekilde "**Uzak ağ geçitleri kullan**" seçeneğini etkinleştirin

3. Azure portalında Merkez-RM’den Uç-RM’ye sanal ağ eşlemesi oluşturun veya güncelleştirin. Merkez-RM sanal ağ kaynağına gidin, "Eşlemeler" ve sonra "Ekle" öğesine tıklayın:
    - "Resource Manager" seçeneğini ayarlayın
    - "Sanal ağ erişimine izin ver" seçeneğinin "Etkin" olduğundan emin olun
    - İlgili abonelikte "Uç-RM" sanal ağını seçin
    - "**Ağ geçidi aktarımına izin ver**" seçeneğini ayarlayın
    - "Tamam"’a tıklayın

      ![hubrm-to-spokerm](./media/vpn-gateway-peering-gateway-transit/hubrm-spokerm-peering.png)

4. Eşleme zaten oluşturulduysa eşleme kaynağına gidin, ardından adım (3)’te gösterilen ekran görüntüsüne benzer şekilde "**Ağ geçidi aktarımına izin ver**" seçeneğini etkinleştirin

5. Her iki sanal ağda eşleme durumunun "**Bağlı**" olduğunu doğrulayın

### <a name="powershell-sample"></a>PowerShell örneği

Yukarıdaki örnekle eşleme oluşturmak veya güncelleştirmek için PowerShell’i de kullanabilirsiniz. Değişkenleri, sanal ağlarınızın ve kaynak gruplarınızın adlarıyla değiştirin.

```azurepowershell-interactive
$SpokeRG = "SpokeRG1"
$SpokeRM = "Spoke-RM"
$HubRG   = "HubRG1"
$HubRM   = "Hub-RM"

$spokermvnet = Get-AzVirtualNetwork -Name $SpokeRM -ResourceGroup $SpokeRG
$hubrmvnet   = Get-AzVirtualNetwork -Name $HubRM -ResourceGroup $HubRG

Add-AzVirtualNetworkPeering `
  -Name SpokeRMtoHubRM `
  -VirtualNetwork $spokermvnet `
  -RemoteVirtualNetworkId $hubrmvnet.Id `
  -UseRemoteGateways

Add-AzVirtualNetworkPeering `
  -Name HubRMToSpokeRM `
  -VirtualNetwork $hubrmvnet `
  -RemoteVirtualNetworkId $spokermvnet.Id `
  -AllowGatewayTransit
```

## <a name="classic-to-resource-manager-peering-with-gateway-transit"></a>Ağ geçidi aktarımı ile Klasikten Resource Manager’a eşleme

Adımlar Resource Manager örneğine benzer, ancak işlemler yalnızca Merkez-RM sanal ağına uygulanır.

1. Azure portalında Merkez-RM’den Uç-RM’ye sanal ağ eşlemesi oluşturun veya güncelleştirin. Merkez-RM sanal ağ kaynağına gidin, "Eşlemeler" ve sonra "Ekle" öğesine tıklayın:
   - Sanal ağ dağıtım modeli için "Klasik" seçeneğini ayarlayın
   - İlgili abonelikte "Uç-Klasik" sanal ağını seçin
   - "Sanal ağ erişimine izin ver" seçeneğinin "Etkin" olduğundan emin olun
   - "**Ağ geçidi aktarımına izin ver**" seçeneğini ayarlayın
   - "Tamam"’a tıklayın

     ![hubrm-to-spokeclassic](./media/vpn-gateway-peering-gateway-transit/hubrm-spokeclassic-peering.png)

2. Eşleme zaten oluşturulduysa eşleme kaynağına gidin, ardından adım (1)’de gösterilen ekran görüntüsüne benzer şekilde "**Ağ geçidi aktarımına izin ver**" seçeneğini etkinleştirin

3. Uç-Klasik sanal ağında işlem yok

4. Merkez-RM sanal ağında eşleme durumunun "**Bağlı**" olduğunu doğrulayın

Durum "Bağlı" olarak gösterildikten sonra uç sanal ağlar merkez sanal ağındaki VPN ağ geçidi üzerinden sanal ağlar arası veya şirket içi ve dışı karışık bağlantıyı kullanmaya başlayabilir.

### <a name="powershell-sample"></a>PowerShell örneği

Yukarıdaki örnekle eşleme oluşturmak veya güncelleştirmek için PowerShell’i de kullanabilirsiniz. Değişkenleri ve abonelik kimliğini sanal ağınızın, kaynak grubunuzun ve aboneliğinizin değerleri ile değiştirin. Yalnızca merkez sanal ağı üzerinde sanal ağ eşlemesi oluşturmanız gerekir.

```azurepowershell-interactive
$HubRG   = "HubRG1"
$HubRM   = "Hub-RM"

$hubrmvnet   = Get-AzVirtualNetwork -Name $HubRM -ResourceGroup $HubRG

Add-AzVirtualNetworkPeering `
  -Name HubRMToSpokeRM `
  -VirtualNetwork $hubrmvnet `
  -RemoteVirtualNetworkId "/subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/Spoke-Classic" `
  -AllowGatewayTransit
```

## <a name="next-steps"></a>Sonraki adımlar

* Üretimde kullanım için sanal ağ eşlemesi oluşturmadan önce [sanal ağ eşleme kısıtlamaları ve davranışları](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints) ile [sanal ağ eşleme ayarları](../virtual-network/virtual-network-manage-peering.md#create-a-peering) hakkında daha fazla bilgi edinin.
* Sanal ağ eşleme ve ağ geçidi aktarımı ile [merkez ve uç ağ topolojisi oluşturma](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) hakkında bilgi edinin.
