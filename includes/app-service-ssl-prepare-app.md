---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: ecdd419331c88e712644851f9213861f882cf0f6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188531"
---
## <a name="prepare-your-web-app"></a>Web uygulamanızı hazırlama

Özel bir SSL sertifikasını (üçüncü taraf sertifika veya App Service sertifikası) web uygulamanıza bağlamak için [App Service planınız](https://azure.microsoft.com/pricing/details/app-service/) **Temel**, **Standart**, **Premium** veya **Yalıtılmış** katmanında olmalıdır. Bu adımda, web uygulamanızın desteklenen bir fiyatlandırma katmanında olduğundan emin olacaksınız.

### <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalı](https://portal.azure.com) açın.

### <a name="navigate-to-your-web-app"></a>Web uygulamanıza gidin

Sol menüden **Uygulama Hizmetleri**'ne ve ardından web uygulamanızın adına tıklayın.

![Web uygulaması seçme](./media/app-service-ssl-prepare-app/select-app.png)

Web uygulamanızın yönetim sayfasına geldiniz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

Web uygulaması sayfasının sol gezinti bölmesinde **Ayarlar** bölümüne kaydırın ve **Ölçeği artır (App Service planı)** öğesini seçin.

![Ölçeği artır menüsü](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Web uygulamanızın **F1** veya **D1** katmanında olmadığından emin olun. Web uygulamanızın geçerli katmanı koyu mavi bir kutuyla vurgulanır.

![Fiyatlandırma katmanını denetleyin](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

**F1** veya **D1** katmanında özel SSL desteklenmez. Ölçeği artırmanız gerekirse sonraki bölümde verilen adımları izleyin. Aksi halde, kapatmak **ölçeği** sayfasında ve atlama [App Service planınızın ölçeğini](#scale-up-your-app-service-plan) bölümü.

### <a name="scale-up-your-app-service-plan"></a>App Service planınızın ölçeğini artırma

Ücretsiz olmayan katmanlardan birini seçin (**B1**, **B2**, **B3**, veya **Üretim** kategorisindeki herhangi bir katmanı). Ek seçenekler için **Ek seçeneklere bakın**’a tıklayın.

**Uygula**'ya tıklayın.

![Fiyatlandırma katmanı seçme](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Aşağıdaki bildirimi gördüğünüzde, ölçeklendirme işlemi tamamlanmıştır.

![Ölçek artırma bildirimi](./media/app-service-ssl-prepare-app/scale-notification.png)

