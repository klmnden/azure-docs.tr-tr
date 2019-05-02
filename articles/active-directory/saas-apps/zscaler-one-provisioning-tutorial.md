---
title: 'Öğretici: Zscaler bir Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını kaldırma Zscaler bir kullanıcı hesapları için Azure Active Directory yapılandırmayı öğrenin.
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
ms.openlocfilehash: 5319b0ac06c4ddf1a7627a4e7fe0bfb2694f79f6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706591"
---
# <a name="tutorial-configure-zscaler-one-for-automatic-user-provisioning"></a>Öğretici: Zscaler bir yapılandırma için otomatik kullanıcı hazırlama

Bu öğreticide, bir Zscaler ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için otomatik olarak sağlamak ve kullanıcıların ve grupların Zscaler bir sağlamasını kaldırmak için gerçekleştirme adımları gösterilmektedir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular hakkında daha fazla bilgi için bkz: [kullanıcı sağlamayı ve Azure Active Directory ile hizmet olarak yazılım-a-(SaaS) uygulamalarına sağlama kaldırmayı otomatikleştirme](../active-directory-saas-app-provisioning.md).
>
> Bu bağlayıcı, şu anda önizleme olarak kullanılabilir. Genel Microsoft Azure için kullanım koşulları Önizleme özellikleri hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, olduğunu varsayar:

* Azure AD kiracısı.
* Zscaler tek bir kiracı.
* Bir kullanıcı hesabında Zscaler bir yönetici izinlerine sahip.

> [!NOTE]
> Azure AD tümleştirmesi sağlama Zscaler bir SCIM API'yi kullanır. Bu API Zscaler bir geliştiriciler için Kurumsal paketi hesaplarıyla kullanılabilir.

## <a name="add-zscaler-one-from-the-azure-marketplace"></a>Azure Market'ten Zscaler bir ekleme

Zscaler bir Azure AD ile sağlama otomatik kullanıcı için yapılandırmadan önce Zscaler bir listenize yönetilen SaaS uygulamalarını Azure Market'te ekleyin.

Zscaler bir marketten eklemek için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com), sol taraftaki gezinti bölmesinde seçin **Azure Active Directory**.

    ![Azure Active Directory simgesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler bir** seçip **Zscaler bir** sonucu panelinden. Uygulama eklemek için seçin **Ekle**.

    ![Zscaler bir sonuç listesinde](common/search-new-app.png)

## <a name="assign-users-to-zscaler-one"></a>Zscaler bir için kullanıcı atama

Azure Active Directory kullanan adlı bir kavram *atamaları* hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcılar veya Azure AD'de bir uygulamaya atanan gruplar eşitlenir.

Yapılandırma ve otomatik kullanıcı sağlamayı etkinleştirmek için önce hangi kullanıcıların veya grupların Azure AD'de Zscaler bir erişmesi gereken karar verin. Bu kullanıcılar veya gruplar için bir Zscaler atamak için yönergeleri izleyin. [kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-one"></a>Kullanıcı için bir Zscaler atama önemli ipuçları

* Tek bir atamanızı öneririz Zscaler bir sağlama yapılandırmasını otomatik kullanıcı test etmek için Azure AD kullanıcısı. Daha sonra ek kullanıcılar veya gruplar atayabilirsiniz.

* Bir kullanıcı için bir Zscaler atadığınızda, geçerli herhangi bir uygulamaya özgü rol ataması iletişim kutusunda kullanılabilir olması durumunda seçin. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configure-automatic-user-provisioning-to-zscaler-one"></a>Zscaler bir için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde Azure AD sağlama hizmeti yapılandırma adımlarında size kılavuzluk eder. Oluşturma, güncelleştirme ve kullanıcılar veya gruplar, Azure AD'de kullanıcı veya grup atamalarını göre bir Zscaler devre dışı bırakmak için kullanın.

> [!TIP]
> SAML tabanlı çoklu oturum açma Zscaler bir için etkinleştirebilirsiniz. Bölümündeki yönergeleri [Zscaler tek tek oturum açma öğretici](zscaler-One-tutorial.md). Bu iki özellik birbirini tamamlar ancak otomatik kullanıcı hazırlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-zscaler-one-in-azure-ad"></a>Azure AD'de bir Zscaler için otomatik kullanıcı sağlamayı yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **kurumsal uygulamalar** > **tüm uygulamaları** > **Zscaler bir**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler bir**.

    ![Uygulamalar listesini Zscaler bir bağlantıya](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Zscaler bir sağlama](./media/zscaler-one-provisioning-tutorial/provisioning-tab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Zscaler bir sağlama modu](./media/zscaler-one-provisioning-tutorial/provisioning-credentials.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, doldurun **Kiracı URL'si** ve **gizli belirteç** adım 6'da açıklandığı gibi Zscaler bir ayarlarını kutularıyla hesap.

6. Kiracı URL'sini ve gizli belirteç edinmek için Git **Yönetim** > **kimlik doğrulama ayarları** Zscaler bir portal kullanıcı Arabirimi içinde. Altında **kimlik doğrulama türü**seçin **SAML**.

    ![Zscaler bir kimlik doğrulama ayarları](./media/zscaler-one-provisioning-tutorial/secret-token-1.png)

    a. Seçin **SAML yapılandırma** açmak için **SAML yapılandırma** seçenekleri.

    ![Zscaler bir SAML yapılandırma](./media/zscaler-one-provisioning-tutorial/secret-token-2.png)

    b. Seçin **Enable SCIM-Based sağlama** ayarlarını almak için **temel URL** ve **taşıyıcı belirteci**. Ayarları kaydedin. Kopyalama **temel URL** ayarını **Kiracı URL'si** Azure portalında. Kopyalama **taşıyıcı belirteci** ayarını **gizli belirteç** Azure portalında.

7. Adım 5'te gösterilen kutuları doldurduktan sonra seçin **Test Bağlantısı** Azure'un emin olmak için AD Zscaler bir bağlanabilirsiniz. Bağlantı başarısız olursa Zscaler bir hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Zscaler bir Test Bağlantısı](./media/zscaler-one-provisioning-tutorial/test-connection.png)

8. İçinde **bildirim e-posta** kutusunda kişinin e-posta adresi girin veya sağlama hata bildirimleri almak için Grup. Seçin **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusu.

    ![Zscaler bir bildirim e-postası](./media/zscaler-one-provisioning-tutorial/notification.png)

9. **Kaydet**’i seçin.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları Zscaler bir**.

    ![Zscaler bir kullanıcı eşitleme](./media/zscaler-one-provisioning-tutorial/user-mappings.png)

11. Zscaler bir Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, kullanıcı hesaplarını Zscaler bir güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için seçmeniz **Kaydet**.

    ![Zscaler bir eşleşen kullanıcı öznitelikleri](./media/zscaler-one-provisioning-tutorial/user-attribute-mappings.png)

12. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory gruplarına Zscaler bir**.

    ![Zscaler bir grup eşitleme](./media/zscaler-one-provisioning-tutorial/group-mappings.png)

13. Zscaler bir Azure AD'den eşitlenen grup öznitelikleri gözden **öznitelik eşlemelerini** bölümü. Seçilen öznitelikler **eşleşen** özellikleri gruplarında Zscaler bir güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için seçmeniz **Kaydet**.

    ![Zscaler bir eşleşen grup öznitelikleri](./media/zscaler-one-provisioning-tutorial/group-attribute-mappings.png)

14. Kapsam belirleme filtrelerini yapılandırmak için yönergeleri izleyin. [kapsam belirleme filtresi öğretici](./../active-directory-saas-scoping-filters.md).

15. Azure AD içinde sağlama hizmeti için bir Zscaler etkinleştirmek için **ayarları** bölümünde **sağlama durumu** için **üzerinde**.

    ![Zscaler bir sağlama durumu](./media/zscaler-one-provisioning-tutorial/provisioning-status.png)

16. Zscaler bir sağlamak için kullanıcıları veya istediğiniz grupları tanımlayın. İçinde **ayarları** bölümünde, istediğiniz değerleri seçin **kapsam**.

    ![Zscaler bir kapsam](./media/zscaler-one-provisioning-tutorial/scoping.png)

17. Sağlama için hazır olduğunuzda **Kaydet**.

    ![Zscaler kaydedin](./media/zscaler-one-provisioning-tutorial/save-provisioning.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme daha sonraki eşitlemeler gerçekleştirmek için daha uzun sürer. Azure AD sağlama hizmeti çalıştırdığı sürece, yaklaşık 40 dakikada oluşur. 

Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama etkinliği raporunu için bağlantıları izleyin. Rapor hizmette Zscaler bir sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz: [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı, kurumsal uygulamalar için hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-03.png
