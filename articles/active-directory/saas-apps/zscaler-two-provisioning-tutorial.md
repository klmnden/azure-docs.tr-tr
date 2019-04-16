---
title: 'Öğretici: Zscaler iki Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Bu öğreticide, otomatik olarak sağlama ve sağlamasını kaldırma Zscaler iki kullanıcı hesaplarını Azure Active Directory'yi yapılandırma öğreneceksiniz.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0a250fcd-6ca1-47c2-a780-7a6278186a69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 837014fde6962f64d7da023a001a4c41089a0097
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578285"
---
# <a name="tutorial-configure-zscaler-two-for-automatic-user-provisioning"></a>Öğretici: Zscaler iki yapılandırma için otomatik kullanıcı hazırlama

Bu öğreticide, Azure otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zscaler iki sağlamasını kaldırmak için Active Directory'yi (Azure AD) yapılandırma öğreneceksiniz.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Hangi bu hizmet hakkında önemli ayrıntıları için yapar ve nasıl çalıştığı ve sık sorulan soruların yanıtları görmek [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).
>
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Azure kullanım koşullarını genel Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen adımları tamamlamak için aşağıdakiler gerekir:

* Azure AD kiracısı.
* Zscaler iki Kiracı için.
* Bir kullanıcı hesabında Zscaler iki yönetici izinlerine sahip.

> [!NOTE]
> Azure AD sağlama tümleştirme Zscaler iki SCIM API'yi, Kurumsal hesaplar için kullanılabilir kullanır.

## <a name="add-zscaler-two-from-the-gallery"></a>Zscaler iki Galeriden Ekle

Zscaler iki otomatik kullanıcı hazırlama ile Azure AD için yapılandırmadan önce Zscaler iki Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

![Azure Active Directory'yi seçin](common/select-azuread.png)

Git **kurumsal uygulamalar** seçip **tüm uygulamaları**:

![Kurumsal uygulamalar](common/enterprise-applications.png)

Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

![Yeni uygulama seçme](common/add-new-app.png)

Arama kutusuna **Zscaler iki**. Seçin **Zscaler iki** sonuçları ve ardından **Ekle**.

![Sonuçları listesi](common/search-new-app.png)

## <a name="assign-users-to-zscaler-two"></a>Zscaler iki için kullanıcı atama

Azure AD kullanıcıları, kullanmadan önce seçili uygulamalar için erişim atanması gerekir. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcılar veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı sağlamayı etkinleştirin önce hangi kullanıcılara ve/veya Azure AD'de grupları Zscaler iki erişmesi gereken karar vermeniz gerekir. Karar verdikten sonra bu kullanıcılar ve grupları için iki Zscaler yönergelerini takip ederek atayabilirsiniz [kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-two"></a>Kullanıcı için iki Zscaler atama önemli ipuçları

* Tek bir ilk atamanızı öneririz Zscaler iki sağlama yapılandırmasını otomatik kullanıcı test etmek için Azure AD kullanıcısı. Daha sonra başka kullanıcı ve grupları atayabilirsiniz.

* Bir kullanıcı için iki Zscaler atadığınızda, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rol seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="set-up-automatic-user-provisioning"></a>Otomatik kullanıcı sağlamayı ayarlama

Bu bölümde, oluşturmak, güncelleştirmek ve Zscaler Azure AD'de kullanıcı ve Grup atamalarını göre iki içindeki kullanıcılar ve gruplar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size yol gösterir.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Zscaler iki etkinleştirmek isteyebilirsiniz. Yönergeleri izlerseniz [Zscaler iki tek oturum açma öğretici](zscaler-two-tutorial.md). Çoklu oturum açmayı otomatik kullanıcı hazırlama bağımsız olarak yapılandırılabilir, ancak iki özellik birbirini tamamlar.

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar** > **tüm uygulamaları** > **Zscaler iki**:

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler iki**:

    ![Uygulamalar listesi](common/all-applications.png)

3. Seçin **sağlama** sekmesinde:

    ![Zscaler iki sağlama](./media/zscaler-two-provisioning-tutorial/provisioning-tab.png)

4. Ayarlama **hazırlama modu** için **otomatik**:

    ![Sağlama modunu ayarlayın](./media/zscaler-two-provisioning-tutorial/provisioning-credentials.png)

5. İçinde **yönetici kimlik bilgileri** bölümünde, girin **Kiracı URL'si** ve **gizli belirteç** Zscaler iki hesabınızın sonraki adımda açıklandığı gibi.

6. Almak için **Kiracı URL'si** ve **gizli belirteç**Git **Yönetim** > **kimlik doğrulama ayarları** Zscaler iki içinde Portal ve select **SAML** altında **kimlik doğrulama türü**:

    ![Zscaler iki kimlik doğrulama ayarları](./media/zscaler-two-provisioning-tutorial/secret-token-1.png)

    Seçin **SAML yapılandırma** açmak için **SAML yapılandırma** penceresi:

    ![SAML penceresini yapılandırma](./media/zscaler-two-provisioning-tutorial/secret-token-2.png)

    Seçin **Enable SCIM-Based sağlama** ve kopyalama **temel URL** ve **taşıyıcı belirteci**ve ardından ayarları kaydedin. Azure portalında yapıştırın **temel URL** içine **Kiracı URL'si** kutusu ve **taşıyıcı belirteci** içine **gizli belirteç** kutusu.

7. Değerleri girdikten sonra **Kiracı URL'si** ve **gizli belirteç** kutularında seçme **Test Bağlantısı** Azure AD için iki Zscaler bağlanabilir emin olmak için. Bağlantı başarısız olursa Zscaler iki hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bağlantıyı sınama](./media/zscaler-two-provisioning-tutorial/test-connection.png)

8. İçinde **bildirim e-posta** kutusunda, bir kişi veya grup sağlama hatası bildirimlerin gönderileceği e-posta adresini girin. Seçin **bir hata oluştuğunda e-posta bildirimi gönder**:

    ![Bildirim e-posta kurma](./media/zscaler-two-provisioning-tutorial/notification.png)

9. **Kaydet**’i seçin.

10. İçinde **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları ZscalerTwo**:

    ![Azure AD kullanıcıları eşitleme](./media/zscaler-two-provisioning-tutorial/user-mappings.png)

11. Zscaler iki için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesaplarını Zscaler iki güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

    ![Öznitelik Eşlemeleri](./media/zscaler-two-provisioning-tutorial/user-attribute-mappings.png)

12. İçinde **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına ZscalerTwo**:

    ![Azure AD grupları Eşitle](./media/zscaler-two-provisioning-tutorial/group-mappings.png)

13. Zscaler iki için Azure AD'den eşitlenen grup öznitelikleri gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri gruplarında Zscaler iki güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

    ![Öznitelik Eşlemeleri](./media/zscaler-two-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için yönergeleri bakın [Scoping filtre öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD sağlama hizmeti için Zscaler iki etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü:

    ![Sağlama Durumu](./media/zscaler-two-provisioning-tutorial/provisioning-status.png)

16. Kullanıcılara ve/veya olmasını istediğiniz grupları Zscaler iki sağlamak için altında istediğiniz değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü:

    ![Kapsam değerleri](./media/zscaler-two-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda **Kaydet**:

    ![Kaydet'i seçin](./media/zscaler-two-provisioning-tutorial/save-provisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve tanımlı gruplar altında **kapsam** içinde **ayarları** bölümü. İlk eşitleme, Azure AD sağlama hizmeti çalışıyor sürece 40 dakikada hakkında oluşan sonraki eşitlemeler uzun sürer. İlerleme durumunu izleyebilirsiniz **eşitleme ayrıntıları** bölümü. Sağlama hizmeti üzerinde Zscaler iki Azure AD tarafından gerçekleştirilen tüm eylemler açıklar bir sağlama etkinliği raporunu için bağlantılar da izleyebilirsiniz.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz: [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-03.png
