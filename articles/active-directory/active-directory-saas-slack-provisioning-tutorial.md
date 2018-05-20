---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı kayma yapılandırma | Microsoft Docs'
description: Azure Active Directory otomatik sağlama ve devre dışı bırakma sağlama kullanıcı hesaplarına kayma yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: 897121e0dcaaf417430b892c501a243303ae9b6e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-slack-for-automatic-user-provisioning"></a>Öğretici: Kayma otomatik kullanıcı sağlamayı yapılandırın


Kayma ve Azure gerçekleştirmesi gereken adımları göstermek için bu öğreticinin amacı olan kayma için Azure AD'den otomatik sağlama ve devre dışı bırakma sağlama kullanıcı hesapları için AD. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active Active directory kiracısı
*   Bir boşluk Kiracı ile [planı artı](https://aadsyncfabric.slack.com/pricing) veya daha iyi etkin 
*   Kayma takım yönetici izinlerine sahip bir kullanıcı hesabı 

Not: tümleştirme sağlama Azure AD dayanan [Slack'e SCIM'yi API](https://api.slack.com/scim) Slack takıma kullanılabilir olduğu Plus üzerinde planlayabilir ya da daha iyi.

## <a name="assigning-users-to-slack"></a>Slack'e kullanıcılar atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Slack uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Slack uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-slack"></a>Slack'e kullanıcılara atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için kayma atanabilir. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı Slack'e atarken seçmelisiniz **kullanıcı** ya da "Grup" rol ataması iletişim. "Varsayılan erişim" rolü sağlama için çalışmaz.


## <a name="configuring-user-provisioning-to-slack"></a>Slack'e kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD API sağlama kayma'nın kullanıcı hesabına bağlanma yoluyla size rehberlik eder ve kullanıcı ve grup atama Azure AD'de göre kayma kullanıcı hesapları oluşturma, güncelleştirme ve devre dışı bırakmak için sağlama hizmeti yapılandırma atanır.

**İpucu:** etkin SAML tabanlı çoklu oturum açma (Azure Portalı'nda) sağlanan yönergeleri izleyerek kayma için tercih edebilirsiniz [https://portal.azure.com]. Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.


### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı Slack'e Azure AD'de sağlama yapılandırmak için:


1)  İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2) Çoklu oturum açma için kayma zaten yapılandırdıysanız arama alanı kullanarak kayma Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Slack'e** uygulama galerisinde. Arama sonuçlarından kayma seçin ve uygulamaları listenize ekleyin.

3)  Kayma örneğiniz seçin ve ardından **sağlama** sekmesi.

4)  Ayarlama **sağlama modunda** için **otomatik**.

![Sağlama boşluk](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  Altında **yönetici kimlik bilgileri** 'yi tıklatın **Authorize**. Bu, yeni bir tarayıcı penceresinde bir Slack yetkilendirme iletişim kutusunu açar. 

6) Yeni pencerede kayma takım yönetim hesabınızı kullanarak oturum açın. Sonuçta elde edilen yetkilendirme iletişim kutusunda sağlamayı etkinleştirmek istediğiniz Slack ekibi seçin ve ardından **Authorize**. Tamamlandığında, sağlama yapılandırmasını tamamlamak için Azure portalına geri dönün.

![Yetkilendirme iletişim](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Slack uygulamanıza bağlanabilir. Bağlantı başarısız olursa Slack hesabınızın Team yönetici izinlerine sahip olduğundan emin olun ve "Yetkilendir" adımı yeniden deneyin.

8) Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

9) **Kaydet**’e tıklayın. 

10) Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları kayma**.

11) İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Slack'e eşitleneceğini kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri kayma kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

12) Azure AD hizmeti kayma için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

13) **Kaydet**’e tıklayın. 

Bu, herhangi bir kullanıcı ve/veya boşluk kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlatır. İlk eşitlemeyi gerçekleştirmek için yaklaşık her 10 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer unutmayın. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Slack uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

## <a name="optional-configuring-group-object-provisioning-to-slack"></a>[İsteğe bağlı] Grup nesnesi Slack'e sağlama yapılandırma 

İsteğe bağlı olarak, Azure AD'den kayma Grup nesnelerine sağlamayı etkinleştirebilirsiniz. Bu, "kullanıcı gruplarını atamasını" farklıdır, gerçek grup nesne üyeleri yanı sıra, Azure AD'den Slack'e çoğaltılır. Örneğin, Azure AD içinde "My grubu" adlı bir grup varsa, "My grubu" adlı bir identitical grubu kayma içinde oluşturulur.

### <a name="to-enable-provisioning-of-group-objects"></a>Grup nesnelerin sağlamayı etkinleştirmek için:

1) Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory gruplarına kayma**.

2) Eşleme özniteliği dikey penceresinde, etkin Evet olarak ayarlayın.

3) İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Slack'e eşitleneceğini grup öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri kayma gruplara güncelleştirme işlemleri için eşleştirmek için kullanılır. 

4) **Kaydet**’e tıklayın.

Kayma içinde atanmış herhangi bir grup nesne bu sonucu **kullanıcılar ve gruplar** Azure AD'den tam olarak Slack'e eşitlenmekte olan bölüm. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Slack uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](active-directory-saas-provisioning-reporting.md).


## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)
