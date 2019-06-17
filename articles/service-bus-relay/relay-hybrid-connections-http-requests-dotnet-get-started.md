---
title: .NET’te Azure Relay Karma Bağlantılar HTTP istekleri ile çalışmaya başlama | Microsoft Docs
description: .NET’te Azure Relay Karma Bağlantılar HTTP istekleri için bir C# konsol uygulaması yazın.
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
ms.openlocfilehash: 37227b7d0ea1b3630a3c2ce991a61543e6a1503d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428254"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>.NET’te Relay Karma Bağlantılar HTTP istekleri ile çalışmaya başlama
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, HTTP protokolünü kullanarak ileti alma ve gönderme .NET gönderen ve alıcı uygulamalar oluşturun. Uygulamaları Azure geçiş karma bağlantılar özelliğini kullanın. Azure geçişi hakkında genel bilgi edinmek için [Azure geçişi](relay-what-is-it.md). 

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

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetine ileti göndermek Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın. Konsol penceresinde aşağıdaki metni görürsünüz:

    ```
    Online
    Server listening
    ```
1. İstemci uygulamasını çalıştırın. İstemci penceresinde `hello!` görürsünüz. İstemci bir HTTP isteği sunucuya gönderilen ve sunucu yanıtı ile bir `hello!`. 
3. Şimdi, konsol pencerelerini kapatmak için her iki pencerede **ENTER**’a basın. 

Tebrikler, tam bir karma bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HTTP iletileri gönderip almak için kullanılan .NET istemci ve sunucu uygulamaları oluşturuldu. Azure geçişi karma bağlantılar özelliği, ileti göndermek ve almak için WebSockets kullanarak da destekler. WebSockets Azure geçiş karma bağlantıları ile kullanmayı öğrenmek için bkz [WebSockets hızlı](relay-hybrid-connections-dotnet-get-started.md).

Bu hızlı başlangıçta, .NET Framework istemci ve sunucu uygulamaları oluşturmak için kullanılır. Node.js kullanarak istemci ve sunucu uygulamaları yazmak öğrenmek için bkz: [Node.js WebSockets hızlı](relay-hybrid-connections-node-get-started.md) veya [Node.js HTTP hızlı](relay-hybrid-connections-http-requests-dotnet-get-started.md).
