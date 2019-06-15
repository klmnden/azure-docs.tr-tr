---
title: Parola için yapılandırılmış Azure AD galeri uygulamasında oturum açmada sorun çoklu oturum açma | Microsoft Docs
description: Azure AD galeri uygulaması ile ilgili sorunları giderme parola çoklu oturum açma için yapılandırılmış
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
ms.openlocfilehash: 0559213706c499878e0f14a0beeee22dcdbaf59a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65825156"
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Parola çoklu oturum açma için yapılandırılmış Azure AD galeri uygulamasında oturum açma sorunları

Erişim paneli, Azure AD Yöneticisi erişim izni görünümü ve başlatma bulut tabanlı uygulamaları Azure Active Directory (Azure AD) bir iş veya Okul hesabı olan bir kullanıcının sağlayan web tabanlı bir portal sağlamaktır. Azure AD sürümleri olan bir kullanıcı, Self Servis grup ve uygulama erişim panelinde üzerinden yönetim özellikleri de kullanabilirsiniz. Erişim paneli, Azure Portalı'ndan ayrıdır ve kullanıcıların bir Azure aboneliğine sahip olmasını gerektirmez.

Parola tabanlı çoklu oturum açma (SSO) erişim Paneli'nde kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bu uzantı, bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde otomatik olarak indirilir.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Toplantı için erişim paneli tarayıcı gereksinimleri

Erişim paneli JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı çoklu oturum açma (SSO) erişim Paneli'nde kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bu uzantı, bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde otomatik olarak indirilir.

Parola tabanlı SSO için son kullanıcının tarayıcılar olabilir:

-   Internet Explorer 8, 9, 10, 11 - Windows 7 veya üzeri

-   Chrome - Windows 7 ve daha sonra ve MacOS x veya sonrası

-   Firefox 26,0 veya daha sonra - Windows XP SP2 veya üstü ve Mac OS X 10,6 veya sonraki bir sürümü üzerinde

>[!NOTE]
>Parola tabanlı SSO uzantısı kullanılabilir hale gelir için Windows 10'daki Microsoft Edge tarayıcı uzantıları için Microsoft Edge desteklendiğinde haline gelir.
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  ' a tıklayın bir **parola SSO uygulama** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınıza bağlı olarak indirme bağlantısını yönlendirilir. **Ekleme** tarayıcınızı uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirme** veya **izin** uzantısı.

6.  Yüklendiğinde, **yeniden** , tarayıcı oturumu.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamalarınız

Ayrıca uzantısı Chrome ve Firefox için aşağıdaki doğrudan bağlantılardan indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için Grup İlkesi ayarlama

Uzaktan erişim paneli uzantısını Internet Explorer için kullanıcılarınızın makinelerde yüklemenize olanak sağlayan bir Grup İlkesi ayarlayabilirsiniz.

Önkoşullar şunlardır:

-   Ayarlamış olduğunuz [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), ve kullanıcılarınızın makineleri etki alanınıza katılmış.

-   Grup İlkesi nesnesi (GPO) düzenlemek için "Ayarları düzenleme" izniniz olmalıdır. Varsayılan olarak, şu güvenlik gruplarının üyeleri bu izne sahiptir: Etki alanı yöneticileri, kuruluş yöneticileri ve Grup İlkesi Oluşturucu Sahipleri. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Öğreticiyi izleyin [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) Grup İlkesi yapılandırmak ve kullanıcılara dağıtma konusunda adım adım yönergeler için.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Erişim paneli Internet Explorer'da sorunlarını giderme

İzleyin [erişim paneli uzantısını Internet Explorer için sorun giderme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) erişim için bir tanılama aracı ve adım adım yönergeler uzantısı IE için yapılandırma hakkında rehberlik.

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola çoklu oturum açma Azure AD galeri uygulaması için yapılandırma

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   Uygulama Azure AD galeri ekleme

-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

-   [Uygulamaya kullanıcı atama](#assign-users-to-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Uygulama Azure AD galeri ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** TextBox'dan **Galeriden Ekle** bölümünde, uygulama adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin.

9.  Tıklayın **Ekle** düğme, uygulamayı eklemek için.

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola çoklu oturum açma için uygulamayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Modu **parola tabanlı oturum açma.**

9. Uygulamaya kullanıcı atama.

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

### <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

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

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına veya e-posta adresine göre arama** Arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili** listesi.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatması mümkün.

## <a name="if-these-troubleshoot-steps-dont-resolve-the-issue"></a>Durumunda bu sorun giderme adımları sorunu yok 
Aşağıdaki bilgilerle varsa bir destek bileti açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesi sırasında hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
