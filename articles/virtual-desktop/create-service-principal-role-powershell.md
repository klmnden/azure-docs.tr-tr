---
title: Windows sanal masaüstü Önizleme hizmet sorumluları ve rol atamalarını - Azure PowerShell ile oluşturma
description: Hizmet sorumluları oluşturma ve Windows sanal masaüstü Önizleme'de PowerShell ile rol atamasını nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 1bbe89484d72a21c4432d452d4ddae83ea2d2553
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400021"
---
# <a name="tutorial-create-service-principals-and-role-assignments-with-powershell"></a>Öğretici: PowerShell ile hizmet sorumluları ve rol atamalarını oluşturma

Hizmet sorumluları belirli bir amaç için rolleri ve izinleri atamak için Azure Active Directory oluşturabileceğiniz Kimlikleridir. Windows sanal masaüstü Önizleme'de, bir hizmet sorumlusunu oluşturabilirsiniz:

- Belirli Windows sanal masaüstü yönetim görevlerini otomatikleştirin
- Herhangi bir Windows sanal masaüstü Azure Resource Manager şablonu çalıştırılırken yerine kullanıcıları MFA gerekli kimlik bilgilerini kullanın

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Active Directory'de hizmet sorumlusu oluşturma
> * Windows sanal masaüstü rol ataması oluşturma
> * Windows sanal masaüstü hizmet sorumlusu ile oturum açın

## <a name="prerequisites"></a>Önkoşullar

Hizmet sorumluları ve rol atamaları oluşturmadan önce üç şey yapmanız gerekir:

1. AzureAD modülüne yükleyin. Modülü yüklemek için PowerShell'i yönetici olarak çalıştırın ve aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    Install-Module AzureAD
    ```

2. Teklif oturumunuza geçerli değerleri yerine değerleri aşağıdaki cmdlet'leri çalıştırın.

    ```powershell
    $myTenantGroupName = "<my-tenant-group-name>"
    $myTenantName = "<my-tenant-name>"
    ```

3. Aynı PowerShell oturumunda bu makaledeki tüm yönergeleri izleyin. Pencereyi kapatın ve daha sonra geri çalışmayabilir.

## <a name="create-a-service-principal-in-azure-active-directory"></a>Azure Active Directory'de hizmet sorumlusu oluşturma

PowerShell oturumunuzda önkoşulları yerine sonra çok kiracılı bir hizmet sorumlusu azure'da oluşturmak için aşağıdaki PowerShell cmdlet'lerini çalıştırın.

```powershell
Import-Module AzureAD
$aadContext = Connect-AzureAD
$svcPrincipal = New-AzureADApplication -AvailableToOtherTenants $true -DisplayName "Windows Virtual Desktop Svc Principal"
$svcPrincipalCreds = New-AzureADApplicationPasswordCredential -ObjectId $svcPrincipal.ObjectId
```

## <a name="create-a-role-assignment-in-windows-virtual-desktop-preview"></a>Windows sanal masaüstü Önizleme'de bir rol ataması oluştur

Bir hizmet sorumlusu oluşturmuş olduğunuz, Windows sanal masaüstü oturum açmak için kullanabilirsiniz. Rol ataması oluşturma izinlerine sahip bir hesapla oturum emin olun.

İlk olarak, [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

Windows sanal masaüstüne bağlanmak ve bir rol ataması için hizmet sorumlusu oluşturmak için aşağıdaki PowerShell cmdlet'lerini çalıştırın.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsContext -TenantGroupName $myTenantGroupName
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantGroupName $myTenantGroupName -TenantName $myTenantName
```

## <a name="sign-in-with-the-service-principal"></a>Hizmet sorumlusuyla oturum açın

Bir rol ataması için hizmet sorumlusu oluşturduktan sonra artık hizmet sorumlusu aşağıdaki cmdlet'i çalıştırarak Windows sanal masaüstü oturum açarak emin olmanız gerekir:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Bir kez açtığınız, hizmet sorumlusu birkaç Windows sanal masaüstü PowerShell cmdlet'leriyle test ederek her şeyin çalıştığından emin olun.

## <a name="view-your-credentials-in-powershell"></a>PowerShell'de kimlik bilgilerinizi görüntüleyin

PowerShell oturumunuzu sonlandırmak önce kimlik bilgilerinizi görüntüleyin ve gelecekte başvurulmak üzere yazmanızı gerekir. Parola özellikle önemlidir çünkü bu bir PowerShell oturumu kapattıktan sonra onu almak mümkün olmayacaktır.

Not üç kimlik bilgilerini ve bunları almak için çalıştırmak için ihtiyacınız olan cmdlet'lerin şunlardır:

- Parola:

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Kiracı Kimliği:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Uygulama Kimliği:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir hizmet sorumlusu oluşturma ve Windows sanal masaüstü ile oturum açma öğrendiniz. Windows sanal masaüstü oturum açma hakkında daha fazla bilgi için Windows sanal masaüstü bilgi belgeleri Bağlan devam edin.

- [Uzak Masaüstü İstemcisi Windows 7 ve Windows 10 bağlanma](connect-windows-7-and-10.md)
- [Windows sanal masaüstü Önizleme web istemcisi için Bağlan](connect-web.md)
