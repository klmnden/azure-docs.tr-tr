---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: bb03e4b5b04a0272d8fa9b032da5adb50878b620
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811582"
---
1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **tümüne Gözat** > **uygulama hizmetleri**ve Mobile Apps arka ucunuza'ye tıklayın. Altında **ayarları**, tıklayın **App Service gönderim**ve ardından, bildirim hub'ı adına tıklayın.
2. Git **Google (GCM)**, girin **sunucu anahtarı** değeri önceki yordamda Firebase alınan ve ardından **Kaydet**.

    ![Portalda API anahtarını ayarlayın](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

Mobile Apps arka ucu şimdi Firebase Cloud Messaging kullanmak üzere yapılandırılmış. Bu bildirim hub'ı kullanarak bir Android cihazında çalışan uygulamanıza anında iletme bildirimleri göndermenize olanak sağlar.
