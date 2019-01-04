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
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: douglasl
ms.openlocfilehash: e904075908fe7108c0566856b25fe03be0b7fd86
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023827"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Data factory'de değişken etkinlik ekleme

Değişken Ekle etkinliği, bir veri fabrikası ardışık düzeninde tanımlanan var olan bir dizi değişkenine bir değer eklemek için kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gereklidir
-------- | ----------- | --------
ad | İşlem hattındaki etkinliğin adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | Etkinlik AppendVariable türüdür | evet
değer | Belirtilen bir değişkene eklemek için kullanılan dize değişmez değer veya ifade nesne değeri | evet
Değişkenadı | Etkinlik, değişkeni değiştirilecek değişkeni adı 'Array' türünde olmalıdır | evet

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen bir ilgili denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişkenini ayarla etkinliğinin](control-flow-set-variable-activity.md)
