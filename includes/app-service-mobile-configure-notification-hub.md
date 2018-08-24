---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 0a74079677a084b2d4e8221cf5a74b356126811d
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42817441"
---
Azure App Service Mobile Apps özelliğini kullanan [Azure Notification hubs'ı] mobil uygulamanız için bir bildirim hub'ı yapılandırma şekilde, bildirim göndermek için.

1. İçinde [Azure portal]Git **uygulama hizmetleri**ve ardından, uygulama arka ucunuzu seçin. Altında **ayarları**seçin **anında iletme**.
2. Uygulamasına bir bildirim hub'ı kaynak eklemek için seçin **Connect**. Bir hub oluşturmak veya mevcut bir bağlanın.

    ![Bir hub'ı yapılandırma](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Şimdi bir bildirim hub'ı Mobile Apps arka uç projeniz bağlamış olursunuz. Daha sonra bir platform bildirim sistemi (PNS) cihazlarına göndermenize izin bağlanmak için bu bildirim hub'ı yapılandırın.

[Azure portal]: https://portal.azure.com/
[Azure Notification hubs'ı]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/
