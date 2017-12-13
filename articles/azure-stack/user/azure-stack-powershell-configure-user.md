---
title: "Azure yığın kullanıcının PowerShell ortamını yapılandırma | Microsoft Docs"
description: "Azure yığın kullanıcının PowerShell ortamını yapılandırma"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: F4ED2238-AAF2-4930-AA7F-7C140311E10F
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: mabrigg
ms.openlocfilehash: 0bd5b4a98fee7a5d914e53e49a9517f5d3682a88
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="configure-the-azure-stack-users-powershell-environment"></a>Azure yığın kullanıcının PowerShell ortamını yapılandırma

Bir Azure yığın kullanıcı olarak, Azure yığın Geliştirme Seti'nın PowerShell ortam yapılandırabilirsiniz. Yapılandırdıktan sonra PowerShell Azure kaynakları gibi teklifleri için abone yığını yönetmek için kullanabileceğiniz sanal makine oluşturma, dağıtma Azure Resource Manager şablonları, vs. Bu konuda ortamlar yalnızca bulut işleci ortamı için PowerShell ayarlamak istiyorsanız başvurmak için kullanıcı ile kullanılacak kapsamlıdır [Azure yığın işlecin PowerShell ortamını yapılandırma](../azure-stack-powershell-configure-admin.md) makalesi. 

## <a name="prerequisites"></a>Ön koşullar 

Aşağıdaki Önkoşullar herhangi birinden çalıştırmak [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).  
* Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md). 

## <a name="configure-the-user-environment-and-sign-in-to-azure-stack"></a>Kullanıcı ortamını yapılandırmak ve Azure yığınına oturum açın

Dağıtım PowerShell Azure yığın (AAD tenantName, GraphAudience endpoint ve ortam yapılandırmanıza göre ArmEndpoint değerleri değiştirdiğinizden emin olun) yapılandırmak için aşağıdaki komut dosyalarından birini çalıştırma (Azure AD veya AD FS) türüne göre:

### <a name="azure-active-directory-aad-based-deployments"></a>Azure Active Directory (AAD) tabanlı dağıtımlar
       
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAudience endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackUser"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantID 
   ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Active Directory Federasyon Hizmetleri (AD FS) tabanlı dağıtımlar 
          
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAudience endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

  # Get the Active Directory tenantId that is used to deploy Azure Stack     
  $TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackUser"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantID 
  ```

## <a name="register-resource-providers"></a>Kayıt kaynak sağlayıcıları

Portalı aracılığıyla dağıtılan herhangi bir kaynağa sahip olmayan bir yeni oluşturulan kullanıcı abonelik üzerinde çalışırken, kaynak sağlayıcıları otomatik olarak kayıtlı değil. Açıkça bunları aşağıdaki komut dosyasını kullanarak kaydetmeniz:

```powershell
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    } 
```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Biz her şeyi olduğuna göre kurulum yapalım Azure yığın içindeki kaynaklara oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturmak ve bir sanal makine ekleyin. "Contoso.com" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar
* [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)
* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
