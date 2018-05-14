---
title: Parola için yapılandırılmış bir Azure AD galeri uygulaması oturumu açmada sorun çoklu oturum açma | Microsoft Docs
description: Parola çoklu oturum açma için yapılandırılmış Azure AD galeri uygulamaları için oturum açma ile ilgili sorunları gidermek için kılavuzluk sorunlu alanları açıklanır
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
ms.openlocfilehash: f13f9fa54af13dbbe435a846be35c9e06fe65455
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Parola çoklu oturum açma için yapılandırılmış bir Azure AD galeri uygulaması için oturum açma sorunları

Erişim paneli, Azure AD Yöneticisi erişim verildi görünümü ve başlatma bulut tabanlı uygulamalar Azure Active Directory (Azure AD) bir iş veya Okul hesabı olan bir kullanıcının sağlayan bir web tabanlı portal olmaktır. Azure AD sürümleri olan bir kullanıcı, Self Servis grup ve erişim paneli üzerinden uygulama yönetim özellikleri de kullanabilirsiniz. Erişim paneli Azure Portalı'ndan ayrıdır ve kullanıcıların bir Azure aboneliğine sahip olmasını gerektirmez.

Parola tabanlı çoklu oturum açma (SSO) erişimi Masası'nda kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde bu uzantıyı otomatik olarak yüklenir.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erişim paneli toplantı tarayıcı gereksinimleri

Erişim paneli JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı çoklu oturum açma (SSO) erişimi Masası'nda kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde bu uzantıyı otomatik olarak yüklenir.

Parola tabanlı, SSO için son kullanıcının tarayıcılar olabilir:

-   Internet Explorer 8, 9, 10, 11--Windows 7 veya üzeri

-   Chrome--Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde

-   Firefox 26,0 veya daha sonra--Windows XP SP2 veya sonraki ve Mac OS X 10,6 veya üzeri

>[!NOTE]
>Tarayıcı uzantıları köşesi desteklendiğinde hale kenar Windows 10 için kullanılabilir hale parola tabanlı SSO uzantısı.
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleme

Erişim paneli tarayıcı uzantısı yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  tıklatın bir **parola SSO uygulaması** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınıza bağlı için karşıdan yükleme bağlantısı yönlendirilmiş. **Ekleme** tarayıcınız uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirmek** veya **izin** uzantısı.

6.  Bir kez yüklenir, **yeniden** tarayıcı oturumunda.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamaları

Ayrıca uzantısı Chrome ve Firefox için aşağıya doğrudan bağlantılarından yükleyebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için Grup İlkesi ayarı

Uzaktan erişim paneli uzantısı Internet Explorer için kullanıcılarınızın makinelere yüklemeniz olanak tanıyan Grup İlkesi ayarlayabilirsiniz.

Önkoşullar şunlardır:

-   Ayarlamış olduğunuz [Active Directory etki alanı Hizmetleri](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), ve kullanıcılarınızın makineler, etki alanına.

-   Grup İlkesi nesnesi (GPO) düzenlemek için "Ayarları düzenleme" izni olması gerekir. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri bu izne sahip: etki alanı yöneticileri, kuruluş yöneticileri ve Group Policy Creator Owners. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Öğreticiyi izleyin [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısı dağıtma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) adım adım yönergeler için Grup İlkesi yapılandırmak ve kullanıcılara dağıtma.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Internet Explorer erişim panelinde sorun giderme

İzleyin [erişim paneli uzantısı Internet Explorer için sorun giderme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) uzantısı için IE yapılandırma erişim için bir tanılama aracı ve adım adım yönergeler Kılavuzu.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri olmayan uygulama için yapılandırma

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Bir galeri olmayan uygulama ekleme](#add-a-non-gallery-application)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

-   [Uygulamaya kullanıcılar atama](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a>Bir galeri olmayan uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

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

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**. Bu kullanıcılar, kullanıcı adını ve oturum açmak için parola girdiğiniz yere URL'dir. Oturum açma alanları URL'de göründüğünden emin olun.

10. Kullanıcılar uygulamayı atayın.

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

### <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcılar atama

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

11. Üzerine gelerek **kullanıcı** ortaya çıkarmak için listedeki bir **onay kutusunu**. Kullanıcının profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla kullanıcı ekleme**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına göre arama veya e-posta adresi** arama kutusu ve bu kullanıcıyı eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Kullanıcıların seçerek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklatın **atamak** uygulamayı Seçilen kullanıcılara atamak için düğmesi.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatabilir.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözümleme yaparsanız

bir destek bileti aşağıdaki bilgilerle varsa açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Tenantıd

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesine hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

