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
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/16/2018
ms.author: spelluru
ms.openlocfilehash: 4f7aa0b0c5142237f3b6fc66bb8593302c95f5af
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41920644"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>.NET’te Relay Karma Bağlantılar HTTP istekleri ile çalışmaya başlama
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu öğreticide [Azure Geçişi Karma Bağlantıları](relay-what-is-it.md#hybrid-connections) hakkında tanıtıcı bilgilere yer verilmiştir. Microsoft .NET uygulamasını karşılık gelen bir dinleyici uygulamasına istek gönderen bir istemci uygulaması oluşturmak için kullanmayı öğrenin. 

## <a name="what-will-be-accomplished"></a>Ne elde edilecek
Karma Bağlantılar hem istemci hem de sunucu bileşenine ihtiyaç duyar. Bu öğreticideki adımları tamamlayarak iki konsol uygulaması oluşturursunuz:

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İstek almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İstek göndermek için bir istemci (gönderen) konsol uygulaması yazma.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Visual Studio 2015 veya üzeri](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
* Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-by-using-the-azure-portal"></a>1. Azure portalını kullanarak ad alanı oluşturma
Daha önce oluşturduğunuz bir Geçiş ad alanı varsa [Azure portalını kullanarak karma bağlantı oluşturma](#2-create-a-hybrid-connection-using-the-azure-portal) bölümüne gidin.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-by-using-the-azure-portal"></a>2. Azure portalını kullanarak karma bağlantı oluşturma
Daha önce bir karma bağlantı oluşturduysanız [Sunucu uygulaması oluşturma](#3-create-a-server-application-listener) bölümüne gidin.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetine ileti göndermek Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın.
2. İstemci uygulamasını çalıştırın ve metin girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni görüntülediğinden emin olun.

Tebrikler, uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar

* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

