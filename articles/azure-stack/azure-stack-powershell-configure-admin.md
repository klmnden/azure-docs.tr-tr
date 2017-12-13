---
title: "Azure yığın işlecin PowerShell ortamını yapılandırma | Microsoft Docs"
description: "Azure yığın işlecin PowerShell ortam yapılandırmayı öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 37D9CAC9-538B-4504-B51B-7336158D8A6B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: mabrigg
ms.openlocfilehash: 96ce59d0390affaa57d05d6d08657f5c1a3c466a
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>Azure yığın işlecin PowerShell ortamını yapılandırma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir Azure yığın operatör olarak, Azure yığın Geliştirme Seti'nın PowerShell ortam yapılandırabilirsiniz. Yapılandırdıktan sonra uyarılar, vb. Yönetme Teklifler, planları, kotalar, oluşturma gibi Azure yığın kaynakları yönetmek için PowerShell'i kullanabilirsiniz. Bu konuda ortamlar yalnızca, kullanıcı ortamı için PowerShell ayarlamak istiyorsanız, başvuru için bulut operatörü ile kullanılacak kapsamlıdır [Azure yığın kullanıcının PowerShell ortamını yapılandırmak](user/azure-stack-powershell-configure-user.md) konu. 

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki Önkoşullar herhangi birinden çalıştırmak [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

* Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).  
* Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>İşleç ortamını yapılandırmak ve Azure yığınına oturum açın

Dağıtım bir PowerShell (AAD tenantName ve GraphAudience endpoint ArmEndpoint değerlerini ortamınıza uygun şekilde değiştirdiğinizden emin olun ile Azure yığın işleci ortamını yapılandırmak için aşağıdaki betiği çalıştırın (Azure AD veya AD FS) türüne göre Yapılandırma):

### <a name="azure-active-directory-aad-based-deployments"></a>Azure Active Directory (AAD) tabanlı dağıtımlar
       
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    
  Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience 

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Active Directory Federasyon Hizmetleri (AD FS) tabanlı dağıtımlar
         
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


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

Şimdi biz her şeyi ayarlanmış olduğuna göre Azure yığın içindeki kaynaklara oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturmak ve bir sanal makine ekleyin. "Contoso.com" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar
* [Şablonları geliştirmek için Azure yığını](user/azure-stack-develop-templates.md)
* [Şablonları PowerShell ile dağıtma](user/azure-stack-deploy-template-powershell.md)
