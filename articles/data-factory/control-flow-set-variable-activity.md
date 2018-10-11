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
ms.devlang: na
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: douglasl
ms.openlocfilehash: ff9bfce1f9262d78ba17abdd88c481d5057d5f38
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49076428"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Azure Data factory'de kümesi değişken etkinliği

Değişkenini ayarla etkinliğinin, String, Bool veya bir veri fabrikası ardışık düzeninde tanımlanan dizi türünde var olan bir değişkenin değerini ayarlamak için kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | İşlem hattındaki etkinliğin adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | SetVariable etkinlik türüdür | evet
değer | Belirtilen değişken ayarlamak için kullanılan dize değişmez değer veya ifade nesne değeri | evet
Değişkenadı | Bu etkinlik tarafından ayarlanan değişkeni adı | evet


## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen bir ilgili denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişken etkinlik ekleme](control-flow-append-variable-activity.md)
