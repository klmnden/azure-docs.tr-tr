---
title: "Microsoft Azure çok faktörlü kimlik doğrulama kullanıcı durumları"
description: "Azure MFA kullanıcı durumları hakkında bilgi edinin."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 1869b7a4ef42536a3cd909ba2983ae0fe97185a9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Bir kullanıcı veya grup için iki aşamalı doğrulama zorunlu kılma

İki aşamalı doğrulama gerektirme iki yaklaşım vardır. İlk seçenek, her bağımsız kullanıcı Azure çok faktörlü kimlik doğrulama (MFA) etkinleştirmektir. Kullanıcılar tek tek etkinleştirildiğinde, bunlar her zaman iki aşamalı doğrulamayı (bazı özel durumlarla birlikte, güvenilen IP adreslerinden oturum zaman veya hatırlanan aygıt özelliği açık olup olmadığını gibi) gerçekleştirin. İkinci seçenek belirli koşullar altında iki aşamalı doğrulama gerektiren bir koşullu erişim ilkesi ayarlamaktır.

>[!TIP] 
>İki aşamalı doğrulama, ikisini istemek için aşağıdaki yöntemlerden birini seçin. Bir kullanıcı Azure MFA için etkinleştirmeye herhangi koşullu erişim ilkeleri geçersiz kılar.

## <a name="which-option-is-right-for-you"></a>Hangi sizin için uygun bir seçenektir

**Azure MFA kullanıcı durumları değiştirerek etkinleştirme** iki aşamalı doğrulama gerektirme geleneksel bir yaklaşımdır. Hem bulutta Azure MFA ve Azure MFA sunucusu için çalışır. Oturum her zaman iki aşamalı doğrulamayı gerçekleştirmek için aynı deneyimi sağlayan tüm kullanıcıların sahip. Bir kullanıcının kullanıcı etkileyebilecek herhangi bir koşullu erişim ilkeleri geçersiz kılar. 

**Koşullu erişim ilkesi ile Azure MFA'yı etkinleştirerek** iki aşamalı doğrulama gerektirme daha esnek bir yaklaşımdır. Yalnızca çalışır bulutta Azure MFA için yine de ve koşullu erişim bir [Özelliği Azure Active Directory Ücretli](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Tek tek kullanıcılar yanı sıra grupları uygulamak koşullu erişim ilkeleri oluşturabilirsiniz. Daha fazla kısıtlama düşük riskli grupları daha yüksek riskli grupları verilebilir veya iki aşamalı doğrulamayı yalnızca yüksek riskli bulut uygulamaları için gereklidir ve düşük riskli için olanları atlandı. 

Bu seçeneklerin ikisi de Azure çok faktörlü kimlik doğrulaması için gereksinimleri açtıktan sonra oturum ilk kez kaydetmek için kullanıcılara sor. Her iki seçenek de ile yapılandırılabilir çalışması [Azure çok faktörlü kimlik doğrulama ayarları](multi-factor-authentication-whats-next.md)

## <a name="enable-azure-mfa-by-changing-user-status"></a>Kullanıcı durumu değiştirerek Azure MFA etkinleştir

Azure multi-Factor Authentication kullanıcı hesapları şu üç ayrı duruma sahiptir:

| Durum | Açıklama | Etkilenen tarayıcı olmayan uygulamalar |
|:---:|:---:|:---:|
| Devre dışı |Yeni bir kullanıcı için varsayılan duruma Azure çok faktörlü kimlik doğrulama (MFA) kayıtlı değil. |Hayır |
| Etkin |Kullanıcı Azure MFA kayıtlı ancak kayıtlı değil. Bunlar oturum açtığınızda kaydetmek için istenir. |Hayır.  Kayıt işlemi tamamlanana kadar çalışmaya devam eder. |
| Uygulandı |Kullanıcı kaydolmuş ve kaydolma işlemini için Azure MFA tamamlandı. |Evet.  Uygulamaları, uygulama parolaları gerekir. |

Bir kullanıcının durumunu olup bir yönetim bunları Azure MFA kaydetmiştir ve kayıt işlemini tamamlanmadan yansıtır.

Tüm kullanıcılar Başlat *devre dışı*. Azure MFA, durum değişikliklerini kullanıcılar kaydettiğinizde *etkin*. Etkin kullanıcılar oturum açıp kayıt işlemini tamamlayın, durumlarına değişikliklerini *zorunlu*.  

### <a name="view-the-status-for-a-user"></a>Kullanıcı durumunu görüntüleyin

Burada görüntüleyebilir ve kullanıcı durumlarını Yönetme sayfasına erişmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
   ![Çok faktörlü kimlik doğrulama yöntemini seçin](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Kullanıcı durumları görüntüler, yeni bir sayfa açar.
   ![çok faktörlü kimlik doğrulaması kullanıcı durumu - ekran görüntüsü](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Bir kullanıcının durumunu değiştirme

1. Çok faktörlü kimlik doğrulaması kullanıcılar sayfasına ulaşmak için yukarıdaki adımları kullanın.
2. Azure MFA için etkinleştirmek istediğiniz kullanıcıyı bulun. Üst kısımdaki görünümü değiştirmeniz gerekebilir. 
   ![Kullanıcı - ekran görüntüsü Bul](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Kendi adının yanındaki kutuyu işaretleyin.
4. Sağdaki hızlı adımlar altında seçin **etkinleştirmek** veya **devre dışı**.
   ![Seçilen kullanıcının - ekran görüntüsü](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Etkin* kullanıcılar otomatik olarak geçiş için *zorlanan* Azure MFA için bunlar kaydetme zaman. Kullanıcı durumu zorunlu şeklinde el ile değiştirmemeniz. 

5. Açılır pencere Seçiminizi onaylayın. 

Kullanıcıların etkinleştirdikten sonra e-posta aracılığıyla haber vermelisiniz. Kullanıcılar oturum açtığında kaydetmek için istenir söyleyin. Kuruluşunuz modern kimlik doğrulamayı desteklemeyen tarayıcı olmayan uygulamaları kullanıyorsa, ayrıca, kullanıcılar uygulama parolaları oluşturmanız gerekir. Bir bağlantı da içerebilir bizim [Azure MFA Son Kullanıcı Kılavuzu](./end-user/multi-factor-authentication-end-user.md) başlama yardımcı olmak için.

### <a name="use-powershell"></a>PowerShell kullanma
Kullanıcı durumunu durum kullanarak değiştirmek için [Azure AD PowerShell](/powershell/azure/overview), değiştirme `$st.State`. Üç olası durum şunlardır:

* Etkin
* Uygulandı
* Devre dışı  

Kullanıcıların doğrudan taşıma *zorlanmış* durumu. Kullanıcı MFA kaydı gerçekleştirmediğinden ve [uygulama parolası](multi-factor-authentication-whats-next.md#app-passwords) edinmediğinden, tarayıcı tabanlı olmayan uygulamalar devre dışı kalacaktır. 

Toplu etkinleştirme kullanıcıları gerektiğinde PowerShell kullanarak iyi bir seçenektir. Kullanıcıların bir listesini döngüler ve bunları sağlayan bir PowerShell Betiği oluşturun:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Örnek aşağıda verilmiştir:

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

Koşullu erişim Ücretli Azure Active Directory, birçok olası yapılandırma seçenekleriyle özelliğidir. Bu adımları bir ilke oluşturmak için bir yol yol. Hakkında daha fazla bilgi için okuma [Azure Active Directory'de koşullu erişim](../active-directory/active-directory-conditional-access-azure-portal.md).

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Git **Azure Active Directory** > **koşullu erişim**.
3. Seçin **yeni ilke**.
4. Altında **atamaları**seçin **kullanıcılar ve gruplar**. Kullanım **Ekle** ve **hariç** sekmeleri hangi kullanıcıların ve grupların ilke tarafından yönetilen belirtin.
5. Altında **atamaları**seçin **bulut uygulamaları**. Dahil etmeyi **tüm bulut uygulamaları**.
6. Altında **erişim denetimleri**seçin **Grant**. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.
7. Kapatma **ilkesini etkinleştir** için **üzerinde** ve ardından **kaydetmek**.

Koşullu Erişim İlkesi'nde diğer seçenekleri tam olarak iki aşamalı doğrulamayı gerekip gerekmeyeceğini zaman belirtmenizi sağlar. Örneğin, bildiren bir ilke yapabilir: Yükleniciler etki alanına katılmamış cihazlarda güvenilmeyen ağlardan tedarik uygulamamıza erişmeyi denediğinde, iki aşamalı doğrulamayı gerektirir. 

## <a name="next-steps"></a>Sonraki adımlar

- İpuçları almak [koşullu erişim için en iyi uygulamaları](../active-directory/active-directory-conditional-access-best-practices.md)

- Çok faktörlü kimlik doğrulaması ayarlarını yönetme [, kullanıcılar ve aygıtları](multi-factor-authentication-manage-users-and-devices.md)