---
title: 'Öğretici: Zscaler ZSCloud Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Bu öğreticide, otomatik olarak sağlamak ve kullanıcı hesaplarına Zscaler ZSCloud sağlamasını kaldırmak için Azure Active Directory'yi yapılandırma öğreneceksiniz.
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
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 99c94792f48db7a932e670f05216bcea0e90a27c
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672801"
---
# <a name="tutorial-configure-zscaler-zscloud-for-automatic-user-provisioning"></a>Öğretici: Zscaler ZSCloud otomatik kullanıcı hazırlama için yapılandırma

Bu öğreticide, Azure otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zscaler ZSCloud sağlamasını kaldırmak için Active Directory'yi (Azure AD) yapılandırma öğreneceksiniz.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Hangi bu hizmet hakkında önemli ayrıntıları için yapar ve nasıl çalıştığı ve sık sorulan soruların yanıtları görmek [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).
>
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Azure kullanım koşullarını genel Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen adımları tamamlamak için aşağıdakiler gerekir:

* Azure AD kiracısı.
* Zscaler ZSCloud Kiracı.
* Zscaler ZSCloud yönetici izinlerine sahip bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, kuruluş hesapları için kullanılabilir Zscaler ile ilgili ZSCloud SCIM API kullanır.

## <a name="add-zscaler-zscloud-from-the-gallery"></a>Zscaler ZSCloud Galeriden Ekle

Zscaler ZSCloud otomatik kullanıcı hazırlama ile Azure AD için yapılandırmadan önce listenize yönetilen SaaS uygulamasını Azure AD uygulama galerisinden Zscaler ZSCloud eklemeniz gerekir.

İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

![Azure Active Directory'yi seçin](common/select-azuread.png)

Git **kurumsal uygulamalar** seçip **tüm uygulamaları**:

![Kurumsal uygulamalar](common/enterprise-applications.png)

Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

![Yeni uygulama seçme](common/add-new-app.png)

Arama kutusuna **Zscaler ZSCloud**. Seçin **Zscaler ZSCloud** sonuçları ve ardından **Ekle**.

![Sonuçları listesi](common/search-new-app.png)

## <a name="assign-users-to-zscaler-zscloud"></a>Zscaler ZSCloud için kullanıcı atama

Azure AD kullanıcıları, kullanmadan önce seçili uygulamalar için erişim atanması gerekir. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcılar veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı sağlamayı etkinleştirin önce hangi kullanıcılara ve/veya Azure AD'de grupları Zscaler ZSCloud erişmesi karar vermeniz gerekir. Karar verdikten sonra bu kullanıcılar ve gruplar için Zscaler ZSCloud yönergelerini takip ederek atayabilirsiniz [kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-zscloud"></a>Zscaler ZSCloud için kullanıcı atama önemli ipuçları

* Tek bir ilk atamanızı öneririz Zscaler ZSCloud, sağlama yapılandırmasını otomatik kullanıcı test etmek için Azure AD kullanıcısı. Daha sonra başka kullanıcı ve grupları atayabilirsiniz.

* Bir kullanıcı için Zscaler ZSCloud atadığınızda, (varsa) geçerli bir uygulamaya özgü rol ataması iletişim kutusunda seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="set-up-automatic-user-provisioning"></a>Otomatik kullanıcı sağlamayı ayarlama

Bu bölümde, oluşturmak, güncelleştirmek ve kullanıcılar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size yol gösterir ve Azure AD'de kullanıcı ve Grup atamalarını Zscaler ZSCloud gruplara göre.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Zscaler ZSCloud etkinleştirmek isteyebilirsiniz. Yönergeleri izlerseniz [oturum açma Zscaler ZSCloud tek öğretici](zscaler-zsCloud-tutorial.md). Çoklu oturum açmayı otomatik kullanıcı hazırlama bağımsız olarak yapılandırılabilir, ancak iki özellik birbirini tamamlar.

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar** > **tüm uygulamaları** > **Zscaler ZSCloud**:

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler ZSCloud**:

    ![Uygulamalar listesi](common/all-applications.png)

3. Seçin **sağlama** sekmesinde:

    ![Zscaler ZSCloud sağlama](./media/zscaler-zscloud-provisioning-tutorial/provisioningtab.png)

4. Ayarlama **hazırlama modu** için **otomatik**:

    ![Sağlama modunu ayarlayın](./media/zscaler-zscloud-provisioning-tutorial/provisioningcredentials.png)

5. İçinde **yönetici kimlik bilgileri** bölümünde, girin **Kiracı URL'si** ve **gizli belirteç** Zscaler ZSCloud hesabınızın, sonraki adımda açıklandığı gibi.

6. Alınacak **Kiracı URL'si** ve **belirteci gizli anahtarı**gidin **Yönetim** > **kimlik doğrulama ayarları** Zscaler içinde Portal ve select ZSCloud **SAML** altında **kimlik doğrulama türü**:

    ![Zscaler ZSCloud kimlik doğrulama ayarları](./media/zscaler-zscloud-provisioning-tutorial/secrettoken1.png)

    Seçin **SAML yapılandırma** açmak için **SAML yapılandırma** penceresi:

    ![SAML penceresini yapılandırma](./media/zscaler-zscloud-provisioning-tutorial/secrettoken2.png)

    Seçin **Enable SCIM-Based sağlama** ve kopyalama **temel URL** ve **taşıyıcı belirteci**ve ardından ayarları kaydedin. Azure portalında yapıştırın **temel URL** içine **Kiracı URL'si** kutusu ve **taşıyıcı belirteci** içine **gizli belirteç** kutusu.

7. Değerleri girdikten sonra **Kiracı URL'si** ve **gizli belirteç** kutularında seçme **Test Bağlantısı** Azure AD için Zscaler ZSCloud bağlanabilir emin olmak için. Bağlantı başarısız olursa Zscaler ZSCloud hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bağlantıyı sınama](./media/zscaler-zscloud-provisioning-tutorial/testconnection.png)

8. İçinde **bildirim e-posta** kutusunda, bir kişi veya grup sağlama hatası bildirimlerin gönderileceği e-posta adresini girin. Seçin **bir hata oluştuğunda e-posta bildirimi gönder**:

    ![Bildirim e-posta kurma](./media/zscaler-zscloud-provisioning-tutorial/Notification.png)

9. **Kaydet**’i seçin.

10. İçinde **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları ZscalerZSCloud**:

    ![Azure AD kullanıcıları eşitleme](./media/zscaler-zscloud-provisioning-tutorial/usermappings.png)

11. Zscaler ZSCloud içinde için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zscaler ZSCloud kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

    ![Öznitelik eşlemeleri](./media/zscaler-zscloud-provisioning-tutorial/userattributemappings.png)

12. İçinde **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına ZscalerZSCloud**:

    ![Azure AD grupları Eşitle](./media/zscaler-zscloud-provisioning-tutorial/groupmappings.png)

13. Zscaler ZSCloud içinde için Azure AD'den eşitlenen grup öznitelikleri gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri Zscaler ZSCloud gruplarında güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

    ![Öznitelik eşlemeleri](./media/zscaler-zscloud-provisioning-tutorial/groupattributemappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için yönergeleri bakın [Scoping filtre öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD sağlama hizmeti için Zscaler ZSCloud etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü:

    ![Sağlama durumu](./media/zscaler-zscloud-provisioning-tutorial/provisioningstatus.png)

16. Kullanıcılara ve/veya olmasını istediğiniz grupları Zscaler ZSCloud sağlamak için altında istediğiniz değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü:

    ![Kapsam değerleri](./media/zscaler-zscloud-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda **Kaydet**:

    ![Kaydet'i seçin](./media/zscaler-zscloud-provisioning-tutorial/saveprovisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve tanımlı gruplar altında **kapsam** içinde **ayarları** bölümü. İlk eşitleme, Azure AD sağlama hizmeti çalışıyor sürece 40 dakikada hakkında oluşan sonraki eşitlemeler uzun sürer. İlerleme durumunu izleyebilirsiniz **eşitleme ayrıntıları** bölümü. Sağlama hizmeti Zscaler ZSCloud üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklar bir sağlama etkinliği raporunu için bağlantılar da izleyebilirsiniz.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz: [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-03.png
