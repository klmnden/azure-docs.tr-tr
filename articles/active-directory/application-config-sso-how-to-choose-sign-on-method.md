---
title: "Hangi çoklu oturum açma yönteminin kullanılacağını belirleme | Microsoft Docs"
description: "Azure AD tarafından desteklenen tek oturum açma modu anlamak ve hangisinin ilgilendiğiniz uygulama seçmek için seçin."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 80f4a965920fec9cb578c1bee235c7857f37431e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Hangi çoklu oturum açma yönteminin kullanılacağını belirleme

Bu makalede, Azure AD tarafından desteklenen tek oturum açma modu anlamanıza yardımcı olmak ve hangisinin ilgilendiğiniz uygulama seçmek için seçin.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Çoklu oturum açma ve belirli bir uygulama türleri tarafından desteklenen modları sağlama

Aşağıdaki tabloda, farklı çoklu oturum açma ve türlerinin her biri yukarıdaki uygulama tarafından desteklenen sağlama modu açıklanmaktadır. Belirli bir amaca desteklemek üzere eklemenize gerek hangi uygulamanın anlamanıza yardımcı olması için bu tabloyu kullanın.

  ![AP türleri tablosu](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Tek bir oturum açma modu seçme

Desteklenen **çoklu oturum açma** modları Azure AD uygulamaları için aşağıda listelenmiştir.

-   **Azure AD çoklu oturum açma devre dışı özelliğini** – Azure AD çoklu oturum açma devre dışı özelliğini seçin **tek oturum açma modu** , henüz bu uygulama tek oturum açma ile Azure AD ile tümleştirmek hazır değil veya onu yalnızca sınama

-   **Bağlantılı oturum açma** – seçin [bağlantılı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **tek oturum açma modu** mevcut tek oturum açma çözümüyle zaten bağlı olan bir uygulamanız varsa ya da yalnızca basit bir bağlantı, kullanıcılarınızın yayımlamak isterseniz kendi [uygulama erişim Paneli'ne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) veya [Office 365 uygulama Başlatıcı](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Parola tabanlı oturum açma** – seçin [parola tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **tek oturum açma modu** uygulamanız bir HTML kullanıcı adı ve parola alanı oluşturur ve bu kullanıcı adı ve parola güvenli bir şekilde uygulamayı daha sonra yeniden yürütülmesi depolamak istediğiniz

-   **SAML tabanlı oturum açma** – seçin [SAML tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) çoklu oturum açma, uygulamanızın SAML veya Openıd Connect protokollerini destekler veya SAML Taleplerde tanımladığınız kurallar temel alınarak belirli uygulama rollere kullanıcıları eşlemek istiyorsanız modu *(**Not:** uygulama proxy'si bir uygulama için yapılandırıldığında, bu seçenek kullanılamaz) *

-   **Üstbilgi tabanlı oturum açma** – bu seçin [üstbilgi tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) HTTP üstbilgisi destekleyen PingAccess kullanan bir uygulamanız varsa, tek oturum açma modu, çoklu oturum açmayı gerçekleştirmek istediğiniz kimlik doğrulaması tabanlı *(**Not:** bu seçenek yalnızca uygulama proxy'si ve PingAccess yapılandırıldığında bir uygulama için kullanılabilir) *

-   **Tümleşik Windows kimlik doğrulaması** – seçin [tümleşik Windows kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) çoklu oturum açma çoklu oturum açmayı gerçekleştirmek istediğiniz şirket içi WIA uygulamanın gösterme zaman modu *(**Not:** bu seçenek yalnızca uygulama proxy'si bir uygulama için yapılandırıldığında kullanılabilir) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Özel geliştirilmiş uygulamalar için çoklu oturum açma modları

Uygulamaları aracılığıyla geliştirilen özel sahip [özel geliştirilmiş uygulama](#_Custom-Developed_Applications) deneyimi de Yukarıda listelenmeyen ek tek oturum açma modunu destekler. Bunlar:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) oturum açma tabanlı

-   [Openıd Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) oturum açma tabanlı

-   [WS-Federasyon 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) oturum açma tabanlı

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) oturum açma tabanlı

Okuma [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) , bu tek oturum açma modunu destekler ve özel geliştirilmiş bir uygulama oluşturma hakkında daha fazla bilgi için.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Oturum açma uygulamanın tek modunu ayarlama

Bir uygulamanın ayarlamak için **çoklu oturum açma** modu, aşağıdaki yönergeleri izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](active-directory-application-proxy-sso-using-kcd.md)

