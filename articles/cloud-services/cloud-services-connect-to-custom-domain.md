---
title: "Bir bulut hizmeti bir özel etki alanı denetleyicisine bağlanma | Microsoft Docs"
description: "Web/çalışan rolleri bir özel AD PowerShell ve AD etki alanı uzantısını kullanarak etki alanına bağlayın öğrenin"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 4a50ae5e19ff9bf79b7f5361e5a274a2aba350f5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Azure bulut Hizmetleri rolleri bir özel AD etki alanı denetleyicisi Azure'da barındırılan bağlanma
Sanal ağ (VNet) İlk Azure içinde ayarlarsınız. Ardından bir Active Directory etki alanı (bir Azure sanal makinede barındırılan) denetleyicisi Vnet'e ekleyeceğiz. Ardından, biz mevcut bulut hizmeti rollerinizi önceden oluşturulmuş Vnet'e ekleyin, sonra etki alanı denetleyicisine bağlanma.

Başlamadan önce birkaç şey göz önünde bulundurun:

1. Bu öğreticide PowerShell kullanır, böylece Azure PowerShell yüklenmiş ve Git hazır olduğundan emin olun. Azure PowerShell ayarlama konusunda yardım almak için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).
2. AD etki alanı denetleyicisi ve Web/çalışan rolü örneklerinizi ağda olması gerekir.

Bu adım adım kılavuz izleyin ve herhangi bir sorunla çalıştırırsanız, bize makalenin sonunda bir yorum yazın. Birisi geri için karşılaşırsınız (Evet, biz yorumları okuma).

Bulut hizmeti tarafından başvurulan ağ olmalıdır bir **Klasik sanal ağ**.

## <a name="create-a-virtual-network"></a>Sanal Ağ Oluştur
Azure portal veya PowerShell kullanarak Azure'da bir sanal ağ oluşturabilirsiniz. Bu öğretici için PowerShell kullanılır. Azure Portalı'nı kullanarak bir sanal ağ oluşturmak için bkz: [bir sanal ağ oluşturma](../virtual-network/quick-create-portal.md). Sanal ağ (Resource Manager) oluşturma makalede yer almaktadır, ancak bulut Hizmetleri için bir sanal ağ (Klasik) oluşturmanız gerekir. Bunun portalda seçin **kaynak oluşturma**, türü *sanal ağ* içinde **arama** kutusuna ve ardından tuşuna basın **Enter**. Arama sonuçlarında altında **her şeyi**seçin **sanal ağ**. Altında **dağıtım modeli seçin**seçin **Klasik**seçeneğini belirleyip **oluşturma**. Ardından makalesindeki adımları izleyebilirsiniz.

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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
Sanal ağı kurma tamamladıktan sonra bir AD etki alanı denetleyicisi oluşturmanız gerekir. Bu öğretici için size bir AD etki alanı denetleyicisi üzerinde bir Azure sanal makine ayarlarını yapacak.

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
Sanal makineyi bir AD etki alanı denetleyicisi olarak yapılandırmak için VM için oturum açın ve yapılandırmanız gerekir.

VM oturum açmak için RDP dosyasını PowerShell aracılığıyla almak, aşağıdaki komutları kullanın:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

VM oturum açtıktan sonra sanal makineniz adım adım kılavuzu izleyerek bir AD etki alanı denetleyicisi ayarlamak [müşterinizi AD etki alanı denetleyicisi ayarlamak nasıl](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Bulut hizmetiniz için sanal ağ ekleme
Ardından, yeni Vnet'in bulut hizmeti dağıtımınızı eklemeniz gerekir. Bunu yapmak için Visual Studio ya da bir düzenleyiciyi kullanarak, cscfg ilgili bölümlerine ekleyerek, bulut hizmeti cscfg değiştirin.

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

Sonraki bulut Hizmetleri projeyi oluşturun ve Azure'a dağıtın. Bulut Hizmetleri paketinizi Azure'a dağıtma konusunda yardım almak için bkz: [nasıl oluşturulacağı ve bir bulut hizmeti dağıtma](cloud-services-how-to-create-deploy-portal.md)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Web/çalışan rolleri etki alanına bağlayın
Bulut hizmeti projenizi Azure üzerinde dağıtıldığında, rolü örneklerinizi AD etki alanı uzantısını kullanarak özel AD etki alanına bağlayın. AD etki alanı uzantısı, var olan bulut Hizmetleri dağıtımına eklemek ve özel etki alanına katılmak için PowerShell içinde aşağıdaki komutları çalıştırın:

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

Ve bu kadar.

Bulut Hizmetleri, özel etki alanı denetleyicisine katılması. AD etki alanı uzantısını yapılandırmak nasıl kullanılabilir farklı seçenekler hakkında daha fazla bilgi edinmek istiyorsanız, PowerShell Yardımı kullanın. Örnekler izleyin birkaç:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
