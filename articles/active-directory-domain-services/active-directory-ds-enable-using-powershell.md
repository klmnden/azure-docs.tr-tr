---
title: Azure Active Directory etki alanı PowerShell kullanarak hizmetleri etkinleştirme | Microsoft Docs
description: Azure Active Directory etki alanı PowerShell kullanarak Hizmetleri etkinleştir
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d4bc5583-6537-4cd9-bc4b-7712fdd9272a
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: maheshu
ms.openlocfilehash: acae942131903aa86601f023976b0715bf553d4d
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215040"
---
# <a name="enable-azure-active-directory-domain-services-using-powershell"></a>Azure Active Directory etki alanı PowerShell kullanarak Hizmetleri etkinleştir
Bu makalede PowerShell kullanarak Azure Active Directory (AD) etki alanı Hizmetleri'ni etkinleştirme gösterilmiştir.

## <a name="task-1-install-the-required-powershell-modules"></a>Görev 1: gerekli PowerShell modüllerini yükleyin

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell'i yükleme ve yapılandırma
Makaleyi'ndaki yönergeleri izleyin [Azure AD PowerShell modülünü yüklemek ve Azure AD connect](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
Makaleyi'ndaki yönergeleri izleyin [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlanmak](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="task-2-create-the-required-service-principal-in-your-azure-ad-directory"></a>Görev 2: Azure AD dizininizi gerekli hizmet sorumlusu oluşturma
Azure AD dizininizi Azure AD Etki Alanı Hizmetleri'nde gerekli hizmet sorumlusu oluşturmak için aşağıdaki PowerShell komutunu yazın.
```powershell
# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId “2565bd9d-da50-47d4-8b85-4c97f669dc36”
```

## <a name="task-3-create-and-configure-the-aad-dc-administrators-group"></a>Görev 3: Oluşturmak ve 'AAD DC Yöneticiler' grubunu yapılandırma
Sonraki görev, yönetilen etki alanınızda yönetim görevleri atamanıza için kullanılacak Yönetici grubu oluşturmaktır.
```powershell
# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

Grup oluşturduğunuza göre birkaç kullanıcı grubuna ekleyin.
```powershell
# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq 'admin@contoso100.onmicrosoft.com'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId
```

## <a name="task-4-register-the-azure-ad-domain-services-resource-provider"></a>Görev 4: Azure AD etki alanı Hizmetleri kaynak sağlayıcısını Kaydet
Azure AD etki alanı Hizmetleri için kaynak sağlayıcısını kaydetmek için aşağıdaki PowerShell komutu yazın:
```powershell
# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AAD
```

## <a name="task-5-create-a-resource-group"></a>Görev 5: kaynak grubu oluştur
Bir kaynak grubu oluşturmak için aşağıdaki PowerShell komutu yazın:
```powershell
$ResourceGroupName = "ContosoAaddsRg"
$AzureLocation = "westus"

# Create the resource group.
New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation
```

Bu kaynak grubunda sanal ağ ve Azure AD etki alanı Hizmetleri yönetilen etki alanı oluşturabilirsiniz.


## <a name="task-6-create-and-configure-the-virtual-network"></a>Görev 6: Oluşturma ve sanal ağ yapılandırma
Şimdi, Azure AD Etki Alanı Hizmetleri'ni etkinleştirme sanal ağ oluşturun. Azure AD etki alanı Hizmetleri için ayrılmış bir alt ağ oluşturduğunuzdan emin olun. İş yükü VM'ler ayrılmış bu alt ağ dağıtmayın.

Bir sanal ağ için Azure AD etki alanı Hizmetleri ile ayrılmış bir alt ağ oluşturmak için aşağıdaki PowerShell komutlarını yazın.

```powershell
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location westus `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet
```


## <a name="task-7-provision-the-azure-ad-domain-services-managed-domain"></a>Görev 7: Azure AD etki alanı Hizmetleri yönetilen etki alanı sağlama
Dizininiz için Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek için aşağıdaki PowerShell komutu yazın:

```powershell
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "contoso100.com"
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"
$AzureLocation = "westus"

# Enable Azure AD Domain Services for the directory.
New-AzureRmResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Ek yapılandırma adımları, yönetilen etki alanınızı sağladıktan sonra unutmayın.**
> Yönetilen etki alanınızı sağlandıktan sonra hala aşağıdaki görevleri tamamlamanız gerekir:
> * **[DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)**  sanal makinelerin etki alanına katılma veya kimlik doğrulaması için yönetilen etki alanı bulabilmek için sanal ağ için.
* **[Azure AD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)**, son kullanıcılar şirket kimlik bilgilerini kullanarak yönetilen etki alanına oturum açın.
>


## <a name="powershell-script"></a>PowerShell betiği
Bu makalede listelenen tüm görevleri gerçekleştirmek için kullanılan PowerShell Betiği aşağıdadır. Betiği kopyalayın ve '.ps1' uzantılı bir dosyaya kaydedin. PowerShell veya PowerShell Tümleşik komut dosyası ortamı (ISE) kullanarak betiğini yürütün.

> [!NOTE]
> **Bu komut dosyasını çalıştırmak için gereken izinler** Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek için Azure AD dizini için genel yönetici olmanız gerekir. Ayrıca, en azından "Katılımcı" ayrıcalıkları olan bir sanal ağdaki Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek gerekir.
>

```powershell
# Change the following values to match your deployment.
$AaddsAdminUserUpn = "admin@contoso100.onmicrosoft.com"
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "contoso100.com"
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"
$AzureLocation = "westus"

# Connect to your Azure AD directory.
Connect-AzureAD

# Login to your Azure subscription.
Connect-AzureRmAccount

# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId “2565bd9d-da50-47d4-8b85-4c97f669dc36”

# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"

# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq '$AaddsAdminUserUpn'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId

# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AAD

# Create the resource group.
New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $AzureLocation `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet

# Enable Azure AD Domain Services for the directory.
New-AzureRmResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Ek yapılandırma adımları, yönetilen etki alanınızı sağladıktan sonra unutmayın.**
> Yönetilen etki alanınızı sağlandıktan sonra hala aşağıdaki görevleri tamamlamanız gerekir:
> * Sanal makineler etki alanına katılma veya kimlik doğrulaması için yönetilen etki alanı bulabilmek için sanal ağ için DNS ayarlarını güncelleştirin.
* Son kullanıcılar şirket kimlik bilgilerini kullanarak yönetilen etki alanına oturum açabilmeniz için Azure AD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştir.
>

## <a name="next-steps"></a>Sonraki adımlar
Yönetilen etki alanınızı oluşturulduktan sonra yönetilen etki alanı kullanabilmeniz için aşağıdaki yapılandırma görevleri gerçekleştirin:

* [Sanal ağ yönetilen etki alanınıza işaret edecek şekilde DNS sunucusu ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
* [Yönetilen etki alanınız için parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)
