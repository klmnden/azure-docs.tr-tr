---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: d586ca18953b12045fbbaa4a656d78a7192eb88e
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34371244"
---
## <a name="create-an-iot-hub"></a>IoT hub oluşturma
Bağlanılacak sanal cihaz uygulamanız için bir IoT Hub oluşturun. Aşağıdaki adımlar, Azure portalını kullanarak bu görevi nasıl tamamlayacağınızı gösterir.

1. [Azure portalında][lnk-portal] oturum açın.

1. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Iot Hub** seçeneğini belirleyin.
   
    ![Azure portalı Atlama Çubuğu][1]

1. **IoT hub** bölmesine IoT hub’ınızla ilgili aşağıdaki bilgileri girin:

   * **Abonelik**: Bu IoT hub'ını oluşturmak için kullanmak istediğiniz aboneliği seçin.

   * **Kaynak grubu**: IoT hub’ını barındıracak bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın. Daha fazla bilgi için [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma][lnk-resource-groups] konusunu inceleyin.

   * **Bölge**: Size en yakın konumu seçin.

   * **Ad**: IoT hub'ınız için bir ad oluşturun. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![IoT Hub temel bilgileri penceresi][2]

2. IoT hub’ınızı oluşturmaya devam etmek için **Sıradaki: Boyut ve ölçek** öğesini seçin. 

3. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu makale için, aboneliğinizde hala mevcutsa **F1 - Ücretsiz** katmanını seçin. Daha fazla bilgi için [Fiyatlandırma ve ölçek katmanı][lnk-pricing] konusunu inceleyin.

   ![IoT Hub boyut ve ölçek penceresi][3]

4. **İncele ve oluştur**’u seçin.

1. IoT hub bilgilerinizi gözden geçirin, ardından **Oluştur**’a tıklayın. IoT hub’ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Yeni IoT hub’ınız hazır olduğunda Azure portalındaki kutucuğuna tıklayarak özellikler penceresini açın. Bir IoT hub oluşturduktan sonra cihazları ve uygulamaları IoT hub’ınıza bağlamak için kullandığınız önemli bilgileri bulun. **Paylaşılan erişim ilkeleri**’ne tıklayın.
   
1. **Paylaşılan erişim ilkeleri** içinde **iothubowner** ilkesini seçin. IoT Hub **Bağlantı dizesi---birincil anahtar** değerini daha sonra kullanmak üzere kopyalayın. Daha fazla bilgi için "IoT Hub geliştirici kılavuzunun" [Erişim denetimi][lnk-access-control] bölümüne bakın.
   
    ![Paylaşılan erişim ilkeleri][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
