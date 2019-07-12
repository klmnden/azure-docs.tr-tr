---
title: Azure sanal ağında - CLI IPv6 ikili yığını uygulama dağıtma
titlesuffix: Azure Virtual Network
description: Bu makalede nasıl dağıtılacağı Azure CLI kullanarak Azure sanal ağı bir IPv6 ikili yığını uygulamada gösterir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 9e591bdf2ff0b6493f092d666d02c2614c907700
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798972"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure-virtual-network---cli-preview"></a>Azure sanal ağında - CLI (Önizleme) IPv6 ikili yığını uygulama dağıtma

Bu makalede, azure'da çift yığın sanal ağ ile bir yük dengeleyici ön uç çift (IPv4 + IPv6) yapılandırmalar, ikili bir IP yapılandırmasına sahip NIC ile VM ile bir çift yığın alt ağ içeren bir ikili yığın (IPv4 + IPv6) uygulamasının nasıl dağıtılacağı gösterir, çift ağ güvenlik grubu kuralları ve ikili genel IP'ler.

> [!Important]
> Azure sanal ağ için IPv6 ikili yığını şu anda genel Önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak yükleyip yerine karar verirseniz, bu hızlı başlangıçta Azure CLI Sürüm 2.0.49 kullanmanızı gerekli hale getirmiş veya üzeri. Yüklü sürümü bulmak için çalıştırın `az --version`. Bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli) yükleme veya yükseltme bilgileri için.

## <a name="prerequisites"></a>Önkoşullar
Azure sanal ağ özelliği için IPv6 kullanmak için aboneliğinizi Azure PowerShell kullanarak aşağıdaki gibi yapılandırmanız gerekir:

```azurecli
az feature register --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
```
Özellik kaydı tamamlanması 30 dakika kadar sürer. Aşağıdaki Azure CLI komutunu çalıştırarak, kayıt durumunu kontrol edebilirsiniz:

```azurelci
az feature show --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
```
Kayıt tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```azurelci
az provider register --namespace Microsoft.Network
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

İkili yığın sanal ağ oluşturabilmeniz için önce bir kaynak grubu oluşturmanız gerekir [az grubu oluşturma](/cli/azure/group). Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myRGDualStack* içinde *eastus* konumu:

```azurecli
az group create \
--name DsResourceGroup01 \
--location eastus
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses-for-load-balancer"></a>Yük Dengeleyici için IPv4 ve IPv6 genel IP adresi oluşturma
IPv4 ve IPv6 noktalarınızı Internet üzerindeki erişmek için yük dengeleyicinin genel IP adresleri IPv4 ve IPv6 gerekir. [az network public-ip create](/cli/azure/network/public-ip) komutu ile bir genel IP adresi oluşturun. Aşağıdaki örnekte adlı IPv4 ve IPv6 genel IP adresi oluşturur *dsPublicIP_v4* ve *dsPublicIP_v6* içinde *myRGDualStack* kaynak grubu:

```azurecli
# Create an IPV4 IP address
az network public-ip create \
--name dsPublicIP_v4  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4

# Create an IPV6 IP address
az network public-ip create \
--name dsPublicIP_v6  \
--resource-group DsResourceGroup01  \
--location eastus \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv6

```

## <a name="create-public-ip-addresses-for-vms"></a>VM'ler için genel IP adresi oluşturma

Sanal makinelerinize internet üzerindeki uzaktan erişim için genel IP adresleri IPv4 VM'ler için gerekir. [az network public-ip create](/cli/azure/network/public-ip) komutu ile bir genel IP adresi oluşturun.

```azurecli
az network public-ip create \
--name dsVM0_remote_access  \
--resource-group DsResourceGroup01 \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4

az network public-ip create \
--name dsVM1_remote_access  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4
```

## <a name="create-basic-load-balancer"></a>Temel Yük Dengeleyici Oluşturma

Bu bölümde, çift ön uç IP (IPv4 ve IPv6) ve yük dengeleyici için arka uç adres havuzu yapılandırmanız ve ardından bir temel yük dengeleyici oluşturun.

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

Temel Load Balancer ile oluşturma [az ağ lb oluşturma](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) adlı **dsLB** adlı bir ön uç havuzunu içeren **dsLbFrontEnd_v4**, adlı bir arka uç havuzu  **dsLbBackEndPool_v4** IPv4 genel IP adresi ile ilişkili **dsPublicIP_v4** , önceki adımda oluşturduğunuz. 

```azurecli
az network lb create \
--name dsLB  \
--resource-group DsResourceGroup01 \
--sku Basic \
--location eastus \
--frontend-ip-name dsLbFrontEnd_v4  \
--public-ip-address dsPublicIP_v4  \
--backend-pool-name dsLbBackEndPool_v4
```

### <a name="create-ipv6-frontend"></a>IPv6 ön uç oluşturma

IPv6 ön uç IP'si ile oluşturma [az network lb frontend-IP oluşturma](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create). Aşağıdaki örnekte adlı bir ön uç IP yapılandırmasını oluşturur *dsLbFrontEnd_v6* ve ekler *dsPublicIP_v6* adresi:

```azurepowershell-interactive
az network lb frontend-ip create \
--lb-name dsLB  \
--name dsLbFrontEnd_v6  \
--resource-group DsResourceGroup01  \
--public-ip-address dsPublicIP_v6

```

### <a name="configure-ipv6-back-end-address-pool"></a>IPv6 arka uç adres havuzu Yapılandır

Bir IPv6 arka uç adres havuzları ile oluşturma [az ağ lb adres havuzu oluşturma](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create). Aşağıdaki örnekte adlı arka uç adres havuzu oluşturur *dsLbBackEndPool_v6* IPv6 NIC yapılandırmaları olan VM'ler eklemek için:

```azurecli
az network lb address-pool create \
--lb-name dsLB  \
--name dsLbBackEndPool_v6  \
--resource-group DsResourceGroup01
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. 

[az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) komutuyla bir yük dengeleyici kuralı oluşturun. Aşağıdaki örnekte adlı yük dengeleyici kuralları oluşturur *dsLBrule_v4* ve *dsLBrule_v6* ve noktasında trafiği dengeler *TCP* bağlantı noktası *80* için IPv4 ve IPv6 ön uç IP yapılandırması:

```azurecli
az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v4  \
--resource-group DsResourceGroup01  \
--frontend-ip-name dsLbFrontEnd_v4  \
--protocol Tcp  \
--frontend-port 80  \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v4


az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v6  \
--resource-group DsResourceGroup01 \
--frontend-ip-name dsLbFrontEnd_v6  \
--protocol Tcp  \
--frontend-port 80 \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v6

```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Vm'leri dağıtmadan önce destekleyici ağ kaynakları - oluşturmalısınız kullanılabilirlik kümesi, ağ güvenlik grubu, sanal ağ ve sanal NIC. 
### <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Uygulamanızın kullanılabilirliğini artırmak için sanal makinelerinizin bir kullanılabilirlik kümesine yerleştirin.

[az vm availability-set create](https://docs.microsoft.com/cli/azure/vm/availability-set?view=azure-cli-latest) komutunu kullanarak bir kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek, adında bir kullanılabilirlik kümesi oluşturur. *dsAVset*:

```azurecli
az vm availability-set create \
--name dsAVset  \
--resource-group DsResourceGroup01  \
--location eastus \
--platform-fault-domain-count 2  \
--platform-update-domain-count 2  
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma

Sanal ağınızdaki gelen ve giden iletişimi yöneten kurallar için ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create)


```azurecli
az network nsg create \
--name dsNSG1  \
--resource-group DsResourceGroup01  \
--location eastus

```

#### <a name="create-a-network-security-group-rule-for-inbound-and-outbound-connections"></a>Gelen ve giden bağlantılar için bir ağ güvenlik grubu kuralı oluşturma

Bağlantı noktası 80 üzerinden ve ile giden bağlantılar için bağlantı noktası 3389, internet bağlantısı üzerinden RDP bağlantılarına izin vermek için bir ağ güvenlik grubu kuralı oluşturma [az ağ nsg kuralı oluşturmak](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create).

```azurecli
# Create inbound rule for port 3389
az network nsg rule create \
--name allowRdpIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 100  \
--description "Allow Remote Desktop In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges 3389

# Create inbound rule for port 80
az network nsg rule create \
--name allowHTTPIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 200  \
--description "Allow HTTP In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges 80  \
--destination-address-prefixes "*"  \
--destination-port-ranges 80

# Create outbound rule

az network nsg rule create \
--name allowAllOut  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 300  \
--description "Allow All Out"  \
--access Allow  \
--protocol "*"  \
--direction Outbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges "*"
```


### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnekte adlı bir sanal ağ oluşturur *dsVNET* alt ağlar ile *dsSubNET_v4* ve *dsSubNET_v6*:

```azurecli
# Create the virtual network
az network vnet create \
--name dsVNET \
--resource-group DsResourceGroup01 \
--location eastus  \
--address-prefixes "10.0.0.0/16" "ace:cab:deca::/48"

# Create a single dual stack subnet

az network vnet subnet create \
--name dsSubNET \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--address-prefixes "10.0.0.0/24" "ace:cab:deca:deed::/64" \
--network-security-group dsNSG1
```

### <a name="create-nics"></a>NIC’leri oluşturma

Sanal NIC ile her VM için oluşturma [az ağ NIC oluşturup](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create). Aşağıdaki örnek, her VM için bir sanal NIC oluşturur. Her iki IP yapılandırması (1 IPv4 config, IPv6 yapılandırma 1) vardır. IPv6 Yapılandırması ile oluşturduğunuz [az ağ NIC IP yapılandırması oluşturma](https://docs.microsoft.com/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-create).
 
```azurecli
# Create NICs
az network nic create \
--name dsNIC0  \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1  \
--vnet-name dsVNET  \
--subnet dsSubNet  \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4  \
--lb-name dsLB  \
--public-ip-address dsVM0_remote_access

az network nic create \
--name dsNIC1 \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4 \
--lb-name dsLB \
--public-ip-address dsVM1_remote_access

# Create IPV6 configurations for each NIC

az network nic ip-config create \
--name dsIp6Config_NIC0  \
--nic-name dsNIC0  \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name dsNIC1 \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

İle Vm'leri oluşturmak [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create). Aşağıdaki örnek, zaten mevcut değilse iki VM ve gerekli olan sanal ağ bileşenlerini oluşturur. 

Sanal makine oluşturma *dsVM0* gibi:

```azurecli
 az vm create \
--name dsVM0 \
--resource-group DsResourceGroup01 \
--nics dsNIC0 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest  
```
Sanal makine oluşturma *dsVM1* gibi:

```azurecli
az vm create \
--name dsVM1 \
--resource-group DsResourceGroup01 \
--nics dsNIC1 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest 
```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>IPv6 ikili yığını sanal ağ, Azure portalında görüntüleme
Azure portalında sanal ağ IPv6 ikili yığını gibi görüntüleyebilirsiniz:
1. Portalın arama çubuğunda girin *dsVnet*.
2. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin. Böylece **genel bakış** adlı çift yığın sanal ağ sayfasının *dsVnet*. Çift yığın sanal ağı iki NIC adlı çift yığın alt ağda bulunan IPv4 ve IPv6 yapılandırmaları gösterir *dsSubnet*.

  ![IPv6 ikili yığını azure'da sanal ağ](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

> [!NOTE]
> Azure portalında Azure sanal ağ için IPv6 kullanılabilir Bu önizleme sürümü için ise salt okunurdur.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli
 az group delete --name DsRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir çift ön uç IP yapılandırması (IPv4 ve IPv6) ile bir temel yük dengeleyici oluşturulur. Aynı zamanda yük dengeleyicinin arka uç havuzuna eklenmiş olan çift IP yapılandırmaları (IPv4 + IPv6) NIC birlikte iki sanal makine oluşturursunuz. Azure sanal ağlarda IPv6 desteği hakkında daha fazla bilgi için bkz: [Azure sanal ağ için IPv6 nedir?](ipv6-overview.md)
