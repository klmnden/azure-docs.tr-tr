---
title: Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama | Microsoft Docs
description: Azure Active Directory'de bir kullanıcı veya grup için atama için kurumsal uygulama seçme
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
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: barbkess
ms.reviewer: luleon
ms.openlocfilehash: ee0b14123e193f219e403d2608368c27f953013d
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037983"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atama
Bir kullanıcı veya grup için kurumsal bir uygulamayı atamak için Kurumsal uygulamasını yönetmek için uygun izinlere sahip ve dizin için genel yönetici olması gerekir.

> [!NOTE]
> Bu makalede ele alınan özellikleri için gereksinimler lisanslama için bkz: [Azure Active Directory fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory).

> [!NOTE]
> (Örneğin, Office 365 uygulamaları) Microsoft Applications, kullanıcıları kurumsal bir uygulamayı atamak için PowerShell'i kullanın.


## <a name="how-do-i-assign-user-access-to-an-enterprise-app-in-the-azure-portal"></a>Nasıl kullanıcı erişimi için Azure portalında kurumsal bir uygulamanın atarım?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**, Azure Active Directory metin kutusuna girin ve ardından **Enter**.
3. Seçin **kurumsal uygulamalar**.

    ![Açılış kurumsal uygulamalar](./media/assign-user-or-group-access-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** dikey penceresinde **tüm uygulamaları**. Bu, yönettiğiniz uygulamaları listeler.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamalar** dikey penceresinde bir uygulama seçin.
6. Üzerinde ***appname*** seçin (diğer bir deyişle, dikey penceresinin başlık seçilen uygulamanın adını), dikey **kullanıcıları ve grupları**.

    ![Tüm uygulamaları komutu seçme](./media/assign-user-or-group-access-portal/select-app-users.png)
7. Üzerinde ***appname*** **-kullanıcı ve Grup ataması** dikey penceresinde **Ekle** komutu.
8. Üzerinde **atama Ekle** dikey penceresinde **kullanıcılar ve gruplar**.

    ![Bir kullanıcının veya grubun uygulamaya atama](./media/assign-user-or-group-access-portal/assign-users.png)
9. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, listeden bir veya daha fazla kullanıcı veya grup seçin ve ardından **seçin** dikey pencerenin alt kısmındaki düğmesi.
10. Üzerinde **atama Ekle** dikey penceresinde **rol**. Ardından **rolü Seç** dikey penceresinde, seçilen kullanıcılarla veya gruplarla için geçerlidir ve ardından seçmek için rolü seçin **Tamam** dikey pencerenin alt kısmındaki düğmesi.
11. Üzerinde **atama Ekle** dikey penceresinde **atama** dikey pencerenin alt kısmındaki düğmesi. Atanan kullanıcılar veya gruplar bu kurumsal uygulama için seçili rolü tarafından tanımlanan izinlere sahiptir.

## <a name="how-do-i-assign-a-user-to-an-enterprise-app-using-powershell"></a>PowerShell kullanarak kurumsal uygulama için bir kullanıcı nasıl atarım?

1. Yükseltilmiş bir Windows PowerShell komut istemi açın.

    >[!NOTE] 
    > AzureAD modülüne yüklemeniz gerekir (komutunu `Install-Module -Name AzureAD`). Bir NuGet modül veya yeni Azure Active Directory V2 PowerShell modülünü yüklemeniz istenirse, Y yazın ve ENTER tuşuna basın.

2. Çalıştırma `Connect-AzureAD` ve bir genel yönetici kullanıcı hesabıyla oturum açın.
3. Uygulamaya kullanıcı ve rol atamak için aşağıdaki betiği kullanın:

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

Bir uygulama rolü için kullanıcı atama hakkında daha fazla bilgi için belgelerine [yeni AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)

Kurumsal bir uygulamanın bir gruba atamak için değiştirmeniz gereken `Get-AzureADUser` ile `Get-AzureADGroup`.

### <a name="example"></a>Örnek

Bu örnekte kullanıcıyı Britta Simon atayan için [Microsoft Workplace Analytics](https://products.office.com/business/workplace-analytics) PowerShell kullanarak uygulama.

1. PowerShell'de, değişkenleri $username, $app_name ve $app_role_name karşılık gelen değerler atayın. 

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

2. Bu örnekte biz Britta Simon atamak istediğiniz uygulama rolü tam adını nedir bilmiyorum. Kullanıcı ($user) almak için aşağıdaki komutları çalıştırın ve kullanıcının UPN kullanan bir hizmet sorumlusu ($sp) ve hizmet asıl adlarını görüntüler.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```
        
3. Komutunu çalıştırın `$sp.AppRoles` Workplace Analytics uygulama için kullanılabilir olan rolleri görüntülemek için. Bu örnekte, Britta Simon analist (sınırlı erişimi) rol atamak istiyoruz.
    
    ![Workplace Analytics rolü](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

4. Rol adı için Ata `$app_role_name` değişkeni.
        
    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

5. Kullanıcıya uygulama rolü ataması için aşağıdaki komutu çalıştırın:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm Gruplarım](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Kullanıcı oturum açma Kurumsal uygulama için devre dışı bırak](disable-user-sign-in-portal.md)
* [Adını veya kurumsal bir uygulamanın logoyu değiştirme](change-name-or-logo-portal.md)
