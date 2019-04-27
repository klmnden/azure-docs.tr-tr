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
ms.date: 04/12/2019
ms.author: celested
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97759ae992ebe38aa85e9b4724edeebb5285db4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60443090"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamasını kaldırma
Kurumsal uygulamalarınızı Azure Active Directory'de (Azure AD) birine atanan erişimden bir kullanıcıyı veya grubu kaldırmak kolay bir işlemdir. Kurumsal uygulama Yönetme iznine ihtiyacınız var. Ve dizin için genel yönetici olması gerekir.

> [!NOTE]
> (Örneğin, Office 365 uygulamaları) Microsoft Applications, kullanıcıların kurumsal bir uygulamayı kaldırmak için PowerShell kullanın.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Azure portalında bir kullanıcı veya grup ataması kurumsal bir uygulamanın nasıl kaldırırım?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
1. Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
1. Üzerinde **Azure Active Directory - *directoryname***  seçin (diğer bir deyişle, Azure AD sayfasının yönettiğiniz dizini için), sayfa **kurumsal uygulamalar**.
1. Üzerinde **kurumsal uygulamalar - tüm uygulamalar** sayfasında yönetebileceğiniz uygulamaların listesini göreceksiniz. Bir uygulama seçin.
1. Üzerinde ***appname*** seçin (diğer bir deyişle, sayfa başlığını seçilen uygulamanın adını), genel bakış sayfasında **kullanıcıları ve grupları**.
1. Üzerinde ***appname*** **-kullanıcı ve Grup ataması** sayfasında daha fazla kullanıcı ya da grupları seçin ve ardından **Kaldır** komutu. Kararınız satırında onaylayın.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Bir kullanıcı veya grup ataması PowerShell kullanarak bir kurumsal uygulamasına nasıl kaldırabilirim?
1. Yükseltilmiş bir Windows PowerShell komut istemi açın.

    >[!NOTE] 
    > AzureAD modülüne yüklemeniz gerekir (komutunu `Install-Module -Name AzureAD`). Bir NuGet modül veya yeni Azure Active Directory V2 PowerShell modülünü yüklemeniz istenirse, Y yazın ve ENTER tuşuna basın.

1. Çalıştırma `Connect-AzureAD` ve bir genel yönetici kullanıcı hesabıyla oturum açın.
1. Bir uygulamaya ait bir kullanıcı ve rol kaldırmak için aşağıdaki betiği kullanın:

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
