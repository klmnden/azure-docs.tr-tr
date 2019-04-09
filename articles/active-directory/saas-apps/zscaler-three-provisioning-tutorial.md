---
title: 'Öğretici: Zscaler üç Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Zscaler üç kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 385a1153-0f47-4e41-8f44-da1b49d7629e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: ed158ae825ec8aac24a57eb0f5a986b2124b66fb
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273694"
---
# <a name="tutorial-configure-zscaler-three-for-automatic-user-provisioning"></a>Öğretici: Zscaler üç yapılandırmak için otomatik kullanıcı hazırlama

Bu öğreticinin amacı Zscaler üç ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zscaler üç sağlamasını için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).
>
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki sahip olduğunuz varsayılır:

* Azure AD kiracısı
* Üç Zscaler Kiracı
* Zscaler üç, yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirme Zscaler üç SCIM API'yi, Kurumsal paketi olan hesaplar için Zscaler üç geliştiricilerine kullanılabildiği kullanır.

## <a name="adding-zscaler-three-from-the-gallery"></a>Zscaler üç galeri ekleme

Zscaler üç otomatik kullanıcı hazırlama ile Azure AD için yapılandırmadan önce Zscaler üç Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Zscaler üç Azure AD uygulama galerisinden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler üç**seçin **Zscaler üç** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde üç Zscaler](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-three"></a>Zscaler üç için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Zscaler üç erişmesi gereken karar vermeniz gerekir. Karar sonra bu kullanıcılara ve/veya grupları Zscaler üç için buradaki yönergeleri izleyerek atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-three"></a>Zscaler üç için kullanıcı atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı atandığı Zscaler üç için otomatik kullanıcı sağlama yapılandırmasını test etmek için. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Zscaler üç için kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-zscaler-three"></a>Zscaler üç için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılara ve/veya Zscaler Azure AD'de kullanıcı ve/veya grup atamalarını göre üç gruplarında devre dışı bırakmak için Azure AD sağlama hizmeti yapılandırmak için gereken adımları size kılavuzluk eder.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Zscaler üç etkinleştirmek, yönergeleri izleyerek sağlanan [Zscaler üç tek oturum açma öğretici](zscaler-three-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-three-in-azure-ad"></a>Azure AD'de Zscaler üç için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zscaler üç**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler üç**.

    ![Uygulamalar listesinde Zscaler üç bağlantı](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/provisioning-tab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/provisioning-credentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si** ve **gizli belirteç** adım 6'da açıklandığı gibi üç Zscaler hesabınızın.

6. Elde etmek için **Kiracı URL'si** ve **gizli belirteç**, gitmek **Yönetim > kimlik doğrulama ayarları** Zscaler üç portal kullanıcı arabirimi ve tıklayın **SAML** altında **kimlik doğrulama türü**. 

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/secret-token-1.png)

    Tıklayarak **SAML yapılandırma** açmak için **yapılandırma SAML** seçenekleri.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/secret-token-2.png)

    Seçin **Enable SCIM-Based sağlama** alınacak **temel URL** ve **taşıyıcı belirteci**, ayarları kaydedin. Kopyalama **temel URL** için **Kiracı URL'si** ve **taşıyıcı belirteci** için **gizli belirteç** Azure portalında.

7. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD Zscaler üç için bağlanabilirsiniz. Bağlantı başarısız olursa Zscaler üç hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/test-connection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/notification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için Zscaler üç**.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/user-mappings.png)

11. Zscaler üç için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesaplarını Zscaler üç güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/user-attribute-mappings.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Zscaler üç**.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/group-mappings.png)

13. Zscaler üç için Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri gruplarında Zscaler üç güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD sağlama hizmeti için Zscaler üç etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/provisioning-status.png)

16. Kullanıcılara ve/veya istediğiniz grupları Zscaler üç sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/save-provisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti üzerinde Zscaler üç Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-03.png
