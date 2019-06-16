---
title: Arka plan programı uygulama çağıran web API'leri (uygulama kaydı) - Microsoft kimlik platformu
description: Web API - uygulama kaydı çağıran bir arka plan programı uygulaması oluşturmayı öğrenin
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
ms.openlocfilehash: 79a355ab226e56a3dde1df5369deda5142d47848
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076248"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Arka plan programı uygulama çağrıları web API'leri - uygulama kaydı

Bir arka plan programı uygulamayı İşte uygulama kaydolurken bilmeniz gerekenler.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Uygulamayı oluştururken arka plan programı uygulamaları yalnızca Azure AD kiracısında mantıklı koşuluyla, seçmeniz gerekir:

- Her iki **hesapları yalnızca kuruluş bu dizinde**. Bu seçenek genellikle arka plan programı uygulamaları satır iş kolu (LOB) geliştiriciler tarafından yazılan en sık karşılaşılan durum aynıdır.
- veya **herhangi bir kuruluş dizini hesaplarında**. Bu seçenek, müşterileriniz için yardımcı aracı sağlayan bir ISV iseniz yapacaksınız. Müşterinin kiracılar yöneticileri bunu onaylaması gerekir.

## <a name="authentication---no-reply-uri-needed"></a>Kimlik doğrulama - gerekli hiçbir yanıt URI'si

Burada gizli istemci uygulamanızın kullandığı durumda **yalnızca** istemci kimlik bilgileri akışı, yanıt URL'si kaydedilmesi gerekmez. Uygulama yapılandırma/yapılandırılması için gerekli değil. İstemci kimlik bilgileri akışı kullanmaz.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>API izinleri - uygulama izinlerini ve yönetici onayı

Daemon uygulamasının yalnızca uygulama izinleri API'lerine (değil temsilci izinleri) isteyebilir. İçinde **API izin** , seçtikten sonra uygulama kayıt sayfasında **bir izin eklemek** ve API ailesi seçilen, **uygulama izinleri**, ve izinleri seçin

![Uygulama izinlerini ve yönetici onayı](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

Arka plan programı uygulamaları sahip bir kiracı Yöneticisi web API'si çağırma uygulama önceden onayı gerektirir. Bu onay aynı sağlanan **API izin** sayfasında, bir kiracı yönetici seçerek **vermek için yönetici onayı *kuruluşumuz***

Çok kiracılı bir uygulama oluşturmaya bir ISV iseniz denetlemek istersiniz [dağıtım - çok kiracılı arka plan programları durumunun](scenario-daemon-production.md#deployment---case-of-multi-tenant-daemon-apps) paragraf.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arka plan programı app - uygulama kodu yapılandırma](./scenario-daemon-app-configuration.md)
