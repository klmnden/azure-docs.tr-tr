---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 51a75ee7bf87c38e3916bdbc8d85abcfb14dca8b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66140259"
---
1. İçinde [Azure portalında](https://portal.azure.com/), tıklayın **tümüne Gözat** > **uygulama hizmetleri**ve Mobile Apps arka ucunuza'ye tıklayın. Altında **ayarları**, tıklayın **App Service gönderim**ve ardından, bildirim hub'ı adına tıklayın.
2. Git **Google (GCM)** , girin **sunucu anahtarı** değeri önceki yordamda Firebase alınan ve ardından **Kaydet**.

    ![Portalda API anahtarını ayarlayın](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

Mobile Apps arka ucu şimdi Firebase Cloud Messaging kullanmak üzere yapılandırılmış. Bu bildirim hub'ı kullanarak bir Android cihazında çalışan uygulamanıza anında iletme bildirimleri göndermenize olanak sağlar.
