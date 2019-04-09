---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için LinkedIn yükseltmesine yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlama ve sağlamasını LinkedIn yükseltmek için kullanıcı hesapları için Azure Active Directory yapılandırmayı öğrenin.
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
ms.openlocfilehash: e95fae67d3e7fd97cf4be1642f41b64a07fd0145
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59056920"
---
# <a name="tutorial-configure-linkedin-elevate-for-automatic-user-provisioning"></a>Öğretici: LinkedIn yükseltmek için otomatik kullanıcı hazırlama yapılandırın

Bu öğreticinin amacı, LinkedIn yükseltebilir ve Azure AD'deki otomatik olarak sağlama ve sağlamasını LinkedIn yükseltmek için Azure AD'den kullanıcı hesapları için gerçekleştirmeniz gereken adımlar gösterir sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Azure Active Directory kiracısı
* Bir LinkedIn yükseltmesine Kiracı
* LinkedIn yükseltmesine içinde LinkedIn hesap Merkezi'ne erişimi olan bir yönetici hesabı

> [!NOTE]
> Azure Active Directory çalışır LinkedIn Yükselt'i kullanarak [SCIM](http://www.simplecloud.info/) protokolü.

## <a name="assigning-users-to-linkedin-elevate"></a>LinkedIn yükseltmek için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenecektir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları LinkedIn yükseltmesine erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra bu kullanıcılar LinkedIn yükseltmek için buradaki yönergeleri izleyerek atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-elevate"></a>LinkedIn yükseltmek için kullanıcı atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı atanmış LinkedIn yükseltmek için sağlama yapılandırması test etmek için. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* LinkedIn yükseltmek için kullanıcı atama, seçmelisiniz **kullanıcı** rol ataması iletişim. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="configuring-user-provisioning-to-linkedin-elevate"></a>LinkedIn yükseltmek için kullanıcı sağlamayı yapılandırma

Bu bölümde, Azure AD sağlama API'si LinkedIn Yükselt'ın SCIM kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve kullanıcı hesapları, kullanıcı ve Grup ataması dayanan LinkedIn yükseltmesine atanan oluşturma, güncelleştirme ve devre dışı bırakmak için sağlama hizmetini yapılandırma Azure AD'de.

**İpucu:** Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma LinkedIn yükseltmek için yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirini tamamlar ancak otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-elevate-in-azure-ad"></a>Otomatik hesap LinkedIn yükseltmek için Azure AD'de kullanıcı sağlamayı yapılandırmak için:

LinkedIn erişim belirteci almak için ilk adımdır bakın. Bir kuruluş yöneticisiyseniz, kendi kendine bir erişim belirteci sağlayabilirsiniz. Hesap Merkezi'nde Git **ayarları &gt; genel ayarları** açın **SCIM Kurulum** paneli.

> [!NOTE]
> Hesap Merkezi yerine doğrudan bir bağlantı aracılığıyla erişiyorsanız aşağıdaki adımları kullanarak ulaşabilirsiniz.

1. Hesap Merkezi'nde oturum açın.

2. Seçin **yönetici &gt; yönetici ayarları** .

3. Tıklayın **Gelişmiş tümleştirmeler** sol kenar çubuğundaki. Hesap merkezine yönlendirilirsiniz.

4. Tıklayın **+ yeni SCIM Yapılandırması Ekle** ve her alanı doldurarak yordamı izleyin.

    > [!NOTE]
    > Autoassign lisansları etkin değil, yalnızca kullanıcı verilerini eşitlenip anlamına gelir.

    ![LinkedIn yükseltmesine sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate1.PNG)

    > [!NOTE]
    > Autolicense atama etkinleştirildiğinde, uygulama örneği ve lisans türü gerekir. Üzerinde bir ilk gelen atanmış lisansları, tüm lisansları duruma kadar ilk temel hizmet.

    ![LinkedIn yükseltmesine sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate2.PNG)

5. Tıklayın **belirteç Oluştur**. Erişim belirteci görüntü altında görmelisiniz **erişim belirteci** alan.

6. Erişim belirtecinizi sayfadan ayrılmadan önce Pano veya bilgisayara kaydedin.

7. Ardından, oturum [Azure portalında](https://portal.azure.com)ve **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

8. Çoklu oturum açma için LinkedIn yükseltmesine zaten yapılandırdıysanız, örneğiniz LinkedIn arama alanını kullanarak yükseltmek için arama yapın. Aksi takdirde seçin **Ekle** araması **LinkedIn yükseltmesine** uygulama galerisinde. Arama sonuçlarından LinkedIn Yükselt'i seçin ve uygulama listenize ekleyin.

9. LinkedIn yükseltmesine örneğinizin seçip **sağlama** sekmesi.

10. Ayarlama **hazırlama modu** için **otomatik**.

    ![LinkedIn yükseltmesine sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate3.PNG)

11. Altında aşağıdaki alanları doldurun **yönetici kimlik bilgileri** :

    * İçinde **Kiracı URL'si** alanına `https://api.linkedin.com`.

    * İçinde **gizli belirteç** alanı, 1. adımda oluşturulan erişim belirtecini girin ve tıklayın **Bağlantıyı Sına** .

    * Portalınızı upperright tarafında bir başarı bildirimini görmeniz gerekir.

12. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

13. **Kaydet**’e tıklayın.

14. İçinde **öznitelik eşlemelerini** bölümünde, LinkedIn yükseltmek için Azure AD'den eşitlenecek kullanıcı ve grup öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri, kullanıcı hesaplarının ve grupların LinkedIn yükseltmek için güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

    ![LinkedIn yükseltmesine sağlama](./media/linkedinelevate-provisioning-tutorial/linkedin_elevate4.PNG)

15. Azure AD sağlama hizmeti için LinkedIn yükseltmesine etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

16. **Kaydet**’e tıklayın.

Bu, herhangi bir kullanıcı ve/veya LinkedIn yükseltmesine kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitlemeyi başlatır. İlk eşitleme hizmetini çalıştıran sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve LinkedIn yükseltmesine uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
