---
title: Öğretici - RBAC ve Azure portalını kullanarak bir kullanıcı için erişim verme | Microsoft Docs
description: Kullanıcıya Azure portalda bir rol atayarak izinler vermek için rol tabanlı erişim denetimi (RBAC) kullanın.
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
ms.date: 11/30/2018
ms.author: rolyon
ms.openlocfilehash: 8caa5c3b33ac1b483429251e0c1256636c4ece1a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634481"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-the-azure-portal"></a>Öğretici: RBAC ve Azure portalını kullanarak bir kullanıcı için erişim verin.

[Rol tabanlı erişim denetimi (RBAC)](overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Bu öğreticide, bir kullanıcı oluşturun ve bir kaynak grubundaki sanal makineleri yönetme erişimi verin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kaynak grubu kapsamındaki bir kullanıcı için erişim izni ver
> * Erişimi kaldırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. **Kaynak grubu** dikey penceresini açmak için **Ekle**'yi seçin.

   ![Yeni kaynak grubu ekleme](./media/quickstart-assign-role-user-portal/resource-group.png)

1. İçin **kaynak grubu adı**, girin **rbac-resource-group**.

1. Bir abonelik ve bir konum seçin.

1. Kaynak grubunu oluşturmak için **Oluştur**'u seçin.

1. Kaynak grupları listesini yenilemek için **Yenile**'yi seçin.

   Yeni kaynak grubu kaynak grupları listenizde görünür.

   ![Kaynak grubu listesi](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

1. Listesinde **kaynak grupları**, yeni seçin **rbac-resource-group** kaynak grubu.

1. **Erişim denetimi (IAM)** öğesini seçin.

1. Seçin **rol atamaları** rol atamaları geçerli listesini görmek için sekmesinde.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/quickstart-assign-role-user-portal/access-control.png)

1. Seçin **rol ataması Ekle** Ekle rol ataması bölmesini açmak için.

   Rol atama izinleri yoksa, rol ataması Ekle seçeneğini devre dışı bırakılır.

   ![Rol ataması bölmesi ekleme](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Aşağı açılan **Rol** listesinden **Sanal Makine Katılımcısı**'nı seçin.

1. **Seç** listesinde kendinizi ve başka bir kullanıcıyı seçin.

1. Rol atamasını oluşturmak için **Kaydet**'i seçin.

   Birkaç dakika sonra kullanıcıya rbac kaynak grubunun kaynak grubu kapsamında bir sanal makine Katılımcısı rolü atanır.

   ![Sanal Makine Katılımcısı rolü ataması](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir.

1. Rol atamalarını listesinde, sanal makine Katılımcısı rolü kullanıcıyla yanında bir onay işareti ekleyin.

1. **Kaldır**'ı seçin.

   ![Rol atamasını kaldırma iletisi](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Açılan rol atamasını kaldırma mesajında **Evet**'i seçin.

## <a name="clean-up"></a>Temizleme

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Seçin **rbac-resource-group** kaynak grubunu açın.

1. Kaynak grubunu silmek için **Kaynak grubunu sil**'i seçin.

   ![Kaynak grubunu silme](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Üzerinde **silmek istediğinizden emin misiniz** dikey penceresinde, kaynak grubu adını yazın: **rbac-resource-group**.

1. Kaynak grubunu silmek için **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve PowerShell kullanarak kullanıcıya erişim izni verme](tutorial-role-assignments-user-powershell.md)

