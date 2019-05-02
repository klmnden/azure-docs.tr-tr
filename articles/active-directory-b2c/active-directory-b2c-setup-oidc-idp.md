---
title: Openıd Connect - Azure Active Directory B2C ile oturum açma ve kaydolma | Microsoft Docs
description: Azure Active Directory B2C kullanarak Openıd Connect ile kaydolma ve oturum açma ayarlayın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 81968aa3ba9f082194f4f447161a3eef7e014374
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704140"
---
# <a name="set-up-sign-up-and-sign-in-with-openid-connect-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak Openıd Connect ile kaydolma ve oturum açma ayarlama

>[!NOTE]
> Bu özellik genel önizleme aşamasındadır. Bu özellik, üretim ortamında kullanmayın.


[Openıd Connect](active-directory-b2c-reference-oidc.md) kullanıcıların güvenli bir şekilde oturum açmak için kullanılan OAuth 2.0 üzerinde yerleşik bir kimlik doğrulama protokolüdür. Bu protokolü kullanan çoğu kimlik sağlayıcıları, Azure AD B2C'de desteklenir. Bu makalede, özel Openıd Connect kimlik sağlayıcıları kullanıcı akışlarınızı nasıl ekleyebileceğiniz açıklanmaktadır.

## <a name="add-the-identity-provider"></a>Kimlik sağlayıcısı Ekle

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. İçin **kimlik sağlayıcısı türü**seçin **Openıd Connect (Önizleme)**.

## <a name="configure-the-identity-provider"></a>Kimlik sağlayıcısı yapılandırma

Her Openıd Connect kimlik sağlayıcısı oturum açma gerçekleştirmek için gereken bilgilerin çoğunu içeren bir meta veri belgesinin açıklar. Bu, hizmetin ortak İmzalama anahtarları konumunu ve kullanılacak URL'leri gibi bilgileri içerir. Openıd Connect meta veri belgesi her zaman ile biten bir uç nokta şu konumdadır: `.well-known\openid-configuration`. Openıd Connect kimlik sağlayıcısı için aradığınız eklemek için meta veri URL'sini girin.

Oturum açmasına izin vermek için geliştiricilerin kendi hizmetinde uygulamayı kaydetme kimlik sağlayıcısı gerektirir. Bu uygulama şeklinde adlandırılan bir Kimliğe sahip **istemci kimliği** ve **gizli**. Kimlik sağlayıcısı'ndan bu değerleri kopyalayın ve bunlara karşılık gelen alanlara girin.

> [!NOTE]
> Gizli anahtar isteğe bağlıdır. Ancak, kullanmak istiyorsanız, bir istemci parolası girmeniz gerekir [yetkilendirme kod akışı](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), belirteç kodunu değişimi için gizli anahtarı kullanır.

Kapsam bilgileri ve izinleri toplamak için özel kimlik sağlayıcınızdan aradığınız tanımlar. Openıd Connect istekleri içermelidir `openid` kimlik sağlayıcısından gelen kimlik belirteci almak için kapsam değeri. Kimlik belirteci kullanıcılar Azure AD B2C'ye oturum olmayan özel kimlik sağlayıcısı kullanarak. Diğer kapsamları, boşlukla ayrılmış eklenmiş. Ne diğer kapsamları olabilir görmek için özel kimlik sağlayıcısının belgelerine başvurun kullanılabilir.

Hangi tür bilgiler için ilk çağrıda geri gönderilen yanıt türü tanımlayan `authorization_endpoint` özel kimlik sağlayıcısının. Aşağıdaki yanıt türleri kullanılabilir:

- `code`: Olarak başına [yetkilendirme kod akışı](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), kodu bir Azure AD B2C geri döndürülür. Azure AD B2C geçer çağrılacak `token_endpoint` belirteç kodunu Exchange.
- `token`: Bir erişim belirteci, özel bir kimlik sağlayıcısından Azure AD B2C geri döndürülür.
- `id_token`: Bir kimlik belirteci, özel bir kimlik sağlayıcısından Azure AD B2C geri döndürülür.

Yanıt modu verileri özel bir kimlik sağlayıcısından Azure AD B2C'ye göndermek için kullanılması gereken yöntemi tanımlar. Şu yanıt modları kullanılabilir:

- `form_post`: Bu yanıt modu, en iyi güvenlik için önerilir. Yanıt, HTTP üzerinden aktarılan `POST` kodunu veya belirteci kullanarak gövdesi kodlanmakta yöntemi `application/x-www-form-urlencoded` biçimi.
- `query`: Kodunu veya belirteci bir sorgu parametresi olarak döndürülür.

Etki alanı ipucu doğrudan oturum açma sayfasında kullanıcı yap kullanılabilir kimlik sağlayıcıları listesi arasında bir seçim yerine, belirtilen kimlik sağlayıcısının atlamak için kullanılabilir. Bu tür bir davranış izin vermek için etki alanı ipucu için bir değer girin. Parametreyi atlamak için özel kimlik sağlayıcısı eklemek `domain_hint=<domain hint value>` sonuna kadar oturum açma için Azure AD B2C çağırırken isteğiniz.

Sonra özel kimlik sağlayıcısı, Azure AD B2C, Azure AD B2C alınan belirteci gelen talepler için Azure AD B2C tanır ve kullanır talepleri eşlenebilmesi için gerekiyorsa geri dön kimlik belirteci gönderir. Her biri aşağıdaki eşlemeler için özel kimlik sağlayıcısının kimlik sağlayıcısının belirteçleri geri döndürülen talepleri anlamak için belgelere bakın:

- `User ID`: Oturum açmış olan kullanıcı için benzersiz tanımlayıcı sağlar talep girin.
- `Display Name`: Görünen ad veya kullanıcının tam adını sağlayan talep girin.
- `Given Name`: Kullanıcının ilk adını sağlayan talep girin.
- `Surname`: Son kullanıcı adını sağlayan talep girin.
- `Email`: Kullanıcının e-posta adresi sağlayan talep girin.

