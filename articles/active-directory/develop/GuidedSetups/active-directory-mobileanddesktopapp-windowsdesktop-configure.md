---
title: "Azure AD v2 Windows Masaüstü tıklatmalarını başlatıldı - Config | Microsoft Docs"
description: "Nasıl bir Windows Masaüstü .NET (XAML) uygulama erişim belirteci almak ve Azure Active Directory v2 bitiş noktası tarafından korunan bir API çağrısı. | Microsoft Azure | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
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
ms.openlocfilehash: 1dfaa7ade664e43dcb9aa788b0197ca17e6ec4cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="create-an-application-express"></a>(Hızlı) uygulama oluşturma
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Uygulamanızı aracılığıyla kaydetme [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Uygulamanız ve e-posta için bir ad girin
3.  Kurulum destekli seçeneğinin işaretli olduğundan emin olun
4.  Uygulama Kimliğini almak ve kodunuza yapıştırmak için yönergeleri izleyin

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Uygulama kayıt bilgilerinizi çözümünüze (Gelişmiş) ekleyin
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetmek için
2. Uygulamanız ve e-posta için bir ad girin 
3. Destekli kurulumu için seçeneğinin işaretli olduğundan emin olun
4. Tıklatın `Add Platform`seçeneğini belirleyip `Native Application` ve kaydetme ulaştı.
5. Uygulama kimliği GUID kopyalayın, Visual Studio, açık dön `App.xaml.cs` ve değiştirme `your_client_id_here` yalnızca kayıtlı uygulama kimliği:

```csharp
private static string ClientId = "your_application_id_here";
```
