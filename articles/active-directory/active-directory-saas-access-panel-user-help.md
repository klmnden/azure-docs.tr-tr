---
title: Verilere erişme ve Azure Active Directory'de My uygulamaları Portalı'nı kullanarak Yardım alma | Microsoft Docs
description: Oturum açma ve erişim panelinde ortak görevleri gerçekleştirme ile ilgili yardım alın.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 681831dcc65fa74d8d2e26f58849140498843c49
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="troubleshoot-issues-with-accessing-and-using-the-my-apps-portal"></a>Verilere erişme ve My uygulamaları Portalı'nı kullanarak sorunları giderme

Oturum açma veya My uygulamaları Portalı'nı kullanarak sorunlar yaşıyorsanız, Yardım için yardım masasına veya yöneticinize başvurun önce bu sorun giderme ipuçları deneyin.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>My uygulamaları Portalı'na imzalama sorun yaşıyorum

Bu genel ipuçları deneyin:

- İlk olarak, onay doğru URL'yi kullanarak görmek için [ https://myapps.microsoft.com ](https://myapps.microsoft.com).
- Tarayıcınızın güvenilen siteler için URL eklemeyi deneyin.
- Parolanızı doğru olduğundan ve süresi geçmemiş emin olun. Daha fazla bilgi için bkz: [iş veya Okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).
- Kimlik doğrulama iletişim bilgilerinizi en fazla tarih ve erişiminizi engellemediğini olduğundan emin olmak için kontrol edin. Daha fazla bilgi için bkz: [ne Azure çok faktörlü kimlik doğrulaması için beni anlama geliyor?](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user).
- Tarayıcınızın tanımlama bilgilerini temizlemeyi deneyin ve yeniden oturum açmayı deneyin.

Hala oturum açmaya çalışırken sorunlarla karşılaşıyorsanız, sistem yöneticinize başvurun.


## <a name="i-seem-to-be-having-password-issues"></a>Parola sorunları yaşıyor göründüğü

Parolanızı mı unuttunuz, hiçbir zaman, BT personeliniz birinden alınan, hesabınızın kilitli veya parolanızı değiştirmek için bkz: [Yardım, unuttum Azure AD parolamı](active-directory-passwords-update-your-own-password.md).

## <a name="i-need-to-register-for-password-reset"></a>Parola sıfırlama için kaydetme gerekir

Parolanızı sıfırlamak veya birine Self Servis parola sıfırlama (SSPR) kullanarak seslendir gerek kalmadan hesabınızın kilidini açın. Bu işlevi kullanabilmek için kimlik doğrulama yöntemleri kaydedilemedi veya yöneticiniz gerektirir önceden tanımlanmış kimlik doğrulama yöntemlerini onaylayın. Daha fazla bilgi için bkz: [Self Servis parola sıfırlama için kaydetme](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Oturum açma My uygulamaları güvenli uzantısı yükleme sorun yaşıyorum

My uygulamaları portal JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı tek oturum açma uygulamalar kullanıyorsanız, eşlik eden uzantısı da yüklenmesi gerekir. Parola tabanlı tek oturum açma uygulamaları için yapılandırılmış bir uygulama başlattığınızda bu uzantıyı otomatik olarak yüklenir.

Aşağıdaki tarayıcı gereksinimleri karşıladığından emin olun:
- **Kenar**: Windows 10 Anniversary Edition veya sonraki sürümlerde.
- **Chrome**: Windows 7 veya daha sonra ve Mac OS x veya sonraki sürümlerde.
- **Firefox 26,0 veya üzeri**: Windows XP SP2 veya daha sonra ve Mac OS X 10.6 veya sonraki sürümlerde.
- **Internet Explorer 11**: Windows 7 veya üzeri (sınırlı destek).

Chrome ve aşağıdaki siteleri doğrudan kenarından uzantısı da yükleyebilirsiniz:

- [Chrome uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Edge uzantısı](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Uzantısı yüklediyseniz ve hala sorun yaşıyorsanız, aşağıdakileri deneyin:

- Uzantısının etkinleştirildiğinden emin olmak için tarayıcı uzantısı ayarlarınızı denetleyin.
- Tarayıcınızı yeniden başlatın ve My uygulamaları portalında oturum açın.
- Tarayıcınızın tanımlama bilgilerini temizleyin ve My uygulamaları portalında oturum açın.
- Bir tanılama aracı ve adım adım yönergeler için Internet Explorer uzantısı yapılandırma erişmek için bkz: [erişim paneli uzantısı Internet Explorer için sorun giderme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

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
> Bu seçenekler yalnızca kenar, Chrome ve Firefox için kullanılabilir.

## <a name="how-do-i-add-a-new-app"></a>Yeni bir uygulama nasıl eklenir?

1.  Üzerinde **uygulamaları** sayfasında, **uygulama Ekle**.
2.  Ekleyin ve ardından istediğiniz uygulamayı arayın **Ekle**.

   > [!NOTE]
   > * Yalnızca yöneticiniz, hesabınız için etkinleştirilmişse, bu seçenek erişebilirsiniz.
   > * Uygulama izni gerektiriyorsa, yönetici onayı için beklemeniz gerekebilir.
   > 

## <a name="how-do-i-manage-my-group-memberships"></a>My grup üyeliklerini nasıl yönetebilirim?

Seçin **grupları** kutucuğuna ve ardından aşağıdakilerden birini yapın: 
* Bir grup altında oluşturmak için **olduğum grupları**seçin **Grup Oluştur**ve ardından yönergeleri izleyin.
* Altında bir gruba katılması için **ben içinde grupları**seçin **gruba Katıl**ve ardından yönergeleri izleyin.

   > [!NOTE]
   > * Yalnızca yöneticiniz, hesabınız için etkinleştirilmişse, bu seçenek erişebilirsiniz.
   > * Bir grubun üyesi ise, ayrıntıları görüntülemek ve grubu bırakın.
   > * Bir grubun sahibi varsa, ayrıntılarını görüntülemek, eklemek veya üyeleri kaldırın ve grubu bırakın.
   >


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme bilgileri için bkz: [uygulama erişim paneli Web sitesi ya da mobil uygulama kullanarak sorunları](active-directory-application-access-panel-content-map.md).

