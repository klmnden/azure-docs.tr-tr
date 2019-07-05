---
title: Kimlik doğrulama yöntemleri kullanım & raporlama ınsights (Önizleme) - Azure Active Directory
description: Raporlama Azure AD Self Servis parola sıfırlama ve çok faktörlü kimlik doğrulaması kimlik doğrulama metodu kullanımı
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0f6a74308f1bc4a7b77576fb9f39f965de0a4f8
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561035"
---
# <a name="authentication-methods-usage--insights-preview"></a>Kimlik doğrulama yöntemleri kullanım & ınsights (Önizleme)

Kullanım ve öngörüleri kimlik doğrulama yöntemleri gibi özellikler Azure multi-Factor Authentication ve Self Servis parola sıfırlama için kuruluşunuza ilişkin bilgi sağlar. Bu raporlama özelliği, kuruluşunuz yöntemleri kayıtlı ve bunların nasıl kullanıldığını anlamak için araçlar sağlar.

## <a name="permissions-and-licenses"></a>İzinler ve lisansları

Aşağıdaki roller, kullanımını ve öngörüleri erişebilirsiniz:

- Genel Yönetici
- Güvenlik okuyucusu
- Güvenlik Yöneticisi
- Rapor okuyucu

Hiçbir ek lisans erişim kullanım bilgilerini ve öngörüleri için gereklidir. Azure multi-Factor Authentication ve Self Servis parola sıfırlama (SSPR) lisans bilgilerini bulunabilir [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="how-it-works"></a>Nasıl çalışır?

Kimlik doğrulama metodu kullanımı ve öngörüleri erişmek için:

1. [Azure portala](https://portal.azure.com) gidin.
1. Gözat **Azure Active Directory** > **parola sıfırlama** > **kullanım & ınsights**.
1. Gelen **kayıt** veya **kullanım** genel bakışlar seçebilirsiniz önceden filtrelenmiş açmak temel gereksinimlerinize göre filtrelemek için raporlar.

![Kullanım & ınsights genel bakış](./media/howto-authentication-methods-usage-insights/usage-insights-overview.png)

Kullanım ve öngörüleri doğrudan erişmek için Git [ https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade). Bu bağlantıyı kayıt genel bakış çıkarır.

Kullanıcıların kayıtlı kullanıcıların etkin ve kullanıcılarınız için aşağıdaki kayıt verisi kullanıcılar özellikli kutucukları göster:

- Kayıtlı: Kullanıcılar (veya yönetici), kuruluşunuzun SSPR veya çok faktörlü kimlik doğrulaması İlkesi karşılamak için yeterli kimlik doğrulama yöntemleri kayıtlıysanız kayıtlı kullanıcı olarak kabul edilir.
- Etkin: Bir kullanıcının SSPR ilkesi için kapsamdaki ise etkin olarak kabul edilir. SSPR için bir grup etkinleştirilirse, kullanıcının bu gruba olmaları durumunda etkin değerlendirilir. (Konukları hariç) kiracıdaki tüm kullanıcılar, etkin olarak kabul sonra SSPR tüm kullanıcılar için etkin olduğunda.
- Özelliği: Bunlar hem etkin ve kayıtlı olan bir kullanıcı uyumlu kabul edilir. Bu durum, SSPR herhangi bir zamanda gerekirse yapabileceği anlamına gelir.

Bu kutucuklar veya bunları ınsights tıklayarak kayıt ayrıntıları önceden filtrelenmiş bir listesi için çıkarır.

**Kayıtları** üzerindeki grafik **kayıt** sekmesi kimlik doğrulama yönteminin yöntemi kayıtları başarılı ve başarısız kimlik doğrulama sayısını gösterir. **Sıfırlar** üzerindeki grafik **kullanım** sekmesine, başarılı sayısını gösterir ve kimlik doğrulama yöntemi başarısız kimlik doğrulamaları sırasında parola sıfırlama akış.

Grafikleri birini tıklatarak önceden filtrelenmiş bir kayıt listesine taşıyın veya olayları sıfırlayın.

Üst, sağ üst köşede bulunan denetim kullanarak 24 saat, 7 gün veya 30 günlük kaydı ve sıfırlama grafikleri gösterilen denetim verilerini tarih aralığını değiştirebilirsiniz.

Kayıt verileri 

### <a name="registration-details"></a>Kayıt ayrıntıları

Tıklayarak **kayıtlı kullanıcılar**, **etkin kullanıcıların**, veya **kullanıcılar özellikli** kutucukları veya ınsights Getir, kayıt ayrıntılara.

Kayıt Ayrıntıları raporu, her kullanıcı için aşağıdaki bilgileri gösterir:

- Ad
- Kullanıcı adı
- Kayıt durumu (tümü, kayıtlı, kayıtlı değil)
- Durumu etkin (tümü, etkin, etkin değil)
- Uyumlu durumu (tümü özellikli, Not özellikli)
- Yöntemleri (uygulama içi bildirim, uygulama kodu, telefon araması, SMS, e-posta, güvenlik soruları)

Listenin en üstünde denetimleri kullanarak bir kullanıcıyı arayabilir ve kullanıcılara gösterilen sütunları temel alan listesini filtreleyin.

### <a name="reset-details"></a>Sıfırlama ayrıntıları

Kayıtları veya sıfırlar grafiklerin üzerine tıklayarak sıfırlama ayrıntılarını getirir.

Sıfırlama Ayrıntıları raporu kaydı ve sıfırlama olayları dahil olmak üzere son 30 güne ait gösterir:

- Ad
- Kullanıcı adı
- Özellik (tümü, kayıt, sıfırlama)
- Kimlik doğrulama yöntemini (uygulama içi bildirim, uygulama kodu, telefon araması, Office araması, SMS, e-posta, güvenlik soruları)
- (Tümü, başarı, başarısızlık) durumu

Listenin en üstünde denetimleri kullanarak bir kullanıcıyı arayabilir ve kullanıcılara gösterilen sütunları temel alan listesini filtreleyin.

## <a name="limitations"></a>Sınırlamalar

Bu raporlarda gösterilen verileri 60 dakikaya kadar geciktirilecek. Verilerinizi nasıl son olduğunu belirlemek için Azure portalında bir "Son yenilenme" alanı yok.

Kullanım ve öngörüleri verileri etkinlik raporları Azure multi-Factor Authentication ya da Azure AD oturum açma işlemleri raporu yer alan bilgileri için bir değişiklik değil.

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik doğrulama yöntemleri kullanım raporu API'si ile çalışma](https://docs.microsoft.com/graph/api/resources/authenticationmethods-usage-insights-overview?view=graph-rest-beta)
- [Kuruluşunuz için kimlik doğrulama yöntemlerini seçme](concept-authentication-methods.md)
- [Birleşik kayıt deneyimi](concept-registration-mfa-sspr-combined.md)
