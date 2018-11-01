---
title: Azure AD'de dinamik olarak yasaklanmış parolalar
description: Azure AD dinamik olarak yasaklanmış parolalar ortamınızdan Zayıf parolalar yasakla
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 4c5fead0a7f4634a8f5ee005114d24cae9a2590f
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739833"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Hatalı parola kuruluşunuzdaki ortadan kaldırın

|     |
| --- |
| Azure AD parola koruması ve özel yasaklı parola listesi Azure Active Directory genel Önizleme özellikleri şunlardır. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Endüstri liderlerinden aynı parolayı birden fazla yerde karmaşık hale ve/Password123 gibi basit yapmamaya kullanmayı söyleyin. Kuruluşların kullanıcıları kılavuzu takip ettiğiniz nasıl garanti edebilir? Nasıl bunlar kullanıcıların yaygın parolaları veya son veri ihlalleriyle dahil edilecek bilinen parolaları kullanmadığı emin olabilirim?

## <a name="global-banned-password-list"></a>Genel yasaklı parola listesi

Microsoft, siber suçluların her zaman bir adım önünde olmak için çalışmaktadır. Bu nedenle Azure AD kimlik koruması ekibi için yaygın olarak kullanılan ve güvenliği aşılan parolaları sürekli olarak arayın. Bunlar ardından genel yasaklı parola listesi çağrılma yeri çok yaygın olarak kabul edilen bu parolaları engelleyin. Siber suçlular kendi saldırılarında de benzer stratejiler kullanır, bu nedenle Microsoft, bu listenin içeriği herkese açık şekilde yayımlamaz. Bu güvenlik açığı olan parolaların Microsoft müşterileri için gerçek bir tehdit haline gelmeden önce engellenir. Geçerli güvenlik çalışmaları hakkında daha fazla bilgi için bkz: [Microsoft Güvenlik zekası raporu](https://www.microsoft.com/security/intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Önizleme: Özel parola listesine yasaklandı.

Bazı kuruluşlar kendi özelleştirmeleri genel yasaklı parola listesi üzerinde hangi Microsoft özel yasaklı parola listesi çağrıları ekleyerek güvenlik bir adım daha ileri almak isteyebilirsiniz. Kurumsal müşteriler gibi contoso marka adları, şirketinize özgü koşulları veya diğer öğeleri türevleri engellemek seçebilirsiniz.

Özel parola listesi ve şirket içi Active Directory Tümleştirmesi, Azure portalını kullanarak yönetilen sağlama yeteneği yasaklandı.

![Azure Portalı'nda kimlik doğrulama yöntemleri altında özel yasaklı parola listesi değiştirme](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Şirket içi karma senaryolar

Yalnızca bulut hesapları korumaya yardımcı olur, ancak çoğu kuruluş şirket içi Windows Server Active Directory de dahil olmak üzere karma senaryoları korur. Windows Server Active Directory (Önizleme) aracıları var olan altyapınızla yasaklı parola listelerini genişletmek şirket için Azure AD parola koruması yüklemek mümkündür. Kullanıcıların ve değiştirmek, yöneticilerin ayarlayın veya sıfırlanmış artık şirket içi yalnızca bulut kullanıcı olarak aynı parola ilkesiyle uyumlu gereklidir.

## <a name="how-does-the-banned-password-list-work"></a>Yasaklı parola listesi nasıl çalışır

Yasaklı parola listesi, küçük harf ve bilinen yasaklı parolalara benzer öğe eşleştirmesi olan 1'in bir düzenleme uzaklıkta karşılaştırma dizesi dönüştürerek parolalar listesinde eşleşir.

Örnek: Sözcük parola bir kuruluş için engellendi
   - Bir kullanıcı parolasını ayarlamak dener "P@ssword", "parola" dönüştürülür ve bir değişken parola olduğu için engellendi.
   - Bir yönetici kullanıcı parolasının "/ Password123!" için ayarlamaya çalışır Dönüştürülen "/ password123 için!" ve çünkü bu bir değişken parola engellenir.

Her bir kullanıcı sıfırlar veya Azure AD parolalarını yasaklı parola listede olmadığını onaylamak için bu süreci akışları değiştirir. Bu onay, karma senaryoları kullanarak Self Servis parola sıfırlama, parola karma eşitlemesi ve doğrudan kimlik doğrulama dahil edilir.

## <a name="license-requirements"></a>Lisans gereksinimleri

|   | Genel yasaklı parola listesi ile Azure AD parola koruması | Özel yasaklı parola listesi ile Azure AD parola koruması|
| --- | --- | --- |
| Yalnızca bulutta yer alan kullanıcılar | Azure AD Ücretsiz | Azure AD Basic |
| Şirket içi Windows Server Active Directory'de eşitlenen kullanıcılar | Azure AD Premium P1 veya P2 | Azure AD Premium P1 veya P2 |

Maliyetleri de dahil olmak üzere ek lisans bilgilerini bulunabilir [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Bir kullanıcı yasaklandı şeye parola sıfırlamaya çalıştığında şu hata iletisini görürler:

Ne yazık ki parolanızı bir sözcük, tümcecik veya parolanızı kolayca tahmin edilebilir olmasını sağlayan yapan deseni içerir. Lütfen farklı bir parola ile yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel yasaklı parola listesi yapılandırma](howto-password-ban-bad.md)
* [Etkinleştirme Azure AD Parola Koruması Aracısı şirket içi](howto-password-ban-bad-on-premises-deploy.md)
