---
title: Statik iç özel IP - Azure VM - Klasik
description: Statik iç IP (Dıps) ve bunların nasıl yönetileceğini anlama
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 93444c6f-af1b-41f8-a035-77f5c0302bf0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: genli
ms.openlocfilehash: 1cdf33632c282a872d0eb83dd1a1b1c639fc14bd
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>PowerShell (Klasik) kullanarak statik iç özel bir IP adresi ayarlama
Çoğu durumda, sanal makine için statik iç IP adresi belirtmeniz gerekmez. Bir sanal ağdaki sanal makineleri otomatik olarak bir iç IP adresi, belirttiğiniz bir aralıktan alır. Ancak bazı durumlarda, belirli bir VM için bir statik IP adresi belirtme mantıklıdır. Örneğin, VM'yi DNS çalıştıracağınız ise veya bir etki alanı denetleyicisi olacaktır. Statik iç IP adresi VM bile bir Dur/deprovision durumu ile birlikte kalır. 

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların kullanmasını önerir [Resource Manager dağıtım modeli](virtual-networks-static-private-ip-arm-ps.md).
> 
> 

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Belirli bir IP adresi olup olmadığını doğrulama
Olmadığını doğrulamak için IP adresi *10.0.0.7* adlı sanal ağ içinde kullanılabilir *TestVnet*, aşağıdaki PowerShell komutunu çalıştırın ve değerini doğrulamak *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> Yukarıdaki komut güvenli bir ortamda test etmek isterseniz yönergelere [bir sanal ağ (Klasik) oluşturmak](virtual-networks-create-vnet-classic-pportal.md) adlı bir vnet oluşturmak için *TestVnet* ve onu kullandığından emin olun *10.0.0.0/8*  adres alanı.
> 
> 

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Bir VM oluşturulurken statik iç IP belirtme
Aşağıdaki PowerShell komut dosyası adlı yeni bir bulut hizmeti oluşturur *TestService*, Azure'dan bir görüntü alır sonra adlandırılmış bir VM'nin oluşturur *TestVM* alınan görüntü kullanarak yeni bulut hizmetinde ayarlar Adlı bir alt ağda olması VM *Subnet-1*ve ayarlar *10.0.0.7* VM için statik iç IP olarak:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Bir sanal makine için statik iç IP bilgilerini alma
Komut dosyası yukarıdaki VM oluşturulan için statik iç IP bilgilerini görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerlerini uyun *IPADDRESS*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Bir sanal makineden bir statik iç IP kaldırma
Yukarıdaki komut dosyasındaki VM eklenen statik iç IP kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Mevcut bir VM'yi statik iç IP ekleme
İç statik eklemek için VM IP çalışma yukarıdaki betik komutu aşağıdaki kullanılarak oluşturulan:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a>Sonraki adımlar
[Ayrılmış IP](virtual-networks-reserved-public-ip.md)

[Örnek düzeyinde ortak IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx)

