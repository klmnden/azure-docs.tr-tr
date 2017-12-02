---
title: "PowerShell'i yükleme ve Azure yığın Hızlı Başlangıç için yapılandırma | Microsoft Docs"
description: "Yükleme ve PowerShell Azure yığını için yapılandırma hakkında bilgi edinin."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: sngun
ms.openlocfilehash: fe7b38d66e0e17924bad1bf3fde4af486e7968b2
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>Get Azure yığınında PowerShell ile çalışır

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu Hızlı Başlangıç, yüklemek ve PowerShell ile bir Azure yığın ortamını yapılandırmak için yardımcı olur. Bu makalede sağladığımız betik kapsamına **Azure yığın işleci** yalnızca.

Bu makalede açıklanan adımları sıkıştırılmış sürümüdür [PowerShell yükleme]( azure-stack-powershell-install.md), [karşıdan Araçları]( azure-stack-powershell-download.md), ve [Azure yığın işlecin PowerShell ortamıyapılandırın]( azure-stack-powershell-configure-admin.md) makaleleri. Bu konudaki komut dosyalarını kullanarak, Azure Active Directory veya Active Directory Federasyon Hizmetleri (AD FS) ile dağıtılan Azure yığın ortamlar için PowerShell ayarlayabilirsiniz.  


## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>Azure Active Directory tabanlı dağıtımlar için PowerShell ayarlayın

VPN üzerinden bağlıysanız, Azure yığın Geliştirme Seti ya da Windows tabanlı bir dış istemci için oturum açın. Yükseltilmiş bir PowerShell ISE oturumu açın ve ardından aşağıdaki betiği çalıştırın. Güncelleştirdiğinizden emin olun **TenantName**, **ArmEndpoint**, ve **GraphAudience** değişkenleri ortam yapılandırma için gerekli olarak:

> [!IMPORTANT]
> AzureRM 1.2.11 PowerShell modülü sürümü yeni değişiklikler ile ilgili bir listesi bulunur. 1.2.10 yükseltmek için sürüm, bkz: [Geçiş Kılavuzu](https://aka.ms/azspowershellmigration).

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
Get-Module -ListAvailable | `
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

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"


# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName $TenantName `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>AD FS tabanlı dağıtımlar için PowerShell ayarlayın

Azure internet'e bağlı olduğunda yığını çalışıyorsanız, aşağıdaki komut dosyasını kullanabilirsiniz. İnternet bağlantısı Azure yığın çalışıyorsanız, ancak kullanmak [kesilmiş PowerShell yükleme yolu](azure-stack-powershell-install.md#install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) ve PowerShell yapılandırmak için cmdlet'leri Bu komutta gösterildiği gibi aynı kalır. VPN üzerinden bağlıysanız, Azure yığın Geliştirme Seti ya da Windows tabanlı bir dış istemci için oturum açın. Yükseltilmiş bir PowerShell ISE oturumu açın ve ardından aşağıdaki betiği çalıştırın. Güncelleştirdiğinizden emin olun **ArmEndpoint** ve **GraphAudience** değişkenleri ortam yapılandırma için gerekli olarak:

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

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID
```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

PowerShell yapılandırdıysanız, bir kaynak grubu oluşturarak yapılandırmayı test edebilirsiniz:

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

Kaynak grubu oluşturulduktan sonra **sağlama durumu** özelliği ayarlanmış **başarılı**.

## <a name="next-steps"></a>Sonraki adımlar

* [CLI'yi yükleme ve yapılandırma](azure-stack-connect-cli.md)

* [Şablon geliştirme](user/azure-stack-develop-templates.md)







