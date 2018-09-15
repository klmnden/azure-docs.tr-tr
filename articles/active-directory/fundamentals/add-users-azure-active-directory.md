---
title: Ekleme veya Azure Active Directory'de kullanıcıları silmek | Microsoft Docs
description: Yeni kullanıcı ekleme ya da Azure Active Directory kullanarak mevcut kullanıcıları silmek öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 533c02f842918c5c27aaaf421a4f828022de6b8f
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604983"
---
# <a name="how-to-add-or-delete-users-using-azure-active-directory"></a>Nasıl yapılır: ekleme veya Azure Active Directory kullanarak kullanıcı silme
Yeni kullanıcı ekleme veya var olan kullanıcılar, Azure AD kullanarak, Azure Active Directory (Azure AD) kiracısı silin.

## <a name="add-a-new-user"></a>Yeni kullanıcı ekleme
Azure Active Directory'yi kullanarak yeni bir kullanıcı oluşturabilir.

### <a name="to-add-a-new-user"></a>Yeni bir kullanıcı eklemek için
1. Oturum [Azure portalında](https://portal.azure.com/) bir genel yönetici veya kullanıcı dizinin Yöneticisi olarak.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **yeni kullanıcı**.

    ![Kullanıcılar - yeni kullanıcı ile vurgulanmış tüm kullanıcılar sayfası](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. Üzerinde **kullanıcı** sayfasında, gerekli bilgileri doldurun.

    ![Yeni kullanıcı, kullanıcı bilgileri kullanıcının Sayfası Ekle](media/add-users-azure-active-directory/new-user-user-blade.png)

    - **Ad (gerekli).** Yeni kullanıcı ilk ve son adı. Örneğin, Gamze Parker.

    - **Kullanıcı adı (gerekli).** Yeni kullanıcının kullanıcı adı. Örneğin, mary@contoso.com. 
    
        Kullanıcı adının etki alanı bölümünü ya da ilk varsayılan etki alanı adı kullanmanız gerekir <_etkialanıadınız_>. onmicrosoft.com ya da bir özel etki alanı adı contoso.com gibi. Özel etki alanı adı oluşturma hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye özel etki alanı adı ekleme](add-custom-domain.md).

    - **Profili.** İsteğe bağlı olarak, kullanıcı hakkında daha fazla bilgi ekleyebilirsiniz. Daha sonraki bir zamanda kullanıcı bilgileri de ekleyebilirsiniz. Kullanıcı bilgileri ekleme hakkında daha fazla bilgi için bkz. [eklemek veya kullanıcı profili bilgilerini değiştirmek nasıl](active-directory-users-profile-azure-portal.md).

    - **Gruplar.** İsteğe bağlı olarak, bir veya daha fazla mevcut gruplara kullanıcı ekleyebilirsiniz. Ayrıca, kullanıcı gruplarına daha sonra ekleyebilirsiniz. Kullanıcı grupları ekleme hakkında daha fazla bilgi için bkz. [temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

    - **Dizin rolü.** İsteğe bağlı olarak, bir dizin rolüne kullanıcı ekleyebilirsiniz. Bir genel yönetici kullanıcı atayabilirsiniz veya bir veya daha fazla Azure AD'de başka yönetici rolleri. Rol atama hakkında daha fazla bilgi için bkz. [kullanıcılara roller atama](active-directory-users-assign-role-azure-portal.md).

4. Sağlanan otomatik olarak oluşturulan parola kopyalama **parola** kutusu. İlk oturum açma işlemi için bu parolayı kullanıcıya vermeniz gerekir.

5. **Oluştur**’u seçin.

    Kullanıcı oluşturulur ve Azure AD kiracınıza eklenir.

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Bir karma ortamda yeni bir kullanıcı ekleyin
Azure Active Directory (bulut) hem de Windows Server Active Directory (şirket) ile bir ortamınız varsa, var olan kullanıcı hesabı verileri eşitleyerek yeni kullanıcıları ekleyebilirsiniz. Karma ortamlar ve kullanıcılar hakkında daha fazla bilgi için bkz. [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md).

## <a name="delete-a-user"></a>Kullanıcı silme
Mevcut bir kullanıcının Azure Active Directory'yi kullanarak silebilirsiniz.

### <a name="to-delete-a-user"></a>Kullanıcı silme
1. Oturum [Azure portalında](https://portal.azure.com/) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**, arayın ve Azure AD kiracınızdan silmek istediğiniz kullanıcıyı seçin. Örneğin, _Mary Parker_.

3. Seçin **kullanıcıyı silme**.

    ![Kullanıcılar - tüm kullanıcılar sayfası ile vurgulanmış kullanıcıyı silme](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    Kullanıcı silindi ve artık görünür **kullanıcılar - tüm kullanıcılar** sayfası. Kullanıcı görülebilir **silinmiş kullanıcılarını** sayfa sonraki 30 gün boyunca ve bu süre içerisinde geri yüklenebilir. Bir kullanıcıyı geri yükleme hakkında daha fazla bilgi için bkz. [geri yüklemek veya yakın zamanda silinen bir kullanıcıyı kalıcı olarak kaldırmak nasıl](active-directory-users-restore.md).

    >[!Note]
    >Windows Server Active Directory, kimlik, iletişim bilgileri veya Windows Server Active Directory, yetki kaynağı olan kullanıcılar için iş bilgileri güncelleştirmek için kullanmanız gerekir. Güncelleştirme tamamlandıktan sonra değişiklikleri görürsünüz önce tamamlanması için bir sonraki eşitleme döngüsü beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcılarınızın ekledikten sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Kullanıcılara roller atama](active-directory-users-assign-role-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Dinamik gruplar ve kullanıcılar ile çalışma](../users-groups-roles/groups-create-rule.md)

Veya gibi diğer kullanıcı yönetim görevlerini gerçekleştirebilirsiniz [başka bir dizinden Konuk kullanıcıları ekleme](../b2b/what-is-b2b.md) veya [silinmiş bir kullanıcıyı geri yükleme](active-directory-users-restore.md). Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).