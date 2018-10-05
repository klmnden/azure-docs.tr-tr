---
title: Microsoft Azure multi-Factor Authentication kullanıcı durumları
description: Azure multi-Factor authentication'da kullanıcı durumları hakkında bilgi edinin.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 2927521a76e74686592fbc4b3ccb931ece7981fd
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803316"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Bir kullanıcı için iki aşamalı doğrulama gerektirme

İki aşamalı doğrulama gerektirme iki yaklaşımdan birini alabilir. İlk seçenek, her bir kullanıcı Azure multi-Factor Authentication (MFA) etkinleştirmektir. Kullanıcıları tek tek etkinleştirildiğinde, iki aşamalı doğrulama her zaman oturum gerçekleştirdikleri (güvenilen IP oturum gibi bazı özel durumlar adresleri veya _hatırlanan cihazlar_ özellik açık). İkinci seçenek belirli koşullar altında iki aşamalı doğrulama gerektiren bir koşullu erişim ilkesi ayarlamaktır.

> [!TIP]
> İki aşamalı kimlik doğrulaması, ikisini aşağıdaki yöntemlerden birini seçin. Bir kullanıcı Azure multi-Factor Authentication için etkinleştirmek, koşullu erişim ilkeleri geçersiz kılar.

## <a name="choose-how-to-enable"></a>Etkinleştirme seçin

**Kullanıcı durumunu değiştirerek etkin** -Bu, iki aşamalı doğrulama gerektirme geleneksel yöntemidir ve bu makalede ele alınmıştır. Bu, hem de bulutta Azure MFA ve Azure MFA sunucusu ile çalışır. Bu yöntemi kullanarak, kullanıcıların iki aşamalı doğrulamanın gerektirir **her** oturum açın ve koşullu erişim ilkeleri geçersiz kılar.

Koşullu erişim ilkesi tarafından - etkin kullanıcılarınız için iki aşamalı doğrulamayı etkinleştirmek için en esnek yolu budur. Yalnızca koşullu erişim ilkesi kullanarak etkinleştirmek için bulutta Azure MFA çalışır ve Azure AD premium özelliğidir. Bu yöntem hakkında daha fazla bilgi bulunabilir [Azure multi-Factor Authentication'ı bulut tabanlı dağıtım](howto-mfa-getstarted.md).

Bu yöntem Azure AD kimlik koruması tarafından - etkin iki aşamalı kimlik doğrulaması oturum açma riski tüm bulut uygulamaları için yalnızca temel Azure AD kimlik Koruması riski İlkesi kullanır. Bu yöntem, Azure Active Directory P2 lisansı gerektirir. Bu yöntem hakkında daha fazla bilgi bulunabilir [Azure Active Directory kimlik koruması](../identity-protection/howto-sign-in-risk-policy.md)

> [!Note]
> Lisanslar ve fiyatlandırma hakkında daha fazla bilgi bulunabilir [Azure AD'ye](https://azure.microsoft.com/pricing/details/active-directory/
) ve [multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) fiyatlandırma sayfalarına.

## <a name="enable-azure-mfa-by-changing-user-status"></a>Kullanıcı durumu değiştirerek Azure mfa'yı etkinleştirme

Azure multi-Factor Authentication kullanıcı hesapları şu üç ayrı duruma sahiptir:

| Durum | Açıklama | Etkilenen tarayıcı olmayan uygulamalar | Etkilenen tarayıcı uygulamaları | Etkilenen Modern kimlik doğrulaması |
|:---:|:---:|:---:|:--:|:--:|
| Devre dışı |Azure MFA kayıtlı olmayan yeni bir kullanıcı için varsayılan durumu. |Hayır |Hayır |Hayır |
| Etkin |Kullanıcı Azure MFA kaydedilmiş, ancak kayıtlı değil. Bunlar, bir sonraki oturum açışlarında kaydetme istemi alır. |Hayır.  Kayıt işlemi tamamlanana kadar çalışmaya devam eder. | Evet. Oturumun süresi dolduktan sonra Azure MFA kaydı gereklidir.| Evet. Erişim belirtecinin süresi dolduktan sonra Azure MFA kaydı gereklidir. |
| Uygulandı |Kullanıcı kaydedildikten ve Azure MFA için kayıt işlemi tamamlandı. |Evet. Uygulamalar, uygulama parolaları istiyorlarsa. |Evet. Azure MFA, oturum açma işleminde gereklidir. | Evet. Azure MFA, oturum açma işleminde gereklidir. |

Bir kullanıcının durumunu mi Yönetici bunları Azure MFA kaydetmiştir ve kayıt işlemi tamamlanmadan yansıtır.

Tüm kullanıcılar başlar *devre dışı bırakılmış*. Azure MFA kullanıcıları kaydettiğinizde, bunların durumunu değiştirir *etkin*. Etkin kullanıcılar oturum açıp kayıt işlemini tamamlayın, durum değişikliklerini *zorlanan*.  

### <a name="view-the-status-for-a-user"></a>Bir kullanıcının durumunu görüntüleme

Burada görüntüleyebilir ve kullanıcı durumlarını Yönetme sayfasına erişmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
   ![Çok faktörlü kimlik doğrulama yöntemini seçin](./media/howto-mfa-userstates/selectmfa.png)
4. Kullanıcı durumları görüntüleyen yeni bir sayfa açılır.
   ![multi-Factor authentication kullanıcı durumu - ekran görüntüsü](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Bir kullanıcının durumunu değiştirin

1. Azure multi-Factor Authentication'ı almak için önceki adımları kullanın **kullanıcılar** sayfası.
2. Azure MFA için etkinleştirmek istediğiniz kullanıcıyı bulun. Üst kısımdaki görünümü değiştirmeniz gerekebilir.
   ![Kullanıcı - ekran görüntüsü bulun](./media/howto-mfa-userstates/enable1.png)
3. Adının yanındaki kutuyu işaretleyin.
4. Sağ taraftaki altında **hızlı adımları**, seçin **etkinleştirme** veya **devre dışı**.
   ![Seçilen kullanıcının - ekran görüntüsü](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Etkin* kullanıcılar otomatik olarak çalıştırmaz *zorlanan* Azure MFA için açtıklarında kaydolmayı zaman. Kullanıcı durumunun el ile yapın *zorlanan*.

5. Açılır pencere Seçiminizi onaylayın.

Kullanıcılar etkinleştirdikten sonra e-posta aracılığıyla yollayın. Söyleyin bunlar bir sonraki oturum açışlarında kaydetmek için istenir. Ayrıca, kuruluşunuz modern kimlik doğrulamayı desteklemeyen tarayıcı olmayan uygulamaları kullanıyorsa, bunlar uygulama parolaları oluşturmanız gerekir. Ayrıca bir bağlantı ekleyebilirsiniz [Azure mfa'yı Son Kullanıcı Kılavuzu](../user-help/multi-factor-authentication-end-user.md) yardımcı olmak için onlarla kullanmaya başlayın.

### <a name="use-powershell"></a>PowerShell kullanma

Kullanarak kullanıcı durumunu değiştirmek için [Azure AD PowerShell](/powershell/azure/overview), değiştirme `$st.State`. Üç olası durum vardır:

* Etkin
* Uygulandı
* Devre dışı  

Kullanıcıların doğrudan taşıma *zorlanan* durumu. Bunu yaparsanız, kullanıcının değil Azure MFA kaydından geçmediği ve tarayıcı tabanlı olmayan uygulamalar çalışmamaya bir [uygulama parolası](howto-mfa-mfasettings.md#app-passwords).

Modül, ilk olarak kullanarak yükleyin:

       Install-Module MSOnline
       
> [!TIP]
> İlk kez bağlanırken unutmayın **Connect-MsolService**


Toplu etkinleştirme kullanıcıları için gerektiğinde PowerShell kullanarak iyi bir seçenektir. Kullanıcıların bir listesi üzerinden döngüye girer ve bunları sağlayan bir PowerShell Betiği oluşturun:

        Import-Module MSOnline
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Aşağıdaki komut bir örnektir:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }
    
Bu betik devre dışı bırakılmış MFA için kullanılır:

    Get-MsolUser -UserPrincipalName user@domain.com | Set-MsolUser -StrongAuthenticationRequirements @()
    
veya kısaltmak için de olabilir:

    Set-MsolUser -UserPrincipalName user@domain.com -StrongAuthenticationRequirements @()

## <a name="next-steps"></a>Sonraki adımlar

Güvenilen IP'ler, özel sesli mesajları ve sahtekarlık uyarısı gibi diğer ayarları yapılandırmak için bu makaleye bakın [Azure multi-Factor Authentication'ı yapılandırma ayarları](howto-mfa-mfasettings.md)

Azure multi-Factor Authentication makalesinde bulunabilir, kullanıcı ayarlarını yönetme hakkında bilgi [bulutta Azure multi Factor Authentication ile kullanıcı ayarlarını yönetme](howto-mfa-userdevicesettings.md)
