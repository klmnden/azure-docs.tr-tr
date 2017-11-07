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
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7d97fad04a0aa549d0e3a182282f898302e8e41a
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Azure AD self servis parola sıfırlama hızlı dağıtımı

> [!IMPORTANT]
> **Oturum açmada sorun yaşadığınız için mi buradasınız?** Sorun yaşıyorsanız bkz. [kendi parolanızı değiştirme ve sıfırlama](active-directory-passwords-update-your-own-password.md).

Self servis parola sıfırlama (SSPR), BT uzmanlarının kullanıcılara parolalarını veya hesaplarını sıfırlama ya da bunların kilidini açma yetkisi vermesi için basit bir yol sunar. Sistem, kullanıcıların sistemi kullanması sırasında kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimlerle birlikte izlemeye yönelik ayrıntılı raporlama içerir.

Bu kılavuz, çalışan bir deneme sürümü ya da lisanslı bir Azure AD kiracısına zaten sahip olduğunuzu varsayar. Azure AD’yi ayarlama hakkında yardım gerekiyorsa bkz. [Azure AD ile çalışmaya başlama](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>Azure AD kiracınız için SSPR etkinleştirme

1. Var olan Azure AD kiracınızda **"Parola sıfırlama"** öğesini seçin

2. **"Özellikler"** ekranındaki "Self Servis Parola Sıfırlama Etkinleştirildi" altında aşağıdaki seçimlerden birini yapın:
    * Hiçbiri - Hiç kimse SSPR işlevini kullanamaz.
    * Seçili - Yalnızca seçtiğiniz belirli bir Azure AD grubunun üyeleri SSPR işlevini kullanabilir. Bu dağıtımı yaparken kavram kanıtı için bir kullanıcı grubu tanımlamanız ve bu ayarı kullanmanız önerilir.
    * Tümü - Azure AD kiracınızda hesabı olan tüm kullanıcılar SSPR işlevini kullanabilir. Kavram kanıtını tamamladıktan sonra bu işlevi tüm kiracınıza dağıtmaya hazır olduğunuzda bu ayarı belirlemeniz önerilir.

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
            
    ![Kimlik doğrulaması][Authentication]

4. ÖNERİLEN: **"Özelleştirme"**, "Yöneticinize başvurun" bağlantısını, tanımladığınız bir sayfa ya da e-posta adresine işaret edecek şekilde değiştirmenizi sağlar. Bu bağlantıyı kullanıcılarının destek için kullanmaya alışkın olduğu bir e-posta adresi veya web sitesine ayarlamanız önerilir.

5. İSTEĞE BAĞLI: **"Kayıt"** ekranı yöneticilere aşağıdaki seçenekleri sağlar:
    * Kullanıcılardan oturum açarken kaydolmalarını iste
    * Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı

6. İSTEĞE BAĞLI: **"Bildirim"** ekranı yöneticilere aşağıdaki seçenekleri sağlar:
    * Parola sıfırlamayı kullanıcılara bildir
    * Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildir

**Bu noktada Azure AD kiracınız için SSPR’ı yapılandırdınız**. Kullanıcılarınız bundan böyle yönetici müdahalesi olmadan parolalarını güncelleştirmek için [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md) ve [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md) makalelerinde bulunan yönergeleri kullanabilir. Yalnızca bulut kullanıyorsanız burada durabilir veya şirket içi AD etki alanıyla parola eşitlemeyi yapılandırma işlemine geçebilirsiniz.

> [!IMPORTANT]
> Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, SSPR özelliğini yönetici olmayan bir kullanıcıyla test edin. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi makalemize](active-directory-passwords-policy.md#administrator-password-policy-differences) bakın.

## <a name="configure-synchronization-to-existing-identity-source"></a>Var olan kimlik kaynağına eşitlemeyi yapılandırma

Azure AD ile şirket içi kimlik eşitlemesini etkinleştirmek için [Azure AD Connect](./connect/active-directory-aadconnect.md)’i kuruluşunuzdaki bir sunucuya yükleyip yapılandırmanız gerekir. Bu uygulama, var olan kimlik kaynağınızdan Azure AD kiracınıza kullanıcı ve grupları eşitleme işlemini gerçekleştirir.

* [DirSync veya Azure AD Eşitleme’den Azure AD Connect’e yükseltme](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](./connect/active-directory-aadconnect-get-started-express.md)
* Azure AD'deki izinleri şirket içi dizininize geri yazmak için [parola geri yazmayı yapılandırın](active-directory-passwords-writeback.md#configuring-password-writeback).

### <a name="on-premises-policy-change"></a>Şirket içi ilke değişikliği

Şirket içi Active Directory etki alanındaki kullanıcıları eşitliyorsanız ve kullanıcıların parolalarını hemen sıfırlamasına izin vermek istiyorsanız, şirket içi parola ilkenizde aşağıdaki değişikliği yapın:

**Bilgisayar Yapılandırması** > **İlkeler** > **Windows Ayarları** > **Güvenlik Ayarları** > **Hesap İlkeleri** > **Parola İlkesi**

**En az parola geçerlilik süresi** - 0 gün

Bu güvenlik ayarı, bir parolanın kullanıcı değiştirmeden önce geçerli olması gereken süreyi (gün cinsinden) belirler. Bu ayarın **0 gün** olarak belirlenmesi, kullanıcıların parolaları destek ekibi tarafından değiştirilirse SSPR kullanmasına olanak tanır.

![İlke][Policy]

## <a name="disabling-self-service-password-reset"></a>Self servis parola sıfırlamayı devre dışı bırakma

Self servis parola sıfırlama özelliğini devre dışı bırakmak için, Azure AD kiracınızı açıp **Parola Sıfırlama > Özellikler** > menüsüne gidin ve **Self Servis Parola Sıfırlama Etkinleştirildi** altından **Hiç Kimse**’yi seçin.

### <a name="learn-more"></a>Daha fazla bilgi edinin
Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisans ile ilgili sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta kullanıcılarınız için self servis parola sıfırlamasını nasıl yapılandıracağınızı öğrendiniz. Azure portalına devam ederek bu adımları tamamlamak için aşağıdaki portal bağlantısını izleyin.

> [!div class="nextstepaction"]
> [Self servis parola sıfırlamayı etkinleştirme](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Şirket içi parola grup ilkesi 0 gün olarak ayarlanmış"
