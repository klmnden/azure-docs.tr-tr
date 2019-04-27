---
title: RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme | Microsoft Docs
description: Kullanıcılar, gruplar, hizmet sorumluları ve rol tabanlı erişim denetimi (RBAC) ve Azure portalını kullanarak yönetilen kimlikleri için Azure kaynaklarına erişimini yönetmeyi öğrenin. Buna erişimi listeleme, erişim verme ve erişimi kaldırma işlemleri dahildir.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/24/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: bb23cbc275e01eab5361504c547c020b0a29f4c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60533170"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) Azure kaynaklarına erişimi yönetme yoludur. Bu makalede, erişim Azure portalı kullanılarak nasıl yönetileceği açıklanmaktadır. Azure Active Directory erişimi yönetmek ihtiyacınız varsa bkz [Azure Active Directory'de yönetici rolleri görüntüleyin ve Ata](../active-directory/users-groups-roles/directory-manage-roles-portal.md).

## <a name="prerequisites"></a>Önkoşullar

Ekleme ve rol atamalarını kaldırmak için şunlara sahip olmalısınız:

- `Microsoft.Authorization/roleAssignments/write` ve `Microsoft.Authorization/roleAssignments/delete` izinleri gibi [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner)

## <a name="overview-of-access-control-iam"></a>Erişim denetimi (IAM) genel bakış

**Erişim denetimi (IAM)** Azure kaynaklarına erişimi yönetmek için kullandığınız dikey penceredir. Kimlik ve erişim yönetimi olarak da bilinen olduğundan ve Azure portalında çeşitli konumlarda görünür. Erişim denetimi (IAM) dikey penceresinde bir abonelik için bir örneğini gösterir.

![Erişim denetimi (IAM) dikey penceresinde bir abonelik için](./media/role-assignments-portal/access-control-numbers.png)

Aşağıdaki tabloda açıklanmıştır kullanım için öğelerin bazıları şunlardır:

| # | Öğe | İçin kullandığınız kadarı |
| --- | --- | --- |
| 1 | Erişim denetimi (IAM) burada açık kaynak | Kapsam (Bu örnekte aboneliği) tanımlayın |
| 2 | **Ekleme** düğmesi | Rol ataması Ekle |
| 3 | **Erişim denetimi** sekmesi | Tek bir kullanıcı için rol atamalarını görüntüleme |
| 4 | **Rol atamaları** sekmesi | Geçerli kapsamda rol atamalarını görüntüleme |
| 5 | **Rolleri** sekmesi | Tüm rolleri ve izinleri görüntüleme |

Erişimi yönetmek çalışırken şu üç soruları yanıtlamak erişim denetimi (IAM) dikey penceresinde ile en etkili olması için bunu yardımcı olur:

1. **Kimin erişmesi gerekiyor?**

    Kimin bir kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik ifade eder. Bu da adlandırılır bir *güvenlik sorumlusu*.

1. **Hangi izinlerin bunlar gerekiyor mu?**

    İzinleri rollere birlikte gruplandırılır. Birkaç yerleşik rol listesinden seçebilirsiniz.

1. **Burada erişim gerekiyor mu?**

    Burada erişim için geçerli bir kaynak kümesini ifade eder. Burada bir yönetim grubuna, aboneliğe, kaynak grubuna ya da bir depolama hesabı gibi tek bir kaynak olabilir. Bu adlandırılır *kapsam*.

## <a name="open-access-control-iam"></a>Erişim denetimi (IAM) açın

Karar vermek için gereken ilk şey, erişim denetimi (IAM) dikey penceresini açmak yerdir. Bu, istediğiniz hangi kaynaklara erişimi yönetmek üzerinde bağlıdır. Her şey bir yönetim grubu, bir abonelikte bir kaynak grubu veya tek bir kaynak her şey her şey için erişimi yönetmek istiyor musunuz?

1. Azure portalında **tüm hizmetleri** ve kapsamı seçin. Örneğin, seçebileceğiniz **Yönetim grupları**, **abonelikleri**, **kaynak grupları**, ya da bir kaynak.

1. Belirli kaynak'ı tıklatın.

1. Tıklayın **erişim denetimi (IAM)**.

    Erişim denetimi (IAM) dikey penceresinde bir abonelik için bir örneğini gösterir. Tüm erişim denetimi değişiklikler yaparsanız, bunlar tüm aboneliğiniz için geçerli.

    ![Erişim denetimi (IAM) dikey penceresinde bir abonelik için](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Rolleri ve izinleri görüntüleyin

Rol tanımı, rol atamaları için kullandığınız izin koleksiyonudur. Azure üzerinde sahip 70 [Azure kaynakları için yerleşik roller](built-in-roles.md). Kullanılabilir roller ve izinler görüntülemek için aşağıdaki adımları izleyin.

1. Açık **erişim denetimi (IAM)** herhangi bir kapsamda.

1. Tıklayın **rolleri** tüm yerleşik ve özel rollerin bir listesini görmek için sekmesinde.

   Kullanıcıların ve grupların geçerli kapsamda her role atanmış sayısını görebilirsiniz.

   ![Rolleri listesi](./media/role-assignments-portal/roles-list.png)

1. Kimin bu role atanmış olan görmek için ayrı bir rol tıklayın ve rol izinlerini de görüntüleyebilirsiniz.

   ![Rol atamaları](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Rol atamalarını görüntüle

Erişim yönetirken oldukları izinleri kimin erişimi olduğunu bilmek istiyorsunuz ve hangi kapsamda. Liste erişimi bir kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik için rol atamalarını görüntüleyin.

### <a name="view-role-assignments-for-a-single-user"></a>Tek bir kullanıcı için rol atamalarını görüntüle

Tek bir kullanıcı, Grup, hizmet sorumlusu veya belirli bir kapsamda yönetilen kimlik erişimini görüntülemek için aşağıdaki adımları izleyin.

1. Açık **erişim denetimi (IAM)** erişim görüntülemek istediğiniz bir kapsamda, yönetim grubu, abonelik, kaynak grubu ya da kaynağa, gibi.

1. Tıklayın **denetleyin erişim** sekmesi.

    ![Erişim denetimi - onay erişim sekmesi](./media/role-assignments-portal/access-control-check-access.png)

1. İçinde **Bul** listesinde, istediğiniz erişimi denetlemek için güvenlik sorumlusu türü seçin.

1. Arama kutusunda görünen adları, e-posta adresi veya nesne tanımlayıcıları dizinini aramak için bir dize girin.

    ![Seçim listesine erişimi denetleyin](./media/role-assignments-portal/check-access-select.png)

1. Açmak için güvenlik sorumlusunu **atamaları** bölmesi.

    ![Atamalar bölmesinin](./media/role-assignments-portal/check-access-assignments.png)

    Bu bölmede, seçili güvenlik sorumlusu ve kapsamına atanan rollerini görebilirsiniz. Varsa tüm atamalarını bu kapsamda reddetme ya da bu kapsama devralınan, bunlar listelenir.

### <a name="view-all-role-assignments-at-a-scope"></a>Bir kapsamda tüm rol atamalarını görüntüleme

1. Açık **erişim denetimi (IAM)** erişim görüntülemek istediğiniz bir kapsamda, yönetim grubu, abonelik, kaynak grubu ya da kaynağa, gibi.

1. Tıklayın **rol atamaları** tüm rol atamalarını bu kapsamda görüntülemek için sekmesinde.

   ![Erişim denetimi - rol atamalarını sekmesi](./media/role-assignments-portal/access-control-role-assignments.png)

   Rol atamaları sekmesinde bu kapsamda kimlerin erişimi olduğunu görebilirsiniz. Bazı rollerin kapsamı **Bu kaynak** olarak belirlenmişken diğerlerinin başka bir kapsamdan **(Devralınmış)** olduğuna dikkat edin. Erişim özellikle bu kaynağa atanmış veya üst kapsama bir atamadan devralınmış.

## <a name="add-a-role-assignment"></a>Rol ataması ekleme

RBAC, erişim vermek için bir rol bir kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik atayın. Farklı kapsamlarda erişim vermek için aşağıdaki adımları izleyin.

### <a name="assign-a-role-at-a-scope"></a>Bir kapsamda bir rol atayın

1. Açık **erişim denetimi (IAM)** erişim vermek istediğiniz bir kapsamda, yönetim grubu, abonelik, kaynak grubu ya da kaynağa, gibi.

1. Tıklayın **rol atamaları** tüm rol atamalarını bu kapsamda görüntülemek için sekmesinde.

1. Tıklayın **Ekle** > **rol ataması Ekle** Ekle rol ataması bölmesini açmak için.

   Rol atama izinleri yoksa, rol ataması Ekle seçeneğini devre dışı bırakılır.

   ![Menü ekleme](./media/role-assignments-portal/add-menu.png)

   ![Rol ataması bölmesi ekleme](./media/role-assignments-portal/add-role-assignment.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. İçinde **seçin** listesinde, bir kullanıcı, Grup, hizmet sorumlusu veya yönetilen bir kimlik seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Tıklayın **Kaydet** rol atamak için.

   Birkaç dakika sonra güvenlik sorumlusu seçilen kapsamda rolü atanır.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Bir abonelik Yöneticisi olarak kullanıcı atama

Bir kullanıcının bir Azure aboneliğinin bir yöneticisi olmak için bunları atayın [sahibi](built-in-roles.md#owner) abonelik kapsamında bir rol. Sahip rolü, diğerleri erişim hakkı dahil olmak üzere, Abonelikteki tüm kaynaklara kullanıcı tam erişim sağlar. Bu adımları herhangi bir rol ataması ile aynıdır.

1. Azure portalında **tüm hizmetleri** ardından **abonelikleri**.

1. Erişim vermek istediğiniz aboneliğe tıklayın.

1. Tıklayın **erişim denetimi (IAM)**.

1. Tıklayın **rol atamaları** Bu abonelik için tüm rol atamalarını görüntülemek için sekmesinde.

1. Tıklayın **Ekle** > **rol ataması Ekle** Ekle rol ataması bölmesini açmak için.

   Rol atama izinleri yoksa, rol ataması Ekle seçeneğini devre dışı bırakılır.

   ![Menü ekleme](./media/role-assignments-portal/add-menu.png)

   ![Rol ataması bölmesi ekleme](./media/role-assignments-portal/add-role-assignment.png)

1. İçinde **rol** aşağı açılan listesinden **sahibi** rol.

1. İçinde **seçin** listesinde, bir kullanıcı seçin. Kullanıcı listede görmüyorsanız, yazabilirsiniz **seçin** kutusunu dizin görünen adları için arama yapın ve e-posta adresleri.

1. Tıklayın **Kaydet** rol atamak için.

   Birkaç dakika sonra kullanıcıya abonelik kapsamında bir sahip rolü atanır.

## <a name="remove-role-assignments"></a>Rol atamalarını kaldırın

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir. Erişimi kaldırmak için aşağıdaki adımları izleyin.

1. Açık **erişim denetimi (IAM)** erişimini kaldırmak istediğiniz bir kapsamda, yönetim grubu, abonelik, kaynak grubu ya da kaynağa, gibi.

1. Tıklayın **rol atamaları** Bu abonelik için tüm rol atamalarını görüntülemek için sekmesinde.

1. Rol ataması listesinde kaldırmak istediğiniz rol atamasına sahip güvenlik sorumlusunun yanına onay işareti ekleyin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Tıklayın **Kaldır**.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment.png)

1. Görünür kaldırma rol ataması iletisinde tıklayın **Evet**.

    Devralınmış rol atamaları kaldırılamaz. Devralınmış bir rol atamasını kaldırmanız gerekiyorsa bu işlemi rol atamasının oluşturulduğu kapsamda gerçekleştirmeniz gerekir. İçinde **kapsam** sütun, sonraki **(devralınmış)** burada bu rolü atandı kapsamına götüren bir bağlantı. Rol atamasını kaldırmak için orada listelenen kapsama gidin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: RBAC ve Azure portalını kullanarak Azure kaynaklarına kullanıcı erişimi](quickstart-assign-role-user-portal.md)
* [Öğretici: RBAC ve Azure PowerShell kullanarak Azure kaynaklarına kullanıcı erişimi](tutorial-role-assignments-user-powershell.md)
* [RBAC, Azure kaynakları için sorun giderme](troubleshooting.md)
* [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../governance/management-groups/index.md)
