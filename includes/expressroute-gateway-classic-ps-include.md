---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 12/13/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 70ac106995324c758bde942d12191a01e3457e6e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60365171"
---
> [!NOTE]
> Bu örnekler S2S/ExpressRoute için geçerli değildir yapılandırmaları bir arada.
> Coexist yapılandırmasında ağ geçitleri ile çalışma hakkında daha fazla bilgi için bkz. [arada var olabilen bağlantılar yapılandırma.](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Ağ geçidi ekleme

Klasik kaynak modeli kullanarak bir sanal ağ geçidi eklediğinizde, doğrudan ağ geçidi oluşturmadan önce ağ yapılandırma dosyasını değiştirin. Değerleri aşağıdaki örneklerde bir ağ geçidi oluşturmak için dosyasında mevcut olmalıdır. Sanal ağınıza daha önce ilişkili bir ağ geçidi varsa, bu değerlerden bazıları zaten mevcut olacaktır. Aşağıdaki değerleri yansıtacak şekilde dosyasını değiştirin.

### <a name="download-the-network-configuration-file"></a>Ağ yapılandırma dosyasını indirin

1. İçindeki adımları kullanarak ağ yapılandırma dosyasını indirme [ağ yapılandırma dosyasını](../articles/virtual-network/virtual-networks-using-network-configuration-file.md) makalesi. Bir metin düzenleyicisi kullanarak dosyayı açın.
2. Bir yerel ağ alanına dosyaya ekleyin. Herhangi bir geçerli adresi önek kullanabilirsiniz. VPN ağ geçidi herhangi bir geçerli IP adresi ekleyebilirsiniz. Bu bölümdeki adres değerlerini ExpressRoute işlemleri için kullanılmaz, ancak dosya doğrulama için gereklidir. Örnekte, "branch1" sitenin adıdır. Farklı bir ad kullanın ancak aynı değeri ağ geçidi bölümünde kullandığınızdan emin olun.

   ```
   <VirtualNetworkConfiguration>
    <Dns />
    <LocalNetworkSites>
      <LocalNetworkSite name="branch1">
        <AddressSpace>
          <AddressPrefix>165.3.1.0/27</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>3.2.1.4</VPNGatewayAddress>
    </LocalNetworkSite>
   ```
3. İçin VirtualNetworkSites gidin ve alanları değiştirin.

   * Ağ geçidi alt ağı, sanal ağınıza ilişkin mevcut olduğunu doğrulayın. Kullanmıyorsa, şu anda ekleyebilirsiniz. Adı "GatewaySubnet" olmalıdır.
   * Ağ geçidi kısmında var olduğunu doğrulayın. Seçili değilse bunu ekleyin. Bu sanal ağ (bağlanmakta olduğunuz ağ temsil eden) yerel ağ alanı ile ilişkilendirmek için gereklidir.
   * Bağlantı türü doğrulayın adanmış =. Bu, ExpressRoute bağlantıları için gereklidir.

   ```
   </LocalNetworkSites>
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myAzureVNET" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="default">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.1.0/27</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="branch1">
              <Connection type="Dedicated" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
   </VirtualNetworkConfiguration>
   </NetworkConfiguration>
   ```
4. Dosyayı kaydedin ve Azure'a yükleyin.

### <a name="create-the-gateway"></a>Ağ geçidi oluşturma

Bir ağ geçidi oluşturmak için aşağıdaki komutu kullanın. Tüm değerleri kendi değiştirin.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulayın.

Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın. Bu komut ayrıca diğer işlemler için gereken ağ geçidi kimliği alır.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Bir ağ geçidini yeniden boyutlandırın

Bir dizi vardır [ağ geçidi SKU'ları](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Herhangi bir zamanda ağ geçidi SKU'suna değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut, UltraPerformance ağ geçidi çalışmaz. Ağ geçidinize bir UltraPerformance ağ geçidi değiştirmek için önce var olan ExpressRoute ağ geçidini kaldırın ve ardından yeni bir UltraPerformance ağ geçidi oluşturun. Bir UltraPerformance ağ geçidi geçidinizden düşürmek için ilk UltraPerformance ağ geçidi kaldırın ve ardından yeni bir ağ geçidi oluşturun.
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Ağ geçitlerini kaldırma

Bir ağ geçidini kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```