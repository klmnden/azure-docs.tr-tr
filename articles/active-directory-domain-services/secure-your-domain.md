---
title: Azure Active Directory Domain Services yönetilen etki alanınıza güvenli | Microsoft Docs
description: Yönetilen etki alanınızın güvenliğini sağlama
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: e94cd9ca049cfdfd2321ce046714506ed1f23390
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483270"
---
# <a name="secure-your-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanınıza güvenli
Bu makale, yönetilen etki alanınıza güvenli hale getirmenize yardımcı olur. Zayıf şifre paketleri kullanımını devre dışı'ı açın ve NTLM kimlik bilgisi karması eşitleme devre dışı bırakın.

## <a name="install-the-required-powershell-modules"></a>Gerekli PowerShell modüllerini yükleyin

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell'i yükleme ve yapılandırma
İçin makaledeki yönergeleri [Azure AD PowerShell modülünü yüklemek ve Azure AD'ye bağlanma](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
İçin makaledeki yönergeleri [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlayın](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="disable-weak-cipher-suites-and-ntlm-credential-hash-synchronization"></a>Zayıf şifre paketleri ve NTLM kimlik bilgisi karması eşitleme devre dışı bırak
Aşağıdaki PowerShell betiğini kullanın:
1. Yönetilen etki alanında NTLM v1 desteğini devre dışı bırakın.
2. NTLM, şirket içi parola karmalarının eşitlenmesini devre dışı bırakmak AD.
3. Yönetilen etki alanında TLS v1’i devre dışı bırakın.

```powershell
// Login to your Azure AD tenant
Login-AzAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// 1. Disable NTLM v1 support on the managed domain.
// 2. Disable the synchronization of NTLM password hashes from
//    on-premises AD to Azure AD and Azure AD Domain Services
// 3. Disable TLS v1 on the managed domain.
$securitySettings = @{"DomainSecuritySettings"=@{"NtlmV1"="Disabled";"SyncNtlmPasswords"="Disabled";"TlsV1"="Disabled"}}

// Apply the settings to the managed domain.
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $securitySettings -Verbose -Force
```

> [!IMPORTANT]
> Azure AD Domain Services örneğinizin NTLM parola karması eşitleme devre dışı, kullanıcıların (ve hizmet hesapları) LDAP basit bağlamaları gerçekleştirilemiyor.  NTLM parola karması eşitleme devre dışı bırakma hakkında daha fazla bilgi için okuma [Azure AD DOmain Services yönetilen etki alanınıza güvenli](secure-your-domain.md).
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Etki Alanı Hizmetleri'nde eşitleme anlama](synchronization.md)
