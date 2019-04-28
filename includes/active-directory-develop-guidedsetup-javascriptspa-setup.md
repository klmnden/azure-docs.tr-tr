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
ms.openlocfilehash: da2477b19327273fe922ac81f909233cb4ef8f06
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298015"
---
## <a name="setting-up-your-web-server-or-project"></a>Web sunucunuzda veya proje ayarlama

> Bunun yerine bu örneği ait projeyi indirmek tercih ediyorsunuz?
> - [Proje dosyalarını indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) düğüm gibi bir yerel web sunucusu
>
> or
> - [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)
>
> Ve ardından atlamak [yapılandırma adımı](#register-your-application) çalıştırmadan önce kodu örneği yapılandırmak için.

## <a name="prerequisites"></a>Önkoşullar
Bir yerel web sunucusu gibi [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), ya da IIS Express tümleştirmesiyle [Visual Studio 2017](https://www.visualstudio.com/downloads/) Bu öğretici çalıştırmak için gereklidir.

Projeyi çalıştırmak için node.js kullanarak, bir IDE gibi yükleyin [Visual Studio Code](https://code.visualstudio.com/download) proje dosyalarını düzenlemek için.

Bu kılavuzdaki yönergelere hem Node.js hem de Visual Studio 2017'yi temel alır, ancak herhangi bir geliştirme ortamı veya Web sunucusu kullanmaktan çekinmeyin.

## <a name="create-your-project"></a>Kendi projenizi oluşturun

> ### <a name="option-1-node-other-web-servers"></a>1. seçenek: Düğüm / diğer web sunucuları
> Yüklediğinizden emin olun [Node.js](https://nodejs.org/en/download/), ardından aşağıdaki adımları izleyin:
> - Uygulamanızı barındırmak için bir klasör oluşturun.

<p><!-- -->

> ### <a name="option-2-visual-studio"></a>2. seçenek: Visual Studio
> Visual Studio kullanarak ve yeni bir proje oluşturuyorsanız, yeni bir Visual Studio çözümü oluşturmak için aşağıdaki adımları izleyin:
> 1.    Visual Studio'da:  **Dosya > Yeni > Proje**
> 2.    Altında **Visual C# \Web**seçin **ASP.NET Web uygulaması (.NET Framework)**
> 3.    Uygulamanız için bir ad girin ve seçin **Tamam**
> 4.    Altında **yeni ASP.NET Web uygulaması**seçin **boş**

## <a name="create-your-single-page-applications-ui"></a>Tek sayfalı uygulamanızın kullanıcı Arabirimi oluşturma
1. Oluşturma bir `index.html` , JavaScript SPA'ya dosyası. Visual Studio kullanıyorsanız, ' % s'projesi (projenin kök klasöründe) seçin, sağ tıklatın ve seçin: **Ekle > Yeni Öğe > HTML sayfasını** index.html adlandırın.

2. Sayfanız için aşağıdaki kodu ekleyin:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.4/js/msal.js"></script>
       <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
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
