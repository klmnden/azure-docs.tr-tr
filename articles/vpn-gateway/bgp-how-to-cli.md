---
title: 'Bir Azure VPN ağ geçidinde BGP yapılandırın: Resource Manager ve CLI | Microsoft Docs'
description: Bu makalede Azure Resource Manager ve CLI kullanarak bir Azure VPN ağ geçidi ile BGP yapılandıracağınız açıklanmaktadır.
services: vpn-gateway
documentationcenter: na
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/25/2018
ms.author: yushwang
ms.openlocfilehash: f0367a360de97d3935c7fa8de9f3dafa6555811e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60390685"
---
# <a name="how-to-configure-bgp-on-an-azure-vpn-gateway-by-using-cli"></a>CLI kullanarak bir Azure VPN ağ geçidinde BGP yapılandırma

Bu makalede, şirket içi siteden siteye (S2S) VPN bağlantı ve VNet-VNet bağlantısı (diğer bir deyişle, sanal ağlar arasında bağlantı) BGP'yi etkinleştirmek Azure Resource Manager dağıtım modelini ve Azure CLI kullanarak yardımcı olur.

## <a name="about-bgp"></a>BGP hakkında

BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini değiştirmek için internet'te yaygın olarak kullanılan standart yönlendirme protokolüdür. VPN ağ geçitleri ve BGP eşlikleri veya Komşuları, yollarını gönderip almak adlı şirket içi VPN cihazlarınızı BGP sağlar. Yolları iki ağ geçidi kullanılabilirliği ve ulaşılabilirliği ağ geçitlerinden veya yönlendiricilerden ilgili gitmek ön ekleri için ilgili bilgilendirin. BGP ayrıca bir BGP ağ geçidinin bir BGP eşliğinden diğer tüm BGP eşleri öğrenir rotaları yayma ile birden fazla ağ arasında geçiş yönlendirmesi etkinleştirebilirsiniz.

Daha fazla BGP ve BGP kullanma konuları ve teknik gereksinimleri anlamak için avantajları hakkında bilgi için [Azure VPN gateways ile BGP'ye genel bakış](vpn-gateway-bgp-overview.md).

Bu makale aşağıdaki görevlerde size yardımcı olur:

* [VPN ağ geçidiniz için BGP'yi etkinleştir](#enablebgp) (gerekli)

  Ardından, aşağıdaki bölümlerde birini veya her ikisini de tamamlayabilirsiniz:

* [BGP ile şirketler arası bağlantı kurun](#crossprembgp)
* [BGP ile VNet-VNet bağlantı kurun](#v2vbgp)

Bu üç bölümlerin her birinde, ağ bağlantınızı BGP etkinleştirmek için temel yapı bloğu oluşturur. Üç tüm bölümleri tamamlayın, aşağıdaki diyagramda gösterildiği gibi topoloji derleme:

![BGP topolojisi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

İhtiyaçlarınıza uygun daha karmaşık bir çoklu atlama aktarım ağı oluşturmak için bu bölümleri birleştirebilirsiniz.

## <a name ="enablebgp"></a>VPN ağ geçidiniz için BGP'yi etkinleştir

Bu bölümde, diğer iki yapılandırma bölümlerinde adımları gerçekleştirmeden önce gereklidir. Aşağıdaki yapılandırma adımları aşağıdaki diyagramda gösterildiği gibi Azure VPN ağ geçidinin BGP parametreleri ayarlayın:

![BGP ağ geçidi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Başlamadan önce

(2.0 veya üzeri) CLI komutlarının en son sürümünü yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli) ve [Azure CLI’yi Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).

### <a name="step-1-create-and-configure-testvnet1"></a>1. Adım: TestVNet1’i oluşturma ve yapılandırma

#### <a name="Login"></a>1. Aboneliğinize bağlanma

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

#### <a name="2-create-a-resource-group"></a>2. Kaynak grubu oluşturma

Aşağıdaki örnek, "eastus" konumunda TestRG1'adlı bir kaynak grubu oluşturur. Sanal ağınızı oluşturmak için istediğiniz bölgede zaten bir kaynak grubu varsa, bunun yerine bunu kullanabilirsiniz.

```azurecli
az group create --name TestBGPRG1 --location eastus
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluşturma

Aşağıdaki örnekte TestVNet1 ve üç alt adlı bir sanal ağ oluşturur: GatewaySubnet, ön uç ve arka uç. Değerleri değiştirirken her zaman ağ geçidi alt ağınızı adlandırın önemli olduğu özellikle GatewaySubnet olarak. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

İlk komut, ön uç adres alanı ve ön uç alt ağı oluşturur. İkinci komut, arka uç alt ağı için ek adres alanı oluşturur. Üçüncü ve dördüncü komutlar GatewaySubnet ve arka uç alt ağı oluşturur.

```azurecli
az network vnet create -n TestVNet1 -g TestBGPRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24 
 
az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestBGPRG1 
 
az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestBGPRG1 --address-prefix 10.12.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestBGPRG1 --address-prefix 10.12.255.0/27 
```

### <a name="step-2-create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>2. Adım: BGP parametrelerle TestVNet1 için VPN ağ geçidi oluşturun

#### <a name="1-create-the-public-ip-address"></a>1. Genel IP adresi oluşturma

Genel bir IP adresi isteyin. Sanal ağınız için oluşturduğunuz VPN ağ geçidi genel IP adresi ayrılır.

```azurecli
az network public-ip create -n GWPubIP -g TestBGPRG1 --allocation-method Dynamic 
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. VPN ağ geçidi AS numarası ile oluşturma

TestVNet1 için sanal ağ geçidini oluşturun. BGP rota tabanlı VPN ağ geçidi gerektirir. Ayrıca ek bir parametre gerekir `-Asn` TestVNet1 için Otonom sistem numarası (ASN) ayarlamak için. Bir ağ geçidini oluşturmak biraz zaman alabilir (45 dakika veya daha fazla) tamamlayın. 

Bu komutu kullanarak çalıştırırsanız `--no-wait` parametresi, tüm geri bildirim veya çıktı görmezsiniz. `--no-wait` Parametresi, ağ geçidinin arka planda oluşturulmasına olanak sağlar. VPN ağ geçidini hemen oluşturduğunuz anlamına gelmez.

```azurecli
az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address GWPubIP -g TestBGPRG1 --vnet TestVNet1 --gateway-type Vpn --sku HighPerformance --vpn-type RouteBased --asn 65010 --no-wait
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Azure BGP eş IP adresini alın.

Ağ geçidi oluşturulduktan sonra Azure VPN ağ geçidi IP adresi BGP eşinin edinmeniz gerekir. Bu adres, VPN ağ geçidi BGP eşi için şirket içi VPN cihazlarınızı yapılandırmak için gereklidir.

Aşağıdaki komutu çalıştırın ve kontrol `bgpSettings` çıkış üst kısmına:

```azurecli
az network vnet-gateway list -g TestBGPRG1 
 
  
"bgpSettings": { 
      "asn": 65010, 
      "bgpPeeringAddress": "10.12.255.30", 
      "peerWeight": 0 
    }
```

Ağ geçidi oluşturulduktan sonra içi ve dışı karışık bağlantı veya BGP ile VNet-VNet bağlantısı kurmak için bu ağ geçidi'ni kullanabilirsiniz.

## <a name ="crossprembgp"></a>BGP ile şirketler arası bağlantı kurun

Şirketler arası bağlantı kurmak için şirket içi VPN Cihazınızı temsil etmek için bir yerel ağ geçidi oluşturmanız gerekir. Ardından Azure VPN ağ geçidi ile yerel ağ geçidi bağlayın. Bu adımları diğer bağlantıları oluşturmaya benzer olsa da, bunlar BGP yapılandırma parametreleri belirtmek için gereken ek özellikleri içerir.

![BGP'yi şirketler için](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)


### <a name="step-1-create-and-configure-the-local-network-gateway"></a>1. Adım: Oluşturma ve yerel ağ geçidi yapılandırma

Bu alıştırmada, derleme yapılandırması Aşağıdaki diyagramda gösterilen devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun. Yerel ağ geçitleri ile çalışırken şunları göz önünde bulundurun:

* Yerel ağ geçidi aynı konum ve kaynak grubunda VPN Gateway olabilir veya farklı bir konum ve kaynak grubu içinde olabilir. Bu örnekte ağ geçitleri farklı kaynak gruplarında farklı konumlarda gösterilir.
* Yerel ağ geçidi için bildirmek için gereken en düşük ön ek VPN cihazınızın BGP eş IP adresiniz konak adresidir. Bu durumda, bir özelliğini/32 olduğu 10.52.255.254/32 öneki.
* Bir anımsatıcı şirket içi ağlarınız ve Azure sanal ağı arasında farklı BGP Asn'ler kullanmanız gerekir. Aynı olmaları durumunda, şirket içi VPN cihazlarınız ile diğer BGP komşu eşlenecek ASN'yi zaten kullanıyorsanız, VNet ASN'nizi değiştirmeniz gerekir.

Devam etmeden önce tamamladığınızdan emin olun [VPN ağ geçidi için BGP etkinleştir](#enablebgp) bölümü Bu alıştırmada, ve 1. Abonelik'e hala bağlı olduğunuz. Bu örnekte dikkat edin, yeni bir kaynak grubu oluşturun. Ayrıca, yerel ağ geçidi için iki ek parametreler dikkat edin: `Asn` ve `BgpPeerAddress`.

```azurecli
az group create -n TestBGPRG5 -l eastus2 
 
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site5 -g TestBGPRG5 --local-address-prefixes 10.51.255.254/32 --asn 65050 --bgp-peering-address 10.51.255.254
```

### <a name="step-2-connect-the-vnet-gateway-and-local-network-gateway"></a>2. Adım: VNet ağ geçidi ve yerel ağ geçidine bağlanma

Bu adımda, bağlantı için Site5 testvnet1-oluşturursunuz. Belirtmelisiniz `--enable-bgp` Bu bağlantı için BGP'yi etkinleştirmek için parametre. 

Bu örnekte, sanal ağ geçidi ve yerel ağ geçidi farklı kaynak gruplarındadır. Ağ geçitleri farklı kaynak gruplarında olduğunda, sanal ağlar arasında bir bağlantı ayarlamak için iki ağ geçidi tüm kaynak Kimliğini belirtmeniz gerekir.

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. Kimliği VNet1GW öğesinin kaynak alma

VNet1GW için kaynak Kimliğini almak için çıktısı, aşağıdaki komutu kullanın:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

Çıktıda Bul `"id":` satır. Sonraki bölümde bağlantıyı oluşturmak için tırnak içindeki değerler gerekir.

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

Sonra değerleri kopyalayın `"id":` Not Defteri gibi bir metin düzenleyicisi ve böylece, kolayca bunları bağlantınızı oluştururken yapıştırabilirsiniz. 

```
"id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
```

#### <a name="2-get-the-resource-id-of-site5"></a>2. ' % S'kaynak kimliği, Site5 alma

' % S'kaynak kimliği, Site5 çıktısını almak için aşağıdaki komutu kullanın:

```azurecli
az network local-gateway show -n Site5 -g TestBGPRG5
```

#### <a name="3-create-the-testvnet1-to-site5-connection"></a>3. TestVNet1 Site5 bağlantı oluşturma

Bu adımda, bağlantı için Site5 testvnet1-oluşturursunuz. Daha önce bahsedildiği gibi aynı Azure VPN ağ geçidi için BGP ve BGP olmayan bağlantıları olması mümkündür. BGP bağlantı özelliği etkin değilse, her iki ağ geçitlerinde BGP parametreleri zaten yapılandırılmış olsa bile Azure BGP Bu bağlantı için etkin değildir. Abonelik kimlikleri kendi değerlerinizle değiştirin.

```azurecli
az network vpn-connection create -n VNet1ToSite5 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "abc123" --local-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG5/providers/Microsoft.Network/localNetworkGateways/Site5 --no-wait
```

Bu alıştırma için aşağıdaki örnekte şirket içi VPN cihazınızın BGP yapılandırma bölümünde girmenizi parametreleri listelenmektedir:

```
Site5 ASN            : 65050
Site5 BGP IP         : 10.52.255.254
Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
Azure VNet ASN       : 65010
Azure VNet BGP IP    : 10.12.255.30
Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bağlantı birkaç dakika içerisinde kurulacaktır. IPSec bağlantı kurulduktan sonra BGP eşdeğer oturumu başlatır.

## <a name ="v2vbgp"></a>BGP ile VNet-VNet bağlantı kurun

Bu bölümde, aşağıdaki diyagramda gösterildiği gibi bir VNet-VNet bağlantısı BGP ile ekler: 

![VNet-VNet için BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Aşağıdaki yönergeler, önceki bölümlerde yer alan adımların devam edin. Oluşturma ve TestVNet1 ve VPN ağ geçidi ile BGP yapılandırma için tamamlamanız gereken [VPN ağ geçidi için BGP etkinleştir](#enablebgp) bölümü.

### <a name="step-1-create-testvnet2-and-the-vpn-gateway"></a>1. Adım: TestVNet2 ve VPN ağ geçidi oluşturma

IP adres alanı yeni sanal ağ TestVNet2, tüm sanal ağ Aralıklarınızın çakışmadığını emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneliğe ait. Farklı abonelikler arasında VNet-VNet bağlantılarında ayarlayabilirsiniz. Daha fazla bilgi için bkz. [bir VNet-VNet bağlantısını yapılandırma](vpn-gateway-howto-vnet-vnet-cli.md). Eklediğiniz emin `-EnableBgp $True` BGP'yi etkinleştirmek için bir bağlantı oluşturulurken.

#### <a name="1-create-a-new-resource-group"></a>1. Yeni bir kaynak grubu oluşturma

```azurecli
az group create -n TestBGPRG2 -l westus
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Yeni kaynak grubunda TestVNet2 oluşturma

İlk komut, ön uç adres alanı ve ön uç alt ağı oluşturur. İkinci komut, arka uç alt ağı için ek adres alanı oluşturur. Üçüncü ve dördüncü komutlar GatewaySubnet ve arka uç alt ağı oluşturur.

```azurecli
az network vnet create -n TestVNet2 -g TestBGPRG2 --address-prefix 10.21.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.21.0.0/24 
 
az network vnet update -n TestVNet2 --address-prefixes 10.21.0.0/16 10.22.0.0/16 -g TestBGPRG2 
 
az network vnet subnet create --vnet-name TestVNet2 -n BackEnd -g TestBGPRG2 --address-prefix 10.22.0.0/24 
 
az network vnet subnet create --vnet-name TestVNet2 -n GatewaySubnet -g TestBGPRG2 --address-prefix 10.22.255.0/27
```

#### <a name="3-create-the-public-ip-address"></a>3. Genel IP adresi oluşturma

Genel bir IP adresi isteyin. Sanal ağınız için oluşturduğunuz VPN ağ geçidi genel IP adresi ayrılır.

```azurecli
az network public-ip create -n GWPubIP2 -g TestBGPRG2 --allocation-method Dynamic
```

#### <a name="4-create-the-vpn-gateway-with-the-as-number"></a>4. VPN ağ geçidi AS numarası ile oluşturma

Sanal ağ geçidi için TestVNet2 oluşturun. ASN varsayılan Azure VPN ağ geçitlerinizi üzerinde geçersiz kılmanız gerekir. Bağlı sanal ağlar için Asn'ler BGP ve geçiş yönlendirmesi'ni etkinleştirmek için farklı olmalıdır.
 
```azurecli
az network vnet-gateway create -n VNet2GW -l westus --public-ip-address GWPubIP2 -g TestBGPRG2 --vnet TestVNet2 --gateway-type Vpn --sku Standard --vpn-type RouteBased --asn 65020 --no-wait
```

### <a name="step-2-connect-the-testvnet1-and-testvnet2-gateways"></a>2. Adım: TestVNet1 ve TestVNet2 ağ geçitlerini bağlama

Bu adımda, bağlantı için Site5 testvnet1-oluşturursunuz. Bu bağlantı için BGP'yi etkinleştirmek için belirtmelisiniz `--enable-bgp` parametresi.

Aşağıdaki örnekte, sanal ağ geçidi ve yerel ağ geçidi farklı kaynak gruplarındadır. Ağ geçitleri farklı kaynak gruplarında olduğunda, sanal ağlar arasında bir bağlantı ayarlamak için iki ağ geçidi tüm kaynak Kimliğini belirtmeniz gerekir. 

#### <a name="1-get-the-resource-id-of-vnet1gw"></a>1. Kimliği VNet1GW öğesinin kaynak alma 

Aşağıdaki komutun çıktısından VNet1GW öğesinin kimliği kaynak alın:

```azurecli
az network vnet-gateway show -n VNet1GW -g TestBGPRG1
```

#### <a name="2-get-the-resource-id-of-vnet2gw"></a>2. ' % S'kaynak kimliği, VNet2GW alma

' % S'kaynak kimliği, VNet2GW aşağıdaki komutun çıktısından alın:

```azurecli
az network vnet-gateway show -n VNet2GW -g TestBGPRG2
```

#### <a name="3-create-the-connections"></a>3. Bağlantıları oluşturma

TestVNet1 bağlantısından TestVNet2 ve bağlantı TestVNet2 TestVNet1'e oluşturun. Abonelik kimlikleri kendi değerlerinizle değiştirin.

```azurecli
az network vpn-connection create -n VNet1ToVNet2 -g TestBGPRG1 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW --enable-bgp -l eastus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW
```

```azurecli
az network vpn-connection create -n VNet2ToVNet1 -g TestBGPRG2 --vnet-gateway1 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG2/providers/Microsoft.Network/virtualNetworkGateways/VNet2GW --enable-bgp -l westus --shared-key "efg456" --vnet-gateway2 /subscriptions/<subscription ID>/resourceGroups/TestBGPRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
```

> [!IMPORTANT]
> İçin BGP'yi etkinleştir *hem* bağlantıları.
> 
> 

Bu adımları tamamladıktan sonra birkaç dakika sonra bağlantı kurulur. VNet-VNet bağlantısı tamamlandıktan sonra BGP eşdeğer oturumu ayarlama olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz: [sanal makine oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
