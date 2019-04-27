---
title: Azure Data Factory'de değişken etkinlik ayarlama | Microsoft Docs
description: Değişkenini ayarla etkinliğinin bir veri fabrikası ardışık düzeninde tanımlanan var olan bir değişkenin değerini ayarlamak için kullanmayı öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: 71abfdff629f36b278488851b546c7371353a4d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60767974"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Azure Data factory'de kümesi değişken etkinliği

Değişkenini ayarla etkinliğinin, String, Bool veya bir veri fabrikası ardışık düzeninde tanımlanan dizi türünde var olan bir değişkenin değerini ayarlamak için kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | İşlem hattındaki etkinliğin adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | SetVariable etkinlik türüdür | evet
value | Belirtilen değişken ayarlamak için kullanılan dize değişmez değer veya ifade nesne değeri | evet
Değişkenadı | Bu etkinlik tarafından ayarlanan değişkeni adı | evet


## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen bir ilgili denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişken etkinlik ekleme](control-flow-append-variable-activity.md)
