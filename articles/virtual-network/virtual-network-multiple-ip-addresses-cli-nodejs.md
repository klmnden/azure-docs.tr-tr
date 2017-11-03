---
title: "VM ile birden çok IP adreslerini Azure CLI 1.0 kullanarak | Microsoft Docs"
description: "Azure CLI 1.0 kullanarak bir sanal makine için birden çok IP adresi atama hakkında bilgi edinin | Resource Manager."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 9f085dfa1fe4db36d58cb976bb550a46bf241ac7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli-10"></a>Azure CLI 1.0 kullanarak sanal makineleri için birden çok IP adresi atayın

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede, Azure CLI 1.0 kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok IP adresi Klasik dağıtım modeli aracılığıyla oluşturulan kaynakları atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi için okuma [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Birden çok IP adresiyle bir VM oluşturma

Azure CLI 1.0 (Bu makalede) kullanarak bu görevi tamamlayabilirsiniz veya [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md). Adımları birden çok IP adresleriyle VM örneği oluşturmak senaryosunda açıklandığı şekilde açıklanmaktadır. Değişken adları ve IP adresi türleri, uygulamanız için gerekli olarak değiştirin.

1. Yükleme ve Azure CLI 1.0 içindeki adımları izleyerek yapılandırma [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hesap makale ve Azure günlüğüne `azure-login` komutu.

2. Kaynak grubu oluşturun:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Sanal ağ oluşturma:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. Bir alt ağ, sanal ağ oluşturun:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. VM için bir depolama hesabı oluşturun. Aşağıdaki komutu çalıştırmadan önce yerine *mystorageaccount* benzersiz bir ada sahip. Adın Azure arasında benzersiz olması gerekir:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. IP yapılandırmaları, bir NIC oluşturun ve NIC IP yapılandırmaları atayın Eklemek, kaldırmak veya yapılandırmaları gerektiği gibi değiştirin. Aşağıdaki yapılandırmalar senaryoda açıklanmıştır:

    **Ipconfig-1**

    Oluşturmak için aşağıdaki komutları girin:

    - Bir statik genel IP adresi ile genel IP adresi kaynağı
    - Genel IP adresi ve özel bir statik IP adresi atayarak bir NIC.
    
    Değiştir *mypublicdns* Azure konum içinde benzersiz bir ada sahip.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

    **Ipconfig 2**

     Bir statik genel IP adresi ve özel bir statik IP adresi ile yeni bir ortak IP adresi kaynağı ve yeni bir IP yapılandırması oluşturmak için aşağıdaki komutları girin:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **Ipconfig 3**

    Özel bir statik IP adresi ve ortak IP adresi ile bir IP yapılandırması oluşturmak için aşağıdaki komutları girin:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Bu makalede tek bir NIC'ye tüm IP yapılandırmalarının atar ancak birden fazla IP yapılandırması bir VM'de herhangi bir NIC atayabilirsiniz. Birden çok NIC ile VM oluşturmayı öğrenmek için bir VM ile birden çok NIC makalesi oluşturma okuyun.

7. Linux VM oluşturma 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    Standart DS2 v2 VM boyutunu değiştirmek için örneğin, yalnızca aşağıdaki özellik ekleme `--vm-size Standard_DS3_v2` için `azure vm create` adım 6 komutu.

8. NIC ve ilişkili IP yapılandırmaları görüntülemek için aşağıdaki komutu girin:

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin.

## <a name="add"></a>Bir VM için IP adreslerini ekleyin

Adımları izleyerek mevcut NIC'in için ek özel ve genel IP adresleri ekleyebilirsiniz. Temel örnekler yapı [senaryo](#Scenario) bu makalede açıklanan.

1. Azure CLI açın ve tek bir CLI oturumunda bu bölümdeki kalan adımları tamamlayın. Azure CLI yüklenmiş ve yapılandırılmış zaten sahip değilseniz, bölümündeki adımları tamamlamanız [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi ve Azure hesabınızda oturum açın.

2. Gereksinimlerinize göre aşağıdaki bölümlerden birindeki adımları tamamlayın:

    - **Özel bir IP adresi Ekle**
    
        Özel bir IP adresi için bir NIC eklemek için aşağıdaki komutu kullanarak IP yapılandırması oluşturmanız gerekir. Statik adresi alt ağ için kullanılmayan bir adres olmalıdır.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Benzersiz yapılandırma adları ve özel IP adresleri (yapılandırmaları statik IP adresleriyle) kullanarak gereksinim duyduğunuz kadar çok yapılandırmaları oluşturun.

    - **Bir ortak IP adresi Ekle**
    
        Bir ortak IP adresi, yeni bir IP yapılandırması veya var olan IP yapılandırmasını ilişkilendirerek eklenir. Gereksinim duyduğunuz kadar aşağıdaki bölümlerde birindeki adımları tamamlayın.

        > [!NOTE]
        > Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.
        >

        **Yeni bir IP yapılandırması için kaynak ilişkilendirme**
    
        Yeni bir IP yapılandırmasında bir ortak IP adresi eklediğinizde, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiği için özel bir IP adresi eklemelisiniz. Varolan bir ortak IP adresi kaynağı ekleyin veya yeni bir tane oluşturun. Yeni bir tane oluşturmak için aşağıdaki komutu girin:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        Özel bir statik IP adresi ile ilişkili yeni bir IP yapılandırması oluşturmak için *myPublicIP3* genel IP adresi kaynak, aşağıdaki komutu girin:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Var olan IP yapılandırmasını kaynağa ilişkilendirme**

        Yalnızca genel IP adresi kaynağı zaten ilişkili olmayan bir IP yapılandırmasıyla ilişkilendirilmiş olabilir. Aşağıdaki komutu girerek bir IP yapılandırması ilişkili bir ortak IP adresi olup olmadığını belirleyebilirsiniz:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Ipconfig-3 için döndürülen çıktısında izleyen bir benzer bir satır arayın:

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Bu yana **genel IP** sütunu için *IpConfig 3* olduğundan, hiçbir ortak IP adresi kaynağı ilişkili şu anda boştur. Mevcut bir ortak IP adresi kaynağı IpConfig-3'e ekleyin veya oluşturmak için aşağıdaki komutu girin:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Adlı varolan IP yapılandırması için genel IP adresi kaynağı ilişkilendirmek için aşağıdaki komutu girin *IPConfig 3*:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Özel IP adresleri ve aşağıdaki komutu girerek NIC'ye atanan ortak IP adresi kaynaklara bakın:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      Döndürülen çıktı aşağıdakine benzer:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Eklediğiniz NIC VM işletim sistemine'ndaki yönergeleri izleyerek özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adreslerine işletim sistemine eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
