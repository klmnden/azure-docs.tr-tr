---
title: "Bir Azure VPN ağ geçidinde BGP yapılandırın: Resource Manager ve CLI | Microsoft Docs"
description: "Bu makalede Azure Resource Manager ve CLI kullanarak bir Azure VPN ağ geçidi ile BGP yapılandıracağınız anlatılmaktadır."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: cherylmc
ms.openlocfilehash: 98cd606ce930624ec5c591ffd8f13e0feae1a6c4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-bgp-on-an-azure-vpn-gateway-by-using-cli"></a>CLI kullanarak bir Azure VPN ağ geçidinde BGP yapılandırma

Bu makalede, bir şirket içi siteden siteye (S2S) VPN bağlantısı ve VNet-VNet bağlantı (diğer bir deyişle, sanal ağlar arasında bağlantı) BGP etkinleştirmek Azure Resource Manager dağıtım modeli ve Azure CLI kullanarak yardımcı olur.

## <a name="about-bgp"></a>BGP hakkında

BGP, internet üzerinde iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini değiştirmek için kullanılan standart yönlendirme protokolüdür. BGP, VPN ağ geçitleri ve BGP eşlikleri veya Komşuları olarak yollarını gönderip almak için çağrılır, şirket içi VPN cihazlarınızı etkinleştirir. Yollar her iki ağ geçidi kullanılabilirliği ve ulaşılabilirliği ağ geçitleri veya söz konusu yönlendiricilerden geçmeye önekler için hakkında bilgilendirmek. BGP ayrıca bir BGP ağ geçidinin bir BGP eşliğinden diğer tüm BGP eşleri öğrenir rotaları yayarak birden fazla ağ arasında geçiş yönlendirme etkinleştirebilirsiniz.

Daha fazla BGP ve BGP kullanma konuları ve teknik gereksinimleri anlamak için yararları hakkında bilgi için [Azure VPN gateways ile BGP'ye genel bakış](vpn-gateway-bgp-overview.md).

Bu makale, aşağıdaki görevleri ile yardımcı olur:

* [VPN ağ geçidinizi BGP etkinleştirme](#enablebgp) (gerekli)

  Ardından aşağıdaki bölümlerden birine veya her ikisi de da tamamlayın:

* [BGP şirketler arası bağlantı Kur](#crossprembgp)
* [BGP VNet-VNet bağlantı Kur](#v2vbgp)

Bu üç bölümlerin her birindeki ağ bağlantınızı BGP etkinleştirme için temel yapı bloğu oluşturur. Tüm üç bölüm tamamlarsanız, aşağıdaki çizimde gösterildiği gibi topoloji oluşturun:

![BGP topolojisi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

İhtiyaçlarınıza uygun daha karmaşık bir çoklu atlama transit ağ oluşturmak için bu bölümleri birleştirebilirsiniz.

## <a name ="enablebgp"></a>VPN ağ geçidiniz için BGP'yi etkinleştir

Bu bölümde, diğer iki yapılandırma bölümlerinin adımları gerçekleştirmeden önce gereklidir. Aşağıdaki yapılandırma adımlarını, aşağıdaki çizimde gösterildiği gibi Azure VPN ağ geçidi BGP parametreleri ayarlayın:

![BGP ağ geçidi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Başlamadan önce

CLI komutları (2.0 veya üstü) en son sürümünü yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](/cli/azure/install-azure-cli) ve [Azure CLI 2.0’ı Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).

### <a name="step-1-create-and-configure-testvnet1"></a>1. adım: Oluşturma ve testvnet1'i yapılandırma

#### <a name="Login"></a>1. Aboneliğinize bağlanma

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

#### <a name="2-create-a-resource-group"></a>2. Kaynak grubu oluşturma

Aşağıdaki örnekte "eastus" konumunda TestRG1 adlı bir kaynak grubu oluşturur. Sanal ağınızı oluşturmak istediğiniz bölgede bir kaynak grubu zaten varsa, bunun yerine bir kullanabilirsiniz.

```azurecli
az group create --name TestBGPRG1 --location eastus
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluşturma

Aşağıdaki örnekte TestVNet1 ve üç alt ağları adlı bir sanal ağ oluşturur: GatewaySubnet, ön uç ve arka uç. Değerleri değiştirerek, her zaman ağ geçidi alt ağınızı adlandırın önemli özellikle GatewaySubnet. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

İlk komut, ön uç adres alanı ve ön uç alt ağı oluşturur. İkinci komut arka uç alt ağ için ek adres alanı oluşturur. Üçüncü ve dördüncü komutlar GatewaySubnet ve arka uç alt ağ oluşturun.

```azurecli
az network vnet create -n TestVNet1 -g TestBGPRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24 
 
az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestBGPRG1 
 
az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestBGPRG1 --address-prefix 10.12.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestBGPRG1 --address-prefix 10.12.255.0/27 
```

### <a name="step-2-create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>2. adım: VPN ağ geçidi TestVNet1 için BGP parametrelerle oluşturma

#### <a name="1-create-the-public-ip-address"></a>1. Ortak IP adresi oluştur

Genel bir IP adresi isteyin. Sanal ağınız için oluşturduğunuz VPN ağ geçidi için genel IP adresi ayrılır.

```azurecli
az network public-ip create -n GWPubIP -g TestBGPRG1 --allocation-method Dynamic 
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. AS numarası ile VPN ağ geçidi oluşturma

TestVNet1 için sanal ağ geçidini oluşturun. BGP bir rota tabanlı VPN ağ geçidi gerektirir. Ek parametre etmeniz `-Asn` TestVNet1 için Otonom sistem numarası (ASN) ayarlamak için. Bir ağ geçidi oluşturma biraz zaman alabilir (45 dakika veya daha fazla) tamamlamak için. 

Bu komutu kullanarak çalıştırırsanız `--no-wait` parametresi, herhangi bir geri bildirim veya çıkış görmüyorum. `--no-wait` Parametresi arka planda oluşturulması için ağ geçidine izin verir. VPN ağ geçidini hemen oluşturduğunuz anlamına gelmez.

```azurecli
az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address GWPubIP -g TestBGPRG1 --vnet TestVNet1 --gateway-type Vpn --sku HighPerformance --vpn-type RouteBased --asn 65010 --no-wait
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Azure BGP eş IP adresi al

Ağ geçidi oluşturulduktan sonra IP adresi Azure VPN ağ geçidinde BGP eşinin edinmeniz gerekir. Bu adres için şirket içi VPN cihazlarınızı BGP eşi olarak VPN ağ geçidi yapılandırmak için gereklidir.

Aşağıdaki komutu çalıştırın ve denetleyin `bgpSettings` çıkışı üst kısmına:

```azurecli
az network vnet-gateway list -g TestBGPRG1 
 
  
"bgpSettings": { 
      "asn": 65010, 
      "bgpPeeringAddress": "10.12.255.30", 
      "peerWeight": 0 
    }
```

Ağ geçidi oluşturulduktan sonra bir şirket içi veya BGP ile VNet-VNet bağlantısı kurmak için bu ağ geçidi'ni kullanabilirsiniz.

## <a name ="crossprembgp"></a>BGP şirketler arası bağlantı Kur

Şirketler arası bağlantı kurmak için şirket içi VPN aygıtınızın temsil etmek için bir yerel ağ geçidi oluşturmanız gerekir. Ardından Azure VPN ağ geçidi ile yerel ağ geçidi bağlayın. Bu adımları diğer bağlantılar oluşturmak için benzer olsa da, bunlar BGP yapılandırma parametrelerini belirtmek için gereken ek özellikleri içerir.

![Şirket içi BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)


### <a name="step-1-create-and-configure-the-local-network-gateway"></a>1. adım: Oluşturma ve yerel ağ geçidi yapılandırma

Bu alıştırmada, aşağıdaki çizimde gösterilen yapılandırması oluşturmak devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun. Yerel ağ geçitleri ile çalışırken, şunları göz önünde bulundurun:

* Yerel ağ geçidi aynı konum ve VPN ağ geçidi olarak kaynak grubu olabilir veya farklı bir konuma ve kaynak grubu içinde olabilir. Bu örnekte ağ geçitleri farklı kaynak gruplarında farklı konumlarda gösterir.
* Yerel ağ geçidi için bildirmek için gereken minimum önek BGP eş IP adresinizin, VPN cihazınızdaki konak adresidir. Bu durumda, bir /32 olan 10.52.255.254/32 öneki.
* Bir anımsatıcı şirket içi ağlarınız ve Azure sanal ağı arasında farklı BGP Asn'ler kullanmanız gerekir. Aynı farklıysa, şirket içi VPN cihazlarınızın zaten ASN diğer BGP komşuları ile eşlenecek kullanırsanız, VNet ASN değiştirmeniz gerekir.

Devam etmeden önce tamamladığınız emin olun [VPN ağ geçidi için BGP etkinleştirme](#enablebgp) Bu alıştırmada bölümünü ve abonelik 1 olarak yine bağlandınız. Bu örnekte dikkat edin, yeni bir kaynak grubu oluşturun. Ayrıca, yerel ağ geçidi için iki ek parametreler dikkat edin: `Asn` ve `BgpPeerAddress`.

```azurecli
az group create -n TestBGPRG5 -l eastus2 
 
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site5 -g TestBGPRG5 --local-address-prefixes 10.51.255.254/32 --asn 65050 --bgp-peering-address 10.51.255.254
```

### <a name="step-2-connect-the-vnet-gateway-and-local-network-gateway"></a>2. adım: VNet ağ geçidi ve yerel ağ geçidi bağlanma

Bu adımda, bağlantı için Site5 TestVNet1 oluşturun. Belirtmeniz gerekir `--enable-bgp` Bu bağlantı için BGP'yi etkinleştirmek için parametre. 

Bu örnekte, sanal ağ geçidi ve yerel ağ geçidi farklı kaynak gruplarında olduğunda. Ağ geçitleri farklı kaynak gruplarında olduğunda, sanal ağlar arasında bir bağlantı ayarlamak için iki ağ geçidi tüm kaynak Kimliğini belirtmeniz gerekir.

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. Kaynak Kimliği, VNet1GW alma

Kaynak kimliği için VNet1GW almak için aşağıdaki komut çıktısı kullanın:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

Çıktıda Bul `"id":` satır. Sonraki bölümde bağlantısı oluşturmak için tırnak işaretleri içindeki değerleri gerekir.

Örnek çıktı:

```
{ 
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65010, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
  }, 
  "enableBgp": true, 
  "etag": "W/\"<your etag number>\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW",
```

Sonra değerleri kopyalamak `"id":` Not Defteri gibi bir metin düzenleyicisine böylece, kolayca bunları bağlantınızı oluştururken yapıştırabilirsiniz. 

```
"id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
```

#### <a name="2-get-the-resource-id-of-site5"></a>2. Kaynak Kimliği, Site5 alma

Kaynak Kimliği, Site5 çıktısını almak için aşağıdaki komutu kullanın:

```azurecli
az network local-gateway show -n Site5 -g TestBGPRG5
```

#### <a name="3-create-the-testvnet1-to-site5-connection"></a>3. TestVNet1 Site5 bağlantısı oluşturma

Bu adımda, bağlantı için Site5 TestVNet1 oluşturun. Daha önce bahsedildiği gibi aynı Azure VPN ağ geçidi için BGP ve BGP olmayan bağlantıları olması mümkündür. BGP bağlantı özelliği etkin değilse, BGP parametreleri her iki ağ geçidini zaten yapılandırılmış olsa bile Azure BGP Bu bağlantı için izin vermez. Abonelik kimlikleri kendi ile değiştirin.

```azurecli
az network vpn-connection create -n VNet1ToSite5 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "abc123" --local-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG5/providers/Microsoft.Network/localNetworkGateways/Site5 --no-wait
```

Bu alıştırmada, aşağıdaki örnekte, şirket içi VPN cihazınızın BGP yapılandırma bölümünde girmek için parametreleri listelenir:

```
Site5 ASN            : 65050
Site5 BGP IP         : 10.52.255.254
Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
Azure VNet ASN       : 65010
Azure VNet BGP IP    : 10.12.255.30
Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bağlantı birkaç dakika içerisinde kurulacaktır. IPSec bağlantısı kurulduktan sonra BGP eşliği oturumu başlatır.

## <a name ="v2vbgp"></a>BGP VNet-VNet bağlantı Kur

Bu bölüm, aşağıdaki çizimde gösterildiği gibi BGP ile VNet-VNet bağlantı ekler: 

![BGP VNet-VNet için](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Aşağıdaki yönergeler, önceki bölümlerde adımlardan devam edin. Oluşturup TestVNet1 ve VPN ağ geçidi BGP ile yapılandırmak için tamamlamanız gereken [VPN ağ geçidi için BGP etkinleştirme](#enablebgp) bölümü.

### <a name="step-1-create-testvnet2-and-the-vpn-gateway"></a>1. adım: TestVNet2 ve VPN ağ geçidi oluşturma

Yeni sanal ağ TestVNet2, IP adres alanının herhangi bir VNet aralıkları ile çakışmaması emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneliğe ait. VNet-VNet bağlantıları farklı abonelikler arasında ayarlayabilirsiniz. Daha fazla bilgi için bkz: [VNet-VNet bağlantı yapılandırma](vpn-gateway-howto-vnet-vnet-cli.md). Eklediğiniz emin olun `-EnableBgp $True` BGP'yi etkinleştirmek için bir bağlantı oluşturulurken.

#### <a name="1-create-a-new-resource-group"></a>1. Yeni bir kaynak grubu oluşturma

```azurecli
az group create -n TestBGPRG2 -l westus
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Yeni kaynak grubunda TestVNet2 oluşturma

İlk komut, ön uç adres alanı ve ön uç alt ağı oluşturur. İkinci komut arka uç alt ağ için ek adres alanı oluşturur. Üçüncü ve dördüncü komutlar GatewaySubnet ve arka uç alt ağ oluşturun.

```azurecli
az network vnet create -n TestVNet2 -g TestBGPRG2 --address-prefix 10.21.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.21.0.0/24 
 
az network vnet update -n TestVNet2 --address-prefixes 10.21.0.0/16 10.22.0.0/16 -g TestBGPRG2 
 
az network vnet subnet create --vnet-name TestVNet2 -n BackEnd -g TestBGPRG2 --address-prefix 10.22.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet2 -n GatewaySubnet -g TestBGPRG2 --address-prefix 10.22.255.0/27
```

#### <a name="3-create-the-public-ip-address"></a>3. Ortak IP adresi oluştur

Genel bir IP adresi isteyin. Sanal ağınız için oluşturduğunuz VPN ağ geçidi için genel IP adresi ayrılır.

```azurecli
az network public-ip create -n GWPubIP2 -g TestBGPRG2 --allocation-method Dynamic
```

#### <a name="4-create-the-vpn-gateway-with-the-as-number"></a>4. AS numarası ile VPN ağ geçidi oluşturma

Sanal ağ geçidi için TestVNet2 oluşturun. Varsayılan ASN, Azure VPN ağ geçitlerinde geçersiz kılmanız gerekir. Bağlı sanal ağlar için Asn'ler BGP ve transit yönlendirme etkinleştirmek için farklı olmalıdır.
 
```azurecli
az network vnet-gateway create -n VNet2GW -l westus --public-ip-address GWPubIP2 -g TestBGPRG2 --vnet TestVNet2 --gateway-type Vpn --sku Standard --vpn-type RouteBased --asn 65020 --no-wait
```

### <a name="step-2-connect-the-testvnet1-and-testvnet2-gateways"></a>2. adım: TestVNet1 ve TestVNet2 ağ geçitleri bağlanma

Bu adımda, bağlantı için Site5 TestVNet1 oluşturun. Bu bağlantı için BGP'yi etkinleştirmek için belirtmelisiniz `--enable-bgp` parametresi.

Aşağıdaki örnekte, sanal ağ geçidi ve yerel ağ geçidi farklı kaynak gruplarında olduğunda. Ağ geçitleri farklı kaynak gruplarında olduğunda, sanal ağlar arasında bir bağlantı ayarlamak için iki ağ geçidi tüm kaynak Kimliğini belirtmeniz gerekir. 

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. Kaynak Kimliği, VNet1GW alma 

Kaynak Kimliği VNet1GW, aşağıdaki komut çıktısı alın:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

#### <a name="2-get-the-resource-id-of-vnet2gw"></a>2. Kaynak Kimliği, VNet2GW alma

Kaynak Kimliği VNet2GW, aşağıdaki komut çıktısı alın:

```azurecli
az network vnet-gateway show -n VNet2GW -g TestBGPRG2
```

#### <a name="3-create-the-connections"></a>3. Bağlantıları oluşturma

TestVNet1 bağlantısı TestVNet2 için ve bağlantı TestVNet2 testvnet1-oluşturun. Abonelik kimlikleri kendi ile değiştirin.

```azurecli
az network vpn-connection create -n VNet1ToVNet2 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW
```

```azurecli
az network vpn-connection create -n VNet2ToVNet1 -g TestBGPRG2 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW --enable-bgp -l westus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
```

> [!IMPORTANT]
> BGP için etkinleştirme *her ikisi de* bağlantıları.
> 
> 

Bu adımları tamamladıktan sonra birkaç dakika içerisinde bağlantı kurulur. VNet-VNet bağlantı tamamlandıktan sonra BGP eşdeğer oturumu yukarı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz: [bir sanal makine oluşturmak](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
