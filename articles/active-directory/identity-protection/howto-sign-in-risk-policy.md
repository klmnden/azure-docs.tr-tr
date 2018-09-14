---
title: Azure Active Directory kimlik Koruması ' oturum açma riski ilkesini yapılandırma | Microsoft Docs
description: Azure AD kimlik koruması oturum açma riski ilkesini yapılandırmayı öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 7350cbd3e8aed6098f24d0edaa5807d241890287
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581478"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Nasıl yapılır: oturum açma riski ilkesi yapılandırma

Azure Active Directory algılar [risk olayı türleri](../reports-monitoring/concept-risk-events.md#risk-event-types) gerçek zamanlı ve çevrimdışı. Her risk olayı bir oturum açma için kullanıcı algılanan riskli oturum açma adı verilen mantıksal bir kavramken katkıda bulunur. Bir riskli oturum açma kullanıcı hesabının meşru sahibi tarafından gerçekleştirilmiş olabilecek olmayan bir oturum açma girişiminin göstergesidir.


## <a name="sign-in-risk-level"></a>Oturum açma riski düzeyi

Bir oturum açma riski düzeyi bir oturum açma girişimi bir kullanıcı hesabının meşru sahibi tarafından gerçekleştirilmedi (yüksek, Orta veya düşük) olasılığını göstergesidir.

## <a name="mitigating-sign-in-risk-events"></a>Oturum açma risk olayları azaltma

Bir risk azaltma, saldırgan güvenliği aşılmış kimlik veya cihaz kimliği veya cihaz güvenli bir duruma geri yüklemeden yararlanma olanağı sınırlamak için bir eylemdir. Bir risk azaltma kimlik veya cihaz ile ilişkili önceki oturum açma risk olayları çözmez.

Riskli oturum açma işlemleri otomatik olarak gidermek için oturum açma riski güvenlik ilkeleri yapılandırabilirsiniz. Bu ilkeleri kullanarak, kullanıcı veya oturum açma riskli oturum açma işlemleri engellemek ya da gerektirmek için çok faktörlü kimlik doğrulaması gerçekleştirmek için risk düzeyini göz önünde bulundurun. Bu Eylemler, bir saldırganın, zarar verme bir çalınan kimlik yararlanmasını engel olabilir ve kimlik güvenli hale getirmek için biraz zaman tanımanız.

## <a name="sign-in-risk-security-policy"></a>Oturum açma riski İlkesi
Oturum açma riski İlkesi, bir özel oturum açma riskini değerlendirir ve önceden tanımlanmış koşullar ve kurallara dayanan bir risk azaltma işlemleri geçerli bir koşullu erişim ilkesi var.

![Oturum açma riski İlkesi](./media/howto-sign-in-risk-policy/1014.png "oturum açma riski İlkesi")

Azure AD kimlik koruması sağlayarak azaltma riskli oturum açma işlemleri yönetmenize yardımcı olur:

* Kullanıcıları ve grupları ilkenin uygulanacağı ayarlayın:

    ![Oturum açma riski İlkesi](./media/howto-sign-in-risk-policy/1015.png "oturum açma riski İlkesi")
* İlke tetikleyen oturum açma riski düzeyi eşiği (düşük, Orta veya yüksek) ayarlayın:

    ![Oturum açma riski İlkesi](./media/howto-sign-in-risk-policy/1016.png "oturum açma riski İlkesi")
* İlke tetiklendiğinde zorlanacak denetimleri ayarlayın:  

    ![Oturum açma riski İlkesi](./media/howto-sign-in-risk-policy/1017.png "oturum açma riski İlkesi")
* İlke durumu anahtarı:

    ![MFA kaydı](./media/howto-sign-in-risk-policy/403.png "MFA kaydı")
* Gözden geçirin ve bunu etkinleştirdikten önce bir değişikliğin etkisini değerlendirin:

    ![Oturum açma riski İlkesi](./media/howto-sign-in-risk-policy/1018.png "oturum açma riski İlkesi")

## <a name="what-you-need-to-know"></a>Bilmeniz gerekenler
Çok faktörlü kimlik doğrulaması gerektirmek için bir oturum açma riski ilkesi yapılandırabilirsiniz:

![Oturum açma riski İlkesi](./media/howto-sign-in-risk-policy/1017.png "oturum açma riski İlkesi")

Ancak, güvenlik nedeniyle, bu ayar yalnızca çok faktörlü kimlik doğrulaması için zaten kaydedilmiş olan kullanıcılar için çalışır. Kullanıcının, multi-Factor authentication için henüz kaydedilmemiş bir kullanıcı için çok faktörlü kimlik doğrulaması gerektiren bir koşulu karşılaması durumunda engellenir.

Riskli oturum açma işlemleri için multi-Factor authentication gerektirmesine istiyorsanız en iyi uygulama, şunları yapmalısınız:

1. Etkinleştirme [çok faktörlü kimlik doğrulaması kayıt ilkesi](#multi-factor-authentication-registration-policy) etkilenen kullanıcılar için.
2. Etkilenen kullanıcılar oturum açmak için bir MFA kayıt gerçekleştirmek için riskli olmayan bir oturumda gerektirir

Bu adımları bir riskli oturum açma için multi-Factor authentication gerekiyor sağlar.

## <a name="best-practices"></a>En iyi uygulamalar
Seçim bir **yüksek** eşiği ilke tetiklenir ve kullanıcılara etkisini en aza indirir sayısını azaltır.  

Ancak, dışlar **düşük** ve **orta** oturum açma bayrağı risk güvenliği aşılmış kimlik kötüye saldırganın engellemeyebilir ilkesi için.

İlke ayarlanırken

* Olmayan / çok faktörlü kimlik doğrulamasına sahip olmadığınız kullanıcılar hariç
* Kullanıcılar yerel ilkesini etkinleştirme olduğu pratik dışında (örneğin Yardım Masası için hiçbir erişim)
* Çok sayıda yanlış pozitifleri (geliştiriciler, güvenlik analisti) üreteceği kullanıcılar hariç
* Kullanım bir **yüksek** eşiği ilk ilke üretim, sırasında veya son kullanıcılar tarafından görülen sorunları en aza indirmeniz gerekir.
* Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşiği ek kullanıcı oturum açma sorunları, ancak daha fazla güvenlik sunar.

Önerilen çoğu kuruluş için bir kural yapılandırmak için varsayılandır bir **orta** kullanılabilirlik ve güvenlik arasında bir denge için eşiği.

Oturum açma riski İlkesi şöyledir:

* Tüm tarayıcı trafik ve modern kimlik doğrulaması kullanarak oturum açma işlemleri için uygulanır.
* WS-Trust uç noktada ADFS gibi Federasyon IDP devre dışı bırakarak eski güvenlik protokolleri kullanan uygulamalar için geçerli değil.

**Risk olayları** kimlik koruması konsolundaki sayfasında tüm olayları listeler:

* Bu ilke uygulandı
* Gözden geçirme etkinliği ve eylem uygun olup olmadığını belirleme

İlgili kullanıcı deneyimini genel bakış için bkz:

* [Riskli oturum açma kurtarma](flows.md#risky-sign-in-recovery)
* [Riskli engellenen oturum açma](flows.md#risky-sign-in-blocked)  
* [Azure AD kimlik koruması ile oturum açma deneyimleri](flows.md)  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD kimlik koruması** dikey penceresindeki **yapılandırma** bölümünde **oturum açma riski İlkesi**.

    ![Kullanıcı riski İlkesi](./media/howto-sign-in-risk-policy/1014.png "kullanıcı riski İlkesi")




## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
