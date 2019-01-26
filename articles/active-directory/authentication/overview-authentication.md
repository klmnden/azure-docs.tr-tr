---
title: Azure Active Directory kullanıcı kimlik doğrulaması
description: Azure AD yöneticisi olarak son kullanıcıya fazla yansıtmadan kullanıcı kimlik doğrulamasını nasıl koruyabilirim?
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: overview
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: d14b44a45d9001e945e559dcd080683220c6a0b7
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076409"
---
# <a name="what-methods-are-available-for-authentication"></a>Hangi kimlik doğrulama yöntemleri kullanılabilir?

Her gün çalınan parolalar ve gizliliği tehlikeye giren kimliklerle ilgili yeni haberler görüyoruz. Parolaya ek olarak ikinci bir adımın eklenmesi, kuruluşunuzun güvenliğini anında artıran yöntemlerden biridir. Microsoft Azure Active Directory (Azure AD), ek kimlik doğrulama yöntemleri ile yöneticilerin kuruluşlarının ve kullanıcılarının güvenliğini korumasına yardımcı olmak için Azure Multi-Factor Authentication (Azure MFA) ve Azure AD self servis parola sıfırlama (SSPR) gibi özellikler sunmaktadır.

Dahil birçok senaryo vardır: Windows Hello ve diğerleri etkinleştirme parolasını sıfırlama bir uygulamada oturum açarken, kullanıcılarınızın ek doğrulama kimin söyledikleri olmalarını sağlamak için istenebilir.

Ek doğrulama adımları aşağıdaki gibi kimlik doğrulama yöntemleri olabilir:

* E-posta veya SMS ile iletilen bir kod
* Telefon araması
* Telefona gelen bildirim veya kod
* Güvenlik sorularını yanıtlama

![Chrome'da örnek login.microsoftonline.com oturum açma sayfası](media/overview-authentication/overview-login.png)

Azure MFA ve Azure AD self servis parola sıfırlama özellikleri, yöneticilerin kuruluşlarını korumak için Azure AD ve Azure portal kullanarak yapılandırma, ilke, izleme ve raporlama konularında denetim sahibi olmasını sağlar.

## <a name="self-service-password-reset"></a>Self servis parola sıfırlama

Self servis parola sıfırlama, kullanıcıların istediği yerden ve istediği zaman yönetici müdahalesi olmadan parolalarını sıfırlamasını sağlar.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

Self servis parola sıfırlama şu adımlardan oluşur:

* **Parola değiştirme:** Parolamı biliyorum, ancak yeni bir şeyle değiştirmek istediğiniz bildirimi.
* **Parola sıfırlama:** Ben oturum açamaz ve bir veya daha fazla onaylanmış kimlik doğrulama yöntemleri kullanan parolamı sıfırlamak istiyor.
* **Hesap kilidinizi açın:** Hesabımı kilitli ve bir veya daha fazla onaylanmış kimlik doğrulama yöntemleri kullanarak kilidini açmak istediğiniz olduğundan oturum açamıyorum.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Yönetici onaylı kimlik doğrulama yöntemlerini kullanan Azure MFA, oturum açma sürecini karmaşık hale getirmeden verilerinize ve uygulamalarınıza erişimin güvenli hale getirilmesine yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım self servis parola sıfırlama ve Azure Multi-Factor Authentication özelliklerini ayrıntılı bir şekilde incelemek ve yapılandırmak olacaktır.

Self servis parola sıfırlama özelliğini kullanmaya başlamak için bkz. [SSPR 'yi etkinleştirme hızlı başlangıç makalesi](quickstart-sspr.md).

Self Servis parola sıfırlama makalede hakkında daha fazla bilgi [nasıl çalışır: Azure AD Self Servis parola sıfırlama](concept-sspr-howitworks.md)

Makalede, Azure multi-Factor Authentication hakkında daha fazla bilgi edinin [nasıl çalışır: Azure çok faktörlü kimlik doğrulaması](concept-mfa-howitworks.md)
