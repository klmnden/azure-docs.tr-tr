---
title: Azure Active Directory kimlik koruması kullanıcı riski ilkesini yapılandırma | Microsoft Docs
description: Azure AD kimlik koruması kullanıcı riski ilkesini yapılandırmayı öğrenin.
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
ms.openlocfilehash: b94e9b7267c956c07f4171f8cce46c6159affd90
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60459782"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Nasıl Yapılır: Kullanıcı riski ilkesini yapılandırma

Kullanıcı riski ile Azure AD kullanıcı hesabının tehlikede olduğunu olasılık algılar. Bir yönetici olarak bir belirli bir kullanıcı risk düzeyi için otomatik olarak yanıt vermek için bir kullanıcı risk koşullu erişim ilkesi yapılandırabilirsiniz.
 
Bu makalede, bir kullanıcı risk İlkesi yapılandırmak için gereken bilgileri ile sunulmaktadır.


## <a name="what-is-a-user-risk-policy"></a>Kullanıcı riski İlkesi nedir?

Azure AD, her oturum, bir kullanıcının analiz eder. Analiz amacı, oturum açma ile birlikte gelen kuşkulu eylemleri algılar sağlamaktır. Azure AD'de sistem algılayabilir şüpheli olarak da bilinen risk olayları eylemlerdir. While bazı risk olayları algılanamıyor gerçek zamanlı olarak, risk olayları daha çok zaman gerektiren de vardır. Örneğin, bir alışılmadık konumlara imkansız seyahat algılamak için sistem bir kullanıcının normal davranış hakkında bilgi edinmek için 14 günlük bir öğrenme dönemi gerekir. Algılanan risk olayları çözümlemek için birkaç seçenek vardır. Örneğin, tek tek risk olayları el ile çözümlemeniz veya bunları bir oturum açma riski veya bir kullanıcı risk koşullu erişim ilkesi kullanılarak sorun Çözüldü alabilirsiniz.

Bir kullanıcı için tespit ettik ve çözülmesi yaramadı tüm risk olayları etkin risk olayları olarak bilinir. Bir kullanıcı ile ilişkilendirilmiş active risk olaylarını kullanıcı riski bilinir. Azure AD kullanıcı riskine bağlı olarak, kullanıcı gizliliğinin bozulduğunu olasılık (düşük, Orta, yüksek) hesaplar. Olasılık kullanıcı risk düzeyi adı verilir.

![Kullanıcı risk](./media/howto-user-risk-policy/1031.png)

Kullanıcı riski İlkesi belirli bir kullanıcı risk düzeyi için yapılandırdığınız otomatik yanıt ' dir. Kullanıcı riski İlkesi ile kaynaklarınıza erişimi engellemek ya da bir kullanıcı hesabı temiz bir duruma geri dönmek için bir parola değişikliği iste.


## <a name="how-do-i-access-the-user-risk-policy"></a>Kullanıcı riski İlkesi nasıl erişim sağlanır?
   
Oturum açma riski İlkesi bulunduğu **yapılandırma** bölümünde [Azure AD kimlik koruması sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Kullanıcı riski ilkesi](./media/howto-user-risk-policy/1014.png)



## <a name="policy-settings"></a>İlke ayarları

Oturum açma riski İlkesi yapılandırdığınızda ayarlamanız gerekir:

- Kullanıcılar ve ilkenin uygulandığı gruplar:

    ![Kullanıcılar ve gruplar](./media/howto-user-risk-policy/11.png)

- İlke tetikler oturum açma riski düzeyi:

    ![Kullanıcı risk düzeyi](./media/howto-user-risk-policy/12.png)

- Oturum açma riski düzeyinizi sağlandığında uygulanmasını istediğiniz erişim türünü:  

    ![Access](./media/howto-user-risk-policy/13.png)

- İlke durumu:

    ![İlke zorlama](./media/howto-user-risk-policy/14.png)

İlke yapılandırma iletişim kutusu yapılandırmanızı etkisini tahmin etmek için bir seçenek sağlar.

![Tahmini etki](./media/howto-user-risk-policy/15.png)

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Risk düzeyine bağlı olarak oturum açma sonrası kullanıcıları engellemek için bir kullanıcı riski ilkesi ayarlayabilirsiniz.

![Engelleme](./media/howto-user-risk-policy/16.png)


Bir oturum açma engelleme:

* Etkilenen kullanıcı için yeni kullanıcı risk olayları oluşturulmasını engeller
* Yöneticilerin el ile kullanıcı kimliğini etkileyen risk olaylarını düzeltmek ve güvenli bir duruma geri sağlar

## <a name="best-practices"></a>En iyi uygulamalar

Seçim bir **yüksek** eşiği ilke tetiklenir ve kullanıcılara etkisini en aza indirir sayısını azaltır.
Ancak, dışlar **düşük** ve **orta** kimlikleri cihazları, güvenli veya değil ilkeyi, risk için işaretlenmiş kullanıcılar daha önce olduğundan şüphelenilen veya tehlikeye bilinen.

İlke ayarlanırken

* Çok sayıda yanlış pozitifleri (geliştiriciler, güvenlik analisti) üreteceği kullanıcılar hariç
* Kullanıcılar yerel ilkesini etkinleştirme olduğu pratik dışında (örneğin Yardım Masası için hiçbir erişim)
* Kullanım bir **yüksek** eşiği ilk ilke üretim, sırasında veya son kullanıcılar tarafından görülen sorunları en aza indirmeniz gerekir.
* Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşiği ek kullanıcı oturum açma sorunları, ancak daha fazla güvenlik sunar.

Önerilen çoğu kuruluş için bir kural yapılandırmak için varsayılandır bir **orta** kullanılabilirlik ve güvenlik arasında bir denge için eşiği.

İlgili kullanıcı deneyimini genel bakış için bkz:

* [Hesap kurtarma akışı tehlikeye](flows.md#compromised-account-recovery).  
* [Engellenen hesap akış tehlikeye](flows.md#compromised-account-blocked).  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD kimlik koruması** dikey penceresindeki **yapılandırma** bölümünde **kullanıcı riski İlkesi**.

    ![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/1009.png "kullanıcı riski İlkesi")




## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
