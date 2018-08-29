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
ms.openlocfilehash: 08afdcf91499fdb6f2da6e9ccc82313286f5c964
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43111230"
---
## <a name="create-an-iot-hub"></a>IoT hub oluşturma
Bağlanılacak sanal cihaz uygulamanız için bir IoT Hub oluşturun. Aşağıdaki adımlar, Azure portalını kullanarak bu görevi nasıl tamamlayacağınızı gösterir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Iot Hub** seçeneğini belirleyin.
   
    ![Azure portalı Atlama Çubuğu](./media/iot-hub-get-started-create-hub/create-iot-hub1.png)

3. **IoT hub** bölmesine IoT hub’ınızla ilgili aşağıdaki bilgileri girin:

   * **Abonelik**: Bu IoT hub'ını oluşturmak için kullanmak istediğiniz aboneliği seçin.

   * **Kaynak grubu**: IoT hub’ını barındıracak bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../articles/azure-resource-manager/resource-group-portal.md).

   * **Bölge**: Size en yakın konumu seçin.

   * **Ad**: IoT hub'ınız için bir ad oluşturun. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![IoT Hub temel bilgileri penceresi](./media/iot-hub-get-started-create-hub/create-iot-hub2.png)

4. IoT hub’ınızı oluşturmaya devam etmek için **Sıradaki: Boyut ve ölçek** öğesini seçin. 

5. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu makale için, aboneliğinizde hala mevcutsa **F1 - Ücretsiz** katmanını seçin. Daha fazla bilgi için bkz. [Fiyatlandırma ve ölçek katmanı](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![IoT Hub boyut ve ölçek penceresi](./media/iot-hub-get-started-create-hub/create-iot-hub3.png)

6. **İncele ve oluştur**’u seçin.

7. IoT hub bilgilerinizi gözden geçirin, ardından **Oluştur**’a tıklayın. IoT hub’ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

8. Yeni IoT hub’ınız hazır olduğunda Azure portalındaki kutucuğuna tıklayarak özellikler penceresini açın. Bir IoT hub oluşturduktan sonra cihazları ve uygulamaları IoT hub’ınıza bağlamak için kullandığınız önemli bilgileri bulun. **Paylaşılan erişim ilkeleri**’ne tıklayın.
   
9. **Paylaşılan erişim ilkeleri** içinde **iothubowner** ilkesini seçin. IoT Hub **Bağlantı dizesi---birincil anahtar** değerini daha sonra kullanmak üzere kopyalayın. Daha fazla bilgi için "IoT Hub geliştirici kılavuzunun" [Erişim denetimi](../articles/iot-hub/iot-hub-devguide-security.md) bölümüne bakın.
   
    ![Paylaşılan erişim ilkeleri](./media/iot-hub-get-started-create-hub/create-iot-hub5.png)