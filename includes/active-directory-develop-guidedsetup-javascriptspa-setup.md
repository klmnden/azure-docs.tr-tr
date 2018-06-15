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
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 794fd51c38f66b24193c7da7a145d58f7a225b30
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32202553"
---
## <a name="setting-up-your-web-server-or-project"></a>Web sunucusu veya projesi ayarlama

> Bu örnek 's proje yerine indirmeyi tercih ediyorsunuz? 
> - [Visual Studio projesi indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> or
> - [Proje dosyalarını indirmek](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) Python gibi bir yerel web sunucusu için
>
> Ve ardından geçin [yapılandırma adımı](#register-your-application) kod örneği çalıştırmadan önce yapılandırmak için.

## <a name="prerequisites"></a>Önkoşullar
Bir yerel web sunucusu gibi [Python http.server](https://www.python.org/downloads/), [http sunucu](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), ya da IIS Express ile tümleştirme [Visual Studio 2017](https://www.visualstudio.com/downloads/) Bu Destekli kurulumu çalıştırmak için gereklidir. 

Bu kılavuzdaki yönergeleri Python ve Visual Studio 2017 bağlıdır, ancak herhangi bir geliştirme ortamı veya Web sunucusu kullanmak üzere çekinmeyin.

## <a name="create-your-project"></a>Projenizi oluşturma 

> ### <a name="option-1-visual-studio"></a>Seçenek 1: Visual Studio 
> Visual Studio kullanarak ve yeni proje oluşturma, yeni bir Visual Studio çözüm oluşturmak için aşağıdaki adımları izleyin:
> 1.    Visual Studio'da:  `File` > `New` > `Project`
> 2.    Altında `Visual C#\Web`seçin `ASP.NET Web Application (.NET Framework)`
> 3.    Uygulamanızı adlandırın ve tıklayın *Tamam*
> 4.    Altında `New ASP.NET Web Application`seçin `Empty`

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a>Seçenek 2: Python / diğer web sunucuları
> Yüklediğinizden emin olun [Python](https://www.python.org/downloads/), aşağıdaki adımları izleyin:
> - Uygulamanızı barındırmak için bir klasör oluşturun.


## <a name="create-your-single-page-applications-ui"></a>Tek sayfalı uygulama kullanıcı Arabirimi oluşturma
1.  Oluşturma bir *index.html* JavaScript SPA dosyası. Visual Studio kullanıyorsanız, projeyi (Proje kök klasöründe) seçin, sağ tıklatın ve seçin: `Add`  >  `New Item`  >  `HTML page` ve index.html adlandırın
2.  Sayfanıza aşağıdaki kodu ekleyin:
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
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.3/js/msal.min.js"></script>

    <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
