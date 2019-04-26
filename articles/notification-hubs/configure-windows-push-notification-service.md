---
title: Windows anında bildirim hizmeti, Azure Notification hubs'ı yapılandırma | Microsoft Docs
description: Bir Azure bildirim hub'ı Windows anında bildirim hizmeti ayarlarını yapılandırmayı öğrenin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: c3e3f1e7df5c90c690756375ff1e1b0350c72714
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60240274"
---
# <a name="configure-windows-push-notification-service-wns-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure portalında bir bildirim hub'ı için Windows anında bildirim hizmeti (WNS) ayarlarını yapılandırın
Bu makalede Azure portalını kullanarak bir Azure bildirim hub'ı Windows bildirim Hizmeti'ni (WNS) ayarlarını yapılandırmak nasıl gösterir.  

## <a name="prerequisites"></a>Önkoşullar
Bildirim hub'ı henüz oluşturmadıysanız, şimdi oluşturun. Daha fazla bilgi için [Azure portalında bir Azure bildirim hub'ı oluşturma](create-notification-hub-portal.md). 

## <a name="configure-windows-push-notification-service-wns"></a>Windows anında bildirim hizmeti (WNS) yapılandırma

Aşağıdaki yordam, bildirim hub'ında Windows anında bildirim hizmeti (WNS) ayarlarını yapılandırmak için adımları sunar: 

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Windows (WNS)** sol menüsünde.
2. İçin değerler girin **paket SID'si** ve **güvenlik anahtarı**.
3. **Kaydet**’i seçin.

   ![Paket SID'si ve güvenlik anahtarı kutuları gösteren ekran görüntüsü](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Notification hubs'ı ve Windows anında bildirim hizmeti (WNS) kullanarak evrensel Windows platformu uygulamaları için bildirimler göndermek için bir öğretici için adım adım yönergeler içeren bkz [bildirimlerini gönderir UWP uygulamaları için Azure'ı kullanarak Bildirim hub'ları](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).


