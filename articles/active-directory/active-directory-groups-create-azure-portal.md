---
title: "Azure Active Directory'de kullanıcılar için bir grubu oluşturma | Microsoft Docs"
description: "Azure Active Directory'de bir grup oluşturma ve gruba üye ekleme"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d3d37761a9fdf9bd9801396d45f2fcd47efb0be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Bir grup oluşturun ve Azure Active Directory'de üye ekleme
> [!div class="op_single_selector"]
> * [Azure portal](active-directory-groups-create-azure-portal.md)
> * [Klasik Azure Portalı](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Bu makalede, oluşturma ve Azure Active Directory'de yeni bir grubu doldurmak açıklanmaktadır. Lisans veya izinleri bir kullanıcı veya cihaz sayısı için aynı anda atama gibi yönetim görevleri gerçekleştirmek için bir grup kullanın.

## <a name="how-do-i-create-a-group"></a>Nasıl grup oluşturulur?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **daha fazla hizmet**, girin **kullanıcı ve grupları** metin kutusuna ve ardından **Enter**.

   ![Açılış kullanıcı yönetimi](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **tüm grupları**.

   ![Grupları dikey penceresini açma](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Üzerinde **kullanıcılar ve gruplar - tüm grupları** dikey penceresinde, select **Ekle** komutu.

   ![Ekle komutu seçme](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Üzerinde **grup** dikey penceresinde, bir ad ve Grup açıklamasını ekleyin.
6. Gruba eklenecek üyeleri seçmek için **atanan** içinde **üyelik türü** kutusuna ve ardından **üyeleri**. Bir grubun üyeliğini dinamik olarak yönetme hakkında daha fazla bilgi için bkz: [grup üyeliği için Gelişmiş kurallar oluşturmak için öznitelikleri kullanma](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Eklenecek üyeleri seçme](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Üzerinde **üyeleri** dikey penceresinde, select bir veya daha fazla kullanıcılara veya cihazlara grubuna eklemeniz ve seçin **seçin** grubuna eklemek için dikey pencerenin altındaki düğmesini. **Kullanıcı** kutusu, bir kullanıcı veya aygıt adı herhangi bir kısmını girişe eşleşmesini temel alan görüntü filtreler. Joker karakterler bu kutuya kabul edilir.
8. Üyeleri gruba eklemeyi bitirdiğinizde, seçin **oluşturma** üzerinde **grup** dikey.    

   ![Grubu onayı oluşturma](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetmek](active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
