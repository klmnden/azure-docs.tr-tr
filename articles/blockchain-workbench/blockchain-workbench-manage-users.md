---
title: Azure Blockchain çalışma ekranındaki kullanıcıları yönetme
description: Azure Blockchain çalışma ekranındaki kullanıcıları yönetmek nasıl.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 440072a94ff7146ebcdb242a058ab48b434714f9
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Azure Blockchain çalışma ekranındaki kullanıcıları yönetme

Azure Blockchain çalışma ekranı kişiler ve, Konsorsiyumu parçası olan kuruluşlar için kullanıcı yönetimi içerir.

## <a name="prerequisites"></a>Önkoşullar

Blockchain çalışma ekranının dağıtım gereklidir. Bkz: [Azure Blockchain çalışma ekranının dağıtım](blockchain-workbench-deploy.md) dağıtımı hakkında ayrıntılı bilgi için.

## <a name="add-azure-ad-users"></a>Azure AD kullanıcı ekleme

Azure Blockchain çalışma ekranı, kimlik doğrulaması, erişim denetimi ve rolleri için Azure Active Directory (Azure AD) kullanır. Blockchain çalışma ekranı Azure AD Kiracı kullanıcıların kimliğini doğrulamak ve Blockchain çalışma ekranı kullanın. Etkileşim ve eylemler gerçekleştirmek için yönetici uygulama rolüne kullanıcılar ekleyin.

Blockchain çalışma ekranı kullanıcıların bunları uygulamaları ve rolleri atamadan önce Azure AD kiracısı'nda mevcut gerekir. Azure AD kullanıcı eklemek için aşağıdaki adımları kullanın:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Sağ üst köşedeki hesabınızı seçin ve Blockchain çalışma ekranına ilişkili Azure AD kiracısı geçin.
3.  Seçin **Azure Active Directory > kullanıcılar**. Kullanıcıların bir listesini dizininizde bakın.
4.  Dizine kullanıcı eklemek için seçin **yeni kullanıcı**. Dış kullanıcılar için **yeni Konuk kullanıcı**.

    ![Yeni kullanıcı](media/blockchain-workbench-manage-users/add-ad-user.png)

5.  Yeni kullanıcı için gerekli alanları doldurun. **Oluştur**’u seçin.

Ziyaret [Azure AD](../active-directory/add-users-azure-active-directory.md) içinde Azure AD kullanıcıları yönetme hakkında daha fazla ayrıntı için belgeleri.

## <a name="manage-blockchain-workbench-administrators"></a>Blockchain çalışma ekranı yöneticilerini Yönet

Kullanıcıların dizine eklendikten sonra sonraki adıma hangi kullanıcıların Blockchain çalışma ekranı yöneticilerdir seçmektir. Kullanıcılar **Yöneticiler** grubu ile ilişkili **yönetici uygulama rolü** Blockchain çalışma ekranındaki. Yöneticiler ekleyin veya kullanıcılar kaldırın, kullanıcılar için belirli senaryolar atama ve yeni uygulamalar oluşturun.

Kullanıcı eklemek için **Yöneticiler** Azure AD dizini grubu:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Sağ üst köşedeki hesabınızı seçerek Blockchain çalışma ekranına ilişkili Azure AD kiracısı içinde olduğundan emin olun.
3.  Seçin **Azure Active Directory > Kurumsal uygulamalar**.
4.  Blockchain çalışma ekranı için Azure AD istemci uygulaması seçin
    
    ![Tüm kurumsal uygulama kayıtlar](media/blockchain-workbench-manage-users/select-blockchain-client-app.png)

5.  Seçin **kullanıcılar ve Gruplar > Kullanıcı Ekle**.
6.  İçinde **eklemek atama**seçin **kullanıcılar**. Seçin veya bir yönetici olarak eklemek istediğiniz kullanıcı için arama yapın. Tıklatın **seçin** seçme bitirdikten sonra.

    ![Atama ekle](media/blockchain-workbench-manage-users/add-user-assignment.png)

9.  Doğrulama **rol** ayarlanır **yönetici**
10. **Ata**'yı seçin. Eklenen kullanıcıların atanan yönetici rolüne sahip listede görüntülenir.

    ![Blockchain istemci uygulaması kullanıcılar](media/blockchain-workbench-manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Blockchain çalışma ekranı üyeleri yönetme

Blockchain çalışma ekranı uygulama kullanıcıları ve sizin Konsorsiyumu parçası olan kuruluşlar yönetmek için kullanın. Ekleyebilir veya uygulamaları ve rollere kaldırabilirsiniz.

1. [Blockchain çalışma ekranı açmak](blockchain-workbench-deploy.md#blockchain-workbench-web-url) , tarayıcı ve yönetici olarak oturum açın.

    ![Blockchain Workbench](media/blockchain-workbench-manage-users/blockchain-workbench-applications.png)

    Üyeler her uygulamaya eklenir. Üye sözleşmeleri başlatmak veya eylemleri için bir veya daha fazla uygulama rolleri sahip olabilir.

2. Bir uygulama için üyeleri yönetmek için bir uygulama parçasında seçin **uygulamaları** bölmesi.

    Seçili uygulama için ilişkili üye sayısı üyeleri parçasında yansıtılır.

    ![Uygulama seçme](media/blockchain-workbench-manage-users/blockchain-workbench-select-application.png)


#### <a name="add-member-to-application"></a>Üye için uygulama ekleme

1. Üye kutucuğu geçerli üyeler listesini görüntülemek için seçin.
2. Seçin **üye eklemek**.

    ![Üye ekle](media/blockchain-workbench-manage-users/application-add-members.png)

3. Kullanıcının adını arayın.  Yalnızca Blockchain çalışma ekranı Kiracı mevcut Azure AD kullanıcılar listelenir. Kullanıcı bulunamazsa, gerek [Azure AD kullanıcı ekleme](#add-azure-ad-users).

    ![Üye ekle](media/blockchain-workbench-manage-users/find-user.png)

4. Seçin bir **rol** gelen açılır.

    ![Rol üyeleri seçin](media/blockchain-workbench-manage-users/application-select-role.png)

5. Seçin **Ekle** ilişkili rol üyesiyle uygulama eklemek için.

#### <a name="remove-member-from-application"></a>Üye uygulamadan Kaldır

1. Üye kutucuğu geçerli üyeler listesini görüntülemek için seçin.
2. Kaldırmak istediğiniz kullanıcı için seçim **kaldırmak** rolden açılır.

    ![Üyeyi kaldır](media/blockchain-workbench-manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Değiştirme veya rol ekleme

1. Üye kutucuğu geçerli üyeler listesini görüntülemek için seçin.
2. Değiştirmek istediğiniz bu kullanıcı için açılan listeyi tıklatın ve yeni rol seçin.

    ![Rolü Değiştir](media/blockchain-workbench-manage-users/application-change-role.png)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, Azure Blockchain çalışma ekranı için kullanıcıları yönetmek öğrendiniz. Blockchain uygulama oluşturmayı öğrenmek için sonraki nasıl yapılır makalesi için devam edin.

> [!div class="nextstepaction"]
> [İçinde Azure Blockchain çalışma ekranı blockchain uygulaması oluşturma](blockchain-workbench-create-app.md)
