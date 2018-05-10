---
title: Microsoft Azure çok faktörlü kimlik doğrulama kullanıcı durumları
description: Azure multi-Factor Authentication kullanıcı durumları hakkında bilgi edinin.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: b809097e50a17178da12fdb424eba08dc8e0c4cb
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Bir kullanıcı veya grup için iki aşamalı doğrulama zorunlu kılma

İki aşamalı doğrulama gerektirme iki yaklaşım birini alabilir. İlk seçenek, her bir kullanıcı Azure çok faktörlü kimlik doğrulama (MFA) etkinleştirmektir. Kullanıcılar tek tek etkinleştirildiğinde, iki aşamalı doğrulamayı oturum her zaman gerçekleştirdikleri (güvenilen IP ne zaman oturum gibi bazı özel durumlar adresleri veya ne zaman _aygıtları hatırlanan_ özelliği açıktır). İkinci seçenek belirli koşullar altında iki aşamalı doğrulama gerektiren bir koşullu erişim ilkesi ayarlamaktır.

>[!TIP] 
>İki aşamalı doğrulama, ikisini istemek için aşağıdaki yöntemlerden birini seçin. Bir kullanıcı Azure çok faktörlü kimlik doğrulamasını etkinleştirme tüm koşullu erişim ilkeleri geçersiz kılar.

## <a name="which-option-is-right-for-you"></a>Hangi seçeneği sizin için uygun hangisi?

**Kullanıcı durumları değiştirerek Azure çok faktörlü kimlik doğrulamasını etkinleştirme** iki aşamalı doğrulama gerektirme geleneksel bir yaklaşımdır. Hem bulutta Azure MFA ve Azure MFA sunucusu için çalışır. Her oturum açışınızda sağlayan tüm kullanıcıların iki aşamalı doğrulamayı gerçekleştirin. Bir kullanıcının kullanıcı etkileyebilecek herhangi bir koşullu erişim ilkeleri geçersiz kılar. 

**Koşullu erişim ilkesi ile Azure multi-Factor Authentication etkinleştirilerek** iki aşamalı doğrulama gerektirme daha esnek bir yaklaşımdır. Yalnızca bulutta Azure MFA için yine de çalışır ve _koşullu erişim_ olan bir [Özelliği Azure Active Directory Ücretli](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Tek tek kullanıcılar yanı sıra grupları uygulamak koşullu erişim ilkeleri oluşturabilirsiniz. Daha fazla kısıtlama düşük riskli grupları daha yüksek riskli grupları verilebilir veya iki aşamalı doğrulamayı yalnızca yüksek riskli bulut uygulamaları için gereklidir ve düşük riskli için olanları atlandı. 

Her iki seçenek için Azure çok faktörlü kimlik doğrulaması gereksinimleri açtıktan sonra oturum ilk kez kaydetmek için kullanıcılara sor. Her iki seçenek de ile yapılandırılabilir çalışması [Azure çok faktörlü kimlik doğrulama ayarlarını](howto-mfa-mfasettings.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Kullanıcı durumu değiştirerek Azure MFA etkinleştir

Azure multi-Factor Authentication kullanıcı hesapları şu üç ayrı duruma sahiptir:

| Durum | Açıklama | Etkilenen tarayıcı olmayan uygulamalar | Etkilenen tarayıcı uygulamaları | Etkilenen Modern kimlik doğrulaması |
|:---:|:---:|:---:|:--:|:--:|
| Devre dışı |Azure MFA kayıtlı olmayan yeni bir kullanıcı için varsayılan durumu. |Hayır |Hayır |Hayır |
| Etkin |Kullanıcı Azure MFA kayıtlı ancak kayıtlı değil. Kullanıcılar oturum açtığında kaydetmek için bir uyarı alırsınız. |Hayır.  Kayıt işlemi tamamlanana kadar çalışmaya devam eder. | Evet. Oturumun süresi dolduktan sonra Azure MFA kayıt gerekli değil.| Evet. Erişim belirtecinin süresi dolduktan sonra Azure MFA kayıt gerekli değil. |
| Uygulandı |Kullanıcı kaydolmuş ve kaydolma işlemini için Azure MFA tamamlandı. |Evet.  Uygulamaları, uygulama parolaları gerekir. |Evet. Azure MFA oturum açmada gereklidir. | Evet. Azure MFA oturum açmada gereklidir. |

Bir kullanıcının durumunu olup bir yönetim bunları Azure MFA kaydetmiştir ve kayıt işlemini tamamlanmadan yansıtır.

Tüm kullanıcılar Başlat *devre dışı*. Azure MFA kullanıcılar kaydettiğinizde, durumlarına değişikliklerini *etkin*. Etkin kullanıcılar oturum açıp kayıt işlemini tamamlayın, durumlarına değişikliklerini *zorlanmış*.  

### <a name="view-the-status-for-a-user"></a>Kullanıcı durumunu görüntüleyin

Burada görüntüleyebilir ve kullanıcı durumlarını Yönetme sayfasına erişmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
   ![Çok faktörlü kimlik doğrulama yöntemini seçin](./media/howto-mfa-userstates/selectmfa.png)
4. Kullanıcı durumları görüntüler yeni bir sayfa açar.
   ![çok faktörlü kimlik doğrulaması kullanıcı durumu - ekran görüntüsü](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Bir kullanıcının durumunu değiştirme

1. Azure çok faktörlü kimlik doğrulaması almak için yukarıdaki adımları kullanın **kullanıcılar** sayfası.
2. Azure MFA için etkinleştirmek istediğiniz kullanıcıyı bulun. En üstten görünümü değiştirmeniz gerekebilir. 
   ![Kullanıcı - ekran görüntüsü Bul](./media/howto-mfa-userstates/enable1.png)
3. Kendi adının yanındaki kutuyu işaretleyin.
4. Sağ taraftaki altında **hızlı adımlar**, seçin **etkinleştirmek** veya **devre dışı**.
   ![Seçilen kullanıcının - ekran görüntüsü](./media/howto-mfa-userstates/user1.png)

   >[!TIP]
   >*Etkin* kullanıcılar otomatik olarak için anahtarlı *zorlanmış* Azure MFA için bunlar kaydetme zaman. Kullanıcı durumunun el ile değişiklik yapmak *zorlanmış*. 

5. Açılır pencere Seçiminizi onaylayın. 

Kullanıcıların etkinleştirdikten sonra bunları e-posta ile bildirin. Kullanıcılar oturum açtığında kaydetmek için istenir söyleyin. Ayrıca, kuruluşunuz modern kimlik doğrulamayı desteklemeyen tarayıcı olmayan uygulamaları kullanıyorsa, kullanıcılar uygulama parolaları oluşturmanız gerekir. Bir bağlantı da içerebilir [Azure MFA Son Kullanıcı Kılavuzu](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md) başlama yardımcı olmak için.

### <a name="use-powershell"></a>PowerShell kullanma
Kullanarak kullanıcı durumunu değiştirmek için [Azure AD PowerShell](/powershell/azure/overview), değiştirme `$st.State`. Üç olası durum şunlardır:

* Etkin
* Uygulandı
* Devre dışı  

Kullanıcıların doğrudan taşıma *zorlanmış* durumu. Bunu yaparsanız, kullanıcı Azure MFA kaydından gitti değil ve elde edilen çünkü tarayıcı tabanlı olmayan uygulamalar çalışmamaya bir [uygulama parolası](howto-mfa-mfasettings.md#app-passwords). 

Toplu etkinleştirme kullanıcıları gerektiğinde PowerShell kullanarak iyi bir seçenektir. Kullanıcıların bir listesini döngüler ve bunları sağlayan bir PowerShell Betiği oluşturun:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Aşağıdaki komut dosyası örneği verilmiştir:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Azure MFA ile koşullu erişim ilkesini etkinleştir

_Koşullu erişim_ Ücretli Azure Active Directory, birçok yapılandırma seçenekleriyle özelliğidir. Bu adımları bir ilke oluşturmak için bir yol yol. Hakkında daha fazla bilgi için okuma [Azure Active Directory'de koşullu erişim](../active-directory-conditional-access-azure-portal.md).

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **koşullu erişim**.
3. Seçin **yeni ilke**.
4. Altında **atamaları**seçin **kullanıcılar ve gruplar**. Kullanım **INCLUDE** ve **hariç** hangi kullanıcıların ve grupların İlkesi yönetir belirtmek için sekmeler.
5. Altında **atamaları**seçin **bulut uygulamaları**. Dahil etmeyi **tüm bulut uygulamaları**.
6. Altında **erişim denetimleri**seçin **Grant**. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.
7. Kapatma **ilkesini etkinleştir** için **üzerinde**ve ardından **kaydetmek**.

Koşullu Erişim İlkesi'nde diğer seçenekleri tam olarak iki aşamalı doğrulama gerekli olduğunda belirtme olanağı verir. Örneğin, bunun gibi bir ilke yapabilirsiniz: Yükleniciler etki alanına katılmamış cihazlarda güvenilmeyen ağlardan tedarik uygulamamıza erişmeyi denediğinde, iki aşamalı doğrulamayı gerektirir. 

## <a name="next-steps"></a>Sonraki adımlar

- İpuçları almak [koşullu erişim için en iyi uygulamaları](../active-directory-conditional-access-best-practices.md).

- Azure çok faktörlü kimlik doğrulama ayarlarını yönetme [kullanıcılarınızın ve cihazlarının](howto-mfa-userdevicesettings.md).
