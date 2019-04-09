---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Tableau Online yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Tableau çevrimiçi kullanıcı hesapları için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: f732eebd410a6b52a21a46925a29bf4676f7c8cb
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057498"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Öğretici: Tableau çevrimiçi yapılandırmak için otomatik kullanıcı hazırlama

Bu öğreticinin amacı, Tableau çevrimiçi ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Tableau çevrimiçi sağlamasını için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki sahip olduğunuz varsayılır:

*   Azure AD kiracısı
*   A [Tableau çevrimiçi Kiracı](https://www.tableau.com/)
*   Tableau Online'da yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [Tableau çevrimiçi Rest API](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm), Tableau çevrimiçi geliştiricilere sunulan olduğu.

## <a name="adding-tableau-online-from-the-gallery"></a>Tableau çevrimiçi galeriden ekleme
Tableau çevrimiçi Azure AD ile otomatik kullanıcı hazırlama için yapılandırmadan önce Tableau çevrimiçi Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Tableau çevrimiçi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Tableau çevrimiçi**seçin **Tableau çevrimiçi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde tableau çevrimiçi](common/search-new-app.png)

## <a name="assigning-users-to-tableau-online"></a>Kullanıcıları için çevrimiçi Tableau atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Tableau Online'a erişmesi gereken karar vermeniz gerekir. Karar sonra bu kullanıcılara ve/veya grupları Tableau Online'a Buradaki yönergeleri izleyerek atayabilirsiniz:

*   [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Kullanıcıların çevrimiçi Tableau atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı atandığı Tableau Online'a sağlama yapılandırmasını otomatik kullanıcı test etmek için. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Kullanıcı çevrimiçi Tableau atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-tableau-online"></a>Tableau Online'a otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılara ve/veya Tableau ile Azure AD'de kullanıcı ve/veya grup atamalarını tabanlı çevrimiçi gruplarında devre dışı bırakmak için Azure AD sağlama hizmeti yapılandırmak için gereken adımları size kılavuzluk eder.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Tableau çevrimiçi etkinleştirmek, yönergeleri izleyerek sağlanan [Tableau çevrimiçi tek oturum açma öğretici](tableauonline-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Azure AD'de Tableau Online için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamalar**, ardından **Tableau çevrimiçi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau çevrimiçi**.

    ![Uygulamalar listesinde Tableau çevrimiçi bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **etki alanı**, **yönetici kullanıcı adı**, **yönetici parolası**, ve **içerik URL** Tableau Online hesabınızın:

   * İçinde **etki alanı** alanında, adım 6'da temel bir alt etki alanı doldurun.

   * İçinde **yönetici kullanıcı adı** alan, Clarizen kiracınıza yönetici hesabının kullanıcı adını doldurun. Örnek: admin@contoso.com.

   * İçinde **yönetici parolası** alan, yönetici kullanıcı adı için karşılık gelen yönetici hesabının parolasını doldurun.

   * İçinde **içerik URL'si** alanında, adım 6'da temel bir alt etki alanı doldurun.

6. Çevrimiçi Tableau değerlerini yönetici hesabınızda oturum açtıktan sonra **etki alanı** ve **içerik URL'si** yönetici sayfasına URL'den ayıklanabilir.

    * **Etki alanı** Tableau çevrimiçi için hesap URL'sinin bu bölümünden kopyalanabilir:

        ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * **İçerik URL'si** Tableau çevrimiçi için hesap sayfasından Bu bölüm kopyalanabilir ve hesap kurulumu sırasında tanımlanan bir değer. Bu örnekte, "contoso" değeridir:

        ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > **Etki alanı** burada gösterilen farklı olabilir.

7. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD Tableau Online'a bağlayabilirsiniz. Bağlantı başarısız olursa, Tableau Online hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için Tableau**.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. Tableau çevrimiçi Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesaplarını Tableau çevrimiçi güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Tableau**.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. Tableau çevrimiçi Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesaplarını Tableau çevrimiçi güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Azure AD sağlama hizmeti için Tableau çevrimiçi etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. Kullanıcılara ve/veya istediğiniz grupları Tableau çevrimiçi sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama Tableau Online hizmeti Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
