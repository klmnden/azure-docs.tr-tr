---
title: Azure yığın kullanıcının PowerShell ortamını yapılandırma | Microsoft Docs
description: Azure yığın kullanıcının PowerShell ortamını yapılandırma
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: F4ED2238-AAF2-4930-AA7F-7C140311E10F
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/15/2018
ms.author: mabrigg
ms.reviewer: Balsu.G
ms.openlocfilehash: 2655b682d35dd1879c649ed58d524ecd80808896
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258031"
---
# <a name="configure-the-azure-stack-users-powershell-environment"></a>Azure yığın kullanıcının PowerShell ortamını yapılandırma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir Azure yığın kullanıcı için PowerShell ortamını yapılandırmak için bu makaledeki yönergeleri kullanın.
Ortam yapılandırdıktan sonra Azure yığın kaynakları yönetmek için PowerShell'i kullanabilirsiniz. Örneğin, PowerShell tekliflerini abone, sanal makineler oluşturun ve Azure Resource Manager şablonları dağıtmak için kullanabilirsiniz.

>[!NOTE]
>Bu makalede Azure yığın kullanıcı ortamlar için kapsamlıdır. PowerShell bulut işleci ortamı için ayarlamak istediğiniz oluştuysa, [Azure yığın işlecin PowerShell ortamını yapılandırma](../azure-stack-powershell-configure-admin.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

Bu önkoşulları yapılandırabilirsiniz [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).
* Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md).

## <a name="configure-the-user-environment-and-sign-in-to-azure-stack"></a>Kullanıcı ortamını yapılandırmak ve Azure yığınına oturum açın

PowerShell Azure yığını için yapılandırmak üzere aşağıdaki komut dosyaları birini çalıştırın Azure yığın dağıtımınızı (Azure AD veya AD FS) türüne göre.

Aşağıdaki komut dosyası değişkenleri Azure yığın yapılandırmasından değerlerle değiştirin emin olun:

* AAD tenantName
* GraphAudience uç noktası
* ArmEndpoint

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

Kaynak sağlayıcıları otomatik olarak Portalı aracılığıyla dağıtılan herhangi bir kaynağa sahip olmayan yeni kullanıcı abonelik için kayıtlı değil. Aşağıdaki komut dosyasını çalıştırarak, bir kaynak sağlayıcısı açıkça kaydedebilirsiniz:

```powershell
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi ayarlanmış olduğuna, Azure yığınında kaynak oluşturmak için PowerShell kullanarak bağlanabilirliği test edin. Bir test olarak bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "Contoso.com" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)
* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
