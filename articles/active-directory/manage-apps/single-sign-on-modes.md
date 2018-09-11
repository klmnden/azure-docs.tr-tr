---
title: Hangi çoklu oturum açma yönteminin kullanılacağını belirleme | Microsoft Docs
description: Azure AD tarafından desteklenen tek oturum açma modları anlayın ve nasıl hangisinin ilgilendiğiniz uygulama seçmek için seçin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: barbkess
ms.openlocfilehash: a9afb3f611a5762d329788b0aa7e1bc52e41fab3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44357418"
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Hangi çoklu oturum açma yönteminin kullanılacağını belirleme

Bu makalede, Azure AD tarafından desteklenen tek oturum açma modları anlamanıza yardımcı olmak ve nasıl hangisinin ilgilendiğiniz uygulama seçmek için seçin.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Çoklu oturum açma ve belirli bir uygulama türleri tarafından desteklenen modları sağlama

Aşağıdaki tabloda her önceki uygulama türleri tarafından desteklenen farklı tek oturum açma ve sağlama modları açıklanmaktadır. Bu tablo, belirli bir amaca desteklemek için eklemeniz gereken hangi uygulamanın anlamanıza yardımcı olması için kullanabilirsiniz.

  ![Uygulama türleri tablosu](./media/single-sign-on-modes/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Çoklu oturum açma modunu seçme

Aşağıda desteklenen **çoklu oturum açma** modları Azure AD uygulamaları için.

-   **Azure AD çoklu oturum devre dışı açma** – Azure AD çoklu oturum açma devre dışı seçin **çoklu oturum açma modunu** , henüz bu uygulama, çoklu oturum açma ile Azure AD ile tümleştirmek hazır olmayan veya teslim yalnızca sınama

-   **Bağlantılı oturum açma** – seçin [bağlantılı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **çoklu oturum açma modunu** zaten varolan çoklu oturum açma çözümü ile bağlı bir uygulamanız varsa ya da yalnızca istiyorsanız Basit bir bağlantı, kullanıcılarınızın yayımlamak kendi [uygulama erişim panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) veya [Office 365 uygulama Başlatıcısı](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Parola tabanlı oturum açma** – seçin [parola tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **çoklu oturum açma modunu** uygulamanız kullanıcı adı ve parola bir HTML alanı oluşturur ve bu kullanıcı adı depolamak istediğiniz ve güvenli bir şekilde uygulamayı daha sonra yeniden yürütülmesi gereken parola

-   **SAML tabanlı oturum açma** – seçin [SAML tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) çoklu oturum açma uygulamanız SAML veya Openıd Connect protokolleri destekler veya kurallara göre belirli uygulama rollerine kullanıcıları eşleştirmek istiyorsanız modu SAML, talepleri tanımladığınız *(** Not:** uygulama proxy'si bir uygulama için yapılandırıldığında bu seçenek kullanılamaz) *

-   **Üst bilgi tabanlı oturum açma** – bu [üst bilgi tabanlı oturum açma](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) çoklu oturum açma gerçekleştirmek istediğiniz HTTP üst bilgi tabanlı kimlik doğrulamasını destekleyen PingAccess kullanarak bir uygulama varsa tek oturum açma modu *(** Not:** bu seçenek yalnızca bir uygulama için uygulama proxy'si ile PingAccess yapılandırıldığında kullanılabilir) *

-   **Tümleşik Windows kimlik doğrulaması** – seçin [tümleşik Windows kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) çoklu oturum açma modu, çoklu oturum açma gerçekleştirmek istediğiniz şirket içi WIA uygulamanın gösterme *(** Not:** bu seçenek yalnızca bir uygulama için uygulama proxy'si yapılandırıldığında kullanılabilir) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Özel olarak geliştirilmiş uygulamaları için çoklu oturum açma modları

Uygulamalar aracılığıyla geliştirilmiş özel sahip [özel olarak geliştirilmiş bir uygulamada](#_Custom-Developed_Applications) deneyimi de içeren daha önce listelenen, ek tek oturum açma modları destekler:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) tabanlı oturum açma

-   [Openıd Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) tabanlı oturum açma

-   [WS-Federasyon 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) tabanlı oturum açma

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) tabanlı oturum açma

Okuma [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) , bu tek oturum açma modunu destekler ve özel geliştirilmiş bir uygulamada oluşturma hakkında daha fazla bilgi için.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Oturum açma uygulamanın tek modunu ayarlama

Bir uygulamanın ayarlanacak **çoklu oturum açma** modu, aşağıdaki yönergeleri izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)

