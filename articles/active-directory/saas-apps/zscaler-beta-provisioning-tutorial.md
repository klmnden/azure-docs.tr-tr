---
title: 'Öğretici: Zscaler Beta Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Zscaler Beta için kullanıcı hesapları için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 83db6b8d-503b-48f3-b918-f9fba1369d53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 82f9746b8f2f6665506491c328841e6a88438472
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672966"
---
# <a name="tutorial-configure-zscaler-beta-for-automatic-user-provisioning"></a>Öğretici: Zscaler Beta için otomatik kullanıcı hazırlama yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zscaler Beta sağlamasını Zscaler Beta ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).
>

> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki sahip olduğunuz varsayılır:

* Azure AD kiracısı
* Zscaler Beta Kiracı
* Zscaler Beta yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirme, Kurumsal paketi olan hesaplar için Zscaler Beta geliştiricilerine kullanılabilir olduğu Zscaler ile ilgili Beta SCIM API kullanır.

## <a name="adding-zscaler-beta-from-the-gallery"></a>Zscaler Beta galeri ekleme

Zscaler Beta için otomatik kullanıcı hazırlama Azure AD'ye yapılandırmadan önce Azure AD uygulama galerisinden listenize yönetilen SaaS uygulamalarının Zscaler Beta eklemeniz gerekir.

**Azure AD uygulama galerisinden Zscaler Beta eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler Beta**seçin **Zscaler Beta** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Zscaler Beta](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-beta"></a>Zscaler Beta için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Zscaler Beta erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Zscaler Beta için bu kullanıcılara ve/veya grupları atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-beta"></a>Zscaler Beta için kullanıcı atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Zscaler Beta atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Zscaler Beta için kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-zscaler-beta"></a>Zscaler Beta için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya Azure AD'de kullanıcı ve/veya grup atamalarını Zscaler Beta grupları temel.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma Zscaler Beta etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Zscaler Beta tek öğretici](zscaler-beta-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-beta-in-azure-ad"></a>Azure AD'de Zscaler Beta için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zscaler Beta**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Beta**.

    ![Uygulamalar listesinde Zscaler Beta bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/provisioning-tab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/provisioning-credentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si** ve **gizli belirteç** Zscaler Beta hesabınızın adım 6'da açıklandığı gibi.

6. Elde etmek için **Kiracı URL'si** ve **gizli belirteç**, gitmek **Yönetim > kimlik doğrulama ayarları** Zscaler Beta portal kullanıcı arabirimi ve tıklayın **SAML** altında **kimlik doğrulama türü**.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/secret-token-1.png)

    Tıklayarak **SAML yapılandırma** açmak için **yapılandırma SAML** seçenekleri.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/secret-token-2.png)

    Seçin **Enable SCIM-Based sağlama** alınacak **temel URL** ve **taşıyıcı belirteci**, ayarları kaydedin. Kopyalama **temel URL** için **Kiracı URL'si**, ve **taşıyıcı belirteci** için **gizli belirteç** Azure portalında.

7. 5\. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD Zscaler Beta bağlanabilirsiniz. Bağlantı başarısız olursa Zscaler Beta hesabınıza yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/test-connection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/notification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Zscaler Beta**.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/user-mappings.png)

11. Zscaler Beta için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zscaler Beta kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/user-attribute-mappings.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Zscaler Beta**.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/group-mappings.png)

13. Zscaler Beta için Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zscaler Beta grupları güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD sağlama hizmeti Zscaler Beta etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/provisioning-status.png)

16. Kullanıcılara ve/veya istediğiniz grupları Zscaler Beta sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Zscaler sağlama Beta](./media/zscaler-beta-provisioning-tutorial/save-provisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti Zscaler Beta üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-03.png
