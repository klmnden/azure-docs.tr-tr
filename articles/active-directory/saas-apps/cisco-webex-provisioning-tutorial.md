---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Cisco yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını Cisco Webex kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: v-wingf
ms.openlocfilehash: 76a83ef4f647dcf7d79218cb281f1f976b292870
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42061085"
---
# <a name="tutorial-configure-cisco-webex-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Cisco Webex yapılandırın.


Bu öğreticinin amacı, Cisco Webex ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için sağlama ve sağlamasını Cisco Webex kullanıcılara otomatik olarak gerçekleştirilecek adımları göstermektir.


> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

*   Azure AD kiracısı
*   Bir Cisco Webex Kiracı
*   Cisco Webex yönetici izinlerine sahip bir kullanıcı hesabı


> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [Cisco Webex Webservice](https://developer.webex.com/getting-started.html), Cisco Webex takımlar için kullanılabildiği.

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex galeri ekleme
Azure AD ile otomatik kullanıcı hazırlama için Cisco Webex yapılandırmadan önce Cisco Webex Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Cisco Webex eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, üzerinde sol gezinti bölmesinde, tıklayarak **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölümü][2]

3. Cisco Webex eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cisco Webex**.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/AppSearch.png)

5. Sonuçlar panelinde seçin **Cisco Webex**ve ardından **Ekle** düğmesini Cisco Webex SaaS uygulamaları listenize ekleyin.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/AppSearchResults.png)

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-webex"></a>Cisco Webex için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcıların Azure AD'de Cisco Webex erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar için Cisco Webex atayabilirsiniz:

*   [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-webex"></a>Cisco Webex için kullanıcı atama önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Cisco Webex atanır. Ek kullanıcılar daha sonra atanabilir.

*   Cisco Webex için kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-cisco-webex"></a>Cisco Webex için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcıların Azure AD'de kullanıcı atamaları temel alınarak Cisco Webex devre dışı bırakmak için Azure AD sağlama hizmeti yapılandırmak için gereken adımları size kılavuzluk eder.


### <a name="to-configure-automatic-user-provisioning-for-cisco-webex-in-azure-ad"></a>Azure AD'de Cisco Webex için otomatik kullanıcı hazırlama yapılandırmak için:


1. Oturum [Azure portalında](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. Cisco Webex SaaS uygulamaları listesinden seçin.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/Successcenter2.png)

3. Seçin **sağlama** sekmesi.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si**, ve **gizli belirteç** Cisco Webex'in hesabının.

    *   İçinde **Kiracı URL'si** alan, Cisco Webex SCIM API URL'si biçimi alır kiracınız için doldurma `https://api.webex.com/v1/scim/[Tenant ID]/`burada `[Tenant ID]` 6. adımda açıklandığı gibi bir alfasayısal bir dize özelliğidir.

    *   İçinde **gizli belirteç** alan, 6. adımda açıklandığı gibi gizli belirteç doldurun.

1. **Kiracı kimliği** ve **gizli belirteç** hesabı için Cisco Webex açılarak bulunabilir [Cisco Webex Geliştirici sitesi](https://developer.webex.com/) Yönetici hesabınızla. -Bir kez günlüğe kaydedilir
    * Git [Başlarken sayfası](https://developer.webex.com/getting-started.html)
    * Ekranı aşağı kaydırarak [kimlik doğrulaması bölümü](https://developer.webex.com/getting-started.html#authentication)
    ![Cisco Webex kimlik doğrulama belirteci](./media/cisco-webex-provisioning-tutorial/SecretToken.png)
    * Alfasayısal dize kutusunda, **gizli belirteç**. Bu belirteç Panoya Kopyala
    * Git [alma My kendi Ayrıntıları sayfası](https://developer.webex.com/endpoint-people-me-get.html)
        * Test modu açık olduğundan emin olun
        * Ardından bir boşluk "Bearer" sözcüğünü yazın ve sonra gizli belirteç yetkilendirme alanına yapıştırabilirsiniz ![Cisco Webex kimlik doğrulama belirteci](./media/cisco-webex-provisioning-tutorial/GetMyDetails.png)
        * Çalıştır'a tıklayın
    * Yanıt metni sağa içinde **Kiracı kimliği** "Orgıd" görünür:

    ```json
    {
        "id": "(...)",
        "emails": [
            "admin.user@contoso.com"
        ],
        "displayName": "John Smith",
        "nickName": "John",
        "orgId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        (...)
    }
    ```

1. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure sağlamak için Cisco Webex AD bağlanabilirsiniz. Bağlantı başarısız olursa, Cisco Webex hesabının yönetici izinlerine sahip olun ve yeniden deneyin.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/TestConnection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/EmailNotification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için Cisco Webex**.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/UserMapping.png)

11. Cisco Webex içinde için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, Cisco Webex kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/UserMappingAttributes.png)

12. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../active-directory-saas-scoping-filters.md).

13. Azure AD sağlama hizmeti için Cisco Webex etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/ProvisioningStatus.png)

14. Kullanıcılara ve/veya istediğiniz grupları Cisco Webex sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/SyncScope.png)

15. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Cisco Webex sağlama](./media/cisco-webex-provisioning-tutorial/Save.png)


Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Cisco Webex hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Cisco Webex şu anda Cisco'nun erken alan test etme (EFT) aşamasındadır. Daha fazla bilgi için lütfen başvurun [Cisco'nun Destek ekibine](https://www.webex.co.in/support/support-overview.html). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_03.png
