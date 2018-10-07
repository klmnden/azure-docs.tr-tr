---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: faa3ad2376935aee4508b814f1b67fdacb98cf6e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843578"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için iki seçeneğiniz vardır:

### <a name="option-1-express-mode"></a>1. seçenek: Hızlı mod

Aşağıdakileri yaparak, uygulamanızı hızlı bir şekilde kaydedebilirsiniz:

1. Uygulamanızı kaydetmek [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Uygulamanız ve e-posta adresiniz için bir ad girin
3.  Destekli kurulum seçeneğinin işaretli olduğundan emin olun
4.  Bir yeniden yönlendirme URL'si uygulamanıza eklemek için yönergeleri izleyin

### <a name="option-2-advanced-mode"></a>2. seçenek: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetme
2. Uygulamanız ve e-posta adresiniz için bir ad girin 
3. Destekli kurulum seçeneğini işaretli olduğundan emin olun
4. Tıklayın `Add Platform`seçin `Web`
5. Visual Studio ve geri, Çözüm Gezgini'nde, projeyi seçin ve (bir Özellikler penceresinde, F4 tuşuna görmüyorsanız) özellikler penceresine bakın.
6. SSL Etkin ayarını `True` olarak değiştirin
7. Visual Studio'da projeye sağ tıklayın ve ardından **özellikleri**ve **Web** sekmesi. İçinde *sunucuları* bölümünde değişiklik *proje URL'si* SSL URL olmalı
8. SSL URL'sini kopyalayın ve bu URL yeniden yönlendirme URL'lerini kayıt portalının listesini yeniden yönlendirme URL'lerini listesinde ekleyin:<br/><br/>![Proje özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Aşağıdakileri eklemek `web.config` bölümünde kök klasöründe bulunan `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Değiştirin `ClientId` yalnızca kayıtlı uygulama Kimliğine sahip
11. Değiştirin `redirectUri` projenizin SSL URL'si

