---
title: PowerShell - Azure'ı kullanarak Windows sanal masaüstü Önizleme hizmet sorumluları ve rol atamalarını oluşturma
description: Nasıl hizmet sorumlusu oluşturma ve sanal masaüstü Önizleme'de Windows PowerShell kullanarak rolleri atayın.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/12/2019
ms.author: helohr
ms.openlocfilehash: 44c823653ecbad1c4dd1fd35b676c8a6d8bd1620
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206652"
---
# <a name="tutorial-create-service-principals-and-role-assignments-by-using-powershell"></a>Öğretici: Hizmet sorumluları ve rol atamalarını PowerShell kullanarak oluşturma

Hizmet sorumluları belirli bir amaç için rolleri ve izinleri atamak için Azure Active Directory oluşturabileceğiniz Kimlikleridir. Windows sanal masaüstü Önizleme'de, bir hizmet sorumlusunu oluşturabilirsiniz:

- Belirli Windows sanal masaüstü yönetim görevlerini otomatikleştirin.
- Windows sanal masaüstü için herhangi bir Azure Resource Manager şablonu çalıştırılırken yerine kullanıcıları MFA gerekli kimlik bilgilerini kullanın.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Azure Active Directory'de Hizmet sorumlusu oluşturun.
> * Windows sanal masaüstü bir rol ataması oluşturun.
> * Windows sanal masaüstüne hizmet sorumlusunu kullanarak oturum açın.

## <a name="prerequisites"></a>Önkoşullar

Hizmet sorumluları ve rol atamaları oluşturmadan önce üç şey yapmanız gerekir:

1. AzureAD modülüne yükleyin. Modülü yüklemek için PowerShell'i yönetici olarak çalıştırın ve aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    Install-Module AzureAD
    ```

2. Teklif oturumunuza geçerli değerleri yerine değerleri aşağıdaki cmdlet'leri çalıştırın.

    ```powershell
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
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantName $myTenantName
```

## <a name="sign-in-with-the-service-principal"></a>Hizmet sorumlusuyla oturum açın

Bir rol ataması için hizmet sorumlusu oluşturduktan sonra hizmet sorumlusu aşağıdaki cmdlet'i çalıştırarak Windows sanal masaüstü oturum açarak emin olun:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Oturumunuz açıldıktan sonra hizmet sorumlusu birkaç Windows sanal masaüstü PowerShell cmdlet'leriyle test ederek her şeyin çalıştığından emin olun.

## <a name="view-your-credentials-in-powershell"></a>PowerShell'de kimlik bilgilerinizi görüntüleyin

PowerShell oturumunuzu sonlandırmak önce kimlik bilgilerinizi görüntüleyin ve gelecekte başvurulmak üzere not edin. Parola özellikle önemlidir çünkü bu bir PowerShell oturumu kapattıktan sonra onu almak mümkün olmayacaktır.

Not üç kimlik bilgilerini ve bunları almak için çalıştırmak için ihtiyacınız olan cmdlet'lerin şunlardır:

- Parola:

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Kiracı kimliği:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Uygulama Kimliği:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="next-steps"></a>Sonraki adımlar

Hizmet sorumlusu oluşturup Windows sanal masaüstü kiracınızda bir rolü atanmış sonra bir konak havuzu oluşturmak için kullanabilirsiniz. Ana bilgisayar havuzları hakkında daha fazla bilgi için Windows sanal masaüstü konak havuzu oluşturmak için öğreticisiyle devam edin.

 > [!div class="nextstepaction"]
 > [Windows sanal masaüstü konak havuzu Öğreticisi](./create-host-pools-azure-marketplace.md)
