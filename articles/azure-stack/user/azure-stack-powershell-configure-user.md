---
title: "Azure yığın kullanıcının PowerShell ortamını yapılandırma | Microsoft Docs"
description: "Azure yığın kullanıcının PowerShell ortamını yapılandırma"
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
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: e0ad968cac50ebb1e9ca0a4ff228c748f2da5f28
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="configure-the-azure-stack-users-powershell-environment"></a>Azure yığın kullanıcının PowerShell ortamını yapılandırma

Bir Azure yığın kullanıcı olarak, Azure yığın Geliştirme Seti'nın PowerShell ortam yapılandırabilirsiniz. Yapılandırdıktan sonra PowerShell Azure kaynakları gibi teklifleri için abone yığını yönetmek için kullanabileceğiniz sanal makine oluşturma, dağıtma Azure Resource Manager şablonları, vs. Bu konuda ortamlar yalnızca bulut işleci ortamı için PowerShell ayarlamak istiyorsanız başvurmak için kullanıcı ile kullanılacak kapsamlıdır [Azure yığın işlecin PowerShell ortamını yapılandırma](../azure-stack-powershell-configure-admin.md) konu. 

## <a name="prerequisites"></a>Ön koşullar 

Aşağıdaki Önkoşullar herhangi birinden çalıştırmak [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).  
* Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md). 

## <a name="configure-the-user-environment-and-sign-in-to-azure-stack"></a>Kullanıcı ortamını yapılandırmak ve Azure yığınına oturum açın

Dağıtım bir PowerShell Azure yığınının (emin AAD tenantName, GraphAudience endpoint ve ortam yapılandırmanıza göre ArmEndpoint değerleri değiştirmek için Oluştur) yapılandırmak için aşağıdaki betiği çalıştırın (Azure AD veya AD FS) türüne göre:

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

Şimdi biz her şeyi ayarlanmış olduğuna göre Azure yığın içindeki kaynaklara oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturmak ve bir sanal makine ekleyin. "Contoso.com" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar
* [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)
* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
