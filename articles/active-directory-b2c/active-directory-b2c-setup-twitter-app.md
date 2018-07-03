---
title: Azure Active Directory B2C için twitter yapılandırması | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan Twitter hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 6/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: dad35f26496306558a6e0105db86321c497a8306
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37343029"
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Tüketiciler için Azure AD B2C kullanarak Twitter hesabıyla kaydolma ve oturum açma sağlayın

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak twitter'ı kullanmak için bir Twitter uygulaması oluşturun ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için bir Twitter hesabıyla ihtiyacınız var. Yoksa, adresinden edinebilirsiniz [ https://twitter.com/signup ](https://twitter.com/signup).

1. Git [Twitter uygulamaları](https://apps.twitter.com/) ve kimlik bilgilerinizle oturum açın.
2. Tıklayın **yeni uygulama oluştur**.
3. Formda sağlamak için bir değer **adı**, **açıklama**, ve **Web sitesi**.
4. İçin **geri çağırma URL'si**, girin `https://login.microsoftonline.com/te/{tenant}/{policyId}/oauth1/authresp`. Değiştirdiğinizden emin olun **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ve {Policyıd} ilke kimliğinizle (örneğin, b2c_1_policy).  Bu geri çağırma URL'si tümü küçük harf olması gerekir. Bir geri çağırma URL'si için Twitter oturum açma kullanan tüm ilkeleri eklemeniz gerekir. Kullandığınızdan emin olun `b2clogin.com` yerine ` login.microsoftonline.com` uygulamanızda kullanıyorsanız.
5. Onay kutusunu kabul etmeniz **Geliştirici sözleşmesi** tıklatıp **kendi Twitter uygulamanızı oluşturun**.
6. Uygulama oluşturulduktan sonra listeden, select seçin **ayarları** sekmesine ve ardından **ayarlarını güncelleştirme**.
7. Seçin **anahtarlar ve erişim belirteçleri** sekmesi.
8. Değerini kopyalayın **tüketici anahtarı** ve **tüketici gizli**. Her ikisi de kiracınızdaki bir kimlik sağlayıcısı olarak Twitter'ı yapılandırmak için gerekir.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak twitter'ı yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracısının genel Yöneticisi olarak. 
2. Azure AD B2C kiracınıza geçiş yapmak için portalın sağ üst köşesinde bulunan Azure AD B2C dizinini seçin.
3. Tıklayın **tüm hizmetleri**ve ardından **Azure AD B2C** altında hizmetler listesinden **güvenlik + kimlik**.
4. Tıklayın **kimlik sağlayıcıları**.
5. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Twitter" girin.
6. Tıklayın **kimlik sağlayıcısı türü**seçin **Twitter (Önizleme)**, tıklatıp **Tamam**.
7. Tıklayın **bu kimlik sağlayıcısını ayarlama** ve Twitter girin **tüketici anahtarı** için **istemci kimliği** ve Twitter **tüketici gizli** için **gizli**.
8. Tıklayın **Tamam**ve ardından **Oluştur** Twitter yapılandırmanızı kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma veya düzenleme bir [yerleşik ilke](active-directory-b2c-reference-policies.md) ve kimlik sağlayıcısı olarak Twitter'ı ekleyin.