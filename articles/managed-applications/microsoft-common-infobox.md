---
title: Azure InfoBox UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Common.TextBlock UI öğesi açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: 2330197b4512dfdd72de3529145103b644594e25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64711229"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Microsoft.Common.InfoBox kullanıcı Arabirimi öğesi
Bir bilgi kutusu ekler denetimi. Önemli bir metin kutusu içeren veya bunlar sunuyorsunuz değerleri, iş kullanıcılarının yardımcı uyarıları anlıyor. Daha fazla bilgi için bir URI da bağlayabilirsiniz.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Şema
```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="remarks"></a>Açıklamalar

* İçin `icon`, kullanın **hiçbiri**, **bilgisi**, **uyarı**, veya **hata**.
* `uri` Özelliği, isteğe bağlıdır.

## <a name="sample-output"></a>Örnek çıktı

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
