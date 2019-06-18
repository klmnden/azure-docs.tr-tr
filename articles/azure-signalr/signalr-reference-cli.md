---
title: Azure CLI Örnekleri - Azure SignalR Hizmeti
description: Azure CLI Örnekleri - Azure SignalR Hizmeti
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 1fbe96d827bcf6bb39a6cf7a4e6811174b862d59
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61071085"
---
# <a name="azure-cli-reference"></a>Azure CLI başvurusu

Aşağıdaki tablo, Azure CLI kullanılarak Azure SignalR Hizmeti için bash betiklerine yönelik bağlantılar içerir.

| | |
|-|-|
|**Oluşturma**||
| [Yeni bir SignalR Hizmeti ve kaynak grubu oluşturma](scripts/signalr-cli-create-service.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur.  |
|**Tümleştirme**||
| [SignalR kullanmak üzere yapılandırılmış yeni bir SignalR Hizmeti ve Web Uygulaması oluşturma](scripts/signalr-cli-create-with-app-service.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Ayrıca SignalR hizmetini kullanan ASP.NET Core Web uygulaması barındırmak için yeni bir Web uygulaması ve App Service planı ekler. Web uygulaması, yeni SignalR hizmet kaynağına bağlanmak için bir Uygulama Ayarı ile yapılandırılır. |
| [SignalR ve GitHub OAuth kullanmak üzere yapılandırılmış yeni bir SignalR Hizmeti ve Web Uygulaması oluşturma](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Ayrıca yeni bir Azure Web uygulaması ekler ve barındırma planı konağa SignalR hizmetini kullanan ASP.NET Core Web uygulaması. Web uygulaması, [kimlik doğrulaması öğreticisinde](signalr-concept-authenticate-oauth.md) gösterildiği gibi, [GitHub kimlik doğrulamasını](https://developer.github.com/v3/guides/basics-of-authentication/) desteklemek amacıyla yeni SignalR hizmeti kaynağının bağlantı dizesi ve istemci gizli dizisi için uygulama ayarlarıyla yapılandırılır. Web uygulaması ayrıca bir yerel git deposu dağıtım kaynağını kullanacak şekilde de yapılandırılır. |
| | |
