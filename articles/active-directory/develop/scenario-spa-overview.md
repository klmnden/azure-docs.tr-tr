---
title: JavaScript tek sayfalı uygulama senaryoya genel bakış - Microsoft kimlik platformu
description: Microsoft kimlik platformu tümleştiren tek sayfalı uygulama (senaryoya genel bakış) oluşturmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07a21e83f304f3e1acc0ed4033d832dd8e901ac9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076368"
---
# <a name="scenario-single-page-application"></a>Senaryo: Tek sayfalı uygulama

Tek sayfalı uygulama (SPA) oluşturmak için gereken her şeyi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

İlk uygulamanızı, JavaScript SPA'ya hızlı başlangıcı izleyerek oluşturabilirsiniz:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Tek sayfalı uygulama](./quickstart-v2-javascript.md)

## <a name="overview"></a>Genel Bakış

Birçok modern web uygulamaları, JavaScript veya Angular ve Vue.js React.js gibi bir SPA framework kullanılarak yazılmış istemci tarafı tek sayfa uygulamaları olarak oluşturulur. Bu uygulamalar, bir web tarayıcısında çalıştırma ve geleneksel sunucu tarafı web uygulamaları'den farklı kimlik doğrulama özellikleri vardır. Tek sayfalı uygulama kullanıcılarının oturumunu ve arka uç hizmetlerine veya web API'lerini kullanarak erişim belirteçleri almak Microsoft kimlik platformu sağlayan [OAuth 2.0 örtük akışını](./v2-oauth2-implicit-grant-flow.md). Örtük akış, kimliği doğrulanmış bir kullanıcıyı temsil eder ve ayrıca korunan API'leri çağırmak için gereken belirteçlerini erişmek için kullanılan kimlik belirteçlerini almak için uygulama izin verir.

![Tek sayfa uygulamaları](./media/scenarios/spa-app.svg)

Bu kimlik doğrulama akışı Elektron, React-Native gibi platformlar arası JavaScript çerçeveleri ve benzeri kullanarak uygulama senaryoları içermez. beri bunlar daha fazla özellikleri için yerel platformları ile etkileşimi gerektirir.

## <a name="specifics"></a>Özellikleri

Uygulamanız için bu senaryoyu etkinleştirmek için aşağıdaki bilgiler gereklidir:

* Azure AD ile uygulama kaydı örtük akışını etkinleştirme ve belirteçleri döndürülen bir yeniden yönlendirme URI'si ayarı içerir.
* Uygulama kimliği gibi kayıtlı uygulama özelliklere sahip uygulama yapılandırması
* Oturum açmak ve belirteçleri almak için kimlik doğrulama akışını yapmak için MSAL kitaplığını kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-spa-app-registration.md)
