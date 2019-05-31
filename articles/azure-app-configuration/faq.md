---
title: Azure uygulama yapılandırması ile ilgili SSS | Microsoft Docs
description: Azure uygulama yapılandırması hakkında sık sorulan sorular
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: e321c0b473b110597b5b87a6e67666737116daa2
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393343"
---
# <a name="azure-app-configuration-faq"></a>Azure uygulama yapılandırması ile ilgili SSS

Bu makalede Azure uygulama yapılandırması hakkında sık sorulan soruları ele alır.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>Uygulama Yapılandırması’nın Azure Key Vault’tan farkı nedir?

Uygulama yapılandırması, ayrı bir kullanım örnekleri kümesi için tasarlanmıştır: uygulama ayarlarını yönetme ve Özellik kullanılabilirliği kontrol olanağı tanır. Birçok karmaşık yapılandırma verileriyle çalışma görevlerini basitleştirmek için amaçlar.

Uygulama yapılandırması destekler:

- Hiyerarşik ad alanları
- Etiketleme
- Kapsamlı sorgular
- Batch alma
- Özel yönetim işlemleri
- Özellik Yönetimi kullanıcı arabirimi

Uygulama yapılandırması, anahtar Kasası'na tamamlayıcı ve iki yan yana çoğu uygulama dağıtımda kullanılmalıdır.

## <a name="should-i-store-secrets-in-app-configuration"></a>Gizli uygulama yapılandırmasında depolamanız gerekir?

Uygulama yapılandırması sıkı güvenlik sağlasa da, Key Vault hala uygulama parolalarını depolamak için en iyi yerdir. Key Vault, donanım düzeyinde şifreleme, ayrıntılı erişim ilkeleri ve sertifika döndürme gibi yönetim işlemlerini sağlar.

## <a name="does-app-configuration-encrypt-my-data"></a>Uygulama yapılandırması, verilerimi şifreleyebilir mi?

Evet. Uygulama yapılandırması tuttuğu tüm anahtar değerleri şifreler ve ağ iletişimi şifreler. Anahtar adları, yapılandırma verilerini almak için dizin kullanılır ve şifreli değildir.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>(Test, hazırlama, üretim ve benzeri) birden çok ortam için yapılandırmaları nasıl depolamanız gerekir?

Şu anda mağaza içi düzeyinde kimlerin erişebileceğini uygulama yapılandırmasını kontrol edebilirsiniz. Farklı izinleri gerektiren her bir ortam için ayrı bir deposunu kullanın. Bu yaklaşım, en iyi güvenlik yalıtımı sağlar.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Önerilen uygulama yapılandırmasını kullanma yolları nelerdir?

Bkz: [en iyi uygulamalar](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Uygulama yapılandırması nin ücreti ne kadardır?

Hizmet genel Önizleme süresince ücretsiz olarak kullanılabilir.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Nasıl bir sorun bildirin veya öneri sunar?

Bize doğrudan üzerinde ulaşabileceği [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uygulama yapılandırması hakkında](./overview.md)
