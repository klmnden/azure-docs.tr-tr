---
title: içerme dosyasını (cihaz akışları Önizleme)
description: içerme dosyasını (cihaz akışları Önizleme)
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/14/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a02c16a75bbdf4827ce6a6c5f50751caf9c5aab8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446020"
---
Bu bölümde, IOT hub'ı kullanarak oluşturmak açıklar [Azure portalında](https://portal.azure.com).

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **kaynak Oluştur**ve ardından **nesnelerin interneti**.

1. Sağdaki listeden seçin **IOT hub'ı**. IOT hub'ı oluşturmak için ilk sayfasını açar.

   ![Azure portalında bir IOT hub'ı oluşturma](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-01.png)

   Şu alanları doldurun:

   a. İçinde **abonelik** aşağı açılan listesinde, IOT hub'ınız için kullanılacak aboneliği seçin.

   b. İçin **kaynak grubu**, aşağıdakilerden birini yapın: 
      * Yeni bir kaynak grubu oluşturmak için Seç **Yeni Oluştur** ve kullanmak istediğiniz adı girin. 
      * Mevcut bir kaynak grubunu kullanmayı seçin **var olanı kullan** ve aşağı açılan listesinde, kaynak grubunu seçin. 
      
        Daha fazla bilgi için [yönetme Azure Resource Manager kaynak grupları](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   c. İçinde **bölge** aşağı açılan listesinde, hub'ınıza yer almasını istediğiniz bölgeyi seçin. IOT Hub cihaz akışları önizleme ya da destekleyen bir bölge seçin **Orta ABD** veya **Orta ABD EUAP**.

   d. İçinde **IOT hub'ı adı** kutusuna, IOT hub'ınız için bir ad girin. Adın genel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

1. IOT hub'ınıza oluşturmaya devam etmek için seçin **sonraki: Boyut ve ölçek**.

   ![Boyut ve ölçek için Azure portalını kullanarak yeni bir IOT hub'ı ayarlama](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-02.png)

   Bu bölümde, varsayılan ayarları kabul edin ve seçin **gözden geçir + Oluştur** altındaki. Aşağıdaki seçenekleri göz önünde bulundurun:

   * İçinde **fiyatlandırma ve ölçek katmanı** aşağı açılan listesinde, standart katmanların birini seçin (**S1**, **S2**, veya **S3**) veya **F1: Ücretsiz katmanı**. Bu seçenek, Filo boyutuna ve hub'ınıza (örneğin, telemetri iletilerini) beklediğiniz akış dışı iş yükleri tarafından da destekli. Örneğin, ücretsiz katmanı, sınama ve değerlendirme için tasarlanmıştır. Günlük ileti 8000'en fazla ve IOT hub'ına bağlanması 500 CİHAZDAN sağlar. Her Azure aboneliğinin bir IOT hub ücretsiz katmanında oluşturabilirsiniz. 

   * İçin **numarası, IOT Hub birimlerinin**: Bu seçenek, akış dışı iş yükü hub'ınıza beklediğiniz bağlıdır. Şu an için 1 seçebilirsiniz.

   Katman seçenekleri hakkında daha fazla bilgi için bkz. [doğru IOT hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md).

1. Seçimlerinizi gözden geçirmek için seçin **gözden + Oluştur** sekmesi. Açılan bölmede aşağıdakine benzer:

   ![Yeni IOT hub'ı oluşturma hakkında bilgi](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-03.png)

1. Yeni IOT hub'ınızı oluşturmak için Seç **Oluştur**. Bu işlem birkaç dakika sürer.
