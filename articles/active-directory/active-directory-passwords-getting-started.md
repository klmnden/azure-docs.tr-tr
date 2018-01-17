---
title: "Self servis parola sıfırlama hızlı başlangıcı - Azure Active Directory"
description: "Azure AD self servis parola sıfırlamayı hızlıca dağıtma"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7b1a8611392b9f7f9648de53a924498631e171f1
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Azure AD self servis parola sıfırlama hızlı dağıtımı

> [!IMPORTANT]
> **Oturum açmada sorun yaşadığınız için mi buradasınız?** Bu durumda, [Yardım edin, Azure AD parolamı unuttum](active-directory-passwords-update-your-own-password.md) konusuna bakın.

Self servis parola sıfırlama (SSPR), BT uzmanlarının kullanıcılara parolalarını veya hesaplarını sıfırlama ya da bunların kilidini açma yetkisi vermesi için basit bir yol sunar. Sistem, kullanıcıların sisteme erişimini izleyen ayrıntılı raporlama içerir, ayrıca kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimler sağlar.

Bu kılavuz, çalışan bir deneme sürümü ya da lisanslı bir Azure Active Directory (Azure AD) kiracısına zaten sahip olduğunuzu varsayar. Azure AD’yi ayarlarken yardım gerekiyorsa bkz. [Azure AD ile çalışmaya başlama](get-started-azure-ad.md).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>Azure AD kiracınız için SSPR etkinleştirme

1. Var olan Azure AD kiracınızda **Parola sıfırlama**'yı seçin.

2. **Özellikler** sayfasında, **Self Servis Parola Sıfırlama Etkinleştirildi** seçeneğinin altında aşağıdakilerden birini seçin:
   * **Hiçbiri**: SSPR işlevini kimse kullanamaz.
   * **Seçili**: SSPR işlevini yalnızca sizin seçtiğiniz belirli bir Azure AD grubunun üyeleri kullanabilir. Bu işlevin dağıtımını yaparken kavram kanıtı için bir kullanıcı grubu tanımlamanız ve bu ayarı kullanmanız önerilir.
   * **Tümü**: SSPR işlevini Azure AD kiracınızda hesabı olan tüm kullanıcılar kullanabilir. Bu ayarı, kavram kanıtını tamamladıktan sonra bu işlevi tüm kiracınıza dağıtmaya hazır olduğunuzda kullanmanızı öneririz.

   > [!IMPORTANT]
   > Azure Yönetici hesapları, ayarlanan seçenekten bağımsız olarak her zaman parolalarını sıfırlama yetkisine sahiptir. 

3. **Kimlik doğrulama yöntemleri** sayfasında aşağıdakileri seçin:
   * **Sıfırlamak için gereken yöntem sayısı**: En az bir veya en fazla iki yöntem desteklenir.
   * **Kullanıcıların kullanılabileceği yöntemler**: En az bir yöntem gerekir, ancak fazladan bir seçeneğin olmasından zarar gelmez.
      * **E-posta**: Kullanıcının yapılandırılmış kimlik doğrulama e-posta adresine kod içeren bir e-posta gönderir.
      * **Cep telefonu**: Kullanıcıya, yapılandırılmış cep telefonu numarasına kod içeren bir çağrı veya kısa mesaj alma seçeneği sunar.
      * **İş telefonu**: Kullanıcıya, yapılandırılmış iş telefonuna kod içeren bir çağrı alma seçeneği sunar.
      * **Güvenlik soruları**: Şunları seçmeniz gerekir:
         * **Kaydolma için gereken soruların sayısı**: Başarılı bir kayıt için gereken alt sınır. Kullanıcı, içinden seçilebilecek bir soru havuzu oluşturmak için birden çok soruya yanıt vermeyi seçebilir. Bu seçenek üç ile beş arasında soruya ayarlanabilir ve kullanıcının parolasını sıfırlamak için gereken soru sayısına eşit veya ondan büyük olmalıdır. Kullanıcı güvenlik sorularını seçerken **Özel** düğmesine tıklamışsa, özel sorular ekleyebilir.
         * **Sıfırlamak için gereken soru sayısı**: Kullanıcının parolasını sıfırlanmasına veya kilidini açmasına izin vermeniz için doğru yanıtlaması gereken üç ile beş arasında soruya ayarlanabilir.
            
    ![Kimlik doğrulaması][Authentication]

4. Önerilen: **Özelleştirme**'nin altında, **Yöneticinize başvurun** bağlantısını, tanımladığınız bir sayfa ya da e-posta adresine işaret edecek şekilde değiştirebilirsiniz. Bu bağlantıyı kullanıcılarının destek soruları için zaten kullandığı bir e-posta adresi veya web sitesine ayarlamanız önerilir.

5. İsteğe bağlı: **Kayıt** sayfası yöneticilere aşağıdaki seçenekleri sağlar:
   * Kullanıcılardan oturum açarken kaydolmalarını isteme.
   * Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısını ayarlama.

6. İsteğe bağlı: **Bildirimler** sayfası yöneticilere aşağıdaki seçenekleri sağlar:
   * Parola sıfırlamayı kullanıcılara bildirme.
   * Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildirme.

Bu noktada Azure AD kiracınız için SSPR’ı yapılandırdınız. Kullanıcılarınız bundan böyle yönetici müdahalesi olmadan parolalarını güncelleştirmek için [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md) ve [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md) makalelerinde bulunan yönergeleri kullanabilir. Yalnızca bulut kullanıyorsanız burada durabilirsiniz. Öte yandan, isterseniz şirket için Active Directory etki alanında parola eşitlemesini yapılandırmak için bir sonraki bölümden devam edebilirsiniz.

> [!TIP]
> Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, SSPR özelliğini yönetici yerine bir kullanıcıyla test edin. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi](active-directory-passwords-policy.md#administrator-password-policy-differences) makalemize bakın.

## <a name="configure-synchronization-to-an-existing-identity-source"></a>Var olan kimlik kaynağına eşitlemeyi yapılandırma

Azure AD ile şirket içi kimlik eşitlemesini etkinleştirmek için [Azure AD Connect](./connect/active-directory-aadconnect.md)’i kuruluşunuzdaki bir sunucuya yükleyip yapılandırmanız gerekir. Bu uygulama, var olan kimlik kaynağınızdan Azure AD kiracınıza kullanıcı ve grupları eşitleme işlemini gerçekleştirir. Daha fazla bilgi için bkz.

* [DirSync veya Azure AD Eşitleme’den Azure AD Connect’e yükseltme](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](./connect/active-directory-aadconnect-get-started-express.md)
* Azure AD'deki izinleri şirket içi dizininize geri yazmak için [parola geri yazmayı yapılandırın](active-directory-passwords-writeback.md#configure-password-writeback)

### <a name="on-premises-policy-change"></a>Şirket içi ilke değişikliği

Şirket içi Active Directory etki alanındaki kullanıcıları eşitliyorsanız ve kullanıcıların parolalarını hemen sıfırlamasına izin vermek istiyorsanız, şirket içi parola ilkenizde aşağıdaki değişikliği yapın:

1. **Bilgisayar Yapılandırması** > **İlkeler** > **Windows Ayarları** > **Güvenlik Ayarları** > **Hesap İlkeleri** > **Parola İlkesi**'ne gidin.

2. **Parola geçerlilik süresi alt sınırı** olarak **0 gün** ayarlayın.

Bu güvenlik ayarı, bir parolanın kullanıcı değiştirmeden önce geçerli olması gereken süreyi gün cinsinden belirler. Kullanım süresi alt sınırı ayarını **0 gün** olarak belirlerseniz, destek ekipleri tarafından parolaları değiştirilen kullanıcılar SSPR'yi kullanabilir.

![İlke][Policy]

## <a name="disable-self-service-password-reset"></a>Self servis parola sıfırlamayı devre dışı bırakma

Self servis parola sıfırlama kolayca devre dışı bırakılabilir. Azure AD kiracınızı açın, **Parola Sıfırlama** > **Özellikler**'e gidin ve sonra da **Self Servis Parola Sıfırlama Etkinleştirildi** alanında **Hiçbiri**'ni seçin.

### <a name="learn-more"></a>Daha fazla bilgi edinin
Aşağıdaki makaleler, Azure AD aracılığıyla parola sıfırlama konusunda ek bilgiler sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta kullanıcılarınız için self servis parola sıfırlamasını nasıl yapılandıracağınızı öğrendiniz. Bu adımları tamamlamak için, Azure Portal'da devam edin:

> [!div class="nextstepaction"]
> [Self servis parola sıfırlamayı etkinleştirme](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Şirket içi parola Grup İlkesi 0 gün olarak ayarlanmış"

