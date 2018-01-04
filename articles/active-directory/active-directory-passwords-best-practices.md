---
title: "Kullanıma Sunma: Azure AD SSPR | Microsoft Docs"
description: "Azure AD self servis parola sıfırlama özelliğinin başarıyla kullanıma sunulması için ipuçları"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/16/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 79089f09342f520f7d43115cc606d794db6c1602
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Self servis parola sıfırlamayı başarıyla kullanıma sunma

Azure Active Directory (Azure AD) self servis parola sıfırlama (SSPR) işlevinin sorunsuz bir şekilde kullanıma sunulması için çoğu müşteri aşağıdaki adımları uygular:

1. [Dizininizde parola sıfırlamayı etkinleştirme](active-directory-passwords-getting-started.md).
2. [Parola geri yazma için şirket içi Active Directory izinlerini yapılandırma](active-directory-passwords-writeback.md#active-directory-permissions).
3. Azure AD'deki izinleri şirket içi dizininize geri yazmak için [parola geri yazmayı yapılandırın](active-directory-passwords-writeback.md#configure-password-writeback).
4. [Gerekli lisansları atayıp doğrulama](active-directory-passwords-licensing.md).
5. Aşamalı bir kullanıma sunma işlemi yapmak isteyip istemediğinizi belirleyin. SSPR’yi kademeli olarak kullanıma sunmak istiyorsanız, programı belirli bir grup ile deneyebilmek için erişimi bir kullanıcı grubu ile sınırlayabilirsiniz. Belirli bir grubun kullanımına sunmak için **Self Servis Parola Sıfırlama Etkin** düğmesini **Seçili** olarak ayarlayın ve parola sıfırlamayı kullanabilmek istediğiniz güvenlik grubunu seçin. 
6. Kullanıcılarınızın kaydolması için gereken [kimlik doğrulama verileri](active-directory-passwords-data.md) alanını ofis telefonu, cep telefonu ve alternatif e-posta adresi gibi bir değerle doldurun.
7. [Azure AD oturum açma deneyimini şirket markanızı içerecek şekilde özelleştirin](active-directory-passwords-customize.md).
8. Kullanıcılarınıza SSPR kullanmayı öğretin. Kaydolma ve parolalarını sıfırlama hakkında bilgiler veren yönergeler gönderin.
9. Kaydı ne zaman zorunlu tutacağınızı belirleyin. Kaydı herhangi bir noktada zorunlu tutmayı seçebilirsiniz. Ayrıca, kullanıcıların belirli bir süre sonra kimlik doğrulama bilgilerini yeniden onaylamasını isteyebilirsiniz.
10. Raporlama özelliğini kullanın. Zaman içinde, [Azure AD’nin sağladığı raporlama özelliği](active-directory-passwords-reporting.md) ile kullanıcı kaydını ve kullanımını gözden geçirebilirsiniz.
11. Parola sıfırlamayı etkinleştirin. Hazır olduğunuzda, **Self Servis Parola Sıfırlama Etkin** ayarını **Tümü** olarak değiştirerek tüm kullanıcılar için parola sıfırlamayı etkinleştirin. 

   > [!NOTE]
   > Bu seçeneğin ayarlarını seçili grup iken herkes olarak değiştirmek, kullanıcının test grubunun bir parçası olarak kaydettiği mevcut kimlik doğrulaması verilerini geçersiz kılmaz. Yapılandırılmış ve geçerli kimlik doğrulaması verilerine sahip kullanıcılar işlem yapabilir.

12. [Windows 10 kullanıcılarının oturum açma ekranında parola sıfırlamasını etkinleştirin](active-directory-passwords-login.md).

   > [!IMPORTANT]
   > Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, SSPR özelliğini yönetici yerine bir kullanıcıyla test edin. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi](active-directory-passwords-policy.md#administrator-password-policy-differences) makalemize bakın.

## <a name="email-based-rollout"></a>E-posta tabanlı kullanıma sunma

Birçok müşteri için kullanıcıların SSPR kullanmasının en kolay yolu, kullanımı kolay yönergeler içeren bir e-posta kampanyasıdır. [Kullanıma sunma işleminizde şablon olarak kullanabileceğiniz üç basit e-posta oluşturduk](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16):

* **Çok Yakında**: Kullanıcılara yapması gerekenleri bildirmek üzere kullanıma sunmadan birkaç hafta veya gün önce kullandığınız e-posta şablonu.
* **Şimdi kullanılabilir**: Kullanıcıları kaydolmaya ve kimlik doğrulama verilerini onaylamaya yönlendirmek üzere programı kullanıma sunma gününde kullandığınız e-posta şablonu. Kullanıcılar hemen kaydolursa, gerektiğinde SSPR’yi kullanabilir.
* **Kaydolma anımsatıcısı**: Kullanıcılara kaydolmayı ve kimlik doğrulama verilerini onaylamayı anımsatmak üzere dağıtımdan sonraki birkaç gün veya hafta sonra gönderilen e-posta şablonu.

![E-posta][Email]

## <a name="create-your-own-password-portal"></a>Kendi parola portalınızı oluşturma

Büyük müşterilerimizin birçoğu web sayfası barındırmayı ve https://passwords.contoso.com gibi bir kök DNS girişi oluşturmayı seçer. Müşteriler bu sayfayı aşağıdaki bilgilerin bağlantılarıyla doldurur:

* [Azure AD parola sıfırlama portalı - https://aka.ms/sspr](https://aka.ms/sspr)
* [Azure AD parola sıfırlama kayıt portalı - http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
* [Azure AD parola değiştirme portalı - https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Kuruluşa özgü diğer bilgiler

Bu sayede, gönderdiğiniz herhangi bir e-posta yazışmasına veya ilanlara kullanıcıların hizmetleri kullanmak gerektiğinde gidebileceği işaretli ve akılda kalıcı bir URL ekleyebilirsiniz. Yararlanmanız için, kuruluşunuzun gereksinimlerine uygun olarak kullanıp özelleştirebileceğiniz [örnek bir parola sıfırlama sayfası](https://github.com/ajamess/password-reset-page) oluşturduk.

## <a name="use-enforced-registration"></a>Zorunlu kayıt kullanma

Kullanıcıların parola sıfırlama için kaydolmasını istiyorsanız, Azure AD kullanarak oturum açtıklarında kaydolmayı zorunlu hale getirebilirsiniz. Dizininizin **Parola sıfırlama** bölmesindeki **Kayıt** sekmesinden **Kullanıcılardan Oturum Açarken Kaydolmalarını İste** seçeneğini belirleyerek bu seçeneği etkinleştirebilirsiniz.

Yöneticiler, kullanıcıların belirli bir süre sonra yeniden kaydolmasını isteyebilir. **Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı** seçeneğini 0 ila 730 güne ayarlayabilirler.

Bu seçeneği etkinleştirdikten sonra, oturum açan kullanıcılar, kimlik doğrulama bilgilerini onaylamaları yönünde bir yönetici talebi olduğunu bildiren iletiyi görür.

## <a name="populate-authentication-data"></a>Kimlik doğrulama verilerini doldurma

[Kullanıcılarınız için kimlik doğrulama verilerini doldurmanız](active-directory-passwords-data.md) gerekir. Böylece kullanıcıların SSPR kullanabilmek için parola sıfırlamaya kaydolması gerekmez. Kullanıcıların sağladığı kimlik doğrulama verileri sizin tanımladığınız parola sıfırlama ilkesini karşılıyorsa, kullanıcılar parolalarını sıfırlayabilir.

## <a name="disable-self-service-password-reset"></a>Self servis parola sıfırlamayı devre dışı bırakma

Self servis parola sıfırlama kolayca devre dışı bırakılabilir. Azure AD kiracınızı açın, **Parola Sıfırlama** > **Özellikler**'e gidin ve sonra da **Self Servis Parola Sıfırlama Etkinleştirildi** alanında **Hiçbiri**'ni seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Email]: ./media/active-directory-passwords-best-practices/sspr-emailtemplates.png "Bu e-posta şablonlarını kuruluşunuzun gereksinimlerine uyacak şekilde özelleştirin"
