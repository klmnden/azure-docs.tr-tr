---
title: Azure Blockchain Workbench'i kullanıcıları yönetme
description: Azure Blockchain Workbench kullanıcıları yönetmek nasıl.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/09/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 15babefda36ba37cf6df7820ac888668e4a502be
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65509914"
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Azure Blockchain Workbench'i kullanıcıları yönetme

Azure Blockchain Workbench, kişiler ve, consortium parçası olan kuruluşlar için kullanıcı yönetimi içerir.

## <a name="prerequisites"></a>Önkoşullar

Blockchain Workbench'i dağıtım gerekli değildir. Bkz: [Azure Blockchain Workbench dağıtım](deploy.md) dağıtımı hakkında ayrıntılı bilgi için.

## <a name="add-azure-ad-users"></a>Azure AD kullanıcı ekleme

Azure Blockchain Workbench, kimlik doğrulaması, erişimi ve rolleri için Azure Active Directory (Azure AD) kullanır. Blockchain Workbench'i Azure AD kiracısındaki kullanıcılar, kimlik doğrulaması ve Blockchain Workbench'i kullanın. Kullanıcılar, etkileşim ve eylemler gerçekleştirmek için yönetici uygulama rolüne ekleyin.

Blockchain Workbench'i kullanıcıların uygulamaları ve rolleri için bunları atamadan önce Azure AD kiracısında mevcut gerekir. Azure AD'ye kullanıcı eklemek için aşağıdaki adımları kullanın:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Sağ üst köşedeki hesabınızı seçin ve Blockchain Workbench'i ilişkili Azure AD kiracısı geçin.
3.  Seçin **Azure Active Directory > kullanıcılar**. Dizininizdeki kullanıcıların bir listesini görürsünüz.
4.  Kullanıcı dizine eklemek için seçin **yeni kullanıcı**. Dış kullanıcılar için **yeni Konuk kullanıcı**.

    ![Yeni kullanıcı](./media/manage-users/add-ad-user.png)

5.  Yeni kullanıcı için gerekli alanları doldurun. **Oluştur**’u seçin.

Ziyaret [Azure AD'ye](../../active-directory/fundamentals/add-users-azure-active-directory.md) içinde Azure AD kullanıcıları yönetme hakkında daha fazla ayrıntı için belgeleri.

## <a name="manage-blockchain-workbench-administrators"></a>Blockchain Workbench'i yöneticileri yönetme

Kullanıcıların dizine eklendikten sonra sonraki adımda hangi kullanıcıların Blockchain Workbench'i yöneticilerdir seçmektir. Kullanıcıların **yönetici** grubu ile ilişkili **yönetici uygulama rolü** Blockchain Workbench içinde. Yöneticiler ekleme veya kullanıcıları kaldırmak, belirli senaryolar için kullanıcı atama ve yeni uygulamalar oluşturun.

Kullanıcıları eklemek için **yönetici** grubunda Azure AD dizini:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Sağ üst köşedeki hesabınızı seçerek Blockchain Workbench'i ilişkili Azure AD kiracısında olduğundan emin olun.
3.  Seçin **Azure Active Directory > Kurumsal uygulamalar**.
4.  Blockchain Workbench'i için Azure AD İstemci uygulamayı seçin
    
    ![Tüm kurumsal uygulama kayıtları](./media/manage-users/select-blockchain-client-app.png)

5.  Seçin **kullanıcılar ve Gruplar > Kullanıcı Ekle**.
6.  İçinde **atama Ekle**seçin **kullanıcılar**. Seçin veya bir yönetici olarak eklemek istediğiniz kullanıcı için arama yapın. Tıklayın **seçin** bittiğinde seçme.

    ![Atama Ekle](./media/manage-users/add-user-assignment.png)

9.  Doğrulama **rol** ayarlanır **yönetici**
10. **Ata**'yı seçin. Eklenen kullanıcılar Yönetici rolü atanan listesinde görüntülenir.

    ![Blok zinciri istemci uygulama kullanıcıları](./media/manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Blockchain Workbench'i üyeleri yönetme

Blockchain Workbench uygulaması, consortium parçası olan kuruluşların yönetmek için kullanın. Ekleyebilir veya kullanıcılara uygulamaları ve rolleri kaldırın.

1. [Blockchain Workbench'i açın](deploy.md#blockchain-workbench-web-url) tarayıcısı ve bir yönetici olarak oturum açın.

    ![Blockchain Workbench](./media/manage-users/blockchain-workbench-applications.png)

    Üyeler her uygulamaya eklenir. Sözleşmeler başlatmak veya eylemi için bir veya daha fazla uygulama rolleri üyeleri olabilir.

2. Bir uygulama için üyeleri yönetmek için bir uygulama kutucuğunda seçin **uygulamaları** bölmesi.

    Seçili uygulama için ilişkili üye sayısı üyeleri kutucukta yansıtılır.

    ![Uygulama seçin](./media/manage-users/blockchain-workbench-select-application.png)


#### <a name="add-member-to-application"></a>Üye için uygulama ekleme

1. Geçerli üyelerin listesi görüntülemek için üye kutucuğu seçin.
2. Seçin **üye ekleme**.

    ![Üye Ekle](./media/manage-users/application-add-members.png)

3. Kullanıcının adını arayın.  Blockchain Workbench'i kiracısında mevcut Azure AD kullanıcıları yalnızca listelenir. Kullanıcı bulunamazsa, yapmanız [Azure AD kullanıcı ekleme](#add-azure-ad-users).

    ![Üye Ekle](./media/manage-users/find-user.png)

4. Seçin bir **rol** açılır listeden.

    ![Rol üyeleri seçin](./media/manage-users/application-select-role.png)

5. Seçin **Ekle** uygulama ile ilişkili rol üyesi eklemek için.

#### <a name="remove-member-from-application"></a>Uygulamadan üye kaldırma

1. Geçerli üyelerin listesi görüntülemek için üye kutucuğu seçin.
2. Kaldırmak istediğiniz kullanıcı için **Kaldır** rolünden açılır.

    ![Üyeyi Kaldır](./media/manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Değiştirme veya rol ekleme

1. Geçerli üyelerin listesi görüntülemek için üye kutucuğu seçin.
2. Değiştirmek istediğiniz bu kullanıcı için açılan listeyi tıklatın ve yeni rol seçin.

    ![Rolü değiştir](./media/manage-users/application-change-role.png)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, Azure Blockchain Workbench için kullanıcıları yönetmek öğrendiniz. Bir blok zinciri uygulaması oluşturmayı öğrenmek için sonraki nasıl yapılır makalesi için devam edin.

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma](create-app.md)
