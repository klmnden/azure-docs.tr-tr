---
title: "Kullanıma Sunma: Azure AD SSPR | Microsoft Docs"
description: "Azure AD self servis parola sıfırlama özelliğinin başarıyla kullanıma sunulması için ipuçları"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 5f3900aef2b432527454da72f3ff15e533543758
ms.contentlocale: tr-tr
ms.lasthandoff: 06/07/2017

---
# Kullanıcılar için parola sıfırlamayı kullanıma sunma
<a id="roll-out-password-reset-for-users" class="xliff"></a>

Çoğu müşteri, SSPR işlevini sorunsuzca kullanıma sunmak için aşağıdaki adımları izlemelidir.

1. [Dizininizde parola sıfırlamayı etkinleştirin](active-directory-passwords-getting-started.md)
2. [Parola geri yazma için şirket içi AD izinlerini yapılandırın](active-directory-passwords-how-it-works.md#active-directory-permissions)
3. Azure AD'deki izinleri şirket içi dizininize geri yazmak için [parola geri yazmayı yapılandırın](active-directory-passwords-writeback.md#configuring-password-writeback)
4. [Gerekli lisansları atayıp doğrulayın](active-directory-passwords-licensing.md)
5. Kademeli olarak kullanıma sunmak istiyorsanız, parola sıfırlama özelliğini bir kullanıcı grubuyla sınırlamayı seçerek özelliği zaman içinde kullanıma sunabilirsiniz. Bunu yapmak için **Self Servis Parola Sıfırlama Etkinleştirildi** için **Herkes** olan ayarı **Bir grup** olarak değiştirin ve parola sıfırlama özelliğinin etkinleştirileceği güvenlik grubunu seçin. Bu grubun tüm üyelerine lisans atanmış olmalıdır. Bu, [Grup Tabanlı Lisanslama](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing) için ideal bir yoldur.
6. İlkenize göre en küçük [Kimlik Doğrulama Verileri](active-directory-passwords-data.md) kümesini doldurun.
7. Kullanıcılarınıza kaydolma ve sıfırlama yönergeleri göndererek SSPR kullanmayı öğretin.
    > [!NOTE]
    > Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, SSPR özelliğini yönetici olmayan bir kullanıcıyla test edin. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [ayrıntılı makalemize](active-directory-passwords-how-it-works.md) bakın.

8. Herhangi bir noktada kayıt yapılmasını zorunlu hale getirebilir ve kullanıcıların belirli bir süre sonra kimlik doğrulama bilgilerini yeniden onaylamasını isteyebilirsiniz. Kullanıcılarınız için kaydolmayı zorunlu tutmak istemiyorsanız, [son kullanıcı kaydı istemeden parola sıfırlamayı dağıtabilirsiniz](active-directory-passwords-data.md).
9. Zaman geçtikçe, [Azure AD tarafından sağlanan raporları](active-directory-passwords-reporting.md) görüntüleyerek kullanıcılarınızın kayıt işlemlerini ve kullanımını gözden geçirin.

## E-posta tabanlı kullanıma sunma
<a id="email-based-rollout" class="xliff"></a>

Birçok müşteri için kullanıcılarının SSPR kullanmaya başlamasının en kolay yolu, kullanımı kolay yönergeler sağlayan bir e-posta kampanyasıdır. [Kullanıma sunma işleminizde şablon olarak kullanabileceğiniz üç basit e-posta oluşturduk.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* **Çok Yakında** e-posta şablonu, kullanıcılara yapması gerekenleri bildirmek üzere kullanıma sunmadan birkaç hafta veya gün önce kullanılır.
* **Şimdi Kullanılabilir** e-posta şablonu, kullanıcıları gerektiğinde SSPR kullanabilmek için kaydolmaya ve kimlik doğrulama verilerini onaylamaya yönlendirmek üzere kullanıma sunma gününde kullanılır.
* **Kaydolma Anımsatıcısı** e-posta şablonu, kullanıcılara kaydolmayı ve kimlik doğrulama verilerini onaylamayı anımsatmak üzere dağıtımdan sonraki birkaç gün veya hafta sonra gönderilir.

## Kendi parola portalınızı oluşturma
<a id="creating-your-own-password-portal" class="xliff"></a>

Büyük müşterilerimizin birçoğu web sayfası barındırmayı ve https://passwords.contoso.com gibi bir kök DNS girişi oluşturmayı seçer. Bu sayfayı Azure AD parola sıfırlama bağlantıları, parolama sıfırlama kaydı, parola değiştirme portalları ve kuruluşa özel diğer bilgilerle doldurur. Bu sayede, gönderdiğiniz herhangi bir e-posta yazışmasına veya ilanlara kullanıcıların hizmetleri kullanmak gerektiğinde gidebileceği işaretli ve akılda kalıcı bir URL ekleyebilirsiniz.

* Parola sıfırlama portalı - https://passwordreset.microsoftonline.com/
* Parola sıfırlama kayıt portalı - http://aka.ms/ssprsetup
* Parola değiştirme portalı - https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## Zorunlu kayıt kullanma
<a id="using-enforced-registration" class="xliff"></a>

Kullanıcıların parola sıfırlama için kaydolmasını istiyorsanız, kullanıcıları Azure AD kullanarak oturum açtıklarında kaydolmaya zorlayabilirsiniz. Dizininizin **Parola sıfırlama** dikey penceresindeki **Kayıt** sekmesinden **Kullanıcılardan Oturum Açarken Kaydolmalarını İste** seçeneğini belirleyerek bu seçeneği etkinleştirebilirsiniz.

Yöneticiler, **Kullanıcıların kimlik doğrulama bilgilerini yeniden onaylamasını istemek için geçen gün sayısı** değerini 0-730 güne ayarlayarak kullanıcıların yeniden kaydolmasını isteyebilir.

Bu seçeneği etkinleştirdikten sonra, oturum açan kullanıcılar, kimlik doğrulama bilgilerini onaylamaları yönünde bir yönetici talebi olduğunu bildiren iletiyi görür.

## Kimlik doğrulama verilerini doldurma
<a id="populate-authentication-data" class="xliff"></a>

[Kullanıcılarınızın kimlik doğrulama verilerini doldurursanız](active-directory-passwords-data.md), kullanıcıların SSPR kullanabilmek için parola sıfırlamaya kaydolması gerekmez. Kullanıcıların tanımlanmış kimlik doğrulama verileri sizin tanımladığınız parola sıfırlama ilkesini karşılıyorsa, kullanıcılar parolalarını sıfırlayabilir.

## Self servis parola sıfırlamayı devre dışı bırakma
<a id="disabling-self-service-password-reset" class="xliff"></a>

Self servis parola sıfırlama özelliğini devre dışı bırakmak için Azure AD kiracınızı açıp **Parola Sıfırlama**, **Özellikler** menüsüne gidin ve **Self Servis Parola Sıfırlama Etkinleştirildi** altından **Hiç Kimse**’yi seçin

## Sonraki adımlar
<a id="next-steps" class="xliff"></a>

Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [**Hızlı Başlangıç**](active-directory-passwords-getting-started.md) - Azure AD self servis parola yönetimi ile çalışmaya hazırlanın 
* [**Lisanslama**](active-directory-passwords-licensing.md) - Azure AD Lisanslarınızı yapılandırın
* [**Veri**](active-directory-passwords-data.md) - Gerekli olan verileri ve parola yönetimi için nasıl kullanıldığını anlayın
* [**Özelleştirme**](active-directory-passwords-customize.md) - SSPR deneyiminin görünümünü şirketiniz için özelleştirin.
* [**İlke**](active-directory-passwords-policy.md) - Azure AD parola ilkelerini anlayın ve ayarlayın
* [**Parola Geri Yazma**](active-directory-passwords-writeback.md) - Şirket içi dizininizde parola geri yazma özelliğinin nasıl çalıştığını anlayın
* [**Raporlama**](active-directory-passwords-reporting.md) - Kullanıcılarınızın SSPR işlevine erişip erişmediğini, ne zaman ve nerede eriştiğini öğrenin
* [**Teknik Ayrıntı**](active-directory-passwords-how-it-works.md) - Nasıl çalıştığını anlamak için perde arkasına gidin
* [**Sık Sorulan Sorular**](active-directory-passwords-faq.md) - Nasıl? Neden? Ne? Nerede? Kim? Ne zaman? - Her zaman sormak istediğiniz soruların yanıtları
* [**Sorun giderme**](active-directory-passwords-troubleshoot.md) - SSPR ile yaygın olarak karşılaştığımız sorunların çözümü hakkında bilgi alın
