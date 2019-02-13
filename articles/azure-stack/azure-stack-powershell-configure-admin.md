---
title: Azure Stack operatör olarak PowerShell ile bağlanma | Microsoft Docs
description: Azure Stack operatör olarak PowerShell ile bağlanma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/11/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: d5b2b1a73a15874072caacd7549c1c328e7edb26
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116567"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>PowerShell ile Azure Stack operatör bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack tekliflerini, planları, kotalar ve Uyarılar oluşturmak gibi kaynakları yönetmek için PowerShell kullanmak için yapılandırabilirsiniz. Bu konuda işleci ortam yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar'nden ya da çalıştırmak [Geliştirme Seti](./asdk/asdk-connect.md#connect-with-rdp) veya size bir istemciden Windows tabanlı dış [ASDK VPN aracılığıyla bağlı](./asdk/asdk-connect.md#connect-with-vpn). 

 - Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](azure-stack-powershell-install.md).  
 - İndirme [Azure Stack ile çalışması için gereken araçları](azure-stack-powershell-download.md).  

## <a name="connect-with-azure-ad"></a>Azure AD'ye bağlanma

PowerShell ile Azure Stack operatörü ortam yapılandırın. Aşağıdaki komut dosyalarından birini çalıştırın: Azure Active Directory (Azure AD) Kiracıadı ve Azure Resource Manager uç nokta değerleri kendi ortamınızdaki yapılandırmayı ile değiştirin. <!-- GraphAudience endpoint -->

```PowerShell  
    # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external"

    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackAdmin").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>AD FS ile bağlanma

PowerShell ile Azure Active Directory Federasyon Hizmetleri (Azure AD FS) ile Azure Stack operatörü ortama bağlanın. Azure Stack Geliştirme Seti için bu Azure Resource Manager uç nokta kümesine `https://adminmanagement.local.azurestack.external`. Azure Stack tümleşik sistemleri için Azure Resource Manager uç noktası almak için hizmet sağlayıcınıza başvurun.

<!-- GraphAudience endpoint -->

  ```PowerShell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
  Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external"

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackAdmin"
  ```

> [!Note]  
> AD FS kullanıcı kimlikleri ile etkileşimli kimlik doğrulaması yalnızca destekler. Bir kimlik bilgisi nesnesi gerekiyorsa, bir hizmet sorumlusu (SPN) kullanmanız gerekir. Azure Stack ve AD FS ile hizmet sorumlusu, Kimlik Yönetimi Hizmeti ayarlama hakkında daha fazla bilgi için bkz. [Yönet AD FS için hizmet sorumlusu](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Artık her şeyi kendinizi, Kurulum, Azure Stack içindeki kaynakları oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. Adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın **MyResourceGroup**.

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar

 - [Şablonları Azure Stack için geliştirme](user/azure-stack-develop-templates.md)
 - [Şablonları PowerShell ile dağıtma](user/azure-stack-deploy-template-powershell.md)
