---
title: Parola çoklu oturum açma Azure AD galeri uygulaması için yapılandırma sorunu | Microsoft Docs
description: Zaten Azure AD uygulama galerisinde listelenmiş uygulamalar için parola çoklu oturum açmayı yapılandırırken ortak sorunları insanların yüz anlama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: a35ef95074099499186eae0fadd37f1995d8e725
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190286"
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola çoklu oturum açma Azure AD galeri uygulaması için yapılandırma sorunu

Bu makale ortak sorunları insanların yüz yapılandırırken anlamanıza yardımcı olur **parola ile çoklu oturum açma** ile Azure AD galeri uygulaması.

## <a name="credentials-are-filled-in-but-the-extension-does-not-submit-them"></a>Kimlik bilgileri doldurulur ancak bunları uzantısı göndermez

Bu sorun genellikle uygulamanın satıcısına kısa bir süre önce bir alan eklemek için oturum açma sayfası değiştirildi varsa, kullanıcı adı ve parola alanları algılamak için kullanılan tanımlayıcı değiştirildi veya nasıl oturum açma deneyimi, uygulamanın çalıştığı değişiklik olur. Neyse ki, çoğu durumda, Microsoft bu sorunları hızlı bir şekilde çözmek için uygulama satıcıları ile çalışabilirsiniz.

Microsoft teknolojileri tümleştirmeler böldüğünüzde otomatik olarak algılamak için olsa da hemen sorunları bulmak mümkün olmayabilir veya sorunları düzeltmek için biraz zaman alabilir. Bu tümleştirmelere biri düzgün çalışmaz, olabildiğince çabuk düzeltilebilir şekilde durumda destek talebinde bulunun.

**Bu uygulamanın satıcısına kurmuş olduğunuz** Microsoft yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için bunlarla çalışabilmesi için bunları eklemeyeceğimize. Satıcıya gönderdiğiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](../develop/howto-app-gallery-listing.md) bunları kullanmaya almak için.

## <a name="credentials-are-filled-in-and-submitted-but-the-page-indicates-the-credentials-are-incorrect"></a>Kimlik bilgileri doldurulur ve gönderildi, ancak kimlik bilgileri hatalıdır sayfasını gösterir

Bu sorunu çözmek için önce bunları deneyin:

- İlk çalıştığınızda kullanıcının **oturum açın uygulama Web sitesine doğrudan** kendileri için depolanan kimlik bilgileri ile.

  * Oturum açma çalışırsa, ardından tıklayın kullanıcı sahip **kimlik bilgilerini güncelleştirme** düğmesini **uygulama kutucuğu** içinde **uygulamaları** bölümünü [uygulama erişimi Paneli](https://myapps.microsoft.com/) en son bilinen çalışma kullanıcı adı ve parola olarak güncelleştirmek için.

  * Bu kullanıcı için kimlik bilgilerini, siz veya başka bir yöneticinin atadıysanız, kullanıcının veya grubun uygulama ataması için giderek bulabilirsiniz **kullanıcıları ve grupları** atama seçip uygulamanınsekmesi **Kimlik bilgilerini güncelleştirme** düğmesi.

- Kullanıcı kendi kimlik bilgilerini atanmış ise, kullanıcı sahip **parolalarını uygulamada dolduğunu değil emin olmak için onay** ve bu durumda, **süresi dolmuş parolasını güncelleştirmesi** uygulamada oturum açarken tarafından doğrudan.

  * Uygulama parolası güncelleştirildikten sonra kullanıcının'ı istek **kimlik bilgilerini güncelleştirme** düğmesini **uygulama kutucuğu** içinde **uygulamaları** bölümünü [Uygulama erişim panelinde](https://myapps.microsoft.com/) en son bilinen çalışma kullanıcı adı ve parola olarak güncelleştirmek için.

  * Bu kullanıcı için kimlik bilgilerini, siz veya başka bir yöneticinin atadıysanız, kullanıcının veya grubun uygulama ataması için giderek bulabilirsiniz **kullanıcıları ve grupları** atama seçip uygulamanınsekmesi **Kimlik bilgilerini güncelleştirme** düğmesi.

- Aşağıdaki adımları izleyerek erişim paneli tarayıcı uzantısını güncelleştirme kullanıcı [erişim paneli tarayıcı uzantısını nasıl yükleyeceğiniz](#how-to-install-the-access-panel-browser-extension) bölümü.

- Erişim paneli tarayıcı uzantısını çalışır ve kullanıcı tarayıcısında etkin olduğundan emin olun.

- Kullanıcılarınız uygulamaya erişim panelinden oturum açmaya çalışırken değil olun **gizli, InPrivate veya özel modu**. Erişim paneli uzantısını bu modda desteklenmez.

Önceki öneriler çalışmaz durumda uygulamanın Azure AD ile tümleştirme geçici olarak bozuk uygulama tarafında gerçekleşen bir değişikliği durumda olabilir. Uygulamanın satıcısına getirir, örneğin, bu giriş, hangi nedenleri ayırmak için tümleştirme, kendi, gibi otomatikleştirilmiş bir betik, el ile vs için farklı davranır sayfasında otomatik ortaya çıkabilir. Neyse ki, çoğu durumda, Microsoft bu sorunları hızlı bir şekilde çözmek için uygulama satıcıları ile çalışabilirsiniz.

Microsoft teknolojileri uygulama tümleştirmeler böldüğünüzde otomatik olarak algılamak için olsa da hemen sorunları bulmak mümkün olmayabilir veya sorunları düzeltmek için biraz zaman alabilir. Bir tümleştirme düzgün çalışmaz, olabildiğince çabuk sorunu düzeltmesi için bir destek talebi açabilir. 

Bu, ek olarak **bu uygulamanın satıcısına kurmuş olduğunuz** **bunları eklemeyeceğimize** yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için bunlarla çalışabilmesi için. Satıcıya gönderdiğiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](../develop/howto-app-gallery-listing.md) bunları kullanmaya almak için.

## <a name="the-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>Uzantı, Chrome ve Firefox, ancak Internet Explorer'da çalışır.

Bu sorunun iki ana nedeni vardır:

- Web sitesi ise, Internet Explorer'da etkin güvenlik ayarlarına bağlı olarak parçası olan bir **güvenilen**, bazen betiğimizi engellenmesi için uygulamanın yürütülmesini.

  *  Bu sorunu çözmek için kullanıcıya bildirin **uygulamanın Web sitesini Ekle** için **Güvenilen siteler** içinde listesinde kendi **Internet Explorer güvenlik ayarlarınızın**. Kullanıcılarınızın gönderdiğiniz [my Güvenilen siteler listesine bir site ekleme](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) makalede ayrıntılı yönergeler için.

- Nadir durumlarda, Internet Explorer'ın güvenlik doğrulaması bazen daha yavaş yüklenir betiğimizi yürütülmesi için sayfayı neden olabilir.

  * Ne yazık ki bu durumda, tarayıcı sürümü, bilgisayar hızı veya ziyaret edilen site bağlı olarak farklılık gösterebilir. Bu durumda, biz bu belirli bir uygulama için tümleştirme düzeltebilmek için Destek birimine başvurmanızı öneririz.

Bu, ek olarak **bu uygulamanın satıcısına kurmuş olduğunuz** **bunları eklemeyeceğimize** yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için bunlarla çalışabilmesi için. Satıcıya gönderdiğiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](../develop/howto-app-gallery-listing.md) bunları kullanmaya almak için.

## <a name="check-if-the-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Uygulamanın oturum açma sayfası kısa bir süre önce değiştirildi veya başka bir alan gerektirir

Uygulamanın oturum açma sayfası önemli ölçüde değiştiyse, bazen bu bizim tümleştirmeler ayırmak neden olur. Bir uygulamanın satıcısına deneyimlerini için bir oturum açma alanı, captcha veya çok faktörlü kimlik doğrulaması eklediğinde, bu örneğidir. Neyse ki, çoğu durumda, Microsoft bu sorunları hızlı bir şekilde çözmek için uygulama satıcıları ile çalışabilirsiniz.

Microsoft teknolojileri uygulama tümleştirmeler böldüğünüzde otomatik olarak algılamak için olsa da hemen sorunları bulmak mümkün olmayabilir veya sorunları düzeltmek için biraz zaman alabilir. Bir tümleştirme düzgün çalışmaz, olabildiğince çabuk sorunu düzeltmesi için bir destek talebi açabilir. 

Bu, ek olarak **bu uygulamanın satıcısına kurmuş olduğunuz** **bunları eklemeyeceğimize** yerel olarak kendi uygulama Azure Active Directory ile tümleştirmek için bunlarla çalışabilmesi için. Satıcıya gönderdiğiniz [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](../develop/howto-app-gallery-listing.md) bunları kullanmaya almak için.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  ' a tıklayın bir **parola SSO uygulama** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınıza bağlı olarak, yükleme bağlantısını yönlendirilir. **Ekleme** tarayıcınızı uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirme** veya **izin** uzantısı.

6.  Yüklendiğinde, **yeniden** , tarayıcı oturumu.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamalarınız

Ayrıca uzantısı Chrome ve Firefox için aşağıdaki doğrudan bağlantılardan indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)

