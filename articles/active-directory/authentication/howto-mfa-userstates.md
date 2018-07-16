---
title: Microsoft Azure multi-Factor Authentication kullanıcı durumları
description: Azure multi-Factor authentication'da kullanıcı durumları hakkında bilgi edinin.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 6945966d4a701ea6e2684b7da766c8b6c9f9a283
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049057"
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Bir kullanıcı veya grup için iki aşamalı doğrulama gerektirme

İki aşamalı doğrulama gerektirme iki yaklaşımdan birini alabilir. İlk seçenek, her bir kullanıcı Azure multi-Factor Authentication (MFA) etkinleştirmektir. Kullanıcıları tek tek etkinleştirildiğinde, iki aşamalı doğrulama her zaman oturum gerçekleştirdikleri (güvenilen IP oturum gibi bazı özel durumlar adresleri veya _hatırlanan cihazlar_ özellik açık). İkinci seçenek belirli koşullar altında iki aşamalı doğrulama gerektiren bir koşullu erişim ilkesi ayarlamaktır.

>[!TIP] 
>İki aşamalı kimlik doğrulaması, ikisini aşağıdaki yöntemlerden birini seçin. Bir kullanıcı Azure multi-Factor Authentication için etkinleştirmek, koşullu erişim ilkeleri geçersiz kılar.

## <a name="which-option-is-right-for-you"></a>Hangi seçeneğin size uygun?

**Azure multi-Factor Authentication, kullanıcı durumlarını değiştirerek etkinleştirme** iki aşamalı doğrulama gerektirme geleneksel bir yaklaşımdır. Bu, hem de bulutta Azure MFA ve Azure MFA sunucusu için çalışır. Etkinleştirdiğiniz tüm kullanıcılar, her oturum açtığınızda iki aşamalı doğrulamayı gerçekleştirin. Kullanıcının kullanıcı etkileyebilecek herhangi bir koşullu erişim ilkeleri geçersiz kılar. 

**Koşullu erişim ilkesi ile Azure multi-Factor Authentication etkinleştirilerek** iki aşamalı doğrulama gerektirme daha esnek bir yaklaşımdır. Yalnızca bulutta Azure MFA için yine de çalışır ve _koşullu erişim_ olduğu bir [özelliği, Azure Active Directory Ücretli](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Bireysel kullanıcılar yanı sıra grupları için geçerli olan koşullu erişim ilkeleri oluşturabilirsiniz. Daha fazla kısıtlama düşük riskli grupları daha yüksek riskli gruplar verilebilir veya iki aşamalı doğrulama yalnızca yüksek riskli bulut uygulamaları için gereklidir ve düşük riskli için olanları atlandı. 

İki seçenek de Azure multi-Factor Authentication için gereksinimleri açtıktan sonra oturum ilk kez kaydetmek için kullanıcılara sor. Her iki seçenek de yapılandırılabilir ile çalışma [Azure multi-Factor Authentication ayarlarını](howto-mfa-mfasettings.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Kullanıcı durumu değiştirerek Azure mfa'yı etkinleştirme

Azure multi-Factor Authentication kullanıcı hesapları şu üç ayrı duruma sahiptir:

| Durum | Açıklama | Etkilenen tarayıcı olmayan uygulamalar | Etkilenen tarayıcı uygulamaları | Etkilenen Modern kimlik doğrulaması |
|:---:|:---:|:---:|:--:|:--:|
| Devre dışı |Azure MFA kayıtlı olmayan yeni bir kullanıcı için varsayılan durumu. |Hayır |Hayır |Hayır |
| Etkin |Kullanıcı Azure MFA kaydedilmiş, ancak kayıtlı değil. Bunlar, bir sonraki oturum açışlarında kaydetme istemi alır. |Hayır.  Kayıt işlemi tamamlanana kadar çalışmaya devam eder. | Evet. Oturumun süresi dolduktan sonra Azure MFA kaydı gereklidir.| Evet. Erişim belirtecinin süresi dolduktan sonra Azure MFA kaydı gereklidir. |
| Uygulandı |Kullanıcı kaydedildikten ve Azure MFA için kayıt işlemi tamamlandı. |Evet.  Uygulamalar, uygulama parolaları istiyorlarsa. |Evet. Azure MFA, oturum açma işleminde gereklidir. | Evet. Azure MFA, oturum açma işleminde gereklidir. |

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

   >[!TIP]
   >*Etkin* kullanıcılar otomatik olarak çalıştırmaz *zorlanan* Azure MFA için açtıklarında kaydolmayı zaman. Kullanıcı durumunun el ile yapın *zorlanan*. 

5. Açılır pencere Seçiminizi onaylayın. 

Kullanıcılar etkinleştirdikten sonra e-posta aracılığıyla yollayın. Söyleyin bunlar bir sonraki oturum açışlarında kaydetmek için istenir. Ayrıca, kuruluşunuz modern kimlik doğrulamayı desteklemeyen tarayıcı olmayan uygulamaları kullanıyorsa, bunlar uygulama parolaları oluşturmanız gerekir. Ayrıca bir bağlantı ekleyebilirsiniz [Azure mfa'yı Son Kullanıcı Kılavuzu](../user-help/multi-factor-authentication-end-user.md) yardımcı olmak için onlarla kullanmaya başlayın.

### <a name="use-powershell"></a>PowerShell kullanma
Kullanarak kullanıcı durumunu değiştirmek için [Azure AD PowerShell](/powershell/azure/overview), değiştirme `$st.State`. Üç olası durum vardır:

* Etkin
* Uygulandı
* Devre dışı  

Kullanıcıların doğrudan taşıma *zorlanan* durumu. Bunu yaparsanız, kullanıcının değil Azure MFA kaydından geçmediği ve tarayıcı tabanlı olmayan uygulamalar çalışmamaya bir [uygulama parolası](howto-mfa-mfasettings.md#app-passwords). 

Toplu etkinleştirme kullanıcıları için gerektiğinde PowerShell kullanarak iyi bir seçenektir. Kullanıcıların bir listesi üzerinden döngüye girer ve bunları sağlayan bir PowerShell Betiği oluşturun:

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

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Koşullu erişim ilkesi ile Azure mfa'yı etkinleştirme

_Koşullu erişim_ Ücretli Azure Active Directory, birçok yapılandırma seçenekleri ile birlikte özelliğidir. Bu adımlar, bir ilke oluşturmak için bir yol açıklamaktadır. Hakkında daha fazla bilgi için okuma [Azure Active Directory'de koşullu erişim](../active-directory-conditional-access-azure-portal.md).

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **koşullu erişim**.
3. Seçin **yeni ilke**.
4. Altında **atamaları**seçin **kullanıcılar ve gruplar**. Kullanım **INCLUDE** ve **hariç** hangi kullanıcıların ve grupların İlkesi yönetir belirtmek için sekmeler.
5. Altında **atamaları**seçin **bulut uygulamaları**. Dahil etmeyi **tüm bulut uygulamaları**.
6. Altında **erişim denetimleri**seçin **Grant**. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.
7. Kapatma **ilkesini etkinleştir** için **üzerinde**ve ardından **Kaydet**.

Koşullu Erişim İlkesi'nde diğer seçenekleri, tam olarak iki aşamalı doğrulamayı gerekli olduğunda belirtme olanağı sağlar. Örneğin, bunun gibi bir ilke yapabilirsiniz: Yükleniciler etki alanına katılmamış cihazlarda güvenilmeyen ağlardan tedarik uygulamamızı erişmeyi denediğinde, iki aşamalı doğrulamayı gerektirir. 

## <a name="next-steps"></a>Sonraki adımlar

- İpuçları alın [koşullu erişim için en iyi yöntemler](../active-directory-conditional-access-best-practices.md).

- Azure multi-Factor Authentication ayarlarını yönetmek [kullanıcılarınız ve onların cihazlarını](howto-mfa-userdevicesettings.md).
