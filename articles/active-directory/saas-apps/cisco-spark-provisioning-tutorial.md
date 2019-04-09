---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Cisco Spark'ı yapılandırma | Microsoft Docs"
description: Otomatik olarak sağlama ve sağlamasını Cisco Spark için kullanıcı hesapları için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77dab6ad0480bc1565c219766d17211995dcfc20
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59056954"
---
# <a name="tutorial-configure-cisco-spark-for-automatic-user-provisioning"></a>Öğretici: Cisco Spark için otomatik kullanıcı hazırlama yapılandırın

Bu öğreticinin amacı, Cisco Spark ve Azure Active Directory (Azure AD) Azure AD yapılandırmak için sağlama ve sağlamasını Cisco Spark kullanıcılara otomatik olarak gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulları zaten sahip olduğunuzu varsayar:

* Azure AD kiracısı
* Bir Cisco Spark Kiracı
* Cisco Spark yönetici izinlerine sahip bir kullanıcı hesabı

> [!NOTE]
> Azure AD tümleştirmesi sağlama dayanan [Cisco Spark Webservice](https://developer.webex.com/getting-started.html), Cisco Spark takımlar için kullanılabildiği.

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark galeri ekleme

Azure AD ile otomatik kullanıcı hazırlama için Cisco Spark yapılandırmadan önce Cisco Spark Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Azure AD uygulama galerisinden Cisco Spark eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Cisco Spark**seçin **Cisco Spark** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Cisco Spark](common/search-new-app.png)

## <a name="assigning-users-to-cisco-spark"></a>Cisco Spark için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hazırlama bağlamında, yalnızca kullanıcı ve/veya "Azure AD'de bir uygulama için atandı" grupları eşitlenir.

Yapılandırma ve otomatik kullanıcı hazırlama etkinleştirmeden önce hangi kullanıcıların Azure AD'de Cisco Spark erişmesi karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar için Cisco Spark atayabilirsiniz:

* [Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-spark"></a>Cisco Spark için kullanıcı atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını otomatik kullanıcı test etmek için Cisco Spark'a atanır. Ek kullanıcılar daha sonra atanabilir.

* Cisco Spark için kullanıcı atama, atama iletişim kutusunda (varsa) geçerli bir uygulamaya özgü rolü seçmeniz gerekir. Kullanıcılarla **varsayılan erişim** rol sağlamasından dışlanır.

## <a name="configuring-automatic-user-provisioning-to-cisco-spark"></a>Cisco Spark için otomatik kullanıcı sağlamayı yapılandırma

Bu bölümde oluşturmak, güncelleştirmek ve kullanıcıların Azure AD'de kullanıcı atamaları temel alınarak Cisco Spark devre dışı bırakmak için Azure AD sağlama hizmeti yapılandırmak için gereken adımları size kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-cisco-spark-in-azure-ad"></a>Azure AD'de Cisco Spark için otomatik kullanıcı hazırlama yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Cisco Spark**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Cisco Spark**.

    ![Uygulamalar listesini Cisco Spark bağlantıdaki](common/all-applications.png)

3. Seçin **sağlama** sekmesi.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/ProvisioningTab.png)

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/ProvisioningCredentials.png)

5. Altında **yönetici kimlik bilgileri** giriş bölümünde **Kiracı URL'si**, ve **gizli belirteç** Cisco Spark'ın hesabının.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/secrettoken1.png)

    * İçinde **Kiracı URL'si** alan, Cisco Spark SCIM API URL'si biçimi alır kiracınız için doldurma `https://api.ciscospark.com/v1/scim/[Tenant ID]/`burada `[Tenant ID]` 6. adımda açıklandığı gibi bir alfasayısal bir dize özelliğidir.

    * İçinde **gizli belirteç** alan, 6. adımda açıklandığı gibi gizli belirteç doldurun.

6. **Kiracı kimliği** ve **gizli belirteç** Cisco Spark için hesap açılarak bulunabilir [Cisco Spark Geliştirici sitesi](https://developer.webex.com/) Yönetici hesabınızla. -Bir kez günlüğe kaydedilir

   * Git [Başlarken sayfası](https://developer.webex.com/getting-started.html)

   * Ekranı aşağı kaydırarak [kimlik doğrulaması bölümü](https://developer.webex.com/getting-started.html#authentication)
  
    ![Cisco Spark kimlik doğrulama belirteci](./media/cisco-spark-provisioning-tutorial/SecretToken.png)

   * Alfasayısal dize kutusunda, **gizli belirteç**. Bu belirteç Panoya Kopyala

   * Git [alma My kendi Ayrıntıları sayfası](https://developer.webex.com/endpoint-people-me-get.html)
       * Test modu açık olduğundan emin olun
       * Ardından bir boşluk "Bearer" sözcüğünü yazın ve sonra gizli belirteç yetkilendirme alanına yapıştırabilirsiniz ![Cisco Spark kimlik doğrulama belirteci](./media/cisco-spark-provisioning-tutorial/GetMyDetails.png)
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

7. 5. adımda gösterilen alanlar doldurma üzerine tıklayın **Test Bağlantısı** Azure emin olmak için AD, Cisco Spark'a bağlanabilirsiniz. Bağlantı başarısız olursa, Cisco Spark hesabının yönetici izinlerine sahip olun ve yeniden deneyin.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/TestConnection.png)

8. İçinde **bildirim e-posta** alanında, bir kişi veya grubun ve sağlama hata bildirimleri almak - onay e-posta adresi girin **birhataoluşursa,bire-postabildirimigönder**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/EmailNotification.png)

9. **Kaydet**’e tıklayın.

10. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için Cisco Spark**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/UserMapping.png)

11. Cisco Spark Azure AD'den eşitlenen kullanıcı özniteliklerini gözden **eşleme özniteliği** bölümü. Seçilen öznitelikler **eşleşen** özellikleri, Cisco Spark kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Seçin **Kaydet** düğmesine değişiklikleri uygulayın.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/UserMappingAttributes.png)

12. Kapsam belirleme filtrelerini yapılandırmak için aşağıdaki yönergelere bakın [Scoping filtre öğretici](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Azure AD sağlama hizmeti için Cisco Spark'ı etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/ProvisioningStatus.png)

14. Kullanıcılara ve/veya istediğiniz grupları Cisco Spark sağlamak için istenen değerleri seçerek tanımlamak **kapsam** içinde **ayarları** bölümü.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/SyncScope.png)

15. Sağlama için hazır olduğunuzda, tıklayın **Kaydet**.

    ![Cisco Spark sağlama](./media/cisco-spark-provisioning-tutorial/Save.png)

Bu işlem, tüm kullanıcıların ilk eşitleme başlar ve/veya tanımlı gruplar **kapsam** içinde **ayarları** bölümü. İlk eşitleme yaklaşık 40 dakikada Azure AD sağlama hizmeti çalışıyor sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve sağlama hizmeti Cisco Spark üzerinde Azure AD tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik Raporu sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Cisco Spark şu anda Cisco'nun erken alan test etme (EFT) aşamasındadır. Daha fazla bilgi için lütfen başvurun [Cisco'nun Destek ekibine](https://www.webex.co.in/support/support-overview.html). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri gözden geçirin ve sağlama etkinliği raporları alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_03.png
