---
title: PowerShell'i yükleme ve Azure Stack Hızlı Başlangıç için yapılandırma | Microsoft Docs
description: Yükleme ve Azure Stack için PowerShell yapılandırma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.openlocfilehash: 5d988e8a8a32924b8424a07cf20c75f0e8f8cf4d
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391083"
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>Azure stack'teki PowerShell ile çalışmaya Al

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu hızlı başlangıçta, yükleme ve PowerShell ile bir Azure Stack ortamı yapılandırma yardımcı olur. Bu makalede sunuyoruz. komut dosyası için kapsama **Azure Stack operatörü** yalnızca.

Bu makalede açıklanan adımları sıkıştırılmış sürümüdür [PowerShell yükleme]( azure-stack-powershell-install.md), [araçları indirme]( azure-stack-powershell-download.md), ve [Azure Stack işlecin PowerShell ortamınıyapılandırma]( azure-stack-powershell-configure-admin.md) makaleler. Bu makalede komut dosyalarını kullanarak, Azure Active Directory veya Active Directory Federasyon Hizmetleri (AD FS) ile dağıtılan Azure Stack ortamlar için PowerShell'i ayarlayabilirsiniz.  


## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>Azure Active Directory tabanlı dağıtımlar için PowerShell'i ayarlayın

<a name="sign-in-to-your-azure-stack-development-kit-or-a-windows-based-external-client-if-you-are-connected-through-vpn-open-an-elevated-powershell-ise-session-and-then-run-the-following-script"></a>VPN birbirine bağlandıysa, Azure Stack geliştirme Seti'ni veya Windows tabanlı bir dış istemci için oturum açın. Yükseltilmiş bir PowerShell ISE oturumu açın ve ardından aşağıdaki betiği çalıştırın. 
-  
- Güncelleştirdiğinizden emin olun **Kiracıadı**, **ArmEndpoint**, ve **GraphAudience** ortamınızdaki yapılandırmayı için gerekirse değişkenleri:

```powershell
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName $TenantName `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Add-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>AD FS tabanlı dağıtımlar için PowerShell'i ayarlayın

İnternet'e bağlı olduğunda Azure Stack çalışıyorsanız, aşağıdaki betiği kullanabilirsiniz. İnternet bağlantısı olmadan Azure Stack çalışıyorsanız, ancak kullanmak [PowerShell'i yükleme yolu bağlantısı kesildi](azure-stack-powershell-install.md) ve PowerShell cmdlet'lerinin Bu komutta gösterildiği gibi aynı kalır. VPN birbirine bağlandıysa, Azure Stack geliştirme Seti'ni veya Windows tabanlı bir dış istemci için oturum açın. Yükseltilmiş bir PowerShell ISE oturumu açın ve ardından aşağıdaki betiği çalıştırın. Güncelleştirdiğinizden emin olun **ArmEndpoint** ve **GraphAudience** ortamınızdaki yapılandırmayı için gerekirse değişkenleri:

```powershell

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Add-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID
```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

PowerShell yapılandırdığınıza göre bir kaynak grubu oluşturarak yapılandırmasını test edebilirsiniz:

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

> [!note]  
> Bir kaynak grubu belirtmek için aboneliğinizde bir kaynak grubu olması gerekir. Abonelikler hakkında daha fazla bilgi için bkz. [Plan, teklif, kota ve abonelik genel bakış](azure-stack-plan-offer-quota-overview.md)

Kaynak grubu oluşturulduktan sonra **sağlama durumu** özelliği **başarılı**.

## <a name="next-steps"></a>Sonraki adımlar

* [CLI'yi yükleme ve yapılandırma](azure-stack-connect-cli.md)

* [Şablon geliştirme](user/azure-stack-develop-templates.md)
