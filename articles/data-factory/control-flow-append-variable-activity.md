---
title: Azure Data factory'de değişken etkinlik ekleme | Microsoft Docs
description: Bir veri fabrikası ardışık düzeninde tanımlanan var olan bir dizi değişkenine bir değer eklemek için değişken Ekle etkinliği ayarlamayı öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/09/2018
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: a5efe946000eb00e65d314ae53d7136761e2109d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60557231"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Data factory'de değişken etkinlik ekleme

Değişken Ekle etkinliği, bir veri fabrikası ardışık düzeninde tanımlanan var olan bir dizi değişkenine bir değer eklemek için kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
name | İşlem hattındaki etkinliğin adı | Evet
description | Etkinliğin ne yaptığını açıklayan metin | hayır
türü | Etkinlik AppendVariable türüdür | evet
value | Belirtilen bir değişkene eklemek için kullanılan dize değişmez değer veya ifade nesne değeri | evet
Değişkenadı | Etkinlik, değişkeni değiştirilecek değişkeni adı 'Array' türünde olmalıdır | evet

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen bir ilgili denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişkenini ayarla etkinliğinin](control-flow-set-variable-activity.md)
