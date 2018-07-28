---
title: Azure Stack'te çok kiracılılık
description: Azure Stack'te birden çok Azure Active Directory dizin desteği hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: patricka
ms.openlocfilehash: e61b4457cd88c236145ce7595ee7db4340538465
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331062"
---
# <a name="multi-tenancy-in-azure-stack"></a>Azure Stack'te çok kiracılılık

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack, Azure Stack'te hizmetler kullanmak için Azure Active Directory (Azure AD) birden çok kiracıdan gelen kullanıcıların destekleyecek şekilde yapılandırabilirsiniz. Örneğin, aşağıdaki senaryoyu göz önünde bulundurun:

 - Azure Stack yüklendiği contoso.onmicrosoft.com adresinin, Hizmet Yöneticisi olursunuz.
 - Mary Directory Fabrikam.onmicrosoft.com adresli, konuk kullanıcıların bulunduğu yere yöneticisidir. 
 - Mary'nin şirket Iaas ve PaaS Hizmetleri, şirketten alır ve Azure Stack kaynaklarına contoso.onmicrosoft.com olarak oturum açın ve kullanıcıların Konuk dizininden (Fabrikam.onmicrosoft.com adresli) gerekir.

Bu kılavuz, çok kiracılı Azure Stack'te yapılandırmak için bu senaryo bağlamında gerekli adımları sağlar. Bu senaryoda ve birincil kullanıcılar oturum açabilir ve Azure Stack dağıtım contoso'da servislerini Fabrikam etkinleştirme adımları tamamlamanız gerekir.  

## <a name="enable-multi-tenancy"></a>Çok kiracılı modeli etkinleştirme

Azure Stack'te çok kiracılı yapılandırmadan önce hesaba birkaç önkoşul vardır:
  
 - Siz ve birincil yönetim adımları hem Azure Stack (Contoso) içinde yüklü olduğu dizin ve Konuk dizin (Fabrikam) arasında koordine gerekir.  
 - Seçtiğiniz emin [yüklü](azure-stack-powershell-install.md) ve [yapılandırılmış](azure-stack-powershell-configure-admin.md) Azure Stack için PowerShell.
 - [Azure Stack araçları indirin](azure-stack-powershell-download.md)ve bağlanma ve kimlik modülleri içeri aktarın:

    ````PowerShell  
    Import-Module .\Connect\AzureStack.Connect.psm1
    Import-Module .\Identity\AzureStack.Identity.psm1
    ````

 - Mary gerektirecek [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) Azure Stack için erişim. 

### <a name="configure-azure-stack-directory"></a>Azure Stack dizinini yapılandırın

Bu bölümde, Azure Stack, oturum açma Fabrikam Azure AD directory kiracılardan izin verecek şekilde yapılandırın.

Yerleşik yapılandırarak Azure Resource Manager'ı, kullanıcıların kabul etmesi ve hizmet sorumluları Konuk dizin kiracısında Azure Stack için konuk dizin Kiracı (Fabrikam).

````PowerShell  
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

## Replace the value below with the region location of the resource group. 
$location = "local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location $location `
 -ResourceGroupName $ResourceGroupName
````

### <a name="configure-guest-directory"></a>Konuk dizininden yapılandırın

Azure Stack dizinde adımları tamamladıktan sonra Mary Konuk dizine Azure Stack için rıza sağlamanın ve Azure Stack ile Konuk dizine kaydedin. 

#### <a name="registering-azure-stack-with-the-guest-directory"></a>Azure Stack ile Konuk dizininden kaydediliyor

Mary Konuk dizin Yöneticisi Fabrikam'ın dizine erişmek Azure Stack için bir onay vermediği sonra Azure Stack Fabrikam'ın directory kiracısı ile kaydetmeniz gerekir.

````PowerShell
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````

> [!IMPORTANT]
> Azure Stack yöneticinize yeni hizmetlerin veya güncelleştirmelerin gelecekte yüklerse, bu komut dosyasını yeniden çalıştırmanız gerekebilir.
>
> Bu betik dizininizdeki Azure Stack uygulamalarının durumunu denetlemek için dilediğiniz zaman yeniden çalıştırın.

### <a name="direct-users-to-sign-in"></a>Doğrudan kullanıcılar oturum açabilir

Mary, Fabrikam kullanıcılar oturum açabilir ve Mary ekleme Mary'nin dizinine adımları tamamladığınıza göre yönlendirebilir.  Fabrikam kullanıcıların (diğer bir deyişle, Fabrikam.onmicrosoft.com adresli sonekini taşıyan kullanıcılar) ziyaret ederek oturum https://portal.local.azurestack.external.  

Mary herhangi doğrudan [yabancı sorumlu](../role-based-access-control/rbac-and-directory-admin-roles.md) Fabrikam dizininde (diğer bir deyişle, Fabrikam.onmicrosoft.com adresli soneki olmadan Fabrikam dizinindeki kullanıcı) kullanarak oturum açmanız https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.  Bu URL'yi kullanmayın, varsayılan dizini (Fabrikam) için gönderilen ve bunların yönetici olmayan onay verdi diyen bir hata alırsınız.

## <a name="disable-multi-tenancy"></a>Çok kiracılılık devre dışı bırak

Azure Stack birden fazla Kiracı artık istemiyorsanız aşağıdaki adımları sırayla uygulayarak çok kiracılı devre dışı bırakabilirsiniz:

1. Çalıştır (Bu senaryoda Mary), Konuk dizin Yöneticisi olarak *Unregister-AzsWithMyDirectoryTenant*. Cmdlet, tüm Azure Stack uygulamaları yeni dizinden kaldırır.

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $tenantARMEndpoint = "https://management.local.azurestack.external"
        
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantName = "fabrikam.onmicrosoft.com"
    
    Unregister-AzsWithMyDirectoryTenant `
     -TenantResourceManagerEndpoint $tenantARMEndpoint `
     -DirectoryTenantName $guestDirectoryTenantName `
     -Verbose 
    ```

2. Çalıştırma Azure Stack (, bu senaryoda), Hizmet Yöneticisi olarak *Unregister-AzSGuestDirectoryTenant*. 

    ``` PowerShell  
    ## The following Azure Resource Manaager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"
    
    ## Replace the value below with the Azure Stack directory
    $azureStackDirectoryTenant = "contoso.onmicrosoft.com"
    
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantToBeDecommissioned = "fabrikam.onmicrosoft.com"
    
    ## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
    $ResourceGroupName = "system.local"
    
    Unregister-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
     -DirectoryTenantName $azureStackDirectoryTenant `
     -GuestDirectoryTenantName $guestDirectoryTenantToBeDecommissioned `
     -ResourceGroupName $ResourceGroupName
    ```

    > [!WARNING]
    > Çok kiracılılık devre dışı bırakma adımları sırayla gerçekleştirilmesi gerekir. #2. adım önce tamamlanırsa #1. adım başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Sağlayıcı temsilcisi olarak yönetme](azure-stack-delegated-provider.md)
- [Azure Stack temel kavramları](azure-stack-key-features.md)
