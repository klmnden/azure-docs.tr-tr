---
title: Bir kullanıcı veya grup ataması kurumsal bir uygulamayı Azure Active Directory'de kaldırma | Microsoft Docs
description: Bir kullanıcı veya grup erişimi atama bir Azure Active Directory'de Kurumsal uygulamadan kaldırma
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/14/2018
ms.author: celested
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b72ec628e048560fbfb9da63123bbb7461811b9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58074293"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamasını kaldırma
Erişim için Kurumsal uygulamalarınızı Azure Active Directory'de (Azure AD) birini atanmasını bir kullanıcıyı veya grubu kaldırmak kolay bir işlemdir. Kurumsal uygulamasını yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olması gerekir.

> [!NOTE]
> (Örneğin, Office 365 uygulamaları) Microsoft Applications, kullanıcıların kurumsal bir uygulamayı kaldırmak için PowerShell kullanın.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Azure portalında bir kullanıcı veya grup ataması kurumsal bir uygulamanın nasıl kaldırırım?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. Seçin **diğer hizmetler**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory - *directoryname***  seçin (diğer bir deyişle, Azure AD sayfasının yönetirken dizini için), sayfa **kurumsal uygulamalar**.

    ![Açılış kurumsal uygulamalar](./media/remove-user-or-group-access-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** sayfasında **tüm uygulamaları**. Yönetebileceğiniz uygulamaların listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamalar** sayfasında, bir uygulama seçin.
6. Üzerinde ***appname*** seçin (diğer bir deyişle, sayfa başlığını seçilen uygulamanın adını), sayfa **kullanıcıları ve grupları**.

    ![Kullanıcıları veya grupları seçme](./media/remove-user-or-group-access-portal/remove-app-users.png)
7. Üzerinde ***appname*** **-kullanıcı ve Grup ataması** sayfasında daha fazla kullanıcı ya da grupları seçin ve ardından **Kaldır** komutu. Kararınız satırında onaylayın.

    ![Remove komutu seçme](./media/remove-user-or-group-access-portal/remove-users.png)

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Bir kullanıcı veya grup ataması PowerShell kullanarak bir kurumsal uygulamasına nasıl kaldırabilirim?
1. Yükseltilmiş bir Windows PowerShell komut istemi açın.

    >[!NOTE] 
    > AzureAD modülüne yüklemeniz gerekir (komutunu `Install-Module -Name AzureAD`). Bir NuGet modül veya yeni Azure Active Directory V2 PowerShell modülünü yüklemeniz istenirse, Y yazın ve ENTER tuşuna basın.

2. Çalıştırma `Connect-AzureAD` ve bir genel yönetici kullanıcı hesabıyla oturum açın.
3. Bir uygulamaya ait bir kullanıcı ve rol kaldırmak için aşağıdaki betiği kullanın:

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

- [Tüm Gruplarım](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Kurumsal bir uygulamayı kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
- [Kullanıcı oturum açma Kurumsal uygulama için devre dışı bırak](disable-user-sign-in-portal.md)
- [Adını veya kurumsal bir uygulamanın logoyu değiştirme](change-name-or-logo-portal.md)
