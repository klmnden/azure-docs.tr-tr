---
title: Dinamik olarak yasaklanmış parolalar - Azure Active Directory
description: Azure AD dinamik olarak yasaklanmış parolalar ortamınızdan Zayıf parolalar yasakla
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
ms.openlocfilehash: 50452dc5a0c2074c452878c890643f7b21591689
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65977302"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Hatalı parola kuruluşunuzdaki ortadan kaldırın

Endüstri liderlerinden aynı parolayı birden fazla yerde karmaşık hale ve/Password123 gibi basit yapmamaya kullanmayı söyleyin. Kuruluşların kullanıcıları kılavuzu takip ettiğiniz nasıl garanti edebilir? Nasıl bunlar kullanıcıların yaygın parolaları veya son veri ihlalleriyle dahil edilecek bilinen parolaları kullanmadığı emin olabilirim?

## <a name="global-banned-password-list"></a>Genel yasaklı parola listesi

Microsoft, siber suçluların her zaman bir adım önünde olmak için çalışmaktadır. Bu nedenle Azure AD kimlik koruması ekibi için yaygın olarak kullanılan ve güvenliği aşılan parolaları sürekli olarak arayın. Bunlar ardından genel yasaklı parola listesi çağrılma yeri çok yaygın olarak kabul edilen bu parolaları engelleyin. Siber suçlular kendi saldırılarında de benzer stratejiler kullanır, bu nedenle Microsoft, bu listenin içeriği herkese açık şekilde yayımlamaz. Bu güvenlik açığı olan parolaların Microsoft müşterileri için gerçek bir tehdit haline gelmeden önce engellenir. Geçerli güvenlik çalışmaları hakkında daha fazla bilgi için bkz: [Microsoft Güvenlik zekası raporu](https://www.microsoft.com/security/operations/security-intelligence-report).

## <a name="custom-banned-password-list"></a>Özel yasaklanan parola listesi

Bazı kuruluşlar kendi özelleştirmeleri genel yasaklı parola listesi üzerinde hangi Microsoft özel yasaklı parola listesi çağrıları ekleyerek güvenlik bir adım daha ileri almak isteyebilirsiniz. Kurumsal müşteriler gibi contoso marka adları, şirketinize özgü koşulları veya diğer öğeleri türevleri engellemek seçebilirsiniz.

Özel parola listesi ve şirket içi Active Directory Tümleştirmesi, Azure portalını kullanarak yönetilen sağlama yeteneği yasaklandı.

![Özel yasaklı parola listesi kimlik doğrulama yöntemleri altındaki değiştirme](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Şirket içi karma senaryolar

Yalnızca bulut hesapları korumaya yardımcı olur, ancak çoğu kuruluş şirket içi Windows Server Active Directory de dahil olmak üzere karma senaryoları korur. Windows Server Active Directory aracıları var olan altyapınızla yasaklı parola listelerini genişletmek şirket için Azure AD parola koruması yüklemek mümkündür. Kullanıcıların ve değiştirmek, yöneticilerin ayarlayın veya sıfırlanmış artık şirket içi yalnızca bulut kullanıcı olarak aynı parola ilkesiyle uyumlu gereklidir.

## <a name="how-are-passwords-evaluated"></a>Parolaları nasıl değerlendirilir

Her bir kullanıcı, değiştirir veya kullanıcının parolasını sıfırlar yeni parola gücü ve karmaşıklığı (ikincisi yapılandırılmışsa), hem genel ve özel yasaklı parola listesi karşı doğrulama tarafından denetlenir.

Bir kullanıcının parolasını yasaklı parola içeriyorsa bile genel parola yeterince güçlü yanlışsa parola hala kabul. Yeni yapılandırılan bir parola, kabul reddedilir veya belirlemek için genel güçlü değerlendirmek için aşağıdaki adımları geçer.

### <a name="step-1-normalization"></a>1. Adım: Normalleştirme

Yeni bir parola önce bir normalleştirme sürecinden geçer. Bu yasaklı parola küçük bir kümesi için bir çok daha büyük olabilecek zayıf parolalarda kümesine eşlenecek sağlar.

Normalleştirme iki bölümden oluşur.  İlk, tüm büyük harfleri küçük harfe dönüştürülür.  İkinci, ortak karakter değişimler örneğin gerçekleştirilir:  

| Özgün harf  | Değiştirilen harf |
| --- | --- |
| '0'  | ' |
| '1'  | 'l' |
| '$'  | 's' |
| '\@'  | 'bir' |

Örnek: "boş" parola yasaklanmış ve bir kullanıcı için parola değiştirmesini dener varsayılır "Bl@nK". Olsa da "Bl@nk" olan özel olarak yasaklanmış bir yasaklı parola olan bu parola için "blank" normalleştirme işlemi dönüştürür.

### <a name="step-2-check-if-password-is-considered-banned"></a>2. Adım: Parola yasaklanmış değerlendirilmiyorsa denetimi

#### <a name="fuzzy-matching-behavior"></a>Benzer eşleştirme davranışı

Benzer öğe eşleştirmesi normalleştirilmiş parolasını ya da genel üzerinde bulunan bir parola içerir veya özel parola listelerini yasaklanmış tanımlamak için kullanılır. Eşleştirme işlemi, bir (1) karşılaştırma üzerinde bir düzenleme uzaklık temel alır.  

Örnek: "abcdef" parola yasaklanmış ve bir kullanıcı parolasını aşağıdakilerden birini değiştirmeye çalışırsa varsayılır:

'abcdeg'    *('f' değiştirildi karakter son 'g 'tuşlarını)* 'abcdefg'   *' (g' sonuna eklenen)* 'abcde'     *(sondaki 'f' sonundan silindi)*

Her yukarıdaki parolaların yasaklı parola "abcdef" özellikle eşleşmiyor. Her örneğin bir düzenleme uzaklık yasaklı belirteci 'abcdef' ın 1 olduğundan, ancak bunlar tüm "abcdef" için bir eşleşme olarak değerlendirilir.

#### <a name="substring-matching-on-specific-terms"></a>Alt dize eşleştirme (belirli koşullarda)

Eşleşen alt dizenin normalleştirilmiş parola kullanıcının ilk kontrol edin ve son Kiracı yanı sıra adı için kullanılır (Kiracı adı ile eşleşen bir Active Directory etki alanı denetleyicisindeki parolalarını doğrularken yapılmaz olduğunu unutmayın).

Örnek: bir kullanıcı, "P0l123fb" için kullanıcının parolasını sıfırlamasını isteyen Pol sahibiz varsayalım. Normalleştirme sonra bu parola, "pol123fb" hale gelir. Parola kullanıcının ilk adını "Pol" içeriyor eşleşen alt dizeyi bulur. Özellikle ya da yasaklı parola listesi üzerinde "P0l123fb" değildi olsa da, altdizgi eşleştirme "Pol" parola bulundu. Bu parola dolayısıyla reddedilecek.

#### <a name="score-calculation"></a>Puan hesaplanmasında

Sonraki adım, kullanıcının yeni parolasını normalleştirilmiş yasaklanmış parolalar tüm örneklerini belirlemektir. Daha sonra:

1. Bir kullanıcının parolasını bulunan her bir yasaklı parola bir nokta verilir.
2. Geri kalan her benzersiz bir karakteri bir nokta verilir.
3. En az 5 nokta bunu kabul edilmesi için bir parola olmalıdır.

Sonraki iki örnek için Contoso Azure AD parola koruması kullanarak ve "contoso" kendi özel listede olduğunu varsayalım. Ayrıca genel listede "boş" olduğunu varsayalım.

Örnek: kullanıcı parolasını "C0ntos0Blank12 için" değiştirir.

Normalleştirme sonra bu parola, "contosoblank12" olur. Bu parola iki yasaklı parola içeren eşleştirme işlemi bulur: contoso ve boş. Bu parolayı daha sonra bir puan verilmiştir:

[contoso] + [boş] + [1] + [2] = 4 nokta bu parolayı 5 noktaları altında olduğundan, reddedilir.

Örnek: bir kullanıcı için parola değişiklikleri "ContoS0Bl@nkf9!".

Normalleştirme sonra bu parola, "contosoblankf9!" olur. Bu parola iki yasaklı parola içeren eşleştirme işlemi bulur: contoso ve boş. Bu parolayı daha sonra bir puan verilmiştir:

[contoso] + [boş] + [f] + [9] + [!] = 5 noktaları bu parola en az 5 nokta olduğu kabul edilir.

   > [!IMPORTANT]
   > Genel liste birlikte yasaklı parola algoritması erişebileceği unutmayın ve azure'da devam eden güvenlik analizi ve araştırma göre dilediğiniz zaman değiştirin. DC Aracısı yazılımını yeniden yüklenir sonra şirket içi DC Aracısı hizmeti için güncelleştirilmiş algoritmalar yalnızca geçerli olacaktır.

## <a name="license-requirements"></a>Lisans gereksinimleri

|   | Genel yasaklı parola listesi ile Azure AD parola koruması | Özel yasaklı parola listesi ile Azure AD parola koruması|
| --- | --- | --- |
| Yalnızca bulutta yer alan kullanıcılar | Azure AD Ücretsiz | Azure AD Premium P1 veya P2 |
| Şirket içi Windows Server Active Directory'de eşitlenen kullanıcılar | Azure AD Premium P1 veya P2 | Azure AD Premium P1 veya P2 |

> [!NOTE]
> Azure Active Directory ile eşitlenen olmayan şirket içi Windows Server Active Directory Kullanıcıları, ayrıca mevcut eşitlenmiş kullanıcılar için lisans tabanlı Azure AD parola koruması avantajlarından yararlanabilmek.

Maliyetleri de dahil olmak üzere ek lisans bilgilerini bulunabilir [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Bir kullanıcı yasaklandı şeye parola sıfırlamaya çalıştığında şu hata iletisini görürler:

Ne yazık ki parolanızı bir sözcük, tümcecik veya parolanızı kolayca tahmin edilebilir olmasını sağlayan yapan deseni içerir. Lütfen farklı bir parola ile yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel yasaklı parola listesi yapılandırma](howto-password-ban-bad.md)
* [Etkinleştirme Azure AD Parola Koruması Aracısı şirket içi](howto-password-ban-bad-on-premises-deploy.md)
