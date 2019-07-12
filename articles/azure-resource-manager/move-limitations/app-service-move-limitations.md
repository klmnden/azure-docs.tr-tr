---
title: Azure App Service kaynaklarını yeni abonelik veya kaynak grubuna taşıma
description: App Service kaynakları yeni kaynak grubuna veya aboneliğe taşıma için Azure Resource Manager'ı kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: c1a09ff4c29a2fedfea2c165a95c042985b3c83a
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723577"
---
# <a name="move-guidance-for-app-service-resources"></a>App Service kaynaklarını Taşıma Kılavuzu

App Service kaynaklarını farklı taşıma adımları bir abonelik içinde veya yeni bir aboneliğe kaynakları olup Taşımakta olduğunuz temel.

## <a name="move-in-same-subscription"></a>Aynı abonelikte Taşı

Bir Web uygulaması taşınırken _aynı abonelik içindeki_, üçüncü taraf SSL sertifikaları taşınamıyor. Ancak, üçüncü taraf sertifikasını taşımadan bir Web uygulaması yeni kaynak grubuna taşıyabilirsiniz ve uygulamanızın SSL işlevselliği yine de çalışır.

SSL sertifikası ile Web uygulaması taşımak istiyorsanız, şu adımları izleyin:

1. Web uygulamasından üçüncü taraf sertifika silin, ancak sertifikanızın birer kopya bulundurun
2. Web uygulaması taşıyın.
3. Üçüncü taraf sertifika taşınan Web uygulamasına yükleyin.

## <a name="move-across-subscriptions"></a>Abonelikler arasında taşıma

Bir Web uygulaması taşınırken _abonelikler arasında_, aşağıdaki sınırlamalar geçerlidir:

- Hedef kaynak grubu mevcut App Service kaynaklar olmaması gerekir. App Service kaynakları şunlardır:
    - Web Apps
    - App Service planları
    - Karşıya yüklenen veya içeri aktarılan SSL sertifikaları
    - App Service ortamları
- Kaynak grubundaki tüm App Service kaynakların birlikte taşınması gerekir.
- App Service kaynaklarını, bunlar ilk olarak oluşturulduğu kaynak grubunun yalnızca taşınabilir. Bir App Service kaynak artık özgün kaynak grubunda değilse, geri taşımak için orijinal kaynak grubu. Daha sonra kaynak, abonelikler arasında taşıyın.

Özgün kaynak grubunu hatırlamıyorsanız tanılama bulabilirsiniz. Web uygulamanızı seçin **tanılayın ve sorunlarını çözmek**. Ardından, **yapılandırma ve Yönetim**.

![Tanılama seçin](./media/app-service-move-limitations/select-diagnostics.png)

Seçin **geçiş seçenekleri**.

![Geçiş seçenekleri seçin](./media/app-service-move-limitations/select-migration.png)

Web uygulaması taşımak için önerilen adımlar seçeneğini belirleyin.

![Önerilen adımları seçin](./media/app-service-move-limitations/recommended-steps.png)

Kaynakları taşımadan önce gerçekleştirilecek önerilen eylemler görürsünüz. Web uygulaması için orijinal kaynak grubu bilgileri içerir.

![Öneriler](./media/app-service-move-limitations/recommendations.png)

## <a name="move-app-service-certificate"></a>App Service sertifikası Taşı

App Service sertifikanız bir yeni kaynak grubuna veya aboneliğe taşıyabilirsiniz. App Service sertifikanız bir web uygulaması ile ilişkili ise, yeni bir abonelik için kaynakları taşımadan önce bazı adımları atmanız gerekir. Özel sertifika ve SSL bağlaması kaynakları taşımadan önce web uygulamasını silin. App Service sertifikası için silinmesi gereken değil yalnızca web App'te özel sertifika.

## <a name="move-support"></a>Taşıma desteği

Hangi App Service kaynaklarını taşınabileceğini saptamak için bkz: taşıma için destek durumu:

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
