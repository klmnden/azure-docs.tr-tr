---
title: Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup ataması kaldırma | Microsoft Docs
description: Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup erişimi atamasını kaldırmak nasıl
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
ms.date: 02/14/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: b6da8eed16b67db098ceb90079b7da7dfadcd5e3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303939"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup ataması kaldırma
Bir kullanıcı veya grup erişimi Azure Active Directory'de (Azure AD), Kurumsal uygulamalarınızın birini atanmasına kaldırmak daha kolaydır. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir.

> [!NOTE]
> İçin Microsoft Applications (örneğin, Office 365 uygulamaları), Kurumsal uygulama kullanıcılara kaldırmak için PowerShell kullanın.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Nasıl bir kullanıcı veya grup ataması Kurumsal uygulama için Azure portalında kaldırabilirim?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **daha fazla hizmet**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  seçme sayfası (diğer bir deyişle, Azure AD sayfa yönettiğiniz dizin için), **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/remove-user-or-group-access-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** sayfasında, **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** sayfasında, bir uygulama seçin.
6. Üzerinde ***appname*** seçme sayfası (diğer bir deyişle, Sayfa başlığında seçilen uygulamanın adını), **kullanıcıları ve grupları**.

    ![Kullanıcıları veya grupları seçme](./media/remove-user-or-group-access-portal/remove-app-users.png)
7. Üzerinde ***appname*** **-kullanıcı ve grup atama** sayfasında, daha fazla kullanıcı veya grup seçin ve ardından **kaldırmak** komutu. Kararınızı satırında onaylayın.

    ![Remove komutu seçme](./media/remove-user-or-group-access-portal/remove-users.png)

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Bir kullanıcı veya grup ataması PowerShell kullanarak bir kurumsal uygulama için nasıl kaldırabilirim?
1. Yükseltilmiş bir Windows PowerShell komut istemi açın.

    >[!NOTE] 
    > Azuread'i modülü yüklemeniz gerekir (komutunu `Install-Module -Name AzureAD`). NuGet modülü veya yeni Azure Active Directory V2 PowerShell modülü yüklemek isteyip istemediğiniz sorulduğunda Y yazın ve ENTER tuşuna basın.

2. Çalıştırma `Connect-AzureAD` ve bir genel yönetici kullanıcı hesabıyla oturum açın.
3. Bir kullanıcı ve rol uygulamaya atamak için aşağıdaki komut dosyasını kullanın:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ``` 
## <a name="next-steps"></a>Sonraki adımlar

- [Tüm my gruplarının bakın](../active-directory-groups-view-azure-portal.md)
- [Bir kullanıcı veya grup için bir kuruluş uygulama atama](assign-user-or-group-access-portal.md)
- [Kullanıcı oturum açma işlemleri bir kurumsal uygulama için devre dışı bırak](disable-user-sign-in-portal.md)
- [Adını veya bir kuruluş uygulama logosunu değiştirme](change-name-or-logo-portal.md)
