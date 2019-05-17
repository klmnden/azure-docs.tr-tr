---
title: Portalımdaki ayrıntılı bağlantıyı kullanarak bir uygulamada oturum açarken sorun | Microsoft Docs
description: Azure AD kullanarak bir ayrıntılı bağlantı URL'si uygulama erişim sorunlarını giderme
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
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44825f32a13db0a221252c042dc9f23ec43a9c8f
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65825424"
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Uygulamaya portalımdaki ayrıntılı bağlantıyı kullanarak oturum açma sorunları

Erişim paneli için bir kullanıcı Azure Active Directory (görüntüleyin ve bulut tabanlı uygulamalar Azure AD Yöneticisi bunları erişim izni vermiştir başlatmak için Azure AD) bir iş veya Okul hesabıyla sağlayan bir web tabanlı bir portaldır. 

Bu uygulamalar kullanıcılar Azure AD portalında yapılandırılır. Uygulama düzgün yapılandırılmış ve kullanıcı veya kullanıcı, uygulamayı erişim panelinde görmek için üyesi olduğu gruba atandı.

Ayrıntılı bağlantılar veya kullanıcı erişimi URL'leri, kullanıcılar parola SSO uygulamalarını doğrudan kendi tarayıcılarının URL çubuklarından erişmek için kullanabilir bağlantılar verilmiştir. Bu bağlantıyı giderek kullanıcıların olması otomatik olarak uygulamaya ilk erişim paneline yönlendiren gitmek zorunda kalmadan imzalanmış. Bu, kullanıcıların bu uygulamalara Office 365 uygulama başlatıcısından erişmek için aynı bir bağlantıdır.

## <a name="general-issues-to-check-first"></a>İlk denetlenecek genel sorunlar

-   Kullanarak emin bir **tarayıcı** erişim panelinde en düşük gereksinimlerini karşılayan.

-   Kullanıcının tarayıcı, uygulamaya URL'sini ekledi emin olun, **Güvenilen siteler**.

-   Uygulama denetlediğinizden emin olun **yapılandırılmış** doğru.

-   Kullanıcı hesabı olduğundan emin olun **etkin** için oturum açma işlemleri.

-   Kullanıcı hesabı olduğundan emin olun **kilitli değil.**

-   Kullanıcının emin **parola süresi değil veya unutulursa.**

-   Emin **multi-Factor Authentication** kullanıcı erişimi engellemediğini.

-   Emin bir **koşullu erişim ilkesi** veya **kimlik koruması** İlkesi, kullanıcı erişimini engellemediğinden.

-   Emin olun kullanıcının **kimlik doğrulaması iletişim bilgileri** yürütülebilmesi çok faktörlü kimlik doğrulaması veya koşullu erişim ilkelerinin izin güncel olduğundan.

-   Tarayıcınızın tanımlama bilgilerini temizleyip tekrar oturum açmaya çalışırken de deneyin için emin olun.

## <a name="checking-the-deeplink"></a>Ayrıntılı bağlantı denetleniyor

Doğru ayrıntılı bağlantı olup olmadığını denetlemek için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

7. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

8. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

9. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

10. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

11. Onay için ayrıntılı bağlantı istediğiniz uygulamayı seçin.

12. Etiket Bul **kullanıcı erişim URL'SİNDEN**. Bu URL, ayrıntılı bağlantı eşleşmelidir.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  ' a tıklayın bir **parola SSO uygulama** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınız indirme bağlantısını yönlendirildiği temel. **Ekleme** tarayıcınızı uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirme** veya **izin** uzantısı.

6.  Yüklendiğinde, **yeniden** , tarayıcı oturumu.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamalarınız

Ayrıca uzantı Chrome ve Firefox için doğrudan bu bağlantıları karşıdan:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola çoklu oturum açma Azure AD galeri uygulaması için yapılandırma

Bir uygulamaya ait Azure AD galeri yapılandırmak için yapmanız gerekir:

-   [Uygulama Azure AD galeri ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Uygulama Azure AD galeri ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** TextBox'dan **Galeriden Ekle** bölümünde, uygulama adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin.

9.  Uygulama eklemek için tıklatın **Ekle**.

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola çoklu oturum açma için uygulamayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Modu **parola tabanlı oturum açma.**

9. [Uygulamaya kullanıcı atama](#how-to-assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri dışı bir uygulama için yapılandırma

Bir uygulamaya ait Azure AD galeri yapılandırmak için yapmanız gerekir:

-   [Galeri dışı bir uygulama ekleyin](#add-a-non-gallery-application)

-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Galeri dışı bir uygulama ekleyin

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  tıklayın **galeri dışı uygulama.**

7.  Uygulamanızda adını **adı** metin. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola çoklu oturum açma için uygulamayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    1.  Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modu **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**, kullanıcılara nereden girin, kullanıcı adını ve oturum açmak için parola URL'si. Oturum açma alanlarını URL'SİNDE görünür olduğundan emin olun.

10. Uygulamaya kullanıcı atama.

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcının uygulamaya doğrudan atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8. Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcı için eklenecek **seçili** listesinde, kullanıcının profil fotoğrafı veya logosu yanındaki onay kutusuna tıklayın.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına veya e-posta adresine göre arama** Arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili** listesi.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatması mümkün.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımlarını çözümleme sorunu yaparsanız. 

Aşağıdaki bilgilerle varsa bir destek bileti açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesi sırasında hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
