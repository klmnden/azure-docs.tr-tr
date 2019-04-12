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
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 123a2ece06cb58ce6dbb35a914a87eb45fbcf5be
ms.sourcegitcommit: f24b62e352e0512dfa2897362021b42e0cb9549d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59505826"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için iki seçeneğiniz vardır:

### <a name="option-1-express-mode"></a>1. seçenek: Hızlı mod

Aşağıdakileri yaparak, uygulamanızı hızlı bir şekilde kaydedebilirsiniz:

1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) bölmesi.
1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.

### <a name="option-2-advanced-mode"></a>2. seçenek: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:

1. Visual Studio gidin ve:
   - Çözüm Gezgini'nde projeyi seçin ve (bir Özellikler penceresinde, F4 tuşuna görmüyorsanız) özellikler penceresine bakın.
   - Değiştirmek için etkinleştirilmiş SSL `True`.
   - Visual Studio'da projeye sağ tıklayın ve ardından **özellikleri**ve **Web** sekmesi. İçinde *sunucuları* bölümünde değişiklik *proje URL'si* SSL URL olmalı.
   - SSL URL'sini kopyalayın. Bu URL yeniden yönlendirme URL'lerini listesinde kayıt Portal'ın bir sonraki adımda yeniden yönlendirme URL'lerini listesine ekleyecek:<br/><br/>![Proje özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
1. Seçin **yeni kayıt**.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `ASPNET-Tutorial`.
   - Adım 1'deki Visual Studio'dan kopyalanan SSL URL'sini ekleyin (örneği için `https://localhost:44368/`) içinde **yanıt URL'si**, tıklatıp **kaydetme**.
1. **Kimlik doğrulaması** menüsünü seçin, **Örtük izin verme** bölümünde **kimlik belirteçlerini** ayarlayın ve **Kaydet**'i seçin.
1. Aşağıdakileri eklemek `web.config` bölümünde kök klasöründe bulunan `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. Değiştirin `ClientId` yalnızca kayıtlı uygulama Kimliğine sahip.
1. Değiştirin `redirectUri` projenizin SSL URL.
