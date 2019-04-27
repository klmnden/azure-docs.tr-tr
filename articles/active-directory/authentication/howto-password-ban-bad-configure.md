---
title: Azure AD'de - Azure Active Directory zayıf parolalarda yasaklamak nasıl
description: Zayıf parolalarınızı Azure AD dinamik olarak yasaklanmış passwrords ile envirionment yasakla
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: a7f6dbc869db4a0a444d09a2dc234e171758c706
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60414877"
---
# <a name="configuring-the-custom-banned-password-list"></a>Özel yasaklı parola listesi yapılandırma

Çoğu kuruluş, kullanıcılarına okul, takımınızın veya bunları kolayca tahmin bırakarak ünlü kişi gibi sık kullanılan yerel sözcükleri kullanarak parolaları oluşturmak bulun. Microsoft'un özel yasaklı parola listesi, kurumlar değerlendirmek ve engellemek için dizeleri ekleyin, değiştirin veya parola sıfırlama kullanıcıların ve yöneticilerin denediğinizde parola listesine ek olarak genel yasaklandı.

## <a name="add-to-the-custom-list"></a>Özel listesine ekleyin

Özel yasaklı parola listesi yapılandırma, bir Azure Active Directory Premium P1 veya P2 lisansı gerektirir. Azure Active Directory Lisansı hakkında daha ayrıntılı bilgi için bkz: [Azure Active Directory fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory/). |

1. Oturum [Azure portalında](https://portal.azure.com) ve **Azure Active Directory**, **kimlik doğrulama yöntemleri**, ardından **parola koruması**.
1. Seçenek kümesi **zorla özel liste**, **Evet**.
1. Dizelere ekleme **özel parola listesine Yasaklanmış**, her satırda bir dize
   * Özel yasaklı parola listesi en fazla 1000 sözcükler içerebilir.
   * Özel yasaklı parola listesi büyük/küçük harf duyarlıdır.
   * Özel yasaklı parola listesi ortak karakter değiştirme göz önünde bulundurur.
      * Örnek: "o" ve "0" veya "a" ve "\@"
   * En az dize uzunluğu dört karakterdir ve en fazla 16 karakter.
1. Tüm dizeleri eklendiğinde tıklayın **Kaydet**.

> [!NOTE]
> Uygulanacak özel yasaklı parola listesi bu güncelleştirmeler için birkaç saat sürebilir.

![Azure Portalı'nda kimlik doğrulama yöntemleri altında özel yasaklı parola listesi değiştirme](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Nasıl çalışır?

Bir kullanıcının veya yöneticinin sıfırlar veya bir Azure AD parola değişiklikleri her zaman bir listede olmadığını onaylamak için yasaklı parola listelerini aracılığıyla akar. Bu onay, ayarlama veya Azure AD kullanarak tüm parolalar dahil edilir.

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Bir kullanıcı yasaklandı şeye parola sıfırlamaya çalıştığında şu hata iletisini görürler:

Ne yazık ki parolanızı bir sözcük, tümcecik veya parolanızı kolayca tahmin edilebilir olmasını sağlayan yapan deseni içerir. Lütfen farklı bir parola ile yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

[Yasaklı parola listelerini kavramsal genel bakış](concept-password-ban-bad.md)

[Azure AD parola koruması kavramsal genel bakış](concept-password-ban-bad-on-premises.md)

[Yasaklı parola listelerini ile şirket içi tümleştirmeyi etkinleştir](howto-password-ban-bad-on-premises.md)
