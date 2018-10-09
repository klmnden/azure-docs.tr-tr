---
title: Hızlı Başlangıç - RBAC ve Azure portalı kullanarak kullanıcıya erişim izni verme | Microsoft Docs
description: Kullanıcıya Azure portalda bir rol atayarak izinler vermek için rol tabanlı erişim denetimi (RBAC) kullanın.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/11/2018
ms.author: rolyon
ms.openlocfilehash: 74ecca671409b6e163bc0db29d66167d240b645c
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092531"
---
# <a name="quickstart-grant-access-for-a-user-using-rbac-and-the-azure-portal"></a>Hızlı Başlangıç: RBAC ve Azure portalı kullanarak kullanıcıya erişim izni verme

Rol tabanlı erişim denetimi (RBAC) Azure'da kaynaklara erişimi yönetmek için kullanılan yöntemdir. Bu hızlı başlangıçta, bir kaynak grubunda sanal makineler oluşturması ve bunları yönetmesi için bir kullanıcıya erişim vereceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. **Kaynak grubu** dikey penceresini açmak için **Ekle**'yi seçin.

   ![Yeni kaynak grubu ekleme](./media/quickstart-assign-role-user-portal/resource-group.png)

1. **Kaynak grubu adı** olarak **rbac-quickstart-resource-group** ifadesini girin.

1. Bir abonelik ve bir konum seçin.

1. Kaynak grubunu oluşturmak için **Oluştur**'u seçin.

1. Kaynak grupları listesini yenilemek için **Yenile**'yi seçin.

   Yeni kaynak grubu kaynak grupları listenizde görünür.

   ![Kaynak grubu listesi](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

1. **Kaynak grupları** listesinde yeni **rbac-quickstart-resource-group** kaynak grubunu seçin.

1. Geçerli rol atamaları listesini görmek için **Erişim denetimi (IAM)** öğesini seçin.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/quickstart-assign-role-user-portal/access-control.png)

1. **Ekle**'yi seçerek **İzin ekle** bölmesini açın.

   Rol atamak için gerekli izinlere sahip değilseniz **Ekle** seçeneği görüntülenmez.

   ![İzin ekleme bölmesi](./media/quickstart-assign-role-user-portal/add-permissions.png)

1. Aşağı açılan **Rol** listesinden **Sanal Makine Katılımcısı**'nı seçin.

1. **Seç** listesinde kendinizi ve başka bir kullanıcıyı seçin.

1. Rol atamasını oluşturmak için **Kaydet**'i seçin.

   Birkaç saniye sonra kullanıcıya rbac-quickstart-resource-group kaynak grubu kapsamında Sanal Makine Katılımcısı rolü atanır.

   ![Sanal Makine Katılımcısı rolü ataması](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir.

1. Rol atamaları listesinde, rolü Sanal Makine Katılımcısı olan kullanıcının yanına bir onay işareti koyun.

1. **Kaldır**'ı seçin.

   ![Rol atamasını kaldırma iletisi](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Açılan rol atamasını kaldırma mesajında **Evet**'i seçin.

## <a name="clean-up"></a>Temizleme

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. Kaynak grubunu açmak için **rbac-quickstart-resource-group** seçeneğini belirleyin.

1. Kaynak grubunu silmek için **Kaynak grubunu sil**'i seçin.

   ![Kaynak grubunu silme](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. **Silmek istediğinizden emin misiniz** dikey penceresine **rbac-quickstart-resource-group** olan kaynak grubu adını yazın.

1. Kaynak grubunu silmek için **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve PowerShell kullanarak kullanıcıya erişim izni verme](tutorial-role-assignments-user-powershell.md)

