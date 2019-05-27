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
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: c4dc3d422e18d9ee41bf16ac3e6f22c3d7e466d7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121592"
---
## <a name="register-your-application"></a>Uygulamanızı kaydedin

Uygulamanızı iki yoldan biriyle kaydedebilirsiniz.

### <a name="option-1-express-mode"></a>1. seçenek: Hızlı mod

Aşağıdakileri yaparak, uygulamanızı hızlı bir şekilde kaydedebilirsiniz:
1. Git [Azure Portalı - Uygulama kaydı](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/WinDesktopQuickstartPage/sourceType/docs).
1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.

### <a name="option-2-advanced-mode"></a>2. seçenek: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:
1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
1. Seçin **yeni kayıt**.
   - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `Win-App-calling-MsGraph`.
   - **Desteklenen hesap türleri** bölümünde **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları (ör. Skype, Xbox, Outlook.com)** seçeneğini belirtin.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
   1. İçinde **yeniden yönlendirme URI'leri** bölümünde yeniden yönlendirme URI'leri listesi:
   1. İçinde **türü** Sütun Seç **genel istemci (Mobil ve Masaüstü)**.
   1. Girin `urn:ietf:wg:oauth:2.0:oob` içinde **yeniden yönlendirme URI'si** sütun.
1. **Kaydet**’i seçin.
1. Visual Studio, açık Git *App.xaml.cs* dosya ve sonra değiştirmek `Enter_the_Application_Id_here` yalnızca kayıtlı ve kopyaladığınız uygulama kimliği.

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```
