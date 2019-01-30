---
title: Azure Stack için bir kayıt rolü oluşturma
description: Kayıt için genel yönetici kullanmaktan kaçınmak için özel bir rolü nasıl oluşturulur.
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
ms.date: 01/10/2019
ms.author: patricka
ms.reviewer: rtiberiu
ms.lastreviewed: 01/10/2019
ms.openlocfilehash: 80caa470675a78a9c2e3d4c055333719f54fe64a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247893"
---
# <a name="create-a-registration-role-for-azure-stack"></a>Azure Stack için bir kayıt rolü oluşturun

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir Azure aboneliği sahibi izin vermek için burada istemediğiniz senaryoları için Azure Stack kaydetmek için bir kullanıcı hesabı için izinler atamak için özel bir rol oluşturabilirsiniz.

> [!WARNING]
> Bu bir güvenlik duruşu özelliği değildir. Azure aboneliği yanlışlıkla yapılan değişiklikleri önlemek için kısıtlamalar istediğiniz senaryolarda kullanın. Bir kullanıcı bu özel rol temsilcisi hakkı olduğunda, kullanıcı hakları ve izinleri Düzenle haklarına sahiptir. Yalnızca özel bir rol için güvendiğiniz kullanıcılara atayın.

Azure Stack kaydederken, kayıt hesabı Azure abonelik izinleri ve aşağıdaki Azure Active Directory izinlerini gerektirir:

* **Azure Active Directory kiracınızdaki uygulama kayıt izinleri:** Yöneticiler uygulama kayıt izinlerine sahiptir. Kullanıcıların izinlerini, kiracıdaki tüm kullanıcılar için genel bir ayardır. Görüntüleme veya değiştirme ayarı bakın [Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

    *Kullanıcı uygulamaları kaydedebilir* ayarı ayarlanmalıdır **Evet** , Azure Stack kaydetmek bir kullanıcı hesabını etkinleştirmek. Uygulama kayıtları ayarı ayarlanırsa **Hayır**, bir kullanıcı hesabı kullanamaz ve Azure Stack kaydetmek için bir genel yönetici hesabı kullanmanız gerekir.

* **Bir dizi bir Azure aboneliği silinemedi:** Sahipleri grupta bulunan kullanıcılara yeterli izinlere sahip. Diğer hesapları için izin aşağıdaki bölümlerde belirtildiği gibi özel bir rol atayarak kümesini atayabilirsiniz.

## <a name="create-a-custom-role-using-powershell"></a>PowerShell kullanarak özel bir rol oluşturun

Özel bir rol oluşturmak için olmalıdır `Microsoft.Authorization/roleDefinitions/write` tüm izin `AssignableScopes`, gibi [sahibi](../role-based-access-control/built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator). Özel rol tanımlama kolaylaştırmak için aşağıdaki JSON şablonunu kullanın. Şablon gerekli okuma ve yazma erişimi için Azure Stack kayıt imkan tanıyan özel bir rol oluşturur.

1. Bir JSON dosyası oluşturun. Örneğin,  `C:\CustomRoles\registrationrole.json`
2. Aşağıdaki JSON’u dosyaya ekleyin. <SubscriptionID> öğesini Azure abonelik kimliğinizle değiştirin.

    ```json
    {
      "Name": "Azure Stack registration role",
      "Id": null,
      "IsCustom": true,
      "Description": "Allows access to register Azure Stack",
      "Actions": [
        "Microsoft.Resources/subscriptions/resourceGroups/write",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.AzureStack/registrations/*",
        "Microsoft.AzureStack/register/action",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Authorization/roleAssignments/write",
        "Microsoft.Authorization/roleAssignments/delete",
        "Microsoft.Authorization/permissions/read"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
        "/subscriptions/<SubscriptionID>"
      ]
    }
    ```

3. PowerShell'de Azure Resource Manager'ı Azure'a bağlanın. İstendiğinde, bir hesabın yeterli izinlere sahip gibi kullanarak kimlik doğrulaması [sahibi](../role-based-access-control/built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator).

    ```azurepowershell
    Connect-AzureRmAccount
    ```

4. Abonelik rolü eklemek için **New-AzureRmRoleDefinition** JSON şablon dosyası belirtme.

    ``` azurepowershell
    New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\registrationrole.json"
    ```

## <a name="assign-a-user-to-registration-role"></a>Bir kullanıcı kaydı rolüne atayın.

Kayıt özel rolü oluşturduktan sonra Azure Stack kaydetme rol kullanıcılar atayın.

1. Oturum yeterli izne sahip bir hesapla Azure aboneliği üzerinde hakları - gibi temsilci olarak [sahibi](../role-based-access-control/built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) .
2. İçinde **abonelikleri**seçin **erişim denetimi (IAM) > Rol ataması Ekle**.
3. İçinde **rol**, oluşturduğunuz özel rolü seçin *Azure Stack kayıt rolü*.
4. Role atamak istediğiniz kullanıcıları seçin.
5. Seçin **Kaydet** seçili kullanıcı rolüne atamak için.

    ![Kullanıcı rolüne atamak için seçin](media/azure-stack-registration-role/assign-role.png)

Özel roller kullanma hakkında daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Azure ile kaydedin](azure-stack-registration.md)
