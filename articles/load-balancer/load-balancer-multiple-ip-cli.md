---
title: Azure CLI kullanarak birden çok IP yapılandırmalarında Yük Dengelemesi
titlesuffix: Azure Load Balancer
description: Azure CLI kullanarak bir sanal makineye birden çok IP adresi atama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: kumud
ms.openlocfilehash: 1e8911847a555e3b6326f15d15a09344a4472f2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860564"
---
# <a name="load-balancing-on-multiple-ip-configurations-using-azure-cli"></a>Azure CLI kullanarak birden çok IP yapılandırmalarında Yük Dengelemesi

Bu makalede, bir ikincil ağ arabirimi (NIC) üzerinde birden çok IP adresi ile Azure Load Balancer kullanmayı açıklar. Bu senaryoda, her birincil ve ikincil bir NIC ile Windows çalıştıran iki sanal makine sunuyoruz İkincil ağ arabirimlerine her iki IP yapılandırması vardır. Her VM, hem Web sitesi contoso.com ve fabrikam.com barındırır. Her Web sitesi IP yapılandırmaları birine ikincil NIC üzerinde bağlı Azure Load Balancer iki ön uç IP adresi, bir Web sitesi için ilgili IP yapılandırması trafiği dağıtmak için her Web sitesi için kullanıma sunmak için kullanırız. Bu senaryo aynı bağlantı noktası numarasını hem ön uçlar, hem de arasında her iki arka uç havuz IP adreslerini kullanır.

![LB senaryo görüntüsü](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Yük dengelemek birden fazla IP yapılandırması için adımları

Bu makalede açıklanan senaryo elde etmek için aşağıdaki adımları tamamlayın:

1. [Azure CLI'yi yükleme ve yapılandırma](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure hesabınıza bağlı makale ve günlük adımları izleyerek.
2. [Bir kaynak grubu oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) adlı *contosofabrikam* gibi:

    ```azurecli
    az group create contosofabrikam westcentralus
    ```

3. [Kullanılabilirlik kümesi oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) için iki VM için. Bu senaryo için aşağıdaki komutu kullanın:

    ```azurecli
    az vm availability-set create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. [Sanal ağ oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) adlı *myVNet* ve bir alt ağı adlı *mySubnet*:

    ```azurecli
    az network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus --subnet-name MySubnet --subnet-prefix 10.0.0.0/24

    ```

5. [Yük Dengeleyici oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) adlı *mylb*:

    ```azurecli
    az network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. İki dinamik genel IP adresleri için ön uç IP yapılandırmaları yük dengeleyicinizin oluşturun:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    az network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. İki ön uç IP yapılandırmaları oluşturma *contosofe* ve *fabrikamfe* sırasıyla:

    ```azurecli
    az network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    az network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. Arka uç adres havuzları - oluşturma *contosopool* ve *fabrikampool*, [araştırma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*ve yükleme Dengeleme kurallarını - *HTTPc* ve *HTTPf*:

    ```azurecli
    az network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    az network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    az network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    az network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. Çıktıyı denetleyin [load balancer'ınız doğrulayın](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) aşağıdaki komutu çalıştırarak doğru şekilde oluşturulmuş:

    ```azurecli
    az network lb show --resource-group contosofabrikam --name mylb
    ```

10. [Bir genel IP oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *Mypublicıp*, ve [depolama hesabı](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* ilk sanal makinenizi VM1 kılmak için:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    az storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. [Ağ arabirimlerini oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) VM1 için ve ikinci bir IP Yapılandırması Ekle *VM1 ipconfig2*, ve [VM oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-vm) gibi:

    ```azurecli
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    az network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    az vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availability-set myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. İkinci sanal Makineniz için 10-11. adımları yineleyin:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    az storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    az network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    az vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availability-set myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. Son olarak, DNS kaynak kayıtlarını yük dengeleyicinin ilgili ön uç IP adresine işaret edecek şekilde yapılandırmanız gerekir. Azure DNS etki alanlarınızı barındırabilir. Load Balancer ile Azure DNS kullanma hakkında daha fazla bilgi için bkz. [kullanarak Azure DNS diğer Azure hizmetleriyle](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Sonraki adımlar
- Yük Dengeleme hizmetlerini Azure kapsamında birleştirme hakkında daha fazla bilgi [Azure'da Yük Dengeleme hizmetlerini kullanarak](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Günlükleri farklı türde Azure'da yönetmek ve yük dengeleyicide sorunlarını gidermek için nasıl kullanacağınızı öğrenin [Azure Load Balancer için Log analytics](../load-balancer/load-balancer-monitor-log.md).
