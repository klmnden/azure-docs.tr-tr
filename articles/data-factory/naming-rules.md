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
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: c02a9393de72b827b7e38b52d06589f042d581b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60787011"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - adlandırma kuralları
Aşağıdaki tablo için Data Factory yapıtlarının adlandırma kuralları sağlar.

| Ad | Ad benzersizliğini | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure genelinde benzersiz. Adları büyük/küçük harfe, diğer bir deyişle, `MyDF` ve `mydf` aynı veri fabrikasına bakın. |<ul><li>Her veri fabrikasının tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam ve tire (-) karakteri içermelidir.</li><li>Her tire (-) karakterinin hemen önünde ve bir harf veya sayı tarafından izlenen gerekir. Kapsayıcı adlarında art arda tirelere izin verilmez.</li><li>Ad 3 ila 63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/veri kümeleri/işlem hatları |Veri fabrikasında ile benzersiz. Adları büyük/küçük harfe duyarsızdır. |<ul><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Karakterler kullanılamaz: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li><li>Tire ("-") bağlı hizmetler ve yalnızca veri kümesi adlarındaki izin verilmez.</li></ul>  |
| Kaynak Grubu |Microsoft Azure genelinde benzersiz. Adları büyük/küçük harfe duyarsızdır. | Daha fazla bilgi için bkz. [Azure adlandırma kuralları ve kısıtlamaları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). |

## <a name="next-steps"></a>Sonraki adımlar
Adım adım yönergeleri uygulayarak veri fabrikaları oluşturmayı öğrenmek [hızlı başlangıç: veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md) makalesi. 
