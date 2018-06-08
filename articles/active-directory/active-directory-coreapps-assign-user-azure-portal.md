---
title: Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atamak | Microsoft Docs
description: Bir kullanıcı veya grup için Azure Active Directory'de atamak için bir kuruluş uygulama seçme
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: barbkess
ms.reviewer: luleon
ms.openlocfilehash: d7e237e1e9daae3830f9a9943d54bc6bfa90a34c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830835"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atayın
Bir kullanıcı veya grup için bir kuruluş uygulama atamak için Kurumsal uygulamasını yönetmek için uygun izinlere sahip ve dizin için genel yönetici olmanız gerekir.

> [!NOTE]
> Bu makalede açıklanan özellikleri, bir Azure Active Directory Premium P1 veya Premium P2 lisansı gerektirir. Daha fazla bilgi için bkz: [fiyatlandırma sayfası Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory).

> [!NOTE]
> İçin Microsoft Applications (örneğin, Office 365 uygulamaları), kullanıcıların bir kurumsal uygulama atamak için PowerShell kullanın.


## <a name="how-do-i-assign-user-access-to-an-enterprise-app-in-the-azure-portal"></a>Nasıl ı kullanıcı erişimi Azure portalında bir kuruluş uygulaması atayabilirim?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **tüm hizmetleri**, Azure Active Directory metin kutusuna girin ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  (diğer bir deyişle, Azure AD dikey yönettiğiniz dizin için), dikey penceresinde seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** dikey penceresinde, select **tüm uygulamaları**. Bu, yönettiğiniz uygulamaları listeler.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** dikey penceresinde, bir uygulama seçin.
6. Üzerinde ***appname*** dikey penceresinde (diğer bir deyişle, dikey penceresinde seçili uygulamasının başlık adı ile) seçin **kullanıcıları ve grupları**.

    ![Tüm uygulamalar komutu seçme](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Üzerinde ***appname*** **-kullanıcı ve grup atama** dikey penceresinde, select **Ekle** komutu.
8. Üzerinde **eklemek atama** dikey penceresinde, select **kullanıcılar ve gruplar**.

    ![Bir kullanıcı veya grup için uygulama atama](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, listeden bir veya daha fazla kullanıcı veya grup seçin ve ardından **seçin** dikey pencerenin altındaki düğmesini.
10. Üzerinde **eklemek atama** dikey penceresinde, select **rol**. Ardından **rolü Seç** dikey penceresinde, seçilen kullanıcılarla veya gruplarla uygulayın ve ardından için rolü seçin **Tamam** dikey pencerenin altındaki düğmesini.
11. Üzerinde **eklemek atama** dikey penceresinde, select **atamak** dikey pencerenin altındaki düğmesini. Atanan kullanıcılar veya gruplar bu kurumsal uygulama için seçili rol tarafından tanımlanan izinlere sahip.

## <a name="how-do-i-assign-a-user-to-an-enterprise-app-using-powershell"></a>PowerShell kullanarak bir kurumsal uygulama için nasıl bir kullanıcı atayabilirim?

1. Yükseltilmiş bir Windows PowerShell komut istemi açın.

    >[!NOTE] 
    > Azuread'i modülü yüklemeniz gerekir (komutunu `Install-Module -Name AzureAD`). NuGet modülü veya yeni Azure Active Directory V2 PowerShell modülü yüklemek isteyip istemediğiniz sorulduğunda Y yazın ve ENTER tuşuna basın.

2. Çalıştırma `Connect-AzureAD` ve bir genel yönetici kullanıcı hesabıyla oturum açın.
3. Bir kullanıcı ve rol uygulamaya atamak için aşağıdaki komut dosyasını kullanın:

    ```powershell
    # Assign the values to the variables
    $username = "<You user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"
    
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```     

Bir kullanıcı bir uygulama rolü atama hakkında daha fazla bilgi için belgelere ziyaret [yeni AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)

Bir kurumsal uygulama için bir gruba atamak için değiştirmeniz gerekiyor `Get-AzureADUser` ile `Get-AzureADGroup`.

### <a name="example"></a>Örnek

Bu örnek Britta Simon kullanıcı atar için [Microsoft çalışma alanına Analytics](https://products.office.com/business/workplace-analytics) PowerShell kullanarak uygulama.

1. PowerShell'de değişkenleri $username, $app_name ve $app_role_name karşılık gelen değerler atayın. 

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

2. Bu örnekte, biz Britta Simon atamak istediğiniz uygulama rolü tam adı nedir bilinmiyor. Kullanıcı ($user) almak için aşağıdaki komutları çalıştırın ve Kullanıcı UPN kullanarak hizmet sorumlusu ($sp) ve hizmet asıl adlarını görüntüler.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```
        
3. Komutu çalıştırın `$sp.AppRoles` çalışma alanına Analytics uygulama için kullanılabilir rolleri görüntülemek için. Bu örnekte, Britta Simon analist (sınırlı erişimi) rol atamak istiyoruz.
    
    ![Çalışma alanına Analytics rolü](media/active-directory-coreapps-assign-user-azure-portal/workplace-analytics-role.png)

4. Rol adı atamak `$app_role_name` değişkeni.
        
    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

5. Kullanıcının uygulama role atamak için aşağıdaki komutu çalıştırın:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm my gruplarının bakın](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Kullanıcı oturum açma işlemleri bir kurumsal uygulama için devre dışı bırak](active-directory-coreapps-disable-app-azure-portal.md)
* [Adını veya bir kuruluş uygulama logosunu değiştirme](active-directory-coreapps-change-app-logo-user-azure-portal.md)
