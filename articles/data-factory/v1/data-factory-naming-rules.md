---
title: Azure Data Factory varlıklarını adlandırma kuralları | Microsoft Docs
description: Data Factory varlıkları için adlandırma kurallarını açıklar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: daf0b1c12ab10230690a62eb5dc772417d8b92f3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60824244"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - adlandırma kuralları
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [adlandırma kuralları Data factory'de](../naming-rules.md).

Aşağıdaki tablo için Data Factory yapıtlarının adlandırma kuralları sağlar.

| Ad | Ad benzersizliğini | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure genelinde benzersiz. Adları büyük/küçük harfe, diğer bir deyişle, `MyDF` ve `mydf` aynı veri fabrikasına bakın. |<ul><li>Her veri fabrikasının tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam ve tire (-) karakteri içermelidir.</li><li>Her tire (-) karakterinin hemen önünde ve bir harf veya sayı tarafından izlenen gerekir. Kapsayıcı adlarında art arda tirelere izin verilmez.</li><li>Ad 3 ila 63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/tablolar/işlem hatları |Veri fabrikasında ile benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>Tablo adı karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Karakterler kullanılamaz: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Kaynak Grubu |Microsoft Azure genelinde benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>En fazla karakter sayısı: 1000.</li><li>Ad harf, rakam ve şu karakterleri içerebilir: "-", "_",","ve"."</li></ul> |

