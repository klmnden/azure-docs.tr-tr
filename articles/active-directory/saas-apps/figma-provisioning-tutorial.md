---
title: 'Öğretici: Figma Azure Active Directory ile otomatik kullanıcı sağlamayı yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Figma kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: b71aa6709b1c93688ea3eece4ce3f4066f9a6b7a
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673177"
---
# <a name="tutorial-configure-figma-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Figma yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Figma sağlamasını Figma ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).
>
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Figma Kiracı](https://www.figma.com/pricing/).
* Figma yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="assign-users-to-figma"></a>Kullanıcılar için Figma atayın.
Azure Active Directory atamaları adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Figma erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Figma için bu kullanıcılara ve/veya grupları atayabilirsiniz:
 
* [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)
## <a name="important-tips-for-assigning-users-to-figma"></a>Kullanıcılar için Figma atamak için önemli ipuçları

 * Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Figma atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için Figma atarken, (varsa) geçerli bir uygulamaya özgü rol ataması iletişim kutusunda seçmeniz gerekir. Varsayılan erişim rolüne sahip kullanıcılar, sağlamasından bırakılır.

## <a name="set-up-figma-for-provisioning"></a>Sağlama için Figma ayarlayın

Otomatik kullanıcı hazırlama ile Azure AD için Figma yapılandırmadan önce Figma bazı sağlama bilgilerini almak gerekir.

1. Oturum açın, [Figma Yönetici Konsolu](https://www.Figma.com/). Kiracınızın yanındaki dişli simgesine tıklayın.

    ![FigmaFigma çalışan sağlama](media/Figma-provisioning-tutorial/image0.png)

2. Gidin **genel > güncelleştirme ayarları günlüğünde**.

    ![FigmaFigma çalışan sağlama](media/Figma-provisioning-tutorial/figma03.png)

3. Kopyalama **Kiracı kimliği**. Bu değer, içine girdiğiniz SCIM uç nokta URL'si oluşturmak için kullanılacak **Kiracı URL'si** Figma uygulamanızı Azure portalında sağlama sekmesindeki alan.

    ![Figma belirteci oluşturma](media/Figma-provisioning-tutorial/figma-tenantid.png)

4. Ekranı aşağı kaydırın ve tıklayarak **API belirteci üretmek**.

    ![Figma belirteci oluşturma](media/Figma-provisioning-tutorial/token.png)

5. Kopyalama **API belirteci** değeri. Bu değer girilir **gizli belirteç** Figma uygulamanızı Azure portalında sağlama sekmesindeki alan. 

    ![Figma belirteci oluşturma](media/Figma-provisioning-tutorial/figma04.png)

## <a name="add-figma-from-the-gallery"></a>Galeriden Figma Ekle

Figma otomatik kullanıcı hazırlama Azure AD ile yapılandırmak için Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Figma eklemeniz gerekir.

1. İçinde  **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** bölmenin üstünde düğme.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Figma**seçin **Figma** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Figma](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-figma"></a>Figma için otomatik kullanıcı sağlamayı yapılandırma 

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya Figma gruplarında Azure AD'de kullanıcı ve/veya grup atamaları temel alarak.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Figma etkinleştirmek, yönergeleri izleyerek sağlanan [Figma tek oturum açma öğretici](figma-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-figma--in-azure-ad"></a>Azure AD'de Figma için otomatik kullanıcı hazırlama yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **kurumsal uygulamalar**, ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Figma**.

    ![Uygulamalar listesinde Figma bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Sağlama sekmesinde](common/provisioning.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Sağlama sekmesinde](common/provisioning-automatic.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde `https://www.figma.com/scim/v2/<TenantID>` içinde **Kiracı URL'si** burada **Tenantıd** Figma daha önce aldığınız değerdir. Giriş **API belirteci** değerini **gizli belirteç**. Tıklayın **Test Bağlantısı** Azure emin olmak için AD için Figma bağlanabilirsiniz. Bağlantı başarısız olursa Figma hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL'si + simgesi](common/provisioning-testconnection-tenanturltoken.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Figma**.

    ![Figma kullanıcı eşlemeleri](media/Figma-provisioning-tutorial/figma05.png)

11. İçinde Figma için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Figma kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Figma kullanıcı öznitelikleri](media/Figma-provisioning-tutorial/figma06.png)

12. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Azure AD sağlama hizmeti için Figma etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Açıkken sağlama durumu](common/provisioning-toggle-on.png)

14. Kullanıcılara ve/veya istediğiniz grupları Figma sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Kapsam sağlama](common/provisioning-scope.png)

15. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Sağlama yapılandırmasını kaydetme](common/provisioning-configuration-save.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti Figma üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)