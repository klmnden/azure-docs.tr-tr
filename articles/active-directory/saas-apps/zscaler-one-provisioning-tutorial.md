---
title: 'Öğretici: Zscaler bir Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Zscaler bir kullanıcı hesapları için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 72f6ba2b-73ed-420a-863a-aff672f26fa3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 26448d5056e58cf1110e825ad04c5123fca20684
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59259448"
---
# <a name="tutorial-configure-zscaler-one-for-automatic-user-provisioning"></a>Öğretici: Zscaler bir yapılandırma için otomatik kullanıcı hazırlama

Bu öğreticinin amacı Zscaler bir ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zscaler bir sağlamasını için gerçekleştirilmesi gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).
>

> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki sahip olduğunuz varsayılır:

* Azure AD kiracısı
* Zscaler tek bir kiracı
* Bir kullanıcı hesabında Zscaler bir yönetici izinlerine sahip

> [!NOTE]
> Azure AD sağlama tümleştirme Zscaler bir SCIM API'yi, Kurumsal paketi sahip hesaplar için Zscaler bir geliştiriciler için kullanılabildiği kullanır.

## <a name="adding-zscaler-one-from-the-gallery"></a>Zscaler bir galeri ekleme

Zscaler bir Azure AD ile sağlama otomatik kullanıcı için yapılandırmadan önce Zscaler bir Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Zscaler bir Azure AD uygulama galerisinden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler bir**seçin **Zscaler bir** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Zscaler bir sonuç listesinde](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-one"></a>Zscaler bir için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Zscaler bir erişmesi gereken karar vermeniz gerekir. Karar sonra bu kullanıcılara ve/veya grupları bir Zscaler için buradaki yönergeleri izleyerek atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-one"></a>Kullanıcı için bir Zscaler atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı atandığı Zscaler bir için otomatik kullanıcı sağlama yapılandırmasını test etmek için. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için bir Zscaler atarken (varsa) geçerli bir uygulamaya özgü rol ataması iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-zscaler-one"></a>Zscaler bir için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcılara ve/veya gruplara Zscaler Azure AD'de kullanıcı ve/veya grup atamaları hakkında temel bir devre dışı bırakmak için Azure AD sağlama hizmeti yapılandırmak için gereken adımları size kılavuzluk eder.

> [!TIP]
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Zscaler bir etkinleştirmek, yönergeleri izleyerek sağlanan [Zscaler tek tek oturum açma öğretici](zscaler-One-tutorial.md). Bu iki özellik birbirine tamamlayıcı rağmen otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-one-in-azure-ad"></a>Azure AD'de bir Zscaler için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamalar**, ardından **Zscaler bir**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler bir**.

    ![Uygulamalar listesini Zscaler bir bağlantıya](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/provisioning-tab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/provisioning-credentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si** ve **gizli belirteç** adım 6'da açıklandığı gibi Zscaler bir hesabınızın.

6. Elde etmek için **Kiracı URL'si** ve **gizli belirteç**, gitmek **Yönetim > kimlik doğrulama ayarları** Zscaler bir portal kullanıcı arabirimi ve tıklayın **SAML** altında **kimlik doğrulama türü**.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/secret-token-1.png)

    Tıklayarak **SAML yapılandırma** açmak için **yapılandırma SAML** seçenekleri.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/secret-token-2.png)

    Seçin **Enable SCIM-Based sağlama** alınacak **temel URL** ve **taşıyıcı belirteci**, ayarları kaydedin. Kopyalama **temel URL** için **Kiracı URL'si** ve **taşıyıcı belirteci** için **gizli belirteç** Azure portalında.

7. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD Zscaler bir bağlanabilirsiniz. Bağlantı başarısız olursa Zscaler bir hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/test-connection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/notification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Zscaler bir**.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/user-mappings.png)

11. Zscaler bir Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesaplarını Zscaler bir güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/user-attribute-mappings.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Zscaler bir**.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/group-mappings.png)

13. Zscaler bir Azure AD'den eşitlenen grup öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri gruplarında Zscaler bir güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD sağlama hizmeti için Zscaler bir etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/provisioning-status.png)

16. Kullanıcılara ve/veya istediğiniz grupları Zscaler bir sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/save-provisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti üzerinde Zscaler bir Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-03.png
