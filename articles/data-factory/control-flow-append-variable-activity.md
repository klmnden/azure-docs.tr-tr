---
title: Azure Data factory'de değişken etkinlik ekleme | Microsoft Docs
description: Bir veri fabrikası ardışık düzeninde tanımlanan var olan bir dizi değişkenine bir değer eklemek için değişken Ekle etkinliği ayarlamayı öğrenin
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: douglasl
ms.openlocfilehash: 03652ce20d82565d5714cdc43a01a9e7c3074f6a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48904381"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Data factory'de değişken etkinlik ekleme

Değişken Ekle etkinliği, bir veri fabrikası ardışık düzeninde tanımlanan var olan bir dizi değişkenine bir değer eklemek için kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | İşlem hattındaki etkinliğin adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | Etkinlik AppendVariable türüdür | evet
değer | Belirtilen bir değişkene eklemek için kullanılan dize değişmez değer veya ifade nesne değeri | evet
Değişkenadı | Etkinlik, değişkeni değiştirilecek değişkeni adı 'Array' türünde olmalıdır | evet

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen bir ilgili denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişkenini ayarla etkinliğinin](control-flow-set-variable-activity.md)
