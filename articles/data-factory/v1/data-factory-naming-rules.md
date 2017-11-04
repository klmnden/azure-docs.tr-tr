---
title: "Azure Data Factory varlıklarını adlandırma kuralları | Microsoft Docs"
description: "Data Factory varlıklarını için adlandırma kurallarını açıklar."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 2f4979a07db794f012ba602ab65ac54872003027
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - adlandırma kuralları
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [adlandırma kurallarını sürüm 2 veri fabrikasında](../naming-rules.md).

Aşağıdaki tabloda için Data Factory yapıtlarının adlandırma kuralları sağlar.

| Ad | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure arasında benzersiz. Adları büyük küçük harf duyarsız, diğer bir deyişle, `MyDF` ve `mydf` aynı veri fabrikası bakın. |<ul><li>Her veri fabrikası tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayı ile başlamalı ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakterinden hemen önünde ve bir harf veya sayı gelmelidir gerekir. Kapsayıcı adlarında art arda tirelere izin verilmez.</li><li>Adı 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/tablolar/işlem hatları |Veri Fabrikası'nda ile benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>Bir tablo adı karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Kaynak Grubu |Microsoft Azure arasında benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>En fazla karakter sayısı: 1000.</li><li>Ad harf, rakam ve şu karakterleri içerebilir: "-", "_",","ve"."</li></ul> |

