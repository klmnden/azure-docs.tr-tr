---
title: . NET'te Azure geçiş karma bağlantıları WebSockets kullanmaya başlama | Microsoft Docs
description: Yazma bir C# konsol uygulaması için Azure geçişi karma bağlantıları WebSockets.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 6ad1d5415feefcf30ebae860bc8f4d8a3e2261d5
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428345"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-net"></a>Geçiş karma bağlantıları WebSockets .NET içinde kullanmaya başlayın
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, Azure geçiş karma bağlantıları WebSockets kullanarak ileti alma ve gönderme .NET gönderen ve alıcı uygulamalar oluşturun. Azure geçişi hakkında genel bilgi edinmek için [Azure geçişi](relay-what-is-it.md). 

Bu hızlı başlangıçta, aşağıdaki adımları uygulayın:

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamalar çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-a-namespace"></a>Ad alanı oluşturma
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Karma bağlantı oluşturma
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetine ileti göndermek Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın.
2. İstemci uygulamasını çalıştırın ve metin girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni görüntülediğinden emin olun.

    ![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Tebrikler, tam bir karma bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, WebSockets ileti göndermek ve almak için kullanılan .NET istemci ve sunucu uygulamaları oluşturuldu. Azure geçişi karma bağlantılar özelliği, ileti göndermek ve almak için HTTP kullanarak da destekler. Azure geçiş karma bağlantıları ile HTTP kullanmayı öğrenmek için bkz [HTTP hızlı](relay-hybrid-connections-http-requests-dotnet-get-started.md).

Bu hızlı başlangıçta, .NET Framework istemci ve sunucu uygulamaları oluşturmak için kullanılır. Node.js kullanarak istemci ve sunucu uygulamaları yazmak öğrenmek için bkz: [Node.js WebSockets hızlı](relay-hybrid-connections-node-get-started.md) veya [Node.js HTTP hızlı](relay-hybrid-connections-http-requests-dotnet-get-started.md).

