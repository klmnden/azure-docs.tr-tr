---
title: ".NET’te Azure Geçiş Karma Bağlantıları ile çalışmaya başlama | Microsoft Docs"
description: "Azure Geçişi Karma Bağlantıları için bir C# konsol uygulaması yazın."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: 647f0705500c708680f000654b343ef6937d60f1
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Geçiş Karma Bağlantıları ile çalışmaya başlama
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu öğretici [Azure Geçişi Karma Bağlantıları](relay-what-is-it.md#hybrid-connections)’nı tanıtır ve .NET kullanarak, karşılık gelen dinleyici uygulamasına ileti gönderen bir istemci uygulaması oluşturma işlemini gösterir. 

## <a name="what-will-be-accomplished"></a>Ne elde edilecek
Karma Bağlantılar hem istemci hem de sunucu bileşenini gerektirdiğinden bu öğreticide iki konsol uygulaması oluşturulur. Adımlar aşağıdaki gibidir:

1. Azure portalı kullanılarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturun.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

1. [Visual Studio 2015 veya üzeri](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure portalı kullanılarak ad alanı oluşturma
Daha önce oluşturduğunuz bir Geçiş ad alanı varsa [Azure portalını kullanarak karma bağlantı oluşturma](#2-create-a-hybrid-connection-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Azure portalını kullanarak karma bağlantı oluşturma
Daha önce bir karma bağlantı oluşturduysanız [Sunucu uygulaması oluşturma](#3-create-a-server-application-listener) bölümüne atlayın.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için Visual Studio kullanılarak bir C# konsol uygulaması yazılacaktır.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. İstemci uygulaması (gönderici) oluşturma
Geçiş’e ileti göndermek için Visual Studio kullanılarak bir C# konsol uygulaması yazılacaktır.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın.
2. İstemci uygulamasını çalıştırın ve metin girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni çıkardığından emin olun.

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Tebrikler, uçtan uca Karma Bağlantılar uygulaması oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar:
* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

