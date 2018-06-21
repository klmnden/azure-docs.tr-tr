---
title: Azure AD'de parolaları bYönlendirme nasıl
description: Azure AD dinamik olarak yasaklanan passwrords ile envirionment zayıf parolalardan bBu
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 0c1517f94d4a6d59077b62614eec8fef665b1529
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296144"
---
# <a name="configuring-the-custom-banned-password-list"></a>Özel Engellenenler parola listesini yapılandırma

|     |
| --- |
| Azure AD parola koruması ve özel Engellenenler parola listesini Azure Active Directory genel Önizleme özellikleri verilmiştir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları'nı Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Birçok kuruluş, parolaları okul, spor takım veya tahmin kolay bırakarak ünlü kişi gibi sık kullanılan yerel sözcükleri kullanarak, kullanıcılar oluşturma bulun. Microsoft'un özel Engellenenler parola listesi kuruluşların değerlendirmek ve engellemek için dizeleri ekleme olanak tanır, kullanıcıların ve yöneticilerin değiştirmek veya parola sıfırlama girişiminde bulunduğunuzda genel yanı sıra parola listesine yasaklanan.

## <a name="add-to-the-custom-list"></a>Özel listeye ekleyin

Özel Engellenenler parola listesini yapılandırma bir Azure Active Directory Premium P1 veya P2 lisansı gerektirir. Azure Active Directory lisanslama hakkında daha ayrıntılı bilgi için bkz: [fiyatlandırma sayfası Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). |

1. Oturum [Azure portal](https://portal.azure.com) ve **Azure Active Directory**, **kimlik doğrulama yöntemleri**, ardından **parola protection (Önizleme)**.
1. Seçenek kümesi **zorla özel liste**, **Evet**.
1. Dizelere ekleme **özel parola listesine yasaklanan**, her satırda tek bir dize
   * Özel Engellenenler parola listenin en fazla 1000 sözcükler içerebilir.
   * Özel Engellenenler parola listesini büyük/küçük harf duyarlıdır.
   * Özel Engellenenler parola listesini ortak karakter değiştirme göz önünde bulundurur.
      * Örnek: "o" ve "0" veya "a" ve "@"
   * En az dize uzunluğu dört karakterdir ve en fazla 16 karakter.
1. Tüm dizeleri eklendiğinde tıklatın **kaydetmek**.

> [!NOTE]
> Uygulanacak özel Engellenenler parola listesine bu güncelleştirmeler için birkaç saat sürebilir.

![Azure portalında kimlik doğrulama yöntemleri altında özel Engellenenler parola listesini değiştirme](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Nasıl çalışır?

Bir kullanıcının veya yöneticinin sıfırlar veya bir Azure AD parola değişiklikleri her zaman bir listede olmadığını doğrulamak için Engellenenler parola listeleri aracılığıyla akar. Bu onay ayarlayın veya Azure AD kullanarak değiştirilen herhangi parolalarda dahil edilir.

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Bir kullanıcı bir şeye yasaklanan parola sıfırlama girişiminde bulunduğunda, aşağıdaki hata iletisini görürler:

Ne yazık ki parolanızı bir sözcük, tümcecik veya parolanızı kolayca guessable yapar düzeni içerir. Lütfen farklı bir parola ile yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

[Engellenenler parola listeleri kavramsal genel bakış](concept-password-ban-bad.md)

[Azure AD parola koruması kavramsal genel bakış](concept-password-ban-bad-on-premises.md)

[Engellenenler parola listeleri ile şirket içi tümleştirmeyi etkinleştir](howto-password-ban-bad-on-premises.md)
