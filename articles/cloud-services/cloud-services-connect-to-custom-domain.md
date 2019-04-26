---
title: Bulut hizmetini özel bir etki alanı denetleyicisine bağlama | Microsoft Docs
description: Bir özel AD PowerShell ve AD etki alanı uzantısı kullanarak etki alanı için web/çalışan rollerinizi bağlantı kurmayı öğrenin
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 8bee2e2038ee39c777e1ca09994ad21872d2029a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337349"
---
# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Azure Cloud Services rolleri bir özel AD etki alanı denetleyicisi Azure'da barındırılan bağlanma
İlk sanal ağ (VNet) Azure'da ayarlayacağız. Ardından bir Active Directory etki alanı (bir Azure sanal makinesinde barındırılan) denetleyicisi Vnet'e ekleyeceğiz. Ardından, biz mevcut bulut hizmeti rolleri önceden oluşturulmuş bir sanal ağa ekleyin, sonra bunları etki alanı denetleyicisine bağlanma.

Başlamadan önce göz önünde bulundurmanız gereken şey birkaç:

1. Bu öğreticide, PowerShell kullanılır, bu nedenle Azure PowerShell yüklenmiş ve kullanılmaya hazır olduğundan emin olun. Azure PowerShell ayarlama ayarlama konusunda yardım almak için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).
2. AD etki alanı denetleyicisi ve Web/çalışan rolü örneklerinizi VNet içinde olması gerekir.

Bu adım adım kılavuzu izleyin ve herhangi bir sorunla karşılaşırsanız çalıştırırsanız, bize makale sonuna bir yorum yazın. Birisi geri dönelim (Evet, biz yorumları okuyun).

Bulut hizmeti tarafından başvurulan ağ olmalıdır bir **Klasik sanal ağ**.

## <a name="create-a-virtual-network"></a>Sanal Ağ Oluştur
Azure portal veya PowerShell kullanarak Azure'da bir sanal ağ oluşturabilirsiniz. Bu öğreticide, PowerShell kullanılır. Azure portalını kullanarak bir sanal ağ oluşturmak için bkz [sanal ağ oluşturma](../virtual-network/quick-create-portal.md). Bu makalede, sanal ağ (Resource Manager) oluşturma yer almaktadır, ancak bulut Hizmetleri için sanal ağ (Klasik) oluşturmanız gerekir. Portalda Bunu yapmak için **kaynak Oluştur**, türü *sanal ağ* içinde **arama** kutusuna ve ardından basın **Enter**. Arama sonuçlarında altında **her şeyi**seçin **sanal ağ**. Altında **dağıtım modeli seçin**seçin **Klasik**, ardından **Oluştur**. Ardından, bu makaledeki adımları izleyebilirsiniz.

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Bir Sanal Makine Oluşturun
Sanal ağ ayarlama tamamladıktan sonra AD etki alanı denetleyicisi oluşturmak gerekir. Bu öğretici için size bir AD etki alanı denetleyicisi üzerinde bir Azure sanal makine ayarlarını yapacak.

Bunu yapmak için aşağıdaki komutları kullanarak PowerShell aracılığıyla bir sanal makine oluşturun:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Sanal makinenize bir etki alanı denetleyicisine Yükselt
Sanal makine bir AD etki alanı denetleyicisi olarak yapılandırmak için VM'de oturum açın ve yapılandırmanız gerekir.

VM'de oturum açmak için PowerShell üzerinden RDP dosyası alma, aşağıdaki komutları kullanın:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

VM'de oturum açtıktan sonra sanal makineyi bir AD etki alanı denetleyicisi olarak adım adım kılavuzu izleyerek ayarlayın. [müşterinizi AD etki alanı denetleyicisini ayarlama](https://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Bulut hizmetinizin sanal ağa ekleyin.
Ardından, bulut hizmeti dağıtımınız yeni bir sanal ağa eklemeniz gerekir. Bunu yapmak için Visual Studio veya tercih ettiğiniz düzenleyiciyi kullanarak, cscfg için ilgili bölümlerine ekleyerek, bulut hizmeti cscfg değiştirin.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Ardından cloud services projenizi derleyin ve Azure'a dağıtın. Azure'a, bulut Hizmetleri paketi dağıtma konusunda yardım almak için bkz: [nasıl bir bulut hizmeti oluşturma ve dağıtma](cloud-services-how-to-create-deploy-portal.md)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Web/çalışan rollerinizi etki alanına bağlayın.
Bulut hizmeti projenizi Azure'da dağıtıldığında, rol örneklerinizin AD etki alanı uzantısı kullanarak özel AD etki alanına bağlayın. AD etki alanı uzantısı var olan bulut Hizmetleri dağıtıma ekleme ve özel etki alanına katılmak için PowerShell'de aşağıdaki komutları yürütün:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Ve İşte bu kadar.

Cloud services, özel etki alanı denetleyicisine katılması. AD etki alanı uzantısı nasıl yapılandıracağınızı öğrenmek için kullanılabilen farklı seçenekler hakkında daha fazla bilgi edinmek istiyorsanız, PowerShell Yardımı kullanın. Birkaç örnek izleyin:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
