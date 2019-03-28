---
title: Azure Active Directory kimlik Koruması ' oturum açma riski ilkesini yapılandırma | Microsoft Docs
description: Azure AD kimlik koruması oturum açma riski ilkesini yapılandırmayı öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe9e0a4d481ef7b802c50fdc347872e389fa8ef7
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518057"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Nasıl Yapılır: Oturum açma risk ilkesini yapılandırma

Azure Active Directory algılar [risk olayı türleri](../reports-monitoring/concept-risk-events.md#risk-event-types) gerçek zamanlı ve çevrimdışı. Her risk olayı bir oturum açma için kullanıcı algılanan riskli oturum açma adı verilen mantıksal bir kavramken katkıda bulunur. Bir riskli oturum açma kullanıcı hesabının meşru sahibi tarafından gerçekleştirilmiş olabilecek olmayan bir oturum açma girişiminin göstergesidir.


## <a name="what-is-the-sign-in-risk-policy"></a>Oturum açma riski İlkesi nedir?

Azure AD, her oturum, bir kullanıcının analiz eder. Analiz amacı, oturum açma ile birlikte gelen kuşkulu eylemleri algılar sağlamaktır. Örneğin, gerçekleştirilen anonim bir IP adresi kullanarak oturum açın, veya başlatılan bilinmeyen bir konumdan oturum açma? Azure AD'de sistem algılayabilir şüpheli olarak da bilinen risk olayları eylemlerdir. Bir oturum açma sırasında Azure AD, bir değer hesaplar algılanan risk etkinliklere göre. Değer, oturum açmanın meşru bir kullanıcı tarafından gerçekleştirildiğini değil olasılığı (düşük, Orta, yüksek) temsil eder. Olasılık adlı **oturum açma risk düzeyini**.

Oturum açma riski ilkesi için özel oturum açma risk düzeyini yapılandırabilirsiniz otomatik yanıt ' dir. Yanıt olarak, kaynaklarınıza erişimi engellemek ya da erişim elde etmek için çok faktörlü kimlik doğrulaması (MFA) testini geçerek gerektirir.

   
## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Oturum açma riski İlkesi nasıl erişim sağlanır?
   
Oturum açma riski İlkesi bulunduğu **yapılandırma** bölümünde [Azure AD kimlik koruması sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Oturum açma riski İlkesi](./media/howto-sign-in-risk-policy/1014.png "oturum açma riski İlkesi")


## <a name="policy-settings"></a>İlke ayarları

Oturum açma riski İlkesi yapılandırdığınızda ayarlamanız gerekir:

- Kullanıcılar ve ilkenin uygulandığı gruplar:

    ![Kullanıcılar ve gruplar](./media/howto-sign-in-risk-policy/11.png)

- İlke tetikler oturum açma riski düzeyi:

    ![Oturum açma risk düzeyi](./media/howto-sign-in-risk-policy/12.png)

- Oturum açma riski düzeyinizi sağlandığında uygulanmasını istediğiniz erişim türünü:  

    ![Access](./media/howto-sign-in-risk-policy/13.png)

- İlke durumu:

    ![İlke zorlama](./media/howto-sign-in-risk-policy/14.png)


İlke yapılandırma iletişim kutusu yapılandırması etkisini tahmin etmek için bir seçenek sağlar.

![Tahmini etki](./media/howto-sign-in-risk-policy/15.png)

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

MFA gerektirmek için bir oturum açma riski ilkesi yapılandırabilirsiniz:

![MFA gerektirme](./media/howto-sign-in-risk-policy/16.png)

Ancak, güvenlik nedeniyle, bu ayar yalnızca MFA için önceden kaydedilmiş olan kullanıcılar için çalışır. Kimlik koruması, kullanıcılar için mfa'yı henüz kaydolmadıysanız MFA gereksinimi kullanıcılarla engeller.

Riskli oturum açma işlemleri için mfa'yı gerekli istiyorsanız, şunları yapmalısınız:

1. Etkinleştirme [çok faktörlü kimlik doğrulaması kayıt ilkesi](howto-mfa-policy.md) etkilenen kullanıcılar için.

2. Etkilenen kullanıcıların bir MFA kayıt gerçekleştirmek için riskli olmayan bir oturum için oturum açmanız gerekir.

Bu adımları bir riskli oturum açma için multi-Factor authentication gerekiyor sağlar.

Oturum açma riski İlkesi şöyledir:

- Tüm tarayıcı trafik ve modern kimlik doğrulaması kullanarak oturum açma işlemleri için uygulanır.

- WS-Trust uç noktada ADFS gibi Federasyon IDP devre dışı bırakarak eski güvenlik protokolleri kullanan uygulamalar için geçerli değil.


İlgili kullanıcı deneyimini genel bakış için bkz:

* [Riskli oturum açma kurtarma](flows.md#risky-sign-in-recovery)
* [Riskli engellenen oturum açma](flows.md#risky-sign-in-blocked)  
* [Azure AD kimlik koruması ile oturum açma deneyimleri](flows.md)  

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

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
