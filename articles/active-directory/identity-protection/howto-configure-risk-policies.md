---
title: Azure Active Directory kimlik koruması (yenilenmiş) risk ilkelerini yapılandırma | Microsoft Docs
description: Azure Active Directory kimlik koruması (yenilenmiş) risk ilkelerini yapılandırma
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdacdf604ab7a4ded7ddf302a217084630f60b31
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60295791"
---
# <a name="how-to-configure-risk-policies-in-azure-active-directory-identity-protection-refreshed"></a>Nasıl Yapılır: Azure Active Directory kimlik koruması (yenilenmiş) risk ilkelerini yapılandırma


Azure AD, riskli olabilecek kimlikler için göstergeleri risk olaylarını algılar. Risk ilkelerini yapılandırarak, algılama sonuçları için otomatik yanıtlar da tanımlayabilirsiniz:

- Oturum açma riski İlkesi ile bir yanıt olarak bir kullanıcının oturum açma sırasında tespit edilen gerçek zamanlı risk olayları yapılandırabilirsiniz. 
- Kullanıcı riski İlkesi ile bir kullanıcı için zaman içinde algılanmış olan tüm etkin kullanıcı risk yanıt yapılandırabilirsiniz.  

> [!VIDEO https://www.youtube.com/embed/zEsbbik-BTE]


## <a name="what-is-the-sign-in-risk-policy"></a>Oturum açma riski İlkesi nedir?

Azure AD, her oturum, bir kullanıcının analiz eder. Analiz amacı, oturum açma ile birlikte gelen kuşkulu eylemleri algılar sağlamaktır. Örneğin, gerçekleştirilen anonim bir IP adresi kullanarak oturum açın, veya başlatılan bilinmeyen bir konumdan oturum açma? Azure AD'de sistem algılayabilir şüpheli olarak da bilinen risk olayları eylemlerdir. Bir oturum açma sırasında Azure AD, bir değer hesaplar algılanan risk etkinliklere göre. Değer, oturum açmanın meşru bir kullanıcı tarafından gerçekleştirildiğini değil olasılığı (düşük, Orta, yüksek) temsil eder. Olasılık adlı **oturum açma risk düzeyini**.

Oturum açma riski ilkesi için özel oturum açma risk düzeyini yapılandırabilirsiniz otomatik yanıt ' dir. Yanıt olarak, kaynaklarınıza erişimi engellemek ya da erişim elde etmek için çok faktörlü kimlik doğrulaması (MFA) testini geçerek gerektirir.

Bir kullanıcı oturum açma riski İlkesi tarafından tetiklenen bir MFA istemi başarıyla tamamlandığında, kimlik koruması, oturum açma kullanıcıdan geldiğini geri bildirim sağlar. Bu nedenle, MFA istemi tetikleyen oturum açma risk olayı otomatik olarak kapatılacak ve kimlik koruması, kullanıcı riski yükseltilmesini katkıda bulunan öğesinden bu olay engeller. Oturum açma riski ilkesini etkinleştirme noisiness riskli oturum açma işlemleri görünümünde MFA için istem görüntülendiğinde kendi kendini düzeltme izin vererek ve daha sonra otomatik olarak ilişkili riskli oturum açma kapatma azaltabilir.

## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Oturum açma riski İlkesi nasıl erişim sağlanır?
   
Oturum açma riski İlkesi bulunduğu **yapılandırma** bölümünde [Azure AD kimlik koruması sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Oturum açma riski İlkesi](./media/howto-configure-risk-policies/1014.png "oturum açma riski İlkesi")


## <a name="sign-in-risk-policy-settings"></a>Oturum açma riski İlkesi ayarları

Oturum açma riski İlkesi yapılandırdığınızda ayarlamanız gerekir:

- Kullanıcılar ve ilkenin uygulandığı gruplar:

    ![Kullanıcılar ve gruplar](./media/howto-configure-risk-policies/11.png)

- İlke tetikler oturum açma riski düzeyi:

    ![Oturum açma riski düzeyi](./media/howto-configure-risk-policies/12.png)

- Oturum açma riski düzeyinizi sağlandığında uygulanmasını istediğiniz erişim türünü:  

    ![Access](./media/howto-configure-risk-policies/13.png)

- İlke durumu:

    ![İlke zorlama](./media/howto-configure-risk-policies/14.png)


İlke yapılandırma iletişim kutusu yapılandırması etkisini tahmin etmek için bir seçenek sağlar.

![Tahmini etki](./media/howto-configure-risk-policies/15.png)

## <a name="what-you-should-know-about-sign-in-risk-policies"></a>Oturum açma riski ilkeleri hakkında bilmeniz gerekenler

MFA gerektirmek için bir oturum açma riski ilkesi yapılandırabilirsiniz:

![MFA gerektirme](./media/howto-configure-risk-policies/16.png)

Ancak, güvenlik nedeniyle, bu ayar yalnızca MFA için önceden kaydedilmiş olan kullanıcılar için çalışır. Kimlik koruması, kullanıcılar için mfa'yı henüz kaydolmadıysanız MFA gereksinimi kullanıcılarla engeller.

Riskli oturum açma işlemleri için mfa'yı gerekli istiyorsanız, şunları yapmalısınız:

1. Etkilenen kullanıcılar için çok faktörlü kimlik doğrulaması kayıt ilkesi etkinleştirin.

2. Etkilenen kullanıcılar oturum açmak için bir MFA kayıt gerçekleştirmek için riskli olmayan bir oturumda gerektirir.

Bu adımları bir riskli oturum açma için multi-Factor authentication gerekiyor sağlar.

Oturum açma riski İlkesi şöyledir:

- Tüm tarayıcı trafik ve modern kimlik doğrulaması kullanarak oturum açma işlemleri için uygulanır.

- WS-Trust uç noktada ADFS gibi Federasyon IDP devre dışı bırakarak eski güvenlik protokolleri kullanan uygulamalar için geçerli değil.


İlgili kullanıcı deneyimini genel bakış için bkz:

* [Riskli oturum açma kurtarma](flows.md#risky-sign-in-recovery)
* [Riskli engellenen oturum açma](flows.md#risky-sign-in-blocked)  
* [Azure AD kimlik koruması ile oturum açma deneyimleri](flows.md)  









## <a name="what-is-a-user-risk-policy"></a>Kullanıcı riski İlkesi nedir?

Azure AD, her oturum, bir kullanıcının analiz eder. Analiz amacı, oturum açma ile birlikte gelen kuşkulu eylemleri algılar sağlamaktır. Azure AD'de sistem algılayabilir şüpheli olarak da bilinen risk olayları eylemlerdir. While bazı risk olayları algılanamıyor gerçek zamanlı olarak, risk olayları daha çok zaman gerektiren de vardır. Örneğin, bir alışılmadık konumlara imkansız seyahat algılamak için sistem bir kullanıcının normal davranış hakkında bilgi edinmek için 14 günlük bir öğrenme dönemi gerekir. Algılanan risk olayları çözümlemek için birkaç seçenek vardır. Örneğin, tek tek risk olayları el ile çözümlemeniz veya bunları bir oturum açma riski veya bir kullanıcı risk koşullu erişim ilkesi kullanılarak sorun Çözüldü alabilirsiniz.

Bir kullanıcı için tespit ettik ve çözülmesi yaramadı tüm risk olayları etkin risk olayları olarak bilinir. Bir kullanıcı ile ilişkilendirilmiş active risk olaylarını kullanıcı riski bilinir. Azure AD kullanıcı riskine bağlı olarak, kullanıcı gizliliğinin bozulduğunu olasılık (düşük, Orta, yüksek) hesaplar. Olasılık kullanıcı risk düzeyi adı verilir.

![Kullanıcı risk](./media/howto-configure-risk-policies/11031.png)

Kullanıcı riski İlkesi belirli bir kullanıcı risk düzeyi için yapılandırdığınız otomatik yanıt ' dir. Kullanıcı riski İlkesi ile kaynaklarınıza erişimi engellemek ya da bir kullanıcı hesabı temiz bir duruma geri dönmek için bir parola değişikliği iste.


## <a name="how-do-i-access-the-user-risk-policy"></a>Kullanıcı riski İlkesi nasıl erişim sağlanır?
   
Kullanıcı riski İlkesi bulunduğu **yapılandırma** bölümünde [Azure AD kimlik koruması sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Kullanıcı riski İlkesi](./media/howto-configure-risk-policies/11014.png)



## <a name="user-risk-policy-settings"></a>Kullanıcı riski İlkesi ayarları

Kullanıcı riski İlkesi yapılandırdığınızda ayarlamanız gerekir:

- Kullanıcılar ve ilkenin uygulandığı gruplar:

    ![Kullanıcılar ve gruplar](./media/howto-configure-risk-policies/111.png)

- İlke tetikler oturum açma riski düzeyi:

    ![Kullanıcı risk düzeyi](./media/howto-configure-risk-policies/112.png)

- Oturum açma riski düzeyinizi sağlandığında uygulanmasını istediğiniz erişim türünü:  

    ![Access](./media/howto-configure-risk-policies/113.png)

- İlke durumu:

    ![İlke zorlama](./media/howto-configure-risk-policies/114.png)

İlke yapılandırma iletişim kutusu yapılandırmanızı etkisini tahmin etmek için bir seçenek sağlar.

![Tahmini etki](./media/howto-configure-risk-policies/115.png)

## <a name="what-you-should-know-about-user-risk-polices"></a>Kullanıcı riski hakkında bilmeniz gerekenler ilkeleri

Risk düzeyine bağlı olarak oturum açma sonrası kullanıcıları engellemek için bir kullanıcı riski ilkesi ayarlayabilirsiniz.

![Engelleme](./media/howto-configure-risk-policies/116.png)


Bir oturum açma engelleme:

* Etkilenen kullanıcı için yeni kullanıcı risk olayları oluşturulmasını engeller
* Yöneticilerin el ile kullanıcı kimliğini etkileyen risk olaylarını düzeltmek ve güvenli bir duruma geri sağlar


























## <a name="best-practices"></a>En iyi uygulamalar

Seçim bir **yüksek** eşiği ilke tetiklenir ve kullanıcılara etkisini en aza indirir sayısını azaltır.  

Ancak, dışlar **düşük** ve **orta** oturum açma bayrağı risk güvenliği aşılmış kimlik kötüye saldırganın engellemeyebilir ilkesi için.

İlke ayarlanırken

- Olmayan / çok faktörlü kimlik doğrulamasına sahip olmadığınız kullanıcılar hariç

- Kullanıcılar yerel ilkesini etkinleştirme olduğu pratik dışında (örneğin Yardım Masası için hiçbir erişim)

- Çok sayıda yanlış pozitifleri (geliştiriciler, güvenlik analisti) üreteceği kullanıcılar hariç

- Kullanım bir **yüksek** eşiği ilk ilke sunum sırasında veya son kullanıcılar tarafından görülen sorunları en aza indirmeniz gerekir.

- Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşiği ek kullanıcı oturum açma sorunları, ancak daha fazla güvenlik sunar.

Önerilen çoğu kuruluş için bir kural yapılandırmak için varsayılandır bir **orta** kullanılabilirlik ve güvenlik arasında bir denge için eşiği.





## <a name="next-steps"></a>Sonraki adımlar

 [Kanal 9: Azure AD kimlik gösterin: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

