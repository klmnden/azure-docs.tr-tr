---
title: "Kullanıma Sunma: Azure AD SSPR | Microsoft Docs"
description: "Azure AD self servis parola sıfırlama özelliğinin başarıyla kullanıma sunulması için ipuçları"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 0be1616f5df915e566dc73c15dbea2e53177aa1c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="how-to-successfully-rollout-self-service-password-reset"></a>Self servis parola sıfırlamayı başarıyla kullanıma sunma

Çoğu müşteri, SSPR işlevini sorunsuzca kullanıma sunmak için aşağıdaki adımları izler.

1. [Dizininizde parola sıfırlamayı etkinleştirme](active-directory-passwords-getting-started.md).
2. [Parola geri yazma için şirket içi AD izinlerini yapılandırma](active-directory-passwords-writeback.md#active-directory-permissions).
3. Azure AD'deki izinleri şirket içi dizininize geri yazmak için [parola geri yazmayı yapılandırın](active-directory-passwords-writeback.md#configuring-password-writeback).
4. [Gerekli lisansları atayıp doğrulama](active-directory-passwords-licensing.md).
5. SSPR’yi kademeli olarak kullanıma sunmak istiyorsanız, belirli bir grup ile deneyebilmek için erişimi bir kullanıcı grubu ile sınırlayabilirsiniz. Bunu yapmak için **Self Servis Parola Sıfırlama Etkinleştirildi** ayarını **Seçili** olarak değiştirin ve parola sıfırlama özelliğinin etkinleştirileceği güvenlik grubunu seçin. 
6. Kullanıcılarınız için [Kimlik Doğrulama Verileri](active-directory-passwords-data.md) alanını ofis telefonu, cep telefonu ve alternatif e-posta adresi gibi bir değerle doldurun.
7. [Azure AD oturum açma deneyimini şirket markanızı içerecek şekilde özelleştirin. ](active-directory-passwords-customize.md).
8. Kullanıcılarınıza kaydolma ve sıfırlama yönergeleri göndererek SSPR kullanmayı öğretin.
9. Herhangi bir noktada kayıt yapılmasını zorunlu hale getirebilir ve kullanıcıların belirli bir süre sonra kimlik doğrulama bilgilerini yeniden onaylamasını isteyebilirsiniz.
10. Zaman geçtikçe, [Azure AD tarafından sağlanan raporları](active-directory-passwords-reporting.md) görüntüleyerek kullanıcılarınızın kayıt işlemlerini ve kullanımını gözden geçirin.
11. Hazır olduğunuzda, tüm kullanıcılar için parola sıfırlamayı etkinleştirin ve **Self Servis Parola Sıfırlama Etkin** ayarını **Tümü** olarak değiştirin. 

   > [!IMPORTANT]
   > Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, SSPR özelliğini yönetici olmayan bir kullanıcıyla test edin. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi makalemize](active-directory-passwords-policy.md#administrator-password-policy-differences) bakın.

## <a name="email-based-rollout"></a>E-posta tabanlı kullanıma sunma

Birçok müşteri için kullanıcılarının SSPR kullanmaya başlamasının en kolay yolu, kullanımı kolay yönergeler sağlayan bir e-posta kampanyasıdır. [Kullanıma sunma işleminizde şablon olarak kullanabileceğiniz üç basit e-posta oluşturduk.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* **Çok Yakında** e-posta şablonu, kullanıcılara yapması gerekenleri bildirmek üzere kullanıma sunmadan birkaç hafta veya gün önce kullanılır.
* **Şimdi Kullanılabilir** e-posta şablonu, kullanıcıları gerektiğinde SSPR kullanabilmek için kaydolmaya ve kimlik doğrulama verilerini onaylamaya yönlendirmek üzere kullanıma sunma gününde kullanılır.
* **Kaydolma Anımsatıcısı** e-posta şablonu, kullanıcılara kaydolmayı ve kimlik doğrulama verilerini onaylamayı anımsatmak üzere dağıtımdan sonraki birkaç gün veya hafta sonra gönderilir.

![E-posta][Email]

## <a name="creating-your-own-password-portal"></a>Kendi parola portalınızı oluşturma

Büyük müşterilerimizin birçoğu web sayfası barındırmayı ve https://passwords.contoso.com gibi bir kök DNS girişi oluşturmayı seçer. Bu sayfayı Azure AD parola sıfırlama bağlantıları, parolama sıfırlama kaydı, parola değiştirme portalları ve kuruluşa özel diğer bilgilerle doldurur. Bu sayede, gönderdiğiniz herhangi bir e-posta yazışmasına veya ilanlara kullanıcıların hizmetleri kullanmak gerektiğinde gidebileceği işaretli ve akılda kalıcı bir URL ekleyebilirsiniz.

* Parola sıfırlama portalı - https://aka.ms/sspr
* Parola sıfırlama kayıt portalı - http://aka.ms/ssprsetup
* Parola değiştirme portalı - https://account.activedirectory.windowsazure.com/ChangePassword.aspx

Yararlanmanız için, [GitHub](https://github.com/ajamess/password-reset-page)’dan indirilebilen ve kuruluşunuzun gereksinimlerine uygun olarak kullanıp özelleştirebileceğiniz örnek bir sayfa oluşturduk.

## <a name="using-enforced-registration"></a>Zorunlu kayıt kullanma

Kullanıcıların parola sıfırlama için kaydolmasını istiyorsanız, kullanıcıları Azure AD kullanarak oturum açtıklarında kaydolmaya zorlayabilirsiniz. Dizininizin **Parola sıfırlama** dikey penceresindeki **Kayıt** sekmesinden **Kullanıcılardan Oturum Açarken Kaydolmalarını İste** seçeneğini belirleyerek bu seçeneği etkinleştirebilirsiniz.

Yöneticiler, **Kullanıcıların kimlik doğrulama bilgilerini yeniden onaylamasını istemek için geçen gün sayısı** değerini 0-730 güne ayarlayarak kullanıcıların yeniden kaydolmasını isteyebilir.

Bu seçeneği etkinleştirdikten sonra, oturum açan kullanıcılar, kimlik doğrulama bilgilerini onaylamaları yönünde bir yönetici talebi olduğunu bildiren iletiyi görür.

## <a name="populate-authentication-data"></a>Kimlik doğrulama verilerini doldurma

[Kullanıcılarınızın kimlik doğrulama verilerini doldurursanız](active-directory-passwords-data.md), kullanıcıların SSPR kullanabilmek için parola sıfırlamaya kaydolması gerekmez. Kullanıcıların tanımlanmış kimlik doğrulama verileri sizin tanımladığınız parola sıfırlama ilkesini karşılıyorsa, kullanıcılar parolalarını sıfırlayabilir.

## <a name="disabling-self-service-password-reset"></a>Self servis parola sıfırlamayı devre dışı bırakma

Self servis parola sıfırlama özelliğini devre dışı bırakmak için Azure AD kiracınızı açıp **Parola Sıfırlama**, **Özellikler** menüsüne gidin ve **Self Servis Parola Sıfırlama Etkinleştirildi** altından **Hiç Kimse**’yi seçin

## <a name="next-steps"></a>Sonraki adımlar

* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisans ile ilgili sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Email]: ./media/active-directory-passwords-best-practices/sspr-emailtemplates.png "Bu e-posta şablonlarını kuruluşunuzun gereksinimlerine uyacak şekilde özelleştirin"