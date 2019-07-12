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
ms.openlocfilehash: c043b2ed1a626e362d7edd1a83429aa14046f8ac
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703069"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Hatalı parola kuruluşunuzdaki ortadan kaldırın

Endüstri liderlerinden aynı parolayı birden fazla yerde karmaşık hale ve "/"Password123 gibi basit yapmamaya kullanmayı söyleyin. Kuruluşların kullanıcıları en iyi uygulama kılavuzunu takip ettiğiniz nasıl garanti edebilir? Nasıl bunlar kullanıcılar zayıf parolalarda Zayıf parolalar veya hatta çeşitlemeleri kullanmadığınız emin olabilirim?

Daha güçlü parolaların ilk adımı, kullanıcılarınıza rehberlik sağlamaktır. Bu konu Microsoft'un geçerli yönergeler aşağıdaki bağlantıda bulunabilir:

[Microsoft parolası Kılavuzu](https://www.microsoft.com/research/publication/password-guidance)

Rehber olması önemli, ancak bile ile zayıf parolalarda seçme çok sayıda kullanıcı yine de sonlandırılacak biliyoruz. Azure AD parola koruması kuruluşunuz tarafından algılanıyor ve bilinen Zayıf parolalar ve bunların türevleri engelleme, hem de isteğe bağlı olarak, kuruluşunuza özgü ek zayıf koşullar engelleme korur.

Geçerli güvenlik çalışmaları hakkında daha fazla bilgi için bkz: [Microsoft Güvenlik zekası raporu](https://www.microsoft.com/security/operations/security-intelligence-report).

## <a name="global-banned-password-list"></a>Genel yasaklı parola listesi

Azure AD kimlik koruması ekibi, Azure AD güvenlik telemetri verileri için yaygın olarak kullanılan zayıf ya da güvenliği aşılmış parolaları veya daha açık belirtmek gerekirse, zayıf, temel olarak Zayıf parolalar için sık kullanılan terimler temel sürekli analiz eder. Bu zayıf koşullarını bulunduğunda, bunlar genel yasaklı parola listesine eklenir. Herhangi bir dış veri kaynağı üzerinde genel yasaklı parola listesi içeriğini bağlı değil. Genel yasaklı parola listesi tamamen Azure AD güvenlik telemetri ve analiz devam eden sonuçlarına bağlıdır.

Her yeni bir parola değiştirildi veya herhangi bir kiracıdaki tüm kullanıcılar için Azure AD'de sıfırlama genel yasaklı parola listesi geçerli sürümü parolanın güçlülüğünü doğrularken giriş anahtar kullanılır. Bu doğrulama, tüm Azure AD müşterileri için daha güçlü parolalar sonuçlanır.

> [!NOTE]
> Siber suçlular, ayrıca kendi saldırılarında benzer stratejiler kullanır. Bu nedenle Microsoft, bu listenin içeriği herkese açık şekilde yayımlamaz.

## <a name="custom-banned-password-list"></a>Özel parola listesine yasaklandı

Bazı kuruluşlar kendi özelleştirmeleri genel yasaklı parola listesi üzerinde hangi Microsoft özel yasaklı parola listesi çağrıları ekleyerek güvenliği daha da artırmak isteyebilirsiniz. Microsoft, bu listeye eklenmesine koşulları öncelikle kuruluşa özgü koşullarınızda gibi odaklanan olduğunu önerir:

- Marka adları
- Ürün adları
- Konum (örneğin, örneğin şirketin merkez)
- Şirkete özgü iç koşulları
- Özel bir şirket anlamı olan kısaltmalar.

Koşulları özel yasaklı parola listesine eklendikten sonra bunlar için genel yasaklı parola listesi parolaları doğrularken eklenir.

> [!NOTE]
> Özel yasaklı parola listesi en fazla 1000 koşulları bulunması sınırlıdır. Bu, son derece büyük listeler parolaların engellemek için tasarlanmamıştır. Tam olarak özel yasaklı parola listesi avantajlarından yararlanmak için önce gözden önerir ve parola değerlendirme algoritması anlama (bkz [de parolaları nasıl değerlendirilir](concept-password-ban-bad.md#how-are-passwords-evaluated)) yeni koşullarını eklemeden önce Özel Engellenenler listesi. Algoritma works verimli bir şekilde algılayın ve çok sayıda Zayıf parolalar ve bunların türevleri engellemek için kuruluşunuz nasıl olanak sağlayacak anlama.

Örneğin: "Contoso" Londra'da temel alır ve "Widget" adlı bir ürün yapar, adlı bir müşterinin göz önünde bulundurun. Böyle bir müşteri için kısıp hem de bu koşulları belirli çeşitleri gibi engelleyecek şekilde denemek için daha az güvenli şekilde olacaktır:

- "Contoso! 1"
- "Contoso@London"
- "ContosoWidget"
- "! Contoso"
- "LondonHQ"
- ... etcetera

Bunun yerine, çok daha verimli ve güvenli yalnızca anahtar temel koşulları engellemek için:

- "Contoso"
- "London"
- "Widget"

Parola doğrulama algoritması sonra otomatik olarak zayıf çeşitleri ve yukarıdaki birleşimiyle engeller.

Özel parola listesi ve şirket içi Active Directory Tümleştirmesi, Azure portalını kullanarak yönetilen sağlama yeteneği yasaklandı.

![Özel yasaklı parola listesi kimlik doğrulama yöntemleri altındaki değiştirme](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="password-spray-attacks-and-third-party-compromised-password-lists"></a>Parola ilaç saldırıları ve üçüncü taraf gizliliği bozulan parola listeler

Bir Azure AD parola koruma avantajı parola ilaç saldırılarına karşı korumaya yardımcı olmak için anahtardır. Çoğu parola ilaç saldırıları, benzer bir davranış algılama, hesap kilitleme veya başka bir yolla aracılığıyla olasılığını önemli ölçüde artırır. bu yana herhangi bir belirli bireysel hesabı birkaç kereden fazla saldırılara karşı çalışmayın. Parola ilaç saldırıların büyük bölümü, bu nedenle kullanan her biri ile bir kurumsal hesap bilinen Zayıf parolalar yalnızca az sayıda gönderme hakkında. Bu teknik saldırganın bir kolayca tehlikeye giren hesap olası algılama eşikleri önleme aynı anda sırada hızlıca arayın sağlar.

Azure AD parola koruması, Azure AD tarafından görülen gerçek güvenlik telemetri verileri temel parola ilaç saldırıları kullanılma olasığılı olan tüm bilinen zayıf parolalarda verimli bir şekilde engelleyecek biçimde tasarlanmıştır.  Microsoft önceki genel olarak bilinen güvenlik ihlallerini tehlikeye girmiş parolaları milyonlarca listeleme üçüncü taraf Web sitelerinin farkındadır. Bu parolalar milyonlarca karşı deneme yanılma karşılaştırma temel için üçüncü taraf parola doğrulaması ürünleri yaygındır. Microsoft, bu tür teknikler parola ilaç saldırganlar tarafından kullanılan tipik stratejiler verilen genel parola gücünü artırmak için en iyi yolu olmadığını hissettirir.

> [!NOTE]
> Genel yasaklı parola listesi değil Microsoft, herhangi bir üçüncü taraf veri çubuğunda vermemektedir gizliliği bozulan parola listeleri gibi kaynakları temel.

Microsoft Genel Engellenenler listesi kıyasla bazı üçüncü taraf toplu listeler küçük olsa da, güvenlik etkileri gerçek parola ilaç saldırıların yanı sıra, olgu üzerinde gerçek güvenlik telemetriden alındığından emin olarak yükseltilmiş, Microsoft parola doğrulama algoritması akıllı benzer öğe eşleştirmesi teknikleri kullanır. Son Sonuç verimli bir şekilde algılanır ve milyonlarca en yaygın Zayıf parolalar, kuruluşunuzda kullanılmasını engellemek emin olur. Kuruluşa özgü koşulları özel yasaklı parola listesine eklemek için istemeyen müşterilere aynı algoritmadan avantaj sağlıyor.

## <a name="on-premises-hybrid-scenarios"></a>Şirket içi karma senaryolar

Yalnızca bulut hesapları korumaya yardımcı olur, ancak çoğu kuruluş şirket içi Windows Server Active Directory de dahil olmak üzere karma senaryoları korur. Windows Server Active Directory aracıları var olan altyapınızla yasaklı parola listelerini genişletmek şirket için Azure AD parola koruması yüklemek mümkündür. Kullanıcıların ve değiştirmek, yöneticilerin ayarlayın veya sıfırlanmış artık şirket içi yalnızca bulut kullanıcı olarak aynı parola ilkesiyle uyumlu gereklidir.

## <a name="how-are-passwords-evaluated"></a>Parolaları nasıl değerlendirilir

Her bir kullanıcı, değiştirir veya kullanıcının parolasını sıfırlar yeni parola gücü ve karmaşıklığı (ikincisi yapılandırılmışsa), hem genel ve özel yasaklı parola listesi karşı doğrulama tarafından denetlenir.

Bir kullanıcının parolasını yasaklı parola içeriyorsa bile genel parola yeterince güçlü yanlışsa parola hala kabul. Yeni yapılandırılan bir parola, kabul reddedilir veya belirlemek için genel güçlü değerlendirmek için aşağıdaki adımları geçer.

### <a name="step-1-normalization"></a>1\. adım: Normalleştirme

Yeni bir parola önce bir normalleştirme sürecinden geçer. Bu teknik, potansiyel olarak zayıf parolalarda çok daha büyük bir kümesine eşlenecek yasaklı parola küçük bir kümesi için sağlar.

Normalleştirme iki bölümden oluşur.  İlk, tüm büyük harfleri küçük harfe dönüştürülür.  İkinci, ortak karakter değişimler örneğin gerçekleştirilir:  

| Özgün harf  | Değiştirilen harf |
| --- | --- |
| '0'  | ' |
| '1'  | 'l' |
| '$'  | 's' |
| '\@'  | 'bir' |

Örnek: "boş" parola yasaklanmış ve bir kullanıcı için parola değiştirmesini dener varsayılır "Bl@nK". Olsa da "Bl@nk" olan özel olarak yasaklanmış bir yasaklı parola olan bu parola için "blank" normalleştirme işlemi dönüştürür.

### <a name="step-2-check-if-password-is-considered-banned"></a>2\. adım: Parola yasaklanmış değerlendirilmiyorsa denetimi

#### <a name="fuzzy-matching-behavior"></a>Benzer eşleştirme davranışı

Benzer öğe eşleştirmesi normalleştirilmiş parolasını ya da genel üzerinde bulunan bir parola içerir veya özel parola listelerini yasaklanmış tanımlamak için kullanılır. Eşleştirme işlemi, bir (1) karşılaştırma üzerinde bir düzenleme uzaklık temel alır.  

Örnek: "abcdef" parola yasaklanmış ve bir kullanıcı parolasını aşağıdakilerden birini değiştirmeye çalışırsa varsayılır:

'abcdeg'    *('f' değiştirildi karakter son 'g 'tuşlarını)* 'abcdefg'   *' (g' sonuna eklenen)* 'abcde'     *(sondaki 'f' sonundan silindi)*

Her yukarıdaki parolaların yasaklı parola "abcdef" özellikle eşleşmiyor. Her örneğin bir düzenleme uzaklık 'abcdef' yasaklı döneminin 1 olduğundan, ancak bunlar tüm "abcdef" için bir eşleşme olarak değerlendirilir.

#### <a name="substring-matching-on-specific-terms"></a>Alt dize eşleştirme (belirli koşullarda)

Eşleşen alt dizenin normalleştirilmiş parola kullanıcının ilk kontrol edin ve son Kiracı yanı sıra adı için kullanılır (Kiracı adı ile eşleşen bir Active Directory etki alanı denetleyicisindeki parolalarını doğrularken yapılmaz olduğunu unutmayın).

Örnek: bir kullanıcı, "P0l123fb" için kullanıcının parolasını sıfırlamasını isteyen Pol sahibiz varsayalım. Normalleştirme sonra bu parola, "pol123fb" hale gelir. Parola kullanıcının ilk adını "Pol" içeriyor eşleşen alt dizeyi bulur. Özellikle ya da yasaklı parola listesi üzerinde "P0l123fb" değildi olsa da, altdizgi eşleştirme "Pol" parola bulundu. Bu parola dolayısıyla reddedilecek.

#### <a name="score-calculation"></a>Puan hesaplanmasında

Sonraki adım, kullanıcının yeni parolasını normalleştirilmiş yasaklanmış parolalar tüm örneklerini belirlemektir. Daha sonra:

1. Bir kullanıcının parolasını bulunan her bir yasaklı parola bir nokta verilir.
2. Geri kalan her benzersiz bir karakteri bir nokta verilir.
3. En az beş (5) noktaları bunu kabul edilmesi için bir parola olmalıdır.

Sonraki iki örnek için Contoso Azure AD parola koruması kullanarak ve "contoso" kendi özel listede olduğunu varsayalım. Ayrıca genel listede "boş" olduğunu varsayalım.

Örnek: kullanıcı parolasını "C0ntos0Blank12 için" değiştirir.

Normalleştirme sonra bu parola, "contosoblank12" olur. Bu parola iki yasaklı parola içeren eşleştirme işlemi bulur: contoso ve boş. Bu parolayı daha sonra bir puan verilmiştir:

[contoso] + [boş] + [1] + [2] = 4 nokta bu parolayı altında beş (5) noktaları gerçekleştiği için reddedilir.

Örnek: bir kullanıcı için parola değişiklikleri "ContoS0Bl@nkf9!".

Normalleştirme sonra bu parola, "contosoblankf9!" olur. Bu parola iki yasaklı parola içeren eşleştirme işlemi bulur: contoso ve boş. Bu parolayı daha sonra bir puan verilmiştir:

[contoso] + [boş] + [f] + [9] + [!] = 5 noktaları bu parola en az beş (5) noktaları olduğu kabul edilir.

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

- [Özel yasaklı parola listesi yapılandırma](howto-password-ban-bad.md)
- [Etkinleştirme Azure AD Parola Koruması Aracısı şirket içi](howto-password-ban-bad-on-premises-deploy.md)
