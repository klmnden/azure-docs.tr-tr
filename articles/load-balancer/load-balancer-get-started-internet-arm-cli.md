---
title: "Herkese açık yük dengeleyici oluşturma - Azure CLI | Microsoft Docs"
description: "Azure CLI kullanarak herkese açık bir yük dengeleyici nasıl oluşturabileceğinizi öğrenin"
services: load-balancer
documentationcenter: na
author: KumudD
manager: jennoc
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: bd8c2703a1b43834e1c82e0776e2dee807bb3192
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="creating-a-public-load-balancer-using-the-azure-cli"></a>Azure CLI kullanarak herkese açık bir yük dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)


[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. [Klasik dağıtım kullanarak herkese açık yük dengeleyici oluşturma](load-balancer-get-started-internet-classic-portal.md) sayfasını da inceleyebilirsiniz

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Çözümü Azure CLI kullanarak dağıtma

Aşağıda Azure Resource Manager ve CLI kullanarak herkese açık yük dengeleyici oluşturma adımları yer almaktadır. Azure Resource Manager ile her bir kaynak ayrı ayrı oluşturulup yapılandırıldıktan sonra kaynak oluşturmak için bir araya getirilir.

Yük dengeleyici dağıtmak için aşağıdaki nesneleri oluşturmanız ve yapılandırmanız gerekir:

* Ön uç IP yapılandırması: Gelen ağ trafiği için genel IP adreslerini içerir.
* Arka uç adres havuzu: Sanal makinelerin yük dengeleyiciden ağ trafiği alması için ağ arabirimlerini (NIC’ler) içerir.
* Yük dengeleme kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki bağlantı noktasına eşleme kurallarını içerir.
* Gelen NAT kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye ait bağlantı noktasına eşleme kurallarını içerir.
* Araştırmalar: Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan durum araştırmalarını içerir.

Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>CLI’yi Resource Manager’ı kullanacak şekilde ayarlama

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Resource Manager moduna geçmek için **azure config mode** komutunu aşağıda gösterildiği gibi çalıştırın.

    ```azurecli
        azure config mode arm
    ```

    Beklenen çıktı:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Ön uç IP havuzu için sanal ağ ve genel IP adresi oluşturma

1. Doğu ABD konumunda *NRPVnet* adında ve *NRPRG* adlı kaynak grubunu kullana bir sanal ağ (VNet) oluşturun.

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    *NRPVnet* içinde *NRPVnetSubnet* adına ve 10.0.0.0/24 CIDR bloğuna sahip bir alt ağ oluşturun.

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. *loadbalancernrp.eastus.cloudapp.azure.com* DNS adına sahip ön uç IP havuzu tarafından kullanılacak *NRPPublicIP* adlı bir genel IP adresi oluşturun. Aşağıdaki komutta statik ayırma türü ve 4 dakikalık boşta kalma zaman aşımı kullanılmaktadır.

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > Yük dengeleyici, FQDN olarak genel IP'nin etki alanı etiketini kullanır. Bu durum, yük dengeleyici Tam Etki Alanı Adı (FQDN) olarak bulut hizmetini kullanan klasik dağıtımdan farklıdır.
   > Bu örnekte FQDN: *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Aşağıdaki komut *Doğu ABD* Azure konumundaki *NRPRG* adlı kaynak grubunda *NRPlb* adlı bir yük dengeleyici oluşturur.

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Ön uç IP havuzu ve arka uç adres havuzu oluşturma
Bu örnekte yük dengeleyicide gelen ağ trafiğini alan ön uç IP havuzunun ve ön uç havuzun yük dengeli ağ trafiğini gönderdiği arka uç IP havuzunun nasıl oluşturulacağı gösterilmektedir.

1. Önceki adımda oluşturulan genel IP’yi ve yük dengeleyiciyi ilişkilendirerek bir ön uç IP havuzu oluşturun.

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. Ön uç IP havuzundan gelen trafiği almak için kullanılacak arka uç adres havuzunu kurun.

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>LB kurallarını, NAT kurallarını ve araştırmayı oluşturma

Bu örnek aşağıdaki nesneleri oluşturur.

* 21 numaralı bağlantı noktasına gelen tüm trafiği 22 numaralı bağlantı noktasına yönlendiren NAT kuralı<sup>1</sup>
* 23 numaralı bağlantı noktasına gelen tüm trafiği 22 numaralı bağlantı noktasına yönlendiren NAT kuralı
* 80 numaralı bağlantı noktasına gelen tüm trafiği arka uç havuzundaki adreslerin 80 numaralı bağlantı noktasıyla dengeleyen yük dengeleyici kuralı.
* *HealthProbe.aspx* adlı sayfanın durumunu denetleyen araştırma kuralı.

<sup>1</sup> NAT kuralları yük dengeleyici arkasındaki belirli sanal makine örnekleriyle ilişkilendirilmiştir. 21 numaralı bağlantı noktasına gelen ağ trafiği, bu NAT kuralıyla ilişkilendirilmiş 22 numaralı bağlantı noktasındaki belirli sanal makineye gönderilir. NAT kuralı için bir protokol (UDP veya TCP) belirtmeniz gerekir. Her iki protokolü aynı bağlantı noktasına atayamazsınız.

1. NAT kurallarını oluşturun.

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. Yük dengeleyici kuralı oluşturun.

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. Durum araştırması oluşturun.

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. Ayarlarınızı denetleyin.

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    Beklenen çıktı:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>NIC’leri oluşturma

NIC’ler oluşturmanız (veya var olanları düzenlemeniz) ve bunları NAT kuralları, yük dengeleyici kuralları ve araştırmalarla ilişkilendirmeniz gerekir.

1. *lb-nic1-be* adlı bir NIC oluşturup *rdp1* NAT kuralı ve *NRPbackendpool* arka uç adres havuzuyla ilişkilendirin.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    Beklenen çıktı:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. *lb-nic2-be* adlı bir NIC oluşturup *rdp2* NAT kuralı ve *NRPbackendpool* arka uç adres havuzuyla ilişkilendirin.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. *web1* adlı bir sanal makine (VM) oluşturun ve *lb-nic1-be* adlı NIC ile ilişkilendirin. Önceden aşağıdaki komutla *web1nrp* adlı bir depolama hesabı oluşturulmuştu.

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > Bir yük dengeleyiciye ait VM’lerin aynı kullanılabilirlik kümesinde olması gerekir. Kullanılabilirlik kümesi oluşturmak için `azure availset create` kullanın.

    Çıktının aşağıdakine benzer olması gerekir:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > İnternet’e bağlanan yük dengeleyici için oluşturulan NIC, yük dengeleyici genel IP adresini kullandığından **Bu NIC için genel IP yapılandırılmadı** bilgi iletisi görüntülenebilir.

    *lb-nic1-be* adlı NIC *rdp1* NAT kuralıyla ilişkilendirildiğinden *web1* adlı makineye yük dengeleyici üzerindeki 3441 numaralı bağlantı noktasından RDP ile bağlanabilirsiniz.

4. *web2* adlı bir sanal makine (VM) oluşturun ve *lb-nic2-be* adlı NIC ile ilişkilendirin. Önceden aşağıdaki komutla *web1nrp* adlı bir depolama hesabı oluşturulmuştu.

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>Mevcut yük dengeleyiciyi güncelleştirme
Var olan bir yük dengeleyiciye başvuran kurallar ekleyebilirsiniz. Sonraki örnekte var olan **NRPlb** yük dengeleyiciye yeni bir yük dengeleyici kuralı eklenmektedir.

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>Yük dengeleyici silme
Bir yük dengeleyiciyi kaldırmak için aşağıdaki komutu kullanın:

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>Sonraki adımlar
[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-cli.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
