---
title: Azure kaynak adı hataları ayrılmış | Microsoft Docs
description: Ayrılmış bir sözcük içeren bir kaynak adı sağlanırken hatalarını çözümlemeyi açıklar.
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
ms.openlocfilehash: b91a53d17d64afb0a56f745505f10e8cabbc22cc
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="resolve-reserved-resource-name-errors"></a>Ayrılmış kaynak adı hatalarını çözümleme

Bu makalede, ayrılmış bir sözcük adını içeren bir kaynağa dağıtırken karşılaştığınız hatası açıklanır.

## <a name="symptom"></a>Belirti

Ortak bir uç noktası aracılığıyla kullanılabilir olan bir kaynak dağıtırken, aşağıdaki hata iletisini alabilirsiniz:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Nedeni

Genel bir uç nokta sahip kaynakları ayrılmış sözcükleri veya ticari adı kullanamazsınız.

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
* HYPER-V
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
* SKYPE
* VISIO
* VİSUAL STUDİO

Aşağıdaki sözcükler, tam bir sözcük veya bir alt dize adı olarak kullanılamaz:

* OTURUM AÇMA
* MICROSOFT
* WINDOWS
* XBOX

## <a name="solution"></a>Çözüm

Ayrılmış sözcükler birini kullanmayan bir ad sağlayın.