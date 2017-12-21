---
title: "IPv6 - Azure CLI ile bir genel yük dengeleyiciye oluşturun | Microsoft Docs"
description: "Azure CLI kullanarak IPv6 Azure Kaynak Yöneticisi'nde bir genel yük dengeleyiciye oluşturmayı öğrenin."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
keywords: "IPv6, azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IOT"
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 3abd47460999f7b059469a58a59a3e297e88effb
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-public-load-balancer-with-ipv6-in-azure-resource-manager-by-using-azure-cli"></a>Azure CLI kullanarak IPv6 Azure Kaynak Yöneticisi'nde bir genel yük dengeleyiciye oluşturma

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Şablon](load-balancer-ipv6-internet-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük Dengeleyici, gelen trafiği bulut Hizmetleri sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Yük Dengeleyici Ayrıca, birden çok bağlantı noktası veya birden çok IP adresi veya her ikisi de bu hizmetleri sunabilirsiniz.

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Aşağıdaki diyagram bu makalede açıklanan örnek şablon kullanılarak dağıtılan çözümü dengelemesini gösterir.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Bu senaryoda, aşağıdaki Azure kaynakları oluşturun:

* iki sanal makine (VM)
* atanan IPv4 ve IPv6 adreslerinin her VM için bir sanal ağ arabirimi
* Bir genel yük dengeleyiciye bir IPv4 ve IPv6 ortak IP adresi
* İki VM içeren bir kullanılabilirlik kümesi
* iki Yük Dengeleme kuralları özel uç noktaları için ortak VIP'ler eşlemek için

## <a name="deploy-the-solution-by-using-azure-cli"></a>Azure CLI kullanarak çözümü dağıtma

Aşağıdaki adımlar, Azure CLI ile Azure Kaynak Yöneticisi'ni kullanarak bir genel yük dengeleyiciye oluşturulacağını gösterir. Azure Resource Manager ile oluşturun ve her nesneyi ayrı ayrı yapılandırın ve ardından bunları birlikte bir kaynak oluşturmak için alın.

Bir yük dengeleyici dağıtmayı oluşturun ve aşağıdaki nesneleri yapılandırın:

* **Ön uç IP yapılandırmasını**: ortak IP adresleri için gelen ağ trafiğini içerir.
* **Arka uç adres havuzu**: ağ trafiğini yük dengeleyiciden almak sanal makineler için ağ arabirimlerine (NIC'ler) içerir.
* **Yük Dengeleme kuralları**: arka uç adres havuzundaki bir bağlantı noktasına yük dengeleyici üzerinde genel bir bağlantı noktası eşleme kurallarını içerir.
* **Gelen NAT kuralları**: genel bir bağlantı noktası üzerinde yük dengeleyici arka uç adres havuzundaki belirli bir sanal makine için bir bağlantı noktası eşleme ağ adresi çevirisi (NAT) kuralları içerir.
* **Yoklamaları**: arka uç adres havuzu sanal makine örneklerinin kullanılabilirliğini denetlemek için kullanılan sistem durumu araştırmalarının içerir.

Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](load-balancer-arm.md).

## <a name="set-up-your-azure-cli-environment-to-use-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanmak için Azure CLI ortamınızı ayarlayın

Bu örnekte, bir PowerShell komut penceresinde Azure CLI araçlarını çalıştırın. Okunabilirlik ve yeniden iyileştirmek için PowerShell'ın komut dosyası özellikleri, Azure PowerShell cmdlet'lerini kullanın.

1. Azure CLI hiç kullanmadıysanız bkz [yükleyin ve Azure CLI yapılandırma](../cli-install-nodejs.md) ve burada, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.

2. Resource Manager moduna geçmek için Çalıştır **azure config modu** komutu:

    ```azurecli
    azure config mode arm
    ```

    Beklenen çıktı:

        info:    New mode is arm

3. Azure'da oturum açın ve Aboneliklerin listesini alın:

    ```azurecli
    azure login
    ```

4. İsteminde Azure kimlik bilgilerinizi girin:

    ```azurecli
    azure account list
    ```

5. Kullanmak istediğiniz aboneliği seçin ve bir sonraki adımda kullanmak üzere abonelik Kimliğini not edin.

6. Azure CLI komutları ile kullanmak için PowerShell değişkenleri ayarlayın:

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Bir kaynak grubu, bir yük dengeleyici, bir sanal ağ ve alt ağlar oluşturun

1. Kaynak grubu oluşturun:

    ```azurecli
    azure group create $rgName $location
    ```

2. Bir yük dengeleyicisi oluşturun:

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. Sanal ağ oluşturma:

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

4. Bu sanal ağda iki alt ağ oluşturun:

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Ortak IP adresleri için ön uç havuzu oluşturma

1. PowerShell değişkenleri ayarlayın:

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. Ön uç IP havuzu için bir genel IP adresi oluşturun:

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > Yük Dengeleyici genel IP etki alanı etiketini tam etki alanı adı (FQDN) olarak kullanır. Bu bulut hizmeti kullanan Klasik dağıtım değişiklik adı yük dengeleyici FQDN olarak.
    >
    > Bu örnekte FQDN'sidir *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Ön uç ve arka uç havuzları oluşturma

Bu bölümde, aşağıdaki IP havuzlarını oluşturun:
* Yük dengeleyicideki gelen ağ trafiğini alan ön uç IP havuzu.
* Arka uç IP havuzu burada ön uç havuzu yük dengeli ağ trafiği gönderir.

1. PowerShell değişkenleri ayarlayın:

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. Bir ön uç IP havuzu oluşturun ve önceki adımda ve yük dengeleyici oluşturduğunuz genel IP ile ilişkilendirebilirsiniz.

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-load-balancer-rules"></a>NAT kuralları, araştırma oluşturmak ve yük dengeleyici kuralları

Bu örnek aşağıdaki nesneleri oluşturur:

* TCP bağlantı noktası 80 bağlantısını denetlemek için bir araştırma kuralı.
* 3389 numaralı bağlantı noktasına 3389 numaralı bağlantı noktasındaki tüm gelen trafiği için RDP çevirmek için NAT kuralı.\*
* Uzak Masaüstü Protokolü (RDP) için 3389 numaralı bağlantı noktasına 3391 numaralı bağlantı noktasındaki tüm gelen trafiği çevirmek için NAT kuralı.\*
* bağlantı noktası 80 üzerinde arka uç havuzundaki adresleri bağlantı noktası 80 üzerinde tüm gelen trafiği dengelemek için bir yük dengeleyici kuralı.

\*NAT kuralları yük dengeleyicinin arkasındaki belirli bir sanal makine örneği ile ilişkilendirilmiş. Belirli bir sanal makine ve NAT kuralı ile ilişkili bağlantı noktası 3389 numaralı bağlantı noktasına ulaşan ağ trafiğini gönderilir. NAT kuralı için bir protokol (UDP veya TCP) belirtmeniz gerekir. Her iki protokolü aynı bağlantı noktasına atayamazsınız.

1. PowerShell değişkenleri ayarlayın:

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. Araştırma oluşturun.

    Aşağıdaki örnek, 15 dakikada denetleyen bir TCP araştırması arka uç TCP bağlantı noktası 80 bağlantısını oluşturur. İki ardışık arızalarının ardından arka uç kaynak kullanılamaz olarak işaretler.

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. Arka uç kaynaklarına RDP bağlantılara izin veren gelen NAT kurallarını oluşturun:

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. Bir istek aldı ön uç bağlı olarak farklı arka uç noktalarına giden trafik yük dengeleyici kuralları oluşturun.

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. Ayarlarınızı denetleyin:

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    Beklenen çıktı:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a>NIC’leri oluşturma

NIC oluşturun ve bunları NAT kuralları, yük dengeleyici kuralları ve araştırmalar ile ilişkilendirin.

1. PowerShell değişkenleri ayarlayın:

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. Her arka ucu için bir NIC oluşturun ve IPv6 yapılandırmasını ekleyin:

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Arka uç VM kaynakları oluşturun ve her NIC ekleme

Sanal makineleri oluşturmak için bir depolama hesabı olması gerekir. Yük Dengeleme için sanal makineleri bir kullanılabilirlik kümesi üyesi olması gerekir. Sanal makineleri oluşturma hakkında daha fazla bilgi için bkz: [PowerShell kullanarak bir Azure VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

1. PowerShell değişkenleri ayarlayın:

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > Bu örnek, kullanıcı adı ve parola temiz metin VM'ler için kullanır. Bu kimlik bilgileri düz metinde kullandığınızda uygun dikkat edin. Daha güvenli bir yöntem PowerShell kimlik bilgilerini işleme bilgi için bkz: [ `Get-Credential` ](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i.

2. Depolama hesabı ve kullanılabilirlik kümesi oluşturun.

    Sanal makineleri oluşturduğunuzda, mevcut bir depolama hesabını kullanabilirsiniz. Aşağıdaki komutu kullanarak yeni bir depolama hesabı oluşturun:

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

3. Kullanılabilirlik kümesi oluştur:

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

4. Sanal makineler ile ilişkili NIC oluşturun:

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-cli.md)  
[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)  
[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
