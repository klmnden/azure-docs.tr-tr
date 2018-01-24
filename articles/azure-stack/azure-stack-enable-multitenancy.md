---
title: "Azure yığınında çoklu kiracı etkinleştirme | Microsoft Docs"
description: "Birden çok Azure Active Directory dizin Azure yığınında destek öğrenin"
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: a2cba85a553f20040d2fb118b35859b05870e361
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
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

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location "local"
````



## <a name="configure-guest-directory"></a>Konuk dizin yapılandırın
Azure yığın dizininde adımları tamamladıktan sonra Mary izin Azure yığınına Konuk dizinine erişmesini sağlamak ve Azure yığın Konuk Directory'ye kaydetme gerekir. 

### <a name="registering-azure-stack-with-the-guest-directory"></a>Azure yığın Konuk directory ile kaydetme
Konuk dizin Yöneticisi Azure Fabrikam'ın dizine erişmek için yığın onaylarının sağlamıştır sonra bunlar Azure yığın Fabrikam'ın directory kiracısı ile kaydetmeniz gerekir.

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
Sizin ve Mary yerleşik Mary'nin dizinine adımları tamamladığınıza göre Mary Fabrikam oturum açmalarını yönlendirebilirsiniz.  Https://portal.local.azurestack.external adresini ziyaret ederek Fabrikam kullanıcıları (diğer bir deyişle, kullanıcılar fabrikam.onmicrosoft.com sonekiyle) oturum açın.  

Mary herhangi doğrudan [yabancı sorumluları](../active-directory/active-directory-understanding-resource-access.md) Fabrikam dizininde (Fabrikam dizin fabrikam.onmicrosoft.com soneki olmadan kullanıcılar) https://portal.local.azurestack.external/fabrikam.onmicrosoft.com kullanarak oturum.  Bu URL kullanmayın, varsayılan dizini (Fabrikam) için gönderilen ve bunların yönetici olmayan seçtiği bildiren bir hata alıyorsunuz.

## <a name="next-steps"></a>Sonraki Adımlar

- [Temsilci sağlayıcılarını Yönet](azure-stack-delegated-provider.md)
- [Azure yığın temel kavramları](azure-stack-key-features.md)
