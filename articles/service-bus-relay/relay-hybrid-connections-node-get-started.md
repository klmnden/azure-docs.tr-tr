---
title: "Geçiş Karma Bağlantıları ile çalışmaya başlama | Microsoft Belgeleri"
description: "Karma Bağlantılar için bir Node konsol uygulaması yazma"
services: service-bus-relay
documentationcenter: node
author: jtaubensee
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 10/28/2016
ms.author: jotaub,sethm
translationtype: Human Translation
ms.sourcegitcommit: e26b6005116c7aacdf42afea6ea39e100a9d8080
ms.openlocfilehash: 218082e4cedba2c11871d47bcfb7b92688d97f04


---
# <a name="get-started-with-relay-hybrid-connections"></a>Geçiş Karma Bağlantıları ile çalışmaya başlama
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Ne elde edilecek
Karma Bağlantılar hem istemci hem de sunucu bileşenini gerektirdiğinden bu öğreticide iki konsol uygulaması oluşturulacaktır. Adımlar aşağıdaki gibidir:

1. Azure portalı kullanılarak Geçiş ad alanı oluşturma.
2. Azure portalı kullanılarak Karma Bağlantı oluşturma.
3. İleti almak için bir sunucu konsol uygulaması yazma.
4. İleti göndermek için bir istemci konsol uygulaması yazma.

## <a name="prerequisites"></a>Önkoşullar
1. [Node.js](https://nodejs.org/en/) (Bu örnekte Node 7.0 kullanılır).
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure portalı kullanılarak ad alanı oluşturma
Daha önce oluşturduğunuz bir Geçiş ad alanı varsa [Azure portalını kullanarak Karma Bağlantı oluşturma](#2-create-a-hybrid-connection-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Azure portalı kullanılarak Karma Bağlantı oluşturma
Daha önce bir Karma Bağlantı oluşturduysanız [Sunucu uygulaması oluşturma](#3-create-a-server-application-listener) bölümüne atlayın.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için bir Node.js konsol uygulaması yazılacaktır.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetinden ileti göndermek için bir Node.js konsol uygulaması yazılacaktır.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın.
2. İstemci uygulamasını çalıştırın ve metin girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni çıkardığından emin olun.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Tebrikler, uçtan uca Karma Bağlantılar uygulaması oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar:
* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)




<!--HONumber=Nov16_HO3-->


