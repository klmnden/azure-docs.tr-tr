---
title: Özel IP adresleri için VM'ler (Klasik) - Azure PowerShell yapılandırma | Microsoft Docs
description: PowerShell kullanarak sanal makineler (Klasik) için özel IP adresleri yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: 60c7b489-46ae-48af-a453-2b429a474afd
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1be2966cf568fc5101fb0d887f0d55a8a1876d0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952491"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-powershell"></a>PowerShell kullanarak sanal makine (Klasik) için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde statik özel IP adresi yönetme](virtual-networks-static-private-ip-arm-ps.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek PowerShell komutları, önceden oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce açıklanan test ortamında yapı [VNet oluşturma](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Belirli bir IP adresi kullanılabilir olup olmadığını doğrulama
Doğrulama IP adresi *192.168.1.101* adlı bir sanal ağda kullanılabilir *TestVNet*, aşağıdaki PowerShell komutunu çalıştırın ve değerini doğrulamak *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Beklenen çıktı:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Bir VM oluşturulurken özel bir statik IP adresi belirtme
Adlı yeni bir bulut hizmeti aşağıdaki PowerShell Betiği oluşturur *TestService*, ardından görüntüyü Azure'dan alır, adlı bir VM oluşturur *DNS01* yeni bir bulut hizmetinde alınan görüntüsünü kullanarak ayarlar VM adlı bir alt ağda olması *ön uç*ve ayarlar *192.168.1.7* VM için bir statik özel IP adresi:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Beklenen çıktı:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir sanal makine için statik özel IP adresi bilgilerini alma
Komut dosyası yukarıdaki oluşturulan VM için statik özel IP adresi bilgilerini görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerlerini gözlemleyin *IPADDRESS*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Beklenen çıktı:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>VM'den özel statik IP adresi kaldırma
Statik özel IP adresini kaldırmak için yukarıdaki komut VM'ye aşağıdaki PowerShell komutunu çalıştırın eklendi:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Beklenen çıktı:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Mevcut bir VM'ye statik özel IP adresi ekleme
Yukarıdaki komut dosyası kullanılarak oluşturulan VM'ye statik özel IP adresi eklemek için aşağıdaki komutu çalıştırın:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Beklenen çıktı:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, önerilen gerekli. İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure VM'sine atanan özel IP adresiyle aynı adresi olduğundan emin olun veya sanal makineye bağlantı kaybedebilir. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [ayrılmış genel IP](virtual-networks-reserved-public-ip.md) adresleri.
* Hakkında bilgi edinin [örnek düzeyi genel IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri.
* Başvurun [ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx).

