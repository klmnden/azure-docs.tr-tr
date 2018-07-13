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
ms.date: 07/11/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: aaa36d850516ff4d8e40b62c588347468da5c6d2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008170"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>RBAC ve Azure portalı kullanarak erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Bu makalede RBAC ve Azure portalı kullanarak kullanıcı, grup ve uygulama yönetimini nasıl yapacağınız açıklanmaktadır.

## <a name="list-roles"></a>Rolleri listeleme

Rol tanımı, rol atamaları için kullandığınız izin koleksiyonudur. Azure'da 60'ın üzerinde [yerleşik rol](built-in-roles.md) vardır.

1. Azure portalda **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

   ![Roller seçeneği](./media/role-assignments-portal/list-subscription-access-control.png)

1. Yerleşik ve özel rollerin listesini görmek için **Roller**'i seçin.

   ![Roller seçeneği](./media/role-assignments-portal/roles-option.png)

   Her bir role atanmış olan kullanıcı ve grup sayısını görebilirsiniz.

   ![Rolleri listesi](./media/role-assignments-portal/roles-list.png)

## <a name="list-access"></a>Erişimi listeleme

Erişim yönetimi sırasında erişim sahibi olanları, izinlerini ve izin düzeylerini bilmek istersiniz. Erişimi listelemek için rol atamalarını listelemeniz gerekir.

### <a name="list-role-assignments-for-a-subscription"></a>Bir aboneliğin rol atamalarını listeleme

1. Azure portalda **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

    Kimlik ve erişim yönetimi de olarak bilinen Erişim denetimi (IAM) dikey penceresinde bu aboneliğe erişime sahip olanları ve rollerini görebilirsiniz.

    ![Erişim denetimi (IAM) dikey penceresi](./media/role-assignments-portal/subscription-access-control.png)

    RBAC modelinde, klasik abonelik yöneticileri ve ortak yöneticileri aboneliğin sahipleri olarak kabul edilir.


### <a name="list-role-assignments-for-a-resource-group"></a>Bir kaynak grubunun rol atamalarını listeleme

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Bir kaynak grubunu ve ardından **Erişim denetimi (IAM)** öğesini seçin.

   Erişim denetimi (IAM) dikey penceresinde bu kaynak grubuna erişimi olan kullanıcıları görebilirsiniz. Bazı rollerin kapsamı **Bu kaynak** olarak belirlenmişken diğerlerinin başka bir kapsamdan **(Devralınmış)** olduğuna dikkat edin. Erişim, özellikle kaynak grubuna atanmış veya üst aboneliğe yapılan atamadan devralınmış olabilir.

   ![Kaynak grupları](./media/role-assignments-portal/resource-group-access-control.png)

### <a name="list-role-assignments-for-a-user"></a>Bir kullanıcının rol atamalarını listeleme

1. Gezinti listesinde **Azure Active Directory**'yi seçin.

1. **Kullanıcılar**'ı seçerek **Tüm kullanıcılar** dikey penceresini açın.

   ![Azure Active Directory Tüm kullanıcılar dikey penceresi](./media/role-assignments-portal/aad-all-users.png)

1. Listedeki kullanıcılardan birini seçin.

1. **Yönet** bölümünde **Azure kaynakları**'nı seçin.

   ![Azure Active Directory kullanıcısı Azure kaynakları](./media/role-assignments-portal/aad-user-azure-resources.png)

   Azure kaynaklarını dikey penceresinde, seçili abonelik ve seçilen kullanıcı için rol atamalarını görebilirsiniz. Bu liste yalnızca Rol atamaları için okuma iznine sahip olduğunuz kaynakları içerir. Örneğin, kullanıcı aynı zamanda, okunamıyor rol atamaları varsa, bu rol atamaları listede görünmez.

1. Birden fazla aboneliğiniz varsa, seçebileceğiniz **abonelik** rol atamaları farklı bir abonelikte görmek için aşağı açılan listesi.

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

### <a name="create-a-role-assignment-at-a-subscription-scope"></a>Abonelik kapsamında rol ataması oluşturma

1. Azure portalda **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** bölümünde abonelik kapsamındaki mevcut rol ataması listesini görebilirsiniz.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/role-assignments-portal/grant-subscription-access-control.png)

1. **Ekle**'yi seçerek **İzin ekle** bölmesini açın.

   Rol atamak için gerekli izinlere sahip değilseniz **Ekle** seçeneği görüntülenmez.

   ![İzin ekleme bölmesi](./media/role-assignments-portal/add-permissions.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rol atamasını oluşturmak için **Kaydet**'i seçin.

   Birkaç dakika sonra güvenlik sorumlusuna abonelik kapsamında rol atanmış olur.

### <a name="create-a-role-assignment-at-a-resource-group-scope"></a>Kaynak grubu kapsamında rol ataması oluşturma

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Kaynak grubu seçin.

1. **Erişim denetimi (IAM)** bölümünde kaynak grubu kapsamındaki mevcut rol ataması listesini görebilirsiniz.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/role-assignments-portal/grant-resource-group-access-control.png)

1. **Ekle**'yi seçerek **İzin ekle** bölmesini açın.

   Rol atamak için gerekli izinlere sahip değilseniz **Ekle** seçeneği görüntülenmez.

   ![İzin ekleme bölmesi](./media/role-assignments-portal/add-permissions.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rol atamasını oluşturmak için **Kaydet**'i seçin.

   Birkaç dakika sonra güvenlik sorumlusuna kaynak grubu kapsamında rol atanmış olur.

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir.

### <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

1. Kaldırmak istediğiniz rol atamasına sahip olan aboneliğin, kaynak grubunun veya kaynağın **Erişim denetimi (IAM)** dikey penceresini açın.

1. Rol ataması listesinde kaldırmak istediğiniz rol atamasına sahip güvenlik sorumlusunun yanına onay işareti ekleyin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-select.png)

1. **Kaldır**'ı seçin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment.png)

1. Açılan rol atamasını kaldırma mesajında **Evet**'i seçin.

Devralınmış rol atamaları kaldırılamaz. Devralınmış bir rol atamasını kaldırmanız gerekiyorsa bu işlemi rol atamasının oluşturulduğu kapsamda gerçekleştirmeniz gerekir. **Kapsam** sütunundaki **Devralınmış** seçeneğinin yanında, sizi rolün oluşturulduğu kaynaklara götüren bir bağlantı yer alır. Rol atamasını kaldırmak için orada listelenen kapsama gidin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: RBAC ve Azure portalı kullanarak bir kullanıcıya erişim izni verme](quickstart-assign-role-user-portal.md)
* [Öğretici: RBAC ve PowerShell kullanarak bir kullanıcıya erişim izni verme](tutorial-role-assignments-user-powershell.md)
* [Yerleşik roller](built-in-roles.md)
