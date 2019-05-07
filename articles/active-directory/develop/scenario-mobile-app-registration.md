---
title: Mobil uygulama web API'leri - uygulama kodu yapılandırma çağrıları | Microsoft kimlik platformu
description: Bir mobil uygulama oluşturmayı öğrenin çağrıları veritabanını web API'leri (uygulama kodu yapılandırma)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9eac9d1dfce79ac4ad9d49a6cfe6b7dee7f6681a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075168"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>Mobil uygulama web API'leri - uygulama kaydı çağrıları

Bu makale, bir mobil uygulama oluşturmak için uygulama kayıt yönergeleri içerir.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Mobil uygulamalarda desteklenen hesap türleri, etkinleştirmek istediğiniz deneyimi ve uygulamanızın hedeflediği kullanıcılar bağlıdır.

## <a name="platform-configuration-and-redirect-uris"></a>Platform yapılandırmasını ve yeniden yönlendirme URI'leri  

Bir mobil uygulama oluştururken, en önemli kayıt yeniden yönlendirme URI'si adımdır. Bu, aracılığıyla ayarlanabilir [platform yapılandırmasını kimlik doğrulaması dikey penceresinde](https://aka.ms/MobileAppReg).

Bu deneyim, çoklu oturum açma (SSO) Microsoft Authenticator (ve android'de Intune Şirket portalı) aracılığıyla destek yanı sıra cihaz Yönetimi ilkeleri için uygulamanızı sağlayacaktır.

El ile yeniden yönlendirme URI'si yapılandırmak isterseniz, uygulama bildirimi bunu yapabilirsiniz. Önerilen biçimi aşağıdaki şekildedir:

- ***iOS***: `msauth.<BUNDLE_ID>://auth`
- ***Android***: `msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - Android imza karma KeyTool komutu aracılığıyla yayınlama veya hata ayıklama anahtarlar kullanılarak oluşturulabilir.

## <a name="api-permissions"></a>API izinleri

Mobil uygulamaları, oturum açmış kullanıcı adına API'lerini çağırma. Kapsamı olarak da adlandırılan temsilci izinleri istemek uygulamanız gerekir. İstenen deneyimi bağlı olarak, bu statik olarak Azure portalı üzerinden veya çalışma zamanında dinamik olarak yapılabilir. Statik olarak izinleri kaydetme, uygulamanızı kolayca onaylanacak yöneticilerinin sağlar ve önerilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir belirteç alınırken](scenario-mobile-acquire-token.md)
