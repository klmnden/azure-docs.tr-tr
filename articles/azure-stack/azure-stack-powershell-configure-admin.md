---
title: Azure yığın PowerShell ortamını yapılandırma | Microsoft Docs
description: Azure yığın PowerShell ortam yapılandırmayı öğrenin.
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
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 86608ef8b3623682bd10498605f8b7b62c377ff1
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="configure-the-azure-stack-powershell-environment"></a>Azure yığın PowerShell ortamını yapılandırma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Teklifler, planları, kotalar ve Uyarıları oluşturma gibi kaynakları yönetmek için PowerShell kullanmak için Azure yığın yapılandırabilirsiniz. Bu konuda işleci ortamı yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar herhangi birinden çalıştırmak [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

 - Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).  
 - Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>İşleç ortamını yapılandırmak ve Azure yığınına oturum açın

Azure yığın işleci ortamı PowerShell ile yapılandırın. Dağıtım, Azure AD veya AD FS, türüne göre aşağıdaki betikler birini çalıştırın: Azure AD tenantName, GraphAudience endpoint ve ArmEndpoint değerleri kendi ortamı yapılandırması ile değiştirin.

### <a name="azure-active-directory-azure-ad-based-deployments"></a>Azure Active Directory (Azure AD) tabanlı dağıtımlar

````PowerShell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````


### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Active Directory Federasyon Hizmetleri (AD FS) tabanlı dağıtımlar

````PowerShell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi olduğuna göre kurulum yapalım Azure yığın içindeki kaynaklara oluşturmak için PowerShell kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturmak ve bir sanal makine ekleyin. "Contoso.com" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Sonraki adımlar
 - [Şablonları geliştirmek için Azure yığını](user/azure-stack-develop-templates.md)
 - [Şablonları PowerShell ile dağıtma](user/azure-stack-deploy-template-powershell.md)
