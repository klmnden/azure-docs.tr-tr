---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Pingboard yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve kullanıcı hesaplarına Pingboard sağlanmasını için Azure Active Directory yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: b1d2e5468aa5b6a10b93ea118969d66789a17f50
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Öğretici: Pingboard otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı otomatik sağlama ve Pingboard için Azure Active Directory'den (Azure AD) kullanıcı hesaplarını XML'deki sağlama gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure AD kiracısı
*   Pingboard Kiracı [Pro hesabı](https://pingboard.com/pricing) 
*   Pingboard yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE] 
> Azure AD tümleştirme sağlama kullanır [Pingboard API](`https://your_domain.pingboard.com/scim/v2`), hesabınıza kullanılabilir olduğu.

## <a name="assign-users-to-pingboard"></a>Kullanıcılar için Pingboard atama

Azure AD "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca bir uygulamaya Azure AD'de atanan kullanıcılar eşitlenir. 

Yapılandırıp sağlama hizmeti etkinleştirmeden önce hangi kullanıcıların Azure AD'de Pingboard uygulamanızı erişmesi karar vermeniz gerekir. Ardından, buradaki yönergeleri izleyerek Pingboard uygulamanız bu kullanıcılar atayabilirsiniz:

[Kullanıcı bir kurumsal uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Kullanıcılar için Pingboard atamak için önemli ipuçları

Tek bir atamanızı öneririz Pingboard sağlama yapılandırmayı test etmek için Azure AD kullanıcı. Ek kullanıcılar daha sonra atanabilir.

## <a name="configure-user-provisioning-to-pingboard"></a>Kullanıcı için Pingboard sağlamayı Yapılandır 

Bu bölümde, Azure AD API sağlama Pingboard kullanıcı hesabı konusunda size rehberlik eder. Ayrıca, oluşturmak, güncelleştirmek ve Azure AD'de kullanıcı atamaları temel alınarak Pingboard atanan kullanıcı hesaplarında devre dışı bırakmak için sağlama hizmeti de yapılandırın.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Pingboard etkinleştirmek için sağlanan yönergeleri izleyin [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için Pingboard Azure AD'de sağlama yapılandırmak için

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Pingboard zaten yapılandırdıysanız, Pingboard Örneğiniz için arama alanı kullanarak arayın. Aksi takdirde seçin **Ekle** arayın ve **Pingboard** uygulama galerisinde. Seçin **Pingboard** Arama sonuçlarından ve uygulamaları listenize ekleyin.

3. Pingboard örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![Pingboard sağlama](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **Kiracı URL**, girin `https://your_domain.pingboard.com/scim/v2`ve "şöyledir: your_domain" gerçek etki alanı ile değiştirin.

    b. Oturum [Pingboard](https://pingboard.com/) yönetici hesabı kullanarak.

    c. Seçin **eklentileri** > **tümleştirmeler** > **Azure Active Directory**.

    d. Git **yapılandırma** sekmesini tıklatın ve seçin **Azure'dan kullanıcı sağlamayı etkinleştirin**.

    e. Belirteçte kopyalama **OAuth taşıyıcı belirteci**ve bunu girin **gizli belirteci**.

6. Azure portalında seçin **Bağlantıyı Sına** Azure emin olmak için AD Pingboard uygulamanıza bağlanabilir. Bağlantı başarısız olursa Pingboard hesabınız yönetici izinlerine sahip olduğundan emin olun ve deneyin **Bağlantıyı Sına** adım yeniden uygulayın.

7. Bir kişi veya sağlama hata bildirimlerini almak istediğiniz Grup e-posta adresini girin **bildirim e-posta**. Altındaki onay kutusunu seçin.

8. **Kaydet**’i seçin. 

9. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Pingboard**.

10. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Pingboard için eşitlenmek üzere kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Pingboard kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** değişiklikleri kaydedilemedi. Daha fazla bilgi için bkz: [Özelleştir kullanıcı öznitelik eşlemelerini hazırlama](active-directory-saas-customizing-attribute-mappings.md).

11. Azure AD Pingboard, hizmet sağlamayı etkinleştirmek için **ayarları** bölümünde **sağlama durumu** için **üzerinde**.

12. Seçin **kaydetmek** Pingboard için atanan kullanıcılar ilk eşitleme başlatmak için.

İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanım **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve etkinlik günlükleri sağlamak için bağlantıları izleyin. Günlükleri Pingboard uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [rapor otomatik olarak bir kullanıcı hesabı sağlama](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-pingboard-tutorial.md)
