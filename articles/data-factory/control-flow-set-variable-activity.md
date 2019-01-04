---
title: Azure Data Factory'de değişken etkinlik ayarlama | Microsoft Docs
description: Değişkenini ayarla etkinliğinin bir veri fabrikası ardışık düzeninde tanımlanan var olan bir değişkenin değerini ayarlamak için kullanmayı öğrenin
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: douglasl
ms.openlocfilehash: cc573028779bcd6b77394bbeefbea58f714b835c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017353"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Azure Data factory'de kümesi değişken etkinliği

Değişkenini ayarla etkinliğinin, String, Bool veya bir veri fabrikası ardışık düzeninde tanımlanan dizi türünde var olan bir değişkenin değerini ayarlamak için kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gereklidir
-------- | ----------- | --------
ad | İşlem hattındaki etkinliğin adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | SetVariable etkinlik türüdür | evet
değer | Belirtilen değişken ayarlamak için kullanılan dize değişmez değer veya ifade nesne değeri | evet
Değişkenadı | Bu etkinlik tarafından ayarlanan değişkeni adı | evet


## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen bir ilgili denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişken etkinlik ekleme](control-flow-append-variable-activity.md)
