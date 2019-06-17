---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 5ce0f18c1ec7a0fcb6465ab20e774976552687f1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67132984"
---
## <a name="set-up-your-web-server-or-project"></a>Web sunucusu veya proje ayarlama

> Bunun yerine bu örneği ait projeyi indirmek tercih ediyorsunuz? Aşağıdakilerden birini yapın:
> 
> - Node.js gibi bir yerel web sunucusu ile projeyi çalıştırmak için [proje dosyalarını indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> - (İsteğe bağlı) IIS sunucusu ile projeyi çalıştırmak için [Visual Studio projeyi indirmek](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).
>
> Ve ardından, yürütmeden önce onun kod örneği yapılandırmak için atlamak [yapılandırma adımı](#register-your-application).

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticide çalıştırmak için bir yerel web sunucusu aşağıdaki gibi ihtiyacınız [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), ya da IIS Express tümleştirmesiyle [Visual Studio 2017](https://www.visualstudio.com/downloads/).

* Projeyi çalıştırmak için Node.js kullanıyorsanız, bir tümleşik geliştirme ortamı (IDE) gibi yükleyen [Visual Studio Code](https://code.visualstudio.com/download), proje dosyalarını düzenlemek için.

* Bu kılavuzdaki yönergelere Node.js hem Visual Studio 2017'yi temel alır, ancak herhangi bir geliştirme ortamı veya web sunucusu kullanabilirsiniz.

## <a name="create-your-project"></a>Kendi projenizi oluşturun

> ### <a name="option-1-nodejs-or-other-web-servers"></a>1\. seçenek: Node.js veya diğer web sunucuları
> Yüklediğinizden emin olun [Node.js](https://nodejs.org/en/download/), ve ardından aşağıdakileri yapın:
> - Uygulamanızı barındırmak için bir klasör oluşturun.
>
> ### <a name="option-2-visual-studio"></a>2\. seçenek: Visual Studio
> Visual Studio kullanıyorsanız ve yeni bir proje oluşturuyorsanız, aşağıdakileri yapın:
> 1. Visual Studio'da **dosya** > **yeni** > **proje**.
> 1. **Visual C#\Web** bölümünde **ASP.NET Web Uygulaması (.NET Framework)** girişini seçin.
> 1. Uygulamanız için bir ad girin ve ardından **Tamam**.
> 1. Altında **yeni ASP.NET Web uygulaması**seçin **boş**.

## <a name="create-the-spa-ui"></a>SPA kullanıcı Arabirimi oluşturma
1. Oluşturma bir *index.html* , JavaScript SPA'ya dosyası. Visual Studio kullanıyorsanız, proje (projenin kök klasöründe), sütuna sağ tıklayıp seçin **Ekle** > **yeni öğe** > **HTML sayfası**, ve Dosya adı *index.html*.

1. İçinde *index.html* dosyasında, aşağıdaki kodu ekleyin:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

   > [!TIP]
   > Önceki komut MSAL.js sürümünde altında yayımlanan en son sürümle değiştirebilirsiniz [MSAL.js sürümleri](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).
