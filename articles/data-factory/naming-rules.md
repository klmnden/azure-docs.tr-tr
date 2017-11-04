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
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 8f0207853097a3d31e06a722025f3964678414bf
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - adlandırma kuralları
Aşağıdaki tabloda için Data Factory yapıtlarının adlandırma kuralları sağlar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin 1 sürümünü kullanıyorsanız, olduğu genel olarak kullanılabilir (GA) bkz. [Data Factory version1 kurallarında adlandırma](v1/data-factory-naming-rules.md).

| Ad | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure arasında benzersiz. Adları büyük küçük harf duyarsız, diğer bir deyişle, `MyDF` ve `mydf` aynı veri fabrikası bakın. |<ul><li>Her veri fabrikası tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayı ile başlamalı ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakterinden hemen önünde ve bir harf veya sayı gelmelidir gerekir. Kapsayıcı adlarında art arda tirelere izin verilmez.</li><li>Adı 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/tablolar/işlem hatları |Veri Fabrikası'nda ile benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>Bir tablo adı karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Kaynak Grubu |Microsoft Azure arasında benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>En fazla karakter sayısı: 1000.</li><li>Ad harf, rakam ve şu karakterleri içerebilir: "-", "_",","ve"."</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikaları içindeki adım adım insturctions izleyerek oluşturmayı öğrenin [hızlı başlangıç: bir veri fabrikası oluşturun](quickstart-create-data-factory-powershell.md) makalesi. 