---
title: Azure örnek düzeyi genel IP (Klasik) adresleri | Microsoft Docs
description: Örnek düzeyi genel IP (ILPIP) yöneliktir ve bunların nasıl yönetileceğini anlamak PowerShell kullanarak.
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
ms.date: 08/03/2018
ms.author: genli
ms.openlocfilehash: 2f6db23e02c836dea6d640757d12275b654ad468
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60186804"
---
# <a name="instance-level-public-ip-classic-overview"></a>Örnek düzeyi genel IP (Klasik) genel bakış
Bir örnek düzeyi genel IP (ILPIP) doğrudan bir sanal makine veya Bulut Hizmetleri rolü örneği yerine, VM veya rol örneğindeki bulunan bir bulut hizmeti atayabileceğiniz genel bir IP adresi ' dir. Bir ILPIP sanal IP (bulut hizmetinize atanan VIP) yer almaz. Bunun yerine, bu doğrudan, VM'deki veya rol örneğine bağlanmak için kullanabileceğiniz bir ek IP adresidir.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, Vm'leri Resource Manager üzerinden oluşturulmasını önerir. Anladığınızdan emin olun nasıl [IP adresleri](virtual-network-ip-addresses-overview-classic.md) azure'da çalışır.

![VIP ILPIP arasındaki fark](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Şekil 1'de gösterildiği gibi bulut hizmetine tek tek sanal makineleri, normalde VIP kullanılarak erişilir sırasında bir VIP kullanılarak erişilir:&lt;bağlantı noktası numarası&gt;. Belirli bir VM'ye bir ILPIP atayarak, o sanal IP adresi kullanarak doğrudan erişilebilir.

Azure'da bir bulut hizmeti oluşturduğunuzda, karşılık gelen DNS A kayıtlarını otomatik olarak bir tam etki alanı adı (FQDN) üzerinden hizmete erişmesine izin vermek için gerçek VIP kullanmak yerine oluşturulur. ILPIP yerine FQDN DEĞERİNE göre VM veya rol örneğine erişmesine izin vererek, bir ILPIP için aynı işlem gerçekleşir. Örneği için adlı bir bulut hizmeti oluşturursanız *contosoadservice*, adında bir web rolü yapılandırmanız *contosoweb* iki örneği ile ve .cscfg `domainNameLabel` ayarlanır  *WebPublicIP*, Azure örnekleri için aşağıdaki A kayıtları kayıtları:


* WebPublicIP.0.contosoadservice.cloudapp.net
* WebPublicIP.1.contosoadservice.cloudapp.net
* ...


> [!NOTE]
> Her sanal makine veya rol örneği için yalnızca bir ILPIP atayabilirsiniz. Abonelik başına en fazla 5 ILPIPs kullanabilirsiniz. ILPIPs multi-NIC sanal makineler için desteklenmez.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Bir ILPIP istek neden?
Bulut kullanmak yerine VIP Doğrudan kendisine atanmış bir IP adresiyle VM veya rol Örneğinize bağlanmak istiyorsanız, hizmet:&lt;bağlantı noktası numarası&gt;, VM'nize veya rol Örneğiniz için bir ILPIP istek.

* **Etkin FTP** -bir VM için bir ILPIP atayarak herhangi bir bağlantı noktasında trafik alabilir. Uç noktaları trafiği almak sanal makine için gerekli değildir.  Bkz: [FTP protokolüne genel bakış](https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) FTP protokolünü hakkında ayrıntılı bilgi için.
* **Giden IP** - sanal makineden kaynaklanan giden trafik kaynağı olarak ILPIP eşleştirilir ve ILPIP dış varlıklar VM benzersiz olarak tanımlar.

> [!NOTE]
> Geçmişte, ILPIP adresi genel IP (PIP) adresi başvuruldu.
> 

## <a name="manage-an-ilpip-for-a-vm"></a>VM için bir ILPIP yönetme
Aşağıdaki görevler, oluşturmak, atamak ve ILPIPs Vm'lerinizden kaldırın olanak sağlar:

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a>PowerShell kullanarak VM oluşturma sırasında bir ILPIP nasıl
Aşağıdaki PowerShell betiğini adlı bir bulut hizmeti oluşturur *FTPService*, Azure'dan bir görüntü alır, adlı bir VM oluşturur *FTPInstance* alınan görüntüsünü kullanarak bir ILPIP kullanmak üzere VM ayarlar ve ekler Yeni hizmet VM:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

#Set "current" storage account for the subscription. It will be used as the location of new VM disk

Set-AzureSubscription -SubscriptionName <SubName> -CurrentStorageAccountName <StorageAccountName>

#Create a new VM configuration object

New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

```
Yeni VM disk konumu olarak başka bir depolama hesabı belirtmek istiyorsanız, kullanabileceğiniz **MediaLocation** parametresi:

```powershell
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
     -MediaLocation https://management.core.windows.net/<SubscriptionID>/services/storageservices/<StorageAccountName> `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Bir VM için ILPIP bilgi alma
Önceki betiği ile oluşturulan VM için ILPIP bilgileri görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerlerini gözlemleyin *Publicıpaddress* ve *PublicIPName*:

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
Önceki komut VM'yi eklenen ILPIP kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Mevcut bir VM'ye bir ILPIP ekleme
Önceki komut dosyası kullanılarak oluşturulan sanal Makineye bir ILPIP eklemek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>Cloud Services rol örneği için bir ILPIP yönetme

Cloud Services rol örneği için bir ILPIP eklemek için aşağıdaki adımları tamamlayın:

1. Bulut hizmeti için .cscfg dosyası içindeki adımları tamamlayarak indirme [bulut hizmetlerini yapılandırma](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) makalesi.
2. .Cscfg dosyasını ekleyerek güncelleştirme `InstanceAddress` öğesi. Aşağıdaki örnek adlı bir ILPIP ekler *Mypublicıp* adlı rol örneği için *WebRole1*: 

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
          <PublicIP name="MyPublicIP" domainNameLabel="WebPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. Bulut hizmeti için .cscfg dosyasını karşıya yükleme adımları tamamlayarak [bulut hizmetlerini yapılandırma](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) makalesi.

### <a name="how-to-retrieve-ilpip-information-for-a-cloud-service"></a>Bir bulut hizmeti için ILPIP bilgi alma
Rol örneği başına ILPIP bilgileri görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerlerini gözlemleyin *Publicıpaddress*, *PublicIPName*, *PublicIPDomainNameLabel* ve *PublicIPFqdns*:

```powershell
Add-AzureAccount

$roles = Get-AzureRole -ServiceName <Cloud Service Name> -Slot Production -RoleName WebRole1 -InstanceDetails

$roles[0].PublicIPAddress
$roles[1].PublicIPAddress
```

Ayrıca `nslookup` alt etki alanını sorgulamak için bir kayıt kullanıcının:

```batch
nslookup WebPublicIP.0.<Cloud Service Name>.cloudapp.net
``` 

## <a name="next-steps"></a>Sonraki adımlar
* Anlamak nasıl [IP adresleme](virtual-network-ip-addresses-overview-classic.md) Klasik dağıtım modelinde çalışır.
* Hakkında bilgi edinin [ayrılmış IP'ler](virtual-networks-reserved-public-ip.md).
