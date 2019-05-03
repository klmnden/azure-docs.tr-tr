---
title: Azure Blockchain sınırları
description: Azure Blockchain hizmetindeki işlev sınırları ve hizmet genel bakış
services: azure-blockchain
keywords: Blok zinciri
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 169ec7a8ef407af3f754046aa8e3b06793a7e962
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028178"
---
# <a name="limits-in-azure-blockchain-service"></a>Azure Blockchain hizmet sınırları

Azure Blockchain hizmet, hizmet ve düğümleri üyesi olabilir, consortium kısıtlamaları ve depolama alanı miktarları sayısı gibi işlevsel sınırları vardır.

## <a name="pricing-tier"></a>Fiyatlandırma katmanı

Temel veya standart fiyatlandırma katmanları Azure blok zinciri hizmeti olup sağlama işlemleri ve Doğrulayıcı düğümleri sınırlarını bağlıdır.

| Fiyatlandırma katmanı | En fazla işlem düğümleri | En fazla Doğrulayıcı düğümleri |
|:---|:---:|:---:|
| Temel | 10 | 1 |
| Standart | 10 | 2 |

Üye oluşturma desteklenmiyor sonra fiyatlandırma katmanını temel ve standart arasında değiştirme.

## <a name="storage-capacity"></a>Depolama kapasitesi

En fazla düğüm başına kayıt defteri verileri ve günlükleri için kullanılan depolama 1 terabayt miktarıdır.

Muhasebe ve günlük depolama boyutunu küçültme desteklenmiyor.

## <a name="consortium-limits"></a>Consortium sınırları

* **Consortium ve üye adları benzersiz olmalıdır** Azure blok zinciri hizmetindeki diğer consortium ve üye adlarından.

* **Üye ve consortium adları değiştirilemez.**

* **Konsorsiyumu tüm üyeleri aynı fiyatlandırma katmanında olmalıdır**

* **İçinde bir konsorsiyum katılan tüm üyeleri aynı bölgede bulunmalıdır.**

    İlk üye Konsorsiyumu içinde oluşturulan bölgeyi belirler. Davet edilen üye consortium ilk üye ile aynı bölgede bulunmalıdır. Tüm üyeleri aynı bölgeye sınırlama, ağ fikir birliğine varılmış olumsuz etkilenmez garanti eder.

* **Konsorsiyumu en az bir yönetici olması gerekir**

    İçinde bir konsorsiyum yalnızca bir yönetici ise bunlar kendilerini consortium kaldıramaz veya üye başka bir yöneticinin eklendiğinde veya consortium yükseltilen kadar sil.

* **Consortium kaldırılan üyeler tekrar eklenemez.**

    Bunun yerine, bunlar consortium katılın ve yeni bir üye oluşturmak için ortamına yeniden davet gerekir. Mevcut kullanıcıların üye kaynağı, geçmiş işlemleri korumak için silinmez.

* **Konsorsiyumu tüm üyeleri aynı kayıt defteri sürüm kullanıyor olmalıdır**

    Düzeltme eki uygulama, güncelleştirmeleri ve Azure blok zinciri hizmetinde kullanılabilir muhasebe sürümleri hakkında daha fazla bilgi için bkz. [düzeltme eki uygulama, güncelleştirmeleri ve sürümleri](ledger-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Düzeltme eki uygulama, güncelleştirmeleri ve sürümleri](ledger-versions.md)
