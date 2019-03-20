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
ms.date: 05/04/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: cce0bb9d1a9317396d197d182a424a45c8448f1b
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203642"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için iki seçeneğiniz vardır:

### <a name="option-1-express-mode"></a>1. seçenek: Hızlı mod

Aşağıdakileri yaparak, uygulamanızı hızlı bir şekilde kaydedebilirsiniz:

1. Uygulamanızı kaydetmek [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure).
2. Uygulamanız ve e-posta adresiniz için bir ad girin.
3. Destekli kurulum seçeneğinin işaretli olduğundan emin olun.
4. Uygulamanız için bir yeniden yönlendirme URL'si eklemek için yönergeleri izleyin.

### <a name="option-2-advanced-mode"></a>2. seçenek: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:

1. Uygulamayı kaydetmek için [Microsoft Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/portal/register-app)’na gidin.
2. Uygulamanız ve e-posta adresiniz için bir ad girin.
3. Destekli kurulum seçeneğini işaretli olduğundan emin olun
4. Seçin `Add Platform`ve ardından `Web`.
5. Visual Studio ve geri, Çözüm Gezgini'nde, projeyi seçin ve (bir Özellikler penceresinde, F4 tuşuna görmüyorsanız) özellikler penceresine bakın.
6. Değiştirmek için etkinleştirilmiş SSL `True`.
7. Visual Studio'da projeye sağ tıklayın ve ardından **özellikleri**ve **Web** sekmesi. İçinde *sunucuları* bölümünde değişiklik *proje URL'si* SSL URL olmalı.
8. SSL URL'sini kopyalayın ve bu URL yeniden yönlendirme URL'lerini kayıt portalının listesini yeniden yönlendirme URL'lerini listesinde ekleyin:<br/><br/>![Proje özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Aşağıdakileri eklemek `web.config` bölümünde kök klasöründe bulunan `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Değiştirin `ClientId` yalnızca kayıtlı uygulama Kimliğine sahip.
11. Değiştirin `redirectUri` projenizin SSL URL.
