---
title: Azure ayrılmış IP adresleri (Klasik) yönetme | Microsoft Docs
description: Ayrılmış IP adresleri (Klasik) ve bunları yönetmek nasıl Azure PowerShell ve Azure CLI kullanarak.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: genli
ms.openlocfilehash: 8afed4eb1add0ba3a7db474e54b2f78a0babab06
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60789086"
---
# <a name="reserved-ip-addresses-classic-deployment"></a>Ayrılmış IP adresleri (Klasik dağıtım)

 Azure'da IP adresleri iki kategoriye ayrılır: dinamik ve ayrılmış. Varsayılan olarak Azure tarafından yönetilen, genel IP adresleri dinamiktir. Bu IP adresi (VIP) belirli bir bulut hizmeti için veya bir sanal Makineye erişmek için kullanılan veya kaynakları kapatın veya durduruldu (serbest bırakıldı) olduğunda doğrudan rol örneği (ILPIP) zaman zaman değişebilir anlamına gelir.

IP adresleri değiştirmesini önlemek için bir IP adresi ayırabilirsiniz. Ayrılmış IP'ler, kaynakları kapatın veya durduruldu (serbest bırakıldı) bile gibi bulut hizmeti için IP adresi aynı kalmasını sağlayarak yalnızca bir VIP, kullanılabilir. Ayrıca, ayrılmış bir IP adresi için bir VIP olarak kullanılan mevcut dinamik IP'ler dönüştürebilirsiniz.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bir statik genel IP adresi kullanmak ayırabilir öğrenin [Resource Manager dağıtım modeli](virtual-network-ip-addresses-overview-arm.md).

Azure'da IP adresleri hakkında daha fazla bilgi edinmek için [IP adresleri](virtual-network-ip-addresses-overview-classic.md) makalesi.

## <a name="when-do-i-need-a-reserved-ip"></a>Ayrılmış IP olduğunda gerekiyor mu?
* **IP aboneliğinizde ayrılmıştır sağlamak istediğiniz**. Aboneliğinizde hiçbir koşulda yayımlanmamış bir IP adresi ayırmak istiyorsanız, ayrılmış bir genel IP kullanmanız gerekir.  
* **IP'niz bile boyunca durduruldu veya serbest durumu (VM'ler), bulut hizmetiyle kalmasını istediğiniz**. Bir IP adresi kullanılarak erişilmesi için hizmetinizi istiyorsanız, hatta ne zaman bulut hizmetindeki sanal makineleri kapatılır veya durduruldu (serbest bırakıldı) değişmez.
* **Giden trafiğin Azure tarafından sağlanan tahmin edilebilir bir IP adresi kullandığından emin olmak istediğiniz**. Şirket içi güvenlik duvarında yalnızca belirli IP adreslerinden gelen trafiğe izin verecek şekilde yapılandırılmış olabilir. IP ayırma tarafından kaynak IP adresini bilmeniz ve IP herhangi bir değişiklik nedeniyle, güvenlik duvarı kuralları güncelleştirmeniz gerekmez.

## <a name="faqs"></a>SSS
- Tüm Azure Hizmetleri için ayrılmış bir IP'yi kullanabilir miyim?
    Hayır. Ayrılmış IP'ler yalnızca VM'ler ve bulut hizmeti örneği rolleri bir VIP kullanıma sunulan için kullanılabilir.
- Kaç tane ayrılmış IP'ler sahip olabilir miyim?
    Ayrıntılar için bkz [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesi.
- Ayrılmış IP'ler için bir ücret var?
    Bazen. Fiyatlandırma ayrıntıları için bkz: [ayrılmış IP adresi fiyatlandırma ayrıntıları](https://go.microsoft.com/fwlink/?LinkID=398482) sayfası.
- Bir IP adresi nasıl ayırabilirim?
    PowerShell, kullanabileceğiniz [Azure Yönetimi REST API'si](https://msdn.microsoft.com/library/azure/dn722420.aspx), veya [Azure portalında](https://portal.azure.com) bir Azure bölgesindeki bir IP adresi ayırmak için. Aboneliğiniz için ayrılmış bir IP adresi ilişkilidir.
- Ayrılmış IP benzeşim grubu tabanlı sanal ağlar ile kullanabilir miyim?
    Hayır. Ayrılmış IP'ler yalnızca Bölgesel sanal ağlar desteklenir. Ayrılmış IP'ler, benzeşim grupları ile ilişkili sanal ağlar için desteklenmez. Bir sanal ağ bir bölge veya benzeşim grubu ile ilişkilendirme hakkında daha fazla bilgi için bkz. [hakkında bölgesel sanal ağlar ve benzeşim grupları](virtual-networks-migrate-to-regional-vnet.md) makalesi.

## <a name="manage-reserved-vips"></a>Ayrılmış VIP yönetme

### <a name="using-azure-powershell-classic"></a>Azure PowerShell (Klasik) kullanarak

Ayrılmış IP'ler kullanabilmeniz için aboneliğinizde eklemeniz gerekir. Kullanılabilir genel IP adresi havuzundan bir ayrılmış IP Oluştur *Orta ABD* aşağıdaki gibi konumu:

> [!NOTE]
> Klasik dağıtım modeli için Azure PowerShell'in Hizmet Yönetimi sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure PowerShell Hizmet Yönetimi modülünü yükleme](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0). 

  ```powershell
    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
  ```
Ancak, IP yüklenmekte olan belirtemezsiniz fark ayrılmış. Aboneliğinizde hangi IP adresleri ayrılmıştır görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerler *Reservedıpname* ve *adresi*:

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
>PowerShell ile ayrılmış bir IP adresi oluşturduğunuzda, ayrılmış IP ile oluşturmak için bir kaynak grubu belirtemezsiniz. Bir kaynak grubu içine adlı azure yerler *varsayılan ağ* otomatik olarak. Ayrılmış IP kullanarak oluşturursanız [Azure portalında](https://portal.azure.com), seçtiğiniz herhangi bir kaynak grubunu belirtebilirsiniz. Ayrılmış İP'si bir kaynak grubunda dışında oluşturursanız *varsayılan ağ* ancak, her başvuru ayrılmış IP ile komutları gibi `Get-AzureReservedIP` ve `Remove-AzureReservedIP`, adı başvurmalıdır  *Kaynak grubu adı ayrılmış-IP-name grup*.  Adlı bir ayrılmış IP oluşturursanız, örneğin, *myReservedIP* adlı bir kaynak grubu içinde *myResourceGroup*, ayrılmış IP adı başvurmalıdır *grubu myResourceGroup myReservedIP*.   


Bir IP ayrılmış sonra siz silene kadar aboneliğinize ilişkili kalır. Ayrılmış IP aşağıda belirtildiği gibi silin:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

### <a name="using-azure-cli-classic"></a>Azure CLI (Klasik) kullanarak
Kullanılabilir genel IP adresi havuzundan bir ayrılmış IP Oluştur *Orta ABD* konumu olarak kullanarak Azure CLI Klasik izler:

> [!NOTE]
> Klasik dağıtım için Azure Klasik CLI kullanmanız gerekir. Klasik Azure CLI yükleme hakkında daha fazla bilgi için bkz: [Klasik Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-classic-cli?view=azure-cli-latest)
  
 Komut:
 
```azurecli
azure network reserved-ip create <name> <location>
```
Örnek:
 ```azurecli
 azure network reserved-ip create MyReservedIP centralus
 ```

Azure CLI kullanarak aşağıdaki gibi aboneliğinizde hangi IP adresleri ayrılmıştır görüntüleyebilirsiniz: 

Komut:
```azurecli
azure network reserved-ip list
```
Bir IP ayrılmış sonra siz silene kadar aboneliğinize ilişkili kalır. Ayrılmış IP aşağıda belirtildiği gibi silin:

Komut:

 ```azurecli
 azure network reserved-ip delete <name>
 ```
  Örnek:  
 ```azurecli
 azure network reserved-ip delete MyReservedIP
 ```
## <a name="reserve-the-ip-address-of-an-existing-cloud-service"></a>Mevcut bir bulut hizmetinin IP adresini ayırma
Ekleyerek mevcut bir bulut hizmetinin IP adresini ayırabilir miyim `-ServiceName` parametresi. Bir bulut hizmetinin IP adresini ayırma *TestService* içinde *Orta ABD* aşağıdaki gibi konumu:

- Azure PowerShell (Klasik) kullanarak:

  ```powershell
  New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
  ```

- Azure CLI (Klasik) kullanarak:
  
    Komut:

    ```azurecli
     azure network reserved-ip create <name> <location> -r <service-name> -d <deployment-name>
    ```
    Örnek:

    ```azurecli
      azure network reserved-ip create MyReservedIP centralus -r TestService -d asmtest8942
    ```

## <a name="associate-a-reserved-ip-to-a-new-cloud-service"></a>Ayrılmış bir IP'yi yeni bulut hizmetiyle ilişkilendirme
Aşağıdaki komut dosyası için yeni bir ayrılmış IP oluşturur, ardından bu adlı yeni bir bulut hizmeti için ilişkilendirir *TestService*.

### <a name="using-azure-powershell-classic"></a>Azure PowerShell (Klasik) kullanarak
```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```
> [!NOTE]
> Bir bulut hizmeti ile kullanmak için ayrılmış bir IP'yi oluşturduğunuzda, yine de VM kullanarak başvuru *VIP:&lt;bağlantı noktası numarası >* gelen iletişim istekleri için. IP ayırma doğrudan VM'ye bağlanmak anlamına gelmez. Ayrılmış IP, bir VM dağıtıldı bulut hizmetine atanır. Bir VM IP tarafından doğrudan bağlanmak istiyorsanız, bir örnek düzeyi genel IP yapılandırmanız gerekir. Örnek düzeyi genel IP, doğrudan VM'nize atanan genel IP (bir ILPIP denir) türüdür. Ayrılamaz. Daha fazla bilgi için okuma [örnek düzeyi genel IP (ILPIP)](virtual-networks-instance-level-public-ip.md) makalesi.
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Ayrılmış IP çalışan bir dağıtımdan kaldırın.

Yeni bir bulut hizmeti şu şekilde eklenen bir ayrılmış IP kaldırın: 
### <a name="using-azure-powershell-classic"></a>Azure PowerShell (Klasik) kullanarak

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

### <a name="using-azure-cli-classic"></a>Azure CLI (Klasik) kullanarak
Komut:

```azurecli
azure network reserved-ip disassociate <name> <service-name> <deployment-name>
```

Örnek:

```azurecli
azure network reserved-ip disassociate MyReservedIP TestService asmtest8942
```

> [!NOTE]
> Ayrılmış IP çalışan bir dağıtımdan kaldırma ayırma aboneliğinizden kaldırmaz. Yalnızca aboneliğinizdeki başka bir kaynak tarafından kullanılacak IP serbest bırakır.
> 

Ayrılmış İP'si tamamen bir abonelikten kaldırmak için aşağıdaki komutu çalıştırın:

Komut:

```azurecli
azure network reserved-ip delete <name>
```
Örnek:

```azurecli
azure network reserved-ip delete MyReservedIP
```

## <a name="associate-a-reserved-ip-to-a-running-deployment"></a>Çalışan bir geliştirme için ayrılmış bir IP'yi ilişkilendirme

### <a name="using-azure-powershell-classic"></a>Azure PowerShell (Klasik) kullanarak

Aşağıdaki komutları adlı bir bulut hizmeti oluşturma *TestService2* adlı yeni bir VM ile *TestVM2*. Ayrılmış IP adlı varolan *MyReservedIP* olan ardından bulut hizmetine ilişkili.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

### <a name="using-azure-cli-classic"></a>Azure CLI (Klasik) kullanarak
Azure CLI kullanarak aşağıdaki gibi çalışan bulut hizmeti dağıtımı için yeni bir ayrılmış IP ilişkilendirebilirsiniz:

Komut:
```azurecli
azure network reserved-ip associate <name> <service-name> <deployment-name>
```
Örnek:
```azurecli
azure network reserved-ip associate MyReservedIP TestService asmtest8942
```
## <a name="associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Hizmet yapılandırma dosyasını kullanarak bir bulut hizmeti için ayrılmış bir IP'yi ilişkilendirme
Ayrıca, hizmet yapılandırma (CSCFG) dosyasını kullanarak bir bulut hizmeti için ayrılmış bir IP'yi ilişkilendirebilirsiniz. Aşağıdaki örnek xml adında ayrılmış bir VIP kullanılacak bir bulut hizmeti yapılandırma işlemi gösterilmektedir *MyReservedIP*:
```
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
```
## <a name="next-steps"></a>Sonraki adımlar
* Anlamak nasıl [IP adresleme](virtual-network-ip-addresses-overview-classic.md) Klasik dağıtım modelinde çalışır.
* Hakkında bilgi edinin [ayrılmış özel IP adresleri](virtual-networks-reserved-private-ip.md).
* Hakkında bilgi edinin [örnek düzeyi genel IP (ILPIP) adresleri](virtual-networks-instance-level-public-ip.md).

