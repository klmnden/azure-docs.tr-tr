---
title: Birden çok NIC ile - Azure CLI 1.0 (Klasik) bir VM oluşturma | Microsoft Docs
description: Azure komut satırı arabirimi (CLI) 1.0 kullanarak birden çok NIC ile VM (Klasik) oluşturmayı öğrenin.
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
ms.openlocfilehash: 0b56ab474ff23748487c50bd34487c80242c6429
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="create-a-vm-classic-with-multiple-nics-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak birden çok NIC ile VM (Klasik) oluşturun

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Azure'da sanal makineleri (VM'ler) oluşturun ve her Vm'leriniz için birden çok ağ arabirimlerine (NIC'ler) ekleyin. Birden çok NIC NIC'ler arasında trafik türlerini ayrılması etkinleştirin. Örneğin, yalnızca Internet'e bağlı iç kaynaklara kurduğu başka bir sırada bir NIC Internet ile iletişim kuran. Birden çok NIC'ler arasında ağ trafiğini ayırmak olanağı uygulama sunma ve WAN iyileştirmesi çözümleri gibi birçok ağ sanal uygulamaları için gereklidir.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Kullanarak bu adımların nasıl gerçekleştirileceğini öğrenin [Resource Manager dağıtım modeli](../virtual-machines/linux/multiple-nics.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Adlı bir kaynak grubu aşağıdaki adımları kullanın *IaaSStory* WEB sunucuları ve bir kaynak grubu için adlı *IaaSStory arka uç* DB sunucuları için.

## <a name="prerequisites"></a>Önkoşullar
DB sunucuları oluşturabilmeniz için önce oluşturmanız gerekir *IaaSStory* bu senaryo için gereken tüm kaynakların kaynak grubuyla. Bu kaynakları oluşturmak için izlediği adımları tamamlayın. İçindeki adımları izleyerek bir sanal ağ oluşturma [bir sanal ağ oluşturma](virtual-networks-create-vnet-classic-cli.md) makalesi.

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Arka uç sanal makineleri dağıtma
Arka uç VM'ler aşağıdaki kaynaklara oluşturulmasını bağlıdır:

* **Veri diskleri için depolama hesabı**. Daha iyi performans için veritabanı sunucularında veri diskler bir premium depolama hesabı gerektirir katı hal sürücüsü (SSD) teknolojisi kullanır. Premium depolama desteklemek için dağıtmanız Azure konumu emin olun.
* **NIC**. Her VM veritabanı erişimi için iki NIC gerekir ve yönetimi için bir tane.
* **Kullanılabilirlik kümesi**. Tüm veritabanı sunucuları, en az bir sanal makineleri çalışır durumda emin olmak için ayarlayın ve bakım sırasında çalıştıran tek bir kullanılabilirlik eklenir.

### <a name="step-1---start-your-script"></a>1. adım - kodunuzu Başlat
Kullanılan tam bash komut dosyası indirebilirsiniz [burada](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Ortamınızda çalışması için komut dosyasını değiştirmek için aşağıdaki adımları tamamlayın:

1. Yukarıdaki dağıtılmış varolan kaynak grubunuz temelinde değişkenleri aşağıdaki değerlerini değiştirmek [Önkoşullar](#Prerequisites).

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. Arka uç dağıtımınız için kullanmak istediğiniz değerleri temel alarak aşağıdaki değişkenlerinin değerlerini değiştirin.

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

### <a name="step-2---create-necessary-resources-for-your-vms"></a>2. adım - Vm'leriniz için gerekli kaynakları oluşturma
1. Tüm arka uç VM'ler için yeni bir bulut hizmeti oluşturun. Kullanımına dikkat edin `$backendCSName` kaynak grubu adı için değişken ve `$location` Azure bölgesi.

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. Sizinki tarafından VM'ler kullanılacak işletim sistemi ve veri diskleri için bir premium depolama hesabı oluşturun.

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a>3. adım - VMs birden çok NIC ile oluşturma
1. Temel birden çok VM oluşturmak için bir döngü Başlat `numberOfVMs` değişkenleri.

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. Her bir VM adını ve her iki NIC IP adresini belirtin.

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. VM oluşturun. Kullanımını fark `--nic-config` adı, alt ağ ve IP adresi ile tüm NIC listesini içeren parametresi.

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

4. Her bir VM, iki veri diski oluşturun.

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-the-script"></a>Adım 4 - komut dosyasını çalıştır
İndirilen ve gereksinimlerinize göre komut değiştirilen göre arka uç veritabanı VM'ler ile birden çok NIC oluşturmak için komut dosyasını çalıştırın.

1. Komut dosyanızı kaydedin ve çalıştırın, **Bash** terminal. Aşağıda gösterildiği gibi ilk çıkış göreceksiniz.

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

2. Birkaç dakika sonra yürütme sona erer ve rest çıkış aşağıda gösterildiği gibi görürsünüz.

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

Azure DHCP, sanal makineye bağlı ilk (birincil) ağ arabirimi için varsayılan ağ geçidi atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimlerine varsayılan ağ geçidi atamaz. Bu nedenle varsayılan olarak, alt ağın dışında kalan ve ikincil bir ağ arabirimi içeren kaynaklarla iletişim kurulamaz. İkincil ağ arabirimlerine ancak, kendi alt ağı dışından kaynaklarla iletişim kurabilirsiniz. İkincil ağ arabirimleri için yönlendirmeyi yapılandırmak için bkz: [birden çok ağ arabirimlerine sahip bir sanal makine işletim sistemi içinde yönlendirme](virtual-network-network-interface-vm.md).
