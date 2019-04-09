---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Bonusly yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Bonusly kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 879b0ee9-042a-441b-90a7-8c364d62426a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ad0ee590572dbc92e67be9f84ffc65afc3e8473
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278743"
---
# <a name="tutorial-configure-bonusly-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Bonusly yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Bonusly sağlamasını Bonusly ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki sahip olduğunuz varsayılır:

* Azure AD kiracısı
* A [Bonusly Kiracı](https://bonus.ly/pricing)
* Bonusly yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [Bonusly Rest API](https://bonusly.gelato.io/reference), Bonusly geliştiricilere sunulan olduğu.

## <a name="adding-bonusly-from-the-gallery"></a>Galeriden Bonusly ekleme

Azure AD ile otomatik kullanıcı hazırlama için Bonusly yapılandırmadan önce Bonusly Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Bonusly eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Bonusly**seçin **Bonusly** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde bonusly](common/search-new-app.png)

## <a name="assigning-users-to-bonusly"></a>Bonusly için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir. 

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Bonusly erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Bonusly için bu kullanıcılara ve/veya grupları atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-bonusly"></a>Kullanıcılar için Bonusly atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Bonusly atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için Bonusly atarken, (varsa) geçerli bir uygulamaya özgü rol ataması iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-bonusly"></a>Bonusly için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya Bonusly gruplarında Azure AD'de kullanıcı ve/veya grup atamaları temel alarak.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Bonusly etkinleştirmek, yönergeleri izleyerek sağlanan [Bonusly tek oturum açma öğretici](bonus-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-bonusly-in-azure-ad"></a>Azure AD'de Bonusly için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Bonusly**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Bonusly**.

    ![Uygulamalar listesinde Bonusly bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **gizli belirteç** adım 6'da açıklandığı gibi Bonusly hesap.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/secrettoken.png)

6. **Gizli belirteç** hesabının bulunduğu için Bonusly **yönetici > Şirket > tümleştirmeler**. İçinde **kodu istiyorsanız** bölümünde, tıklayarak **API > oluşturma yeni API erişim belirteci** yeni bir gizli dizi belirteci oluşturmak için.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/BonuslyIntegrations.png)

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/BonsulyRestApi.png)

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/CreateToken.png)

7. Belirtilen metin kutusunda, erişim belirtecinde tuşuna için aşağıdaki ekranda bir ad yazın. **API anahtarı oluştur**. Yeni bir erişim belirteci için birkaç saniye içinde bir açılır pencere görüntülenir.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/Token01.png)

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/Token02.png)

8. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD için Bonusly bağlanabilirsiniz. Bağlantı başarısız olursa Bonusly hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/TestConnection.png)

9. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/EmailNotification.png)

10. **Kaydet**’e tıklayın.

11. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Bonusly**.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/UserMappings.png)

12. İçinde Bonusly için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Bonusly kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/UserAttributeMapping.png)

13. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Azure AD sağlama hizmeti için Bonusly etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/ProvisioningStatus.png)

15. Kullanıcılara ve/veya istediğiniz grupları Bonusly sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/ScopeSync.png)

16. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Bonusly sağlama](./media/bonusly-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti Bonusly üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/bonusly-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bonusly-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/bonusly-provisioning-tutorial/tutorial_general_03.png
