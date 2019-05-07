---
title: Çağrıları (üretim taşıma) - Microsoft kimlik platformu web API'leri, mobil uygulama
description: Web API'leri (üretim taşıma) çağıran bir mobil uygulama oluşturmayı öğrenin
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
ms.openlocfilehash: d37d2de561a6f5841bf17a47fef86ad7639750d5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074958"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Web çağıran mobil uygulama API'ler - üretim ortamına taşıyın

Bu makalede, kalite ve uygulamanızı üretim ortamına taşımak için güvenilirliği geliştirme konusunda durmaksızın ayrıntıları sağlar.

## <a name="handling-errors-in-mobile-applications"></a>Mobil uygulamalardaki hataları işleme

Şu ana kadar eyaletini vurguladım farklı akışlarında kaynaklanabilir hata koşulları çeşitli vardır. İşlemek için birincil sessiz hataları ve etkileşim geri dönüş senaryodur. Hiçbir ağ durumlarında, hizmet kesintileri, yönetici onayı gerekli ve diğer senaryoya özel durumları da dahil olmak üzere üretim için de dikkate almanız gereken ek koşullar vardır.

Her MSAL kitaplığı, bu koşullar işleme halinde daha derin türlerine geçiyor örnek kod ve wiki içeriği içeriyor.

- [MSAL Android Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Azaltma ve sorunlarını araştırma

Veri toplama sorunları tanılamak uygulamanızı yardımcı olur. Veri türü hakkında daha fazla ayrıntı için toplamak, her MSAL platformları wiki konusuna bakın.

- Kullanıcılar, Yardım için bir sorunla karşılaşıldığında isteyebilir. En iyi uygulama, yakalayın ve günlükleri geçici olarak depolar ve kullanıcıların herhangi bir yerde karşıya yüklemesine izin sağlamaktır. MSAL yetkili hakkında ayrıntılı bilgi yakalayacak şekilde günlük uzantıları sağlar.
- Mevcut ise, kullanıcıların uygulamanıza nasıl oturum hakkında veri toplamak için MSAL aracılığıyla telemetri etkinleştirin.

## <a name="testing-your-app"></a>Uygulamanızı test etme

Uygulamanızı karşı test etmeyi unutmayın [tümleştirme onay listesi](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
