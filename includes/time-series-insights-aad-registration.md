---
title: include dosyası
description: include dosyası
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 07/02/2019
ms.openlocfilehash: a463e3cf475909c34054717460dc10dbba4ad8f0
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543914"
---
> [!IMPORTANT]
> * Yeni **Azure Active Directory** > **uygulama kayıtları** dikey penceresinde eski değiştirir **Azure Active Directory** > **uygulama kayıtlar (eski)** dikey Mayıs 2019.
> * Oluşturulan veya eski dikey pencerede gösterilen uygulama kayıtları otomatik olarak yeni dikey pencerede görüntülenir.
> * Yeni Azure uygulama kayıt deneyimi için geçiş hakkında kapsamlı bilgi için okuma [Kılavuzu eğitim Azure uygulama kayıtları](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide) ve [Azure Active Directory hızlı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. İçinde [Azure portalında](https://ms.portal.azure.com/)seçin **Azure Active Directory** > **uygulama kayıtları** > **yeni kayıt**.

   [![Azure Active Directory'de yeni uygulama kaydı](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

   > [!TIP]
   > Yeni Azure Active Directory uygulaması kayıt paneli seçerek görüntülenen uygulamaları filtrelemenize olanak tanır **uygulamaları ait**.

    Uygulamanızı kaydettikten sonra burada listelenir.

1. Uygulama adı ve select vermek **hesapları yalnızca kuruluş bu dizinde** belirtmek için **desteklenen hesap türleri** API erişebilir. Bunlar, ardından kimlik doğrulama işleminden sonra kullanıcılara yönlendirmek için geçerli bir URI seçin **kaydetme**.

   [![Azure Active Directory'de uygulama oluşturma](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Önemli Azure Active Directory Uygulama bilgileri listelenen uygulamanızın içinde görüntülenen **genel bakış** dikey penceresi. Uygulamanızı altında seçin **uygulamaları ait**, ardından **genel bakış**.

   [![Uygulama Kimliğini kopyalama](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   Kopyalama, **uygulama (istemci) kimliği** istemci uygulamanızda kullanılacak.

1. **Kimlik doğrulaması** dikey önemli kimlik doğrulaması yapılandırma ayarlarını belirtir. 

    1. **Yeniden yönlendirme URI'leri** kimlik doğrulaması istek tarafından sağlanan adres ile eşleşmesi gerekir:

        * Yerel geliştirme ortamı'nda barındırılan uygulamaları için **genel istemci (Mobil ve Masaüstü)** . Ayarladığınızdan emin olun **varsayılan istemci türü** Evet.
        * Azure App Service üzerinde barındırılan tek sayfa uygulamaları için **Web**.

    1. Örtük verme akışı kontrol ederek etkinleştirme **kimlik belirteçlerini**.

   [![Yeni bir gizli anahtarı oluşturma](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   **Kaydet**’e tıklayın.

1. Azure Active Directory uygulaması Azure TIme Series Insights ilişkilendirin. Seçin **API izinleri** > **bir izin eklemek** > **Kuruluşum kullandığı API'leri**. 

    [![Bir API uygulamanızı Azure Active Directory ile ilişkilendirme](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Tür `Azure Time Series Insights` seçip çubuğu Ara `Azure Time Series Insights`.

1. Ardından, uygulamanız için gerekli API izin türü belirtin. Varsayılan olarak, **temsilci izinleri** vurgulanır. Ardından bir izin türünü seçin, **izinleri eklemek**.

    [![Uygulamanız için gerekli API izin türünü belirtin](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)