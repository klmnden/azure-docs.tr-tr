---
title: RBAC ve Azure portalı kullanarak erişimi yönetme | Microsoft Docs
description: Kullanıcılar, gruplar, hizmet sorumluları ve rol tabanlı erişim denetimi (RBAC) ve Azure portalını kullanarak yönetilen kimlikleri için erişimi yönetmeyi öğrenin. Buna erişimi listeleme, erişim verme ve erişimi kaldırma işlemleri dahildir.
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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c339556353967db26f022384f2cf877962dc6d83
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54412317"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>RBAC ve Azure portalı kullanarak erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Bu makalede, kullanıcıları, grupları, hizmet sorumluları ve RBAC ve Azure portalını kullanarak yönetilen kimlikleri için erişimi nasıl yönetileceği açıklanmaktadır.

## <a name="open-access-control-iam"></a>Erişim denetimi (IAM) açın

**Erişim denetimi (IAM)** dikey penceresinde, kimlik ve erişim yönetimi da bilinen portalda görüntülenir. Erişim Portalı'nda yönetmek veya görüntülemek için genellikle yaptığınız ilk görüntülemek veya bir değişiklik yapmak istediğiniz kapsamında erişim denetimi (IAM) dikey penceresini açın şeydir.

1. Azure portalında **tüm hizmetleri** ve kapsam veya yönetmek veya görüntülemek istediğiniz kaynak seçin. Örneğin, seçebileceğiniz **Yönetim grupları**, **abonelikleri**, **kaynak grupları**, ya da bir kaynak.

1. Yönetmek veya görüntülemek istediğiniz belirli kaynak'ı tıklatın.

1. Tıklayın **erişim denetimi (IAM)**.

    Erişim denetimi (IAM) dikey penceresinde bir abonelik için bir örneğini gösterir.

    ![Erişim denetimi (IAM) dikey penceresinde bir abonelik için](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Rolleri ve izinleri görüntüleyin

Rol tanımı, rol atamaları için kullandığınız izin koleksiyonudur. Azure üzerinde sahip 70 [yerleşik roller](built-in-roles.md). Yönetim ve veri düzlemi gerçekleştirilebilir izinleri ve rolleri görüntülemek için aşağıdaki adımları izleyin.

1. Açık **erişim denetimi (IAM)** rolleri ve izinleri görüntülemek istediğiniz bir kapsamda, yönetim grubu, abonelik, kaynak grubu ya da kaynağa, gibi.

1. Tıklayın **rolleri** tüm yerleşik ve özel rollerin bir listesini görmek için sekmesinde.

   Kullanıcılar ve gruplar bu kapsamda her role atanmış olan sayısını görebilirsiniz.

   ![Rolleri listesi](./media/role-assignments-portal/roles-list.png)

1. Kimin bu role atanmış olan görmek için ayrı bir rol tıklayın ve rol izinlerini de görüntüleyebilirsiniz.

   ![Rol atamaları](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Rol atamalarını görüntüle

Erişim yönetimi sırasında erişim sahibi olanları, izinlerini ve izin düzeylerini bilmek istersiniz. Liste erişimi bir kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik için rol atamalarını görüntüleyin.

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

1. Tıklayın **rol atamaları** sekme (veya **görünümü** rol atamaları kutucuğu görüntüle düğmesine) tüm rol atamalarını bu kapsamda görüntülemek için.

   ![Erişim denetimi - rol atamalarını sekmesi](./media/role-assignments-portal/access-control-role-assignments.png)

   Rol atamaları sekmesinde bu kapsamda kimlerin erişimi olduğunu görebilirsiniz. Bazı rollerin kapsamı **Bu kaynak** olarak belirlenmişken diğerlerinin başka bir kapsamdan **(Devralınmış)** olduğuna dikkat edin. Erişim özellikle bu kaynağa atanmış veya üst kapsama bir atamadan devralınmış.

## <a name="add-a-role-assignment"></a>Rol ataması ekleme

RBAC, erişim vermek için bir rol bir kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik atayın. Farklı kapsamlarda erişim vermek için aşağıdaki adımları izleyin.

### <a name="assign-a-role-at-a-scope"></a>Bir kapsamda bir rol atayın

1. Açık **erişim denetimi (IAM)** erişim vermek istediğiniz bir kapsamda, yönetim grubu, abonelik, kaynak grubu ya da kaynağa, gibi.

1. Tıklayın **rol atamaları** tüm rol atamalarını bu kapsamda görüntülemek için sekmesinde.

1. Tıklayın **rol ataması Ekle** Ekle rol ataması bölmesini açmak için.

   Rol atama izinleri yoksa, rol ataması Ekle seçeneğini devre dışı bırakılır.

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

1. Tıklayın **rol ataması Ekle** Ekle rol ataması bölmesini açmak için.

   Rol atama izinleri yoksa, rol ataması Ekle seçeneğini devre dışı bırakılır.

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

* [Öğretici: RBAC ve Azure portalını kullanarak bir kullanıcı için erişim izni ver](quickstart-assign-role-user-portal.md)
* [Öğretici: RBAC ve Azure PowerShell kullanarak bir kullanıcı için erişim izni ver](tutorial-role-assignments-user-powershell.md)
* [Azure RBAC sorunlarını giderme](troubleshooting.md)
* [Kaynaklarınızı Azure Yönetim grupları ile düzenleme](../azure-resource-manager/management-groups-overview.md)
