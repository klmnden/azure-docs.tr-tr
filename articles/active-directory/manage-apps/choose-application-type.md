---
title: Uygulama eklerken kullanılacak uygulama türünü seçme | Microsoft Docs
description: Uygulamalar Azure AD ile tümleştirilebilir desteklenen türlerini anlamanız ve bunların ilgili yapılandırma seçenekleri
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6e1a1c614bfa126d58cf9343f945d16fd1c2733
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65781006"
---
# <a name="choosing-the-application-type-when-adding-an-application-in-azure-active-directory"></a>Azure Active Directory'de uygulama eklerken uygulama türünü seçme
Uygulamaları Azure Active Directory (Azure AD) ekleyebilirsiniz dört türleri hakkında daha fazla bilgi edinin. Azure Active Directory'de uygulama eklerken dört uygulama türünü birini seçmeniz istenir. 

## <a name="what-are-the-types-of-applications"></a>Uygulama türleri nelerdir?

Azure AD kullanarak ekleyebileceğiniz dört ana uygulama türlerini destekler **Ekle** özelliği bulunan altında **kurumsal uygulamalar**. Bunlar:

-   **Azure AD galeri uygulamaları** – çoklu oturum açma için Azure AD ile önceden tümleştirilmiş uygulama.

-   **Uygulama Proxy uygulamaları** – güvenli çoklu oturum açmayı harici olarak sağlamak istiyorsanız, şirket içi ortamınızda çalışan bir uygulama.

-   **Özel olarak geliştirilmiş uygulamaların** kuruluşunuzun Azure AD uygulama geliştirme platformu üzerinde geliştirmek isteyen, ancak bu mevcut olmayabilir henüz uygulama bir.

-   **Galeri dışı uygulamalar** – kendi uygulamalarınızı getirin! İstediğiniz herhangi bir web bağlantısına veya bir kullanıcı adı ve parola alanını işleyen herhangi bir uygulama SAML veya Openıd Connect protokolleri destekler veya çoklu oturum açma için Azure AD ile tümleştirmek istediğiniz SCIM destekler.

## <a name="features-and-capabilities-supported-by-the-application-types"></a>Özellikler ve yetenekler uygulama türleri tarafından desteklenir

Aşağıdaki özellikleri Azure AD'de herhangi bir önceki dört uygulama türünden desteklenir:

-   **Hızlı Başlangıç** – izleyerek bir uygulama ile hızlı bir şekilde başlayın [basit dağıtım adımları](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Genel Özellikler Yönetim** – almak bir [doğrudan deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) bir uygulamaya [markalamayı özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) bir uygulamanın veya [uygulamayı devre dışı](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) tüm kullanıcılar için.

-   **Kullanıcı ve Grup Yönetimi** – [atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) veya [Kaldır](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) kullanıcıları ve grupları bir uygulamaya erişiminiz grupları ve isteğe bağlı olarak bu kullanıcıları belirli uygulama rolleri Ata

-   **Self Servis uygulama erişiminin** – istemek kullanıcılarınızın [Self Servis uygulama erişiminin](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) bir uygulamada uygulama erişim panelleri üzerinden ya da bir uygulamanın doğrudan ekleyerek veya [ Self Servis etkinleştirilmiş gruba katılıyor](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), isteğe bağlı olarak yol boyunca iş onay alma zorunlu kılındığında

-   **Oturum açma günlükleri** – bkz [tüm oturum açma işlemleri uygulamaya](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), veya tüm uygulamalarınız

-   **Denetim günlükleri** – bkz [denetim günlüklerini uygulamaya yapılan değişiklikler hakkında ayrıntılı](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), veya tüm uygulamalarınız için

-   **Ve risk tabanlı koşullu erişim** – güçlü ayarlanmış [koşul tabanlı erişim kuralları](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) kullanıcıların belirli bir uygulamada oturum açmaya çalıştığında zorlanır

-   **İzinleri görüntüleme** – herhangi bir görüntüleme [OAuth2 izinleri](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) bir uygulamanın tek bir dizin yerleştirmek için erişimi var.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Çoklu oturum açma ve belirli bir uygulama türleri tarafından desteklenen modları sağlama

Aşağıdaki tabloda her önceki uygulama türleri tarafından desteklenen farklı tek oturum açma ve sağlama modları açıklanmaktadır. Bu tablo, belirli bir amaca desteklemek için eklemeniz gereken hangi uygulamanın anlamanıza yardımcı olması için kullanabilirsiniz.

  ![Uygulama türleri tablosu](./media/choose-application-type/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Çoklu oturum açma modunu seçme

Aşağıda desteklenen **çoklu oturum açma** modları Azure AD uygulamaları için.

- **Azure AD çoklu oturum devre dışı açma** – Azure AD çoklu oturum açma devre dışı seçin **çoklu oturum açma modunu** , henüz bu uygulama, çoklu oturum açma ile Azure AD ile tümleştirmek hazır olmayan veya teslim yalnızca sınama

- **Bağlantılı oturum açma** – seçin [bağlantılı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) **çoklu oturum açma modunu** zaten varolan çoklu oturum açma çözümü ile bağlı bir uygulamanız varsa ya da yalnızca istiyorsanız Basit bir bağlantı, kullanıcılarınızın yayımlamak kendi [uygulama erişim panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) veya [Office 365 uygulama Başlatıcısı](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

- **Parola tabanlı oturum açma** – seçin [parola tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) **çoklu oturum açma modunu** uygulamanız kullanıcı adı ve parola bir HTML alanı oluşturur ve bu kullanıcı adı depolamak istediğiniz ve güvenli bir şekilde uygulamayı daha sonra yeniden yürütülmesi gereken parola

- **SAML tabanlı oturum açma** – seçin [SAML tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) çoklu oturum açma uygulamanız SAML veya Openıd Connect protokolleri destekler veya kurallara göre belirli uygulama rollerine kullanıcıları eşleştirmek istiyorsanız modu SAML, talepleri tanımladığınız *

  >[!NOTE]
  >Bu seçenek, uygulama proxy'si bir uygulama için yapılandırıldığında kullanılabilir değildir.
  >
  >

- **Üst bilgi tabanlı oturum açma** – bu [üst bilgi tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) çoklu oturum açma gerçekleştirmek istediğiniz HTTP üst bilgi tabanlı kimlik doğrulamasını destekleyen PingAccess kullanarak bir uygulama varsa tek oturum açma modu 

  >[!NOTE]
  >Bu seçenek yalnızca, uygulama proxy'si ile PingAccess yapılandırıldığında bir uygulama için kullanılabilir.
  >
  >

- **Tümleşik Windows kimlik doğrulaması** – seçin [tümleşik Windows kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) çoklu oturum açma modu, çoklu oturum açma gerçekleştirmek istediğiniz şirket içi WIA uygulamanın gösterme 

  >[!NOTE]
  >Bu seçenek yalnızca, uygulama proxy'si bir uygulama için yapılandırıldığında kullanılabilir.
  >
  >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Özel olarak geliştirilmiş uygulamaları için çoklu oturum açma modları

Özel geliştirilmiş özel olarak geliştirilmiş uygulama sahip olduğunuz uygulamalar da dahil daha önce listelenen destek ek tek oturum açma modları karşılaşırsınız:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) tabanlı oturum açma

-   [Openıd Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) tabanlı oturum açma

-   [WS-Federasyon 1.2](https://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) tabanlı oturum açma

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) tabanlı oturum açma

Okuma [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) , bu tek oturum açma modunu destekler ve özel geliştirilmiş bir uygulamada oluşturma hakkında daha fazla bilgi için.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Oturum açma uygulamanın tek modunu ayarlama

Bir uygulamanın ayarlanacak **çoklu oturum açma** modu, aşağıdaki yönergeleri izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

## <a name="how-to-choose-a-provisioning-mode"></a>Hazırlama modunu seçme

- **El ile sağlama** – seçin [el ile](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) mevcut hesapları varsa, veya hesapları Azure AD dışında bu uygulama için yönetmek istediğiniz hazırlama modu.

- **Otomatik sağlama** – seçin [otomatik](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **sağlama modunu** API tabanlı otomatik sağlama ve/veya bu uygulamaya yönelik kullanıcı hesapları, sağlamayı etkinleştirmek istiyorsanız 

  >[!NOTE]
  >Bu seçenek, yalnızca içinde uygulamalar için kullanılabilir **öne çıkan** kategorisi [Azure AD uygulama Galerisi](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal).
  >
  >

- **SCIM tabanlı otomatik sağlama** – kullanın [SCIM tabanlı otomatik sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) uygulamanızı SCIM Protokolü destekleyip desteklemediğini kullanıcıları ve grupları, değişiklikleri otomatik olarak gösterilen değişiklikler algılamak için Azure AD ile tümleştirilmiş herhangi bir uygulama 

  >[!NOTE]
  >Bu seçenek, belirli bir sağlama modu olarak listelenmez, ancak Azure AD ile tümleştirilmiş tüm uygulamalar için varsayılan olarak etkindir.
  >
  >

## <a name="how-to-set-an-applications-provisioning-mode"></a>Hazırlama modu uygulama ayarlama

Bir uygulamanın ayarlanacak **sağlama** modu, aşağıdaki yönergeleri izleyin:

Bir uygulamanın ayarlanacak **çoklu oturum açma** modu, aşağıdaki yönergeleri izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Sağlama yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **sağlama** uygulamanın sol taraftaki gezinti menüsünde.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
