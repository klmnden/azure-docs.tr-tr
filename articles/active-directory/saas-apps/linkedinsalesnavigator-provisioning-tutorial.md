---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için LinkedIn Sales Navigator yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını LinkedIn Sales Navigator kullanıcı hesaplarını Azure Active Directory yapılandırmayı öğrenin.
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
ms.date: 03/28/2019
ms.author: asmalser-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ce841a45893ebfbb0d9006e6b9eadc77f08a491
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057770"
---
# <a name="tutorial-configure-linkedin-sales-navigator-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için LinkedIn Sales Navigator yapılandırın

Bu öğreticinin amacı, LinkedIn Sales Navigator ve Azure AD'ye otomatik olarak sağlama ve sağlamasını Azure AD'den kullanıcı hesapları için LinkedIn Sales Navigator gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* Bir LinkedIn Sales Navigator Kiracı 
* LinkedIn Sales Navigator LinkedIn hesap Merkezi'ne erişimi olan bir yönetici hesabı

> [!NOTE]
> Azure Active Directory çalışır LinkedIn Sales Navigator kullanarak [SCIM](http://www.simplecloud.info/) protokolü.

## <a name="assigning-users-to-linkedin-sales-navigator"></a>Kullanıcı için LinkedIn Sales Navigator atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenecektir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları LinkedIn Sales Navigator erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar için LinkedIn Sales Navigator atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a>Kullanıcılar için LinkedIn Sales Navigator atamak için önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için LinkedIn Sales Navigator atanabilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için LinkedIn Sales Navigator atarken seçmelisiniz **kullanıcı** rol ataması iletişim. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a>LinkedIn Sales Navigator için kullanıcı sağlamayı yapılandırma

Bu bölümde Azure AD sağlama API'si LinkedIn Sales Navigator'ın SCIM kullanıcı hesabına bağlanma sırasında size kılavuzluk eder ve oluşturma, güncelleştirme ve devre dışı bırakmak için sağlama hizmetine atanan kullanıcı hesapları, kullanıcıya dayanarak LinkedIn Sales Navigator ve Azure AD'de Grup ataması.

> [!TIP]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için LinkedIn Sales Navigator, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirini tamamlar ancak otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a>Otomatik kullanıcı hesap için LinkedIn Sales Navigator Azure AD'de sağlama yapılandırmak için:

LinkedIn erişim belirteci almak için ilk adımdır bakın. Bir kuruluş yöneticisiyseniz, kendi kendine bir erişim belirteci sağlayabilirsiniz. Hesap Merkezi'nde Git **ayarları &gt; genel ayarları** açın **SCIM Kurulum** paneli.

> [!NOTE]
> Hesap Merkezi yerine doğrudan bir bağlantı aracılığıyla erişiyorsanız aşağıdaki adımları kullanarak ulaşabilirsiniz.

1. Hesap Merkezi'nde oturum açın.

2. Seçin **yönetici &gt; yönetici ayarları** .

3. Tıklayın **Gelişmiş tümleştirmeler** sol kenar çubuğundaki. Hesap merkezine yönlendirilirsiniz.

4. Tıklayın **+ yeni SCIM Yapılandırması Ekle** ve her alanı doldurarak yordamı izleyin.

    > [!NOTE]
    > Autoassign lisansları etkin değil, yalnızca kullanıcı verilerini eşitlenip anlamına gelir.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

    > [!NOTE]
    > Autolicense atama etkinleştirildiğinde, uygulama örneği ve lisans türü gerekir. Üzerinde bir ilk gelen atanmış lisansları, tüm lisansları duruma kadar ilk temel hizmet.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5. Tıklayın **belirteç Oluştur**. Erişim belirteci görüntü altında görmelisiniz **erişim belirteci** alan.

6. Erişim belirtecinizi sayfadan ayrılmadan önce Pano veya bilgisayara kaydedin.

7. Ardından, oturum [Azure portalında](https://portal.azure.com)ve **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

8. Çoklu oturum açma için LinkedIn Sales Navigator zaten yapılandırdıysanız arama alanını kullanarak LinkedIn Sales Navigator Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **LinkedIn Sales Navigator** uygulama galerisinde. Arama sonuçlarından LinkedIn Sales Navigator seçin ve uygulama listenize ekleyin.

9. LinkedIn Sales Navigator örneğinizi seçin ve ardından **sağlama** sekmesi.

10. Ayarlama **hazırlama modu** için **otomatik**.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11. Altında aşağıdaki alanları doldurun **yönetici kimlik bilgileri** :

    * İçinde **Kiracı URL'si** alanına https://api.linkedin.com.

    * İçinde **gizli belirteç** alanı, 1. adımda oluşturulan erişim belirtecini girin ve tıklayın **Bağlantıyı Sına** .

    * Portalınızı upperright tarafında bir başarı bildirimini görmeniz gerekir.

12. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

13. **Kaydet**’e tıklayın.

14. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den için LinkedIn Sales Navigator eşitlenecek kullanıcı ve grup öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri, kullanıcı hesaplarının ve güncelleştirme işlemleri için LinkedIn Sales Navigator grupların eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

    ![LinkedIn Sales Navigator sağlama](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15. Azure AD sağlama hizmeti için LinkedIn Sales Navigator etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

16. **Kaydet**’e tıklayın.

Bu, herhangi bir kullanıcı ve/veya LinkedIn Sales Navigator kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitlemeyi başlatır. İlk eşitleme hizmetini çalıştıran sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve LinkedIn Sales Navigator uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
