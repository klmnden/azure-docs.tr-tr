---
title: Azure AD kullanarak önleme deneme yanılma saldırıları akıllı kilitleme - Azure Active Directory
description: Azure Active Directory akıllı kilitleme Kuruluşunuz parolalar tahmin etmeye çalışırken deneme yanılma saldırılarına karşı korumaya yardımcı olur.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 150ecbdfcc21ee7ec0bf54fd5b824bc93e0c76ce
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483315"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory akıllı kilitleme

Akıllı kilitleme, kullanıcılarınızın parolaları tahmin veya almak için deneme yanılma yöntemleri kullanmak için çalışan bir kötü aktörleri kilitleme yardımcı olur. Bu geçerli kullanıcılardan gelen oturum açma tanıyabilir ve bunları farklı olanları saldırganlar ve bilinmeyen diğer kaynakları ele almanız. Akıllı kilitleme saldırganlar, izin vermek, kullanıcılarınızın hesaplarına erişmek ve üretken devam ederken kullanıma kilitler.

Varsayılan olarak, akıllı kilitleme oturum açma denemeleri başarısız olan 10 denemeden sonra bir dakika hesabından kilitler. Sonraki başarısız oturum açma girişimleri, ilk ve sonraki denemeler de daha uzun bir dakika sonra yeniden hesabı kilitler.

Akıllı kilitleme aynı parola kilidi açma sayacı artıyor olmasını önlemek için son üç hatalı parola karmaları izler. Birisi birden çok kez aynı yanlış parola girerse, bu davranış, hesap kilitleme için neden olmaz.

 > [!NOTE]
 > İzleme işlevi karma kimlik doğrulaması, şirket içinde ve bulutta olduğu sürece etkin geçişli kimlik doğrulaması ile müşteriler için kullanılabilir değil.

Akıllı kilitleme her zaman doğru karışımını güvenlik ve kullanılabilirlik sunan bu varsayılan ayarları olan tüm Azure AD müşterileri için açıktır. Akıllı kilitleme ayarları, kuruluşunuza özgü değerlerle özelleştirmesini kullanıcılarınız için Azure AD temel veya daha yüksek lisansı gerektirir.

Akıllı kilitleme kullanarak orijinal bir kullanıcı hiçbir zaman kilitlenir garanti etmez. Bir kullanıcı hesabı akıllı kilitleme kilitler biz kilitleme orijinal kullanıcı için elimizden geleni deneyin. Kötü aktörleri orijinal kullanıcı hesabı için erişim sağlayamadığı emin olmak kilitleme hizmeti çalışır.  

* Her Azure Active Directory veri merkezi kilitleme bağımsız olarak izler. Kullanıcı (threshold_limit * datacenter_count) kullanıcı, her veri merkezi değerse deneme sayısı.
* Akıllı kilitleme tanıdık vs bilinmeyen konum kötü bir aktör ve orijinal kullanıcı arasında ayırt etmek için kullanır. Tanınmayan ve tanıdık konumları ayrı kilitleme sayaçları sahip olur.

Akıllı kilitleme, saldırganlar tarafından kilitli şirket içi Active Directory hesapları korumak için parola karması eşitleme veya doğrudan kimlik doğrulaması'nı kullanarak, karma dağıtımlar ile tümleştirilebilir. Şirket içi Active Directory ulaşmadan önce akıllı kilitleme ilkeleri Azure AD'de uygun şekilde ayarlayarak saldırıları filtrelenebilen.

Kullanırken [geçişli kimlik doğrulaması](../hybrid/how-to-connect-pta.md), emin olmanız gerekir:

* Azure AD kilitleme eşiği **daha az** Active Directory hesap kilitleme eşik değerinden. Böylece Active Directory hesap kilitleme eşiği en az iki veya üç kez Azure AD kilitleme eşik değerinden daha uzun değerleri ayarlayın. 
* Azure AD kilitleme süresi, Active Directory süreden sonra hesap kilitleme sayacını sıfırla daha uzun olarak ayarlanmalıdır. Azure AD süresi saniye olarak ayarlanır, AD süresi dakika cinsinden ayarlanır unutmayın. 

Örneğin, Azure AD sayacı AD yüksek olmasını istiyorsanız, Azure AD AD (60 saniye) 1 dakika olarak ayarlanır, şirket içi sırasında 120 saniye (2 dakika) olacaktır.

> [!IMPORTANT]
> Şu anda bunlar akıllı kilitleme özelliğini tarafından kilitlenmiş, bir yönetici kullanıcıların bulut hesapları kilidi açılamıyor. Yönetici, kilitleme süresi dolmak üzere beklemeniz gerekir.

## <a name="verify-on-premises-account-lockout-policy"></a>Şirket içi hesap kilitleme ilkesi doğrulayın

Şirket içi Active Directory hesap kilitleme ilkesi doğrulamak için aşağıdaki yönergeleri kullanın:

1. Grup İlkesi Yönetimi Aracı'nı açın.
2. Örneğin, kuruluşunuzun hesap kilitleme ilkesi içeren bir Grup İlkesi düzenleme **varsayılan etki alanı ilkesi**.
3. Gözat **Bilgisayar Yapılandırması** > **ilkeleri** > **Windows ayarları** > **güvenlik ayarları**   >  **Hesap ilkeleri** > **hesap kilitleme ilkesi**.
4. Doğrulayın, **hesap kilitleme eşiği** ve **sonra hesap kilitleme sayacını sıfırla** değerleri.

![Şirket içi Active Directory hesap kilitleme ilkesi](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD akıllı kilitleme değerleri yönetme

Kuruluş gereksinimlerinize bağlı olarak, akıllı kilitleme değerleri özelleştirilmiş gerekebilir. Akıllı kilitleme ayarları, kuruluşunuza özgü değerlerle özelleştirmesini kullanıcılarınız için Azure AD temel veya daha yüksek lisansı gerektirir.

Denetleyin veya kuruluşunuz için akıllı kilitleme değerleri değiştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portalında](https://portal.azure.com), tıklayın **Azure Active Directory**, ardından **kimlik doğrulama yöntemleri**.
1. Ayarlama **kilitleme eşiği**bağlı olarak kaç başarısız oturum açma hesabınız, ilk kilitlenmeden önce izin verilir. Varsayılan değer 10'dur.
1. Ayarlama **kilitleme süresini saniye cinsinden**, her kilitleme saniye cinsinden uzunluğu. 60 saniye (bir dakika) varsayılandır.

> [!NOTE]
> İlk kilitleme ayrıca başarısız olduktan sonra oturum, hesabı yeniden kilitler durumunda. Bir hesabın tekrar tekrar kilitler, kilitleme süresi artar.

![Azure portalında Azure AD akıllı kilitleme ilkesi özelleştirme](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)

## <a name="how-to-determine-if-the-smart-lockout-feature-is-working-or-not"></a>Akıllı kilitleme özelliğini veya çalışıp çalışmadığını belirleme

Akıllı kilitleme eşiği tetiklendiğinde, hesap kilitliyken şu iletiyi alırsınız:

**Hesabınız yetkisiz kullanımı önlemek için geçici olarak kilitlendi. Daha sonra yeniden deneyin ve sorun devam ederse yöneticinizle iletişime geçin**

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD kullanarak kuruluşunuzdaki yanlış parolalar yasaklamak öğrenin.](howto-password-ban-bad.md)
* [Self Servis parola sıfırlamayı kullanıcılara kendi hesaplarının kilidini açmak izin verecek şekilde yapılandırın.](quickstart-sspr.md)
