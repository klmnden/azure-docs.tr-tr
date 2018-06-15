---
title: Parola tek oturum açma için Azure AD galeri uygulamanın yapılandırma sorunu | Microsoft Docs
description: Azure AD uygulama galerisinde zaten listelenen uygulamalar için parola çoklu oturum açmayı yapılandırırken ortak sorunları kişiler yüz anlama
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
ms.openlocfilehash: f19b684a6c7426134844a2657b886280af2f061c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34067070"
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola tek oturum açma için Azure AD galeri uygulamanın yapılandırma sorunu

Bu makalede, yapılandırırken ortak sorunları kişiler yüzünü anlamanıza yardımcı olur **parola çoklu oturum açma** Azure AD galeri uygulamayla.

## <a name="credentials-are-filled-in-but-the-extension-does-not-submit-them"></a>Kimlik bilgileri girilir ancak uzantı bunları göndermez

Bu sorun genellikle uygulamanın satıcısına yakın zamanda bir alan eklemek için kullanıcıların oturum açma sayfası değiştirilen varsa, kullanıcı adı ve parola alanlarına algılamak için kullanılan tanımlayıcı değiştirilmiş veya nasıl oturum açma deneyimi, uygulama için çalışır değiştiren gerçekleşir. Neyse ki, çoğu durumda, Microsoft Hızlı bir şekilde bu sorunları gidermek için uygulama satıcıları ile çalışabilirsiniz.

Microsoft teknolojileri tümleştirmeler böldüğünüzde otomatik olarak algıla sahipken, sorunları hemen bulmak mümkün olmayabilir veya sorunları gidermek için biraz zaman alabilir. Bu tümleştirmeler birini düzgün çalışmıyor, durumda olabildiğince çabuk düzeltilebilir için bir destek servis talebi açın.

**Bu uygulamanın satıcısına iletişim kurmayan varsa** Microsoft bunları yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için ile çalışabilmek için bunları bizim şekilde gönderin. Satıcıya gönderebilirsiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](./develop/active-directory-app-gallery-listing.md) başlatılan getirmek için.

## <a name="credentials-are-filled-in-and-submitted-but-the-page-indicates-the-credentials-are-incorrect"></a>Kimlik bilgileri doldurulur ve gönderildi, ancak kimlik bilgileri yanlışsa sayfa gösterir

Bu sorunu çözmek için önce bunları deneyin:

-   Önce dener kullanıcınız **oturum uygulama Web sitesine doğrudan** kendileri için depolanan kimlik bilgileri.

  * Oturum açma çalışırsa, tıklatın kullanıcı sahip **güncelleştirme kimlik bilgileri** düğmesini **uygulama döşeme** içinde **uygulamaları** bölümünü [uygulama erişimi Panel](https://myapps.microsoft.com/) en son bilinen çalışma kullanıcı adı ve parola güncelleştirmek için.

   * Siz veya başka bir yöneticinin bu kullanıcı için kimlik bilgilerini atanmışsa, kullanıcı veya grubun uygulama atama giderek bulmak **kullanıcıları ve grupları** atama seçerek ve tıklatarakuygulamasınınsekmesi **Kimlik bilgilerini güncelleştirmeniz** düğmesi.

-   Kullanıcı kendi kimlik bilgilerini atanan kullanıcı varsa **parolalarını uygulamada dolduğunu değil emin olmak için onay** ve bu durumda, **süresi dolmuş parolasını güncelleştirmesi** oturum açarak uygulamayı doğrudan.

   * Parola uygulamada güncelleştirildikten sonra kullanıcının'ı isteği **güncelleştirme kimlik bilgileri** düğmesini **uygulama döşeme** içinde **uygulamaları** bölümü [Uygulama erişim Paneli'ne](https://myapps.microsoft.com/) en son bilinen çalışma kullanıcı adı ve parola güncelleştirmek için.

   * Siz veya başka bir yöneticinin bu kullanıcı için kimlik bilgilerini atanmışsa, kullanıcı veya grubun uygulama atama giderek bulmak **kullanıcıları ve grupları** atama seçerek ve tıklatarakuygulamasınınsekmesi **Kimlik bilgilerini güncelleştirmeniz** düğmesi.

-   Aşağıda açıklanan adımları izleyerek erişim paneli tarayıcı uzantısı güncelleştirmek için kullanıcının sahip [erişim paneli tarayıcı uzantısı yükleme](#how-to-install-the-access-panel-browser-extension) bölümü.

-   Erişim paneli tarayıcı uzantısı çalışır ve kullanıcı tarayıcıda etkin olduğundan emin olun.

-   Kullanıcılarınız uygulamaya erişim panelinde oturum açmak ayarlamadığınızdan emin olun **incognito, InPrivate ya da özel mod**. Erişim paneli uzantısı bu modda desteklenmiyor.

Yukarıdaki önerileri çalışmaz durumda geçici olarak Azure AD ile tümleştirme uygulamanın bozuk uygulama tarafında gerçekleşen bir değişikliği durumda olabilir. Uygulamanın satıcısına tanıtır Örneğin, bu giriş, hangi nedenler ayırmak için tümleştirme, gibi kendi, otomatik bir komut dosyası için el ile vs farklı şekilde davranan kendi sayfasında otomatik ortaya çıkabilir. Neyse ki, çoğu durumda, Microsoft Hızlı bir şekilde bu sorunları gidermek için uygulama satıcıları ile çalışabilirsiniz.

Microsoft uygulama tümleştirmeler böldüğünüzde otomatik olarak algılamak için teknolojileri sahipken, sorunları hemen bulmak mümkün olmayabilir veya sorunları gidermek için biraz zaman alabilir. Bir tümleştirme düzgün çalışmıyor, olabildiğince çabuk sabit almak için bir destek servis talebi açabilirsiniz. 

Bu, ek olarak **bu uygulamanın satıcısına iletişim kurmayan varsa** **bizim şekilde Gönder** bunları yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için ile çalışabilmek için. Satıcıya gönderebilirsiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](./develop/active-directory-app-gallery-listing.md) başlatılan getirmek için.

## <a name="the-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>Chrome ve Firefox, ancak Internet Explorer uzantısı çalışır

Bu sorun için iki ana nedeni vardır:

-   Web sitesi değilse, Internet Explorer'da etkin güvenlik ayarlarına bağlı olarak parçası bir **Güvenilen Bölge**, bazen bizim betik engellenmesi için uygulama yürütme.

  *  Bu sorunu çözmek için kullanıcıya isteyin **uygulamanın Web sitesi ekleme** için **Güvenilen siteler** içinde listesinde kendi **Internet Explorer güvenlik ayarlarını**. Kullanıcılarınıza gönderebilirsiniz [bir siteyi my Güvenilen siteler listesine eklemek nasıl](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) makale ayrıntılı yönergeler için.

-   Nadir durumlarda, Internet Explorer'ın güvenlik doğrulaması bazen bizim betik yürütme işlemi daha yavaş yüklemek sayfanın neden olabilir.

   * Ne yazık ki, bu durum tarayıcı sürümü, bilgisayarın hızı veya site ziyaret bağlı olarak değişebilir. Bu durumda, biz bu belirli uygulama tümleştirmesi giderebilmemiz desteğe başvurun öneririz.

Bu, ek olarak **bu uygulamanın satıcısına iletişim kurmayan varsa** **bizim şekilde Gönder** bunları yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için ile çalışabilmek için. Satıcıya gönderebilirsiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](./develop/active-directory-app-gallery-listing.md) başlatılan getirmek için.

## <a name="check-if-the-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Uygulamanın oturum açma sayfasına kısa süre önce değiştirildi veya ek bir alan gerektirir, denetleyin

Uygulamanın oturum açma sayfasına büyük ölçüde değiştiyse, bazen bu ayırmak bizim tümleştirmeler neden olur. Bir uygulamanın satıcısına deneyimlerini için bir oturum açma alanı, captcha veya çok faktörlü kimlik doğrulaması eklediğinde, bu örneğidir. Neyse ki, çoğu durumda, Microsoft Hızlı bir şekilde bu sorunları gidermek için uygulama satıcıları ile çalışabilirsiniz.

Microsoft uygulama tümleştirmeler böldüğünüzde otomatik olarak algılamak için teknolojileri sahipken, sorunları hemen bulmak mümkün olmayabilir veya sorunları gidermek için biraz zaman alabilir. Bir tümleştirme düzgün çalışmıyor, olabildiğince çabuk sabit almak için bir destek servis talebi açabilirsiniz. 

Bu, ek olarak **bu uygulamanın satıcısına iletişim kurmayan varsa** **bizim şekilde Gönder** bunları yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için ile çalışabilmek için. Satıcıya gönderebilirsiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](./develop/active-directory-app-gallery-listing.md) başlatılan getirmek için.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleme

Erişim paneli tarayıcı uzantısı yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  tıklatın bir **parola SSO uygulaması** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınıza alarak, indirme bağlantısı yönlendirilir. **Ekleme** tarayıcınız uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirmek** veya **izin** uzantısı.

6.  Bir kez yüklenir, **yeniden** tarayıcı oturumunda.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamaları

Ayrıca uzantısı Chrome ve Firefox için aşağıya doğrudan bağlantılarından yükleyebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

