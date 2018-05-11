---
title: Bir uygulama eklerken kullanmak için hangi uygulama türü seçme | Microsoft Docs
description: Azure AD ile tümleştirebilir uygulamaları desteklenen türlerini anlamanız ve bunların ilgili yapılandırma seçenekleri
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: a34c3343b669cb80ad88c1b09fe95b1b1d9b5275
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="how-to-choose-which-application-type-to-use-when-adding-an-application"></a>Bir uygulama eklerken kullanmak için hangi uygulama türü seçme

Bu makalede Azure AD ile tümleştirebilirsiniz uygulama dört ana türlerini anlamanıza yardımcı olur:

* Bunların her biri tarafından desteklenen özellikler
* Hangi uygulamanın neden seçebilirsiniz
* Kullanıcıların şeklini gibi bu uygulamanın çekirdek özelliklerinin nasıl yapılandırılacağı **sağlanan**, veya ne **çoklu oturum açma** teknolojisini kullanın.

## <a name="supported-application-types-in-azure-ad"></a>Azure AD'de desteklenen uygulama türleri

Azure AD kullanarak ekleyebilirsiniz dört ana uygulama türlerini destekler **Ekle** özelliği bulunan altında **kurumsal uygulamalar**. Bunlar:

-   **Azure AD galeri uygulamaları** – bir tek oturum açma için Azure AD ile önceden tümleştirilmiş uygulama.

-   **Uygulama proxy'si uygulamalar** – güvenli çoklu oturum açmayı dışarıdan sağlamak istediğiniz, şirket içi ortamınızda çalışan bir uygulama.

-   **Özel geliştirilmiş uygulamaları** – kuruluşunuz Azure AD uygulama geliştirme platformu üzerinde geliştirmek istiyor ancak var henüz uygulama.

-   **Galeri olmayan uygulamaları** – kendi uygulamalarınızı Getir! İstediğiniz herhangi bir web bağlantıyı ya da bir kullanıcı adı ve parola alanı işleyen herhangi bir uygulama SAML veya Openıd Connect protokollerini destekler veya çoklu oturum açma için Azure AD ile tümleştirmek istediği SCIM'yi destekler.

## <a name="features-and-capabilities-supported-by-all-the-preceding-application-types"></a>Özellikler ve önceki tüm uygulama türleri tarafından desteklenen yetenekler

Aşağıdaki özellikler herhangi bir önceki dört uygulama türleri tarafından Azure AD'de desteklenir:

-   **Hızlı Başlangıç** – izleyerek bir uygulama ile hızlı bir şekilde yapmalarını [basit dağıtım adımları](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Genel Özellikler Yönetim** – alma bir [doğrudan ayrıntılı bağlantı](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) bir uygulamaya [marka özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) bir uygulamanın veya [uygulamayı devre dışı](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) tüm kullanıcılar için.

-   **Kullanıcı ve Grup Yönetimi** – [atamak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) veya [kaldırmak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) kullanıcılar ve gruplar bir uygulamaya ve isteğe bağlı olarak bu kullanıcıları belirli uygulama rolleri Ata ve gruplar için erişimi

-   **Self Servis uygulamaya erişim** – istemek kullanıcılarınızın [Self Servis uygulamaya erişim](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) kendi uygulama erişimini ya da bir uygulama doğrudan ekleyerek paneller bir uygulama için veya [ Self Servis etkinleştirilmiş gruba katılıyor](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), isteğe bağlı olarak yol iş onay gerektiren

-   **Oturum açma günlükleri** – bkz [tüm oturum açma işlemlerine uygulama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), ya da tüm uygulamalarınızın

-   **Denetim günlükleri** – bkz [denetim günlüklerini değişiklikleri bir uygulama hakkında ayrıntılı](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), veya tüm uygulamalar için

-   **Koşullu ve risk tabanlı erişim** – güçlü ayarlanmış [koşul tabanlı erişim kuralları](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) kullanıcıların belirli bir uygulamaya oturum açmaya çalıştığında uygulanır

-   **İzinleri Görünüm** – herhangi birini görüntülemek [OAuth2 izinleri](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) bir uygulamanın tek bir dizin yerleştirmek için erişimi var.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Çoklu oturum açma ve belirli bir uygulama türleri tarafından desteklenen modları sağlama

Aşağıdaki tabloda her önceki uygulama türleri tarafından desteklenen farklı tek oturum açma ve sağlama modu açıklanmaktadır. Belirli bir amaca desteklemek üzere eklemenize gerek hangi uygulamanın anlamanıza yardımcı olması için bu tabloyu kullanın.

  ![Uygulama türleri tablosu](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Tek bir oturum açma modu seçme

Şunlardır desteklenen **çoklu oturum açma** modları Azure AD uygulamaları için.

-   **Azure AD çoklu oturum açma devre dışı özelliğini** – Azure AD çoklu oturum açma devre dışı özelliğini seçin **tek oturum açma modu** , henüz bu uygulama tek oturum açma ile Azure AD ile tümleştirmek hazır değil veya onu yalnızca sınama

-   **Bağlantılı oturum açma** – seçin [bağlantılı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **tek oturum açma modu** mevcut tek oturum açma çözümüyle zaten bağlı olan bir uygulamanız varsa ya da yalnızca istiyorsanız, Kullanıcılarınız için basit bir bağlantı yayımlama kendi [uygulama erişim Paneli'ne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) veya [Office 365 uygulama Başlatıcı](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Parola tabanlı oturum açma** – seçin [parola tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **tek oturum açma modu** uygulamanız bir HTML kullanıcı adı ve parola alanı oluşturur ve bu kullanıcı adını depolamak istiyorsanız ve güvenli bir şekilde uygulamayı daha sonra yeniden yürütülmesi için parola

-   **SAML tabanlı oturum açma** – seçin [SAML tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) çoklu oturum açma, uygulamanızın SAML veya Openıd Connect protokollerini destekler veya kurallara göre belirli uygulama rollere kullanıcıları eşlemek istiyorsanız modu SAML Taleplerde tanımladığınız *

   >[!NOTE]
   >Uygulama proxy'si bir uygulama için yapılandırıldığında, bu seçenek kullanılamaz.
   >
   >

-   **Üstbilgi tabanlı oturum açma** – bu seçin [üstbilgi tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) çoklu oturum açmayı gerçekleştirmek istediğiniz HTTP üstbilgisi tabanlı kimlik doğrulamasını destekleyen PingAccess kullanan bir uygulamanız varsa, tek oturum açma modu 

   >[!NOTE]
   >Bu seçenek, yalnızca uygulama proxy'si ve PingAccess yapılandırıldığında bir uygulama için kullanılabilir.
   >
   >

-   **Tümleşik Windows kimlik doğrulaması** – seçin [tümleşik Windows kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) çoklu oturum açma çoklu oturum açmayı gerçekleştirmek istediğiniz şirket içi WIA uygulamanın gösterme zaman modu 

   >[!NOTE]
   >Bu seçenek, yalnızca uygulama proxy'si bir uygulama için yapılandırıldığında kullanılabilir.
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Özel geliştirilmiş uygulamalar için çoklu oturum açma modları

Uygulamaları aracılığıyla geliştirilen özel sahip [özel geliştirilmiş uygulama](#_Custom-Developed_Applications) deneyimi de dahil daha önce listelenen ek tek oturum açma modunu destekler:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) oturum açma tabanlı

-   [Openıd Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) oturum açma tabanlı

-   [WS-Federasyon 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) oturum açma tabanlı

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) oturum açma tabanlı

Okuma [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) bu tek oturum açma modunu destekler özel geliştirilmiş bir uygulama oluşturma hakkında daha fazla bilgi için.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Oturum açma uygulamanın tek modunu ayarlama

Bir uygulamanın ayarlamak için **çoklu oturum açma** modu, aşağıdaki yönergeleri izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

## <a name="how-to-choose-a-provisioning-mode"></a>Hazırlama modunu seçme

-   **El ile sağlama** – seçin [el ile](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) var olan hesapları varsa veya Azure AD dışında bu uygulama için hesaplarını yönetmek istediğiniz hazırlama modu.

-   **Otomatik sağlama** – seçin [otomatik](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **sağlama modu** API tabanlı otomatik sağlama ve/veya bu uygulama için kullanıcı hesaplarının sağlamayı kaldırma özelliklerini etkinleştirmek isteyip istemediğinizi 

   >[!NOTE]
   >Bu seçenek yalnızca içinde uygulamalar için kullanılabilir **öne çıkan** kategorisini [Azure AD uygulama galerisinde](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).
   >
   >

-   **Otomatik sağlama SCIM'yi tabanlı** – kullanmak [SCIM'yi tabanlı otomatik sağlamayı](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) kullanıcılar ve gruplar değişiklikler için otomatik olarak gösterilen değişiklikleri algılamak için uygulamanızın SCIM'yi Protokolü destekliyorsa Azure AD ile tümleşik herhangi bir uygulama 

   >[!NOTE]
   >Bu seçenek, belirli bir sağlama modu olarak listelenmez, ancak Azure AD ile tümleşik tüm uygulamalar için varsayılan olarak etkindir.
   >
   >

## <a name="how-to-set-an-applications-provisioning-mode"></a>Bir uygulamanın nasıl kurulur hazırlama modu

Bir uygulamanın ayarlamak için **sağlama** modu, aşağıdaki yönergeleri izleyin:

Bir uygulamanın ayarlamak için **çoklu oturum açma** modu, aşağıdaki yönergeleri izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Sağlama yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **sağlama** uygulamanın sol taraftaki gezinti menüsünde.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](manage-apps/what-is-application-management.md)
