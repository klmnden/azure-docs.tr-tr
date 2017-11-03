---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı LinkedIn satış Gezgini yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve LinkedIn satış Gezgini kullanıcı hesaplarına sağlanmasını için Azure Active Directory yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: 86357949c8e6927f78ca5bb8b7e20a6b88c37ef3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-linkedin-sales-navigator-for-automatic-user-provisioning"></a>Öğretici: LinkedIn satış Gezgini otomatik kullanıcı sağlamayı için yapılandırma


Bu öğreticinin amacı LinkedIn satış Gezgini ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den LinkedIn satış Gezgini sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active Directory kiracısı
*   Bir LinkedIn satış Gezgini Kiracı 
*   Bir yönetici hesabıyla LinkedIn satış Gezgininde LinkedIn hesap Merkezi erişim

> [!NOTE]
> Azure Active Directory tümleştirir LinkedIn satış Gezgini kullanarak [SCIM'yi](http://www.simplecloud.info/) protokolü.

## <a name="assigning-users-to-linkedin-sales-navigator"></a>Kullanıcıları LinkedIn satış Gezgini'ne atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları LinkedIn satış Gezgini erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar LinkedIn satış Gezgini'ne atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a>LinkedIn satış Gezgini'ne kullanıcılara atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek LinkedIn satış Gezgini'ne atanabilir. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı LinkedIn satış Gezgini'ne atarken seçmelisiniz **kullanıcı** rol ataması iletişim. "Varsayılan erişim" rolü sağlama için çalışmaz.


## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a>LinkedIn satış Gezgini kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD API sağlama LinkedIn satış Gezgini'nın SCIM'yi kullanıcı hesabına bağlanma yoluyla size rehberlik eder ve LinkedIn satış kullanıcı ve grup atama Azure AD'de göre Gezgini içindeki kullanıcı hesapları oluşturma, güncelleştirme ve devre dışı bırakmak için sağlama hizmeti yapılandırma atanır.

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için LinkedIn satış Gezgini, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı LinkedIn satış Gezgini'ne Azure AD'de sağlama yapılandırmak için:


LinkedIn erişim belirteci almak için ilk adımdır bakın. Bir kuruluş yöneticisi iseniz, bir erişim belirteci otomatik olarak sağlayabilirsiniz. Hesap Merkezinize gidin **ayarları &gt; genel ayarları** açarak **SCIM'yi Kurulum** paneli.

> [!NOTE]
> Hesap Merkezi'nde yerine doğrudan bir bağlantı aracılığıyla erişiyorsanız aşağıdaki adımları kullanarak ulaşabilirsiniz.

1)  Merkezi hesap için oturum açın.

2)  Seçin **yönetici &gt; yönetici ayarları** .

3)  Tıklatın **Gelişmiş tümleştirmeler** sol kenar çubuğunda. Hesap Merkezi'nde yönlendirilir.

4)  Tıklatın **+ Ekle yeni SCIM'yi yapılandırma** ve her alanı doldurarak yordamı izleyin.

> Autoassign lisansları etkin değil, yalnızca kullanıcı verilerini eşitlenip anlamına gelir.

![LinkedIn satış Gezgini sağlama](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

> Autolicense atama etkinleştirildiğinde, uygulama örneği ve lisans türü Not gerekir. İlk gelen üzerinde atanmış lisansların, tüm lisanslar duruma kadar temel ilk hizmet.

![LinkedIn satış Gezgini sağlama](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5)  Tıklatın **belirteç Oluştur**. Erişim belirteci görüntü altında görmelisiniz **erişim belirteci** alan.

6)  Erişim belirteci sayfadan ayrılmadan önce Pano veya bilgisayara kaydedin.

7) Ardından, oturum [Azure portal](https://portal.azure.com)ve **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

8) Çoklu oturum açma için zaten LinkedIn satış Gezgini yapılandırdıysanız, LinkedIn satış arama alanı kullanarak Gezgini Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **LinkedIn satış Gezgini** uygulama galerisinde. Arama sonuçlarından LinkedIn satış Gezgini seçin ve uygulamaları listenize ekleyin.

9)  LinkedIn satış Gezgini örneğiniz seçin ve ardından **sağlama** sekmesi.

10) Ayarlama **sağlama modunda** için **otomatik**.

![LinkedIn satış Gezgini sağlama](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11)  Altında aşağıdaki alanları doldurun **yönetici kimlik bilgileri** :

* İçinde **Kiracı URL** alanında, https://api.linkedin.com girin.

* İçinde **gizli belirteci** alan, 1. adımda oluşturulan erişim belirteci girin ve tıklayın **Bağlantıyı Sına** .

* Portalınızı upperright tarafında bir başarı bildirimi görmelisiniz.

12) Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

13) **Kaydet** düğmesine tıklayın. 

14) İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den LinkedIn satış Gezgini'ne eşitleneceğini kullanıcı ve grup öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri, kullanıcı hesapları ve grupları LinkedIn satış Gezgininde güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

![LinkedIn satış Gezgini sağlama](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15) Azure AD hizmeti LinkedIn satış Gezgini için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

16) **Kaydet** düğmesine tıklayın. 

Bu, herhangi bir kullanıcı ve/veya LinkedIn satış Gezgin kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlatır. İlk eşitlemeyi gerçekleştirmek için yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer unutmayın. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve LinkedIn satış Gezgini uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.


## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
