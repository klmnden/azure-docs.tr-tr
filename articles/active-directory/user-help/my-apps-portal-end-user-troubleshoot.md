---
title: Uygulamalarım portal - Azure Active Directory ile ilgili yardım alın | Microsoft Docs
description: Oturum açma ve uygulamalarım portalında ortak görevleri gerçekleştirme konusunda yardım alın.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: lizross
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6e87ae0a8adf28176d9a97cbf1b78943179884a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60475036"
---
# <a name="troubleshoot-problems-with-the-my-apps-portal"></a>Uygulamalarım portalı ile ilgili sorunları giderme
Açmayı veya kullanma konusunda sorun yaşıyorsanız **uygulamalarım** portal, bu sorun giderme ipuçları Yardım için yardım masasına veya yöneticinize başvurun önce deneyin.

## <a name="im-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>My Apps güvenli oturum açma uzantısı yükleme konusunda sorun yaşıyorum
My Apps güvenli oturum açma uzantısı yükleme sorunları yaşıyorsanız:

- Desteklenen bir tarayıcı kullandığınızdan emin olun dahil olmak üzere:

    - **Microsoft Edge.** Windows 10 Anniversary Edition veya sonrasını çalıştırıyor.
    - **Google Chrome.** Windows 7 veya üzeri ve Mac OS X veya daha sonra çalışıyor.
    - **Mozilla Firefox 26,0 veya üzeri.** Windows XP SP2 veya sonraki sürümlerde ve Mac OS X 10.6 veya sonraki sürümlerde çalışan.
    - **Internet Explorer 11.** Windows 7 veya üzeri (sınırlı destek) üzerinde çalışıyor.

- Tarayıcı uzantısı ayarlarınızı açık olduğundan emin olun.

- Deneyin tarayıcınızı yeniden başlatıp oturum açmayı **uygulamalarım** yeniden portalı.

- Tarayıcınızın tanımlama bilgilerini temizlemeyi deneyin ve ardından yeniden başlatın ve oturum **uygulamalarım** yeniden portalı.

## <a name="i-cant-sign-in-to-the-my-apps-portal"></a>Oturum **uygulamalarım** portalı
Oturum sorun yaşıyorsanız **uygulamalarım** portal, aşağıdakileri:

- Doğru URL'yi kullandığınızdan emin olun. Olmalıdır https://myapps.microsoft.com veya kuruluşunuz için özelleştirilmiş bir sayfa gibi https://myapps.microsoft.com/contoso.com.

- Parolanızı doğru olduğundan ve süresinin dolmadığından emin olun. Daha fazla bilgi için bkz. [iş veya Okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).

- Doğrulama bilgilerinizi güncel ve doğru olduğundan emin olun. Daha fazla bilgi için [Azure multi-Factor Authentication benim için ne demektir?](multi-factor-authentication-end-user.md) veya [güvenlik bilgisi yöntemleri ve bilgileri değiştirme](security-info-add-update-methods-overview.md).

- Ekleme **Uygulamam** portal URL'sine **Internet özellikleri > Güvenlik > Güvenilen siteler** ayarı.

- Tarayıcınızın önbelleğini temizleyin ve yeniden oturum açmayı deneyin.

## <a name="my-password-isnt-working"></a>Parolamı çalışmıyor
Parolanızı mı unuttunuz, hiçbir zaman bir kuruluşunuzdaki alınan, hesabınızın kilitli veya parolanızı değiştirmek için bkz [Yardım, Azure AD parolamı unuttum](active-directory-passwords-update-your-own-password.md).

## <a name="i-want-to-be-able-to-reset-my-own-password"></a>Kendi parolanızı sıfırlayamazsınız istiyorum
Kendi parolanızı sıfırlamak için yöneticinize ilk özellik kuruluşunuz için etkinleştirmeniz gerekir ve ardından gerekir güncelleştirin ve gerekli doğrulama yöntemlerinizi doğrulayın. Doğrulama yöntemlerinizi güncelleştirme hakkında daha fazla bilgi için bkz. [Self Servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).

## <a name="im-getting-an-access-denied-message-when-i-start-an-app"></a>Ben bir uygulama başlatıldığında bir erişim reddedildi iletisi alıyorum
Alıyorsanız, bir **erişim reddedildi** uygulama başlatıldıktan sonra ileti **Uygulamam** portal, aşağıdakileri:

- Yüklediğinizden emin olun [My Apps güvenli oturum açma uzantısı](my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension) ve kullanmakta olduğunuz bir [desteklenen bir tarayıcıdan](my-apps-portal-end-user-access.md#supported-browsers).

- Emin olun uygulama için doğru URL'yi kullanarak ve URL açıktır, **Internet özellikleri > Güvenlik > Güvenilen siteler** listesi.

- Parolanızı doğru olduğundan ve süresinin dolmadığından emin olun. Daha fazla bilgi için bkz. [iş veya Okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).

- Doğrulama bilgilerinizi güncel ve doğru olduğundan emin olun. Daha fazla bilgi için [Azure multi-Factor Authentication benim için ne demektir?](multi-factor-authentication-end-user.md) veya [güvenlik bilgisi yöntemleri ve bilgileri değiştirme](security-info-add-update-methods-overview.md).

- Tarayıcınızın önbelleğini temizleyin ve yeniden oturum açmayı deneyin.

Bunları denedikten sonra uygulamanızı hala erişemiyorsanız, Yardım almak için kuruluşunuzun yardım masasına başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
İçin oturum açtıktan sonra **uygulamalarım** portal ayrıca profilinizi güncelleştirebilirsiniz ve hesap bilgileri, grup bilgilerini ve erişim bilgileri gözden geçirin (iznine sahipseniz).

- [Erişmeyi ve uygulamaları uygulamalarım portalında](my-apps-portal-end-user-access.md).

- [Profil bilginizi değiştirmek](my-apps-portal-end-user-update-profile.md).

- [Grupları ile ilgili bilgilerinizi görebilecek ve](my-apps-portal-end-user-groups.md).

- [Kendi erişim gözden geçirmeleri gerçekleştirmek](my-apps-portal-end-user-access-reviews.md).