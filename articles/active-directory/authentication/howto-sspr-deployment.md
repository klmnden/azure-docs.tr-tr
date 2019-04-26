---
title: Self servis parola sıfırlama dağıtım kılavuzu - Azure Active Directory
description: Azure AD self servis parola sıfırlama özelliğinin başarıyla kullanıma sunulması için ipuçları
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb9333845a78204ea1c6e56e17b3b6f9e5513c9c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60358008"
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Self servis parola sıfırlamayı başarıyla kullanıma sunma

Azure Active Directory (Azure AD) self servis parola sıfırlama (SSPR) işlevinin sorunsuz bir şekilde kullanıma sunulması için çoğu müşteri aşağıdaki adımları uygular:

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. Kuruluşunuzun küçük bir alt kümesi ile bir pilot dağıtım tamamlayın.
   * Pilot hakkında bilgi bulunabilir [Öğreticisi: Eksiksiz bir Azure AD Self Servis parola sıfırlama pilot dağıtmadan](tutorial-sspr-pilot.md).
1. Yardım Masası eğitin.
   * Kullanıcılarınıza nasıl yardımcı?
   * Kullanıcıların SSPR kullanabilmek ve kullanıcılara yardım etmek masanıza izin vermeyecek zorlayacak?
   * Kayıt için bunları URL'leri sağlanan var ve sıfırlama?
      * Kaydı:  https://aka.ms/ssprsetup
      * Sıfırlama: https://aka.ms/sspr
1. Kullanıcılarınızı bilgilendirin.
   * Aşağıdaki bölümler bu belgenin üzerinden iletişim örnek, kayıt zorlama ve kimlik doğrulama verilerini doldurma parola portalları gidin.
   * Azure Active Directory ürün grubu, kuruluşların bir işletme durumu ve self servis parola sıfırlama dağıtımı için bir plan oluşturmak için bu sitede bulunan bilgilerle paralel olarak kullanabilecekleri bir [adım adım dağıtım planı](https://aka.ms/SSPRDeploymentPlan) oluşturmuştur.
1. Self Servis parola sıfırlama kuruluşunuzun tamamı için etkinleştirin.
   * Hazır olduğunuzda, **Self Servis Parola Sıfırlama Etkin** ayarını **Tümü** olarak değiştirerek tüm kullanıcılar için parola sıfırlamayı etkinleştirin.

## <a name="sample-communication"></a>Örnek iletişimi

Birçok müşteri için kullanıcıların SSPR kullanmasının en kolay yolu, kullanımı kolay yönergeler içeren bir e-posta kampanyasıdır. [Basit e-posta ve uygulamanızı kullanıma sunma işleminizde şablon olarak kullanabileceğiniz diğer malzemeleri oluşturduk](https://www.microsoft.com/download/details.aspx?id=56768):

* **Çok yakında**: Bir şey yapmak için ihtiyaç duydukları bilmesini sağlamak üzere birkaç hafta veya gün önce piyasaya çıkma kullandığınız bir e-posta şablonu.
* **Artık kullanılabilir**: Program günü kullandığınız bir e-posta şablonu sürücü kullanıcılara kaydolmayı ve kimlik doğrulama verilerini onaylamayı başlatın. Kullanıcılar hemen kaydolursa, gerektiğinde SSPR’yi kullanabilir.
* **Kaydolma anımsatıcısı**: Birkaç gün veya hafta kullanıcılara kaydolmayı ve kimlik doğrulama verilerini onaylamayı anımsatmak üzere dağıtımdan sonra bir e-posta şablonu.
* **SSPR posterler**: Posterler özelleştirebilir ve kuruluşunuz gün ve en fazla önde gelen hafta sonra dağıtım etrafında görüntüler.
* **SSPR tablo tents**: Tablo kartları öğle odada Konferans salonu veya kullanıcılarınızın kaydınızı tamamlamak için teşvik etmek için masalarını yerleştirebilirsiniz.
* **SSPR Çıkartmalar**: Etiket şablonları özelleştirme ve dizüstü bilgisayarlar, monitör, klavye veya cep telefonları SSPR'ye erişmek nasıl hatırlamak yerleştirmek için yazdırma.

![SSPR kullanıcılara piyasaya çıkma için e-posta örnekleri][Email]

## <a name="create-your-own-password-portal"></a>Kendi parola portalınızı oluşturma

Büyük müşterilerimizin birçoğu web sayfası barındırmayı ve https://passwords.contoso.com gibi bir kök DNS girişi oluşturmayı seçer. Müşteriler bu sayfayı aşağıdaki bilgilerin bağlantılarıyla doldurur:

* [Azure AD parola sıfırlama portalı - https://aka.ms/sspr](https://aka.ms/sspr)
* [Azure AD parola sıfırlama kayıt portalı - https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Azure AD parola değiştirme portalı - https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Kuruluşa özgü diğer bilgiler

Bu sayede, gönderdiğiniz herhangi bir e-posta yazışmasına veya ilanlara kullanıcıların hizmetleri kullanmak gerektiğinde gidebileceği işaretli ve akılda kalıcı bir URL ekleyebilirsiniz. Yararlanmanız için, kuruluşunuzun gereksinimlerine uygun olarak kullanıp özelleştirebileceğiniz [örnek bir parola sıfırlama sayfası](https://github.com/ajamess/password-reset-page) oluşturduk.

## <a name="use-enforced-registration"></a>Zorunlu kayıt kullanma

Kullanıcıların parola sıfırlama için kaydolmasını istiyorsanız, Azure AD kullanarak oturum açtıklarında kaydolmayı zorunlu hale getirebilirsiniz. Dizininizin **Parola sıfırlama** bölmesindeki **Kayıt** sekmesinden **Kullanıcılardan Oturum Açarken Kaydolmalarını İste** seçeneğini belirleyerek bu seçeneği etkinleştirebilirsiniz.

Yöneticiler, kullanıcıların belirli bir süre sonra yeniden kaydolmasını isteyebilir. **Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı** seçeneğini 0 ila 730 güne ayarlayabilirler.

Bu seçeneği etkinleştirdikten sonra, oturum açan kullanıcılar, kimlik doğrulama bilgilerini onaylamaları yönünde bir yönetici talebi olduğunu bildiren iletiyi görür.

## <a name="populate-authentication-data"></a>Kimlik doğrulama verilerini doldurma

Dikkate almanız gereken [bazı kullanıcılarınız için kimlik doğrulama verilerini önceden doldurmak](howto-sspr-authenticationdata.md). Böylece kullanıcıların SSPR kullanabilmek için parola sıfırlamaya kaydolması gerekmez. Kullanıcıların sağladığı kimlik doğrulama verileri sizin tanımladığınız parola sıfırlama ilkesini karşılıyorsa, kullanıcılar parolalarını sıfırlayabilir.

## <a name="disable-self-service-password-reset"></a>Self servis parola sıfırlamayı devre dışı bırakma

Kuruluşunuz Self Servis parola sıfırlamayı devre dışı bırakma karar verirse basit bir işlemdir. Azure AD kiracınızı açın, **Parola Sıfırlama** > **Özellikler**'e gidin ve sonra da **Self Servis Parola Sıfırlama Etkinleştirildi** alanında **Hiçbiri**'ni seçin. Kullanıcılar yine de gelecekte kullanım için kendi kayıtlı kimlik doğrulama yöntemlerini tutacaktır.

## <a name="next-steps"></a>Sonraki adımlar

* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Azure multi-Factor Authentication ve Azure AD Self Servis parola sıfırlama için yakınsanmış kaydını etkinleştirin](concept-registration-mfa-sspr-converged.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Bu e-posta şablonlarını kuruluşunuzun gereksinimlerine uyacak şekilde özelleştirin"
