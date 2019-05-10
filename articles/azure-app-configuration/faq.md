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
ms.openlocfilehash: 3181609bf34a04de4e31b73429f9bc5fa3fe3408
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411904"
---
# <a name="azure-app-configuration-faq"></a>Azure uygulama yapılandırması ile ilgili SSS

Bu makalede adresleri, Azure uygulama yapılandırması hakkında sorular sık.

## <a name="how-is-app-configuration-different-from-key-vault"></a>Uygulama yapılandırması Key Vault'tan farklı mı?

Uygulama yapılandırması, ayrı bir kullanım örnekleri kümesi için tasarlanmıştır: uygulama ayarlarını yönetme ve Özellik kullanılabilirliği kontrol olanağı tanır. Birçok karmaşık yapılandırma verileriyle çalışma görevlerini basitleştirmek için amaçlar. Bu, hiyerarşik ad alanı, etiketleme, kapsamlı sorgular, batch alma ve özel yönetim işlemlerini ve kullanıcı arabirimlerini destekler. Uygulama yapılandırması anahtar Kasası'na tamamlayıcı ve iki yan yana çoğu uygulama dağıtımda kullanılmalıdır.

## <a name="should-i-store-secrets-in-app-configuration"></a>Gizli uygulama yapılandırmasında depolamanız gerekir?

Uygulama yapılandırması sıkı güvenlik sağlarken, anahtar kasası yine de uygulama parolalarını depolamak için en iyi yerdir. Bu, donanım düzeyinde şifreleme, ayrıntılı erişim ilkeleri ve sertifika döndürme gibi yönetim işlemlerini sağlar.

## <a name="does-app-configuration-encrypt-my-data"></a>Uygulama yapılandırması, verilerimi şifreleyebilir mi?

Evet. Uygulama yapılandırması tutar ve ağ iletişimi tüm anahtar değerleri şifreler. Anahtar adları dizinlerini yapılandırma verilerini almak için kullanılır ve şifrelenmez.

## <a name="does-app-configuration-support-azure-virtual-network-vnet"></a>Uygulama yapılandırması, Azure sanal ağ (VNET) destekliyor mu?

Henüz değil. VNET desteği genel kullanıma sunulması planlanmaktadır.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>(Test, hazırlama, üretim ve benzeri) birden çok ortam için yapılandırmaları nasıl depolamanız gerekir?

Şu anda mağaza içi düzeyinde kimlerin erişebileceğini uygulama yapılandırmasını kontrol edebilirsiniz. Farklı izinleri gerektiren her bir ortam için ayrı bir depoda kullanmanız gerekir. Bu yaklaşım, en iyi güvenlik yalıtımı sağlar.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Önerilen uygulama yapılandırmasını kullanma yolları nelerdir?

Bkz: [en iyi uygulamalar](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Uygulama yapılandırması nin ücreti ne kadardır?

Hizmet genel Önizleme süresince ücretsiz olarak kullanılabilir.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Nasıl bir sorun bildirin veya öneri sunar?

Bize doğrudan üzerinde ulaşabileceği [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uygulama yapılandırması hakkında](./overview.md)
