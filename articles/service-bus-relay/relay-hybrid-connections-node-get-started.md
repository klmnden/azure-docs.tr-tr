---
title: "Node’da Azure Geçiş Karma Bağlantıları ile çalışmaya başlama | Microsoft Docs"
description: "Azure Geçiş Karma Bağlantıları için bir Node.js konsol uygulaması yazın."
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 12/20/2017
ms.author: sethm
ms.openlocfilehash: d79c05253da568d3a9b2f2912d22eab2df586821
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Geçiş Karma Bağlantıları ile çalışmaya başlama

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu öğretici [Azure Geçiş Karma Bağlantıları](relay-what-is-it.md#hybrid-connections)’nı tanıtır ve Node.js kullanarak ilgili dinleyici uygulamasına ileti gönderen bir istemci uygulaması oluşturma işlemini gösterir. 

## <a name="what-will-be-accomplished"></a>Ne elde edilecek

Karma Bağlantılar hem istemci hem de sunucu bileşenini gerektirdiğinden bu öğreticide iki konsol uygulaması oluşturun. Adımlar aşağıdaki gibidir:

1. Azure portalı kullanılarak Geçiş ad alanı oluşturma.
2. Azure portalı kullanılarak karma bağlantı oluşturma.
3. İleti almak için bir sunucu konsol uygulaması yazma.
4. İleti göndermek için bir istemci konsol uygulaması yazma.

## <a name="prerequisites"></a>Ön koşullar

1. [Node.js](https://nodejs.org/en/).
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure portalı kullanılarak ad alanı oluşturma

Daha önce oluşturduğunuz bir Geçiş ad alanı varsa [Azure portalını kullanarak karma bağlantı oluşturma](#2-create-a-hybrid-connection-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Azure portalını kullanarak karma bağlantı oluşturma

Daha önce bir karma bağlantı oluşturduysanız [Sunucu uygulaması oluşturma](#3-create-a-server-application-listener) bölümüne atlayın.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Sunucu uygulaması (dinleyici) oluşturma

Geçiş hizmetinden ileti dinleyip almak için bir Node.js konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. İstemci uygulaması (gönderici) oluşturma

Geçiş hizmetinden ileti göndermek için bir Node.js konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Uygulamaları çalıştırma

1. Sunucu uygulamasını çalıştırın: Node.js komut istemine `node listener.js` yazın.
2. İstemci uygulamasını çalıştırın: Node.js komut istemine `node sender.js` yazın ve bazı metinler girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni çıkardığından emin olun.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Tebrikler, Node.js kullanarak uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar

* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

