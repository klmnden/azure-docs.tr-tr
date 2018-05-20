---
title: Azure yığınında çoklu kiracı etkinleştirme | Microsoft Docs
description: Birden çok Azure Active Directory dizin Azure yığınında destek öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: mabrigg
ms.openlocfilehash: 59b0f8e4c7234b246d4fb54d065ff318939e2662
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="enable-multi-tenancy-in-azure-stack"></a>Çoklu kiracı Azure yığınında etkinleştir

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Hizmetleri Azure yığınında kullanmak için birden çok Azure Active Directory (Azure AD) kiracıdan kullanıcıları desteklemek için yığın yapılandırabilirsiniz. Örnek olarak, aşağıdaki senaryoyu göz önünde bulundurun:

 - Azure yığın yüklendiği contoso.onmicrosoft.com, Hizmet Yöneticisi olduğunuz.
 - Mary dizin fabrikam.onmicrosoft.com, Konuk kullanıcılar bulunduğu yöneticisidir. 
 - Mary'nin şirket Iaas ve PaaS Hizmetleri şirketinizden alır ve Konuk dizin (fabrikam.onmicrosoft.com) oturum açın ve Azure yığın kaynakları içinde contoso.onmicrosoft.com kullanıcıların izin vermek gerekiyor.

Bu kılavuz, Azure yığınında çoklu kiracı yapılandırmak için bu senaryo bağlamında gereken adımları sağlar.  Bu senaryoda, siz ve Mary oturum açın ve Contoso Azure yığın dağıtımda Hizmetleri'nden kullanmak için Fabrikam kullanıcıları etkinleştirme adımları tamamlamanız gerekir.  

## <a name="before-you-begin"></a>Başlamadan önce
Çoklu kiracı Azure yığınında yapılandırmadan önce hesaba birkaç ön koşul vardır:
  
 - Sizin ve Mary yönetim adımları hem Azure yığın (Contoso) içinde yüklü olduğu dizin ve Konuk dizin (Fabrikam) koordine gerekir.  
 - Seçtiğiniz emin olun [yüklü](azure-stack-powershell-install.md) ve [yapılandırılmış](azure-stack-powershell-configure-admin.md) Azure yığını için PowerShell.
 - [Azure yığın Araçları'nı indirmek](azure-stack-powershell-download.md)ve Connect ve kimlik modülleri alın:

    ````PowerShell
        Import-Module .\Connect\AzureStack.Connect.psm1
        Import-Module .\Identity\AzureStack.Identity.psm1
    ```` 
 - Mary gerektirecektir [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) Azure yığın erişimi. 

## <a name="configure-azure-stack-directory"></a>Azure yığın dizin yapılandırın
Bu bölümde, Azure oturum açma işlemleri Fabrikam Azure AD directory kiracılardan izin vermek için yığın yapılandırın.

### <a name="onboard-guest-directory-tenant"></a>Yerleşik Konuk dizin Kiracı
Ardından, yerleşik Azure yığınına Konuk dizin Kiracı (Fabrikam).  Bu adım, Azure Resource Manager'ı, kullanıcı ve hizmet asıl adı Konuk directory kiracısındaki kabul edecek şekilde yapılandırır.

````PowerShell
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location "local" `
 -ResourceGroupName $ResourceGroupName
````



## <a name="configure-guest-directory"></a>Konuk dizin yapılandırın
Azure yığın dizininde adımları tamamladıktan sonra Mary izin Azure yığınına Konuk dizinine erişmesini sağlamak ve Azure yığın Konuk Directory'ye kaydetme gerekir. 

### <a name="registering-azure-stack-with-the-guest-directory"></a>Azure yığın Konuk directory ile kaydetme
Konuk dizin Yöneticisi Azure Fabrikam'ın dizine erişmek için yığın onaylarının sağlamıştır sonra Mary'nin Azure yığın Fabrikam'ın dizin Kiracı ile kaydetmeniz gerekir.

````PowerShell
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````
## <a name="direct-users-to-sign-in"></a>Kullanıcıların oturum açmak için
Sizin ve Mary yerleşik Mary'nin dizinine adımları tamamladığınıza göre Mary Fabrikam oturum açmalarını yönlendirebilirsiniz.  Fabrikam kullanıcıları (diğer bir deyişle, kullanıcılar fabrikam.onmicrosoft.com sonekiyle) oturum ziyaret ederek https://portal.local.azurestack.external.  

Mary herhangi doğrudan [yabancı sorumluları](../role-based-access-control/rbac-and-directory-admin-roles.md) Fabrikam dizininde (Fabrikam dizin fabrikam.onmicrosoft.com soneki olmadan kullanıcılar) kullanarak oturum https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.  Bu URL kullanmayın, varsayılan dizini (Fabrikam) için gönderilen ve bunların yönetici olmayan seçtiği bildiren bir hata alıyorsunuz.

## <a name="next-steps"></a>Sonraki Adımlar

- [Temsilci sağlayıcılarını Yönet](azure-stack-delegated-provider.md)
- [Azure yığın temel kavramları](azure-stack-key-features.md)
