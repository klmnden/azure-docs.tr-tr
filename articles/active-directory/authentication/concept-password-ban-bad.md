---
title: Dinamik olarak Azure AD'de parolaları yasaklanan
description: BBu zayıf parolalardan ortamınız ile dinamik olarak yasaklanan Azure AD parolalarını
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 89cbe386d87c6ccb81df7fabd86b197bb69e41e1
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295613"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Kuruluşunuzdaki bozuk parola ortadan kaldırma

|     |
| --- |
| Azure AD parola koruması ve özel Engellenenler parola listesini Azure Active Directory genel Önizleme özellikleri verilmiştir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları'nı Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Endüstri kılavuzları aynı parolayı birden fazla yerde karmaşık hale ve Password123 gibi basit yapmamaya kullanmamayı söyleyin. Kuruluşlar, kullanıcılar kılavuzunu takip nasıl garanti edebilir mi? Nasıl bunlar ortak parolalar veya yeni veri ihlalleriyle dahil edilecek bilinen parolaları kullanıcıların kullanmadığınız emin olabilir?

## <a name="global-banned-password-list"></a>Genel Engellenenler parola listesi

Microsoft, her zaman tek bir adımda siber suçlular öncesinde kalmak için çalışmaktadır. Bu nedenle Azure AD Identity Protection ekibine sürekli için yaygın olarak kullanılan ve güvenliği aşılan parolaları arayın. Bunlar daha sonra genel Engellenenler parola listesini adlı çok genel olarak kabul edilen Bu parolalar engelleyin. Siber suçlular benzer stratejileri kendi saldırılarında de, bu nedenle Microsoft bu listenin içeriği genel olarak yayımlamaz. Microsoft'un müşterileri için gerçek bir tehdit haline gelmeden önce bu güvenlik açığından parolalar engellenir. Geçerli güvenlik çalışmaları hakkında daha fazla bilgi için bkz: [Microsoft Güvenlik Intelligence rapor](https://www.microsoft.com/security/intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Önizleme: Özel parola listesine yasaklanan

Bazı kuruluşlar kendi özelleştirmelerini genel Engellenenler parola listenin en üstünde ne Microsoft özel Engellenenler parola listesini çağrıları ekleyerek güvenlik bir adım öteye isteyebilirsiniz. Contoso, marka adları, şirkete özgü koşulları veya diğer öğeleri çeşitlemelerini engellemek tercih edebilirsiniz gibi Kurumsal müşteriler.

Özel parola listesi ve şirket içi Active Directory Tümleştirme Azure portalını kullanarak yönetilen etkinleştirme özelliği yasaklanan.

![Azure portalında kimlik doğrulama yöntemleri altında özel Engellenenler parola listesini değiştirme](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Şirket içi karma senaryolar

Yalnızca bulut hesapları korumaya yardımcı olur ancak birçok kuruluşun şirket içi Windows Server Active Directory dahil olmak üzere karma senaryoları korur. Windows Server Active Directory (Önizleme) aracıları, mevcut altyapınızı Engellenenler parola listelerine genişletmek şirket için Azure AD parola koruması yüklemek mümkündür. Kullanıcılar ve değiştirmek, yöneticiler ayarlayın veya parolalarını sıfırlama artık şirket içi yalnızca bulut kullanıcılar aynı parola ilkesiyle uyum sağlamak için gereklidir.

## <a name="how-does-the-banned-password-list-work"></a>Engellenenler parola listesini nasıl çalışır

Engellenenler parola listesini, küçük harf ve benzer eşleşen 1 bir düzen uzaklıkta bilinen Engellenenler parolalar karşılaştırma dizesi dönüştürerek parolalar listesinde eşleşir.

Örnek: Bir kuruluş için word parola engellendi
   - Bir kullanıcı parolasını ayarlamak denediğinde "P@ssword", "parola" dönüştürülür ve parolayı değişik olduğundan engellendi.
   - Yönetici kullanıcıların bir parola "Password123!" olarak ayarlanmış dener "password123!" dönüştürülen ve çünkü parolayı değişik engellenir.

Her bir kullanıcı sıfırlar veya Azure AD parolalarını Engellenenler parola listede olmadığını doğrulamak için bu işlemi aracılığıyla akar değiştirir. Bu denetimi senaryoları kullanarak Self Servis parola sıfırlama karma, parola karma eşitlemesi ve geçişli kimlik doğrulaması dahil edilmiştir.

## <a name="license-requirements"></a>Lisans gereksinimleri

Genel Engellenenler parola listesini yararları Azure Active Directory (Azure AD) tüm kullanıcılara uygulanır.

Özel Engellenenler parola liste Azure AD temel lisansı gerektirir.

Windows Server Active Directory için Azure AD parola koruması Azure AD Premium lisansı gerektirir. 

Maliyetleri de dahil olmak üzere ek lisans bilgilerini, üzerinde bulunabilir [Azure Active Directory sitesi fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Bir kullanıcı bir şeye yasaklanan parola sıfırlama girişiminde bulunduğunda, aşağıdaki hata iletisini görürler:

Ne yazık ki parolanızı bir sözcük, tümcecik veya parolanızı kolayca guessable yapar düzeni içerir. Lütfen farklı bir parola ile yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Engellenenler parola listesi yapılandırın](howto-password-ban-bad.md)
* [Etkinleştirme Azure AD parola koruma aracıları şirket içi](howto-password-ban-bad-on-premises.md)
