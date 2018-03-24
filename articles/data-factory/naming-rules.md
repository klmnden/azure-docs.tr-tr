---
title: Azure Data Factory varlıklarını adlandırma kuralları | Microsoft Docs
description: Data Factory varlıklarını için adlandırma kurallarını açıklar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: 0d1ff97aef7be7fa9f9f07f2743e1a1a9399e48a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - adlandırma kuralları
Aşağıdaki tabloda için Data Factory yapıtlarının adlandırma kuralları sağlar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory version1 kurallarında adlandırma](v1/data-factory-naming-rules.md).

| Ad | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure arasında benzersiz. Adları büyük küçük harf duyarsız, diğer bir deyişle, `MyDF` ve `mydf` aynı veri fabrikası bakın. |<ul><li>Her veri fabrikası tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayı ile başlamalı ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakterinden hemen önünde ve bir harf veya sayı gelmelidir gerekir. Kapsayıcı adlarında art arda tirelere izin verilmez.</li><li>Adı 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/tablolar/işlem hatları |Veri Fabrikası'nda ile benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>Bir tablo adı karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li><li>Kısa çizgi ("-") bağlı hizmetler ve veri kümeleri yalnızca adlarını izin verilmiyor.</li></ul>  |
| Kaynak Grubu |Microsoft Azure arasında benzersiz. Adları büyük/küçük harfe duyarsızdır. | Daha fazla bilgi için bkz: [Azure adlandırma kuralları ve sınırlamaları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). |

## <a name="next-steps"></a>Sonraki adımlar
Adım adım yönergeleri izleyerek veri fabrikaları oluşturmayı öğrenin [hızlı başlangıç: bir veri fabrikası oluşturun](quickstart-create-data-factory-powershell.md) makalesi. 