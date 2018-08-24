---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 25bb5cfb0073f7533faddec43b38fb5031b82a43
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811603"
---
1. Mac'inizde başlatma **Anahtarlık erişimi**. Sol gezinti çubuğunda, altında **kategori**açın **My sertifikaları**. Önceki bölümde indirdiğiniz SSL sertifikasını bulmak ve ardından içeriği ifşa. Yalnızca sertifikası seçin (özel anahtarı seçmek için değil). Ardından [dışarı](https://support.apple.com/kb/PH20122?locale=en_US).
2. İçinde [Azure portalında](https://portal.azure.com/)seçin **tümüne Gözat** > **uygulama hizmetleri**. Ardından select, Mobile Apps arka ucu. 
3. Altında **ayarları**seçin **App Service gönderim**. Ardından, bildirim hub'ı adı seçin. 
4. Git **Apple anında iletilen bildirim servisi** > **sertifikayı karşıya yükle**. Doğru seçerek .p12 dosyası karşıya **modu** (istemci SSL sertifikası önceki olup olmadığına bağlı olarak üretim ya da korumalı alan'dir). Tüm değişiklikleri kaydedin.

Hizmetiniz, artık iOS anında iletme bildirimleri ile çalışacak şekilde yapılandırılmıştır.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
