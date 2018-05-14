---
title: Bir ayrıntılı bağlantı kullanarak bir uygulama için oturum açma sorunları | Microsoft Docs
description: Bir uygulamayı Azure AD kullanarak bir ayrıntılı bağlantı URL'si erişim sorunlarını giderme
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
ms.openlocfilehash: 3bf357fef2aad85c45abb1fa8e06ff4420a6f14a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Bir ayrıntılı bağlantı kullanarak bir uygulama için oturum açma sorunları

Erişim için bir iş veya Okul hesabı Azure Active Directory'de (görüntülemek ve Azure AD Yöneticisi bunları erişim izni bulut tabanlı uygulamalar başlatmak için Azure AD) olan bir kullanıcı sağlayan bir web tabanlı portal panosudur. 

Bu uygulamalar, Azure AD portalında kullanıcı adına yapılandırılır. Uygulama düzgün şekilde yapılandırılıp yapılandırılmadığını ve kullanıcı veya kullanıcı erişim paneli uygulamasında görmek için bir üyesi olan bir gruba atanmış.

Ayrıntılı bağlantılar veya kullanıcı erişimi URL'leri kullanıcılarınızın doğrudan kendi tarayıcılar URL çubukları parola SSO uygulamalarına erişmek için kullanabilir bağlantılardır. Bu bağlantıyı giderek kullanıcıların olması otomatik olarak erişim paneline ilk gitmek zorunda kalmadan uygulamaya imzalanmış. Bu kullanıcılar bu uygulamalardan Office 365 uygulama Başlatıcısı'ndan erişmek için kullandıkları aynı bağlantıdır.

## <a name="general-issues-to-check-first"></a>İlk denetlemek için genel sorunlar

-   Kullanarak emin bir **tarayıcı** erişim paneli için minimum gereksinimleri karşılayan.

-   Kullanıcının tarayıcısı için uygulama URL'sini ekledi emin olun, **Güvenilen siteler**.

-   Uygulama denetlediğinizden emin olun **yapılandırılmış** doğru.

-   Kullanıcının hesabı olduğundan emin olun **etkin** oturum açma işlemleri için.

-   Kullanıcının hesabı olduğundan emin olun **kilitli değil.**

-   Kullanıcının emin olun **parola süresi değil veya unutulursa.**

-   Emin olun **çok faktörlü kimlik doğrulaması** kullanıcı erişimini engellemediğinden.

-   Emin olun bir **koşullu erişim ilkesi** veya **kimlik koruması** İlkesi kullanıcı erişimini engellemediğinden.

-   Olduğundan emin olun kullanıcının **kimlik doğrulaması kişi bilgilerini** yürütülebilmesi çok faktörlü kimlik doğrulaması veya koşullu erişim ilkeleri izin güncel değil.

-   Tarayıcınızın tanımlama bilgilerini temizleyip ve yeniden oturum açmaya de deneyin emin olun.

## <a name="checking-the-deeplink"></a>Ayrıntılı bağlantı denetleniyor

Doğru ayrıntılı bağlantı olup olmadığını denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

7.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

8.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

9.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

10. tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

11. Onay için ayrıntılı bağlantı istediğiniz uygulamayı seçin.

12. Etiketini bulun **kullanıcı erişim URL'si**. Bu URL, ayrıntılı bağlantı eşleşmelidir.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleme

Erişim paneli tarayıcı uzantısı yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  tıklatın bir **parola SSO uygulaması** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınız için karşıdan yükleme bağlantısı yönlendirilir temel. **Ekleme** tarayıcınız uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirmek** veya **izin** uzantısı.

6.  Bir kez yüklenir, **yeniden** tarayıcı oturumunda.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamaları

Ayrıca uzantısı Chrome ve Firefox için doğrudan bu bağlantılarından yükleyebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola tek oturum açma için Azure AD galeri uygulaması yapılandırma

Azure AD Galeriden bir uygulama yapılandırmak için şunları yapmalısınız:

-   [Azure AD Galeriden bir uygulama ekleme](#add-an-application-from-the-Azure-AD-gallery)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** metin **galerisinden Ekle** bölümünde, uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin kutusu.

9.  Uygulama eklemek için tıklatın **Ekle**.

Kısa bir süre sonra uygulamanın yapılandırma bölmesi görüyor.

### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  [Uygulamaya kullanıcılar atama](#how-to-assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri olmayan uygulama için yapılandırma

Azure AD Galeriden bir uygulama yapılandırmak için şunları yapmalısınız:

-   [Bir galeri olmayan uygulama ekleme](#add-a-non-gallery-application)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Bir galeri olmayan uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  tıklatın **olmayan galeri uygulaması.**

7.  Uygulamanızda adını girin **adı** metin kutusu. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma bölmesi görüyor.

### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    1.  Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**, kullanıcıların nerede girdiği kullanıcı adı ve parola oturum açmak için URL. Oturum açma alanları URL'de göründüğünden emin olun.

10. Kullanıcılar uygulamayı atayın.

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Kullanıcının uygulamaya doğrudan atama

Bir veya daha fazla kullanıcının uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** bölmesi.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama ilgilenen kullanıcının **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **kullanıcı** ortaya çıkarmak için listedeki bir **onay kutusunu**. Kullanıcı eklemek için **seçili** listesinde, kullanıcının profil fotoğrafınız veya logosu yanındaki onay kutusuna tıklayın.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla kullanıcı ekleme**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına göre arama veya e-posta adresi** arama kutusu ve bu kullanıcıyı eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Kullanıcıların seçerek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklatın **atamak** uygulamayı Seçilen kullanıcılara atamak için düğmesi.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatabilir.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözümleme yaparsanız. 

bir destek bileti aşağıdaki bilgilerle varsa açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Tenantıd

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesine hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
