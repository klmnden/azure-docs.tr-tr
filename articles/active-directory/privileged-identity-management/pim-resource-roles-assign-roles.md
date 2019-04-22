---
title: Azure kaynağı rolleri PIM - Azure Active Directory atama | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri atama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 610aeec9e4c40d0aad0c28f02697e2cf01edbe4a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59494014"
---
# <a name="assign-azure-resource-roles-in-pim"></a>PIM Azure kaynak Rolleri Ata

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) (ancak bunlarla sınırlı olmayan) özel roller yanı sıra yerleşik Azure kaynağı rolleri yönetebilirsiniz:

- Sahip
- Kullanıcı Erişimi Yöneticisi
- Katılımcı
- Güvenlik Yöneticisi
- Güvenlik Yöneticisi ve daha fazlası

> [!NOTE]
> Kullanıcılar veya sahibi ya da kullanıcı erişimi yöneticisi rolü ve Azure AD'de abonelik yönetimini etkinleştirme genel Yöneticiler atanmış bir grubun üyelerinin kaynağa yöneticilerdir. Bu yöneticileri, Rolleri Ata, rol ayarlarını yapılandırma ve Azure kaynakları için PIM kullanarak erişimi gözden geçirin. Listesini görüntülemek [Azure kaynakları için yerleşik roller](../../role-based-access-control/built-in-roles.md).

## <a name="assign-a-role"></a>Rol atama

Bir kullanıcı bir Azure kaynak rol için uygun hale getirmek için aşağıdaki adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

    PIM yönetmek için başka bir yönetici erişim verme konusunda daha fazla bilgi için bkz: [PIM yönetmek için diğer yöneticilere erişim ver](pim-how-to-give-access-to-pim.md).

1. Açık **Azure AD Privileged Identity Management**.

    Azure portalında PIM henüz başlamamış, Git [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **Azure kaynaklarını**.

1. Kullanım **kaynak filtresi** yönetilen kaynaklar listesine filtre uygulamak için.

    ![Azure kaynaklarını yönetmek için listesi](./media/pim-resource-roles-assign-roles/resources-list.png)

1. Bir abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağa tıklayın.

1. Yönet altında **rolleri** Azure kaynakları için rolleri listesini görmek için.

    ![Azure kaynak rolleri](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. Tıklayın **Üye Ekle** yeni atama bölmesini açmak için.

1. Tıklayın **bir rol seçin** seçin, bir rol bölmesini açmak için.

    ![Yeni atama bölmesi](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. Bir rol atayın ve ardından istediğiniz tıklayın **seçin**.

    Select üyelerinin veya grubun bölmesinde açılır.

1. Bir üye veya role atamak ve ardından istediğiniz grubu **seçin**.

    ![Bir üye veya grup bölmesinde seçin](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

    Üyelik ayarları bölmesi açılır.

1. İçinde **atama türü** listesinden **uygun** veya **etkin**.

    ![Üyeliği ayarları bölmesi](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    Azure kaynakları için PIM iki ayrı bir atama türü sağlar:

    - **Uygun** rolü kullanmak için bir eylem gerçekleştirmek rolünün üyesi atamalarını gerektirir. Bir iş gerekçesi sağlamak veya belirlenmiş onaylayanlar onayı isteyen bir çok faktörlü kimlik doğrulaması (MFA) denetimi gerçekleştirme işlemleri içerebilir.

    - **Etkin** atamaları yok rolünü kullanmak için herhangi bir eylemi gerçekleştirmek üye gerektirir. Etkin olarak atanan üyeleri, her zaman rolüne atanan ayrıcalıklara sahiptir.

1. Atama kalıcı olması gerekiyorsa bunu seçin (kalıcı olarak uygun veya kalıcı olarak atanan) **kalıcı olarak** onay kutusu.

    Rol ayarlarına bağlı olarak onay kutusu görünmeyebilir veya değiştirilemeyen olabilir.

1. Belirli atama süresi belirtmek için onay kutusunu temizleyin ve başlangıç ve/veya bitiş tarihi ve saati kutularını değiştirin.

    ![Üyeliği ayarları - tarih ve saat](./media/pim-resource-roles-assign-roles/resources-membership-settings-date.png)

1. Tamamladığınızda **Bitti**’ye tıklayın.

    ![Yeni atama - ekleyin](./media/pim-resource-roles-assign-roles/resources-new-assignment-add.png)

1. Yeni rol ataması oluşturmak için tıklayın **Ekle**. Bir uyarı durumu görüntülenir.

    ![Yeni atama - bildirimi](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Güncelleştirme veya mevcut bir rolü atamasını kaldırma

Güncelleştirme veya mevcut bir rol atamasını kaldırmak için aşağıdaki adımları izleyin.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Bir abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağa tıklayın.

1. Yönet altında **rolleri** Azure kaynakları için rolleri listesini görmek için.

    ![Azure kaynağı rolleri - rol seçme](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. Güncelleştirmek veya kaldırmak istediğiniz rol tıklayın.

1. Rol ataması bulmak **uygun roller** veya **etkin rollerin** sekmeler.

    ![Güncelleştirme veya rol atamasını kaldırma](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. Tıklayın **güncelleştirme** veya **Kaldır** güncelleştirmek veya rol atamasını kaldırmak için.

    Bir rol atamasını genişletme hakkında daha fazla bilgi için bkz: [genişletme veya Azure kaynağı rolleri PIM](pim-resource-roles-renew-extend.md).

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri genişletmek veya yenileme](pim-resource-roles-renew-extend.md)
- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Azure AD PIM Rolleri Ata](pim-how-to-add-role-to-user.md)
