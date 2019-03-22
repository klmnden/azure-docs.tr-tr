---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5c2bce5635dfe488c1725efdd2954ecd81cf8e79
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58214428"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgilerini uygulamanıza ekleme

Bu adımda, uygulama kimliği projenize eklemeniz gerekir.

1. Açık `App.xaml.cs` içeren satırı değiştirin `ClientId` ile:

```csharp
private static string ClientId = "[Enter the application Id here]";
```
