---
title: Korumalı Web API'sini - uygulama kaydı | Azure
description: Korumalı Web API'si ve uygulamayı kaydetmek için gereken bilgileri oluşturmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 59af4e20c7fe838f7c725b47e45968941fa85cb7
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254067"
---
# <a name="protected-web-api---app-registration"></a>Korumalı web API'si - uygulama kaydı

Bu makalede, korumalı web API'si için uygulama kaydı özellikleri açıklanmaktadır.

Bkz: [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](quickstart-register-app.md) uygulamanın nasıl kaydedileceği hakkında genel adımlar için.

## <a name="accepted-token-version"></a>Belirteci kabul edilen sürüm

Microsoft kimlik platformu uç nokta, iki tür belirteçleri verebilir: v1.0 ve v2.0 belirteçler. Bu belirteçleri hakkında daha fazla bilgi [erişim belirteçlerini](access-tokens.md). Kabul edilen belirteci sürüm bağımlı **desteklenen hesap türleri** uygulamanızı oluştururken seçtiğiniz:

- Varsa değerini **desteklenen hesap türleri** olduğu **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları (örneğin, Skype, Xbox, Outlook.com) hesaplarında**, kabul edilen belirteci sürüm v2.0 olacaktır.
- Aksi takdirde, kabul edilen belirteci sürüm v1.0 olacaktır.

Uygulamayı oluşturduktan sonra aşağıdaki adımları izleyerek kabul edilen belirteci sürüm değiştirebilirsiniz:

1. Azure portalında, uygulamanızı seçin ve ardından **bildirim** uygulamanız için.
2. Bildirimde, arama **"accessTokenAcceptedVersion"** , değeri görmek ve **2**. Bu özellik, Azure AD web API'si v2.0 belirteçleri kabul ettiğini bildiğiniz olanak tanır. Eğer öyleyse **null**, kabul edilen belirteci sürüm v1.0 olacaktır.
3. **Kaydet**’i seçin.

> [!NOTE]
> Bu web API'sine (v1.0 veya v2.0) belirteci hangi sürümü kabul ettiği karar aittir. İstemcileri, web API'sini kullanarak Microsoft kimlik platformu v2.0 uç noktası için bir belirteç istediğinde, web API'si tarafından hangi sürümü kabul gösteren bir belirteç elde edersiniz.

## <a name="no-redirect-uri"></a>Yeniden yönlendirme URI'si

Web API'leri, hiçbir kullanıcı, oturum açmış etkileşimli olarak olduğu gibi bir yeniden yönlendirme URI'si kaydolması gerekmez.

## <a name="expose-an-api"></a>Bir API'yi kullanıma sunmak

Başka bir web API'leri için belirli açık bir API ve sunulan kapsamları ayarıdır.

### <a name="resource-uri-and-scopes"></a>Kaynak URI'si ve kapsamları

Kapsamları genellikle şu biçimdedir `resourceURI/scopeName`. Microsoft Graph için kısayolları gibi kapsamlar `User.Read`, ancak bu dize yalnızca bir kısayol bulunur `https://graph.microsoft.com/user.read`.

Uygulama kaydı sırasında aşağıdaki parametreleri tanımlamanız gerekir:

- Uygulama kayıt portalı varsayılan bir kaynak URI - önerir, kullanılacak `api://{clientId}`. Bu kaynak URI'si benzersiz ancak insan değil okunabilir. Bunu değiştirmek, ancak bunun benzersiz olduğundan emin olun.
- Bir veya birden çok kapsamı

Kapsam Ayrıca uygulamanızı kullanan son kullanıcılar için sunulan onay ekranında görüntülenir. Bu nedenle, kapsamı tanımlamak karşılık gelen dizeleri sağlamanız gerekir:

- Son kullanıcı tarafından görülen
- Kimlerin yönetici onayı verebilir Kiracı Yöneticisi tarafından görülen

### <a name="how-to-expose-the-api"></a>API nasıl sunacağınızı öğrenin

1. Seçin **bir API'yi kullanıma sunmak** uygulama kaydı bölümünde ve:
   1. **Kapsam ekle**’yi seçin.
   1. Önerilen uygulama kimliği URI'si kabul (API :// {ClientID}) seçerek **Kaydet ve devam et**.
   1. Aşağıdaki parametreleri girin:
      - İçin **kapsam adı**, kullanın `access_as_user`.
      - İçin **kimin onay**, emin olun **yöneticileri ve kullanıcılar** seçeneği belirlenir.
      - İçinde **yönetici onayı görünen adı**, türü `Access TodoListService as a user`.
      - İçinde **yönetici onayı açıklaması**, türü `Accesses the TodoListService Web API as a user`.
      - İçinde **kullanıcı onayı görünen adı**, türü `Access TodoListService as a user`.
      - İçinde **kullanıcı onayı açıklaması**, türü `Accesses the TodoListService Web API as a user`.
      - Tutun **durumu** kümesine **etkin**.
      - Seçin **kapsamı Ekle**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırma](scenario-protected-web-api-app-configuration.md)
