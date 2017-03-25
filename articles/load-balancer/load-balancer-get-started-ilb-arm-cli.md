---
title: "İç yük dengeleyicisi oluşturma - Azure CLI | Microsoft Docs"
description: "Azure CLI kullanarak Resource Manager’da iç yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 128b91c685b5f7e494a69ca5b04165a0ee7cbb78
ms.lasthandoff: 03/21/2017

---

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Azure CLI kullanarak iç yük dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md).  Bu makale, Microsoft’un çoğu yeni dağıtım için [klasik dağıtım modeli](load-balancer-get-started-ilb-classic-cli.md) yerine önerdiği Resource Manager dağıtım modelini açıklamaktadır.

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Çözümü Azure CLI kullanarak dağıtma

Aşağıda Azure Resource Manager ve CLI kullanarak İnternet’e yönelik yük dengeleyici oluşturma adımları yer almaktadır. Azure Resource Manager ile her bir kaynak ayrı ayrı oluşturulup yapılandırıldıktan sonra kaynak oluşturmak için bir araya getirilir.

Yük dengeleyici dağıtmak için aşağıdaki nesneleri oluşturmanız ve yapılandırmanız gerekir:

* **Ön uç IP yapılandırması**: Gelen ağ trafiği için genel IP adreslerini içerir
* **Arka uç adres havuzu**: Sanal makinelerin yük dengeleyiciden ağ trafiği almasını sağlayan ağ arabirimlerini (NIC’ler) içerir
* **Yük dengeleme kuralları**: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki bağlantı noktasına eşleyen kuralları içerir
* **Gelen NAT kuralları**: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye ait bağlantı noktasına eşleyen kuralları içerir
* **Araştırmalar**: Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan sistem durumu araştırmalarını içerir

Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>CLI’yi Resource Manager’ı kullanacak şekilde ayarlama

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI hizmetini yükleme ve yapılandırma](../cli-install-nodejs.md). Talimatları Azure hesabı ve abonelik seçme adımına kadar uygulayın.
2. Resource Manager moduna geçmek için **azure config mode** komutunu aşağıdaki gibi çalıştırın:

    ```azurecli
    azure config mode arm
    ```

    Beklenen çıktı:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Adım adım iç yük dengeleyici oluşturma

1. Azure'da oturum açın.

    ```azurecli
    azure login
    ```

    İstendiğinde Azure kimlik bilgilerinizi girin.

2. Komut araçlarını Azure Resource Manager moduna alın.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager’daki tüm kaynaklar bir kaynak grubuyla ilişkilendirilmiştir. Henüz yapmadıysanız, bir kaynak grubu oluşturun.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>İç yük dengeleyici kümesi oluşturma

1. İç yük dengeleyici oluşturma

    Aşağıdaki senaryoda Doğu ABD bölgesinde nrprg adlı bir kaynak grubu oluşturulmaktadır.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Sanal ağlar ve sanal ağ alt ağları gibi tüm iç yük dengeleyici kaynaklarının aynı kaynak grubunda ve aynı bölgede olması gerekir.

2. İç yük dengeleyici için ön uç IP adresi oluşturun.

    Kullandığınız IP adresi sanal ağınızla aynı alt ağ aralığında olmalıdır.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Arka uç adres havuzunu oluşturun.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    Ön uç IP adresi ve arka uç adres havuzu tanımladıktan sonra yük dengeleyici kurallarını, gelen NAT kurallarını ve özel sistem durumu araştırmalarını oluşturabilirsiniz.

4. İç yük dengeleyici için yük dengeleyici kuralı oluşturun.

    Önceki adımları uyguladığınızda verilen komut ön uç havuzunun 1433 numaralı bağlantı noktasında dinleme yapan ve yük dengelenmiş ağ trafiğini yine 1433 numaralı bağlantı noktasını kullanarak arka uç adres havuzuna gönderen bir yük dengeleyici kuralı oluşturur.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Gelen NAT kurallarını oluşturun.

    Gelen NAT kuralları, yük dengeleyicide belirli bir sanal makine örneğine giden uç noktaları oluşturmak için kullanılır. Önceki adımlarda uzak masaüstü için iki NAT kuralı oluşturulmuştur.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Yük dengeleyici için sistem durumu araştırmaları oluşturun.

    Sistem durumu araştırması tüm sanal makine örneklerini denetleyerek ağ trafiği gönderdiklerinden emin olur. Sistem durumu denetimi başarısız olan sanal makine örnekleri tekrar çevrimiçi olana ve sistem durumu denetimi iyi olduğuna karar verene kadar yük dengeleyiciden kaldırılır.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > Microsoft Azure platformu, farklı yönetim senaryoları için genel olarak yönlendirilebilen statik bir IPv4 adresi kullanır. IP adresi: 168.63.129.16. Bu IP adresinin güvenlik duvarları tarafından engellenmesi beklenmeyen davranışlara neden olabilir.
    > Azure iç yük dengeleme açısından bu IP adresi, araştırmaların yük dengeleyiciden izlenerek yük dengeli kümede yer alan sanal makinelerin durumunun belirlenmesini sağlar. Bir iç yük dengeli kümedeki Azure sanal makinelerine gelen trafiği kısıtlamak için bir ağ güvenlik grubu kullanılması veya sanal alt ağ uygulanması halinde 168.63.129.16 adresinden gelen trafiğe izin verecek şekilde bir ağ güvenlik kuralının uygulandığından emin olun.

## <a name="create-nics"></a>NIC’leri oluşturma

NIC’ler oluşturmanız (veya var olanları düzenlemeniz) ve bunları NAT kuralları, yük dengeleyici kuralları ve araştırmalarla ilişkilendirmeniz gerekir.

1. *lb-nic1-be* adlı bir NIC oluşturup *rdp1* NAT kuralı ve *beilb* arka uç adres havuzuyla ilişkilendirin.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
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

2. *lb-nic2-be* adlı bir NIC oluşturup *rdp2* NAT kuralı ve *beilb* arka uç adres havuzuyla ilişkilendirin.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. *DB1* adlı bir sanal makine oluşturun ve *lb-nic1-be* adlı NIC ile ilişkilendirin. Önceden aşağıdaki komutla *web1nrp* adlı bir depolama hesabı oluşturulmuştu:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > Bir yük dengeleyiciye ait VM’lerin aynı kullanılabilirlik kümesinde olması gerekir. Kullanılabilirlik kümesi oluşturmak için `azure availset create` kullanın.

4. *DB2* adlı bir sanal makine (VM) oluşturun ve *lb-nic2-be* adlı NIC ile ilişkilendirin. Önceden aşağıdaki komutla *web1nrp* adlı bir depolama hesabı oluşturulmuştu.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Yük dengeleyici silme

Bir yük dengeleyiciyi kaldırmak için aşağıdaki komutu kullanın:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Sonraki adımlar

[Kaynak IP benzeşimi kullanarak yük dengeleyici dağıtım modunu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)


