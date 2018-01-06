---
title: "Uygulama erişimi yükleme sorunu panel tarayıcı uzantısı | Microsoft Docs"
description: "Erişim paneli tarayıcı uzantısı yüklenirken karşılaşılan yaygın hataları gidermek nasıl"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 26dc5d5ffce84206450123132c0633c2aa323e9f
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="problem-installing-the-application-access-panel-browser-extension"></a>Uygulama erişimi yükleme sorun tarayıcı uzantısı paneli

Erişim paneli, Azure AD Yöneticisi erişim verildi görünümü ve başlatma bulut tabanlı uygulamalar Azure Active Directory (Azure AD) bir iş veya Okul hesabı olan bir kullanıcının sağlayan bir web tabanlı portal olmaktır. Azure AD sürümleri olan bir kullanıcı, Self Servis grup ve erişim paneli üzerinden uygulama yönetim özellikleri de kullanabilirsiniz. Erişim paneli Azure Portalı'ndan ayrıdır ve kullanıcıların bir Azure aboneliğine sahip olmasını gerektirmez.

Parola tabanlı çoklu oturum açma (SSO) erişimi Masası'nda kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde bu uzantıyı otomatik olarak yüklenir.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erişim paneli toplantı tarayıcı gereksinimleri

Erişim paneli JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı çoklu oturum açma (SSO) erişimi Masası'nda kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde bu uzantıyı otomatik olarak yüklenir.

Parola tabanlı, SSO için son kullanıcının tarayıcılar olabilir:

-   Edge Windows 10 Anniversary Edition veya daha yenisi 

-   Chrome--Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde

-   Firefox 26,0 veya daha sonra--Windows XP SP2 veya sonraki ve Mac OS X 10,6 veya üzeri

-   Internet Explorer 8, 9, 10, 11--Windows 7 veya üzeri (sınırlı destek)
## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleme

Erişim paneli tarayıcı uzantısı yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  tıklatın bir **parola SSO uygulaması** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınıza bağlı için karşıdan yükleme bağlantısı yönlendirilmiş. **Ekleme** tarayıcınız uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirmek** veya **izin** uzantısı.

6.  Bir kez yüklenir, **yeniden** tarayıcı oturumunda.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamaları

Ayrıca uzantısı Chrome ve kenar için aşağıdaki doğrudan bağlantılarından yükleyebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Edge erişim paneli uzantısı](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>Oturum açma My uygulamaları güvenli uzantısı nasıl kullanabilirim?
My uygulamalar varsayılan URL uzantısı değiştirme

Https://myapps.microsoft.com daha farklı bir My uygulamaları URL kullanıyorsanız, sonra varsayılan URL ancak aşağıdaki adımları yapılandırmanız gerekir:
1. Uzantısına, imzalanmamış sırada **sağ tıklatın** uzantısı simgesi.
2. Tıklayın **My uygulamaları URL Seç** menüsünde.
3. **Seçin** , varsayılan URL.
4. Uzantı simgesine tıklayın.
5. Oturum açma seçerek uzantısına **başlamak oturum**.

Bir uygulama tarayıcıdan doğrudan oturum açın
1. Oturum açma seçerek uzantısının uzantı yükledikten sonra **başlamak oturum**.
2. Gidin **URL oturum açma** oturum açmak için istediğiniz uygulama bu genellikle oturum açma formu görüntüleyen uygulama URL'sidir.
3. Uzantı durumunu değiştirme ve bir parola kullanılabilir olduğunu biliyorsanız, tıklayın izin **uzantısı simgesi** oturum açmak için.

Uzantı uygulama başlatma
1. Oturum açma seçerek uzantısının uzantı yükledikten sonra **başlamak oturum**.
2. Açmak için uzantı simgesini tıklatın, **menü**.
3. **Arama** My uygulamaları Portalı'nda kullanılabilir bir uygulama için.
4. Uygulamadan tıklayın **arama sonuçlarında** başlatmak için.
5. Başlatılan son üç uygulamalar da kategoride görüneceğini **kısa süre önce kullanılan** kısayol listesi

> [!NOTE]
> Bu seçenekler yalnızca kenar, Chrome, Firefox için kullanılabilir.

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için Grup İlkesi ayarı

Uzaktan erişim paneli uzantısı Internet Explorer için kullanıcılarınızın makinelere yüklemeniz olanak tanıyan Grup İlkesi ayarlayabilirsiniz.

Önkoşullar şunlardır:

-   Ayarlamış olduğunuz [Active Directory etki alanı Hizmetleri](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), ve kullanıcılarınızın makineler, etki alanına.

-   Grup İlkesi nesnesi (GPO) düzenlemek için "Ayarları düzenleme" izni olması gerekir. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri bu izne sahip: etki alanı yöneticileri, kuruluş yöneticileri ve Group Policy Creator Owners. [Daha fazla bilgi edinin](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Öğreticiyi izleyin [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısı dağıtma](active-directory-saas-ie-group-policy.md) adım adım yönergeler için Grup İlkesi yapılandırmak ve kullanıcılara dağıtma.

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Internet Explorer erişim paneli uzantı sorunlarını giderme

İzleyin [erişim paneli uzantısı Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md) uzantısı için IE yapılandırma erişim için bir tanılama aracı ve adım adım yönergeler Kılavuzu.

> [!NOTE]
> IE üzerinde sınırlı destek ve artık yeni yazılım güncelleştirmeleri alır. Edge önerilen tarayıcısıdır.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözmezse

bir destek bileti aşağıdaki bilgilerle varsa açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Tenantıd

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesine hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
