---
title: Öğretici - bir kullanıcı, RBAC ve Azure portalını kullanarak Azure kaynaklarına erişim | Microsoft Docs
description: Azure portalında rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına kullanıcı erişim hakkında bilgi edinin.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/22/2019
ms.author: rolyon
ms.openlocfilehash: 5786f7b48477fa705b43e3a953ac15b2c768bd71
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60344844"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>Öğretici: RBAC ve Azure portalını kullanarak Azure kaynaklarına kullanıcı erişimi

[Rol tabanlı erişim denetimi (RBAC)](overview.md) Azure kaynaklarına erişimi yönetme yoludur. Bu öğreticide, bir kullanıcı oluşturun ve bir kaynak grubundaki sanal makineleri yönetme erişimi verin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kaynak grubu kapsamındaki bir kullanıcı için erişim izni ver
> * Erişimi kaldırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. Gezinti listesinde **kaynak grupları**.

1. Tıklayın **Ekle** açmak için **kaynak grubu** dikey penceresi.

   ![Yeni kaynak grubu ekleme](./media/quickstart-assign-role-user-portal/resource-group.png)

1. İçin **kaynak grubu adı**, girin **rbac-resource-group**.

1. Bir abonelik ve bir konum seçin.

1. Tıklayın **Oluştur** kaynak grubu oluşturun.

1. Tıklayın **Yenile** kaynak grupları listesini yenileyin.

   Yeni kaynak grubu kaynak grupları listenizde görünür.

   ![Kaynak grubu listesi](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

1. Listesinde **kaynak grupları**, yeni **rbac-resource-group** kaynak grubu.

1. Tıklayın **erişim denetimi (IAM)** .

1. Tıklayın **rol atamaları** rol atamaları geçerli listesini görmek için sekmesinde.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/quickstart-assign-role-user-portal/access-control.png)

1. Tıklayın **Ekle** > **rol ataması Ekle** Ekle rol ataması bölmesini açmak için.

   Rol atama izinleri yoksa, rol ataması Ekle seçeneğini devre dışı bırakılır.

   ![Menü ekleme](./media/role-assignments-portal/add-menu.png)

   ![Rol ataması bölmesi ekleme](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Aşağı açılan **Rol** listesinden **Sanal Makine Katılımcısı**'nı seçin.

1. **Seç** listesinde kendinizi ve başka bir kullanıcıyı seçin.

1. Tıklayın **Kaydet** rol ataması oluşturmak için.

   Birkaç dakika sonra kullanıcıya rbac kaynak grubunun kaynak grubu kapsamında bir sanal makine Katılımcısı rolü atanır.

   ![Sanal Makine Katılımcısı rolü ataması](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir.

1. Rol atamalarını listesinde, sanal makine Katılımcısı rolü kullanıcıyla yanında bir onay işareti ekleyin.

1. Tıklayın **Kaldır**.

   ![Rol atamasını kaldırma iletisi](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Görünür kaldırma rol ataması iletisinde tıklayın **Evet**.

## <a name="clean-up"></a>Temizleme

1. Gezinti listesinde **kaynak grupları**.

1. Tıklayın **rbac-resource-group** kaynak grubunu açın.

1. Tıklayın **kaynak grubunu Sil** kaynak grubu silinemedi.

   ![Kaynak grubunu silme](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Üzerinde **silmek istediğinizden emin misiniz** dikey penceresinde, kaynak grubu adını yazın: **rbac-resource-group**.

1. Tıklayın **Sil** kaynak grubu silinemedi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve Azure PowerShell kullanarak Azure kaynaklarına kullanıcı erişimi](tutorial-role-assignments-user-powershell.md)

