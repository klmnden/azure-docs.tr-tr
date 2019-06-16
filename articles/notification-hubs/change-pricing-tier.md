---
title: Fiyatlandırma katmanı Notification hubs'ı ad değişikliği | Microsoft Docs
description: Azure Notification hubs'ı ad alanının fiyatlandırma katmanını değiştirmeyi öğrenin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/28/2019
ms.author: jowargo
ms.openlocfilehash: 99ea21b3eb01a674a89c70a1b923f02e600cc3c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60234510"
---
# <a name="change-pricing-tier-of-an-azure-notification-hubs-namespace"></a>Bir Azure notification hubs ad alanı fiyatlandırma katmanını değiştirme
Bildirim hub'ları üç katmanda sunulur: **ücretsiz**, **temel**, ve **standart**. Bu makalede bir Azure Notification hubs'ı ad alanı için fiyatlandırma katmanını değiştirmek gösterilmektedir. 

## <a name="overview"></a>Genel Bakış
Azure Notification Hubs içinde bir **hub** en küçük kaynak/varlıktır. Genellikle uygulama için destekliyoruz her Platform bildirim sistemi için bir uygulama ve can eşlenir bir sertifika tutun. Uygulama, karma veya yerel ve platformlar arası bir uygulama olabilir.

A **ad alanı** notification hubs'ı oluşan bir koleksiyondur. Her ad alanı genellikle ilgili ve belirli bir amaç için kullanılan hubs oluşur. Örneğin, üç farklı ad alanları geliştirme, test ve üretim amaçları için sırasıyla olabilir. 

Ad alanı düzeyinde bir fiyatlandırma katmanı ilişkilendirebilirsiniz. Bildirim hub'ları üç katmanda destekler: **ücretsiz**, **temel**, ve **standart**. Katmanı, gereksinimlerinize uyan bir ad alanı için kullanabilirsiniz. Aşağıdaki bölümlerde, Notification hubs'ı ad alanının fiyatlandırma katmanını değiştirmek nasıl gösterir. 

## <a name="use-azure-portal"></a>Azure portalı kullanma 
Azure portalını kullanarak, ad alanı veya hub sayfası üzerinde bir ad alanı için fiyatlandırma katmanını değiştirebilirsiniz.  Bir hub'ı sayfasında değiştirdiğinizde, aslında bunu ad alanı düzeyinde değiştirebilir. Bu ad alanı ve ad alanındaki tüm hub'ları için fiyatlandırma katmanını değiştirir. 

### <a name="change-tier-on-the-namespace-page"></a>Ad alanı sayfasında katmanını değiştirme
Aşağıdaki yordam, ad sayfasında bir ad alanı için fiyatlandırma katmanını değiştirmek için adımları sağlar. Bir ad alanı için katmanı değiştirdiğinizde, ad alanındaki tüm hub'ları için geçerlidir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol menüsünde. 
3. Seçin **bildirim hub'ı ad alanları** içinde **nesnelerin interneti** bölümü. Yıldız seçerseniz (`*`) metnin yanında, sol gezinti çubuğunun altında için eklenen **Sık Kullanılanlar**. Ad alanlarını sayfanın daha hızlı ve sonraki sürümlerde sonraki açışınızda erişime yardımcı olur. Sık Kullanılanlar ekledikten sonra seçin **bildirim hub'ı ad alanları**. 

    ![Tüm hizmetler -> bildirim hub'ı ad alanları](./media/change-pricing-tier/all-services-nhub.png)
1. Üzerinde **bildirim hub'ı ad alanları** sayfasında, fiyatlandırma katmanını değiştirmek istediğiniz ad alanını seçin. 
2. Üzerinde **bildirim hub'ı Namespace** sayfasında ad alanınız için ad alanında geçerli fiyatlandırma katmanını gördüğünüz **Essentials** bölümü. Aşağıdaki görüntüde, ad alanının fiyatlandırma katmanını olduğunu görebilirsiniz **ücretsiz**. 

    ![Geçerli ad alanı sayfasında fiyatlandırma katmanı](./media/change-pricing-tier/pricing-tier-before.png)
1. Üzerinde **bildirim hub'ı Namespace** seçin, ad sayfasında **fiyatlandırma katmanı** altında **Yönet** bölümü. 

    ![Ad alanı sayfasında fiyatlandırma katmanı seçin](./media/change-pricing-tier/namespace-select-pricing-menu.png)
6. Fiyatlandırma katmanınızı değiştirin ve **seçin** düğmesi.    
7. Değiştirme eylemi katmanı durumunu görmek **uyarılar**. 
8. Geçiş **genel bakış** sayfası. Yeni katman için gösterildiğini onaylayın **fiyatlandırma katmanı** alanındaki **Essentials** bölümü.     
1. Bu adım isteğe bağlıdır. Ad alanındaki tüm hub'ı seçin. Katmanı aynı fiyatlandırma gördüğünüzü onaylayın **Essentials** bölümü. Ad alanındaki tüm hub'lara yönelik aynı fiyatlandırma katmanını görmeniz gerekir. 

### <a name="change-tier-on-the-hub-page"></a>Merkez sayfası üzerinde katmanını değiştirme
Aşağıdaki yordam, hub'ı sayfasında bir ad alanı için fiyatlandırma katmanını değiştirmek için adımları sağlar. Hub sayfasından başlangıç adımları bunu olsa da, aslında ad alanı ve ad alanındaki tüm hub'ları için fiyatlandırma katmanını değiştirin. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol menüsünde.
3. Seçin **Notification hubs'ı** içinde **nesnelerin interneti** bölümü. 
4. Bildiriminiz seçin **hub**. 
5. Seçin **fiyatlandırma katmanı** sol menüsünde. 
6. Fiyatlandırma katmanını değiştirin ve **seçin** düğmesi. Bu eylem, hub'ı içeren ad alanı için fiyatlandırma katmanı ayarını değiştirir. Bu nedenle, ad alanı sayfası ve tüm hub sayfaları yeni fiyatlandırma katmanına görürsünüz. 

## <a name="use-rest-api"></a>REST API’yi kullanma
Geçerli fiyatlandırma katmanı almak ve güncelleştirmek için aşağıdaki kaynak sağlayıcısı REST API'lerini kullanabilirsiniz. 

### <a name="get-current-pricing-tier-for-a-namespace"></a>Geçerli fiyatlandırma katmanı için bir ad alanı alma
Alınacak **geçerli ad alanı katmanı**, aşağıdaki örnekte gösterildiği gibi bir GET komutu Gönder: 

```REST
GET: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
```

### <a name="update-pricing-tier-for-a-namespace"></a>Bir ad alanı için fiyatlandırma katmanını güncelleştirme
İçin **ad alanı katmanını güncelleştirme**, aşağıdaki örnekte gösterildiği gibi PUT komut gönderebilirsiniz: 

```REST
PUT: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
Body: <NotificationHubPlan xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"><SKU>Standard</SKU></NotificationHubPlan>
```



## <a name="next-steps"></a>Sonraki adımlar
Bu katmanlar hakkında daha fazla bilgi ve fiyatlandırma bkz [Notification Hubs fiyatlandırma](https://azure.microsoft.com/pricing/details/notification-hubs/).
