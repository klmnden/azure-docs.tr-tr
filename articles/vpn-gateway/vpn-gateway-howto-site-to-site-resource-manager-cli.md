---
title: "Şirket içi ağınızı bir Azure sanal ağına bağlama: Siteden Siteye VPN: CLI | Microsoft Docs"
description: "Şirket içi ağınız ile bir Azure sanal ağı arasında genel İnternet üzerinden bir IPSec bağlantısı oluşturma adımları. Bu adımlar CLI kullanarak Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı oluşturmanıza yardımcı olur."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c3563f3a3fa46d40ba02fe97b3b0ebe3c45caddd
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>CLI kullanarak Siteden Siteye VPN bağlantısı olan bir sanal ağ oluşturma

Bu makalede, Azure CLI kullanarak şirket içi ağınızdan VNet’e Siteden Siteye VPN ağ geçidi bağlantısı oluşturma işlemi gösterilir. Bu makaledeki adımlar Resource Manager dağıtım modeli için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager - CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klasik - Azure portalı](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasik - klasik portal](vpn-gateway-site-to-site-create.md)
> 
>


![Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-connection-diagram.png)

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN ağ geçidi hakkında](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Resource Manager dağıtım modeliyle çalışmak istediğinizi doğrulayın. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Uyumlu VPN cihazları ve cihaz yapılandırması hakkında daha fazla bilgi için bkz.[VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md).
* VPN cihazınız için dışarıya yönelik genel bir IPv4 adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
* Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir. Bu yapılandırmayı oluşturduğunuzda, Azure’un şirket içi konumunuza yönlendireceği IP adres aralığı ön eklerini oluşturmanız gerekir. Şirket içi ağınızın alt ağlarından hiçbiri, bağlanmak istediğiniz sanal ağ alt ağlarıyla çakışamaz. 
* CLI komutlarının en son sürümü (2.0 veya üzeri). CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="example-values"></a>Örnek değerler

Aşağıdaki değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.12.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.12.0.0/24 
GatewaySubnet           = 10.12.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24 
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Azure'da oturum açma

[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini listeleyin.

```azurecli
Az account list --all
```

Kullanmak istediğiniz aboneliği belirtin.

```azurecli
Az account set --subscription <replace_with_your_subscription_id>
```

## <a name="2-create-a-resource-group"></a>2. Kaynak grubu oluşturma

Aşağıdaki örnekte, 'eastus' konumunda 'TestRG1' adlı bir kaynak grubu oluşturulur. VNet’inizi oluşturmak istediğiniz bölgede zaten bir kaynak grubunuz varsa, bunun yerine onu da kullanabilirsiniz.

```azurecli
az group create -n TestRG1 -l eastus
```

## <a name="VNet"></a>3. Sanal ağ oluşturma

Sanal ağınız yoksa bir sanal ağ oluşturun. Sanal ağ oluştururken, belirlediğiniz adres alanlarının şirket içi ağınızdaki adres alanlarından herhangi biriyle çakışmadığından emin olun. 

Aşağıdaki örnekte, 'TestVNet1' adlı bir sanal ağ ve 'Subnet1' adlı alt ağ oluşturulur.

```azurecli
az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.12.0.0/16 -l eastus --subnet-name Subnet1 --subnet-prefix 10.12.0.0/24
```

## 4. <a name="gwsub"></a>Ağ geçidi alt ağını oluşturma

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Bu yapılandırma için, bir ağ geçidi alt ağına da ihtiyacınız olacaktır. Sanal ağın ağ geçidi, VPN ağ geçidi hizmetleri tarafından kullanılan IP adreslerinin bulunduğu ağ geçidi alt ağını kullanır. Ağ geçidi alt ağını oluşturduğunuzda, bu alt ağ 'GatewaySubnet' olarak adlandırılmalıdır. Başka bir ad kullanırsanız alt ağ oluşturulur ancak Azure bunu bir ağ geçidi alt ağı olarak değerlendirmez.

Belirttiğiniz ağ geçidi alt ağının boyutu, oluşturmak istediğiniz VPN ağ geçidi yapılandırmasına bağlıdır. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkün olsa da, /27 veya /28’i seçerek daha fazla adres içeren büyük bir alt ağ oluşturmanızı öneririz. Daha büyük bir ağ geçidi alt ağı kullanmak, olası gelecek yapılandırmaları barındırmak için yeterli IP adresi bulunmasını sağlar.


```azurecli
az network vnet subnet create --address-prefix 10.12.255.0/27 -n GatewaySubnet -g TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Yerel ağ geçidini oluşturma

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Siteye Azure’un başvuruda bulunmak için kullanabileceği bir ad verir, ardından bağlantı oluşturacağınız şirket içi VPN cihazının IP adresini belirtirsiniz. Ayrıca, VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP adresi ön eklerini de belirtirsiniz. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir. Şirket içi ağınız değişirse, ön ekleri kolayca güncelleştirebilirsiniz.

Aşağıdaki değerleri kullanın:

* *--gateway-ip-address* şirket içi VPN cihazınızın IP adresidir. VPN cihazınız bir NAT’nin arkasında olamaz.
* *--local-address-prefixes* şirket içi adres alanlarınızdır.

Aşağıdaki örnekte, birden çok adres ön ekine sahip bir yerel ağ geçidinin nasıl eklendiği gösterilir:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Genel bir IP adresi talep etme

Sanal ağ VPN ağ geçidinize genel bir IP adresinin ayrılmasını isteyin. Bu, bağlanmak üzere VPN cihazınızı yapılandırdığınız IP adresidir.

Resource Manager dağıtım modeline ait sanal ağ geçidi, genel IP adreslerini şu anda yalnızca Dinamik Ayırma yöntemini kullanarak desteklemektedir. Ancak bu, IP adresinin değiştiği anlamına gelmez. VPN ağ geçidi IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. Sanal ağ geçidi genel IP adresi, VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez. 

```azurecli
az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. VPN ağ geçidini oluşturma

Sanal ağ VPN ağ geçidini oluşturun. VPN ağ geçidi oluşturma işleminin tamamlanması 45 dakika veya daha uzun sürebilir.

Aşağıdaki değerleri kullanın:

* Siteden Siteye bir yapılandırma için *--gateway-type* değeri *Vpn* olmalıdır. Ağ geçidi türü her zaman, uygulamakta olduğunuz yapılandırmaya özeldir. Daha fazla bilgi için bkz. [Ağ geçidi türleri](vpn-gateway-about-vpn-gateway-settings.md#gwtype)
* *-vpn-type*, *RouteBased* (bazı belgelerde Dinamik Ağ Geçidi olarak adlandırılır) veya *PolicyBased* (bazı belgelerde Statik Ağ Geçidi olarak adlandırılır) olabilir. Ayar, bağlamakta olduğunuz cihazın gereksinimlerine özgüdür. VPN ağ geçidi türleri hakkında daha fazla bilgi için bkz. [VPN Gateway yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* *--sku* değeri Basic, Standard veya HighPerformance olabilir. Bazı SKU’larda yapılandırma sınırlamaları vardır. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gateway-skus).

Bu komutu çalıştırdıktan sonra, herhangi bir geri bildirim veya çıkış görmezsiniz. Ağ geçidinin oluşturulması 45 dakika kadar sürebilir.

```azurecli
az network vnet-gateway create -n VNet1GW --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku Standard --no-wait 
```

## <a name="VPNDevice"></a>8. VPN cihazınızı yapılandırma

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]
  Sanal ağ geçidinizin genel IP adresini bulmak için, değerleri kendi değerlerinizle değiştirerek aşağıdaki örneği kullanın. Kolay okunması için, çıkış genel IP’lerin tablo biçiminde gösterileceği şekilde biçimlendirilmiştir.

  ```azurecli
  az network public-ip list -g TestRG1 -o table
  ```

## <a name="CreateConnection"></a>9. VPN bağlantısını oluşturma

Sanal ağ geçidiniz ile şirket içi VPN cihazınız arasında Siteden Siteye VPN bağlantısı oluşturun. Paylaşılan anahtar değerine özellikle dikkat edin; bu değer VPN cihazınız için yapılandırılmış paylaşılan anahtar değeriyle eşleşmelidir.

```azurecli
az network vpn-connection create -n VNet1toSite2 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Kısa bir süre içerisinde bağlantı kurulur.

## <a name="toverify"></a>10. VPN bağlantısını doğrulama

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)] 

Bağlantınızı doğrulamak için başka bir yöntem kullanmak istiyorsanız, bkz. [VPN Gateway bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).

## <a name="common-tasks"></a>Genel görevler

### <a name="to-view-local-network-gateways"></a>Yerel ağ geçitlerini görüntülemek için

```azurecli
az network local-gateway list --resource-group TestRG1
```

### <a name="modify"></a>Yerel bir ağ geçidinin IP adresi ön eklerini değiştirmek için
Yerel ağ geçidiniz için ön ekleri değiştirmeniz gerekirse aşağıdaki yönergeleri uygulayın. Her değişiklik yaptığınızda, yalnızca değiştirmek istediğiniz ön ekler değil ön ek listesinin tamamı belirtilmelidir.

- **Belirtilmiş bir bağlantınız varsa**, aşağıdaki örneği kullanın. Mevcut ön eklerden ve eklemek istediklerinizden oluşan tam ön ek listesini belirtin. Bu örnekte, 10.0.0.0/24 ve 20.0.0.0/24 zaten mevcuttur. 30.0.0.0/24 ve 40.0.0.0/24 ön eklerini ekliyoruz.

  ```azurecli
  az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 -n VNet1toSite2 -g TestRG1
  ```

- **Belirtilmiş bir bağlantınız yoksa**, yerel ağ geçidini oluşturmak için kullandığınız komutu kullanın. VPN cihazının ağ geçidi ip adresini güncelleştirmek için de bu komutu kullanabilirsiniz. Bu komutu, yalnızca henüz bir bağlantınız olmadığı durumlarda kullanın. Bu örnekte, 10.0.0.0/24, 20.0.0.0/24, 30.0.0.0/24 ve 40.0.0.0/24 mevcuttur. Yalnızca alıkoymak istediğimiz ön ekleri belirtiyoruz. Bu durumda, söz konusu ön ekler 10.0.0.0/24 ve 20.0.0.0/24’tür.

  ```azurecli
  az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
  ```

### <a name="modifygwipaddress"></a>Yerel bir ağ geçidi için ağ geçidi IP adresini değiştirme

Bu yapılandırmadaki IP adresi, kendisine bağlantı oluşturduğunuz VPN cihazının IP adresidir. VPN cihazı IP adresi değişirse, bu değeri değiştirebilirsiniz. Ağ geçidi bağlantısı olduğunda bile IP adresi değiştirilebilir.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 -n Site2 -g TestRG1
```

Sonuçları görüntülerken, IP adresi ön eklerinin eklendiğini doğrulayın.

  ```azurecli
  "localNetworkAddressSpace": { 
    "addressPrefixes": [ 
      "10.0.0.0/24", 
      "20.0.0.0/24", 
      "30.0.0.0/24" 
    ] 
  }, 
  "location": "eastus", 
  "name": "Site2", 
  "provisioningState": "Succeeded",  
  ```

### <a name="to-view-the-virtual-network-gateway-public-ip-address"></a>Sanal ağ geçidi genel IP adresini görüntülemek için

Sanal ağ geçidinizin genel IP adresini bulmak için aşağıdaki örneği kullanın. Kolay okunması için, çıkış genel IP’lerin tablo biçiminde gösterileceği şekilde biçimlendirilmiştir.

```azurecli
az network public-ip list -g TestRG1 -o table
```

### <a name="to-verify-the-shared-key-values"></a>Paylaşılan anahtar değerlerini doğrulamak için

Paylaşılan anahtar değerinin, VPN cihaz yapılandırmanızda kullandığınız değerle aynı olduğunu doğrulayın. Aynı değilse, cihazdan alınan değeri kullanarak bağlantıyı yeniden çalıştırın veya cihazı dönüş değeriyle güncelleştirin. Değerler eşleşmelidir.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 -g TestRG1
```

## <a name="next-steps"></a>Sonraki adımlar

*  Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.
* Zorlamalı tünel hakkında bilgi için bkz. [Zorlamalı tüneli yapılandırma](vpn-gateway-forced-tunneling-rm.md).
* Ağ Azure CLI komutlarının listesi için bkz. [Azure CLI](https://docs.microsoft.com/cli/azure/network).
