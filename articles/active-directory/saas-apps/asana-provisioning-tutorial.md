---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Asana yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve kullanıcı hesaplarına Asana sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 65501bff4ca6c13fe5951ebc260e3ec75514bc8f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972136"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Öğretici: Asana otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı Asana ve Azure Active Directory (Azure AD) otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den Asana sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure AD kiracısı
*   Asana Kiracı ile bir [Kurumsal](https://www.asana.com/pricing) planlayabilir ya da daha iyi etkin 
*   Asana yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE] 
> Azure AD tümleştirme sağlama kullanır [Asana API](https://app.asana.com/api/1.0/scim/Users), Asana için kullanılabilir olduğu.

## <a name="assign-users-to-asana"></a>Kullanıcılar için Asana atama

Azure AD "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca bir uygulamaya Azure AD'de atanan kullanıcılar eşitlenir. 

Yapılandırıp sağlama hizmeti etkinleştirmeden önce hangi kullanıcıların Azure AD'de Asana uygulamanızı erişmesi karar vermeniz gerekir. Ardından, buradaki yönergeleri izleyerek Asana uygulamanız bu kullanıcılar atayabilirsiniz:

[Kullanıcı bir kurumsal uygulama atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Kullanıcılar için Asana atamak için önemli ipuçları

Tek bir atamanızı öneririz Asana sağlama yapılandırmayı test etmek için Azure AD kullanıcı. Ek kullanıcılar daha sonra atanabilir.

## <a name="configure-user-provisioning-to-asana"></a>Kullanıcı için Asana sağlamayı Yapılandır 

Bu bölümde, Azure AD Asana kullanıcı hesabına API sağlama konusunda size rehberlik eder. Ayrıca, oluşturmak, güncelleştirmek ve Azure AD'de kullanıcı atamaları temel alınarak Asana atanan kullanıcı hesaplarında devre dışı bırakmak için sağlama hizmeti de yapılandırın.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Asana etkinleştirmek için sağlanan yönergeleri izleyin [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için Asana Azure AD'de sağlama yapılandırmak için

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Asana zaten yapılandırdıysanız, Asana Örneğiniz için arama alanı kullanarak arayın. Aksi takdirde seçin **Ekle** arayın ve **Asana** uygulama galerisinde. Seçin **Asana** Arama sonuçlarından ve uygulamaları listenize ekleyin.

3. Asana örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![Asana sağlama](./media/asana-provisioning-tutorial/asanaazureprovisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, belirteç oluşturmak ve içinde girmek için bu yönergeleri izleyin **gizli belirteci**:

    a. Oturum [Asana](https://app.asana.com) yönetici hesabı kullanarak.

    b. Profil fotoğrafınız üst çubuğundan seçin ve geçerli kuruluş adı ayarlarınızı seçin.

    c. Git **hizmet hesaplarını** sekmesi.

    d. Seçin **hizmet hesabını ekleyin**.

    e. Güncelleştirme **adı** ve **hakkında** ve gerektiğinde profil fotoğrafınız. Belirteçte kopyalama **belirteci**ve seçin **Değişiklikleri Kaydet**.

6. Azure portalında seçin **Bağlantıyı Sına** Azure AD Asana uygulamanıza bağlandığından emin olmak için. Bağlantı başarısız olursa Asana hesabınız yönetici izinlerine sahip olduğundan emin olun ve deneyin **Bağlantıyı Sına** adım yeniden uygulayın.

7. Bir kişi veya sağlama hata bildirimlerini almak istediğiniz Grup e-posta adresini girin **bildirim e-posta**. Altındaki onay kutusunu seçin.

8. **Kaydet**’i seçin. 

9. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Asana**.

10. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Asana için eşitlenmek üzere kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Asana kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** değişiklikleri kaydedilemedi. Daha fazla bilgi için bkz: [kullanıcı sağlama öznitelik eşlemelerini özelleştirme](../active-directory-saas-customizing-attribute-mappings.md).

11. Azure AD Asana, hizmet sağlamayı etkinleştirmek için **ayarları** bölümünde **sağlama durumu** için **üzerinde**.

12. **Kaydet**’i seçin. 

Asana atanan kullanıcılar için ilk eşitleme başlar şimdi **kullanıcılar** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanım **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve etkinlik günlükleri sağlamak için bağlantıları izleyin. Denetim günlüklerini Asana uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [rapor otomatik olarak bir kullanıcı hesabı sağlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](asana-tutorial.md)
