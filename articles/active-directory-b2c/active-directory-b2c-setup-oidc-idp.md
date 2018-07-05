---
title: Openıd Connect kimlik sağlayıcıları Azure Active Directory B2C, yerleşik ilkeleri ekleme | Microsoft Docs
description: Openıd Connect sağlayıcısının içinde Azure AD B2C'yi yerleşik ilkeler ekleme kılavuzuna genel bakış.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e5baf95cd2426c9753620cba7c5a67b4c1672788
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444234"
---
# <a name="azure-active-directory-b2c-add-a-custom-openid-connect-identity-provider-in-built-in-policies"></a>Azure Active Directory B2C: özel Openıd Connect kimlik sağlayıcı yerleşik ilkeleri ekleyin.

>[!NOTE]
> Bu özellik genel Önizleme aşamasındadır. Bu özellik, üretim ortamında kullanmayın.

[Openıd Connect](http://openid.net/specs/openid-connect-core-1_0.html) kullanıcıların güvenli bir şekilde oturum açmak için kullanılan OAuth 2.0 üzerinde yerleşik bir kimlik doğrulama protokolüdür. Gibi bu protokolü kullanan çoğu kimlik sağlayıcıları [Azure AD'ye](active-directory-b2c-setup-oidc-azure-active-directory.md), Azure AD B2C'de desteklenir. Bu makalede, özel Openıd Connect kimlik sağlayıcıları, yerleşik ilkeleri nasıl ekleyebileceğinizi açıklar.

## <a name="configuring-a-custom-openid-connect-identity-provider"></a>Özel Openıd Connect kimlik sağlayıcısını yapılandırma

Özel Openıd Connect kimlik sağlayıcısı eklemek için:

1. Bu adımları [Azure AD B2C ayarlarına gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
1. Tıklayarak **kimlik sağlayıcıları**.
1. **Ekle**'ye tıklayın.
1. İçin **kimlik sağlayıcısı türü**seçin **Openıd Connect**.

### <a name="setting-up-the-openid-connect-identity-provider"></a>Openıd Connect kimlik sağlayıcısını ayarlama

#### <a name="metadata-url"></a>Meta veri URL'si

Belirtimine göre her Openıd Connect kimlik sağlayıcısı oturum açma gerçekleştirmek için gereken bilgilerin çoğunu içeren bir meta veri belgesinin açıklar. Bu, hizmetin ortak İmzalama anahtarları konumunu ve kullanılacak URL'leri gibi bilgileri içerir. Openıd Connect meta veri belgesi her zaman ile biten bir uç nokta şu konumdadır: `.well-known\openid-configuration`.

Openıd Connect kimlik sağlayıcısı için aradığınız eklemek için meta veri URL'sini girin.

#### <a name="client-id-and-secret"></a>İstemci kimliği ve parolası

Oturum açmasına izin vermek için kimlik sağlayıcısı geliştiricilerin kendi hizmetinde uygulamayı kaydetme gerektirir. Bu uygulama bir Kimliğe sahip (olarak adlandırılan **istemci kimliği**) ve bir **gizli**. Kimlik sağlayıcısı'ndan bu değerleri kopyalayın ve bunlara karşılık gelen alanlara girin.

> [!NOTE]
> Gizli anahtar isteğe bağlıdır. Ancak, kullanmak istiyorsanız, bir istemci parolası girmeniz gerekir [yetkilendirme kod akışı](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), belirteç kodunu değişimi için gizli anahtarı kullanır.

#### <a name="scope"></a>Kapsam

Kapsamlar ve bilgi toplamak için özel kimlik sağlayıcınızdan aradığınız izinleri tanımlayın. Openıd Connect istekleri içermelidir `openid` kimlik sağlayıcısından gelen kimlik belirteci almak için kapsam değeri. Kimlik belirteci kullanıcılar Azure AD B2C'ye açmanız mümkün olmayacaktır özel kimlik sağlayıcısı kullanarak.

Diğer kapsamları (boşlukla ayrılmış) eklenmiş. Ne diğer kapsamları olabilir görmek için özel kimlik sağlayıcısının belgelerine başvurun kullanılabilir.

#### <a name="response-type"></a>Yanıt türü

Hangi tür bilgiler için ilk çağrıda geri gönderilecek yanıt türü tanımlayan `authorization_endpoint` özel kimlik sağlayıcısının. 

* `code`: Olarak başına [yetkilendirme kod akışı](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), kodu bir Azure AD B2C geri döndürülür. Azure AD B2C çağrılacak sonra devam `token_endpoint` belirteç kodunu Exchange.
* `token`: Bir erişim belirteci, özel bir kimlik sağlayıcısından Azure AD B2C geri döndürülür.
* `id_token`: Bir kimlik belirteci, özel bir kimlik sağlayıcısından Azure AD B2C geri döndürülür.


#### <a name="response-mode"></a>Yanıt modu

Yanıt modu verileri özel bir kimlik sağlayıcısından Azure AD B2C'ye göndermek için kullanılması gereken yöntemi tanımlar.

* `form_post`: Yanıt modu bu en iyi güvenlik için önerilir. Yanıt, HTTP üzerinden aktarılan `POST` kodunu veya belirteci kullanarak gövdesi kodlanmakta yöntemi `application/x-www-form-urlencoded` biçimi.
* `query`: Kodunu veya belirteci, sorgu parametresi olarak döndürülür.


#### <a name="domain-hint"></a>Etki alanı ipucu

Etki alanı ipucu doğrudan oturum açma sayfasında kullanıcı yap kullanılabilir kimlik sağlayıcıları listesi arasında bir seçim yerine, belirtilen kimlik sağlayıcısının atlamak için kullanılabilir. Bu tür bir davranış izin vermek için etki alanı ipucu için bir değer girin.

Parametreyi atlamak için özel kimlik sağlayıcısı eklemek `domain_hint=<domain hint value>` sonuna kadar oturum açma için Azure AD B2C çağırırken isteğiniz.


### <a name="mapping-the-claims-from-the-openid-connect-identity-provider"></a>Openıd Connect kimlik sağlayıcısından gelen talepleri eşleme

Sonra özel kimlik sağlayıcısı, Azure AD B2C, Azure AD B2C alınan belirteci gelen talepler için Azure AD B2C tanır ve kullanır talepleri eşlenebilmesi için gerekiyorsa geri dön kimlik belirteci gönderir. 

Her biri aşağıdaki eşlemeleri için kimlik sağlayıcısının belirteçleri geri döndürülen talepleri anlamak için özel kimlik sağlayıcısının belgelerine başvurun.

* `User ID`: Oturum açmış olan kullanıcı için benzersiz tanımlayıcı sağlar talep girin.
* `Display Name`: Kullanıcının tam adını ve görünen ad sağlayan talep girin.
* `Given Name`: Kullanıcının ilk adını sağlayan talep girin.
* `Surname`: Son kullanıcı adını sağlayan talep girin.
* `Email`: Kullanıcının e-posta adresi sağlayan talep girin.

## <a name="next-steps"></a>Sonraki adımlar

Özel Openıd Connect kimlik sağlayıcısına eklemek, [yerleşik ilke](active-directory-b2c-reference-policies.md).
