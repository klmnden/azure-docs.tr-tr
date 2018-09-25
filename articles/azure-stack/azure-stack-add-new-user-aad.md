---
title: Yeni bir Azure Stack Kiracı hesabı Azure Active Directory'ye ekleme | Microsoft Docs
description: Microsoft Azure Stack geliştirme Seti'ni dağıttıktan sonra Kiracı portalında keşfedebilirsiniz için en az bir Kiracı Kullanıcı hesabı oluşturmanız gerekir.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 9a4d7200a2bc2445fcdfefc0332d67a045b5a2e1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038026"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Azure Active Directory'de yeni bir Azure Stack Kiracı hesabı Ekle

Sonra [Azure Stack geliştirme Seti'ni dağıtma](azure-stack-run-powershell-script.md), Kiracı portalında keşfedin ve test teklifleri ve planları için bir Kiracı Kullanıcı hesabı gerekir. Bir kiracı hesabı ile oluşturabileceğiniz [Azure portalını kullanarak](#create-an-azure-stack-tenant-account-using-the-azure-portal) ya da [PowerShell kullanarak](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Azure portalını kullanarak bir Azure Stack Kiracı hesabı oluşturun

Azure portalını kullanmak üzere bir Azure aboneliğine sahip olmalıdır.

1. Oturum [Azure](https://portal.azure.com).
2. Sol gezinti çubuğunda **Active Directory** ve Azure Stack için kullanmak istediğiniz dizine geçin veya yeni bir tane oluşturun.
3. Seçin **Azure Active Directory** > **kullanıcılar** > **yeni kullanıcı**.

    ![Kullanıcılar - yeni kullanıcı ile vurgulanmış tüm kullanıcılar sayfası](media/azure-stack-add-new-user-aad/new-user-all-users.png)

4. Üzerinde **kullanıcı** sayfasında, gerekli bilgileri doldurun.

    ![Yeni kullanıcı, kullanıcı bilgileri kullanıcının Sayfası Ekle](media/azure-stack-add-new-user-aad/new-user-user.png)

    - **Ad (gerekli).** Yeni kullanıcı ilk ve son adı. Örneğin, Gamze Parker.
    - **Kullanıcı adı (gerekli).** Yeni kullanıcının kullanıcı adı. Örneğin, mary@contoso.com.
        Kullanıcı adının etki alanı bölümünü ya da ilk varsayılan etki alanı adı kullanmanız gerekir <_etkialanıadınız_>. onmicrosoft.com ya da bir özel etki alanı adı contoso.com gibi. Özel etki alanı adı oluşturma hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye özel etki alanı adı ekleme](../active-directory/fundamentals/add-custom-domain.md).
    - **Profili.** İsteğe bağlı olarak, kullanıcı hakkında daha fazla bilgi ekleyebilirsiniz. Daha sonraki bir zamanda kullanıcı bilgileri de ekleyebilirsiniz. Kullanıcı bilgileri ekleme hakkında daha fazla bilgi için bkz. [eklemek veya kullanıcı profili bilgilerini değiştirmek nasıl](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md).
    - **Dizin rolü.**  seçin **kullanıcı**.

5. Denetleme **Göster parola** ve sağlanan otomatik olarak oluşturulan parola kopyalayın **parola** kutusu. İlk oturum açma işlemi için bu parola gerekir.

6. **Oluştur**’u seçin.

    Kullanıcı oluşturulur ve Azure AD kiracınıza eklenir.

7. Microsoft Azure portalında yeni hesapla oturum açın. İstendiğinde parolayı değiştirin.
8. Oturum `https://portal.local.azurestack.external` Kiracı portalında görmek için yeni bir hesap ile.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>PowerShell kullanarak bir Azure Stack Kiracı hesabı oluşturma

Azure aboneliğiniz yoksa, bir Kiracı Kullanıcı hesabı eklemek için Azure portal'ı kullanamazsınız. Bu durumda, bunun yerine Azure Active Directory için Windows PowerShell modülü kullanabilirsiniz.

> [!NOTE]
> Azure Stack geliştirme Seti'ni dağıtmak için Microsoft Account (Live ID) kullanıyorsanız, Kiracı hesabı oluşturmak için AAD PowerShell kullanamazsınız. 
> 
> 

1. Yükleme [Microsoft Çevrimiçi Hizmetler oturum açma Yardımcısı BT uzmanları RTW için](https://www.microsoft.com/en-us/download/details.aspx?id=41950).
2. Yükleme [Azure Active Directory için Windows PowerShell Modülü (64-bit sürüm)](http://go.microsoft.com/fwlink/p/?linkid=236297) ve açın.
3. Aşağıdaki cmdlet'leri çalıştırın:

    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack Development Kit

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. Microsoft Azure'da yeni bir hesapla oturum açın. İstendiğinde parolayı değiştirin.
2. Oturum `https://portal.local.azurestack.external` Kiracı portalında görmek için yeni bir hesap ile.

