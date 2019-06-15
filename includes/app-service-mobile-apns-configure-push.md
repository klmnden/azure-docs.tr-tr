---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 852a3b00e8513f9ce68c5aae54c974505d0c9af6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66140133"
---
1. Mac'inizde başlatma **Anahtarlık erişimi**. Sol gezinti çubuğunda, altında **kategori**açın **My sertifikaları**. Önceki bölümde indirdiğiniz SSL sertifikasını bulmak ve ardından içeriği ifşa. Yalnızca sertifikası seçin (özel anahtarı seçmek için değil). Ardından [dışarı](https://support.apple.com/kb/PH20122?locale=en_US).
2. İçinde [Azure portalında](https://portal.azure.com/)seçin **tümüne Gözat** > **uygulama hizmetleri**. Ardından select, Mobile Apps arka ucu. 
3. Altında **ayarları**seçin **App Service gönderim**. Ardından, bildirim hub'ı adı seçin. 
4. Git **Apple anında iletilen bildirim servisi** > **sertifikayı karşıya yükle**. Doğru seçerek .p12 dosyası karşıya **modu** (istemci SSL sertifikası önceki olup olmadığına bağlı olarak üretim ya da korumalı alan'dir). Tüm değişiklikleri kaydedin.

Hizmetiniz, artık iOS anında iletme bildirimleri ile çalışacak şekilde yapılandırılmıştır.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
