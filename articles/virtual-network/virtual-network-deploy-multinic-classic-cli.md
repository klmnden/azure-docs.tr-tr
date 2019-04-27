---
title: Birden çok NIC - Klasik Azure CLI ile VM (Klasik) oluşturma | Microsoft Docs
description: Azure Klasik komut satırı arabirimi (CLI) kullanarak birden çok NIC ile VM (Klasik) oluşturmayı öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: b436e41e-866c-439f-a7c7-7b4b041725ef
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1e47b1e548516960c6aab3c48d64255370c94a77
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743317"
---
# <a name="create-a-vm-classic-with-multiple-nics-using-the-azure-classic-cli"></a>Klasik Azure CLI kullanarak birden çok NIC ile VM (Klasik) oluşturma

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Azure'da sanal makineler (VM) oluşturma ve birden çok ağ arabirimine (NIC) sanal makinelerinizin her birine ekleyin. Birden çok NIC NIC'ler arasında trafik türlerinin ayrımı sağlar. Örneğin, yalnızca iç kaynaklara Internet'e bağlı olmayan başka bir iletişim kurar, ancak bir NIC Internet ile iletişim kuran. Birden çok NIC arasında ağ trafiğini ayırmak olanağı, uygulama teslimi ve WAN iyileştirme çözümleri gibi birçok ağ sanal Gereçleri için gereklidir.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu adımları kullanarak gerçekleştirmeyi öğrenin [Resource Manager dağıtım modeli](../virtual-machines/linux/multiple-nics.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Adlı bir kaynak grubu aşağıdaki adımları kullanın *IaaSStory* WEB sunucuları ve bir kaynak grubu için adlı *IaaSStory arka uç* DB sunucuları için.

## <a name="prerequisites"></a>Önkoşullar
DB sunucuları oluşturabilmeniz için önce oluşturmanız gerekir *IaaSStory* bu senaryo için gerekli tüm kaynakları kaynak grubu. Bu kaynakları oluşturmak için aşağıdaki adımları tamamlayın. İçindeki adımları izleyerek bir sanal ağ oluşturma [sanal ağ oluşturma](virtual-networks-create-vnet-classic-cli.md) makalesi.

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Arka uç sanal makine dağıtma
Arka uç sanal makinelerin oluşturulmasını aşağıdaki kaynaklar üzerinde bağlıdır:

* **Veri diskleri için depolama hesabı**. Daha iyi performans için veritabanı sunucularında veri diskleri gerektiren bir premium depolama hesabı, katı hal sürücüsü (SSD) teknolojisi kullanır. Premium depolamayı destekler dağıttığınız Azure konumu emin olun.
* **NIC**. Her VM veritabanı erişimi için iki NIC gerekir ve Yönetim için bir tane.
* **Kullanılabilirlik kümesi**. Tüm veritabanı sunucuları, sanal makinelerin en az biri çalışır durumda emin olmak için ayarlayabilir ve bakım sırasında çalışan tek bir kullanılabilirlik eklenir.

### <a name="step-1---start-your-script"></a>1. adım - betiğinizi Başlat
Kullanılan tam bash betiğini indirebilirsiniz [burada](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Ortamınızda çalışması için komut dosyasını değiştirmek için aşağıdaki adımları tamamlayın:

1. Yukarıda dağıtılmış mevcut kaynak grubunuzun göre aşağıdaki değişkenlerin değerlerini değiştirmek [önkoşulları](#prerequisites).

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. Arka uç dağıtımınız için kullanmak istediğiniz değerleri temel alarak aşağıdaki değişkenlerin değerlerini değiştirin.

    ```azurecli
    backendCSName="IaaSStory-Backend"
    prmStorageAccountName="iaasstoryprmstorage"
    image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskPrefix="db"
    dataDiskName="datadisk"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>2. adım - sanal makineleriniz için gerekli kaynakları oluşturma
1. Tüm arka uç sanal makineler için yeni bir bulut hizmeti oluşturun. Kullanımına dikkat edin `$backendCSName` kaynak grubu adı için değişken ve `$location` Azure bölgesi.

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. Sizinki tarafından Vm'leri kullanılacak işletim sistemi ve veri diskleri için bir premium depolama hesabı oluşturun.

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a>3. adım - birden çok NIC ile VM oluşturma
1. Bir döngü göre birden fazla VM oluşturmak için başlangıç `numberOfVMs` değişkenleri.

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. Her VM için her iki NIC IP adresi ve adını belirtin.

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. Bir VM oluşturun. Kullanımı ve fark `--nic-config` adı, alt ağ ve IP adresi ile tüm NIC'ler listesini içeren bir parametre.

    ```azurecli
    azure vm create $backendCSName $image $username $password \
        --connect $backendCSName \
        --vm-name $vmNamePrefix$suffixNumber \
        --vm-size $vmSize \
        --availability-set $avSetName \
        --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
        --virtual-network-name $vnetName \
        --subnet-names $backendSubnetName \
        --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::
    ```

4. Her VM için iki veri diski oluşturun.

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-the-script"></a>Adım 4 - betiği çalıştırın
İndirilen ve gereksinimlerinize göre komut dosyası değiştirildi, arka uç veritabanı VM'lerin birden çok NIC ile oluşturmak için betiği çalıştırın.

1. Kodunuzu kaydedin ve çalıştırın, **Bash** terminal. İlk çıkış, aşağıda gösterildiği gibi göreceksiniz.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Birkaç dakika sonra yürütme sona erecek ve çıkış geri kalanını aşağıda gösterildiği gibi göreceksiniz.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK

### <a name="step-5---configure-routing-within-the-vms-operating-system"></a>5. adım - sanal makinenin işletim sistemi içinde yönlendirmeyi yapılandırma

Azure DHCP, varsayılan bir ağ geçidi sanal makinesine bağlı ilk (birincil) ağ arabirimine atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimlerine varsayılan ağ geçidi atamaz. Bu nedenle varsayılan olarak, alt ağın dışında kalan ve ikincil bir ağ arabirimi içeren kaynaklarla iletişim kurulamaz. Ancak, ikincil ağ arabirimleri kendi alt ağlarının dışında kalan kaynaklarla iletişim kurabilir. İkincil ağ arabirimleri için yönlendirmeyi yapılandırmak için bkz: [birden çok ağ arabirimine sahip bir sanal makine işletim sistemi içinde yönlendirme](virtual-network-network-interface-vm.md).
