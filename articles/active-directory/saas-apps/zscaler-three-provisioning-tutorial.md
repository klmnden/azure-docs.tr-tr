---
title: 'Öğretici: Zscaler üç Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Bu öğreticide, otomatik olarak sağlamak ve kullanıcı hesaplarına Zscaler üç sağlamasını kaldırmak için Azure Active Directory'yi yapılandırma öğreneceksiniz.
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
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: d96444984c503da68ccbda3aef9fea0ede5c7ff9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "67049050"
---
# <a name="tutorial-configure-zscaler-three-for-automatic-user-provisioning"></a>Öğretici: Zscaler üç yapılandırmak için otomatik kullanıcı hazırlama

Bu öğreticide, Azure otomatik olarak sağlamak ve kullanıcılara ve/veya gruplara Zscaler üç sağlamasını kaldırmak için Active Directory'yi (Azure AD) yapılandırma öğreneceksiniz.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Hangi bu hizmet hakkında önemli ayrıntıları için yapar ve nasıl çalıştığı ve sık sorulan soruların yanıtları görmek [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).
>
> Bu bağlayıcı, şu anda genel Önizleme aşamasındadır. Azure kullanım koşullarını genel Önizleme özellikleri hakkında daha fazla bilgi için bkz. [ek kullanım koşulları, Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen adımları tamamlamak için aşağıdakiler gerekir:

* Azure AD kiracısı.
* Üç Zscaler Kiracı.
* Yönetici izinlerine sahip bir kullanıcı hesabı Zscaler üç içinde.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, kuruluş hesapları için kullanılabilir Zscaler ile ilgili ZSCloud SCIM API kullanır.

## <a name="adding-zscaler-three-from-the-gallery"></a>Zscaler üç galeri ekleme

Zscaler üç otomatik kullanıcı hazırlama ile Azure AD için yapılandırmadan önce Zscaler üç Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**:

![Azure Active Directory'yi seçin](common/select-azuread.png)

Git **kurumsal uygulamalar** seçip **tüm uygulamaları**:

![Kurumsal uygulamalar](common/enterprise-applications.png)

Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki:

![Yeni uygulama seçme](common/add-new-app.png)

Arama kutusuna **Zscaler üç**. Seçin **Zscaler üç** sonuçları ve ardından **Ekle**.

![Sonuçları listesi](common/search-new-app.png)

## <a name="assign-users-to-zscaler-three"></a>Zscaler üç için kullanıcı atama

Azure AD kullanıcıları, kullanmadan önce seçili uygulamalar için erişim atanması gerekir. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcılar veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı sağlamayı etkinleştirin önce hangi kullanıcılara ve/veya Azure AD'de grupları Zscaler üç erişmesi gereken karar vermeniz gerekir. Karar verdikten sonra bu kullanıcılar ve grupları için üç Zscaler yönergelerini takip ederek atayabilirsiniz [kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-three"></a>Zscaler üç için kullanıcı atama önemli ipuçları

* İlk tek bir Ata önerilen Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için üç Zscaler. Daha sonra başka kullanıcı ve grupları atayabilirsiniz.

* Bir kullanıcı için üç Zscaler atadığınızda, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rol seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="set-up-automatic-user-provisioning"></a>Otomatik kullanıcı sağlamayı ayarlama

Bu bölümde, oluşturmak, güncelleştirmek ve Zscaler Azure AD'de kullanıcı ve Grup atamalarını göre üç içindeki kullanıcılar ve gruplar devre dışı bırakmak için sağlama hizmetini Azure AD'yi yapılandırma adımlarında size yol gösterir.

> [!TIP]
> SAML tabanlı çoklu oturum açma için Zscaler üç etkinleştirmek isteyebilirsiniz. Yönergeleri izlerseniz [Zscaler üç tek oturum açma öğretici](zscaler-three-tutorial.md). Çoklu oturum açmayı otomatik kullanıcı hazırlama bağımsız olarak yapılandırılabilir, ancak iki özellik birbirini tamamlar.

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar** > **tüm uygulamaları** > **Zscaler üç**:

    ![Kurumsal uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler üç**:

    ![Uygulamalar listesi](common/all-applications.png)

3. Seçin **sağlama** sekmesinde:

    ![Zscaler üç sağlama](./media/zscaler-three-provisioning-tutorial/provisioning-tab.png)

4. Ayarlama **hazırlama modu** için **otomatik**:

    ![Sağlama modunu ayarlayın](./media/zscaler-three-provisioning-tutorial/provisioning-credentials.png)

5. İçinde **yönetici kimlik bilgileri** bölümünde, girin **Kiracı URL'si** ve **gizli belirteç** Zscaler üç hesabınızın sonraki adımda açıklandığı gibi.

6. Alınacak **Kiracı URL'si** ve **belirteci gizli anahtarı**gidin **Yönetim** > **kimlik doğrulama ayarları** Zscaler içinde Üç seçin ve portal **SAML** altında **kimlik doğrulama türü**:

    ![Zscaler üç kimlik doğrulama ayarları](./media/zscaler-three-provisioning-tutorial/secret-token-1.png)

    Seçin **SAML yapılandırma** açmak için **SAML yapılandırma** penceresi:

    ![SAML penceresini yapılandırma](./media/zscaler-three-provisioning-tutorial/secret-token-2.png)

    Seçin **Enable SCIM-Based sağlama** ve kopyalama **temel URL** ve **taşıyıcı belirteci**ve ardından ayarları kaydedin. Azure portalında yapıştırın **temel URL** içine **Kiracı URL'si** kutusu ve **taşıyıcı belirteci** içine **gizli belirteç** kutusu.

7. Değerleri girdikten sonra **Kiracı URL'si** ve **gizli belirteç** kutularında seçme **Test Bağlantısı** Azure AD'ye üç Zscaler için bağlanabilir emin olmak için. Bağlantı başarısız olursa Zscaler üç hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bağlantıyı sınama](./media/zscaler-three-provisioning-tutorial/test-connection.png)

8. İçinde **bildirim e-posta** kutusunda, bir kişi veya grup sağlama hatası bildirimlerin gönderileceği e-posta adresini girin. Seçin **bir hata oluştuğunda e-posta bildirimi gönder**:

    ![Bildirim e-posta kurma](./media/zscaler-three-provisioning-tutorial/notification.png)

9. **Kaydet**’i seçin.

10. İçinde **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları ZscalerThree**:

    ![Azure AD kullanıcıları eşitleme](./media/zscaler-three-provisioning-tutorial/user-mappings.png)

11. Zscaler üç için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesaplarını Zscaler üç güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

    ![Öznitelik eşlemeleri](./media/zscaler-three-provisioning-tutorial/user-attribute-mappings.png)

12. İçinde **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına ZscalerThree**:

    ![Azure AD grupları Eşitle](./media/zscaler-three-provisioning-tutorial/group-mappings.png)

13. Zscaler üç için Azure AD'den eşitlenen grup öznitelikleri gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri gruplarında Zscaler üç güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

    ![Öznitelik eşlemeleri](./media/zscaler-three-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için yönergeleri bakın [Scoping filtre öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD sağlama hizmeti için Zscaler üç etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü:

    ![Sağlama durumu](./media/zscaler-three-provisioning-tutorial/provisioning-status.png)

16. Kullanıcılara ve/veya olmasını istediğiniz grupları Zscaler üç sağlamak için altında istediğiniz değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü:

    ![Kapsam değerleri](./media/zscaler-three-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda **Kaydet**:

    ![Kaydet'i seçin](./media/zscaler-three-provisioning-tutorial/save-provisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve tanımlı gruplar altında **kapsam** içinde **ayarları** bölümü. İlk eşitleme, Azure AD sağlama hizmeti çalışıyor sürece 40 dakikada hakkında oluşan sonraki eşitlemeler uzun sürer. İlerleme durumunu izleyebilirsiniz **eşitleme ayrıntıları** bölümü. Sağlama hizmeti üzerinde Zscaler üç Azure AD tarafından gerçekleştirilen tüm eylemler açıklar bir sağlama etkinliği raporunu için bağlantılar da izleyebilirsiniz.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz: [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-three-provisioning-tutorial/tutorial-general-03.png
