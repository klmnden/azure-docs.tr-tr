---
title: Çok faktörlü kimlik doğrulaması Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C ile güvenliği sağlanan tüketiciye yönelik uygulamalar çok faktörlü kimlik doğrulamasını etkinleştirmek nasıl.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3d18e1b2e45aba4e83989e29c533cfc7bf5033fc
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442717"
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Tüketiciye yönelik uygulamalarınızda çok faktörlü kimlik doğrulamasını etkinleştir
Azure Active Directory (Azure AD) B2C doğrudan ile tümleştirilir [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) böylece tüketiciye yönelik uygulamalarınızda kaydolma ve oturum açma deneyimi için ikinci bir güvenlik katmanı ekleyebilirsiniz. Ve bunu tek satır kod yazmak zorunda kalmadan yapabilirsiniz. Şu anda telefon araması ve metin iletisi doğrulama destekliyoruz. Kaydolma ve oturum açma ilkeleri oluşturduysanız, çok faktörlü kimlik doğrulaması yine de etkinleştirebilirsiniz.

> [!NOTE]
> Çok faktörlü kimlik doğrulaması, kaydolma ve oturum ilkeleri, yalnızca mevcut ilkeleri düzenleyerek oluşturduğunuzda da etkinleştirilebilir.
> 
> 

Bu özellik aşağıdaki gibi senaryoları ele uygulamaları yardımcı olur:

* Bir uygulamaya erişmek çok faktörlü kimlik doğrulaması gerektirmez, ancak başka bir erişim gerektirir. Örneğin, tüketici Otomatik Sigorta uygulamaya sosyal veya yerel bir hesap ile oturum açabilirsiniz, ancak aynı dizinde kayıtlı Giriş sigorta uygulamaya erişmeyi önce telefon numaranızı doğrulamak gerekir.
* Multi-Factor Authentication'ın genel bir uygulamaya erişmeye gerektirmez, ancak içerdiği önemli bölümleri erişmek için gerekli. Örneğin, tüketici bankacılık uygulamaya sosyal veya yerel hesap ve hesap bakiyesi onay ile oturum açabilirsiniz, ancak telefon numarasını havale denemeden önce doğrulamanız gerekir.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Kaydolma ilkenizde, çok faktörlü kimlik doğrulamasını etkinleştirmek için değiştirme
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. **Kaydolma ilkeleri**’ne tıklayın.
3. Açmak için kaydolma ilkenizde (örneğin, "B2C_1_SiUp") tıklayın.
4. Tıklayın **çok faktörlü kimlik doğrulaması** ve **durumu** için **ON**. **Tamam**’a tıklayın.
5. Tıklayın **Kaydet** dikey penceresinin üstünde.

Yönelik tüketici deneyimi doğrulamak için ilkeyi "Şimdi Çalıştır" özelliğini kullanabilirsiniz. Şunları onaylayın:

Multi-Factor Authentication adım gerçekleşmeden önce bir tüketici hesabını dizininize oluşturulur. Adımı sırasında kendi telefon numarasını sağlamak ve doğrulamak için tüketici istenir. Doğrulama başarılı olursa, daha sonra kullanmak için tüketici hesabı için telefon numarası eklenir. Tüketici iptal eder ya da bırakır olsa bile, isterse (çok faktörlü kimlik doğrulaması etkinleştirilmiş) yeniden sonraki oturum açma sırasında telefon numaranızı doğrulamak için sorulacaktır.

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını etkinleştirmek için oturum açma ilkesini değiştirme
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Tıklayın **oturum açma ilkeleri**.
3. Açmak için oturum açma ilkenizin (örneğin, "b2c_1_siın") tıklayın. Tıklayın **Düzenle** dikey penceresinin üstünde.
4. Tıklayın **çok faktörlü kimlik doğrulaması** ve **durumu** için **ON**. **Tamam**’a tıklayın.
5. Tıklayın **Kaydet** dikey penceresinin üstünde.

Yönelik tüketici deneyimi doğrulamak için ilkeyi "Şimdi Çalıştır" özelliğini kullanabilirsiniz. Şunları onaylayın:

Tüketici zaman imzalar (sosyal veya yerel bir hesap kullanarak), doğrulanmış bir telefon numarası tüketici hesabına bağlıysa, isterse doğrulamak istenir. Telefon numarası olmayan bağlıysa, bir tane sağlayın ve doğrulamak için tüketici istenir. Doğrulama başarılı, tüketici hesabı daha sonra kullanmak için telefon numarası eklenir.

## <a name="multi-factor-authentication-on-other-policies"></a>Diğer ilkeleri çok faktörlü kimlik doğrulaması
Kaydolma ve oturum açma ilkeleri için yukarıda açıklandığı gibi kayıt sırasında multi-Factor authentication'ı etkinleştirmek mümkündür veya oturum açma ilkeleri ve parola ilkelerini sıfırlayın. Kısa süre içinde profil düzenleme ilkeleri üzerinde kullanılabilir.

