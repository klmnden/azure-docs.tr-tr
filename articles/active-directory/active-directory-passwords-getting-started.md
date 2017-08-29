---
title: "Hızlı Başlangıç: Azure AD SSPR | Microsoft Docs"
description: "Azure AD self servis parola sıfırlamayı hızlıca dağıtma"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 07c7f3ad066c735054cb339f6e09aa4d7d23f23a
ms.contentlocale: tr-tr
ms.lasthandoff: 08/11/2017

---
# <a name="quickstart-azure-ad-self-service-password-reset"></a>Hızlı Başlangıç: Azure AD self servis parola sıfırlama

> [!IMPORTANT]
> **Oturum açmada sorun yaşadığınız için mi buradasınız?** Sorun yaşıyorsanız bkz. [kendi parolanızı değiştirme ve sıfırlama](active-directory-passwords-update-your-own-password.md).

## <a name="rapidly-deploy-self-service-password-reset"></a>Self servis parola sıfırlamayı hızlıca dağıtma

Self servis parola sıfırlama (SSPR), BT uzmanlarının kullanıcılara parolalarını veya hesaplarını sıfırlama ya da bunların kilidini açma yetkisi vermesi için basit bir yol sunar. Sistem, kullanıcıların sistemi kullanması sırasında kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimlerle birlikte izlemeye yönelik ayrıntılı raporlama içerir.

Bu kılavuz, çalışan bir deneme sürümü ya da lisanslı bir Azure AD kiracısına zaten sahip olduğunuzu varsayar. Azure AD’yi ayarlama hakkında yardım gerekiyorsa bkz. [Azure AD ile çalışmaya başlama](https://azure.microsoft.com/trial/get-started-active-directory/).

1. Var olan Azure AD kiracınızda **"Parola sıfırlama"** öğesini seçin

2. **"Özellikler"** ekranındaki "Self Servis Parola Sıfırlama Etkinleştirildi" altında aşağıdaki seçimlerden birini yapın
    * Hiç kimse - Hiç kimse SSPR işlevini kullanamaz
    * Bir grup - Yalnızca seçtiğiniz belirli bir Azure AD grubunun üyeleri SSPR işlevini kullanabilir
    * Herkes - Azure AD kiracınızda hesabı olan tüm kullanıcılar SSPR işlevini kullanabilir

3. **"Kimlik doğrulama yöntemleri"** ekranında aşağıdakilerden birini seçin
    * Sıfırlamak için gereken yöntem sayısı - En az bir veya en fazla iki yöntem desteklenir
    * Kullanıcıların kullanılabileceği yöntemler - En az bir tane gerekir, ancak fazladan bir seçeneğin olmasından zarar gelmez
        * **E-posta**, kullanıcının yapılandırılmış kimlik doğrulama e-posta adresine kod içeren bir e-posta gönderir
        * **Cep Telefonu**, kullanıcıya yapılandırılmış cep telefonu numarasına kod içeren bir çağrı veya kısa mesaj alma seçeneği sunar
        * **İş Telefonu**, kullanıcıya yapılandırılmış iş telefonuna kod içeren bir çağrı alma seçeneği sunar
        * **Güvenlik Soruları** arasından seçim yapmanız gerekir
            * Kaydolmak için gerekli soru sayısı - Başarılı kayıtlar için alt sınırdır ve kullanıcının seçim yapabileceği bir soru havuzu oluşturmak üzere daha fazla yanıt vermeyi seçebileceği anlamına gelir. Bu seçenek, 3-5 aralığında ayarlanabilir ve sıfırlamak için gereken soru sayısına eşit veya daha büyük olmalıdır.
                * Güvenlik soruları eklenirken "Özel" düğmesine tıklanarak özel sorular eklenebilir
            * Sıfırlamak için gereken soru sayısı - Kullanıcı parolasının sıfırlanması veya kilidinin açılması için doğru cevaplanması gereken 3-5 soruya ayarlanabilir.

4. ÖNERİLEN: **"Özelleştirme"**, "Yöneticinize başvurun" bağlantısını, tanımladığınız bir sayfa ya da e-posta adresine işaret edecek şekilde değiştirmenizi sağlar

5. İSTEĞE BAĞLI: **"Kayıt"** ekranı yöneticilere aşağıdaki seçenekleri sağlar:
    * Kullanıcılardan oturum açarken kaydolmalarını iste
    * Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı

6. İSTEĞE BAĞLI: **"Bildirim"** ekranı yöneticilere aşağıdaki seçenekleri sağlar:
    * Parola sıfırlamayı kullanıcılara bildir
    * Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildir

**Bu noktada Azure AD kiracınız için SSPR’ı yapılandırdınız**. Burada durabilir veya şirket içi AD etki alanıyla parola eşitlemeyi yapılandırma işlemine geçebilirsiniz.

> [!NOTE]
> Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, SSPR özelliğini yönetici olmayan bir kullanıcıyla test edin. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi makalemize](active-directory-passwords-policy.md#administrator-password-policy-differences) bakın.

## <a name="configure-synchronization-to-existing-identity-source"></a>Var olan kimlik kaynağına eşitlemeyi yapılandırma

Azure AD ile şirket içi kimlik eşitlemesini etkinleştirmek için [Azure AD Connect](./connect/active-directory-aadconnect.md)’i kuruluşunuzdaki bir sunucuya yükleyip yapılandırmanız gerekir. Bu uygulama, var olan kimlik kaynağınızdan Azure AD kiracınıza kullanıcı ve grupları eşitleme işlemini gerçekleştirir.

* [DirSync veya Azure AD Eşitleme’den Azure AD Connect’e yükseltme](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](./connect/active-directory-aadconnect-get-started-express.md)
* Azure AD'deki izinleri şirket içi dizininize geri yazmak için [parola geri yazmayı yapılandırın](active-directory-passwords-writeback.md#configuring-password-writeback).

## <a name="disabling-self-service-password-reset"></a>Self servis parola sıfırlamayı devre dışı bırakma

Self servis parola sıfırlama özelliğini devre dışı bırakmak için, Azure AD kiracınızı açıp **Parola Sıfırlama > Özellikler** > menüsüne gidin ve **Self Servis Parola Sıfırlama Etkinleştirildi** altından **Hiç Kimse**’yi seçin.

### <a name="learn-more"></a>Daha fazla bilgi edinin
Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [**Lisanslama**](active-directory-passwords-licensing.md) - Azure AD Lisanslarınızı yapılandırın
* [**Veri**](active-directory-passwords-data.md) - Gerekli olan verileri ve parola yönetimi için nasıl kullanıldığını anlayın
* [**Kullanıma Sunma** ](active-directory-passwords-best-practices.md) - Buradaki yönergelerle SSPR’ı planlayın ve kullanıcılarınıza dağıtın
* [**Özelleştirme**](active-directory-passwords-customize.md) - SSPR deneyiminin görünümünü şirketiniz için özelleştirin.
* [**İlke**](active-directory-passwords-policy.md) - Azure AD parola ilkelerini anlayın ve ayarlayın
* [**Raporlama**](active-directory-passwords-reporting.md) - Kullanıcılarınızın SSPR işlevine erişip erişmediğini, ne zaman ve nerede eriştiğini öğrenin
* [**Teknik Ayrıntı**](active-directory-passwords-how-it-works.md) - Nasıl çalıştığını anlamak için perde arkasına gidin
* [**Sık Sorulan Sorular**](active-directory-passwords-faq.md) - Nasıl? Neden? Ne? Nerede? Kim? Ne zaman? - Her zaman sormak istediğiniz soruların yanıtları
* [**Sorun giderme**](active-directory-passwords-troubleshoot.md) - SSPR ile yaygın olarak karşılaştığımız sorunların çözümü hakkında bilgi alın

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta kullanıcılarınız için self servis parola sıfırlamasını nasıl yapılandıracağınızı öğrendiniz. Azure portalına devam ederek bu adımları tamamlamak için aşağıdaki portal bağlantısını izleyin.

> [!div class="nextstepaction"]
> [Self servis parola sıfırlamayı etkinleştirme](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)


