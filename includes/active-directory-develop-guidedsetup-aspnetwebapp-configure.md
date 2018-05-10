---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7c052f30e564700b0a99b76342dc6c34ec5983b7
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı kaydetme ve uygulama kayıt bilgilerinizi çözümünüze eklemek için iki seçeneğiniz vardır:

### <a name="option-1-express-mode"></a>Seçenek 1: Hızlı mod

Aşağıdakileri yaparak, uygulamanızın hızlı bir şekilde kaydedebilirsiniz:

1. Uygulamanızı aracılığıyla kaydetme [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Uygulamanız ve e-posta için bir ad girin
3.  Kurulum destekli seçeneğinin işaretli olduğundan emin olun
4.  Uygulamanıza bir yeniden yönlendirme URL'si eklemek için yönergeleri izleyin

### <a name="option-2-advanced-mode"></a>Seçenek 2: Gelişmiş mod

Uygulamanızı kaydetme ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetmek için
2. Uygulamanız ve e-posta için bir ad girin 
3. Destekli kurulumu için seçeneğinin işaretli olduğundan emin olun
4. Tıklatın `Add Platform`sonra seçin `Web`
5. Visual Studio'ya geri dönün ve Çözüm Gezgini'nde, projeyi seçin ve ardından (bir özellik penceresinde F4 tuşuna basın görmüyorsanız) özellikleri penceresine bakın gidin
6. Değişiklik SSL etkin `True`
7. Visual Studio'da projeye sağ tıklayın ve ardından **özellikleri**ve **Web** sekmesi. İçinde *sunucuları* bölümünde değişiklik *proje URL'sini* SSL URL olması
8. SSL URL'sini kopyalayın ve bu URL yönlendirme URL'si listesine kayıt Portalı'nın listesinde yönlendirme URL'si ekleyin:<br/><br/>![Proje Özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Aşağıdakileri ekleyin `web.config` bölümünün altında kök klasöründe yer `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Değiştir `ClientId` yalnızca kayıtlı uygulama kimliği
11. Değiştir `redirectUri` projenizin SSL URL'si

