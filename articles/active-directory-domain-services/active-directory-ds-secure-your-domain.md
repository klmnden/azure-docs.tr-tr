---
title: Azure Active Directory Domain Services yönetilen etki alanınıza güvenli | Microsoft Docs
description: Yönetilen etki alanınıza güvenli
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: maheshu
ms.openlocfilehash: 9dbdb5569a6b5718f01b82661f9323812ab16c27
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019879"
---
# <a name="secure-your-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanınıza güvenli
Bu makale, yönetilen etki alanınıza güvenli hale getirmenize yardımcı olur. Zayıf şifre paketleri kullanımını devre dışı'ı açın ve NTLM kimlik bilgisi karması eşitleme devre dışı bırakın.

## <a name="install-the-required-powershell-modules"></a>Gerekli PowerShell modüllerini yükleyin

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell'i yükleme ve yapılandırma
İçin makaledeki yönergeleri [Azure AD PowerShell modülünü yüklemek ve Azure AD'ye bağlanma](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
İçin makaledeki yönergeleri [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlayın](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="disable-weak-cipher-suites-and-ntlm-credential-hash-synchronization"></a>Zayıf şifre paketleri ve NTLM kimlik bilgisi karması eşitleme devre dışı bırak
Aşağıdaki PowerShell betiğini kullanın:
1. Yönetilen etki alanında NTLM v1 desteğini devre dışı bırakın.
2. NTLM, şirket içi parola karmalarının eşitlenmesini devre dışı bırakmak AD.
3. Yönetilen etki alanındaki TLS v1 devre dışı bırakın.

```powershell
// Login to your Azure AD tenant
Login-AzureRmAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

// 1. Disable NTLM v1 support on the managed domain.
// 2. Disable the synchronization of NTLM password hashes from
//    on-premises AD to Azure AD and Azure AD Domain Services
// 3. Disable TLS v1 on the managed domain.
$securitySettings = @{"DomainSecuritySettings"=@{"NtlmV1"="Disabled";"SyncNtlmPasswords"="Disabled";"TlsV1"="Disabled"}}

// Apply the settings to the managed domain.
Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $securitySettings -Verbose -Force
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Etki Alanı Hizmetleri'nde eşitleme anlama](active-directory-ds-synchronization.md)
