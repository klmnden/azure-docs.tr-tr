---
title: Azure Active Directory B2C çok faktörlü kimlik doğrulaması | Microsoft Docs
description: Azure Active Directory B2C tarafından güvence altına tüketiciye yönelik uygulamalarında çok faktörlü kimlik doğrulamasını etkinleştirmek nasıl.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 85592275c13719fec0f40eb76a9bce48890a59d2
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34710202"
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Tüketiciye yönelik uygulamalarınızda çok faktörlü kimlik doğrulamasını etkinleştir
Azure Active Directory (Azure AD) B2C ile doğrudan tümleşir [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md) böylece tüketiciye yönelik uygulamalarınızda kaydolma ve oturum açma deneyimleri için ikinci bir güvenlik katmanı ekleyebilirsiniz. Ve tek satırlık bir kod yazmak zorunda kalmadan bunu yapabilirsiniz. Şu anda telefon çağrısı ve metin iletisi doğrulama destekler. Kaydolma ve oturum açma ilkeleri oluşturduysanız, çok faktörlü kimlik doğrulaması yine etkinleştirebilirsiniz.

> [!NOTE]
> Varolan ilkeleri yalnızca düzenleyerek kaydolma ve oturum açma ilkeleri oluşturduğunuzda, çok faktörlü kimlik doğrulaması de etkinleştirilebilir.
> 
> 

Bu özellik aşağıdaki gibi senaryoları işlemek uygulamaları yardımcı olur:

* Bir uygulamaya erişmek çok faktörlü kimlik doğrulaması gerekmez, ancak başka bir erişimini gerektirir. Örneğin, tüketici uygulamaya Otomatik Sigorta sosyal veya yerel bir hesap ile oturum açabilirsiniz, ancak aynı dizinde kayıtlı Giriş sigorta uygulamaya erişmeyi önce telefon numarasını doğrulamanız gerekir.
* Genel bir uygulamaya erişmek çok faktörlü kimlik doğrulaması gerekmez, ancak içerdiği hassas bölümleri erişimini gerektirir. Örneğin, tüketici sosyal veya yerel hesabı ve onay hesap bakiyesini sahip bir bankacılık uygulaması için oturum açabilirsiniz, ancak bir kablo aktarımı denemeden önce telefon numarasını doğrulamanız gerekir.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını etkinleştirmek için kayıt ilkesini değiştirme
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. **Kaydolma ilkeleri**’ne tıklayın.
3. Açmak için kayıt ilkesi (örneğin, "B2C_1_SiUp") tıklayın.
4. Tıklatın **çok faktörlü kimlik doğrulaması** ve **durumu** için **ON**. **Tamam**’a tıklayın.
5. Tıklatın **kaydetmek** dikey pencerenin üstündeki.

Müşteri Deneyimi doğrulamak için ilkeyi "Şimdi Çalıştır" özelliğini kullanabilirsiniz. Şunları onaylayın:

Çok faktörlü kimlik doğrulama adımı oluşmadan önce bir tüketici hesabı dizininizde oluşturulan. Adımı sırasında kendi telefon numarasını sağlamak ve doğrulamak için tüketici istedi. Doğrulama başarılı olursa, telefon numarası daha sonra kullanmak için tüketici hesabı bağlı. Tüketici iptal eder ya da bırakır olsa bile, isterse (çok faktörlü kimlik doğrulaması etkinleştirilmiş) yeniden sonraki oturum açma sırasında telefon numaranızı doğrulamak için sorulacaktır.

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını etkinleştirmek için oturum açma ilkesini değiştirme
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Tıklatın **oturum açma ilkeleri**.
3. Açmak için oturum açma ilkenizin (örneğin, "B2C_1_SiIn") tıklayın. Tıklatın **Düzenle** dikey pencerenin üstündeki.
4. Tıklatın **çok faktörlü kimlik doğrulaması** ve **durumu** için **ON**. **Tamam**’a tıklayın.
5. Tıklatın **kaydetmek** dikey pencerenin üstündeki.

Müşteri Deneyimi doğrulamak için ilkeyi "Şimdi Çalıştır" özelliğini kullanabilirsiniz. Şunları onaylayın:

Tüketici açtığında (bir sosyal veya yerel hesabı kullanarak), bir doğrulanmış telefon numarası tüketici hesabına bağlıysa, isterse doğrulamak için istedi. Telefon numarası bağlıysa, bir sağlamak ve doğrulamak için tüketici istedi. Başarılı doğrulamayı tüketici hesabı daha sonra kullanmak için telefon numarasını bağlı.

## <a name="multi-factor-authentication-on-other-policies"></a>Diğer ilkeler çok faktörlü kimlik doğrulaması
Kaydolma ve oturum açma için ilkeler yukarıda açıklandığı gibi ilkeleri ilkeler oturum açma ve parola sıfırlama veya çok faktörlü kimlik doğrulamasını kaydolma etkinleştirmek mümkündür. En kısa sürede ilkelerini düzenleme profilinde kullanıma sunulacaktır.

