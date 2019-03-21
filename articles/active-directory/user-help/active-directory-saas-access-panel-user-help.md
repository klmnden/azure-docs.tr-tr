---
title: MyApps portalında - Azure Active Directory kullanarak ve erişim ile ilgili yardım alın | Microsoft Docs
description: Oturum açma ve erişim panelinde ortak görevleri gerçekleştirme konusunda yardım alın.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: lizross
ms.reviewer: japere
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: cad68afd02694ec471d032a554ed468691ff914b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58105480"
---
# <a name="troubleshoot-issues-with-accessing-and-using-the-myapps-portal"></a>Erişme ve MyApps portalında kullanarak sorunlarını giderme

MyApps portalı kullanarak veya oturum açma sorunları yaşıyorsanız, Yardım için yardım masasına veya yöneticinize başvurun önce sorun giderme ipuçları bu deneyin.

## <a name="i-am-having-trouble-signing-into-the-myapps-portal"></a>MyApps portalında imzalama konusunda sorun yaşıyorum

Bu genel ipuçlarını deneyin:

- İlk olarak, denetimi doğru URL'yi kullanarak görmek için [ https://myapps.microsoft.com ](https://myapps.microsoft.com).
- Güvenilen siteler, tarayıcınızın URL eklemeyi deneyin.
- Parolanızı doğru olduğundan ve sona ermediğinden emin olun. Daha fazla bilgi için [iş veya Okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).
- Kimlik doğrulaması iletişim bilgilerinizi en fazla tarih ve erişiminizi engellemeyen olduğundan emin olun. Daha fazla bilgi için [Azure multi-Factor Authentication benim için ne demektir?](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user).
- Tarayıcınızın tanımlama bilgilerini temizleyip deneyin ve sonra yeniden oturum açmayı deneyin.

Oturum açmaya çalışırken sorunlarla hala karşılaşıyorsanız, yöneticinize başvurun.

## <a name="i-seem-to-be-having-password-issues"></a>Parola sorunları yaşıyor görünüyor

Parolanızı mı unuttunuz, hiçbir zaman BT personelinizin birinden alınan, hesabınız kilitlenmiş veya parolanızı değiştirmek için bkz [Yardım, Azure AD parolamı unuttum](active-directory-passwords-update-your-own-password.md).

## <a name="i-need-to-register-for-password-reset"></a>Parola sıfırlama için kaydolmasını gerekir

Parolanızı sıfırlama veya Self Servis parola sıfırlama (SSPR) kullanarak birine konuşmak zorunda kalmadan hesabınızın kilidini açın. Bu işlevi kullanabilmeniz için önce kimlik doğrulama yöntemlerini kaydetmeniz veya yöneticinizin gerektirdiği önceden tanımlanmış kimlik doğrulama yöntemlerini onaylamanız gerekir. Daha fazla bilgi için [Self Servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).

## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>My Apps güvenli oturum açma uzantısı yükleme konusunda sorun yaşıyorum

MyApps portalında JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı çoklu oturum açma uygulamaları kullanıyorsanız, eşlik eden uzantısı de yüklü olmalıdır. Bu uzantı, parola tabanlı çoklu oturum açma uygulamaları için yapılandırılmış bir uygulama başlatıldığında otomatik olarak indirilir.

Tarayıcı aşağıdaki gereksinimleri karşıladığından emin olun:

- **Microsoft Edge**: Windows 10 Anniversary Edition veya sonrası.
- **Chrome**: Windows 7 veya daha sonra ve Mac OS X veya üzeri.
- **26,0 veya üzeri Firefox**: Windows XP SP2 veya sonraki sürümlerde ve Mac OS X 10.6 veya daha sonra.
- **Internet Explorer 11**: Windows 7 veya üzeri (sınırlı destek).

Ayrıca, doğrudan aşağıdaki sitelerden uzantısı da indirebilirsiniz:

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

Uzantıyı yükledikten ve sorunları yaşamaya aşağıdakileri deneyin:

- Uzantı etkinleştirildiğinden emin olmak için tarayıcı uzantısı ayarlarınızı kontrol edin.
- Tarayıcınızı yeniden başlatın ve MyApps portalında oturum açın.
- Tarayıcınızın tanımlama bilgilerini temizleyin ve MyApps portalında oturum açın.
- Bir tanılama aracı ve adım adım yönergeler için Internet Explorer uzantısı yapılandırma erişmek için bkz: [erişim paneli uzantısını Internet Explorer için sorun giderme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Kullanım uygulamalarım güvenli oturum açma uzantısı
* My Apps URL dışında kullanıyorsanız `https://myapps.microsoft.com`, aşağıdakileri yaparak, varsayılan URL yapılandırın:
   1. Siz *değil* uzantı açtığınız uzantı simgesine sağ tıklayın.
   2. Menüsünde **My Apps URL**.
   3. Varsayılan URL'yi seçin.
   4. Genişletme simgesini seçin.
   5. Uzantı oturum açmak için seçin **kullanmaya başlamak oturum**.

* Doğrudan tarayıcıdan uygulamayla oturum açmak için aşağıdakileri yapın:
   1. Uzantıyı yükledikten sonra ona seçerek oturum **kullanmaya başlamak oturum**.
   2. Uygulama oturum açma URL'si ile oturum açın.  
       Oturum açma URL'si genellikle oturum açma formunu görüntüleyen bir uygulama URL'dir.
      Uzantı, durumunu değiştirin ve parola kullanılabilir olduğunu bilmesini sağlar.
   3. Oturum açmak için genişletme simgesini seçin.

* Uzantı bir uygulamayı başlatmak için aşağıdakileri yapın:
   1. Uzantıyı yükledikten sonra ona seçerek oturum **kullanmaya başlamak oturum**.
   2. Alt menüsünü açmak için genişletme simgesini seçin.
   3. MyApps portalında kullanılabilir olan uygulama arayın.
   4. Arama sonuçları listesinde uygulamayı seçin.  
       Son üç uygulamaları kullandığınız görüntülenen **kısa süre önce kullanılan** kısayol listesi.

> [!NOTE]
> Bu seçenekler, yalnızca Microsoft Edge, Chrome ve Firefox için kullanılabilir.

## <a name="how-do-i-add-a-new-app"></a>Yeni bir uygulama nasıl ekleyebilirim?

1. Üzerinde **uygulamaları** sayfasında **uygulama Ekle**.
2. Ekleyin ve ardından istediğiniz uygulamayı arayın **Ekle**.

   > [!NOTE]
   > * Bu seçenek yalnızca yöneticiniz, hesabınız için etkinleştirilmiş erişebilirsiniz.
   > * Uygulama izni gerektiriyorsa, yönetici onayı için beklemeniz gerekebilir.

## <a name="how-do-i-manage-my-group-memberships"></a>Grubu Üyeliklerim nasıl yönetebilirim?

Seçin **grupları** kutucuğuna ve sonra aşağıdakilerden birini yapın:
* Altında bir grup oluşturmak için **olduğum gruplar**seçin **Grup Oluştur**ve ardından yönergeleri izleyin.
* Bir grubu altında katılmaya **ben de grupları**seçin **gruba Katıl**ve ardından yönergeleri izleyin.

   > [!NOTE]
   > * Bu seçenek yalnızca yöneticiniz, hesabınız için etkinleştirilmiş erişebilirsiniz.
   > * Bir grubun üyesi ise, ayrıntılarını görüntülemek ve grubu bırakın.
   > * Bir grubun bir sahibi olması durumunda, ayrıntılarını görüntülemek, ekleme veya üyeleri kaldırın ve grubundan ayrılmak.
