---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Cisco Spark yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve Cisco Spark kullanıcı hesaplarına sağlanmasını için Azure Active Directory yapılandırmayı öğrenin.
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
ms.openlocfilehash: 74907693270e6cd340d3b34585a80077aa87f0f7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059182"
---
# <a name="tutorial-configure-cisco-spark-for-automatic-user-provisioning"></a>Öğretici: Cisco Spark otomatik kullanıcı sağlamayı yapılandırın


Bu öğreticinin amacı otomatik olarak sağlamak ve Cisco Spark kullanıcılara sağlanmasını Cisco Spark ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için gerçekleştirilmesi için adımları göstermektir.


> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

*   Azure AD kiracısı
*   Cisco Spark Kiracı
*   Cisco Spark yönetici izinlerine sahip bir kullanıcı hesabı


> [!NOTE]
> Azure AD tümleştirme sağlama dayanan [Cisco Spark Webservice](https://developer.webex.com/getting-started.html), Cisco Spark takıma kullanılabilir olduğu.

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark Galeriden ekleme
Azure AD ile otomatik kullanıcı sağlamayı Cisco Spark yapılandırmadan önce Cisco Spark Azure AD uygulama Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Cisco Spark Azure AD uygulama Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panelde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölümü][2]

3. Cisco Spark eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cisco Spark**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/AppSearch.png)

5. Sonuçlar panelinde seçin **Cisco Spark**ve ardından **Ekle** Cisco Spark SaaS uygulamaları listenize eklemek için düğmeyi.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/AppSearchResults.png)

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-spark"></a>Cisco Spark kullanıcılar atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik kullanıcı sağlamayı bağlamında, yalnızca kullanıcı gruplarına ve/veya "Azure AD'de bir uygulamaya atanmış" eşitlenir.

Yapılandırma ve otomatik kullanıcı sağlamayı etkinleştirmeden önce hangi kullanıcıların Azure AD'de Cisco Spark erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar için Cisco Spark atayabilirsiniz:

*   [Bir kullanıcı veya grup için bir kuruluş uygulama atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-spark"></a>Cisco Spark kullanıcılara atamak için önemli ipuçları

*   Önerilir tek bir yapılandırma sağlama otomatik kullanıcı test etmek için Cisco Spark Azure AD kullanıcısının atanır. Ek kullanıcılar daha sonra atanabilir.

*   Bir kullanıcı için Cisco Spark atarken atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rol seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-cisco-spark"></a>Cisco Spark otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve Cisco Azure AD'de kullanıcı atamaları temel alınarak Spark kullanıcılar devre dışı bırakmak için hizmet sağlama Azure AD yapılandırma adımlarını size kılavuzluk eder.


### <a name="to-configure-automatic-user-provisioning-for-cisco-spark-in-azure-ad"></a>Azure AD'de için Cisco Spark otomatik kullanıcı sağlamayı yapılandırmak için:


1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları**.

2. Cisco Spark SaaS uygulamaları listesinden seçin.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/Successcenter2.png)

3. Seçin **sağlama** sekmesi.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **sağlama modunda** için **otomatik**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** bölümü, giriş **Kiracı URL**, ve **gizli belirteci** Cisco Spark hesabının.

    *   İçinde **Kiracı URL** alan, Cisco Spark SCIM'yi API URL'si biçimini alır kiracınız için doldurmak `https://api.ciscospark.com/v1/scim/[Tenant ID]/`, burada `[Tenant ID]` 6. adımda açıklandığı gibi alfasayısal bir dize değil.

    *   İçinde **gizli belirteci** alan, 6. adımda açıklandığı gibi gizli belirteci doldurun.

1. **Kiracı kimliği** ve **gizli belirteci** açarak hesabı bulunabilir, Cisco Spark için [Cisco Spark Geliştirici site](https://developer.webex.com/) Yönetici hesabınızla. -Bir kez günlüğe
    * Git [Başlarken sayfa](https://developer.webex.com/getting-started.html)
    * Ekranı aşağı kaydırarak [kimlik doğrulaması bölümü](https://developer.webex.com/getting-started.html#authentication)
    ![Cisco Spark kimlik doğrulama belirteci](./media/cisco-spark-provisioning-tutorial/SecretToken.png)
    * Kutusunda alfasayısal bir dizedir, **gizli belirteci**. Bu belirteç Panoya Kopyala
    * Git [alma My kendi Ayrıntılar sayfası](https://developer.webex.com/endpoint-people-me-get.html)
        * Test modu açık olduğundan emin olun
        * "Bearer" boşlukla sözcüğünün yazın ve sonra gizli belirteci yetkilendirme alanına yapıştırabilirsiniz ![Cisco Spark kimlik doğrulama belirteci](./media/cisco-spark-provisioning-tutorial/GetMyDetails.png)
        * Çalıştır'ı tıklatın
    * Yanıt metni sağa içinde **Kiracı kimliği** "Orgıd" görüntülenir:

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

1. Adım 5'te gösterilen alanları doldurma sırasında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Cisco Spark bağlanabilir. Bağlantı başarısız olursa, Cisco Spark hesabınız yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/TestConnection.png)

8. İçinde **bildirim e-posta** alan, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresini girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/EmailNotification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Cisco Spark**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/UserMapping.png)

11. Cisco Spark Azure AD'den eşitlenir kullanıcı öznitelikleri gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için kullanıcı hesapları Cisco Spark eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/UserMappingAttributes.png)

12. Kapsam filtrelerini yapılandırmak için sağlanan aşağıdaki yönergelerine bakın [Scoping filtre Öğreticisi](../active-directory-saas-scoping-filters.md).

13. Cisco Spark hizmeti sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/ProvisioningStatus.png)

14. Kullanıcılara ve/veya istediğiniz grupları Cisco Spark sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/SyncScope.png)

15. Sağlamak için hazır olduğunuzda tıklatın **kaydetmek**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/Save.png)


Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlanan gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet sağlama Azure AD çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Cisco Spark hizmette sağlama Azure AD tarafından gerçekleştirilen tüm eylemler açıklanmaktadır Etkinlik Raporu sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Sonraki adımlar

* [Günlüklerini gözden geçirin ve etkinlik sağlama raporları alma hakkında bilgi edinin](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_03.png