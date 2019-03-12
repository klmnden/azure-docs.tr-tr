---
title: Azure CLI Örnekleri - Azure SignalR Hizmeti
description: Azure CLI Örnekleri - Azure SignalR Hizmeti
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: dedd9d14bafb342b0b34e3685ef3b7a46a54f9c8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555224"
---
# <a name="azure-cli-reference"></a>Azure CLI başvurusu

Aşağıdaki tablo, Azure CLI kullanılarak Azure SignalR Hizmeti için bash betiklerine yönelik bağlantılar içerir.

| | |
|-|-|
|**Oluşturma**||
| [Yeni bir SignalR Hizmeti ve kaynak grubu oluşturma](scripts/signalr-cli-create-service.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur.  |
|**Tümleştirme**||
| [SignalR kullanmak üzere yapılandırılmış yeni bir SignalR Hizmeti ve Web Uygulaması oluşturma](scripts/signalr-cli-create-with-app-service.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Ayrıca SignalR Hizmetini kullanan bir ASP.NET Core Web Uygulamasını barındırmak için yeni bir Web Uygulaması ve Uygulama Hizmeti planı da ekler. Web uygulaması, yeni SignalR hizmet kaynağına bağlanmak için bir Uygulama Ayarı ile yapılandırılır. |
| [SignalR ve GitHub OAuth kullanmak üzere yapılandırılmış yeni bir SignalR Hizmeti ve Web Uygulaması oluşturma](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Ayrıca SignalR Hizmetini kullanan bir ASP.NET Core Web Uygulamasını barındırmak için yeni bir Azure Web Uygulaması ve barındırma planı da ekler. Web uygulaması, [kimlik doğrulaması öğreticisinde](signalr-concept-authenticate-oauth.md) gösterildiği gibi, [GitHub kimlik doğrulamasını](https://developer.github.com/v3/guides/basics-of-authentication/) desteklemek amacıyla yeni SignalR hizmeti kaynağının bağlantı dizesi ve istemci gizli dizisi için uygulama ayarlarıyla yapılandırılır. Web uygulaması ayrıca bir yerel git deposu dağıtım kaynağını kullanacak şekilde de yapılandırılır. |
| | |