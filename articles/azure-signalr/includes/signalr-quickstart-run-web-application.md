---
title: include dosyası
description: include dosyası
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 03/04/2019
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: aa0ebc731469a2c54343e1ba6721fcda0fa8161f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552936"
---
## <a name="run-the-web-application"></a>Web uygulamasını çalıştırma

1. Sizin için GitHub'da tek sayfalı örnek bir web uygulaması barındırılmaktadır. Tarayıcınızda [https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/](https://azure-samples.github.io/signalr-service-quickstart-serverless-chat/demo/chat-v2/) bağlantısını açın.

    > [!NOTE]
    > Kaynak HTML dosyası şu konumdadır [/docs/demo/chat-v2/index.html](https://github.com/Azure-Samples/signalr-service-quickstart-serverless-chat/blob/master/docs/demo/chat-v2/index.html).

1. İşlev uygulamasının temel URL'si istendiğinde *http://localhost:7071* URL’sini girin.

1. İstenildiğinde bir kullanıcı adı girin.

1. Web uygulaması, Azure SignalR Hizmetine bağlanmak amacıyla bağlantı bilgilerini almak için işlev uygulamasındaki *GetSignalRInfo* işlevini çağırır. Bağlantı tamamlandığında sohbet iletisi giriş kutusu görünür.

1. Bir ileti yazın ve Enter tuşuna basın. Uygulama, iletiyi Azure işlev uygulamasındaki *SendMessage* işlevine gönderir, bu da bağlı tüm istemcilere iletiyi yaymak için SignalR çıkış bağlamasını kullanır. Her şey düzgün çalışıyorsa iletinin uygulamada görünmesi gerekir.

    ![Uygulamayı çalıştırma](../media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-run-application.png)

1. Farklı bir tarayıcı penceresinde web uygulamasının başka bir örneğini açın. Gönderilen tüm iletilerin uygulamanın tüm örneklerinde göründüğünü görürsünüz.

> [!IMPORTANT]
> HTTPS kullanarak HTML sayfası sunulur, ancak yerel Azure işlevleri çalışma zamanı varsayılan olarak HTTP kullanarak için tarayıcınızı (Firefox gibi) bir karışık içerik ilke engelleyen işlevlerinizi web sayfasından isteklerine şart koşabilir. Bunu çözmek için bu kısıtlama olması veya yerel bir HTTP sunucusu gibi başlatın bir tarayıcı kullanın [http-server](https://www.npmjs.com/package/http-server) içinde */docs/demo/chat-v2* dizin. Kaynak eklenir olun `CORS` ayarı *local.settings.json*.