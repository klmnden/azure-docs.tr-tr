---
title: 'Öğretici: Zscaler ZSCloud Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Zscaler ZSCloud kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: a752be80-d3ef-45d1-ac8f-4fb814c07b07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 8962f0cf79a8e4874018021b1f9009cf3dad844e
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59260417"
---
# <a name="tutorial-configure-zscaler-zscloud-for-automatic-user-provisioning"></a>Öğretici: Zscaler ZSCloud otomatik kullanıcı hazırlama için yapılandırma

Bu öğreticinin amacı otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zscaler ZSCloud sağlamasını Zscaler ZSCloud ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).
> 
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki sahip olduğunuz varsayılır:

* Azure AD kiracısı
* Zscaler ZSCloud Kiracı
* Zscaler ZSCloud yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirme, Kurumsal paketi olan hesaplar için Zscaler ZSCloud geliştiricilerine kullanılabilir olduğu Zscaler ile ilgili ZSCloud SCIM API kullanır.

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Zscaler ZSCloud galeri ekleme

Zscaler ZSCloud otomatik kullanıcı hazırlama ile Azure AD için yapılandırmadan önce Azure AD uygulama galerisinden listenize yönetilen SaaS uygulamalarının Zscaler ZSCloud eklemeniz gerekir.

**Azure AD uygulama galerisinden Zscaler ZSCloud eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler ZSCloud**seçin **Zscaler ZSCloud** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Zscaler ZSCloud](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-zscloud"></a>Zscaler ZSCloud için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Zscaler ZSCloud erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek Zscaler ZSCloud için bu kullanıcılara ve/veya grupları atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-zscloud"></a>Zscaler ZSCloud için kullanıcı atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Zscaler ZSCloud atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Zscaler ZSCloud için kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-zscaler-zscloud"></a>Zscaler ZSCloud için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size kılavuzluk eder ve/veya Azure AD'de kullanıcı ve/veya grup atamalarını Zscaler ZSCloud gruplarında bağlı.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Zscaler ZSCloud etkinleştirmek, yönergeleri izleyerek sağlanan [oturum açma Zscaler ZSCloud tek öğretici](zscaler-zsCloud-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-zscloud-in-azure-ad"></a>Azure AD'de Zscaler ZSCloud için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zscaler ZSCloud**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler ZSCloud**.

    ![Uygulamalar listesinde Zscaler ZSCloud bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/provisioningtab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/provisioningcredentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si** ve **gizli belirteç** Zscaler ZSCloud hesabınızın adım 6'da açıklandığı gibi.

6. Elde etmek için **Kiracı URL'si** ve **gizli belirteç**, gitmek **Yönetim > kimlik doğrulama ayarları** Zscaler ZSCloud portal kullanıcı arabirimi ve tıklayın **SAML** altında **kimlik doğrulama türü**.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/secrettoken1.png)

    Tıklayarak **SAML yapılandırma** açmak için **yapılandırma SAML** seçenekleri.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/secrettoken2.png)

    Seçin **Enable SCIM-Based sağlama** alınacak **temel URL** ve **taşıyıcı belirteci**, ayarları kaydedin. Kopyalama **temel URL** için **Kiracı URL'si** ve **taşıyıcı belirteci** için **gizli belirteç** Azure portalında.

7. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD için Zscaler ZSCloud bağlanabilirsiniz. Bağlantı başarısız olursa Zscaler ZSCloud hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/testconnection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/Notification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Zscaler ZSCloud**.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/usermappings.png)

11. Zscaler ZSCloud içinde için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zscaler ZSCloud kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/userattributemappings.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Zscaler ZSCloud**.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/groupmappings.png)

13. Zscaler ZSCloud içinde için Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zscaler ZSCloud gruplarında güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/groupattributemappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD sağlama hizmeti için Zscaler ZSCloud etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/provisioningstatus.png)

16. Kullanıcılara ve/veya istediğiniz grupları Zscaler ZSCloud sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/saveprovisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti Zscaler ZSCloud üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-03.png
