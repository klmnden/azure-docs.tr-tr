---
title: Tek sayfalı uygulama (uygulama kaydı) - Microsoft kimlik platformu
description: Tek sayfalı uygulama (uygulama kaydı) oluşturmayı öğrenin
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
ms.openlocfilehash: b1faf4669dca2aaaf3f873e66f859473ccd99f10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074838"
---
# <a name="single-page-application---app-registration"></a>Tek sayfalı uygulama - uygulama kaydı

Bu sayfa, bir tek sayfalı uygulama (SPA) için uygulama kaydı özellikleri açıklanmaktadır.

Adımlarını izleyin [Microsoft kimlik platformu ile yeni bir uygulama kaydetme](quickstart-register-app.md), uygulamanızın desteklenen hesaplar'ı seçin. SPA senaryo, kuruluşunuz veya tüm kuruluşlar ve kişisel Microsoft hesapları hesaplarla kimlik doğrulamasını destekler.

Ardından, tek sayfalı uygulamalar için geçerli uygulama kaydı belirli yönleri hakkında bilgi edinin.

## <a name="register-a-redirect-uri"></a>Yeniden yönlendirme URI'si kaydetme

Örtük akış, belirteçleri bir web tarayıcısında çalışan tek sayfalı uygulama içinde bir yeniden yönlendirme gönderir. Bu nedenle bir yeniden yönlendirme URI'si Burada, uygulamanızın belirteçleri alabilir kaydetmek için önemli bir gereksinim gereklidir. Lütfen yeniden yönlendirme URI'si uygulamanız için URI ile tam olarak eşleştiğinden emin olun.

İçinde [Azure portalında](https://go.microsoft.com/fwlink/?linkid=2083908), kayıtlı uygulamanıza gidin **kimlik doğrulaması** seçin uygulama sayfasının **Web** platform ve değerini girin uygulamanıza yönelik yeniden yönlendirme URI'si **yeniden yönlendirme URI'si** alan.

## <a name="enable-the-implicit-flow"></a>Örtük akış etkinleştir

Aynı **kimlik doğrulaması** sayfasındaki **Gelişmiş ayarlar**, ayrıca etkinleştirmelisiniz **örtük vermeyi**. Uygulamanız yalnızca oturum açma kullanıcı ve kimlik belirteçlerini alma gerçekleştiriliyorsa etkinleştirmek yeterli **kimlik belirteçlerini** onay kutusu.

Uygulamanız aynı zamanda API'leri çağırmak için erişim belirteçlerini almak gerekiyorsa, etkinleştirdiğinizden emin olun **erişim belirteçlerini** de onay kutusu. Daha fazla bilgi için [kimlik belirteçlerini](./id-tokens.md) ve [erişim belirteçlerini](./access-tokens.md).

## <a name="api-permissions"></a>API izinleri

Tek sayfa uygulamaları, oturum açmış kullanıcı adına API'leri çağırabilirsiniz. Temsilci izinleri istemek gerekir. Ayrıntılar için bkz [web API'lerine erişim izni ekleyin](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırma](scenario-spa-app-configuration.md)
