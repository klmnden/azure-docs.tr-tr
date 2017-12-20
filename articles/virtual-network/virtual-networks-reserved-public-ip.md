---
title: "Azure ayrılmış IP adresleri (Klasik) - PowerShell yönetme | Microsoft Docs"
description: "Ayrılmış IP adresleri (Klasik) ve bunların nasıl yönetileceğini anlamak PowerShell kullanarak."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 5e9c83cebec96c6bc8afd53b0c637d7af899746f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="reserved-ip-addresses-classic"></a>Ayrılmış IP adresleri (Klasik)

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Şablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Klasik)](virtual-networks-reserved-public-ip.md)

Azure'daki IP adresleri iki kategoriye ayrılır: dinamik ve ayrılmış. Azure tarafından yönetilen ortak IP adresleri, varsayılan olarak dinamik. IP adresi (VIP) verilen bulut hizmeti için veya bir VM erişmek için kullanılan veya kaynakları kapatıldı veya durduruldu (serbest bırakıldığında) doğrudan rol örneği (ILPIP) zaman zaman değişebilir anlamına gelir.

IP adreslerini değiştirmesini engellemek için bir IP adresi ayırabilirsiniz. Ayrılmış IP kaynakları kapatıldı veya durduruldu (serbest bırakıldığında) gibi bile IP adresi bulut hizmeti için aynı olmasını sağlarken yalnızca bir VIP, kullanılabilir. Ayrıca, ayrılmış bir IP adresi için bir VIP olarak kullanılan varolan dinamik IP dönüştürebilirsiniz.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Statik genel IP adresi kullanarak bir yedek öğrenin [Resource Manager dağıtım modeli](virtual-network-ip-addresses-overview-arm.md).

Azure'daki IP adresleri hakkında daha fazla bilgi için okuma [IP adreslerini](virtual-network-ip-addresses-overview-classic.md) makalesi.

## <a name="when-do-i-need-a-reserved-ip"></a>Ayrılmış IP olduğunda gerekiyor mu?
* **IP aboneliğinizde ayrılmış emin olmak istiyorsanız**. Aboneliğiniz hiçbir koşulda yayımlanmamış bir IP adresi ayırmak istiyorsanız, ayrılmış bir genel IP kullanmanız gerekir.  
* **Hatta çapraz durdurulmuş veya durumu (VM'ler) serbest bulut hizmetiniz ile kalmak için IP istediğiniz**. Bir IP adresi kullanarak erişilmesi için hizmetinizi istiyorsanız, bile zaman VM'ler bulut hizmetindeki kapatılmış veya (serbest bırakıldığında) durdurun değişmiyor.
* **Azure giden trafiği tahmin edilebilir bir IP adresi kullandığından emin olmak istediğiniz**. Şirket içi duvarınızı yalnızca belirli IP adreslerinden gelen trafiğe izin verecek şekilde yapılandırılmış olabilir. Bir IP ayrılarak kaynak IP adresini bilmeniz ve IP değişikliği nedeniyle, güvenlik duvarı kurallarını güncelleştir gerek yoktur.

## <a name="faq"></a>SSS
1. Tüm Azure hizmetlerini bir ayrılmış IP kullanabilir miyim? <br>
    Hayır. Ayrılmış IP'ler yalnızca VM'ler ve bulut hizmeti örnek rolleriniz VIP kullanıma sunulan için kullanılabilir.
2. Kaç tane ayrılmış IP'ler sahip olabilir miyim? <br>
    Ayrıntılar için bkz [Azure sınırlar](../azure-subscription-service-limits.md#networking-limits) makalesi.
3. Herhangi bir ücret için ayrılmış IP'ler var mı? <br>
    Bazen. Fiyatlandırma ayrıntıları için bkz: [ayrılmış IP adresi fiyatlandırma ayrıntıları](http://go.microsoft.com/fwlink/?LinkID=398482) sayfası.
4. Bir IP adresi nasıl yedek? <br>
    PowerShell, kullanabileceğiniz [Azure Yönetimi REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), veya [Azure portal](https://portal.azure.com) bir Azure bölgesindeki bir IP adresi ayırmak için. Ayrılmış bir IP adresi aboneliğinize ilişkilidir.
5. Benzeşim grubu tabanlı sanal ağlar ile ayrılmış bir IP kullanabilir miyim? <br>
    Hayır. Ayrılmış IP'ler yalnızca Bölgesel sanal ağlar desteklenir. Ayrılmış IP, benzeşim gruplarıyla ilişkili sanal ağlar için desteklenmez. Bir bölge veya benzeşim grubu ile VNet ilişkilendirme hakkında daha fazla bilgi için bkz: [hakkında bölgesel sanal ağlara ve benzeşim grupları](virtual-networks-migrate-to-regional-vnet.md) makalesi.

## <a name="manage-reserved-vips"></a>Ayrılmış VIP'ler yönetme

Yüklenmesini ve PowerShell içindeki adımları tamamlayarak yapılandırılmış [yüklemeniz ve yapılandırmanız](/powershell/azure/overview) makale. 

Ayrılmış IP'ler kullanmadan önce aboneliğinize eklemeniz gerekir. Ayrılmış IP bulunan genel IP adresi havuzu oluşturmak için *Orta ABD* konumu, aşağıdaki komutu çalıştırın:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

Ancak, IP yüklenmekte olan belirtemezsiniz fark ayrılmış. Hangi IP adresleri, aboneliğinizde ayrılmıştır görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerleri fark *Reservedıpname* ve *adres*:

```powershell
Get-AzureReservedIP
```

Beklenen çıktı:

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
>PowerShell ile ayrılmış bir IP adresi oluşturduğunuzda, ayrılmış IP ile oluşturmak için bir kaynak grubu belirtemezsiniz. Bir kaynak grubu içine adlı azure yerler *varsayılan ağ* otomatik olarak. Ayrılmış IP kullanarak oluşturursanız [Azure portal](http://portal.azure.com), seçtiğiniz herhangi bir kaynak grubu belirtebilirsiniz. Ayrılmış IP bir kaynak grubunda dışında oluşturursanız, *varsayılan ağ* ancak, her başvuru komutlarla ayrılmış IP gibi `Get-AzureReservedIP` ve `Remove-AzureReservedIP`, adı başvurmalıdır *Grup kaynak grubu adı ayrılmış-IP-name*.  Adlı bir ayrılmış IP oluşturursanız, örneğin, *myReservedIP* bir kaynak grubunda adlı *myResourceGroup*, ayrılmış IP adı başvurmalıdır *Grup myResourceGroup myReservedIP*.   

Bir IP ayrılmış sonra onu silene kadar aboneliğinize ilişkili kalır. Ayrılmış IP silmek için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-the-ip-address-of-an-existing-cloud-service"></a>Var olan bir bulut hizmetini, IP adresi ayırın
Ekleyerek, var olan bir bulut hizmetini IP adresi ayırabilirsiniz `-ServiceName` parametresi. Bir bulut hizmetinin IP adresini ayırmak için *TestService* içinde *Orta ABD* konumu, aşağıdaki PowerShell komutunu çalıştırın:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-to-a-new-cloud-service"></a>Ayrılmış IP'si yeni bir bulut hizmeti ile ilişkilendirme
Aşağıdaki komut dosyasını yeni bir ayrılmış IP oluşturur, ardından adlı yeni bir bulut hizmeti için ilişkilendirir *TestService*.

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> Bir bulut hizmeti ile kullanılmak üzere ayrılmış bir IP oluşturduğunuzda, hala VM'ye başvurabilmek *VIP:&lt;bağlantı noktası numarası >* gelen iletişimi için. IP ayırma doğrudan VM'ye bağlanmak anlamına gelmez. Ayrılmış IP VM dağıtıldı bulut hizmeti atanır. Bir VM IP tarafından doğrudan bağlanmak istiyorsanız, bir örnek düzeyinde ortak IP yapılandırmanız gerekir. Örnek düzeyinde ortak IP, VM'ye atanan genel IP (bir ILPIP olarak adlandırılır) türüdür. Ayrılamaz. Daha fazla bilgi için okuma [örnek düzeyinde ortak IP (ILPIP)](virtual-networks-instance-level-public-ip.md) makalesi.
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Ayrılmış IP çalışan dağıtımdan kaldırın
Yeni bir bulut hizmeti eklenen bir ayrılmış IP kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> Ayrılmış bir IP adresinden çalışan bir dağıtımını kaldırma ayırma aboneliğinizden kaldırmaz. Yalnızca aboneliğiniz başka bir kaynak tarafından kullanılacak IP serbest bırakır.
> 

## <a name="associate-a-reserved-ip-to-a-running-deployment"></a>Ayrılmış IP'si çalışan dağıtımının ile ilişkilendirme
Aşağıdaki komutları adlı bir bulut hizmeti Oluştur *TestService2* adlı yeni bir VM ile *TestVM2*. Ayrılmış IP'si adlı varolan *MyReservedIP* olduğu bulut hizmetine ilişkilendirilmiş.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Hizmet yapılandırma dosyası kullanarak bir bulut hizmeti için ayrılmış bir IP ilişkilendirin
Ayrıca, bir hizmet yapılandırma (CSCFG) dosyası kullanarak bir bulut hizmeti için ayrılmış bir IP ilişkilendirebilirsiniz. Aşağıdaki örnek xml adlı ayrılmış bir VIP kullanmak için bir bulut hizmetinin nasıl yapılandırılacağı gösterilmektedir *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Sonraki adımlar
* Anlamak nasıl [IP adresleme](virtual-network-ip-addresses-overview-classic.md) Klasik dağıtım modelinde çalışır.
* Hakkında bilgi edinin [ayrılmış özel IP adresi](virtual-networks-reserved-private-ip.md).
* Hakkında bilgi edinin [örnek düzeyinde ortak IP (ILPIP) adresleri](virtual-networks-instance-level-public-ip.md).

