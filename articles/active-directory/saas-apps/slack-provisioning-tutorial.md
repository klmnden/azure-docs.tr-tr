---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Slack yapılandırma | Microsoft Docs'
description: Slack için otomatik olarak sağlama ve devre dışı bırakma sağlama kullanıcı hesapları için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: daveba
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: b50bcada8cfc72c06804793850f1f28a288f5248
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057041"
---
# <a name="tutorial-configure-slack-for-automatic-user-provisioning"></a>Öğretici: Slack için otomatik kullanıcı hazırlama yapılandırın

Slack ve Azure'daki gerçekleştirmesi gereken adımları göstermek için bu öğreticinin amacı olan otomatik olarak sağlama ve devre dışı bırakma sağlama kullanıcı hesaplarına AD'den Azure AD'ye Slack.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* Bir Slack Kiracı ile [planı artı](https://aadsyncfabric.slack.com/pricing) ya da daha iyi etkin
* Slack takım Yöneticisi izinlerine sahip bir kullanıcı hesabı

Not: Azure AD tümleştirmesi sağlama dayanan [Slack SCIM API](https://api.slack.com/scim), Slack takımlar için kullanılabildiği artı planlayabilir ya da daha iyi.

## <a name="assigning-users-to-slack"></a>Slack için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenecektir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Slack uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar Slack uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-slack"></a>Slack için kullanıcı atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için Slack atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için Slack atarken seçmelisiniz **kullanıcı** ya da "Grup" rol ataması iletişim. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="configuring-user-provisioning-to-slack"></a>Slack için kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama API'si Slack'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve Grup atamasına dayalı Slack içinde devre dışı bırak.

**İpucu:** Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma için Slack etkin, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a>Otomatik kullanıcı hesabı için Slack Azure AD'de sağlama yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Slack zaten yapılandırdıysanız arama alanını kullanarak Slack Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **Slack** uygulama galerisinde. Arama sonuçlarından Slack seçin ve uygulama listenize ekleyin.

3. Slack örneğinizi seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**.

   ![Slack sağlama](./media/slack-provisioning-tutorial/Slack1.PNG)

5. Altında **yönetici kimlik bilgileri** bölümünde **Authorize**. Bu, yeni bir tarayıcı penceresinde bir Slack yetkilendirme iletişim kutusu açılır.

6. Yeni pencerede, Slack takım yöneticisi hesabınızı kullanarak oturum açın. Sonuç yetkilendirme iletişim için sağlamayı etkinleştirmek için istediğiniz Slack takımı seçin ve ardından **Authorize**. Tamamlandığında, sağlama yapılandırmasını tamamlamak için Azure portalına dönün.

    ![Yetkilendirme iletişim](./media/slack-provisioning-tutorial/Slack3.PNG)

7. Azure portalında **Test Bağlantısı** Azure emin olmak için AD, Slack uygulamanıza bağlanabilirsiniz. Bağlantı başarısız olursa, Slack hesabınızı takım Yöneticisi izinlerine sahip olduğundan emin olun ve "Yetkilendir" adımı yeniden deneyin.

8. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

9. **Kaydet**’e tıklayın.

10. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için Slack**.

11. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Slack için eşitlenecek kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri, Slack kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

12. Azure AD sağlama hizmeti için Slack etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

13. **Kaydet**’e tıklayın.

Bu, herhangi bir kullanıcı ve/veya Slack kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitlemeyi başlatır. İlk eşitleme hizmeti çalışıyor olarak yaklaşık her 10 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Slack uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan Etkinlik raporlarını sağlama için bağlantıları izleyin.

## <a name="optional-configuring-group-object-provisioning-to-slack"></a>[İsteğe bağlı] Grup nesne için Slack sağlama yapılandırma

İsteğe bağlı olarak, Azure ad grubu nesne için Slack sağlamayı etkinleştirebilirsiniz. Bu, "kullanıcı gruplarını atamasını" farklıdır, Slack için gerçek grup nesnesinin üyelerinin yanı sıra, Azure AD'den çoğaltılır. Örneğin, Azure AD'de "Grubum" adlı bir grubu varsa, "My grubu" adlı bir aynı grup Slack içinde oluşturulur.

### <a name="to-enable-provisioning-of-group-objects"></a>Grup nesnelerin sağlamayı etkinleştirmek için:

1. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory gruplarına Slack**.

2. Öznitelik eşlemesi dikey penceresinde, etkin Evet olarak ayarlayın.

3. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Slack için eşitlenecek grup öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri güncelleştirme işlemleri için Slack gruplarında eşleştirmek için kullanılır. 

4. **Kaydet**’e tıklayın.

Bu sonucu Slack içinde atanmış herhangi bir grup nesne **kullanıcılar ve gruplar** Slack için Azure AD'den tam olarak eşitlenmekte olan bölüm. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Slack uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Slack'ın yapılandırırken **displayName** öznitelik, aşağıdaki davranışları unutmayın:

  * Değerleri (örneğin 2 kullanıcılar, aynı görünen adı olabilir) tamamen benzersiz değil

  * İngilizce olmayan karakterler, boşluk, büyük/küçük harf destekler. 
  
  * Noktalama, nokta, alt çizgi, kısa çizgi, kesme, köşeli ayraçlar içerir izin (örneğin **([{}])**) ve ayırıcılar (örneğin **, /;**).
  
  * Bu iki ayar Slack'ın çalışma alanına/kuruluşta - yapılandırıldıysa yalnızca güncelleştirmeleri **profil eşitleme etkin** ve **kullanıcılar, kullanıcıların görünen adı değiştirilemez**.
  
  * Slack'ın **kullanıcıadı** özniteliğine sahip altında 21 karakter olmalı ve benzersiz bir değere sahip.

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
