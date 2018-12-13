---
title: include dosyası
description: include dosyası
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 73d40bfb5a7e691cead5a84be70398e9cbf6656a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53262790"
---
## <a name="run-the-web-application"></a>Web uygulamasını çalıştırma

1. Sizin için GitHub'da tek sayfalı örnek bir web uygulaması barındırılmaktadır. Tarayıcınızda [https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat/](https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat/) bağlantısını açın.

    > [!NOTE]
    > HTML dosyasının kaynağı [/docs/demo/chat/index.html](https://github.com/Azure-Samples/signalr-service-quickstart-serverless-chat/blob/master/docs/demo/chat/index.html) konumundadır.

1. İşlev uygulamasının temel URL'si istendiğinde *http://localhost:7071* URL’sini girin.

1. İstenildiğinde bir kullanıcı adı girin.

1. Web uygulaması, Azure SignalR Hizmetine bağlanmak amacıyla bağlantı bilgilerini almak için işlev uygulamasındaki *GetSignalRInfo* işlevini çağırır. Bağlantı tamamlandığında sohbet iletisi giriş kutusu görünür.

1. Bir ileti yazın ve Enter tuşuna basın. Uygulama, iletiyi Azure işlev uygulamasındaki *SendMessage* işlevine gönderir, bu da bağlı tüm istemcilere iletiyi yaymak için SignalR çıkış bağlamasını kullanır. Her şey düzgün çalışıyorsa iletinin uygulamada görünmesi gerekir.

    ![Uygulamayı çalıştırma](../media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-run-application.png)

1. Farklı bir tarayıcı penceresinde web uygulamasının başka bir örneğini açın. Gönderilen tüm iletilerin uygulamanın tüm örneklerinde göründüğünü görürsünüz.