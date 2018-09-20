---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: cc5272ed133f4fa5c99eb3b3a397ac2c53b93b7c
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46473894"
---
## <a name="setting-up-your-web-server-or-project"></a>Web sunucunuzda veya proje ayarlama

> Bunun yerine bu örneği ait projeyi indirmek tercih ediyorsunuz?
> - [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> or
> - [Proje dosyalarını indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) düğüm gibi bir yerel web sunucusu
>
> Ve ardından atlamak [yapılandırma adımı](#register-your-application) çalıştırmadan önce kodu örneği yapılandırmak için.

## <a name="prerequisites"></a>Önkoşullar
Bir yerel web sunucusu gibi [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), ya da IIS Express tümleştirmesiyle [Visual Studio 2017](https://www.visualstudio.com/downloads/) Bu öğretici çalıştırmak için gereklidir.

Bu kılavuzdaki yönergelere hem Node.js hem de Visual Studio 2017'yi temel alır, ancak herhangi bir geliştirme ortamı veya Web sunucusu kullanmaktan çekinmeyin.

## <a name="create-your-project"></a>Kendi projenizi oluşturun

> ### <a name="option-1-visual-studio"></a>Seçenek 1: Visual Studio
> Visual Studio kullanarak ve yeni bir proje oluşturuyorsanız, yeni bir Visual Studio çözümü oluşturmak için aşağıdaki adımları izleyin:
> 1.    Visual Studio'da:  `File` > `New` > `Project`
> 2.    Altında `Visual C#\Web`seçin `ASP.NET Web Application (.NET Framework)`
> 3.    Uygulamanızı adlandırın ve tıklayın *Tamam*
> 4.    Altında `New ASP.NET Web Application`seçin `Empty`

<p/><!-- -->

> ### <a name="option-2-node-other-web-servers"></a>2. seçenek: Düğüm / diğer web sunucuları
> Yüklediğinizden emin olun [Node.js](https://nodejs.org/en/download/), ardından aşağıdaki adımları izleyin:
> - Uygulamanızı barındırmak için bir klasör oluşturun.


## <a name="create-your-single-page-applications-ui"></a>Tek sayfalı uygulamanızın kullanıcı Arabirimi oluşturma
1.  Oluşturma bir *index.html* , JavaScript SPA'ya dosyası. Visual Studio kullanıyorsanız, ' % s'projesi (projenin kök klasöründe) seçin, sağ tıklatın ve seçin: `Add`  >  `New Item`  >  `HTML page` index.html adlandırın
2.  Sayfanız için aşağıdaki kodu ekleyin:
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling the page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn to reference msal.js (recommended).
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>

    <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
