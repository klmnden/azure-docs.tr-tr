---
title: Statik iç özel IP - Azure VM - Klasik
description: Statik iç IP (DIP) ve bunların nasıl yönetileceğinin anlama
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
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: b83a6e2c81eac9993c481561e3cebbed681d2c4a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640336"
---
# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>PowerShell (Klasik) kullanarak iç özel statik IP adresi ayarlama
Çoğu durumda, sanal makineniz için statik iç IP adresi belirtmeniz gerekmez. Bir sanal ağdaki VM'ler otomatik olarak bir dahili IP adresine, belirttiğiniz aralıktan alır. Ancak bazı durumlarda, belirli bir sanal makine için statik bir IP adresi belirtme mantıklıdır. Örneğin, sanal makinenizin DNS çalıştıracağınız veya bir etki alanı denetleyicisi olacaktır. Statik iç IP adresi ile VM stop/sağlamayı kaldırma durumunda bile aracılığıyla kalır. 

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun kullanmanızı önerir [Resource Manager dağıtım modeli](virtual-networks-static-private-ip-arm-ps.md).
> 
> 
> ## <a name="install-the-azure-powershell-service-management-module"></a>Azure PowerShell Service Management modülünü yükleme

Aşağıdaki komutları çalıştırmadan önce emin [Azure PowerShell Service Management modülünü](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0
) makineye yüklenir. Azure PowerShell Service Management modülünü sürüm geçmişi için bkz: [Azure modülü PowerShell galerisinde](https://www.powershellgallery.com/packages/Azure/5.3.0).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Belirli bir IP adresi kullanılabilir olup olmadığını doğrulama
Doğrulama IP adresi *10.0.0.7* adlı bir sanal ağda kullanılabilir *TestVnet*, aşağıdaki PowerShell komutunu çalıştırın ve değerini doğrulamak *IsAvailable*.


    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> Yukarıdaki komut güvenli bir ortamda test etmek isterseniz bölümündeki yönergeleri uygulayın [sanal ağ (Klasik) oluşturmak](virtual-networks-create-vnet-classic-pportal.md) adlı bir vnet oluşturmak için *TestVnet* ve onu kullandığından emin olun *10.0.0.0/8*  adres alanı.
> 
> 

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Bir sanal makine oluştururken statik iç IP belirtme
Adlı yeni bir bulut hizmeti aşağıdaki PowerShell Betiği oluşturur *TestService*, ardından görüntüyü Azure'dan alır, ardından adlı bir VM oluşturur *TestVM* alınan görüntü kullanarak yeni bulut hizmetinde ayarlar Adlı bir alt ağ içinde olacak şekilde VM *Subnet-1*ve ayarlar *10.0.0.7* sanal makine için statik iç IP olarak:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Bir sanal makine için statik iç IP bilgilerini alma
Komut dosyası yukarıdaki oluşturulan VM için statik iç IP bilgileri görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerlerini gözlemleyin *IPADDRESS*:

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
Yukarıdaki komut VM'ye eklenen statik iç IP kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Mevcut bir VM'ye statik iç IP ekleme
Yukarıdaki komut dosyası kullanılarak oluşturulan sanal makine için statik iç IP eklemek için aşağıdaki komutu çalıştırın:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a>Sonraki adımlar
[Ayrılmış IP](virtual-networks-reserved-public-ip.md)

[Örnek düzeyi genel IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx)

