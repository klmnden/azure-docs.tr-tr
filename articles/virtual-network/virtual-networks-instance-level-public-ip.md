---
title: Azure örneği düzeyinde (Klasik) ortak IP adresleri | Microsoft Docs
description: Örnek düzeyinde ortak IP (ILPIP) yöneliktir ve bunların nasıl yönetileceğini anlamak PowerShell kullanarak.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: genli
ms.openlocfilehash: 4b4350e6b1616450ce45f9e947cc3b639a341ae7
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="instance-level-public-ip-classic-overview"></a>Örnek düzeyinde ortak IP (Klasik) genel bakış
Bir örnek düzeyinde ortak IP (ILPIP), VM veya Bulut Hizmetleri rol örneği için doğrudan yerine, VM'deki veya rol örneğindeki bulunan bulut hizmetine atayabilirsiniz genel bir IP adresi ' dir. Bir ILPIP, sanal IP (bulut Hizmetinize atanmış VIP) yer almaz. Bunun yerine, doğrudan, VM'deki veya rol örneğine bağlanmak için kullanabileceğiniz ek bir IP adresi değil.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, VM'ler Resource Manager aracılığıyla oluşturma önerir. Anladığınızdan emin olun nasıl [IP adreslerini](virtual-network-ip-addresses-overview-classic.md) Azure içinde çalışır.

![ILPIP VIP arasındaki fark](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Şekil 1'de gösterildiği gibi bulut hizmeti bir VIP kullanırken, tek tek sanal makineleri, normalde VIP kullanılarak erişilir erişilir:&lt;bağlantı noktası numarası&gt;. Belirli bir VM'yi bir ILPIP atayarak bu VM, bu IP adresi kullanarak doğrudan erişilebilir.

Azure'da bulut hizmeti oluşturduğunuzda, karşılık gelen DNS A kayıtlarını otomatik olarak bir tam etki alanı adı (FQDN) üzerinden hizmete erişmesine izin vermek için gerçek VIP kullanmak yerine oluşturulur. FQDN ILPIP yerine VM veya rol örneğine izin veren bir ILPIP için aynı işlem gerçekleşir. Örneği için adlı bir bulut hizmeti oluşturursanız *contosoadservice*, ve adında bir web rolü yapılandırma *contosoweb* iki örnekleriyle aşağıdaki Azure kaydeder A kayıtlarını örnekleri için:

* contosoweb\_IN_0.contosoadservice.cloudapp.net
* contosoweb\_IN_1.contosoadservice.cloudapp.net 

> [!NOTE]
> Her VM veya rol örneği için yalnızca bir ILPIP atayabilirsiniz. Abonelik başına en fazla 5 ILPIPs kullanabilirsiniz. ILPIPs multi-NIC VM'ler için desteklenmez.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Bir ILPIP isteme neden?
Bulut kullanmak yerine VIP Doğrudan kendisine atanmış bir IP adresine göre VM'deki veya rol örneğine bağlanmak istiyorsanız, hizmet:&lt;bağlantı noktası numarası&gt;, VM veya rol örneği için bir ILPIP isteyin.

* **Etkin FTP** -bir VM için bir ILPIP atayarak onu herhangi bir bağlantı noktasında trafik alabilir. Uç noktaları VM trafiği almak gerekli değildir.  Bakın (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[FTP Protokolü genel bakış] FTP protokolünü hakkında ayrıntılı bilgi için.
* **Giden IP** - sanal makineden giden trafiği ILPIP kaynağı olarak eşleştirilir ve ILPIP dış varlıklar VM benzersiz şekilde tanımlar.

> [!NOTE]
> Geçmişte, bir ILPIP adresi genel bir IP (PIP) adresi başvuruldu.
> 

## <a name="manage-an-ilpip-for-a-vm"></a>Bir VM için bir ILPIP yönetme
Aşağıdaki görevler oluşturun, atamak ve VM'lerin ILPIPs kaldırmak etkinleştir:

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a>PowerShell kullanarak VM oluşturma sırasında bir ILPIP istemek nasıl
Aşağıdaki PowerShell betiğini adlı bir bulut hizmeti oluşturur *FTPService*, Azure'dan bir görüntü alır, adlandırılmış bir VM'nin oluşturur *FTPInstance* alınan görüntü kullanarak bir ILPIP kullanmak için VM ayarlar ve ekler Yeni hizmet VM'ye:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Bir VM için ILPIP bilgi alma
Önceki komut dosyasıyla VM oluşturulan için ILPIP bilgileri görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerlerini uyun *Publicıpaddress* ve *PublicIPName*:

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

Beklenen çıktı:
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :   Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-to-remove-an-ilpip-from-a-vm"></a>Bir sanal makineden bir ILPIP kaldırma
Önceki komut dosyasında VM eklenen ILPIP kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Mevcut bir VM'yi bir ILPIP ekleme
Önceki komut dosyası kullanılarak oluşturulan VM bir ILPIP eklemek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>Bulut Hizmetleri rol örneği için bir ILPIP yönetme

Bulut Hizmetleri rol örneği için bir ILPIP eklemek için aşağıdaki adımları tamamlayın:

1. İçindeki adımları tamamlayarak bulut hizmeti için .cscfg dosyasını indirin [bulut hizmetlerini yapılandırma](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) makalesi.
2. .Cscfg dosyasını ekleyerek güncelleştirme `InstanceAddress` öğesi. Aşağıdaki örnek adlı bir ILPIP ekler *MyPublicIP* adlı bir rol örneği *WebRole1*: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. İçindeki adımları tamamlayarak bulut hizmeti için .cscfg dosyasını karşıya [bulut hizmetlerini yapılandırma](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) makalesi.

## <a name="next-steps"></a>Sonraki adımlar
* Anlamak nasıl [IP adresleme](virtual-network-ip-addresses-overview-classic.md) Klasik dağıtım modelinde çalışır.
* Hakkında bilgi edinin [ayrılmış IP'ler](virtual-networks-reserved-public-ip.md).
