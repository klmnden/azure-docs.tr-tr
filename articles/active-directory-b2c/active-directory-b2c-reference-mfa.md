---
title: Çok faktörlü kimlik doğrulaması Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C ile güvenliği sağlanan tüketiciye yönelik uygulamalar çok faktörlü kimlik doğrulamasını etkinleştirmek nasıl.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: eabae0f3575719c6cb93affefe0a393dd13d1439
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014015"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Azure Active Directory B2C ile çok faktörlü kimlik doğrulamasını etkinleştirin

Azure Active Directory (Azure AD) B2C doğrudan ile tümleştirilir [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) böylece uygulamalarınızı kaydolma ve oturum açma deneyimi için ikinci bir güvenlik katmanı ekleyebilirsiniz. Multi-Factor authentication, tek satırlık bir kod yazmadan etkinleştirin. Oturum'kurmak zaten oluşturduğunuz ve oturum açma ilkeleri, yine de etkinleştirebilirsiniz, çok faktörlü kimlik doğrulaması.

Bu özellik aşağıdaki gibi senaryoları ele uygulamaları yardımcı olur:

- Bir uygulamaya erişmek için çok faktörlü kimlik doğrulaması gerektirmeyen, ancak başka bir erişim gerektirir. Örneğin, müşteri Otomatik Sigorta uygulamaya sosyal veya yerel bir hesap ile oturum açabilirsiniz, ancak aynı dizinde kayıtlı Giriş sigorta uygulamaya erişmeyi önce telefon numaranızı doğrulamak gerekir.
- Genel bir uygulamaya erişmesi için çok faktörlü kimlik doğrulaması gerektirmez, ancak içindeki önemli bölümleri erişmek için gerekli. Örneğin, müşteri bir sosyal bankacılık uygulamayla oturum açarak veya yerel hesap ve çek hesabı dengelemek, ancak havale denemeden önce telefon numaranızı doğrulamak gerekir.

## <a name="set-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması

Bir ilke oluşturduğunuzda, çok faktörlü kimlik doğrulamasını etkinleştirmek için seçeneğiniz vardır.

![Çok faktörlü kimlik doğrulaması](./media/active-directory-b2c-reference-mfa/add-policy.png)

Ayarlama **durumu** için **üzerinde**.

Kullanabileceğiniz **Şimdi Çalıştır** deneyimi doğrulamak için ilkedeki. Aşağıdaki senaryoyu onaylayın:

Çok faktörlü kimlik doğrulaması adımı gerçekleşmeden önce bir müşteri hesabı kiracınız oluşturulur. Adımı sırasında bir telefon numarası sağlayın ve doğrulamak için müşteri istenir. Doğrulama başarılı olursa, telefon numarası daha sonra kullanmak için hesaba eklenir. Müşteri iptal ettiğinde veya tarihini bırakır bile yeniden sonraki oturum açma sırasında multi-Factor authentication ile etkin bir telefon numaranızı doğrulamak için müşteri sorulabilir.

## <a name="add-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması ekleme

Daha önce oluşturduğunuz bir ilke çok faktörlü kimlik doğrulamasını etkinleştirmek mümkündür. 

Çok faktörlü kimlik doğrulamasını etkinleştirmek için:

1. İlkeyi açın ve ardından **Düzenle**. 
2. Seçin **çok faktörlü kimlik doğrulaması**
3. Ayarlama **durumu** için **üzerinde**.
4. Sayfanın üst kısmından **Kaydet**'e tıklayın.


