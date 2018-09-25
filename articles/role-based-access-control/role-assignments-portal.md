---
title: RBAC ve Azure portalı kullanarak erişimi yönetme | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) ve Azure portalı kullanarak kullanıcı, grup ve uygulama erişimini yönetmeyi öğrenin. Buna erişimi listeleme, erişim verme ve erişimi kaldırma işlemleri dahildir.
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
ms.date: 09/05/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1cac4e4cee408e5208d2d5d84f81b8ad7a89f03b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034000"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>RBAC ve Azure portalı kullanarak erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Bu makalede RBAC ve Azure portalı kullanarak kullanıcı, grup ve uygulama yönetimini nasıl yapacağınız açıklanmaktadır.

## <a name="list-roles"></a>Rolleri listeleme

Rol tanımı, rol atamaları için kullandığınız izin koleksiyonudur. Azure üzerinde sahip 70 [yerleşik roller](built-in-roles.md). Rolleri Portalı'nda listelemek için aşağıdaki adımları izleyin.

1. Azure portalda **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

   ![Roller seçeneği](./media/role-assignments-portal/list-subscription-access-control.png)

1. Yerleşik ve özel rollerin listesini görmek için **Roller**'i seçin.

   ![Roller seçeneği](./media/role-assignments-portal/roles-option.png)

   Her bir role atanmış olan kullanıcı ve grup sayısını görebilirsiniz.

   ![Rolleri listesi](./media/role-assignments-portal/roles-list.png)

## <a name="list-access"></a>Erişimi listeleme

Erişim yönetimi sırasında erişim sahibi olanları, izinlerini ve izin düzeylerini bilmek istersiniz. Erişimi listelemek için rol atamalarını listelemeniz gerekir. Kullanıcılar için erişimi listelemek ve farklı kapsamlarda erişim listelemek için aşağıdaki adımları izleyin.

### <a name="list-role-assignments-for-a-user"></a>Bir kullanıcının rol atamalarını listeleme

1. Gezinti listesinde **Azure Active Directory**'yi seçin.

1. **Kullanıcılar**'ı seçerek **Tüm kullanıcılar** dikey penceresini açın.

   ![Azure Active Directory Tüm kullanıcılar dikey penceresi](./media/role-assignments-portal/aad-all-users.png)

1. Listedeki kullanıcılardan birini seçin.

1. **Yönet** bölümünde **Azure kaynakları**'nı seçin.

   ![Azure Active Directory kullanıcısı Azure kaynakları](./media/role-assignments-portal/aad-user-azure-resources.png)

   Azure kaynaklarını dikey penceresinde, seçili abonelik ve seçilen kullanıcı için rol atamalarını görebilirsiniz. Bu liste yalnızca Rol atamaları için okuma iznine sahip olduğunuz kaynakları içerir. Örneğin, kullanıcı aynı zamanda, okunamıyor rol atamaları varsa, bu rol atamaları listede görünmez.

1. Birden fazla aboneliğiniz varsa, seçebileceğiniz **abonelik** rol atamaları farklı bir abonelikte görmek için aşağı açılan listesi.

### <a name="list-role-assignments-for-a-resource-group"></a>Bir kaynak grubunun rol atamalarını listeleme

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Bir kaynak grubunu ve ardından **Erişim denetimi (IAM)** öğesini seçin.

   Kimlik ve erişim yönetimi da bilinen erişim denetimi (IAM) dikey penceresinde, bu kaynak grubu için kimlerin erişebileceğini görebilirsiniz. Bazı rollerin kapsamı **Bu kaynak** olarak belirlenmişken diğerlerinin başka bir kapsamdan **(Devralınmış)** olduğuna dikkat edin. Erişim, özellikle kaynak grubuna atanmış veya üst aboneliğe yapılan atamadan devralınmış olabilir.

   ![Kaynak grupları](./media/role-assignments-portal/resource-group-access-control.png)

### <a name="list-role-assignments-for-a-subscription"></a>Bir aboneliğin rol atamalarını listeleme

1. Azure portalda **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

    Bu abonelik ve rollerine kimlerin erişebileceğini erişim denetimi (IAM) dikey penceresinde görebilirsiniz.

    ![Erişim denetimi (IAM) dikey penceresinde bir abonelik için](./media/role-assignments-portal/subscription-access-control.png)

    Klasik abonelik yöneticileri ve ortak yöneticileri aboneliğin RBAC modelinde sahipleri olarak kabul edilir.

### <a name="list-role-assignments-for-a-management-group"></a>Bir yönetim grubu için rol atamalarını listelemek

1. Azure portalında, **tüm hizmetleri** ardından **Yönetim grupları**.

1. Yönetim grubunuzu seçin.

1. Seçin **(Ayrıntılar)** seçili yönetim grubunuz için.

    ![Yönetim grupları](./media/role-assignments-portal/management-groups-list.png)

1. **Erişim denetimi (IAM)** öğesini seçin.

    Bu yönetim grubu ve rollerine kimlerin erişebileceğini erişim denetimi (IAM) dikey penceresinde görebilirsiniz.

    ![Bir yönetim grubu için erişim denetimi (IAM) dikey](./media/role-assignments-portal/management-groups-access-control.png)

## <a name="grant-access"></a>Erişim verme

RBAC, erişim vermek için bir rol atayın. Farklı kapsamlarda erişim vermek için aşağıdaki adımları izleyin.

### <a name="assign-a-role-at-a-resource-group-scope"></a>Bir kaynak grubu kapsamında bir rol atayın

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Kaynak grubu seçin.

1. **Erişim denetimi (IAM)** bölümünde kaynak grubu kapsamındaki mevcut rol ataması listesini görebilirsiniz.

   ![Erişim denetimi (IAM) dikey penceresinde bir kaynak grubu](./media/role-assignments-portal/grant-resource-group-access-control.png)

1. **Ekle**'yi seçerek **İzin ekle** bölmesini açın.

   Rol atamak için gerekli izinlere sahip değilseniz **Ekle** seçeneği görüntülenmez.

   ![İzin ekleme bölmesi](./media/role-assignments-portal/add-permissions.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Seçin **Kaydet** rol atamak için.

   Birkaç dakika sonra güvenlik sorumlusuna kaynak grubu kapsamında rol atanmış olur.

### <a name="assign-a-role-at-a-subscription-scope"></a>Bir abonelik kapsamda bir rol atayın

1. Azure portalda **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** bölümünde abonelik kapsamındaki mevcut rol ataması listesini görebilirsiniz.

   ![Erişim denetimi (IAM) dikey penceresinde bir abonelik için](./media/role-assignments-portal/grant-subscription-access-control.png)

1. **Ekle**'yi seçerek **İzin ekle** bölmesini açın.

   Rol atamak için gerekli izinlere sahip değilseniz **Ekle** seçeneği görüntülenmez.

   ![İzin ekleme bölmesi](./media/role-assignments-portal/add-permissions.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Seçin **Kaydet** rol atamak için.

   Birkaç dakika sonra güvenlik sorumlusuna abonelik kapsamında rol atanmış olur.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Bir abonelik Yöneticisi olarak kullanıcı atama

Bir kullanıcının bir Azure aboneliğinin bir yöneticisi olmak için bunları atayın [sahibi](built-in-roles.md#owner) abonelik kapsamında bir rol. Sahip rolü, diğerleri erişim hakkı dahil olmak üzere, Abonelikteki tüm kaynaklara kullanıcı tam erişim sağlar. Bu adımları herhangi bir rol ataması ile aynıdır.

1. Azure portalda **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** bölümünde abonelik kapsamındaki mevcut rol ataması listesini görebilirsiniz.

   ![Erişim denetimi (IAM) dikey penceresinde bir abonelik için](./media/role-assignments-portal/grant-subscription-access-control.png)

1. **Ekle**'yi seçerek **İzin ekle** bölmesini açın.

   Rol atamak için gerekli izinlere sahip değilseniz **Ekle** seçeneği görüntülenmez.

   ![İzin ekleme bölmesi](./media/role-assignments-portal/add-permissions.png)

1. İçinde **rol** aşağı açılan listesinden **sahibi** rol.

1. İçinde **seçin** listesinde, bir kullanıcı seçin. Kullanıcı listede görmüyorsanız, yazabilirsiniz **seçin** kutusunu dizin görünen adları için arama yapın ve e-posta adresleri.

1. Seçin **Kaydet** rol atamak için.

   Birkaç dakika sonra kullanıcıya abonelik kapsamında bir sahip rolü atanır.

### <a name="assign-a-role-at-a-management-group-scope"></a>Bir yönetim grubu kapsamındaki rol atama

1. Azure portalında, **tüm hizmetleri** ardından **Yönetim grupları**.

1. Yönetim grubunuzu seçin.

1. Seçin **(Ayrıntılar)** seçili yönetim grubunuz için.

    ![Yönetim grupları](./media/role-assignments-portal/management-groups-list.png)

1. **Erişim denetimi (IAM)** bölümünde abonelik kapsamındaki mevcut rol ataması listesini görebilirsiniz.

   ![Bir yönetim grubu için erişim denetimi (IAM) dikey](./media/role-assignments-portal/grant-management-groups-access-control.png)

1. **Ekle**'yi seçerek **İzin ekle** bölmesini açın.

   Rol atamak için gerekli izinlere sahip değilseniz **Ekle** seçeneği görüntülenmez.

   ![İzin ekleme bölmesi](./media/role-assignments-portal/add-permissions-management-groups.png)

1. İçinde **rol** aşağı açılan listesinde, bir rol gibi seçin **yönetim grubu katkıda bulunan**.

    Yönetim grupları çeşitli rolleri için desteklenen işlemler hakkında bilgi için bkz. [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../governance/management-groups/index.md#management-group-access).

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Seçin **Kaydet** rol atamak için.

   Birkaç dakika sonra güvenlik sorumlusu yönetim grubu kapsamındaki rolü atanır.

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir. Erişimi kaldırmak için aşağıdaki adımları izleyin.

### <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

1. Açık **erişim denetimi (IAM)** kaldırmak istediğiniz yönetim grubu, abonelik, kaynak grubu veya rolü ataması kaynak dikey penceresinde.

1. Rol ataması listesinde kaldırmak istediğiniz rol atamasına sahip güvenlik sorumlusunun yanına onay işareti ekleyin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-select.png)

1. **Kaldır**'ı seçin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment.png)

1. Açılan rol atamasını kaldırma mesajında **Evet**'i seçin.

    Devralınmış rol atamaları kaldırılamaz. Devralınmış bir rol atamasını kaldırmanız gerekiyorsa bu işlemi rol atamasının oluşturulduğu kapsamda gerçekleştirmeniz gerekir. İçinde **kapsam** sütun, sonraki **(devralınmış)** burada bu rolü atandı kapsamına götüren bir bağlantı. Rol atamasını kaldırmak için orada listelenen kapsama gidin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: RBAC ve Azure portalı kullanarak bir kullanıcıya erişim izni verme](quickstart-assign-role-user-portal.md)
* [Öğretici: RBAC ve PowerShell kullanarak bir kullanıcıya erişim izni verme](tutorial-role-assignments-user-powershell.md)
* [Yerleşik roller](built-in-roles.md)
* [Kaynaklarınızı Azure Yönetim grupları ile düzenleme](../azure-resource-manager/management-groups-overview.md)
