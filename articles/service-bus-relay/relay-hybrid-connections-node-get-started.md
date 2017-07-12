---
title: "Node’da Azure Geçiş Karma Bağlantıları ile çalışmaya başlama | Microsoft Docs"
description: "Karma Bağlantılar için bir Node konsol uygulaması yazma"
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
ms.date: 05/22/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: d8f3f6fbe256b34b812369dc1f7492ed4f15d3d3
ms.contentlocale: tr-tr
ms.lasthandoff: 06/05/2017


---
<a id="get-started-with-relay-hybrid-connections" class="xliff"></a>

# Geçiş Karma Bağlantıları ile çalışmaya başlama

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<a id="what-will-be-accomplished" class="xliff"></a>

## Ne elde edilecek

Karma Bağlantılar hem istemci hem de sunucu bileşenini gerektirdiğinden bu öğreticide iki konsol uygulaması oluşturulacaktır. Adımlar aşağıdaki gibidir:

1. Azure portalı kullanılarak Geçiş ad alanı oluşturma.
2. Azure portalı kullanılarak Karma Bağlantı oluşturma.
3. İleti almak için bir sunucu konsol uygulaması yazma.
4. İleti göndermek için bir istemci konsol uygulaması yazma.

<a id="prerequisites" class="xliff"></a>

## Önkoşullar

1. [Node.js](https://nodejs.org/en/) (Bu örnekte Node 7.0 kullanılır).
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<a id="1-create-a-namespace-using-the-azure-portal" class="xliff"></a>

## 1. Azure portalı kullanılarak ad alanı oluşturma

Daha önce oluşturduğunuz bir Geçiş ad alanı varsa [Azure portalını kullanarak Karma Bağlantı oluşturma](#2-create-a-hybrid-connection-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

<a id="2-create-a-hybrid-connection-using-the-azure-portal" class="xliff"></a>

## 2. Azure portalı kullanılarak Karma Bağlantı oluşturma

Daha önce bir Karma Bağlantı oluşturduysanız [Sunucu uygulaması oluşturma](#3-create-a-server-application-listener) bölümüne atlayın.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

<a id="3-create-a-server-application-listener" class="xliff"></a>

## 3. Sunucu uygulaması (dinleyici) oluşturma

Geçiş hizmetinden ileti dinleyip almak için bir Node.js konsol uygulaması yazılacaktır.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

<a id="4-create-a-client-application-sender" class="xliff"></a>

## 4. İstemci uygulaması (gönderici) oluşturma

Geçiş hizmetinden ileti göndermek için bir Node.js konsol uygulaması yazılacaktır.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

<a id="5-run-the-applications" class="xliff"></a>

## 5. Uygulamaları çalıştırma

1. Sunucu uygulamasını çalıştırın.
2. İstemci uygulamasını çalıştırın ve metin girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni çıkardığından emin olun.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Tebrikler, uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar:

* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)


