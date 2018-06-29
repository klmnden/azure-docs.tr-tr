---
title: Azure AD akıllı kilitleme kullanarak yanılma saldırılarını önleme
description: Azure Active Directory akıllı kilitleme kuruluşunuz parolaları tahmin etmeyi denemelerini yanılma saldırılarına karşı korumaya yardımcı olur.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: d5beb5ce6e167cd100bec2ed54dc6ea0e78ba37b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036259"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory akıllı kilitleme

Akıllı kilitleme bulut Intelligence kullanıcılarınızın parolaları tahmin ya da kaba kuvvet yöntemlerinin alabilmeniz için kullanmak üzere çalışan kötü aktörleri çıkışı kilitlemek için kullanır. Bu Intelligence geçerli kullanıcılardan gelen oturum açma işlemleri tanıyabilir ve farklı olanları saldırganların ve diğer bilinmeyen kaynaklardan ele alın. Akıllı kilitleme, saldırganlar izin vermek, kullanıcılarınızın kendi hesaplarına erişim ve üretken devam ederken çıkış kilitleyebilirsiniz.

Akıllı kilitleme her zaman üzerinde güvenlik ve kullanılabilirlik doğru karışımını sunan varsayılan ayarlarla tüm Azure AD müşteriler içindir. Kuruluşunuza özgü değerlerle akıllı kilitleme ayarları özelleştirmesini kullanıcılarınız için Azure AD Basic veya daha yüksek lisansı gerektirir.

Akıllı kilitleme, karma dağıtımlar, saldırganlar tarafından kilitlenmelerini şirket içi Active Directory hesapları korumak için parola karma eşitlemesi veya doğrudan kimlik doğrulaması ile tümleştirilebilir. Şirket içi Active Directory düşmeden önce akıllı kilitleme ilkeleri Azure AD'de uygun şekilde ayarlayarak, saldırıları filtrelenebilen.

Kullanırken [doğrudan kimlik doğrulama](../connect/active-directory-aadconnect-pass-through-authentication.md), emin olmak gerekir:

   * Azure AD kilitleme eşiği **daha az** Active Directory hesap kilitleme eşiği değerinden. Böylece Active Directory hesap kilitleme eşiği en az iki veya üç kez Azure AD kilitleme eşikten daha uzun değerlerini ayarlayın. 
   * Azure AD kilitleme süresi **saniye cinsinden** olan **uzun** Active Directory süreden sonra Hesap kilidi sayacını sıfırla daha **dakika**.

> [!IMPORTANT]
> Şu anda Akıllı kilitleme yeteneği bunlar kilitlenen, yönetici kullanıcıların bulut hesaplarının kilidini açamazsınız. Yönetici kilitleme süresi dolmak üzere beklemeniz gerekir.

## <a name="verify-on-premises-account-lockout-policy"></a>Şirket içi hesap kilitleme ilkesi doğrulayın

Şirket içi Active Directory hesap kilitleme ilkesi doğrulamak için aşağıdaki yönergeleri kullanın:

1. Grup İlkesi yönetim aracını açın.
2. Örneğin, kuruluşunuzun hesap kilitleme ilkesi içeren Grup İlkesi düzenleme **varsayılan etki alanı ilkesi**.
3. Gözat **Bilgisayar Yapılandırması** > **ilkeleri** > **Windows ayarları** > **güvenlik ayarları**   >  **Hesap ilkeleri** > **hesap kilitleme ilkesi**.
4. Doğrulayın, **hesap kilitleme eşiği** ve **sıfırlama hesap kilitleme sayacını** değerleri.

![Bir Grup İlkesi nesnesi kullanarak şirket içi Active Directory hesap kilitleme ilkesi değiştirme](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD akıllı kilitleme değerleri yönetme

Kuruluş gereksinimlerinize bağlı olarak, akıllı kilitleme değerleri özelleştirilebilecek gerekebilir. Kuruluşunuza özgü değerlerle akıllı kilitleme ayarları özelleştirmesini kullanıcılarınız için Azure AD Basic veya daha yüksek lisansı gerektirir.

Denetleyin veya kuruluşunuz için akıllı kilitleme değerleri değiştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portal](https://portal.azure.com)ve tıklayın **Azure Active Directory**, ardından **kimlik doğrulama yöntemleri**.
1. Ayarlama **kilitleme eşiği**bağlı olarak kaç başarısız oturum açma işlemleri, ilk kilitlemeden önce bir hesap üzerinde izin verilir. Varsayılan değer 10'dur.
1. Ayarlama **kilitleme süresini saniye cinsinden**, her kilitleme saniye cinsinden uzunluğu.

> [!NOTE]
> İlk oturum kilitleme ayrıca başarısız olduktan sonra açma, hesap yeniden kilitler durumunda. Bir hesap art arda kilitlerse, kilitleme süresini artırır.

![Azure portalında Azure AD akıllı kilitleme ilkesi özelleştirme](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Sonraki adımlar

[Azure AD kullanarak kuruluşunuzdaki bozuk parola bYönlendirme öğrenin.](howto-password-ban-bad.md)

[Self Servis parola sıfırlama kendi hesaplarının kilidini açmasına olanak vermek için yapılandırın.](quickstart-sspr.md)