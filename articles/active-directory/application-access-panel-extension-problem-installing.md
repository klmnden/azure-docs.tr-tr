---
title: Uygulama erişim paneli tarayıcı uzantısı - Azure yükleme | Microsoft Docs
description: Erişim paneli tarayıcı uzantısı yüklediğinizde karşılaşılan yaygın hataları düzeltin.
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
ms.date: 01/15/2018
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 3903e0f55e996d2ff793f17fb710843c5c64127f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleyin

Erişim paneli web tabanlı bir portalıdır. Bir iş veya Okul hesabı Azure Active Directory'de (Azure AD), görüntülemek ve bir Azure AD Yöneticisi için erişim izni bulut tabanlı uygulamalar başlatmak için erişim paneli kullanabilirsiniz. 

Azure AD sürümleri kullanıyorsanız, Self Servis grup ve erişim paneli aracılığıyla uygulama yönetim özelliklerini kullanabilirsiniz. 

Erişim paneli Azure Portalı'ndan ayrıdır. Bir Azure aboneliğinizin olmasını gerektirmez.

## <a name="web-browser-requirements"></a>Web tarayıcısı gereksinimleri

En azından, CSS etkin olduğundan ve erişim paneli JavaScript destekleyen bir tarayıcı gerektirir. Parola tabanlı SSO erişim panelinde aracılığıyla uygulamaları için oturum açmanız erişim paneli uzantı tarayıcınızda yüklü olmalıdır. Uzantı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinizde otomatik olarak yüklenir.

Parola tabanlı, SSO için aşağıdaki tarayıcılardan birini kullanabilirsiniz:

- **Kenar**: Windows 10 Anniversary Edition veya sonraki sürümlerde. 
- **Chrome**: Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde.
- **Firefox 26,0 veya üzeri**: Windows XP SP2 veya daha sonra ve Mac OS X 10.6 veya sonraki sürümlerde.

## <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleyin

Erişim paneli tarayıcı uzantısı yüklemek için aşağıdakileri yapın:

1.  Desteklenen tarayıcılar her birinde açmak [erişim paneli](https://myapps.microsoft.com)ve Azure AD hesabınızda bir kullanıcı olarak oturum açın.

2.  Bir parola tabanlı SSO uygulaması'nı seçin.

3.  İstendiğinde, seçin **Şimdi Yükle**.  
    İçin karşıdan yükleme bağlantısı için seçilen tarayıcınızın yönlendirilir. 
    
4.  **Add (Ekle)** seçeneğini belirleyin.

5.  Ya da istenirse **etkinleştirmek** veya **izin** uzantısı.

6.  Yükleme tamamlandıktan sonra tarayıcınızı yeniden başlatın.

7.  Erişim paneli ve onay için parola tabanlı SSO uygulamalarınızın Başlat olup olmadığını görmek için oturum açın.

Chrome ve aşağıdaki siteleri doğrudan kenarından uzantısı da yükleyebilirsiniz:

- [Chrome uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Edge uzantısı](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Kullanım uygulamalarım güvenli oturum açma uzantısı
* My uygulamaları URL dışında kullanıyorsanız `https://myapps.microsoft.com`, aşağıdakileri yaparak, varsayılan URL yapılandırın:
   1. Siz *değil* uzantısı açtığınız uzantısı simgesine sağ tıklayın.
   2. Menüsünde seçin **My uygulamaları URL**.
   3. Varsayılan URL seçin.
   4. Uzantı simgesini seçin.
   5. Uzantı oturum açmak için seçin **başlamak oturum**.

* Doğrudan bir uygulama tarayıcıdan oturum açmak için aşağıdakileri yapın:
   1. Uzantıyı yükledikten sonra kendisine seçerek oturum **başlamak oturum**.
   2. Uygulama oturum açma URL'si ile oturum açın.  
       Oturum açma genellikle oturum açma formu görüntüler uygulamanın URL'dir.
      Uzantı durumunu değiştirme ve bir parola kullanılabilir olduğunu biliyor sağlar.
   3. Oturum açmak için uzantı simgesini seçin.

* Bir uygulama uzantı başlatmak için aşağıdakileri yapın:
   1. Uzantıyı yükledikten sonra kendisine seçerek oturum **başlamak oturum**.
   2. Kendi menüsünü açmak için uzantı simgesini seçin.
   3. My uygulamaları Portalı'nda kullanılabilir olan bir uygulamayı arayın.
   4. Arama sonuçları listesinde uygulamayı seçin.  
       Kullandığınız son üç uygulamaları görüntülenen **kısa süre önce kullanılan** kısayol listesi.

> [!NOTE]
> Yukarıdaki seçeneklerden yalnızca kenar, Chrome ve Firefox için kullanılabilir.

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için bir Grup İlkesi ayarlama

Uzaktan erişim paneli uzantısı Internet Explorer için kullanıcılarınızın makinelerde yüklenmesine izin veren bir Grup İlkesi ayarlayabilirsiniz.

Bir Grup İlkesi ayarlamadan önce emin olun:

-   Ayarlamış olduğunuz [Active Directory etki alanı Hizmetleri](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), ve kullanıcılarınızın makineler, etki alanına.

-   Grup İlkesi nesnesi (GPO) düzenlemek için bilmeniz gereken *ayarlarını düzenleme* izinleri. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri için bu izin verilir: etki alanı yöneticileri, kuruluş yöneticileri ve Grup İlkesi Oluşturucu Sahipleri.

Grup İlkesi yapılandırma ve kullanıcılara dağıtma hakkında adım adım yönergeler için bkz: [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısı dağıtmak](active-directory-saas-ie-group-policy.md).

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Internet Explorer erişim paneli uzantı sorunlarını giderme

Bir tanılama aracı ve Internet Explorer uzantısı yapılandırma hakkında bilgi erişmek için bkz: [erişim paneli uzantısı Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md).

> [!NOTE]
> Internet Explorer üzerinde sınırlı destek ve artık yeni yazılım güncelleştirmeleri alır. Edge önerilen tarayıcısıdır.

## <a name="if-the-preceding-steps-do-not-resolve-the-issue"></a>Yukarıdaki adımlar sorunu çözmezse

Varsa aşağıdaki bilgileri içeren bir destek bileti açın:

-   Bağıntı hata kimliği
-   UPN (kullanıcı e-posta adresi)
-   Tenantıd
-   Tarayıcı türü
-   Saat dilimi ve saat veya hata oluştuğu sırada zaman çerçevesi
-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
