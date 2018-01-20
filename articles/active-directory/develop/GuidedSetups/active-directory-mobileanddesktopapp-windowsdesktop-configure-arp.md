---
title: "Azure AD v2 Windows Masaüstü tıklatmalarını başlatıldı - Config | Microsoft Docs"
description: "Nasıl bir Windows Masaüstü .NET (XAML) uygulama erişim belirteci almak ve Azure Active Directory v2 bitiş noktası tarafından korunan bir API çağrısı."
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 922fd0e24334eb45995b66dfaaa49d3d4f835e9a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgileri ekleme
Bu adımda, projenize uygulama kimliği eklemeniz gerekir.

1.  Açık `App.xaml.cs` ve satırını içeren Değiştir `ClientId` ile:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Sonraki nedir

[!INCLUDE  [Test and Validate](..\..\..\..\includes\active-directory-develop-guidedsetup-windesktop-test.md)]
