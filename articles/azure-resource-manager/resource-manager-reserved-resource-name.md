---
title: Azure ayrılmış kaynak yapılan ad hatalarını | Microsoft Docs
description: Ayrılmış bir sözcük içeren bir kaynak adı sağlanırken hataları gidermek nasıl açıklar.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 922389b7c6c1bb7ad1d9b8f6ec35ccc1c5656723
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683947"
---
# <a name="resolve-reserved-resource-name-errors"></a>Ayrılmış kaynak adı hataları çözün

Bu makalede, ayrılmış bir sözcük adını içeren bir kaynak dağıtım yaparken karşılaştığınız hata.

## <a name="symptom"></a>Belirti

Genel bir uç nokta kullanılabilir olan bir kaynak dağıtım yaparken aşağıdaki hata iletisini alabilirsiniz:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Nedeni

Genel bir uç nokta içeren kaynak adı ayrılmış bir sözcük veya ticari kullanamazsınız.

Aşağıdaki sözcükler ayrılmıştır:

* ERİŞİM
* AZURE
* BING
* BIZSPARK
* BIZTALK
* CORTANA
* DIRECTX
* DOTNET
* DYNAMICS
* EXCEL
* EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* HYPERV
* KINECT
* LYNC
* MSDN
* O365
* OFFICE
* OFFICE365
* ONEDRIVE
* ONENOTE
* OUTLOOK
* POWERPOINT
* SHAREPOINT
* SKYPE KURUMSAL
* VISIO
* VISUALSTUDIO

Aşağıdaki sözcükler, tam bir sözcük veya bir alt dize adı olarak kullanılamaz:

* OTURUM AÇMA
* MICROSOFT
* WINDOWS
* XBOX

## <a name="solution"></a>Çözüm

Ayrılmış sözcükler birini kullanmayan bir ad sağlayın.