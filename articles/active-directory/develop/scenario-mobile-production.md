---
title: Çağrıları (üretim taşıma) - Microsoft kimlik platformu web API'leri, mobil uygulama
description: Bir mobil uygulama oluşturmayı öğrenin çağrıları veritabanını web API'leri (üretim taşıma)
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
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d8b6a5c2a29228de806088ea93e197d42bf1ab47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65962343"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Web çağıran mobil uygulama API'ler - üretim ortamına taşıyın

Bu makalede, üretime geçmeden önce kalite ve uygulama güvenilirliği geliştirme konusunda ayrıntılar sağlar.

## <a name="handling-errors-in-mobile-applications"></a>Mobil uygulamalardaki hataları işleme

Hata koşulları sayısı, uygulamanızda bu noktada ortaya çıkabilir. İşlemeye yönelik temel senaryolar sessiz hataları ve etkileşim için geri dönüşler ' dir. Üretim için göz önünde bulundurmanız gereken diğer koşullar no-ağ durumlarda, hizmet kesintileri, yönetici onayı için gereksinimleri ve diğer senaryoya özel durumları içerir.

Bu koşulların nasıl ele alınacağını açıklar örnek kod ve wiki içeriği her MSAL kitaplığı sahiptir:

- [MSAL Android Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Azaltma ve sorunlarını araştırma

Uygulamanızdaki sorunları tanılamak için veri toplamak için yardımcı olur. Veri türleri hakkında bilgi toplamak, MSAL platform Wiki konusuna bakın.

- Kullanıcılar, karşılaştıkları sorunları olduğunda yardım isteyin. En iyi uygulama, yakalama ve geçici olarak günlüklerini depolamak ve kullanıcılar bunları burada karşıya bir konum sağlayın sağlamaktır. MSAL, kimlik doğrulaması hakkında ayrıntılı bilgi yakalayacak şekilde günlük uzantılar sağlar.
- Varsa, kullanıcıların uygulamanıza nasıl oturum hakkında veri toplamak için MSAL aracılığıyla telemetri etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
