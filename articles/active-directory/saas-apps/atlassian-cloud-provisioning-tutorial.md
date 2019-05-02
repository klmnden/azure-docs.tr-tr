---
title: 'Öğretici: Atlassian bulut Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Atlassian Cloud kullanıcı hesaplarınızı Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.openlocfilehash: 4e028429ca8a22915eff2b90ca63c6d05a67741b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692230"
---
# <a name="tutorial-configure-atlassian-cloud-for-automatic-user-provisioning"></a>Öğretici: Atlassian bulut için otomatik kullanıcı hazırlama yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Atlassian bulut sağlamasını için Atlassian Bulut ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).
>
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı
* [Atlassian bulut Kiracı](https://www.atlassian.com/licensing/cloud)
* Atlassian bulut yönetici izinlerine sahip bir kullanıcı hesabı.

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan **Atlassian bulut SCIM API**, Atlassian Bulutu takımlara kullanılabildiği.

## <a name="add-atlassian-cloud-from-the-gallery"></a>Atlassian bulut Galeriden Ekle

Atlassian bulut için otomatik kullanıcı hazırlama Azure AD'ye yapılandırmadan önce Atlassian bulut Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Atlassian bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** bölmenin üstünde düğme.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Atlassian bulut**seçin **Atlassian bulut** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Atlassian bulut](common/search-new-app.png)

## <a name="assigning-users-to-atlassian-cloud"></a>Atlassian buluta kullanıcıları atama

Azure Active Directory kullanan adlı bir kavram *atamaları* hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Atlassian bulut erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Atlassian buluta bu kullanıcılara ve/veya grupları atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-atlassian-cloud"></a>Kullanıcılar, Atlassian buluta atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Atlassian buluta atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Atlassian buluta kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-atlassian-cloud"></a>Atlassian bulut için otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya Azure AD'de kullanıcı ve/veya grup atamalarını Atlassian bulutta grupları temel.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma Atlassian bulut için etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Atlassian bulut tek öğretici](atlassian-cloud-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-atlassian-cloud-in-azure-ad"></a>Azure AD'de Atlassian bulut için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Atlassian bulut**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Atlassian bulut**.

    ![Uygulamalar listesinde Atlassian bulut bağlantısı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/provisioning-tab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/credentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si** ve **gizli belirteç** Atlassian bulutun hesabının. Bu değerleri örnekleri şunlardır:

   * İçinde **Kiracı URL'si** alanında, adım 6'da açıklandığı gibi Atlassian aldığınız özel Kiracı uç noktası doldurun. Örneğin: `https://api.atlassian.com/scim/directory/{directoryId}`.

   * İçinde **gizli belirteç** alanında, adım 6'da açıklandığı gibi gizli belirteç doldurun.

6. Gidin [Atlassian kuruluş yöneticisi](https://admin.atlassian.com) **> Kullanıcı sağlamayı** tıklayın **belirteç oluşturma**. Kopyalama **dizini temel URL'si** ve **taşıyıcı belirteci** için **Kiracı URL'si** ve **gizli belirteç** sırasıyla alanları.

    ![Atlassian sağlama bulut](./media/atlassian-cloud-provisioning-tutorial/secret-token-1.png) ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/secret-token-2.png)

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/secret-token-3.png)

7. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD, Atlassian buluta bağlanabilirsiniz. Bağlantı başarısız olursa, Atlassian bulut hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/test-connection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/notification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için Atlassian bulut**.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/provision-users.png)

11. Atlassian buluta Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, Atlassian bulut kullanıcı hesapları için güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/user-mapping.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Atlassian bulut**.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/provision-groups.png)

13. Atlassian buluta Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, Atlassian buluttaki gruplar için güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/group-mapping.png)

14. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Azure AD sağlama hizmeti Atlassian bulut için etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/provisioning-on.png)

16. Kullanıcılara ve/veya istediğiniz grupları için Atlassian bulut sağlamayı istenen değerleri seçerek tanımlayın **kapsam** içinde **ayarları** bölümü.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/provisioning-options.png)

17. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Atlassian bulut sağlama](./media/atlassian-cloud-provisioning-tutorial/save.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti Atlassian bulut üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Atlassian bulut sağlayan yalnızca kullanıcı sağlama [etki alanlarını doğrulandı](https://confluence.atlassian.com/cloud/organization-administration-938859734.html).
* Atlassian bulut grubu yeniden adlandırma şu anda desteklemiyor. Bu, Azure AD'de bir grup displayName herhangi bir değişiklik değil güncelleştirilecek ve Atlassian bulutta yansıtılan olduğunu anlamına gelir.
* Değerini **posta** kullanıcı özniteliği Azure AD'de kullanıcının bir Microsoft Exchange posta kutusu varsa yalnızca doldurulur. Kullanıcı bir sahip değilse, istenen farklı bir öznitelik eşlemek için önerilir **e-postaları** Atlassian bulutta özniteliği.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png
