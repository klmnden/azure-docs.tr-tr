---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: da3793c428c624ce3a224cbd7606ab26c4a50803
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140203"
---
Azure App Service Mobile Apps özelliğini kullanan [Azure Notification hubs'ı] mobil uygulamanız için bir bildirim hub'ı yapılandırma şekilde, bildirim göndermek için.

1. İçinde [Azure portal]Git **uygulama hizmetleri**ve ardından, uygulama arka ucunuzu seçin. Altında **ayarları**seçin **anında iletme**.
2. Uygulamasına bir bildirim hub'ı kaynak eklemek için seçin **Connect**. Bir hub oluşturmak veya mevcut bir bağlanın.

    ![Bir hub'ı yapılandırma](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Şimdi bir bildirim hub'ı Mobile Apps arka uç projeniz bağlamış olursunuz. Daha sonra bir platform bildirim sistemi (PNS) cihazlarına göndermenize izin bağlanmak için bu bildirim hub'ı yapılandırın.

[Azure portal]: https://portal.azure.com/
[Azure Notification hubs'ı]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/
