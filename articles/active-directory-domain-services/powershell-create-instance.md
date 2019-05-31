---
title: Azure Active Directory etki alanı PowerShell kullanarak Services'i etkinleştirme | Microsoft Docs
description: Azure Active Directory etki alanı PowerShell kullanarak Services'i etkinleştirme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: d4bc5583-6537-4cd9-bc4b-7712fdd9272a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: ergreenl
ms.openlocfilehash: f2c4f73af00e0093ce98f2de37e9c3a0ba381eda
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246862"
---
# <a name="enable-azure-active-directory-domain-services-using-powershell"></a>Azure Active Directory etki alanı PowerShell kullanarak Services'i etkinleştirme
Bu makalede, PowerShell kullanarak Azure Active Directory (AD) etki alanı Hizmetleri'ni etkinleştirme işlemini göstermektedir.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="task-1-install-the-required-powershell-modules"></a>1. Görev: Gerekli PowerShell modüllerini yükleyin

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell'i yükleme ve yapılandırma
İçin makaledeki yönergeleri [Azure AD PowerShell modülünü yüklemek ve Azure AD'ye bağlanma](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
İçin makaledeki yönergeleri [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlayın](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="task-2-create-the-required-service-principal-in-your-azure-ad-directory"></a>2. Görev: Azure AD dizininizde gerekli hizmet sorumlusu oluşturma
Azure AD dizininizde Azure AD Domain Services için gereken hizmet sorumlusu oluşturmak için aşağıdaki PowerShell komutunu yazın.
```powershell
# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

## <a name="task-3-create-and-configure-the-aad-dc-administrators-group"></a>3. Görev: 'AAD DC Administrators' grubu oluşturmak ve yapılandırmak
Sıradaki görev, yönetilen etki alanınızda yönetim görevleri için temsilci seçmek için kullanılacak Yönetici grubu oluşturmaktır.
```powershell
# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

Grubu oluşturduktan sonra birkaç kullanıcı grubuna ekleyin.
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

## <a name="task-4-register-the-azure-ad-domain-services-resource-provider"></a>4. Görev: Azure AD Domain Services kaynak sağlayıcısını kaydetme
Azure AD Domain Services için kaynak sağlayıcısını kaydetmek için aşağıdaki PowerShell komutunu yazın:
```powershell
# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD
```

## <a name="task-5-create-a-resource-group"></a>5. Görev: Kaynak grubu oluşturma
Bir kaynak grubu oluşturmak için aşağıdaki PowerShell komutunu yazın:
```powershell
$ResourceGroupName = "ContosoAaddsRg"
$AzureLocation = "westus"

# Create the resource group.
New-AzResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation
```

Bu kaynak grubunda sanal ağ ve Azure AD Domain Services yönetilen etki alanı oluşturabilirsiniz.


## <a name="task-6-create-and-configure-the-virtual-network"></a>6. Görev: Oluşturma ve sanal ağ yapılandırma
Şimdi, Azure AD Domain Services'i etkinleştirdiğiniz sanal ağı oluşturun. Azure AD Domain Services için ayrılmış alt ağında oluşturduğundan emin olun. İş yükü Vm'leri bu ayrılmış bir alt ağa dağıtmayın.

Azure AD Domain Services için ayrılmış bir alt ağ ile sanal ağ oluşturmak için aşağıdaki PowerShell komutlarını yazın.

```powershell
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location westus `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet
```


## <a name="task-7-provision-the-azure-ad-domain-services-managed-domain"></a>Görev 7: Azure AD Domain Services yönetilen etki alanı sağlama
Dizininizde Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek için aşağıdaki PowerShell komutunu yazın:

```powershell
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "contoso100.com"
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"
$AzureLocation = "westus"

# Enable Azure AD Domain Services for the directory.
New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Ek yapılandırma adımları, yönetilen etki alanınıza sağladıktan sonra unutmayın.**
> Yönetilen etki alanınıza sağlandıktan sonra hala aşağıdaki görevleri tamamlamanız gerekir:
> * **[DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)**  sanal makinelerin etki alanına katılma veya kimlik doğrulaması için yönetilen etki alanı bulabilmesi için sanal ağ için.
> * **[Azure AD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)** , son kullanıcılar şirket kimlik bilgilerini kullanarak yönetilen etki alanına oturum açın.


## <a name="powershell-script"></a>PowerShell betiği
Bu makalede listelenen tüm görevleri gerçekleştirmek için kullanılan PowerShell Betiği aşağıda verilmiştir. Betiği kopyalayın ve '.ps1' uzantılı bir dosyaya kaydedin. PowerShell veya PowerShell Tümleşik komut dosyası ortamı (ISE) kullanarak betiği yürütün.

> [!NOTE]
> **Bu komut dosyasını çalıştırmak için gereken izinler** Azure AD Domain Services'ı etkinleştirmek için Azure AD dizini için genel yönetici olmanız gerekir. Ayrıca, "Katılımcı" ayrıcalıkları olan bir sanal ağda Azure AD Domain Services etkinleştirme en az gerekir.
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
Connect-AzAccount

# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"

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
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD

# Create the resource group.
New-AzResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $AzureLocation `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet

# Enable Azure AD Domain Services for the directory.
New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Ek yapılandırma adımları, yönetilen etki alanınıza sağladıktan sonra unutmayın.**
> Yönetilen etki alanınıza sağlandıktan sonra hala aşağıdaki görevleri tamamlamanız gerekir:
> * Sanal makinelerin etki alanına katılma veya kimlik doğrulaması için yönetilen etki alanı bulabilmesi için sanal ağ için DNS ayarlarını güncelleştirin.
> * Son kullanıcılar şirket kimlik bilgilerini kullanarak yönetilen etki alanına oturum açabilmeniz için Azure AD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar
Yönetilen etki alanınız oluşturulduktan sonra yönetilen etki alanı kullanabilmeniz için aşağıdaki yapılandırma görevlerinden gerçekleştirin:

* [Yönetilen etki alanınıza işaret edecek şekilde sanal ağın DNS sunucusu ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
* [Yönetilen etki alanınız için parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync.md)
