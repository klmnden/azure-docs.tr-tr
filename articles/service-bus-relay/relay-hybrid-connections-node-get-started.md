---
title: Node’da Azure Relay Karma Bağlantılar Web Yuvaları ile çalışmaya başlama | Microsoft Docs
description: Azure Relay Karma Bağlantılar Web Yuvaları için bir Node.js konsol uygulaması yazın.
services: service-bus-relay
documentationcenter: node
author: spelluru
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: b4864673e25ba4f5a1f2e8629e0889863051bc07
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60553920"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-nodejs"></a>Geçiş karma bağlantıları WebSockets node.js'de kullanmaya başlama

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, Azure geçiş karma bağlantıları WebSockets kullanarak ileti alma ve gönderme Node.js gönderen ve alıcı uygulamalar oluşturun. Azure geçişi hakkında genel bilgi edinmek için [Azure geçişi](relay-what-is-it.md). 

Bu hızlı başlangıçta, aşağıdaki adımları uygulayın: 

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamalar çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/).
- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-a-namespace"></a>Ad alanı oluşturma
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Karma bağlantı oluşturma
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için bir Node.js konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetinden ileti göndermek için bir Node.js konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

1. Sunucu uygulamasını çalıştırın: Node.js komut istemine `node listener.js` yazın.
2. İstemci uygulamasını çalıştırın: Node.js komut istemine `node sender.js` yazın ve bazı metinler girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni çıkardığından emin olun.

    ![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Tebrikler, Node.js kullanarak uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, WebSockets ileti göndermek ve almak için kullanılan Node.js istemci ve sunucu uygulamaları oluşturuldu. Azure geçişi karma bağlantılar özelliği, ileti göndermek ve almak için HTTP kullanarak da destekler. Azure geçiş karma bağlantıları ile HTTP kullanmayı öğrenmek için bkz [Node.js HTTP hızlı](relay-hybrid-connections-http-requests-node-get-started.md).

Bu hızlı başlangıçta, Node.js istemci ve sunucu uygulamaları oluşturmak için kullanılır. .NET Framework kullanarak istemci ve sunucu uygulamaları yazmak öğrenmek için bkz: [.NET WebSockets hızlı](relay-hybrid-connections-dotnet-get-started.md) veya [.NET HTTP hızlı](relay-hybrid-connections-http-requests-dotnet-get-started.md).


