---
title: Yük Dengeleme Azure CLI kullanarak birden çok IP yapılandırmalarını | Microsoft Docs
description: Azure CLI kullanarak bir sanal makine için birden çok IP adresi atama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: kumud
ms.openlocfilehash: f9cd6405f5c3c87cdf004f8a71b9e72d58532a12
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37108937"
---
# <a name="load-balancing-on-multiple-ip-configurations-using-azure-cli"></a>Yük Dengeleme Azure CLI kullanarak birden çok IP yapılandırmaları hakkında

Bu makalede, bir ikincil ağ arabirimi (NIC) üzerinde birden çok IP adresleriyle Azure yük dengeleyici kullanmayı açıklar. Bu senaryo için Windows, her birincil ve ikincil bir NIC ile çalışan iki VM sahibiz Her ikincil NIC'ler, iki IP yapılandırmaları vardır. Her VM Web siteleri contoso.com ve fabrikam.com barındırır. Her Web sitesi IP yapılandırmaları birine ikincil NIC üzerinde bağlı Azure yük dengeleyici iki ön uç IP adresi, bir Web sitesi için ilgili IP yapılandırmasını trafiğini dağıtmak için her Web sitesi için kullanıma sunmak için kullanırız. Bu senaryo aynı bağlantı noktası numarası hem ön uçlar yanı sıra arasında her iki arka uç havuzu IP adreslerini kullanır.

![LB senaryo görüntüsü](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Birden çok IP yapılandırmaları dengelemek için adımları

Bu makalede açıklanan senaryoyu elde etmek için aşağıdaki adımları tamamlayın:

1. [Azure CLI'yi yükleme ve yapılandırma]((https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)) Azure hesabınızda oturum bağlantılı makale ve günlük içindeki adımları izleyerek.
2. [Bir kaynak grubu oluşturmak](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group) adlı *contosofabrikam* gibi:

    ```azurecli
    az group create contosofabrikam westcentralus
    ```

3. [Kullanılabilirlik kümesi oluştur](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) için iki VM'ler için. Bu senaryo için aşağıdaki komutu kullanın:

    ```azurecli
    az vm availability-set create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. [Bir sanal ağ oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet) adlı *myVNet* ve bir alt ağ olarak adlandırılan *mySubnet*:

    ```azurecli
    az network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus --subnet-name MySubnet --subnet-prefix 10.0.0.0/24

    ```

5. [Yük Dengeleyici oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) adlı *mylb*:

    ```azurecli
    az network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. İki dinamik genel IP adresi, yük dengeleyici ön uç IP yapılandırmaları oluşturun:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    az network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. İki ön uç IP yapılandırmaları oluşturma *contosofe* ve *fabrikamfe* sırasıyla:

    ```azurecli
    az network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    az network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. Arka uç adres havuzları - oluşturma *contosopool* ve *fabrikampool*, [araştırma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*ve yükleme Dengeleme kuralları - *HTTPc* ve *HTTPf*:

    ```azurecli
    az network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    az network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    az network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    az network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. Çıktıyı denetleyin [, yük dengeleyici doğrulayın](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) doğru şekilde aşağıdaki komutu çalıştırarak oluşturuldu:

    ```azurecli
    az network lb show --resource-group contosofabrikam --name mylb
    ```

10. [Genel IP oluşturun](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address), *myPublicIp*, ve [depolama hesabı](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* ilk sanal makineniz için VM1 şekilde:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    az storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. [Ağ arabirimleri oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) VM1 için ve ikinci bir IP yapılandırmasına ekleyin *VM1 ipconfig2*, ve [VM oluşturma](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-vm) gibi:

    ```azurecli
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    az network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    az vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availability-set myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. Adımları 10-11 ikinci VM için yineleyin:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    az storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    az network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    az vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availability-set myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. Son olarak, DNS kaynak kayıtlarını yük dengeleyici ilgili ön uç IP adresine işaret edecek şekilde yapılandırmanız gerekir. Azure DNS'de etki alanlarınızı barındırabilir. Azure DNS yük dengeleyici ile kullanma hakkında daha fazla bilgi için bkz: [kullanarak Azure DNS diğer Azure hizmetleriyle](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Sonraki adımlar
- Yük Dengeleme Azure hizmetlerinde birleştirme hakkında daha fazla bilgi edinin [Azure'da Yük Dengeleme hizmetlerini kullanarak](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Günlükleri farklı türde Azure'da yönetmek ve yük dengeleyici sorunlarını gidermek için nasıl kullanabileceğinizi öğrenin [analytics Azure yük dengeleyici için oturum](../load-balancer/load-balancer-monitor-log.md).
