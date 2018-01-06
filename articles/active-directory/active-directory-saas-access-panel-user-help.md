---
title: "Azure Active Directory'de My uygulamaları portal ile ilgili Yardım gerekiyor mu | Microsoft Docs"
description: "Erişim paneli ile çalışırken, ortak görevleri gerçekleştirmek için yönergeler alın."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 7a7a5d04c55adc33db5ccce761efd622935acefb
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="do-you-need-help-with-the-my-apps-portal"></a>My uygulamaları portalıyla Yardım gerekiyor mu?

Ne yazık ki bir sorun My uygulamaları portal kullanırken çalıştırdığınız için bu sayfayı büyük olasılıkla üst sınırına ulaştınız. Yardım Masası veya bir sorun Çözüldü almak için yöneticinize başvurun ihtiyaç duyduğunuz durumlarda olsa da, ilk yardımcı olabilecek bazı sorun giderme konularına aşağıda verilmiştir.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>My uygulamaları Portalı'na imzalama sorun yaşıyorum

Denetlemek için genel sorunlar:

- Doğru URL'de imzalama olmadığını denetleyin: [https://myapps.microsoft.com](https://myapps.microsoft.com)

- Tarayıcınızın güvenilen siteler için URL eklemeyi deneyin.

- Parolanızı süresi dolmuş ya da unutulursa emin olun. Denetleme [burada](active-directory-passwords-update-your-own-password.md) parolanızı güncelleştirme hakkında daha fazla ayrıntı için.

- Kimlik doğrulama kişi bilgilerinizi en fazla tarih ve erişiminizi engellemediğini olup olmadığını denetleyin. Denetleme [burada](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user) kimlik bilgilerinizi ayarlama hakkında daha fazla bilgi.

- Tarayıcınızın tanımlama bilgilerini temizlemeyi deneyin ve yeniden oturum açmayı deneyin.

Lütfen, hala oturum açmaya çalışırken sorunlarla karşılaşıyorsanız, daha fazla yardım için yöneticinize başvurun.


## <a name="how-do-i-update-my-password"></a>Parolamı nasıl güncelleştiririm?

Parolanızı unuttuysanız, hiçbir zaman, BT personeliniz birinden alınan, hesap ya da değiştirmek için bkz: istediğiniz dışında kilitlendiğinden [Yardım, unuttum Azure AD parolamı](active-directory-passwords-update-your-own-password.md) daha fazla ayrıntı için.

## <a name="how-do-i-register-for-password-reset"></a>Parola sıfırlama için nasıl kaydettirebilirim?

Bir son kullanıcı parolanızı sıfırlama veya Self Servis parola sıfırlama (SSPR) kullanan bir kişi için seslendir gerek kalmadan hesabınızın kilidini açın. Bu işlevi kullanabilmeniz için önce kimlik doğrulama yöntemlerini kaydetmeniz veya yöneticinizin doldurduğu önceden tanımlı kimlik doğrulama yöntemlerini onaylamanız gerekir. Daha fazla ayrıntı için bkz: [Self Servis parola sıfırlama için kaydetme](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Oturum açma My uygulamaları güvenli uzantısı yükleme sorun yaşıyorum

Tarayıcı gereksinimleri karşılayıp karşılamadığını denetleyin:

- Portal, JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı tek oturum açma uygulamalar kullanıyorsanız, eşlik eden uzantısı da yüklenmesi gerekir. Bu uzantı, parola tabanlı tek oturum açma uygulamaları için yapılandırılmış bir uygulama başlattığında otomatik olarak yüklenir.

- Uzantı için tarayıcı gereksinimleri şunlardır:
    - Edge Windows 10 Anniversary Edition veya daha yenisi
    - Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde chrome
    - Firefox 26,0 veya daha sonra Windows XP SP2 veya daha sonra ve Mac OS X 10.6 üzerinde veya üstü
    - Internet Explorer 8, 9, 10, 11 Windows 7 veya üzeri (sınırlı destek)

Ayrıca uzantıyı Chrome ve kenar için aşağıdaki doğrudan bağlantılarından indirebilirsiniz:

- [Chrome uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

- [Edge uzantısı](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Yükleme çalıştıktan sonra aşağıdaki adımları, sorunlarla karşılaşıyorsanız:

- Tarayıcı uzantısı ayarlarınızı uzantısı etkin olduğunu denetleyin.

- Tarayıcınızı yeniden başlatın ve My uygulamaları portalında oturum açın.

- Tarayıcınızın tanımlama bilgilerini temizleyin ve My uygulamaları portalında oturum açın.
- İzleyin [erişim paneli uzantısı Internet Explorer için sorun giderme](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-ie-troubleshooting) için IE Uzantısı Yapılandırma Kılavuzu erişim için bir tanılama aracı ve adım adım yönergeler.

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
3. Uzantı durumunu değiştirme ve bir parola kullanılabilir olduğunu biliyorsanız, tıklayın izin **uzantısı simgesi** oturum açmak için

Uzantı uygulama başlatma
1. Oturum açma seçerek uzantısının uzantı yükledikten sonra **başlamak oturum**.
2. Kendi menüsünü açmak için uzantı simgesine tıklayın.
3. **Arama** My uygulamaları Portalı'nda kullanılabilir bir uygulama için.
4. Uygulamadan tıklayın **arama sonuçlarında** başlatmak için.
5. Başlatılan son üç uygulamalar da kategoride görüneceğini **kısa süre önce kullanılan** kısayol listesi

> [!NOTE]
> Bu seçenekler yalnızca kenar, Chrome, Firefox için kullanılabilir.

## <a name="how-do-i-add-a-new-app"></a>Yeni bir uygulama nasıl eklenir?

1.  Üzerinde **uygulamaları** sayfasında, **uygulama Ekle**.

2.  Ekleyin ve ardından istediğiniz uygulamayı arayın **Ekle**.

**Notlar:**

- Yöneticiniz hesabınız için yapılandırılmışsa, yalnızca bu seçenek erişimi.

- Uygulama izni gerektiriyorsa, yönetici onayı için beklemeniz gerekebilir.


## <a name="how-do-i-manage-my-group-memberships"></a>My grup üyeliklerini nasıl yönetebilirim?

1. Tıklatın **grupları** döşeme. 
2. I kendi, gruplar altında bir grup oluşturmak için tıklatın **Grup Oluştur**ve ardından yönergeleri izleyin.
3. Ben, gruplar altında bir gruba katılması için tıklatın **gruba Katıl**ve ardından yönergeleri izleyin.

**Notlar:**

- Yöneticiniz hesabınız için yapılandırılmışsa, yalnızca bu seçenek erişimi.

- Bir üyesi olan gruplar ayrıntıları görüntülemek ve gruptan ayrılmak sağlar.

- Gruplar sahibi ayrıntılarını görüntülemek sağlar, ekleme veya üyeleri kaldırın ve grubu bırakın.


## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme ilgili bilgileri için bkz: [uygulama erişim paneli Web sitesi ya da mobil uygulama kullanma ile ilgili sorunlar](active-directory-application-access-panel-content-map.md)

